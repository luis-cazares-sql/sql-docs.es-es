---
title: Adición de una base de datos a un grupo de disponibilidad
description: 'Agregue una base de datos a un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio. '
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- primary databases [SQL Server], in availability group
- Availability Groups [SQL Server], configuring
- Availability Groups [SQL Server], databases
ms.assetid: 2a54eef8-9e8e-4e04-909c-6970112d55cc
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2f40156f8a7677de1c80fcc5094ee42b5527682
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766054"
---
# <a name="add-a-database-to-an-always-on-availability-group"></a>Adición de una base de datos a un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se explica cómo agregar una base de datos a un grupo de disponibilidad AlwaysOn con [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  

  
## <a name="prerequisites-and-restrictions"></a>Requisitos previos y restricciones  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
  
-   La base de datos debe residir en la instancia del servidor que hospeda la réplica principal y cumple los requisitos previos y las restricciones de las bases de datos de disponibilidad. Para obtener más información, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="use-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Usar SQL Server Management Studio  

  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic con el botón secundario en el grupo de disponibilidad y seleccione uno de los siguientes comandos:  
  
    -   Para iniciar la función Agregar base de datos al Asistente para grupo de disponibilidad, seleccione el comando **Agregar base de datos** . Para obtener más información, vea [Usar el Asistente para agregar una base de datos al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/availability-group-add-database-to-group-wizard.md).  
  
    -   Para agregar una o varias bases de datos especificándolas en el cuadro de diálogo **Propiedades de grupo de disponibilidad** , seleccione el comando **Propiedades** . Los pasos para agregar una base de datos son los siguientes:  
  
        1.  En el panel **Bases de datos de disponibilidad** , haga clic en el botón **Agregar** . Esto crea y selecciona un campo de la base de datos en blanco.  
  
        2.  Escriba el nombre de una base de datos que cumpla los requisitos previos de las bases de datos de disponibilidad.  
  
         Para agregar otra base de datos, repita los pasos anteriores. Cuando haya terminado de especificar las bases de datos, haga clic en **Aceptar** para completar la operación.  
  
         Después de utilizar el cuadro de diálogo **Propiedades de grupo de disponibilidad** para agregar una base de datos a un grupo de disponibilidad, debe configurar la base de datos secundaria correspondiente en cada instancia de servidor que hospeda una réplica secundaria. Para obtener más información, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  

  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.    
2.  Use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) del siguiente modo:  
  
     ALTER AVAILABILITY GROUP *group_name* ADD DATABASE *database_name* [,...*n*]  
  
     donde *group_name* es el nombre del grupo de disponibilidad y *database_name* es el nombre de una base de datos que se va a agregar al grupo.  
  
     En el ejemplo siguiente se agrega la base de datos *MyDb3* al grupo de disponibilidad *MyAG* .  
  
    ```  
    -- Connect to the server instance that hosts the primary replica.  
    -- Add an existing database to the availability group.  
    ALTER AVAILABILITY GROUP MyAG ADD DATABASE MyDb3;  
    GO  
    ```  
  
3.  Después de agregar una base de datos a un grupo de disponibilidad, debe configurar la base de datos secundaria correspondiente en cada instancia de servidor que hospeda una réplica secundaria. Para obtener más información, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="use-powershell"></a><a name="PowerShellProcedure"></a> Usar de PowerShell  

  
1.  Cambie el directorio (**cd**) a la instancia del servidor que hospeda la réplica principal.  
  
2.  Use el cmdlet **Add-SqlAvailabilityDatabase** .  
  
     Por ejemplo, en el comando siguiente se agrega la base de datos secundaria `MyDd` al grupo de disponibilidad `MyAG` , cuya réplica principal está hospedada en `PrimaryServer\InstanceName`.  
  
    ```  
  
    Add-SqlAvailabilityDatabase `   
    -Path SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
    -Database "MyDb"  
    ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
3.  Después de agregar una base de datos a un grupo de disponibilidad, debe configurar la base de datos secundaria correspondiente en cada instancia de servidor que hospeda una réplica secundaria. Para obtener más información, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
 Para ver un ejemplo completo, vea [Ejemplo (PowerShell)](#PSExample)a continuación.  
  
###  <a name="example-powershell"></a><a name="PSExample"></a> Ejemplo (PowerShell)  
 En el ejemplo siguiente se muestra el proceso completo para preparar una base de datos secundaria de una base de datos en la instancia del servidor que hospeda la réplica principal de un grupo de disponibilidad, agregando la base de datos a un grupo de disponibilidad (como una base de datos principal) y uniendo después la base de datos secundaria al grupo de disponibilidad. Primero, en el ejemplo se realiza una copia de seguridad de la base de datos y del registro de transacciones. En el ejemplo se restauran las copias de seguridad de la base de datos y de registros a las instancias de servidor que hospeda una réplica secundaria.  
  
 En el ejemplo se llama dos veces a **Add-SqlAvailabilityDatabase** : la primera, en la réplica principal para agregar la base de datos al grupo de disponibilidad y, después, en la réplica secundaria para unir la base de datos secundaria de esa réplica al grupo de disponibilidad. Si tiene más de una réplica secundaria, restaure y una la base de datos secundaria en cada una de ellas.  
  
```  
$DatabaseBackupFile = "\\share\backups\MyDatabase.bak"  
$LogBackupFile = "\\share\backups\MyDatabase.trn"  
$MyAgPrimaryPath = "SQLSERVER:\SQL\PrimaryServer\InstanceName\AvailabilityGroups\MyAg"  
$MyAgSecondaryPath = "SQLSERVER:\SQL\SecondaryServer\InstanceName\AvailabilityGroups\MyAg"  
  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "PrimaryServer\InstanceName"  
Backup-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "PrimaryServer\InstanceName" -BackupAction 'Log'  
  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $DatabaseBackupFile -ServerInstance "SecondaryServer\InstanceName" -NoRecovery  
Restore-SqlDatabase -Database "MyDatabase" -BackupFile $LogBackupFile -ServerInstance "SecondaryServer\InstanceName" -RestoreAction 'Log' -NoRecovery  
  
Add-SqlAvailabilityDatabase -Path $MyAgPrimaryPath -Database "MyDatabase"  
Add-SqlAvailabilityDatabase -Path $MyAgSecondaryPath -Database "MyDatabase"  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Creación y configuración de grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/creation-and-configuration-of-availability-groups-sql-server.md)   
 [Usar el Panel de AlwaysOn &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)