---
title: Compatibilidad con streaming de SqlClient
description: Explica cómo escribir aplicaciones que transmiten por secuencias datos de SQL Server sin cargarlos totalmente en memoria.
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744456"
---
# <a name="sqlclient-streaming-support"></a>Compatibilidad con streaming de SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Gracias a la compatibilidad con streaming entre SQL Server y una aplicación se admiten datos no estructurados en el servidor (documentos, imágenes y archivos multimedia). Una base de datos de SQL Server puede almacenar objetos binarios grandes (BLOB), pero la recuperación de BLOB puede usar mucha memoria.

La compatibilidad con streaming hacia y desde SQL Server simplifica la escritura de aplicaciones que transmiten datos, sin necesidad de cargar totalmente los datos en la memoria, lo que da como resultado menos excepciones de desbordamiento de memoria.

La compatibilidad con streaming también posibilita que las aplicaciones de nivel intermedio se escalen mejor, especialmente en escenarios donde los objetos empresariales se conectan a Azure SQL para enviar, recuperar y manipular BLOB grandes.

> [!WARNING]
> Los miembros que admiten streaming se usan para recuperar datos de consultas y para pasar parámetros a consultas y procedimientos almacenados. La característica de streaming aborda escenarios básicos de migración de datos y OLTP, y es aplicable a entornos de migración de datos locales y externos.

## <a name="streaming-support-from-sql-server"></a>Compatibilidad con streaming desde SQL Server

La compatibilidad con streaming desde SQL Server presenta una nueva funcionalidad en las clases <xref:System.Data.Common.DbDataReader> y <xref:Microsoft.Data.SqlClient.SqlDataReader> para obtener objetos <xref:System.IO.Stream>, <xref:System.Xml.XmlReader> y <xref:System.IO.TextReader> y reaccionar ante ellos. Estas clases se usan para recuperar datos de consultas. Como resultado, la compatibilidad con streaming de SQL Server aborda los escenarios OLTP y se aplica a entornos locales y externos.

Los miembros siguientes se agregaron a <xref:Microsoft.Data.SqlClient.SqlDataReader> para habilitar la compatibilidad con streaming desde SQL Server:

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

Los miembros siguientes se agregaron a <xref:System.Data.Common.DbDataReader> para habilitar la compatibilidad con streaming desde SQL Server:

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>Compatibilidad con streaming hacia SQL Server

La compatibilidad con streaming hacia SQL Server está en la clase <xref:Microsoft.Data.SqlClient.SqlParameter>, por lo que puede aceptar objetos <xref:System.Xml.XmlReader>, <xref:System.IO.Stream> y <xref:System.IO.TextReader> y reaccionar ante ellos. <xref:Microsoft.Data.SqlClient.SqlParameter> se usa para pasar parámetros a consultas y procedimientos almacenados.

> [!NOTE]
> Al desechar un objeto <xref:Microsoft.Data.SqlClient.SqlCommand> o llamar a <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> se cancela cualquier operación de streaming. Si una aplicación envía <xref:System.Threading.CancellationToken>, la cancelación no se puede garantizar.

Los siguientes tipos <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> aceptarán <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.IO.Stream>:

- **Binario**

- **VarBinary**

Los siguientes tipos <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> aceptarán <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.IO.TextReader>:

- **Char**

- **NChar**

- **NVarChar**

- **Xml**

El tipo **Xml**<xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> aceptará <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> de <xref:System.Xml.XmlReader>.

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> puede aceptar valores de tipo <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> y <xref:System.IO.Stream>.

Los objetos <xref:System.Xml.XmlReader>, <xref:System.IO.TextReader> y <xref:System.IO.Stream> se transferirán al valor definido por <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A>.

## <a name="sample----streaming-from-sql-server"></a>Ejemplo: streaming desde SQL Server

Use el siguiente Transact-SQL para crear la base de datos de ejemplo:

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

El ejemplo muestra cómo hacer lo siguiente:

- Evitar bloquear un subproceso de interfaz de usuario al proporcionar una manera asincrónica para recuperar archivos grandes.

- Transferir un archivo de texto grande desde SQL Server en .NET.

- Transferir un archivo XML grande desde SQL Server en .NET.

- Recuperar datos desde SQL Server.

- Transferir archivos grandes (BLOB) desde una base de datos de SQL Server a otra sin quedarse sin memoria.

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>Ejemplo: streaming hacia SQL Server

Use el siguiente Transact-SQL para crear la base de datos de ejemplo:

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

El ejemplo muestra cómo hacer lo siguiente:

- Transferir un BLOB grande hacia SQL Server en .NET.

- Transferir un archivo de texto grande hacia SQL Server en .NET.

- Usar la nueva característica asincrónica para transferir un BLOB grande.

- Usar la nueva característica asincrónica y la palabra clave await para transferir un BLOB grande.

- Cancelar la transferencia de un BLOB grande.

- Streaming desde una instancia de SQL Server a otra mediante la característica asincrónica.

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>Ejemplo: streaming desde una instancia de SQL Server a otra instancia de SQL Server

En este ejemplo se muestra cómo realizar el streaming de un BLOB grande de forma asincrónica desde una instancia de SQL Server a otra, con compatibilidad para cancelación.

> [!NOTE]
> Antes de ejecutar el ejemplo siguiente, asegúrese de que las bases de datos Demo y Demo2 están creadas.

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
