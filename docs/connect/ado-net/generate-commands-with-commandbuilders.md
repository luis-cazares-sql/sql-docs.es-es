---
title: Generación de comandos con objetos CommandBuilder
description: Explica cómo usar los generadores de comandos para generar automáticamente comandos INSERT, UPDATE y DELETE para un objeto `DataAdapter` que tiene un comando SELECT de una sola tabla.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 6e3fb8b5-373b-4f9e-ab03-a22693df8e91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d738650db9dbb7b0b2ed3f9411945173f73f748f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771330"
---
# <a name="generating-commands-with-commandbuilders"></a>Generación de comandos con objetos CommandBuilder

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Cuando la propiedad `SelectCommand` del objeto <xref:System.Data.Common.DbDataAdapter> se especifica dinámicamente en tiempo de ejecución, por ejemplo, mediante una herramienta de consulta que toma un comando de texto del usuario, es posible que no pueda especificar el objeto `InsertCommand`, `UpdateCommand` o `DeleteCommand` adecuado en tiempo de diseño. Si el objeto <xref:System.Data.DataTable> se asigna a una única tabla de base de datos o se genera a partir de ella, puede utilizar el objeto <xref:System.Data.Common.DbCommandBuilder> para generar automáticamente las propiedades `DeleteCommand`, `InsertCommand` y `UpdateCommand` de <xref:System.Data.Common.DbDataAdapter>.

> [!NOTE]
> En el proveedor de datos SqlClient de Microsoft para SQL Server, la clase <xref:Microsoft.Data.SqlClient.SqlDataAdapter> se deriva de la clase <xref:System.Data.Common.DbDataAdapter> y la clase <xref:Microsoft.Data.SqlClient.SqlCommandBuilder> se deriva de la clase <xref:System.Data.Common.DbCommandBuilder>.

El requisito mínimo para que la generación automática de comandos funcione correctamente consiste en establecer la propiedad `SelectCommand`. El esquema de tabla que recupera la propiedad `SelectCommand` determina la sintaxis de las instrucciones INSERT, UPDATE y DELETE generadas automáticamente.

<xref:System.Data.Common.DbCommandBuilder> debe ejecutar `SelectCommand` con el objeto de devolver los metadatos necesarios para construir los comandos SQL INSERT, UPDATE y DELETE. Por eso es necesario realizar un viaje adicional al origen de datos, con el consiguiente efecto adverso en el rendimiento. Para mejorar el rendimiento, debe especificar los comandos de forma explícita, en lugar de utilizar <xref:System.Data.Common.DbCommandBuilder>.

> [!NOTE]
> `SelectCommand` también debe devolver como mínimo una clave principal o una columna única. Si no hay ninguno, se genera una excepción `InvalidOperation` y no se generan los comandos.

Cuando se asocia con un objeto `DataAdapter`, <xref:System.Data.Common.DbCommandBuilder> genera automáticamente las propiedades `InsertCommand`, `UpdateCommand` y `DeleteCommand` del objeto `DataAdapter` si son referencias nulas. Si ya existe algún objeto `Command` para una propiedad, se utilizará el objeto `Command` existente.

Las vistas de bases de datos creadas al unir una o varias tablas no se consideran una tabla única de base de datos. En esta instancia, no se puede utilizar <xref:System.Data.Common.DbCommandBuilder> para generar comandos automáticamente; debe especificar los comandos de forma explícita. Para obtener información sobre cómo establecer comandos de forma explícita para devolver al origen de datos las actualizaciones efectuadas en el `DataSet`, consulte [Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md).

Es posible que desee asignar parámetros de salida a la fila actualizada de un `DataSet`. Una tarea habitual consiste en recuperar, a partir del origen de datos, el valor de un campo de identidad de generación automática o una marca de tiempo. <xref:System.Data.Common.DbCommandBuilder> no asigna de forma predeterminada los parámetros de salida a las columnas de una fila actualizada. En esta instancia, debe especificar el comando de manera explícita. Para consultar un ejemplo de asignación de un campo de identidad de generación automática a una columna de una fila insertada, consulte [Recuperación de valores de identidad o autonuméricos](retrieve-identity-or-autonumber-values.md).

## <a name="rules-for-automatically-generated-commands"></a>Reglas para comandos generados automáticamente

En la tabla siguiente se muestran las reglas de la generación automática de comandos.

|Get-Help|Regla|  
|-------------|----------|  
|`InsertCommand`|Inserta una fila en el origen de datos para todas las filas de la tabla con una <xref:System.Data.DataRow.RowState%2A> con el valor <xref:System.Data.DataRowState.Added>. Inserta valores para todas las columnas actualizables, pero no para determinadas columnas como identidades, expresiones o marcas de tiempo.|  
|`UpdateCommand`|Actualiza filas en el origen de datos para todas las filas de la tabla con una propiedad `RowState` con el valor <xref:System.Data.DataRowState.Modified>. Actualiza los valores de todas las columnas, con excepción de las que no son actualizables, como identidades o expresiones. Actualiza todas las filas en las que los valores de columna en el origen de datos coinciden con los valores de la columna de clave principal de la fila, siempre que las restantes columnas del origen de datos coincidan con los valores originales de la fila. Para obtener más información, vea la sección "Modelo de simultaneidad optimista para actualizaciones y eliminaciones" de este mismo tema.|  
|`DeleteCommand`|Elimina filas en el origen de datos para todas las filas de la tabla con una propiedad `RowState` con el valor <xref:System.Data.DataRowState.Deleted>. Elimina todas las filas en las que los valores de columna coinciden con los valores de la columna de clave principal de la fila, siempre que las restantes columnas del origen de datos coincidan con los valores originales de la fila. Para obtener más información, vea [Modelo de simultaneidad optimista para actualizaciones y eliminaciones](#optimistic-concurrency-model-for-updates-and-deletes), más adelante en este tema.|

## <a name="optimistic-concurrency-model-for-updates-and-deletes"></a>Modelo de simultaneidad optimista para actualizaciones y eliminaciones

La lógica empleada en la generación automática de comandos para las instrucciones UPDATE y DELETE se basa en la *simultaneidad optimista*, es decir, los registros no se bloquean para ser editados y pueden ser modificados en cualquier momento por otros usuarios o procesos. Dado que existe la posibilidad de que un registro haya sido modificado después de que haya sido devuelto por la instrucción SELECT y antes de que se emita la instrucción UPDATE o DELETE, la instrucción UPDATE o DELETE generada automáticamente incluye una cláusula WHERE que especifica que la fila solo se actualiza cuando contiene todos los valores originales y no ha sido eliminada del origen de datos. Esto evita que se sobrescriban los datos nuevos.
 
> [!NOTE]
> Si una actualización generada automáticamente intenta actualizar una fila que ha sido eliminada o que no contiene los valores originales del <xref:System.Data.DataSet>, el comando no tienen ningún efecto en los registros y se inicia una excepción <xref:System.Data.DBConcurrencyException>.

Si desea que la instrucción UPDATE o DELETE se ejecute sin tener en cuenta los valores originales, debe establecer de forma explícita la propiedad `UpdateCommand` del `DataAdapter` sin utilizar la generación automática de comandos.

## <a name="limitations-of-automatic-command-generation-logic"></a>Limitaciones de la lógica de generación automática de comandos

La generación automática de comandos tiene las siguientes limitaciones.

### <a name="unrelated-tables-only"></a>Solo tablas no relacionadas

La lógica de generación automática de comandos crea instrucciones INSERT, UPDATE o DELETE para tablas independientes sin tener en cuenta las relaciones que éstas puedan tener con otras tablas en el origen de datos. Por eso, se puede producir un error al llamar a `Update` para realizar cambios en una columna que participa de una restricción de clave externa en la base de datos. Para evitar esa excepción, no utilice <xref:System.Data.Common.DbCommandBuilder> al actualizar las columnas que participan en una restricción de clave externa. En este caso debe especificar de forma explícita las instrucciones que se van a utilizar para llevar a cabo la operación.

### <a name="table-and-column-names"></a>Nombres de tablas y columnas

La lógica de generación automática de comandos puede ocasionar un error cuando los nombres de las tablas o de las columnas incluyen algún carácter especial, como espacios, puntos, comillas y otros caracteres no alfanuméricos, incluso si están delimitados por corchetes. En función del proveedor, el establecimiento de los parámetros QuotePrefix y QuoteSuffix puede permitir que la lógica de generación procese espacios, pero los caracteres especiales no pueden convertirse en caracteres de escape. Se pueden utilizar nombres completos de tabla, como *catalog.schema.table*.

## <a name="use-the-commandbuilder-to-automatically-generate-an-sql-statement"></a>Uso de CommandBuilder para generar automáticamente una instrucción SQL

Para generar instrucciones SQL automáticamente para un `DataAdapter`, defina en primer lugar la propiedad `SelectCommand` del `DataAdapter` y, a continuación, cree un objeto `CommandBuilder` y especifique como argumento el `DataAdapter` para el que `CommandBuilder` generará automáticamente las instrucciones SQL.

[!code-csharp[SqlCommandBuilder_Create#1](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#1)]

## <a name="modify-the-selectcommand"></a>Modificación de SelectCommand

Es posible que se inicie una excepción si modifica valor `CommandText` de `SelectCommand` después de generar automáticamente los comandos INSERT, UPDATE o DELETE. Si el valor `SelectCommand.CommandText` modificado contiene información del esquema que sea incoherente con el valor `SelectCommand.CommandText` utilizado en el momento de la generación automática de los comandos de inserción, actualización o eliminación, las futuras llamadas al método `DataAdapter.Update` pueden tratar de obtener acceso a columnas que ya no existen en la tabla actual a la que hace referencia `SelectCommand`, con lo que se iniciará una excepción.

Puede actualizar la información del esquema que utiliza `CommandBuilder` para generar automáticamente los comandos; para ello, basta con llamar al método `RefreshSchema` de `CommandBuilder`.

Si desea conocer el comando generado automáticamente, puede obtener una referencia a los comandos generados automáticamente mediante los métodos `GetInsertCommand`, `GetUpdateCommand` y `GetDeleteCommand` del objeto `CommandBuilder` y la comprobación de la propiedad `CommandText` del comando asociado.

En el ejemplo de código siguiente se escribe en la consola el comando de actualización generado automáticamente.

[!code-csharp[SqlCommandBuilder_Create#2](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#2)]

En el siguiente ejemplo se crea de nuevo la tabla en el conjunto de datos. Se llama al método **RefreshSchema** para actualizar los comandos generados automáticamente con la información de esta nueva columna.

[!code-csharp[SqlCommandBuilder_Create#3](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#3)]

## <a name="see-also"></a>Vea también

- [Comandos y parámetros](commands-parameters.md)
- [Ejecución de un comando](execute-command.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
