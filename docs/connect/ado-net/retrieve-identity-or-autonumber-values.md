---
title: Recuperación de valores de identidad o autonuméricos
description: Obtenga información sobre cómo recuperar valores de identidad o autonuméricos de claves principales en SQL Server y cómo combinar nuevos valores de identidad en ADO.NET.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: d6b7f9cb-81be-44e1-bb94-56137954876d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4ca2199686359235ff1d2c834b0b19a88296030d
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744455"
---
# <a name="retrieve-identity-or-autonumber-values"></a>Recuperación de valores de identidad o autonuméricos

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una clave principal de una base de datos relacional es una columna o combinación de columnas que siempre contienen valores únicos. Conocer el valor de la clave principal permite localizar la fila que la contiene. Los motores de bases de datos relacionales, como SQL Server, Oracle y Microsoft Access/Jet admiten la creación de columnas de incremento automático que pueden designarse como claves principales. Estos valores los genera el servidor cuando se agregan filas a una tabla. En SQL Server se establece la propiedad de identidad de una columna, en Oracle se crea una secuencia y en Microsoft Access se crea una columna Autonumérica.

<xref:System.Data.DataColumn> también se puede utilizar para generar de manera automática valores incrementales estableciendo la propiedad <xref:System.Data.DataColumn.AutoIncrement%2A> en true. No obstante, podría haber valores duplicados en instancias distintas de <xref:System.Data.DataTable> si varias aplicaciones cliente están generando por separado valores incrementales de manera automática. Si se tiene un servidor que genera de manera automática valores incrementales se eliminan posibles conflictos, pues se permite a cada usuario recuperar el valor generado para cada fila insertada.

Durante una llamada al método `Update` de `DataAdapter`, la base de datos puede volver a enviar datos a la aplicación ADO.NET como parámetros de salida o como el primer registro devuelto del conjunto de resultados de una instrucción SELECT ejecutada en el mismo lote que la instrucción INSERT. El proveedor de datos SqlClient de Microsoft para SQL Server puede recuperar estos valores y actualizar las columnas correspondientes en el <xref:System.Data.DataRow> que se está actualizando.

> [!NOTE]
> Una opción alternativa al uso de un valor de incremento automático es utilizar el método <xref:System.Guid.NewGuid%2A> de un objeto <xref:System.Guid> para generar un GUID (identificador único global) en el equipo cliente que se pueda copiar al servidor cuando se inserte una nueva fila. El método `NewGuid` genera un valor binario de 16 bits que se crea mediante un algoritmo que permite que haya una alta probabilidad de que no se duplique ningún valor. En una base de datos de SQL Server, el GUID se almacena en una columna `uniqueidentifier` que SQL Server puede generar automáticamente mediante la función Transact-SQL `NEWID()`. Utilizar un GUID como clave principal puede afectar de manera negativa al rendimiento. SQL Server proporciona compatibilidad para la función `NEWSEQUENTIALID()`, que genera un GUID secuencial que no está garantizado que sea único globalmente, pero que se puede indexar de forma más eficaz.

## <a name="retrieve-sql-server-identity-column-values"></a>Recuperación de los valores de la columna de identidad de SQL Server

Cuando trabaje con Microsoft SQL Server, puede crear procedimientos almacenados con un parámetro de salida para devolver el valor de identidad de una fila insertada. La siguiente tabla describe las tres funciones de Transact-SQL en SQL Server que se pueden utilizar para recuperar valores de columna de identidad.

|Función|Descripción|
|--------------|-----------------|
|SCOPE_IDENTITY|Devuelve el último valor de identidad en el ámbito de ejecución actual. SCOPE_IDENTITY se recomienda en la mayoría de los casos.|
|@@IDENTITY|Contiene el último valor de identidad generado en cualquier tabla de la sesión actual. @@IDENTITY puede verse afectado por los desencadenadores y no devolver el valor de identidad esperado.|
|IDENT_CURRENT|Devuelve el último valor de identidad generado para una tabla concreta de cualquier sesión y en cualquier ámbito.|

El siguiente procedimiento almacenado muestra cómo insertar una fila en la tabla **Categories** y utilizar un parámetro de salida para devolver el nuevo valor de identidad generado por la función Transact-SQL SCOPE_IDENTITY().

```sql
CREATE PROCEDURE dbo.InsertCategory
  @CategoryName nvarchar(15),
  @Identity int OUT
AS
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)
SET @Identity = SCOPE_IDENTITY()
```

El procedimiento almacenado se puede especificar como el origen de <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> de un objeto <xref:Microsoft.Data.SqlClient.SqlDataAdapter>. La propiedad <xref:Microsoft.Data.SqlClient.SqlCommand.CommandType%2A> de <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> debe establecerse en <xref:System.Data.CommandType.StoredProcedure>. La salida de identidad se recupera creando un <xref:Microsoft.Data.SqlClient.SqlParameter> que tiene un <xref:System.Data.ParameterDirection> de <xref:System.Data.ParameterDirection.Output>. Cuando se procesa `InsertCommand`, el valor de identidad de incremento automático se devuelve y se coloca en la columna **CategoryID** de la fila actual si se establece la propiedad <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> del comando de inserción en `UpdateRowSource.OutputParameters` o `UpdateRowSource.Both`.

Si el comando de inserción ejecuta un lote que incluye tanto una instrucción INSERT como una instrucción SELECT que devuelven el nuevo valor de identidad, entonces puede recuperar el nuevo valor estableciendo la propiedad `UpdatedRowSource` del comando de inserción en `UpdateRowSource.FirstReturnedRecord`.

[!code-csharp[DataWorks SqlClient.RetrieveIdentityStoredProcedure#1](~/../sqlclient/doc/samples/SqlDataAdapter_RetrieveIdentityStoredProcedure.cs#1)]

## <a name="merge-new-identity-values"></a>Combinación de nuevos valores de identidad

Un caso frecuente es llamar al método `GetChanges` de `DataTable` para crear una copia que contiene únicamente filas modificadas y utilizar la nueva copia al llamar al método `Update` de `DataAdapter`. Esto es especialmente útil cuando hay que serializar las filas modificadas en un componente independiente que realiza la actualización. Después de la actualización, la copia puede contener nuevos valores de identidad que se deben volver a combinar en el `DataTable` original. Probablemente los nuevos valores de identidad son diferentes a los valores originales de `DataTable`. Para conseguir la combinación, se deben mantener los valores originales de las columnas **AutoIncrement** en la copia para poder localizar y actualizar filas existentes en el `DataTable` original, en lugar de anexar filas nuevas con los nuevos valores de identidad. No obstante, de manera predeterminada estos valores se pierden después de una llamada al método `Update` de `DataAdapter`, debido a que se llama implícitamente a `AcceptChanges` en cada `DataRow` actualizada.

Hay dos maneras de mantener los valores originales de `DataColumn` en `DataRow` durante una actualización de `DataAdapter`:

- El primer método para mantener los valores originales consiste en establecer la propiedad `AcceptChangesDuringUpdate` de `DataAdapter` en `false`. Esta configuración afecta a cada `DataRow` de `DataTable` que se está actualizando. Para más información y ver un código de ejemplo, vea <xref:System.Data.Common.DataAdapter.AcceptChangesDuringUpdate%2A>.

- El segundo método consiste en escribir código en el controlador de eventos `RowUpdated` de `DataAdapter` para establecer <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> en <xref:System.Data.UpdateStatus.SkipCurrentRow>. `DataRow` se actualiza pero se mantiene el valor original de cada `DataColumn`. Este método permite mantener los valores originales en algunas filas y no en otras. Por ejemplo, el código puede mantener los valores originales de filas agregadas y no los de filas editadas o eliminadas comprobando primero <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> y, a continuación, estableciendo <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> en <xref:System.Data.UpdateStatus.SkipCurrentRow> únicamente para filas con un `StatementType` de `Insert`.

Cuando se usa cualquiera de estos métodos para conservar los valores originales en un `DataRow` durante una actualización de `DataAdapter`, el adaptador de datos SqlClient de Microsoft para SQL Server realiza una serie de acciones para establecer los valores actuales de `DataRow` en los nuevos valores devueltos por los parámetros de salida o por la primera fila devuelta de un conjunto de resultados, al tiempo que se conserva el valor original en cada `DataColumn`. Primero, se llama al método `AcceptChanges` de `DataRow` para mantener los valores actuales como valores originales y, a continuación, se asignan los nuevos valores. Después de estas acciones, las `DataRows` que tienen la propiedad <xref:System.Data.DataRow.RowState%2A> establecida en <xref:System.Data.DataRowState.Added> tendrán su propiedad `RowState` establecida en <xref:System.Data.DataRowState.Modified>, lo que puede ser inesperado.

El modo en que se aplican los resultados del comando a cada <xref:System.Data.DataRow> que se actualiza lo determina la propiedad <xref:System.Data.Common.DbCommand.UpdatedRowSource%2A> de cada <xref:System.Data.Common.DbCommand>. Esta propiedad se establece en un valor desde la enumeración `UpdateRowSource`.

La siguiente tabla describe cómo afectan los valores de enumeración `UpdateRowSource` a la propiedad <xref:System.Data.DataRow.RowState%2A> de las filas actualizadas.

|Nombre del miembro|Descripción|
|-----------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|Se llama a `AcceptChanges` y tanto los parámetros de salida como los valores de la primera fila de cualquier conjunto de resultados devuelto se colocan en la `DataRow` que se está actualizando. Si no hay valores que aplicar, `RowState` será <xref:System.Data.DataRowState.Unchanged>.|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|Si se devuelve una fila, se llama a `AcceptChanges` y la fila se asigna a la fila modificada en `DataTable`, estableciendo `RowState` en `Modified`. Si no se devuelve ninguna fila, entonces no se llama a `AcceptChanges` y `RowState` permanece en `Added`.|
|<xref:System.Data.UpdateRowSource.None>|Se pasan por alto todos los parámetros o filas devueltos. No hay llamada a `AcceptChanges` y `RowState` permanece en `Added`.|
|<xref:System.Data.UpdateRowSource.OutputParameters>|Se llama a `AcceptChanges` y todos los parámetros de salida se asignan a la fila modificada en `DataTable`, estableciendo `RowState` en `Modified`. Si no hay parámetros de salida, `RowState` será `Unchanged`.|

### <a name="example"></a>Ejemplo

Este ejemplo muestra la extracción de filas modificadas desde `DataTable` y el uso de <xref:Microsoft.Data.SqlClient.SqlDataAdapter> para actualizar el origen de datos y recuperar un nuevo valor de columna de identidad. <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> ejecuta dos instrucciones Transact-SQL; la primera es la instrucción INSERT y la segunda es la instrucción SELECT.

```sql
INSERT INTO dbo.Shippers (CompanyName)
VALUES (@CompanyName);
SELECT ShipperID, CompanyName FROM dbo.Shippers
WHERE ShipperID = SCOPE_IDENTITY();
```

La propiedad `UpdatedRowSource` del comando de inserción se establece en `UpdateRowSource.FirstReturnedRow` y la propiedad <xref:System.Data.MissingSchemaAction> de `DataAdapter` se establece en `MissingSchemaAction.AddWithKey`. `DataTable` se rellena y el código agrega una nueva fila a `DataTable`. A continuación, las filas modificadas se extraen en un nuevo `DataTable`, que se pasa a `DataAdapter`, el cual actualiza el servidor.

[!code-csharp[DataWorks SqlClient.MergeIdentity#1](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#1)]

El controlador de eventos `OnRowUpdated` comprueba <xref:System.Data.Common.RowUpdatedEventArgs.StatementType%2A> de <xref:Microsoft.Data.SqlClient.SqlRowUpdatedEventArgs> para determinar si la fila es una inserción. Si lo es, entonces la propiedad se establece <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> en <xref:System.Data.UpdateStatus.SkipCurrentRow>. La fila está actualizada, pero los valores originales de la fila se mantienen. En el cuerpo principal del procedimiento, se llama al método <xref:System.Data.DataSet.Merge%2A> para fusión mediante combinación el nuevo valor de identidad en el `DataTable` original y, finalmente, se llama a `AcceptChanges`.

[!code-csharp[DataWorks SqlClient.MergeIdentity#2](~/../sqlclient/doc/samples/SqlDataAdapter_MergeIdentity.cs#2)]

### <a name="retrieve-identity-values"></a>Recuperación de valores de identidad

A menudo se establece la columna como identidad cuando los valores de la columna deben ser únicos. A veces se necesita el valor de identidad de los nuevos datos. En este ejemplo se muestra cómo recuperar los valores de identidad:

- Crea un procedimiento almacenado para insertar los datos y devolver un valor de identidad.

- Ejecuta un comando para insertar los nuevos datos y mostrar el resultado.

- Usa <xref:Microsoft.Data.SqlClient.SqlDataAdapter> para insertar nuevos datos y mostrar el resultado.

Antes de compilar y ejecutar el ejemplo, debe crear la base de datos de ejemplo mediante el script siguiente:

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
CREATE procedure [dbo].[CourseExtInfo] @CourseId int
as
select c.CourseID,c.Title,c.Credits,d.Name as DepartmentName
from Course as c left outer join Department as d on c.DepartmentID=d.DepartmentID
where c.CourseID=@CourseId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create procedure [dbo].[DepartmentInfo] @DepartmentId int,@CourseCount int output
as
select @CourseCount=Count(c.CourseID)
from course as c
where c.DepartmentID=@DepartmentId

select d.DepartmentID,d.Name,d.Budget,d.StartDate,d.Administrator
from Department as d
where d.DepartmentID=@DepartmentId

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
Create PROCEDURE [dbo].[GetDepartmentsOfSpecifiedYear]
@Year int,@BudgetSum money output
AS
BEGIN
        SELECT @BudgetSum=SUM([Budget])
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

SELECT [DepartmentID]
      ,[Name]
      ,[Budget]
      ,[StartDate]
      ,[Administrator]
  FROM [MySchool].[dbo].[Department]
  Where YEAR([StartDate])=@Year

END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[GradeOfStudent]
-- Add the parameters for the stored procedure here
@CourseTitle nvarchar(100),@FirstName nvarchar(50),
@LastName nvarchar(50),@Grade decimal(3,2) output
AS
BEGIN
select @Grade=Max(Grade)
from [dbo].[StudentGrade] as s join [dbo].[Course] as c on
s.CourseID=c.CourseID join [dbo].[Person] as p on s.StudentID=p.PersonID
where c.Title=@CourseTitle and p.FirstName=@FirstName
and p.LastName= @LastName
END
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE PROCEDURE [dbo].[InsertPerson]
-- Add the parameters for the stored procedure here
@FirstName nvarchar(50),@LastName nvarchar(50),
@PersonID int output
AS
BEGIN
    insert [dbo].[Person](LastName,FirstName) Values(@LastName,@FirstName)

    set @PersonID=SCOPE_IDENTITY()
END
Go

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

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [dbo].[Person]([PersonID] [int] IDENTITY(1,1) NOT NULL,
[LastName] [nvarchar](50) NOT NULL,
[FirstName] [nvarchar](50) NOT NULL,
[HireDate] [datetime] NULL,
[EnrollmentDate] [datetime] NULL,
[Picture] [varbinary](max) NULL,
 CONSTRAINT [PK_School.Student] PRIMARY KEY CLUSTERED
(
[PersonID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[StudentGrade]([EnrollmentID] [int] IDENTITY(1,1) NOT NULL,
[CourseID] [nvarchar](10) NOT NULL,
[StudentID] [int] NOT NULL,
[Grade] [decimal](3, 2) NOT NULL,
 CONSTRAINT [PK_StudentGrade] PRIMARY KEY CLUSTERED
(
[EnrollmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
create view [dbo].[EnglishCourse]
as
select c.CourseID,c.Title,c.Credits,c.DepartmentID
from Course as c join Department as d on c.DepartmentID=d.DepartmentID
where d.Name=N'English'

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
SET IDENTITY_INSERT [dbo].[Person] ON

INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (1, N'Hu', N'Nan', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (2, N'Norman', N'Laura', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (3, N'Olivotto', N'Nino', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (4, N'Anand', N'Arturo', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (5, N'Jai', N'Damien', NULL, CAST(0x0000A0BF00000000 AS DateTime))
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (6, N'Holt', N'Roger', CAST(0x000097F100000000 AS DateTime), NULL)
INSERT [dbo].[Person] ([PersonID], [LastName], [FirstName], [HireDate], [EnrollmentDate]) VALUES (7, N'Martin', N'Randall', CAST(0x00008B1A00000000 AS DateTime), NULL)
SET IDENTITY_INSERT [dbo].[Person] OFF
SET IDENTITY_INSERT [dbo].[StudentGrade] ON

INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (1, N'C1045', 1, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (2, N'C1045', 2, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (3, N'C1045', 3, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (4, N'C1045', 4, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (5, N'C1045', 5, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (6, N'C1061', 1, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (7, N'C1061', 3, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (8, N'C1061', 4, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (9, N'C1061', 5, CAST(1.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (10, N'C2021', 1, CAST(2.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (11, N'C2021', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (12, N'C2021', 4, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (13, N'C2021', 5, CAST(3.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (14, N'C2042', 1, CAST(2.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (15, N'C2042', 2, CAST(3.50 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (16, N'C2042', 3, CAST(4.00 AS Decimal(3, 2)))
INSERT [dbo].[StudentGrade] ([EnrollmentID], [CourseID], [StudentID], [Grade]) VALUES (17, N'C2042', 5, CAST(3.00 AS Decimal(3, 2)))
SET IDENTITY_INSERT [dbo].[StudentGrade] OFF
ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
ALTER TABLE [dbo].[StudentGrade]  WITH CHECK ADD  CONSTRAINT [FK_StudentGrade_Student] FOREIGN KEY([StudentID])
REFERENCES [dbo].[Person] ([PersonID])
GO
ALTER TABLE [dbo].[StudentGrade] CHECK CONSTRAINT [FK_StudentGrade_Student]
GO
```

A continuación se incluye la lista de código:

[!code-csharp[SqlClient_RetrieveIdentity#1](~/../sqlclient/doc/samples/SqlClient_RetrieveIdentity.cs#1)]

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
