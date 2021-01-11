---
title: Parámetros de objetos DataAdapter
description: Obtenga información sobre las propiedades de DbDataAdapter que devuelven datos de un origen de datos y administran los cambios en este.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 90985f155392a78da408ffbda3e1d2c0b12d4648
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771607"
---
# <a name="dataadapter-parameters"></a>Parámetros de objetos DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:System.Data.Common.DbDataAdapter> tiene cuatro propiedades que se utilizan para recuperar y actualizar datos en el origen de datos: la propiedad <xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> devuelve datos del origen de datos y las propiedades <xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>, <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> y <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> se utilizan para administrar los cambios en el origen de datos.

> [!NOTE]
> La propiedad `SelectCommand` debe establecerse antes de llamar al método `Fill` de `DataAdapter`. Las propiedades `InsertCommand`, `UpdateCommand` o `DeleteCommand` se deben establecer antes llamar al método `Update` de `DataAdapter`, en función de las modificaciones realizadas en los datos en <xref:System.Data.DataTable>. Por ejemplo, si se han agregado filas, se debe establecer `InsertCommand` antes de llamar a `Update`. Cuando `Update` procesa una fila insertada, actualizada o eliminada, `DataAdapter` utiliza la propiedad `Command` que corresponde a la acción en cuestión. La información actual relacionada con la fila modificada se pasa al objeto `Command` a través de la colección `Parameters`.

Al actualizar una fila en el origen de datos, se llama a la instrucción UPDATE, que usa un identificador único para identificar la fila de la tabla que se va a actualizar. El identificador único suele ser el valor del campo de clave principal. La instrucción UPDATE utiliza parámetros que contienen el identificador único y las columnas y valores que se van a actualizar, como muestra la siguiente instrucción Transact-SQL.

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> La sintaxis de los marcadores de posición de parámetros depende del origen de datos. En este ejemplo se muestran marcadores de posición para un origen de datos de SQL Server.

En este ejemplo, el campo `CompanyName` se actualiza con el valor del parámetro `@CompanyName` para la fila, donde `CustomerID` equivale al valor del parámetro `@CustomerID`. Los parámetros recuperan información de la fila modificada mediante la propiedad <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlParameter>. A continuación se muestran los parámetros del ejemplo anterior de la instrucción UPDATE. En el código se parte de que el `adapter` de la variable representa a un objeto <xref:Microsoft.Data.SqlClient.SqlDataAdapter> válido.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

El método `Add` de la colección `Parameters` toma el nombre del parámetro, el tipo de datos, el tamaño (si corresponde al tipo) y el nombre de la propiedad <xref:System.Data.Common.DbParameter.SourceColumn%2A> de `DataTable`. Tenga en cuenta que <xref:System.Data.Common.DbParameter.SourceVersion%2A> del parámetro `@CustomerID` se establece en `Original`. De esta forma se garantiza que la fila existente en el origen de datos se actualice cuando el valor de la columna o columnas identificadas haya cambiado en la fila <xref:System.Data.DataRow> modificada. En ese caso, el valor de la fila `Original` coincidiría con el valor actual en el origen de datos y el valor de la fila `Current` contendría el valor actualizado. No se asigna ningún valor a `SourceVersion` para el parámetro `@CompanyName`, por lo que se utiliza el valor predeterminado, el de la fila `Current`.

> [!NOTE]
> En el caso de las operaciones `Fill` de los métodos `DataAdapter` y `Get` de `DataReader`, el tipo de .NET se deduce del tipo devuelto del proveedor de datos SqlClient de Microsoft para SQL Server. Los métodos de descriptor de acceso y los tipos de .NET deducidos para tipos de datos de Microsoft SQL Server se describen en [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md).

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn, Parameter.SourceVersion

`SourceColumn` y `SourceVersion` se pueden pasar como argumentos al constructor `Parameter`, o también se pueden establecer como propiedades de un `Parameter` existente. `SourceColumn` es el nombre de <xref:System.Data.DataColumn> de <xref:System.Data.DataRow> en la que se recupera el valor de `Parameter`. `SourceVersion` especifica la versión de `DataRow` que utiliza `DataAdapter` para recuperar el valor.

En la tabla siguiente se muestran los valores de la enumeración <xref:System.Data.DataRowVersion> disponibles para su uso con `SourceVersion`.

|Enumeración DataRowVersion|Descripción|  
|--------------------------------|-----------------|  
|`Current`|El parámetro utiliza el valor actual de la columna. Este es el valor predeterminado.|  
|`Default`|El parámetro utiliza el `DefaultValue` de la columna.|  
|`Original`|El parámetro utiliza el valor original de la columna.|  
|`Proposed`|El parámetro utiliza un valor propuesto.|  

En el ejemplo de código de `SqlClient` de la siguiente sección se define un parámetro para <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> donde la columna `CustomerID` se utiliza como `SourceColumn` para dos parámetros: `@CustomerID` (`SET CustomerID = @CustomerID`) y `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`). El parámetro `@CustomerID` se usa para actualizar la columna **CustomerID** al valor actual de `DataRow`. Como resultado, se usa el parámetro `CustomerID` `SourceColumn` con un valor `SourceVersion` de `Current`. El parámetro `@OldCustomerID` se usa para identificar la fila actual en el origen de datos. Dado que el valor de la columna coincidente se encuentra en la versión `Original` de la fila, también se usa el mismo objeto `SourceColumn` (`CustomerID`) con `SourceVersion` de `Original`.

## <a name="work-with-sqlclient-parameters"></a>Trabajo con parámetros de SqlClient

En el ejemplo siguiente se muestra cómo crear <xref:Microsoft.Data.SqlClient.SqlDataAdapter> y establecer <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> en <xref:System.Data.MissingSchemaAction.AddWithKey> para recuperar información de esquema adicional de la base de datos. Las propiedades <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> y <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> establecen sus correspondientes objetos <xref:Microsoft.Data.SqlClient.SqlParameter> agregados a la colección <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A>. El método devuelve un objeto `SqlDataAdapter`.

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>Consulte también

- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Comandos y parámetros](commands-parameters.md)
- [Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md)
- [Modificación de datos con procedimientos almacenados](modify-data-with-stored-procedures.md)
- [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
