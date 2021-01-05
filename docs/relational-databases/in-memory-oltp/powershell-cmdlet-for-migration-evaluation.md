---
title: Cmdlet de PowerShell para la evaluación de la migración | Microsoft Docs
description: Obtenga información sobre Save-SqlMigrationReport, que evalúa la idoneidad de la migración de objetos en una base de datos de SQL Server para OLTP en memoria.
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 78b4d84a98d7ea2c753a673a222a1a19e0b7e5e2
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641299"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>Cmdlet de PowerShell para la evaluación de la migración

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

El cmdlet `Save-SqlMigrationReport` es una herramienta que evalúa la idoneidad de la migración de varios objetos en una base de datos de SQL Server.

Actualmente, este cmdlet se limita a evaluar la idoneidad de la migración para OLTP en memoria. El cmdlet puede ejecutarse en un entorno de Windows PowerShell con privilegios elevados y sqlps.

Como alternativa a la ejecución directa de este cmdlet de PowerShell, puede ejecutar el cmdlet de manera implícita mediante SQL Server Management Studio (SSMS). En el **Explorador de objetos** de SSMS, puede hacer clic con el botón derecho en una tabla y, luego, hacer clic en **Asesor de optimización de memoria**.

## <a name="syntax"></a>Sintaxis

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>Parámetros

En la siguiente tabla se describen los parámetros.

Hay aspectos de sintaxis que se deben destacar. Si especifica el parámetro `-InputObject`, no puede especificar ninguno de los parámetros siguientes:

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

Por el contrario, si _no_ especifica `-InputObject`, debe especificar `-Server` y `-Database`. Si especifica `-Server`, tiene la opción de restringir el ámbito especificando `-Schema` o `-Object`, o ambos.

| Nombre de parámetro | Descripción |
| :------------- | :---------- |
| Base de datos | El nombre de la base de datos de SQL Server de destino. Obligatorio cuando `-Server` es obligatorio.<br/><br/> En SQLPS es opcional. |
| FolderPath | La carpeta en la que el cmdlet debe almacenar los informes generados.<br/><br/> Necesario. |
| InputObject | El objeto SMO al que se debe dirigir el cmdlet.<br/><br/> Obligatorio en el entorno de Windows PowerShell si no se proporciona `-Server`.<br/><br/> En SQLPS es opcional. |
| MigrationType | El tipo de escenario de migración al que se dirige el cmdlet. Actualmente, el único valor es el **"OLTP"** predeterminado.<br/><br/> Opcional. |
| Object | Nombre del objeto sobre el que se va a informar. Puede ser una tabla o un procedimiento almacenado. |
| Contraseña | Requerido cuando `-Username` es requerido. |
| Schema | Nombre del esquema al que pertenece el objeto sobre el que se va a informar.<br/><br/> Opcional. |
| Server | El nombre de la instancia de SQL Server de destino. Obligatorio en el entorno de Windows PowerShell si no se proporciona el parámetro `-InputObject`.<br/><br/> En SQLPS es opcional. |
| Nombre de usuario | Requerido al conectarse a través de la autenticación de SQL Server, en contraposición a la autenticación de Windows. En caso contrario, se omite. |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>Requisitos previos

Antes de poder ejecutar este cmdlet, primero debe instalar el módulo denominado **SqlServer**:

- `Install-Module -Name SqlServer`

> [!NOTE]
> Ya no se mantiene el módulo anterior `SQLPS`. Use el módulo `SqlServer` más reciente.

Para más información, consulte [Instalación de un módulo de SQL Server PowerShell](../../powershell/download-sql-server-ps-module.md).

## <a name="example-cmdlet-line"></a>Ejemplo de línea de cmdlet

A continuación se muestra la línea de cmdlet real que se ejecutó para generar el informe que se muestra más adelante en este artículo.

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>Informe de salida de ejemplo

En la carpeta especificada para el parámetro `-FolderPath`, se crean las siguientes dos rutas de acceso de carpeta mediante la ejecución de este cmdlet. Ambas rutas comienzan con el valor _nombre\_servidor_:

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

Cada archivo de informe de objeto se almacena en la carpeta correspondiente.

Los nombres de archivo de los informes tienen la extensión **.html**. Por ejemplo, un nombre de archivo real generado era **MigrationAdvisorChecklistReport_Table2_20190728.html**.

El archivo HTML es principalmente una tabla de dos columnas con los encabezados siguientes:

- Descripción
- Resultado de la validación

A continuación, se presenta un ejemplo real del informe HTML para una tabla.

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

Y a continuación, se ve una aproximación de la apariencia de la tabla.

| Descripción | Resultado de la validación |
| :---------- | :---------------- |
| No hay tipos de datos no admitidos definidos en esta tabla. | Correcto |
| No hay columnas dispersas definidas para esta tabla. | Correcto |
| No hay columnas de identidad con valor de inicialización e incremento no admitidos definidas para esta tabla. | Correcto |
| No hay relaciones de clave externa definidas en esta tabla. | Correcto |
| No hay restricciones no admitidas definidas en esta tabla. | Correcto |
| No hay índices no admitidos definidos en esta tabla. | Correcto |
| No hay desencadenadores no admitidos definidos en esta tabla. | Correcto |
| El tamaño de fila después de la migración no supera el límite de tamaño de fila de las tablas optimizadas para memoria. | Correcto |
| La tabla no tiene particiones ni se ha replicado. | Correcto |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>Vínculos relacionados

- Documentación de referencia: [Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport)
