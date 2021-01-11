---
title: Modificación de datos con procedimientos almacenados
description: Describe cómo utilizar parámetros de entrada y parámetros de salida de procedimientos almacenados para insertar una fila en una base de datos y devolver un nuevo valor de identidad.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744459"
---
# <a name="modify-data-with-stored-procedures"></a>Modificación de datos con procedimientos almacenados

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Los procedimientos almacenados pueden aceptar datos como parámetros de entrada y pueden devolver datos como parámetros de salida, conjuntos de resultados o valores de retorno. En el ejemplo siguiente se muestra cómo el proveedor de datos SqlClient de Microsoft para SQL Server envía y recibe parámetros de entrada, parámetros de salida y valores devueltos. En el ejemplo se inserta un nuevo registro en una tabla en la que la columna de clave principal es una columna de identidad.

> [!NOTE]
> Si usa procedimientos almacenados para editar o eliminar datos mediante un elemento <xref:Microsoft.Data.SqlClient.SqlDataAdapter>, asegúrese de que no usa **SET NOCOUNT ON** en la definición del procedimiento almacenado. Esto hace que el recuento de filas afectadas vuelva a cero, lo que `DataAdapter` interpreta como un conflicto de simultaneidad. En este caso, se iniciará una <xref:System.Data.DBConcurrencyException>.

## <a name="example"></a>Ejemplo

En el ejemplo se utiliza el siguiente procedimiento almacenado para insertar una nueva categoría en la tabla **Categories** de **Northwind**. El procedimiento almacenado recibe el valor de la columna **CategoryName** como parámetro de entrada y usa la función **SCOPE_IDENTITY()** para recuperar el nuevo valor del campo de identidad, **CategoryID**, y devolverlo en un parámetro de salida. La instrucción RETURN utiliza la función **\@\@ROWCOUNT** para devolver el número de filas insertadas.

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

En el siguiente ejemplo de código se utiliza el anterior procedimiento almacenado `InsertCategory` como origen de la propiedad <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> de <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. El parámetro de salida `@Identity` se reflejará en <xref:System.Data.DataSet> una vez que se haya insertado el registro en la base de dados al llamar al método `Update` del <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. El código también recupera el valor devuelto.

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Ejecución de un comando](execute-command.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
