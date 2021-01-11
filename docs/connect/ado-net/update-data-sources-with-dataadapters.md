---
title: Actualización de orígenes de datos con objetos DataAdapter
description: Obtenga información sobre cómo el método Update de DataAdapter resuelve los cambios de un objeto DataSet de nuevo en el origen de datos de las aplicaciones ADO.NET.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771389"
---
# <a name="update-data-sources-with-dataadapters"></a>Actualización de orígenes de datos con objetos DataAdapter

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El método `Update` de <xref:System.Data.Common.DataAdapter> se llama para reflejar en el origen de datos todos los cambios efectuados en <xref:System.Data.DataSet>. El método `Update`, al igual que el método `Fill`, acepta como argumentos una instancia de `DataSet` y, de forma opcional, un objeto <xref:System.Data.DataTable> o un nombre de `DataTable`. La instancia de `DataSet` es el `DataSet` que contiene los cambios efectuados, y `DataTable` identifica la tabla desde la que se pueden recuperar esos cambios. Si no se especifica `DataTable`, se utiliza el primer `DataTable` de `DataSet`.

Al llamar al método `Update`, `DataAdapter` analiza los cambios efectuados y ejecuta el comando apropiado (INSERT, UPDATE o DELETE). Cuando `DataAdapter` encuentra un cambio en <xref:System.Data.DataRow>, utiliza los comandos <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>, <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> o <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> para reflejarlo.

Estas propiedades permiten maximizar el rendimiento de la aplicación ADO.NET si se especifica la sintaxis de comandos en tiempo de diseño y, siempre que sea posible, mediante el uso de procedimientos almacenados. Antes de llamar a `Update` deben establecerse de forma explícita los comandos. Si se llama a `Update` y el comando correspondiente a una actualización determinada no existe (por ejemplo, no hay un comando `DeleteCommand` para las filas eliminadas), se inicia una excepción.

> [!IMPORTANT]
> Si usa procedimientos almacenados de SQL Server para editar o eliminar datos mediante un elemento `DataAdapter`, asegúrese de que no usa `SET NOCOUNT ON` en la definición del procedimiento almacenado. Esto hace que el recuento de filas afectadas vuelva a cero, lo que `DataAdapter` interpreta como un conflicto de simultaneidad. En este caso, se iniciará una <xref:System.Data.DBConcurrencyException>.

Se pueden usar los parámetros de comando para especificar los valores de entrada y salida de una instrucción SQL o un procedimiento almacenado para cada fila modificada en `DataSet`. Para obtener más información, vea [Parámetros de DataAdapter](dataadapter-parameters.md).

> [!NOTE]
> Es importante comprender la diferencia entre eliminar una fila de una <xref:System.Data.DataTable> y quitar la fila. Al llamar al método `Remove` o `RemoveAt`, la fila se quita inmediatamente. Las filas correspondientes en el origen de datos de back-end **no se verán afectadas** si después pasa `DataTable` o `DataSet` a un objeto `DataAdapter` y llama a `Update`. Al utilizar el método `Delete`, la fila permanece en `DataTable` y se marca para eliminación. Si después pasa `DataTable` o `DataSet` a un objeto `DataAdapter` y llama a `Update`, **se elimina** la fila correspondiente del origen de datos de back-end.

Si `DataTable` está asignada a una única base de datos o se ha generado a partir de ella, puede utilizar el objeto <xref:System.Data.Common.DbCommandBuilder> para generar automáticamente los objetos `DeleteCommand`, `InsertCommand` y `UpdateCommand` de `DataAdapter`. Para obtener más información, vea [Generación de comandos con objetos CommandBuilder](generate-commands-with-commandbuilders.md).

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>Uso de UpdatedRowSource para asignar valores a un objeto DataSet

Puede controlar cómo los valores devueltos del origen de datos se vuelven a asignar a `DataTable` después de una llamada al método <xref:System.Data.Common.DbDataAdapter.Update%2A> de un objeto `DataAdapter`, mediante la propiedad <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> de un objeto <xref:Microsoft.Data.SqlClient.SqlCommand>. Al asignar la propiedad `UpdatedRowSource` a uno de los valores de enumeración <xref:System.Data.UpdateRowSource>, puede determinar si los parámetros que devuelven los comandos `DataAdapter` se deben omitir o se deben aplicar a la fila cambiada en `DataSet`. También puede especificar si la primera fila devuelta (si existe) se aplica a la fila modificada en `DataTable`.

En la tabla siguiente se describen los distintos valores de la enumeración `UpdateRowSource` y la forma en que afectan al comportamiento del comando utilizado con `DataAdapter`.

|Enumeración UpdatedRowSource|Descripción|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Tanto los parámetros de salida como la primera fila del conjunto de resultados devuelto se pueden asignar a la fila modificada en `DataSet`.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Solo los datos de la primera fila del conjunto de resultados devuelto se pueden asignar a la fila modificada en el `DataSet`.|
|<xref:System.Data.UpdateRowSource.None>|Se pasan por alto todos los parámetros de salida y las filas del conjunto de resultados devuelto.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Solo los parámetros de salida se pueden asignar a la fila modificada en `DataSet`.|

El método `Update` vuelve a resolver los cambios en el origen de datos; sin embargo, puede que otros clientes hayan modificado datos en el origen de datos desde la última vez que se llenó `DataSet`. Para actualizar `DataSet` con datos actuales, utilice el `DataAdapter` y el método `Fill`. De esta forma se agregan las filas nuevas a la tabla y se actualiza la información en las filas ya existentes.

El método `Fill` determina si se va a agregar una nueva fila o si se va a actualizar una fila existente mediante el examen de los valores de clave principal de las filas de `DataSet` y las filas devueltas por `SelectCommand`. Si el método `Fill` encuentra un valor de clave principal de una fila de `DataSet` que coincide con un valor de clave principal de una fila de los resultados devueltos por `SelectCommand`, éste actualiza la fila existente con la información de la fila devuelta por `SelectCommand` y establece el <xref:System.Data.DataRow.RowState%2A> de la fila existente en `Unchanged`. Si una fila devuelta por `SelectCommand` tiene un valor de clave principal que no coincide con ninguno de los valores de clave principal de las filas de `DataSet`, el método `Fill` agrega una nueva fila con un `RowState` de `Unchanged`.

> [!NOTE]
> Si `SelectCommand` devuelve los resultados de una operación **OUTER JOIN**, el objeto `DataAdapter` no establecerá un valor de `PrimaryKey` para el objeto `DataTable` resultante. Debe definir `PrimaryKey` para asegurarse de que las filas duplicadas se resuelven correctamente.

Para controlar las excepciones que se puedan producir al llamar al método `Update`, puede usar el evento `RowUpdated` para responder a los errores de actualización de filas en cuanto se producen (vea [Control de eventos de objetos DataAdapter](handle-dataadapter-events.md)), o bien puede establecer <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> en `true` antes de llamar a `Update` y responder a la información de error almacenada en la propiedad `RowError` de una fila concreta una vez que se haya completado la actualización.

> [!NOTE]
> La llamada a `AcceptChanges` en `DataSet`, `DataTable` o `DataRow` hará que todos los valores `Original` de un objeto `DataRow` se sobrescriban con los valores `Current` de `DataRow`. Si se han modificado los valores de campo que identifican de forma única a una fila, los valores `AcceptChanges` dejarán de coincidir con los valores del origen de datos después de llamar a `Original`. Se llama a `AcceptChanges` automáticamente para cada fila durante una llamada al método `Update` de un objeto `DataAdapter`. Puede conservar los valores originales durante una llamada al método Update estableciendo primero la propiedad `AcceptChangesDuringUpdate` de `DataAdapter` en false o creando un controlador de eventos para el evento `RowUpdated` y estableciendo <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> en <xref:System.Data.UpdateStatus.SkipCurrentRow>. Para obtener más información, vea [Control de eventos de objetos DataAdapter](handle-dataadapter-events.md).

En los ejemplos siguientes se muestra cómo realizar las actualizaciones en las filas modificadas mediante el establecimiento explícito de `UpdateCommand` de un objeto `DataAdapter` y la llamada a su método `Update`.

> [!NOTE]
> El parámetro especificado en la cláusula `WHERE clause` de la instrucción `UPDATE statement` se establece para usar el valor `Original` del objeto `SourceColumn`. Este hecho es muy importante ya que el valor `Current` puede haber sido modificado de forma que ya no coincida con el valor del origen de datos. El valor `Original` es el que se usó para rellenar la tabla `DataTable` a partir del origen de datos.

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>AutoIncrement (columnas)

Si las tablas del origen de datos tienen columnas con incremento automático, puede rellenar las columnas de `DataSet` devolviendo el valor de incremento automático como un parámetro de salida de un procedimiento almacenado y asignándolo a una columna de la tabla, o devolviendo el valor de incremento automático de la primera fila de un conjunto de resultados devuelto por un procedimiento almacenado o una instrucción SQL, o mediante el evento `RowUpdated` de `DataAdapter` para ejecutar una instrucción SELECT adicional. Para obtener más información y un ejemplo, consulte [Recuperación de valores de identidad o autonuméricos](retrieve-identity-or-autonumber-values.md).

## <a name="ordering-of-inserts-updates-and-deletes"></a>Ordenación de inserciones, actualizaciones y eliminaciones

En algunas circunstancias, es importante el orden en que se envían al origen de datos los cambios realizados en el `DataSet`. Por ejemplo, si se actualiza el valor de una clave principal de una fila existente y se ha agregado una nueva fila con el nuevo valor de la clave principal como una clave externa, es importante que la actualización de la fila se procese antes que la inserción.

Puede usar el método `Select` de `DataTable` para devolver una matriz `DataRow` que solo haga referencia a filas con un estado `RowState` determinado. A continuación, puede pasar la matriz `DataRow` al método `Update` de `DataAdapter` para procesar las filas modificadas. Al especificar un subconjunto de filas que modificar, puede controlar el orden en que se procesan las inserciones, actualizaciones y eliminaciones.

## <a name="example"></a>Ejemplo

Por ejemplo, en el código siguiente se garantiza que en primer lugar se realizan en la tabla las eliminaciones de filas, después las actualizaciones y finalmente las inserciones.

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>Uso de un objeto DataAdapter para recuperar y actualizar datos

Puede usar un objeto DataAdapter para recuperar y actualizar los datos.

- En el ejemplo se usa `DataAdapter.AcceptChangesDuringFill` para clonar los datos de la base de datos. Si la propiedad se establece como **false**, no se llama a **AcceptChanges** al rellenar la tabla, y las filas recién agregadas se tratan como filas insertadas. Por lo tanto, el ejemplo usa estas filas para insertar las filas nuevas en la base de datos.

- En los ejemplos se usa `DataAdapter.TableMappings` para definir la asignación entre la tabla de origen y DataTable.

- En el ejemplo se usa `DataAdapter.FillLoadOption` para determinar cómo rellena el adaptador el objeto **DataTable** de **DbDataReader**. Cuando se crea un objeto DataTable, solo se pueden escribir los datos de la base de datos en la versión actual o la original si se establece la propiedad como **LoadOption.Upsert** o **LoadOption.PreserveChanges**.

- En el ejemplo también se actualizará la tabla mediante `DbDataAdapter.UpdateBatchSize` para realizar operaciones por lotes.

Antes de compilar y ejecutar el ejemplo, debe crear la base de datos de ejemplo:

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>Consulte también

- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Recuperación de valores de identidad o autonuméricos](retrieve-identity-or-autonumber-values.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
