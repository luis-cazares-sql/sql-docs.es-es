---
description: Replication System Stored Procedures Concepts
title: Conceptos sobre los procedimientos almacenados del sistema de replicación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server replication], programming
- programming [SQL Server replication], system stored procedures
- programming interfaces [SQL Server replication]
- system stored procedures [SQL Server replication]
- replication [SQL Server], how-to topics
ms.assetid: 816d2bda-ed72-43ec-aa4d-7ee3dc25fd8a
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: eb082b29411b136edca7ed14f06effa354a5f0e6
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643109"
---
# <a name="replication-system-stored-procedures-concepts"></a>Replication System Stored Procedures Concepts
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]

  En [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], el acceso mediante programación a toda la funcionalidad configurable por el usuario en una topología de replicación se proporciona mediante procedimientos almacenados del sistema. Aunque los procedimientos almacenados se pueden ejecutar individualmente utilizando [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o el programa de línea de comandos sqlcmd, puede ser beneficioso escribir archivos de script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] que se pueden ejecutar para realizar una secuencia lógica de tareas de replicación.  
  
 Las tareas de replicación para scripting proporcionan las ventajas siguientes:  
  
-   Se mantiene una copia permanente de los pasos que se usan para implementar la topología de replicación.  
  
-   Se usa un único script para configurar varios suscriptores.  
  
-   Se instruye rápidamente a los nuevos administradores de bases de datos permitiéndoles evaluar, entender, cambiar o solucionar problemas del código.  
  
    > [!IMPORTANT]  
    >  Los scripts pueden ser fuente de vulnerabilidades de la seguridad, ya que pueden invocar funciones del sistema sin la intervención ni el conocimiento del usuario y contener credenciales de seguridad en texto simple. Antes de usarlos, compruebe los aspectos siguientes de la seguridad de los scripts.  
  
## <a name="creating-replication-scripts"></a>Crear scripts de replicación  
 Desde el punto de vista de la replicación, un script es una serie de una o varias instrucciones de [!INCLUDE[tsql](../../../includes/tsql-md.md)] que cada una ejecuta un procedimiento almacenado de replicación. Los scripts son archivos de texto, a menudo con la extensión .sql, que se pueden ejecutar utilizando la utilidad sqlcmd. Cuando se ejecuta un archivo de script, la utilidad ejecuta las instrucciones de SQL almacenadas en él. De igual forma, un script puede almacenarse como un objeto de consulta en un proyecto de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 Los scripts de replicación se pueden crear de las maneras siguientes:  
  
-   Cree el script manualmente.  
  
-   Use las características de generación de script que se proporcionan en los asistentes de replicación o  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Para más información, consulte [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
-   Utilice Replication Management Objects (RMO) para generar mediante programación el script y crear un objeto RMO.  
  
 Al crear manualmente los scripts de replicación, tenga presente las consideraciones siguientes:  
  
-   Los scripts [!INCLUDE[tsql](../../../includes/tsql-md.md)] constan de uno o varios lotes. El comando GO señala el final de un lote. Si un script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] no contiene ningún comando GO, se ejecutará como un único lote.  
  
-   Al ejecutar varios procedimientos almacenados de la replicación en un único lote, después del primer procedimiento, la palabra clave EXECUTE debe preceder todos a los procedimientos subsiguientes en el lote.  
  
-   Todos los procedimientos almacenados en un lote deben compilarse antes de que se ejecute un lote. Sin embargo, una vez compilado el lote y creado un plan de ejecución, un error de tiempo de ejecución puede aparecer o no.  
  
-   Al crear scripts para configurar la replicación, debería utilizar la autenticación de Windows para evitar almacenar las credenciales de seguridad en el archivo de script. Si debe almacenar las credenciales en un archivo de script, proteja el archivo para evitar el acceso no autorizado.  
  
## <a name="sample-replication-script"></a>Ejemplo de script de replicación  
 El script siguiente se puede ejecutar para configurar la publicación y distribución en un servidor.  
  
```  
-- This script uses sqlcmd scripting variables. They are in the form  
-- $(MyVariable). For information about how to use scripting variables    
-- on the command line and in SQL Server Management Studio, see the   
-- "Executing Replication Scripts" section in the topic  
-- "Programming Replication Using System Stored Procedures".  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
-- Specify the replication working directory.  
SET @directory = N'\\' + $(DistPubServer) + '\repldata';  
-- Specify the publication database.  
SET @publicationDB = N'AdventureWorks2012';   
  
-- Install the server MYDISTPUB as a Distributor using the defaults,  
-- including autogenerating the distributor password.  
USE master  
EXEC sp_adddistributor @distributor = @distributor;  
  
-- Create a new distribution database using the defaults, including  
-- using Windows Authentication.  
USE master  
EXEC sp_adddistributiondb @database = @distributionDB,   
    @security_mode = 1;  
GO  
  
-- Create a Publisher and enable AdventureWorks2012 for replication.  
-- Add MYDISTPUB as a publisher with MYDISTPUB as a local distributor  
-- and use Windows Authentication.  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
USE [distribution]  
EXEC sp_adddistpublisher @publisher=@publisher,   
    @distribution_db=@distributionDB,   
    @security_mode = 1;  
GO  
  
```  
  
 Este script puede guardarse entonces localmente como `instdistpub.sql` para que se pueda ejecutar o volver a ejecutar cuando sea necesario.  
  
 El script anterior incluye variables de scripting de **sqlcmd**, que se usan en muchos de los ejemplos de código de replicación de los Libros en pantalla de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Las variables de scripting se definen con la sintaxis `$(MyVariable)`. Los valores para las variables se pueden pasar a un script en la línea de comandos o en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Para obtener más información, consulte la sección siguiente en este tema, "Ejecutar scripts de replicación".  
  
## <a name="executing-replication-scripts"></a>Ejecutar scripts de replicación  
 Una vez creado, un script de replicación se puede ejecutar de alguna de las maneras siguientes:  
  
### <a name="creating-a-sql-query-file-in-sql-server-management-studio"></a>Crear un archivo de SQL Query en SQL Server Management Studio  
 Un archivo de script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] de replicación se puede crear como un archivo SQL de SQL Query en un proyecto de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Una vez escrito el script, se puede realizar una conexión a la base de datos para este archivo de consulta y se puede ejecutar el script. Para obtener más información sobre cómo crear scripts de [!INCLUDE[tsql](../../../includes/tsql-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], vea [Editores de consultas y texto &#40;SQL Server Management Studio&#41;](../../../ssms/f1-help/database-engine-query-editor-sql-server-management-studio.md).  
  
 Para usar un script que incluya variables de scripting, [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] debe estar ejecutándose en modo **sqlcmd**. En el modo **sqlcmd**, el Editor de consultas acepta una sintaxis adicional concreta de **sqlcmd**, como `:setvar`, que se usa como valor de una variable. Para obtener más información sobre el modo **sqlcmd**, vea [Modificar scripts SQLCMD con el Editor de consultas](../../../ssms/scripting/edit-sqlcmd-scripts-with-query-editor.md). En el script siguiente se usa `:setvar` para proporcionar un valor para la variable `$(DistPubServer)`.  
  
```  
:setvar DistPubServer N'MyPublisherAndDistributor';  
  
-- Install the Distributor and the distribution database.  
DECLARE @distributor AS sysname;  
DECLARE @distributionDB AS sysname;  
DECLARE @publisher AS sysname;  
DECLARE @directory AS nvarchar(500);  
DECLARE @publicationDB AS sysname;  
-- Specify the Distributor name.  
SET @distributor = $(DistPubServer);  
-- Specify the distribution database.  
SET @distributionDB = N'distribution';  
-- Specify the Publisher name.  
SET @publisher = $(DistPubServer);  
  
--  
-- Additional code goes here  
--  
```  
  
### <a name="using-the-sqlcmd-utility-from-the-command-line"></a>Usar la utilidad sqlcmd desde la línea de comandos  
 El ejemplo siguiente muestra cómo se usa la línea de comandos para ejecutar el archivo de script `instdistpub.sql` mediante la [utilidad sqlcmd](../../../tools/sqlcmd-utility.md):  
  
```  
sqlcmd.exe -E -S sqlserverinstance -i C:\instdistpub.sql -o C:\output.log -v DistPubServer="N'MyDistributorAndPublisher'"  
```  
  
 En este ejemplo, el modificador `-E` indica que al conectarse a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se utiliza la autenticación de Windows. Al usar la autenticación de Windows, no hay necesidad de almacenar un nombre de usuario y una contraseña en el archivo de script. El modificador `-i` especifica el nombre y la ruta de acceso del archivo de script y el modificador `-o` especifica el nombre del archivo de salida (cuando se utiliza este modificador, la salida de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se escribe en este archivo en lugar de en la consola). La utilidad `sqlcmd` le permite pasar las variables de scripting a un script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] en tiempo de ejecución utilizando el modificador `-v`. En este ejemplo, `sqlcmd` reemplaza cada instancia de `$(DistPubServer)` en el script con el valor `N'MyDistributorAndPublisher'` antes de la ejecución.  
  
> [!NOTE]  
>  El modificador `-X` deshabilita las variables de scripting.  
  
### <a name="automating-tasks-in-a-batch-file"></a>Automatizar tareas en un archivo por lotes  
 Mediante un archivo por lotes, las tareas de administración de replicación, las tareas de sincronización de replicación y otras diversas se pueden automatizar en el mismo archivo por lotes. El archivo por lotes siguiente usa la utilidad **sqlcmd** para quitar y volver a crear la base de datos de suscripciones y agregar una suscripción de extracción de mezcla. A continuación, el archivo invoca al agente de mezcla para sincronizar la nueva suscripción:  
  
```  
REM ----------------------Script to synchronize merge subscription ----------------------  
REM -- Creates subscription database and   
REM -- synchronizes the subscription to MergeSalesPerson.  
REM -- Current computer acts as both Publisher and Subscriber.  
REM -------------------------------------------------------------------------------------  
  
SET Publisher=%computername%  
SET Subscriber=%computername%  
SET PubDb=AdventureWorks  
SET SubDb=AdventureWorksReplica  
SET PubName=AdvWorksSalesOrdersMerge  
  
REM -- Drop and recreate the subscription database at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE master IF EXISTS (SELECT * FROM sysdatabases WHERE name='%SubDb%' ) DROP DATABASE %SubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE master CREATE DATABASE %SubDb%"  
  
REM -- Add a pull subscription at the Subscriber  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb% EXEC sp_addmergepullsubscription @publisher = %Publisher%, @publication = %PubName%, @publisher_db = %PubDb%"  
sqlcmd /S%Subscriber% /E /Q"USE %SubDb%  EXEC sp_addmergepullsubscription_agent @publisher = %Publisher%, @publisher_db = %PubDb%, @publication = %PubName%, @subscriber = %Subscriber%, @subscriber_db = %SubDb%, @distributor = %Publisher%"  
  
REM -- This batch file starts the merge agent at the Subscriber to   
REM -- synchronize a pull subscription to a merge publication.  
REM -- The following must be supplied on one line.  
"\Program Files\Microsoft SQL Server\130\COM\REPLMERG.EXE"  -Publisher  %Publisher% -Subscriber  %Subscriber%  -Distributor %Publisher%  -PublisherDB  %PubDb% -SubscriberDB %SubDb% -Publication %PubName% -PublisherSecurityMode 1 -OutputVerboseLevel 1  -Output  -SubscriberSecurityMode 1  -SubscriptionType 1 -DistributorSecurityMode 1 -Validate 3  
  
```  
  
## <a name="scripting-common-replication-tasks"></a>Incluir en script tareas de replicación comunes  
 Las siguientes son algunas de las tareas de replicación más comunes que se pueden incluir en scripts utilizando procedimientos almacenados del sistema:  
  
-   Configurar la publicación y la distribución  
  
-   Modificar las propiedades del distribuidor y del publicador  
  
-   Deshabilitar la publicación y la distribución  
  
-   Crear publicaciones y definir artículos  
  
-   Eliminar publicaciones y artículos  
  
-   Crear una suscripción de extracción  
  
-   Modificar una suscripción de extracción  
  
-   Eliminar una suscripción de extracción  
  
-   Crear una suscripción de inserción  
  
-   Modificar una suscripción de inserción  
  
-   Eliminar una suscripción de inserción  
  
-   Sincronizar una suscripción de extracción  
  
## <a name="see-also"></a>Consulte también  
 [Conceptos de la programación de replicación](../../../relational-databases/replication/concepts/replication-programming-concepts.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Crear script para la replicación](../../../relational-databases/replication/scripting-replication.md)  
  
