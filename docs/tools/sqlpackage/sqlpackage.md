---
title: SqlPackage.exe
description: Obtenga información sobre cómo automatizar las tareas de desarrollo de bases de datos con SqlPackage.exe. Vea ejemplos y los parámetros, las propiedades y las variables SQLCMD disponibles.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: 16c4200b70647c08ddee6b531acc3227d2942ad2
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577901"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** es una utilidad de línea de comandos que automatiza las siguientes tareas de desarrollo de base de datos:  
  
- [Versión](#version): devuelve el número de compilación de la aplicación SqlPackage.  Se agregó en la versión 18.6.

- [Extract](sqlpackage-extract.md): crea un archivo de instantáneas de base de datos (.dacpac) a partir de una base de datos activa de SQL Server o Azure SQL Database.  
  
- [Publicar](sqlpackage-publish.md): actualiza de forma incremental un esquema de la base de datos para coincidir con el esquema de un archivo .dacpac de origen. Si no existe la base de datos en el servidor, la operación de publicación la crea. De lo contrario, se actualiza una base de datos existente.  
  
- [Export](sqlpackage-export.md): exporta una base de datos activa, incluidos los datos de usuario y el esquema de base de datos, desde SQL Server o Azure SQL Database a un paquete BACPAC (archivo.bacpac).  
  
- [Import](sqlpackage-import.md): importa los datos de esquema y tabla de un paquete BACPAC en una nueva base de datos de usuario en una instancia de SQL Server o Azure SQL Database.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md): crea un informe XML de los cambios que realizaría una acción de publicación.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md): crea un informe XML de los cambios que se han realizado en una base de datos registrada desde que se registró por última vez.  
  
- [Script](sqlpackage-script.md): crea un script de actualización incremental de Transact-SQL que actualiza el esquema de un destino para que coincida con el esquema de un origen.  
  
La línea de comandos **SqlPackage.exe** permite especificar estas acciones junto con parámetros y propiedades específicos de cada acción.  

**[Descargar la última versión](sqlpackage-download.md)** . Para más información sobre la última versión, consulte las [notas de la versión](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Sintaxis de línea de comandos

**SqlPackage.exe** inicia las acciones especificadas usando los parámetros, las propiedades y las variables de SQLCMD especificadas en la línea de comandos.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Ejemplos de uso

**Generación de una comparación entre las bases de datos mediante archivos .dacpac con una salida de script SQL**

Empiece por crear un archivo .dacpac de los cambios de la base de datos más recientes:

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Cree un archivo .dacpac del destino de la base de datos (sin cambios):

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Cree un script SQL que genere las diferencias de dos archivos .dacpac:

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```


## <a name="version"></a>Versión

Muestra la versión de sqlpackage como un número de compilación.  Se puede usar en solicitudes interactivas o en [canalizaciones automatizadas](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Códigos de salida

Comandos que devuelven los siguientes códigos de salida:

- 0 = success
- distinto de cero = error


## <a name="next-steps"></a>Pasos siguientes

- Mas información sobre [Extract de SqlPackage](sqlpackage-extract.md)
- Mas información sobre [Publish de SqlPackage](sqlpackage-publish.md)
- Mas información sobre [Export de SqlPackage](sqlpackage-export.md)
- Mas información sobre [Import de SqlPackage](sqlpackage-import.md)
