---
description: Mover bases de datos del sistema
title: Movimiento de bases de datos del sistema | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- moving system databases
- disaster recovery [SQL Server], moving database files
- database files [SQL Server], moving
- data files [SQL Server], moving
- tempdb database [SQL Server], moving
- system databases [SQL Server], moving
- scheduled disk maintenace [SQL Server]
- moving databases
- msdb database [SQL Server], moving
- moving database files
- relocating database files
- planned database relocations [SQL Server]
- master database [SQL Server], moving
- model database [SQL Server], moving
- Resource database [SQL Server]
- databases [SQL Server], moving
ms.assetid: 72bb62ee-9602-4f71-be51-c466c1670878
author: stevestein
ms.author: sstein
ms.openlocfilehash: c24a98684e87eb94a3cd9e100f203509789b7a0f
ms.sourcegitcommit: 4a813a0741502c56c0cd5ecaafafad2e857a9d7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98031114"
---
# <a name="move-system-databases"></a>Mover bases de datos del sistema
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo mover bases de datos del sistema en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mover bases de datos del sistema puede resultar útil en las situaciones siguientes:  
  
-   Recuperación de un error. Por ejemplo, la base de datos se encuentra en modo sospechoso o se ha cerrado a causa de un error de hardware.  
  
-   Reubicación planeada.  
  
-   Reubicación para el mantenimiento planeado del disco.  
  
 Los siguientes procedimientos se aplican para mover archivos de base de datos dentro de una misma instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para mover una base de datos a otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a otro servidor, use la operación [copia de seguridad y restauración](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md) .  

 Los procedimientos descritos en este tema requieren el nombre lógico de los archivos de la base de datos. Para obtener el nombre, consulte la columna de nombre de la vista de catálogo [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md) .  
  
> [!IMPORTANT]  
>  Si se mueve una base de datos del sistema y posteriormente se vuelve a generar la base de datos maestra, se debe mover de nuevo la base de datos del sistema porque la operación de regeneración instala todas las bases de datos del sistema en su ubicación predeterminada.  

> [!IMPORTANT]  
>  Después de mover los archivos, la cuenta del servicio de [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] debe tener permiso de acceso a los archivos en la nueva ubicación de la carpeta de archivos. Para más información, consulte [Configurar permisos del sistema de archivos para el acceso al motor de base de datos](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md).
    
  
##  <a name="planned-relocation-and-scheduled-disk-maintenance-procedure"></a><a name="Planned"></a> Procedimiento de reubicación planeada y mantenimiento de disco programado  
 Para mover un archivo de registro o datos de bases de datos del sistema como parte de una operación de reubicación planeada o de mantenimiento programado, siga estos pasos. Este procedimiento se aplica a todas las bases de datos del sistema, excepto las bases de datos maestras y Resource.  
  
1.  Para cada archivo que se va a mover, ejecute la siguiente instrucción.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE ( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
2.  Detenga la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o cierre el sistema para realizar el mantenimiento. Para más información, consulte [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  Mueva los archivos a la nueva ubicación y compruebe que la cuenta del servicio [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] siga teniendo permisos de acceso.

4.  Reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o el servidor. Para más información, consulte [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
5.  Compruebe el cambio de los archivos ejecutando la consulta siguiente.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
 Si se mueve la base de datos msdb y se configura la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para [Correo electrónico de base de datos](../../relational-databases/database-mail/database-mail.md), complete estos pasos adicionales.  
  
1.  Compruebe que [!INCLUDE[ssSB](../../includes/sssb-md.md)] se haya habilitado para la base de datos msdb; para ello, ejecute la siguiente consulta.  
  
    ```  
    SELECT is_broker_enabled   
    FROM sys.databases  
    WHERE name = N'msdb';  
    ```  
  
     Para obtener más información sobre cómo habilitar [!INCLUDE[ssSB](../../includes/sssb-md.md)], vea [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
2.  Envíe un mensaje de correo electrónico para comprobar que el Correo electrónico de base de datos funciona.  
  
##  <a name="failure-recovery-procedure"></a><a name="Failure"></a> Procedimiento de recuperación de errores  
 Si se debe mover un archivo a causa de un error de hardware, siga los pasos que se indican a continuación para colocar el archivo en otra ubicación. Este procedimiento se aplica a todas las bases de datos del sistema, excepto las bases de datos maestras y Resource.  
  
> [!IMPORTANT]  
>  Si no se puede iniciar la base de datos, es decir, si se encuentra en modo sospechoso o en un estado no recuperado, solo los miembros del rol fijo sysadmin podrán mover el archivo.  
  
1.  Detenga la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si se inició.  
  
2.  Inicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de recuperación solo de master especificando uno de los siguientes comandos en el símbolo del sistema. Los parámetros especificados en estos comandos distinguen entre mayúsculas y minúsculas. Los comandos generan un error cuando los parámetros no se especifican como se indica.  
  
    -   Para la instancia predeterminada (MSSQLSERVER), ejecute el siguiente comando:  
  
        ```  
        NET START MSSQLSERVER /f /T3608
        ```  
  
    -   Para una instancia con nombre, ejecute el siguiente comando:  
  
        ```  
        NET START MSSQL$instancename /f /T3608
        ```  
  
     Para más información, consulte [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
3.  En cada uno de los archivos que se van a mover, use los comandos **sqlcmd** o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para ejecutar la siguiente instrucción.  
  
    ```  
    ALTER DATABASE database_name MODIFY FILE( NAME = logical_name , FILENAME = 'new_path\os_file_name' )  
    ```  
  
     Para obtener más información sobre cómo usar la utilidad **sqlcmd** , vea [Usar la utilidad sqlcmd](../../ssms/scripting/sqlcmd-use-the-utility.md).  
  
4.  Salga de la utilidad **sqlcmd** o [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
5.  Detenga la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por ejemplo, ejecute `NET STOP MSSQLSERVER`.  
  
6.  Mueva el archivo o los archivos a la nueva ubicación.  
  
7.  Reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Por ejemplo, ejecute `NET START MSSQLSERVER`.  
  
8.  Compruebe el cambio de los archivos ejecutando la consulta siguiente.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'<database_name>');  
    ```  
  
##  <a name="moving-the-master-database"></a><a name="master"></a> Mover la base de datos maestra  
 Para mover la base de datos maestra, siga estos pasos.  
  
1.  Desde el menú **Inicio** , seleccione **Todos los programas**, **Microsoft SQL Server 2005**, **Herramientas de configuración** y, finalmente, haga clic en **Administrador de configuración de SQL Server**.  
  
2.  En el nodo **Servicios de SQL Server** , haga clic con el botón derecho en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (por ejemplo, **SQL Server (MSSQLSERVER)** ) y elija **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades de (** _nombre_de_instancia_ **) de SQL Server** , haga clic en la pestaña **Parámetros de inicio** .  
  
4.  En el cuadro **Parámetros existentes**, seleccione el parámetro -d. En el cuadro **Especifique un parámetro de inicio**, cambie el parámetro a la nueva ruta del archivo de *datos* maestros. Haga clic en **Actualizar** para guardar el cambio.
  
5.  En el cuadro **Parámetros existentes**, seleccione el parámetro -l. En el cuadro **Especifique un parámetro de inicio**, cambie el parámetro a la nueva ruta del archivo de *registro* maestro. Haga clic en **Actualizar** para guardar el cambio.

     El valor de parámetro del archivo de datos debe ir a continuación del parámetro `-d` y el valor del archivo de registro debe ir a continuación del parámetro `-l` . En el siguiente ejemplo se muestran los valores de los parámetros para la ubicación predeterminada del archivo de datos maestros.  
  
     `-dC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\master.mdf`  
  
     `-lC:\Program Files\Microsoft SQL Server\MSSQL<version>.MSSQLSERVER\MSSQL\DATA\mastlog.ldf`  
  
     Si la reubicación planeada para el archivo de datos maestros es `E:\SQLData`, los valores de parámetros se cambiarán de la siguiente manera:  
  
     `-dE:\SQLData\master.mdf`  
  
     `-lE:\SQLData\mastlog.ldf`  

6.  Haga clic en **Aceptar** para guardarlos cambios de forma permanente y cierre el cuadro de diálogo **Propiedades de (** _nombre_instancia_ **) de SQL Server**.

7.  Para detener la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic con el botón derecho en el nombre de la instancia y elija **Detener**.  
  
8.  Mueva los archivos master.mdf y mastlog.ldf a la nueva ubicación.  
  
9.  Reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
10. Compruebe el cambio de archivo de la base de datos maestra ejecutando la siguiente consulta.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID('master');  
    GO  
    ```  

11. En este punto, SQL Server se debería ejecutar con normalidad. Sin embargo, Microsoft también recomienda ajustar la entrada del Registro en `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\instance_ID\Setup`, donde *instance_ID* es similar a `MSSQL13.MSSQLSERVER`. En ese subárbol, cambie el valor de `SQLDataRoot` a la ruta de acceso nuevo. Si no actualiza el Registro, puede que la aplicación de revisiones y las actualizaciones presenten errores.

  
##  <a name="moving-the-resource-database"></a><a name="Resource"></a> Mover la base de datos Resource  
 La ubicación de la base de datos Resource es \<*drive*>:\Archivos de programa\Microsoft SQL Server\MSSQL\<version>.\<*instance_name*>\MSSQL\Binn\\. No se puede mover la base de datos.  
  
##  <a name="follow-up-after-moving-all-system-databases"></a><a name="Follow"></a> Seguimiento: después de mover todas las bases de datos del sistema  
 Si ha movido todas las bases de datos del sistema a una nueva unidad o volumen o a otro servidor con una letra de unidad diferente, realice las actualizaciones siguientes.  
  
-   Cambie la ruta de acceso del registro del Agente SQL Server. Si no actualiza esta ruta de acceso, el Agente SQL Server no se podrá iniciar.  
  
-   Cambie la ubicación predeterminada de la base de datos. Si la letra de unidad y la ruta de acceso especificada como ubicación predeterminada no existen, es posible que no se pueda crear una nueva base de datos.  
  
#### <a name="change-the-sql-server-agent-log-path"></a>Cambiar la ruta de acceso del registro del Agente SQL Server  
  
1.  En SQL Server Management Studio, en el Explorador de objetos, expanda **Agente SQL Server**.  
  
2.  Haga clic con el botón derecho en **Registros de errores** y haga clic en **Configurar**.  
  
3.  En el cuadro de diálogo **Configurar registros de errores del Agente SQL Server** , especifique la nueva ubicación del archivo SQLAGENT.OUT. La ubicación predeterminada es C:\Archivos de programa\Microsoft SQL Server\MSSQL\<version>.<nombre_instancia>\MSSQL\Log\\.  
  
#### <a name="change-the-database-default-location"></a>Cambiar la ubicación predeterminada de la base de datos  
  
1.  En SQL Server Management Studio, en el Explorador de objetos, haga clic con el botón derecho en el servidor de SQL Server y, después, haga clic en **Propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades del servidor** , seleccione **Configuración de base de datos**.  
  
3.  En **Ubicaciones predeterminadas de la base de datos**, busque la nueva ubicación de los archivos de registro y datos.  
  
4.  Detenga e inicie el servicio SQL Server para completar el cambio.  
  
##  <a name="examples"></a><a name="Examples"></a> Ejemplos  
  
### <a name="a-moving-the-tempdb-database"></a>A. Mover la base de datos tempdb  
 En el ejemplo siguiente se mueven los archivos de datos y registro de `tempdb` a una nueva ubicación como parte de una reubicación planeada.  
  
> [!NOTE]  
>  Puesto que tempdb se vuelve a crear cada vez que se inicia la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , no hay que mover físicamente los archivos de datos y registro. Los archivos se crean en la ubicación nueva cuando se reinicia el servicio en el paso 3. Hasta que se reinicie el servicio, tempdb continúa utilizando los archivos de datos y de registro de la ubicación existente.  
  
1.  Determine los nombres de los archivos lógicos de la base de datos `tempdb` y su ubicación actual en el disco.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    GO  
    ```  
  
2.  Cambie la ubicación de cada archivo con `ALTER DATABASE`.  
  
    ```  
    USE master;  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = tempdev, FILENAME = 'E:\SQLData\tempdb.mdf');  
    GO  
    ALTER DATABASE tempdb   
    MODIFY FILE (NAME = templog, FILENAME = 'F:\SQLLog\templog.ldf');  
    GO  
    ```  
  
3.  Detenga y reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
4.  Compruebe el cambio de los archivos.  
  
    ```  
    SELECT name, physical_name AS CurrentLocation, state_desc  
    FROM sys.master_files  
    WHERE database_id = DB_ID(N'tempdb');  
    ```  
  
5.  Elimine los archivos `tempdb.mdf` y `templog.ldf` de la ubicación original.  
  
## <a name="see-also"></a>Consulte también  
 [Base de datos Resource](../../relational-databases/databases/resource-database.md)   
 [Base de datos tempdb](../../relational-databases/databases/tempdb-database.md)   
 [Base de datos maestra](../../relational-databases/databases/master-database.md)   
 [Base de datos msdb](../../relational-databases/databases/msdb-database.md)   
 [Base de datos model](../../relational-databases/databases/model-database.md)   
 [Mover bases de datos de usuario](../../relational-databases/databases/move-user-databases.md)   
 [Mover archivos de base de datos](../../relational-databases/databases/move-database-files.md)   
 [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Volver a generar bases de datos del sistema](../../relational-databases/databases/rebuild-system-databases.md)  
  
