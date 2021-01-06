---
title: Usar el formato nativo Unicode para importar o exportar datos (SQL Server) | Microsoft Docs
description: Use el formato nativo Unicode para la transferencia masiva de datos entre instancias de SQL Server, lo que elimina la conversión de tipos de datos entre formatos de caracteres.
ms.custom: ''
ms.date: 09/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- Unicode [SQL Server], bulk importing and exporting
- data formats [SQL Server], Unicode native
ms.assetid: a6213308-f3d5-406e-9029-19d8bb3367f3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 977e7c4222a200d697304be0be774214fbc4376a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643473"
---
# <a name="use-unicode-native-format-to-import-or-export-data-sql-server"></a>Usar el formato nativo Unicode para importar o exportar datos (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
El formato nativo Unicode es útil cuando se debe copiar información de una instalación de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en otra. El uso del formato nativo para datos que no son caracteres permite ahorrar tiempo, además de evitar la conversión innecesaria de tipos de datos a y desde el formato de caracteres. El uso del formato de caracteres Unicode para todos los datos de caracteres evita la pérdida de caracteres extendidos durante la transferencia masiva de datos entre servidores que utilizan páginas de códigos diferentes. Los archivos de datos en formato nativo Unicode se pueden leer en cualquier método de importación masiva.  
  
 El formato nativo Unicode se recomienda para la transferencia masiva de datos entre varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante un archivo de datos que contenga caracteres extendidos o DBCS. Para los datos que no son caracteres, el formato nativo Unicode utiliza tipos de datos nativos (de base de datos). En el caso de los datos de caracteres, como [char](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), [text](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), [varchar (max)](../../t-sql/data-types/char-and-varchar-transact-sql.md), [nvarchar (max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)y [ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md), el formato nativo Unicode emplea el formato de datos de caracteres Unicode.  
  
 Los datos [sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md) que se almacenan como SQLVARIANT en un archivo de datos en formato nativo Unicode funcionan de la misma manera que en un archivo de datos en formato nativo, con la excepción de que los valores [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) y [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) se convierten en [nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md) y [nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md), lo que duplica la cantidad de espacio necesario para las columnas afectadas. Los metadatos originales se conservan y los valores se convierten de nuevo al tipo de datos [char](../../t-sql/data-types/char-and-varchar-transact-sql.md) y [varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md) original cuando se importan masivamente en la columna de una tabla.  
 
 |En este tema:|
|---|
|[Opciones de comandos para el formato nativo Unicode](#command_options)|
|[Condiciones de prueba de ejemplo](#etc)<br />&emsp;&#9679;&emsp;[Tabla de ejemplo](#sample_table)<br />&emsp;&#9679;&emsp;[Archivo de formato no XML de ejemplo](#nonxml_format_file)|
|[Ejemplos](#examples)<br />&emsp;&#9679;&emsp;[Usar bcp y el formato nativo Unicode para exportar datos](#bcp_widenative_export)<br />&emsp;&#9679;&emsp;[Usar bcp y el formato nativo Unicode para importar datos sin un archivo de formato](#bcp_widenative_import)<br />&emsp;&#9679;&emsp;[Usar bcp y el formato nativo Unicode para importar datos con un archivo de formato no XML](#bcp_widenative_import_fmt)<br />&emsp;&#9679;&emsp;[Usar BULK INSERT y el formato nativo Unicode sin un archivo de formato](#bulk_widenative)<br />&emsp;&#9679;&emsp;[Usar BULK INSERT y el formato nativo Unicode con un archivo de formato no XML](#bulk_widenative_fmt)<br />&emsp;&#9679;&emsp;[Usar OPENROWSET y el formato nativo Unicode con un archivo de formato no XML](#openrowset_widenative_fmt)|
|[Tareas relacionadas](#RelatedTasks)<p>                                                                                                                                                                                                                  </p>|
  
## <a name="command-options-for-unicode-native-format"></a>Opciones de comandos para el formato nativo Unicode<a name="command_options"></a>  
Puede importar datos en formato nativo Unicode en una tabla con [bcp](../../tools/bcp-utility.md), [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md) o [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md).  Para un comando [bcp](../../tools/bcp-utility.md) o una instrucción [BULK INSERT](../../t-sql/statements/bulk-insert-transact-sql.md), puede especificar el formato de datos en la instrucción.  Para una instrucción [INSERT ... SELECT * FROM OPENROWSET(BULK...)](../../t-sql/functions/openrowset-transact-sql.md), debe especificar el formato de datos en un archivo de formato.  
  
El formato nativo Unicode admite las siguientes opciones de comando:  
  
|Get-Help|Opción|Descripción|  
|-------------|------------|-----------------|  
|BCP|**-N**|Hace que la utilidad **bcp** use el formato nativo Unicode, que emplea tipos de datos nativos (de base de datos) para todos los datos que no son caracteres y el formato de datos de caracteres Unicode para todos los datos de caracteres (**char**, **nchar**, **varchar**, **nvarchar**, **text** y **ntext**).|  
|BULK INSERT|DATAFILETYPE **='widenative'**|Usa el formato nativo Unicode al importar datos masivamente.|  
|OPENROWSET|N/D|Debe usar un archivo de formato|
    
> [!NOTE]
>  Otra posibilidad es especificar el formato por campo en un archivo de formato. Para obtener más información, vea [Archivos de formato para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).
  
## <a name="example-test-conditions"></a>Condiciones de prueba de ejemplo<a name="etc"></a>  
Los ejemplos de este tema se basan en la tabla y en el archivo de formato definidos a continuación.

### <a name="sample-table"></a>**Tabla de ejemplo**<a name="sample_table"></a>
El siguiente script crea una base de datos de prueba, una tabla denominada `myWidenative` , y la rellena con algunos valores iniciales.  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
CREATE DATABASE TestDatabase;
GO

USE TestDatabase;
CREATE TABLE dbo.myWidenative ( 
    PersonID smallint NOT NULL,
    FirstName nvarchar(25) NOT NULL,
    LastName nvarchar(30) NOT NULL,
    BirthDate date,
    AnnualSalary money
);

-- Populate table
INSERT TestDatabase.dbo.myWidenative
VALUES 
(1, N'ϴAnthony', N'Grosse', '02-23-1980', 65000.00),
(2, N'❤Alica', N'Fatnowna', '11-14-1963', 45000.00),
(3, N'☎Stella', N'Rossenhain', '03-02-1992', 120000.00);

-- Review Data
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="sample-non-xml-format-file"></a>**Archivo de formato no XML de ejemplo**<a name="nonxml_format_file"></a>
SQL Server admite dos tipos de archivos de formato: XML y no XML.  El formato no XML es el formato original compatible con versiones anteriores de SQL Server.  Revise [Archivos de formato no XML (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md) para obtener información detallada.  El siguiente comando hará uso de la [utilidad BCP](../../tools/bcp-utility.md) para generar un archivo de formato no XML, `myWidenative.fmt`, basado en el esquema de `myWidenative`.  Si quiere usar un comando [BCP](../../tools/bcp-utility.md) para crear un archivo de formato, especifique el argumento **format** y use **null** en lugar de una ruta de acceso de archivo de datos.  La opción format también requiere la opción **-f** .  Además, en este ejemplo, el calificador **c** se usa para especificar los datos de caracteres, mientras que **T** se usa para especificar una conexión de confianza que usa la seguridad integrada.  En un símbolo del sistema, escriba los comandos siguientes:

```
bcp TestDatabase.dbo.myWidenative format nul -f D:\BCP\myWidenative.fmt -T -N

REM Review file
Notepad D:\BCP\myWidenative.fmt
```

> [!IMPORTANT]
> Asegúrese de que el archivo de formato no XML termina con un retorno de carro o avance de línea.  De lo contrario, es probable que reciba el mensaje de error siguiente:
> 
> `SQLState = S1000, NativeError = 0`  
> `Error = [Microsoft][ODBC Driver 13 for SQL Server]I/O error while reading BCP format file`

## <a name="examples"></a>Ejemplos<a name="examples"></a>
En los siguientes ejemplos se usan la base de datos y los archivos de formato creados anteriormente.

### <a name="using-bcp-and-unicode-native-format-to-export-data"></a>**Usar bcp y el formato nativo Unicode para exportar datos**<a name="bcp_widenative_export"></a>
Modificador **-N** y comando **OUT** .  Nota: el archivo de datos creado en este ejemplo se usará en todos los ejemplos siguientes.  En un símbolo del sistema, escriba los comandos siguientes:
```
bcp TestDatabase.dbo.myWidenative OUT D:\BCP\myWidenative.bcp -T -N

REM Review results
NOTEPAD D:\BCP\myWidenative.bcp
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-without-a-format-file"></a>**Usar bcp y el formato nativo Unicode para importar datos sin un archivo de formato**<a name="bcp_widenative_import"></a>
Modificador **-N** y comando **IN** .  En un símbolo del sistema, escriba los comandos siguientes:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -T -N

REM Review results is SSMS
```

### <a name="using-bcp-and-unicode-native-format-to-import-data-with-a-non-xml-format-file"></a>**Usar bcp y el formato nativo Unicode para importar datos con un archivo de formato no XML**<a name="bcp_widenative_import_fmt"></a>
Modificadores **-N** y **-f** switches y **IN** commy.  En un símbolo del sistema, escriba los comandos siguientes:
```
REM Truncate table (for testing)
SQLCMD -Q "TRUNCATE TABLE TestDatabase.dbo.myWidenative;"

REM Import data
bcp TestDatabase.dbo.myWidenative IN D:\BCP\myWidenative.bcp -f D:\BCP\myWidenative.fmt -T -N

REM Review results is SSMS
```

### <a name="using-bulk-insert-and-unicode-native-format-without-a-format-file"></a>**Usar BULK INSERT y el formato nativo Unicode sin un archivo de formato**<a name="bulk_widenative"></a>
Argumento **DATAFILETYPE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
    FROM 'D:\BCP\myWidenative.bcp'
    WITH (
        DATAFILETYPE = 'widenative'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-bulk-insert-and-unicode-native-format-with-a-non-xml-format-file"></a>**Usar BULK INSERT y el formato nativo Unicode con un archivo de formato no XML**<a name="bulk_widenative_fmt"></a>
Argumento **FORMATFILE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative; -- for testing
BULK INSERT TestDatabase.dbo.myWidenative
   FROM 'D:\BCP\myWidenative.bcp'
   WITH (
        FORMATFILE = 'D:\BCP\myWidenative.fmt'
        );

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

### <a name="using-openrowset-and-unicode-native-format-with-a-non-xml-format-file"></a>**Usar OPENROWSET y el formato nativo Unicode con un archivo de formato no XML**<a name="openrowset_widenative_fmt"></a>
Argumento **FORMATFILE** .  Ejecutar el siguiente Transact-SQL en Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (SSMS):
```sql
TRUNCATE TABLE TestDatabase.dbo.myWidenative;  -- for testing
INSERT INTO TestDatabase.dbo.myWidenative
    SELECT *
    FROM OPENROWSET (
        BULK 'D:\BCP\myWidenative.bcp', 
        FORMATFILE = 'D:\BCP\myWidenative.fmt'  
        ) AS t1;

-- review results
SELECT * FROM TestDatabase.dbo.myWidenative;
```

## <a name="related-tasks"></a>Tareas relacionadas<a name="RelatedTasks"></a>
Para usar formatos de datos para la importación o exportación masivas  
-   [Importar datos con formato nativo y de caracteres de versiones anteriores de SQL Server](../../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md)  
  
-   [Usar el formato de caracteres para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato nativo para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [Usar el formato de caracteres Unicode para importar o exportar datos &#40;SQL Server&#41;](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [bcp (utilidad)](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
