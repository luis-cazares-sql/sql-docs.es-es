---
title: Asignaciones de tipos de datos de SQL Server
description: Obtenga información sobre la asignación entre los distintos sistemas de tipos para SQL Server y .NET Framework. En este artículo se resume el modo en que los sistemas interactúan en el proveedor de datos SqlClient de Microsoft para SQL Server.
ms.date: 11/13/2020
ms.assetid: fafdc31a-f435-4cd3-883f-1dfadd971277
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3cf6d932a3f458e971d827c1ca4c493c298c2241
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771394"
---
# <a name="sql-server-data-type-mappings"></a>Asignaciones de tipos de datos de SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

SQL Server y .NET Framework se basan en sistemas de tipos distintos. Por ejemplo, la estructura <xref:System.Decimal> de .NET Framework tiene una escala máxima de 28, mientras que los tipos de datos decimal y numérico de SQL Server tienen una escala máxima de 38. Para mantener la integridad de los datos al leer y escribir datos, <xref:Microsoft.Data.SqlClient.SqlDataReader> expone métodos de descriptores de acceso con tipo específicos de SQL Server que devuelven objetos de <xref:System.Data.SqlTypes> así como métodos de descriptores de acceso que devuelven tipos de .NET Framework. Los tipos de SQL Server y los de .NET Framework se representan también mediante enumeraciones en las clases <xref:System.Data.DbType> y <xref:System.Data.SqlDbType>, que puede usar al especificar los tipos de datos <xref:Microsoft.Data.SqlClient.SqlParameter>.

En la tabla siguiente se muestra el tipo de .NET Framework deducido, las enumeraciones <xref:System.Data.DbType> y <xref:System.Data.SqlDbType> y los métodos de descriptor de acceso para <xref:Microsoft.Data.SqlClient.SqlDataReader>.

|Tipo de motor de base de datos SQL Server|Tipo de .NET Framework|Enumeración SqlDbType|Descriptor de acceso con tipo SqlDataReader SqlTypes|Enumeración DbType|Descriptor de acceso con tipo SqlDataReader DbType|  
|-------------------------------------|-------------------------|---------------------------|-------------------------------------------|------------------------|-----------------------------------------|  
|bigint|Int64|<xref:System.Data.SqlDbType.BigInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt64%2A>|<xref:System.Data.DbType.Int64>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt64%2A>|  
|binary|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|bit|Boolean|<xref:System.Data.SqlDbType.Bit>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBoolean%2A>|<xref:System.Data.DbType.Boolean>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBoolean%2A>|  
|char|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.Char>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.AnsiStringFixedLength>,<br /><br /> <xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|date <sup>1</sup><br /><br /> (SQL Server 2008 y posteriores)|DateTime|<xref:System.Data.SqlDbType.Date> <sup>1</sup>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.Date> <sup>1</sup>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|datetime|DateTime|<xref:System.Data.SqlDbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|datetime2<br /><br /> (SQL Server 2008 y posteriores)|DateTime|<xref:System.Data.SqlDbType.DateTime2>|Ninguno|<xref:System.Data.DbType.DateTime2>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|datetimeoffset<br /><br /> (SQL Server 2008 y posteriores)|DateTimeOffset|<xref:System.Data.SqlDbType.DateTimeOffset>|ninguno|<xref:System.Data.DbType.DateTimeOffset>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTimeOffset%2A>|  
|Decimal|Decimal|<xref:System.Data.SqlDbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|FILESTREAM attribute (varbinary(max))|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBytes%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|FLOAT|Double|<xref:System.Data.SqlDbType.Float>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDouble%2A>|<xref:System.Data.DbType.Double>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDouble%2A>|  
|imagen|Byte[]|<xref:System.Data.SqlDbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|int|Int32|<xref:System.Data.SqlDbType.Int>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt32%2A>|<xref:System.Data.DbType.Int32>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt32%2A>|  
|money|Decimal|<xref:System.Data.SqlDbType.Money>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlMoney%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|NCHAR|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.StringFixedLength>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|ntext|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NText>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|NUMERIC|Decimal|<xref:System.Data.SqlDbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|NVARCHAR|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.NVarChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|real|Single|<xref:System.Data.SqlDbType.Real>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlSingle%2A>|<xref:System.Data.DbType.Single>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetFloat%2A>|  
|rowversion|Byte[]|<xref:System.Data.SqlDbType.Timestamp>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|smalldatetime|DateTime|<xref:System.Data.SqlDbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDateTime%2A>|<xref:System.Data.DbType.DateTime>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|SMALLINT|Int16|<xref:System.Data.SqlDbType.SmallInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlInt16%2A>|<xref:System.Data.DbType.Int16>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetInt16%2A>|  
|SMALLMONEY|Decimal|<xref:System.Data.SqlDbType.SmallMoney>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlMoney%2A>|<xref:System.Data.DbType.Decimal>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDecimal%2A>|  
|sql_variant|Object <sup>2</sup>|<xref:System.Data.SqlDbType.Variant>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlValue%2A> <sup>2</sup>|<xref:System.Data.DbType.Object>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetValue%2A> <sup>2</sup>|  
|text|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.Text>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|time<br /><br /> (SQL Server 2008 y posteriores)|TimeSpan|<xref:System.Data.SqlDbType.Time>|ninguno|<xref:System.Data.DbType.Time>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetDateTime%2A>|  
|timestamp|Byte[]|<xref:System.Data.SqlDbType.Timestamp>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|TINYINT|Byte|<xref:System.Data.SqlDbType.TinyInt>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlByte%2A>|<xref:System.Data.DbType.Byte>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetByte%2A>|  
|UNIQUEIDENTIFIER|Guid|<xref:System.Data.SqlDbType.UniqueIdentifier>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlGuid%2A>|<xref:System.Data.DbType.Guid>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetGuid%2A>|  
|varbinary|Byte[]|<xref:System.Data.SqlDbType.VarBinary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlBinary%2A>|<xref:System.Data.DbType.Binary>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetBytes%2A>|  
|varchar|String<br /><br /> Char[]|<xref:System.Data.SqlDbType.VarChar>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlString%2A>|<xref:System.Data.DbType.AnsiString>, <xref:System.Data.DbType.String>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetString%2A><br /><br /> <xref:Microsoft.Data.SqlClient.SqlDataReader.GetChars%2A>|  
|Xml|Xml|<xref:System.Data.SqlDbType.Xml>|<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlXml%2A>|<xref:System.Data.DbType.Xml>|ninguno|

<sup>1</sup> No puede establecer la propiedad `DbType` de `SqlParameter` en `SqlDbType.Date`.  
<sup>2</sup> Utilice un descriptor de acceso con tipo si conoce el tipo subyacente de `sql_variant`.

## <a name="sql-server-documentation"></a>SQL Server, documentación

Para obtener información sobre los tipos de datos de SQL Server, vea [Tipos de datos (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql).

## <a name="see-also"></a>Vea también

- [Tipos de datos de SQL Server en ADO.NET](./sql/sql-server-data-types.md)
- [Datos binarios y datos de valores grandes de SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Configuración de parámetros](configure-parameters.md)
- [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md)
