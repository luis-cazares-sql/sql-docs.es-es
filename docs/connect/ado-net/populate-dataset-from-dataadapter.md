---
title: Rellenar un conjunto de datos a partir de un objeto DataAdapter
description: Obtenga información sobre cómo rellenar un objeto DataSet desde un objeto DataAdapter en ADO.NET, que proporciona un modelo de programación relacional coherente independiente del origen de datos.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e6c50bf7255dc77edfd0b93e03dedeec83ed4c4d
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771507"
---
# <a name="populate-a-dataset-from-a-dataadapter"></a>Rellenar un conjunto de datos a partir de un objeto DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El objeto <xref:System.Data.DataSet> de ADO.NET es una representación de datos residente en memoria que proporciona un modelo de programación relacional coherente independiente del origen de datos. `DataSet` representa un conjunto completo de datos que incluye tablas, restricciones y relaciones entre las tablas. Dado que `DataSet` es independiente del origen de datos, `DataSet` puede incluir datos locales de la aplicación y datos de otros muchos orígenes. La interacción con los orígenes de datos existentes se controla mediante el `DataAdapter`.

La propiedad `SelectCommand` de `DataAdapter` es un objeto `Command` que recupera datos del origen de datos. Las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` de `DataAdapter` son objetos `Command` que permiten administrar las actualizaciones de los datos en el origen de datos para reflejar las modificaciones efectuadas en los datos de `DataSet`. Estas propiedades se describen con más detalle en [Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md).

El método `Fill` de `DataAdapter` se usa para rellenar un objeto `DataSet` con los resultados del elemento `SelectCommand` de `DataAdapter`. `Fill` toma como argumentos un elemento `DataSet` que se debe rellenar y un objeto `DataTable` o el nombre del objeto `DataTable` que se debe rellenar con las filas que devuelve `SelectCommand`.

> [!NOTE]
> El uso de `DataAdapter` para recuperar la totalidad de una tabla lleva tiempo, en especial si la tabla incluye un gran número de filas. Esto se debe a que el acceso a la base de datos, la localización y el procesamiento de los datos, y la posterior transferencia de los mismos al cliente son procesos largos. La extracción de la tabla completa al cliente también bloquea todas las filas en el servidor. Para mejorar el rendimiento, puede usar la cláusula `WHERE` para reducir en gran medida el número de filas que se devuelven al cliente. También puede reducir la cantidad de datos que se devuelven al cliente si enumera de forma explícita las columnas necesarias en la instrucción `SELECT` . Otra solución consiste en recuperar las filas por lotes (por ejemplo varios cientos de filas de una vez) y recuperar solo el siguiente lote cuando el cliente haya finalizado con el lote actual.

El método `Fill` utiliza el objeto `DataReader` de forma implícita para devolver los nombres y tipos de columna que se usan para crear las tablas de `DataSet`, y los datos para rellenar las filas de las tablas en `DataSet`. Las tablas y columnas solo se crean cuando no existen; en caso contrario, `Fill` utiliza el esquema existente de `DataSet` . Los tipos de columna se crean como tipos de .NET Framework en función de las tablas en [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md). No se crean claves principales a menos que existan en el origen de datos y `DataAdapter` **.** `MissingSchemaAction` se establezca en `MissingSchemaAction` **.** `AddWithKey`. Si el método `Fill` encuentra que una tabla tiene una clave principal, sobrescribe los datos de `DataSet` con los del origen de datos en las filas donde los valores de columna de clave principal coinciden con los de la fila que devuelve el origen de datos. Si no se detecta ninguna clave principal, los datos se agregan a las tablas de `DataSet`. `Fill` usa las asignaciones que puedan existir al rellenar `DataSet` (vea [Asignaciones de objetos DataAdapter, DataTable y DataColumn](dataadapter-datatable-datacolumn-mappings.md)).

> [!NOTE]
> Si `SelectCommand` devuelve los resultados de OUTER JOIN, `DataAdapter` no establece un valor `PrimaryKey` para el objeto `DataTable`resultante. Debe definir `PrimaryKey` para asegurarse de que las filas duplicadas se resuelven correctamente.

En el ejemplo de código siguiente se crea una instancia de <xref:Microsoft.Data.SqlClient.SqlDataAdapter> que utiliza un objeto <xref:Microsoft.Data.SqlClient.SqlConnection> a la base de datos `Northwind` de Microsoft SQL Server y se rellena un objeto <xref:System.Data.DataTable> en un `DataSet` con la lista de clientes. La instrucción SQL y los argumentos <xref:Microsoft.Data.SqlClient.SqlConnection> pasados al constructor <xref:Microsoft.Data.SqlClient.SqlDataAdapter> se utilizan para crear la propiedad <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> del <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="example"></a>Ejemplo

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

> [!NOTE]
> El código que se muestra en este ejemplo no abre ni cierra explícitamente el objeto `Connection`. El método `Fill` abre de forma implícita el objeto `Connection` que `DataAdapter` utiliza cuando encuentra que la conexión no está abierta todavía. Si el método `Fill` ha abierto la conexión, también la cierra cuando el método `Fill` deja de utilizarla. Este hecho simplifica el código cuando se trabaja con una operación única, como `Fill` o `Update`. Sin embargo, en el caso de que se estén realizando varias operaciones que necesiten tener abierta una conexión, se puede mejorar el rendimiento de la aplicación llamando explícitamente al método `Open` de `Connection`, realizando las operaciones en el origen de datos y, finalmente, llamando al método `Close` de `Connection`. Es conveniente mantener abiertas las conexiones con el origen de datos el menor tiempo posible para liberar recursos, de manera que estén disponibles para otras aplicaciones cliente.

## <a name="multiple-result-sets"></a>Varios conjuntos de resultados

Si `DataAdapter` encuentra varios conjuntos de resultados, crea varias tablas en `DataSet`. Las tablas reciben de forma predeterminada el nombre secuencial Table *N*, comenzando por "Table" que representa Table0. Si se pasa un nombre de tabla como argumento al método `Fill` , las tablas reciben de forma predeterminada el nombre secuencial TableName *N*, comenzando por "TableName" que representa TableName0.  
  
## <a name="populate-a-dataset-from-multiple-dataadapters"></a>Rellenado de un objeto DataSet a partir de varios objetos DataAdapter  

Con una instancia de `DataSet` se puede usar cualquier número de objetos `DataAdapter`. Cada `DataAdapter` se puede usar para rellenar uno o varios objetos `DataTable` y resolver de nuevo las actualizaciones en el origen de datos correspondiente. Se pueden agregar objetos`DataRelation` y `Constraint` a `DataSet` localmente, lo que permite relacionar datos procedentes de varios orígenes distintos. Por ejemplo, un `DataSet` puede contener datos de una base de datos de Microsoft SQL Server, una base de datos de IBM DB2 expuesta mediante OLE DB y un origen de datos que genera secuencias XML. La comunicación con cada origen de datos se puede controlar usando uno o varios objetos `DataAdapter` .  
  
### <a name="example"></a>Ejemplo  

En el ejemplo de código siguiente se rellena una lista de clientes a partir de la base de datos `Northwind` almacenada en Microsoft SQL Server, y una lista de pedidos a partir de la base de datos `Northwind` almacenada en Microsoft Access. Las tablas rellenas se relacionan entre sí mediante `DataRelation`, con lo que se puede mostrar una lista de clientes con los pedidos que ha realizado cada uno.

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="sql-server-decimal-type"></a>Tipo decimal de SQL Server

De forma predeterminada, en `DataSet` se almacenan datos mediante tipos de datos de .NET. En la mayor parte de las aplicaciones, estos tipos proporcionan una representación adecuada de la información del origen de datos. Sin embargo, esa representación puede ocasionar problemas cuando el tipo de datos del origen de datos es decimal o numérico de SQL Server. El tipo de datos `decimal` de .NET permite un máximo de 28 dígitos significativos, mientras que el tipo de datos `decimal` de SQL Server permite 38 dígitos. Si `SqlDataAdapter` determina durante una operación `Fill` que la precisión de un campo `decimal` de SQL Server es superior a 28 caracteres, la fila actual no se agrega a `DataTable`. En su lugar, se produce el evento `FillError` que permite determinar si se va a producir o no una pérdida de precisión y tomar las medidas adecuadas. Para obtener más información sobre el evento `FillError`, vea [Control de eventos de objetos DataAdapter](handle-dataadapter-events.md). Para obtener el valor `decimal` de SQL Server, también se puede utilizar un objeto <xref:Microsoft.Data.SqlClient.SqlDataReader> y llamar al método <xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> .

ADO.NET incluye compatibilidad mejorada con <xref:System.Data.SqlTypes> en `DataSet`. Para obtener más información, consulta [SqlTypes and the DataSet](./sql/sqltypes-dataset.md).

## <a name="see-also"></a>Consulte también

- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md)
- [Conjuntos de resultados activos múltiples (MARS)](./sql/multiple-active-result-sets-mars.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
