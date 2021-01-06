---
title: Configuración de eventos extendidos para grupos de disponibilidad
description: SQL Server define eventos extendidos que son específicos de los grupos de disponibilidad Always On. Puede supervisar estos eventos extendidos en una sesión para ayudar con el diagnóstico de causas raíz cuando solucione problemas de un grupo de disponibilidad.
ms.custom: ag-guide, seodec18
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 5950f98a-3950-473d-95fd-cde3557b8fc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 28eab9dae04e0268c6dd7827aab8be71220e4218
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643752"
---
# <a name="configure-extended-events-for-always-on-availability-groups"></a>Configuración de eventos extendidos para grupos de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  SQL Server define eventos extendidos que son específicos de los grupos de disponibilidad Always On. Puede supervisar estos eventos extendidos en una sesión para ayudar con el diagnóstico de causas raíz cuando solucione problemas de un grupo de disponibilidad. Puede ver los eventos extendidos de un grupo de disponibilidad con la siguiente consulta:  
  
```sql  
SELECT * FROM sys.dm_xe_objects WHERE name LIKE '%hadr%'  
```  
   
##  <a name="alwayson_health-session"></a><a name="BKMK_alwayson_health"></a> Sesión Alwayson_health  
 La sesión de eventos extendidos alwayson_health se crea automáticamente al crear el grupo de disponibilidad y captura un subconjunto de los eventos relacionados del grupo de disponibilidad. Esta sesión está preconfigurada como una útil y cómoda herramienta que ayuda a empezar a trabajar rápidamente a la vez que soluciona los problemas de un grupo de disponibilidad. El Asistente para crear un grupo de disponibilidad inicia automáticamente la sesión en cada réplica de disponibilidad participante configurada en el asistente.  
  
> [!IMPORTANT]  
>  Si no ha creado el grupo de disponibilidad mediante el **Asistente para nuevo grupo de disponibilidad**, puede que la sesión alwayson_health no se inicie automáticamente. Si la sesión no se ha iniciado, no puede capturar datos de eventos cuando se produce un problema inesperado. Debe iniciar la sesión de forma manual y configurarla para que se inicie automáticamente en las propiedades de la sesión.  
  
 Para ver la definición de la sesión alwayson_health:  
  
1.  En el **Explorador de objetos**, expanda **Administración**, **Eventos extendidos** y **Sesiones**.  
  
2.  Haga clic con el botón derecho en **Alwayson_health**, luego elija **Incluir sesión como**, **CREATE To** y, por último, haga clic en **Nueva ventana del Editor de consultas**.  

Para obtener información sobre algunos de los eventos cubiertos por alwayson_health, vea la [referencia sobre eventos extendidos](always-on-extended-events.md#BKMK_Reference).  


##  <a name="extended-events-for-debugging"></a><a name="BKMK_Debugging"></a> Eventos extendidos para la depuración  
 Además de los eventos extendidos cubiertos por la sesión Alwayson_health, SQL Server define un amplio conjunto de eventos de depuración para los grupos de disponibilidad. Para aprovechar estos otros eventos extendidos en una sesión, siga los procedimientos siguientes:  
  
1.  En el **Explorador de objetos**, expanda **Administración**, **Eventos extendidos** y **Sesiones**.  
  
2.  Haga clic con el botón derecho en **Sesiones** y seleccione **Nueva sesión**. O bien, haga clic con el botón derecho en **Alwayson_health** y seleccione **Propiedades**.  
  
3.  En el panel **Seleccionar una página**, haga clic en **Eventos**.  
  
4.  En la biblioteca de eventos, en la columna **Categoría**, seleccione **alwayson** y desactive las demás categorías.  
  
5.  En la columna **Canal**, seleccione **Depurar**. Todos los eventos relacionados del grupo de disponibilidad que aún no estén seleccionados se muestran ahora en la biblioteca de eventos.  
  
6.  Resalte un evento de la biblioteca de eventos y luego haga clic en el botón **>** para seleccionarlo para la sesión.  
  
7.  Cuando termine la sesión, haga clic en **Aceptar** para cerrarla. Asegúrese de que la sesión se haya iniciado para que capture los eventos seleccionados.  
  
##  <a name="always-on-availability-groups-extended-events-reference"></a><a name="BKMK_Reference"></a> Referencia de eventos extendidos de grupos de disponibilidad Always On  
 En esta sección se describen algunos de los eventos extendidos que se usan para supervisar los grupos de disponibilidad.  
  
 [availability_replica_state_change](#BKMK_availability_replica_state_change)  
  
 [availability_group_lease_expired](#BKMK_availability_group_lease_expired)  
  
 [availability_replica_automatic_failover_validation](#BKMK_availability_replica_automatic_failover_validation)  
  
 [error_reported (varios números de error): para problemas de conexión o de transporte](#BKMK_error_reported)  
  
 [data_movement_suspend_resume](#BKMK_data_movement_suspend_resume)  
  
 [alwayson_ddl_executed](#BKMK_alwayson_ddl_executed)  
  
 [availability_replica_manager_state](#BKMK_availability_replica_manager_state)  
  
 [error_reported (1480): rol de réplica de base de datos cambiado](#BKMK_error_reported_1480)  
  
###  <a name="availability_replica_state_change"></a><a name="BKMK_availability_replica_state_change"></a> availability_replica_state_change  
 Se produce cuando cambia el estado de una réplica de disponibilidad. La creación de un grupo de disponibilidad o la combinación de una réplica de disponibilidad pueden desencadenar este evento. Es útil para el diagnóstico de errores en conmutaciones automáticas por error. También se puede usar para el seguimiento de los pasos de conmutación por error.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|availability_replica_state_change|  
|Category|Always On|  
|Canal|Operativos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Identificador del grupo de disponibilidad.|  
|availability_group_name|unicode_string|Nombre del grupo de disponibilidad.|  
|availability_replica_id|guid|Identificador de la réplica de disponibilidad.|  
|previous_state|availability_replica_state|Rol de la réplica antes del cambio.<br /><br /> **Los valores posibles son:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
|current_state|availability_replica_state|Rol de la réplica después del cambio.<br /><br /> **Los valores posibles son:**<br /><br /> Primary_Normal<br /><br /> Secondary_Normal<br /><br /> Resolving_Pending_Failover<br /><br /> Resolving_Normal<br /><br /> Primary_Pending<br /><br /> Not_Available|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_replica_state_change  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_group_lease_expired"></a><a name="BKMK_availability_group_lease_expired"></a> availability_group_lease_expired  
 Se produce cuando el clúster y el grupo de disponibilidad tienen un problema de conectividad y la concesión ha expirado. Este evento indica que se ha interrumpido la conectividad entre el grupo de disponibilidad y el clúster WSFC subyacente. Si el problema de conectividad se produce en la réplica principal, el evento puede producir una conmutación automática por error o dejar sin conexión al grupo de disponibilidad.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|availability_group_lease_expired|  
|Category|Always On|  
|Canal|Operativos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Identificador del grupo de disponibilidad.|  
|availability_group_name|unicode_string|Nombre del grupo de disponibilidad.|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT availability_group_lease_expired  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_automatic_failover_validation"></a><a name="BKMK_availability_replica_automatic_failover_validation"></a> availability_replica_automatic_failover_validation  
 Se produce cuando la conmutación automática por error valida la preparación de una réplica de disponibilidad como réplica principal y muestra si la réplica de disponibilidad de destino está lista para ser la nueva réplica principal. Por ejemplo, la validación de conmutación por error devuelve False si no todas las bases de datos se han sincronizado o se han combinado. Este evento se ha diseñado para proporcionar un punto de error durante las conmutaciones por error. Esta información es de interés para el administrador de la base de datos especialmente para conmutaciones automáticas por error, dado que una conmutación automática por error es una operación desatendida. El administrador de la base de datos puede revisar el evento para ver por qué se ha producido un error en la conmutación automática por error.  
  
#### <a name="event-information"></a>Información del evento  
  
|Nombre|Descripción|  
|----------|-----------------|  
|availability_replica_automatic _failover_validation||  
|Category|Always On|  
|Canal|Analíticos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|----------|----------------|-----------------|  
|availability_group_id|guid|Identificador del grupo de disponibilidad.|  
|availability_group_name|unicode_string|El nombre del grupo de disponibilidad.|  
|availability_replica_id|guid|Identificador de la réplica de disponibilidad.|  
|forced_quorum|validation_result_type|Si el valor es TRUE, la conmutación automática por error se invalida en esta réplica de disponibilidad.<br /><br /> TRUE<br /><br /> FALSE|  
|joined_and_synchronized|validation_result_type|Si el valor es FALSE, la conmutación automática por error se invalida en esta réplica de disponibilidad.<br /><br /> TRUE<br /><br /> FALSE|  
|previous_primary_or_automatic_failover_target|validation_result_type|Si el valor es FALSE, la conmutación automática por error se invalida en esta réplica de disponibilidad.<br /><br /> TRUE<br /><br /> FALSE|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_automatic_failover_validation (  
WHERE (  
[forced_quorum]=(TRUE) OR [joined_and_synchronized]=(FALSE) OR [previous_primary_or_automatic_failover_target]=(TRUE)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
  
```  
  
###  <a name="error_reported-multiple-error-numbers-for-transport-or-connection-issues"></a><a name="BKMK_error_reported"></a> error_reported (varios números de error): para problemas de conexión o de transporte  
 Cada evento filtrado indica que se ha producido un problema de conectividad en el punto de conexión de reflejo de transporte o base de datos del que depende ese grupo de disponibilidad.  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|error_reported<br /><br /> números que se van a filtrar: 35201, 35202, 35206, 35204, 35207, 9642, 9666, 9691, 9692, 9693, 28034, 28036, 28080, 28091, 33309|  
|Category|errors|  
|Canal|Administración|  
  
#### <a name="error-numbers-to-filter"></a>Números de error que se van a filtrar  
  
|Número de error|Descripción|  
|------------------|-----------------|  
|35201|Se ha producido un tiempo de espera de conexión al intentar establecer una conexión con la réplica de disponibilidad "%ls".|  
|35202|Se ha establecido correctamente una conexión del grupo de disponibilidad "%ls" de la réplica de disponibilidad "%ls" con el identificador [%ls] a "%ls" con el identificador [%ls].  Esto es solo un mensaje informativo. No se requiere ninguna acción del usuario.|  
|35206|Se ha producido un tiempo de espera de conexión en una conexión previamente establecida con la réplica de disponibilidad "%ls".|  
|35204|Se ha deshabilitado la conexión entre la instancia "%ls" y "%ls" debido al apagado del punto de conexión.|  
|Tiempo de espera + conectado|  
|35207|Error del intento de conexión en el identificador de grupo de disponibilidad "%ls" del identificador de réplica "%ls" al identificador de réplica "%ls" debido al error %d, gravedad %d, estado %d.  gravedad %d, estado %d. (Esto puede no tener un buen uso por parte del DBA. En ese caso, compruebe y quite más adelante)|  
|9642|Error en el punto de conexión de una conexión de transporte de Service Broker o creación de reflejo de la base de datos; error: %i, estado: %i. Rol de punto de conexión cercano: dirección de punto de conexión lejano de "%S_MSG: %.*hs" Error: %i, estado: %i. Rol de punto de conexión cercano: dirección de punto de conexión lejano de %S_MSG: "%.\*hs"|  
|9666|El punto de conexión %S_MSG está deshabilitado o se ha detenido.|  
|9691|El punto de conexión %S_MSG ha dejado de escuchar las conexiones.|  
|9692|El punto de conexión %S_MSG no puede escuchar en el puerto %d porque lo está usando otro proceso.|  
|9693|El punto de conexión %S_MSG no puede escuchar conexiones debido al siguiente error: "%.*ls".|  
|28034|Error del protocolo de enlace de la conexión. El inicio de sesión '%.*ls' no tiene permiso CONNECT en el extremo. Estado %d.|  
|28036|Error del protocolo de enlace de la conexión. No se encontró el certificado utilizado por este extremo: %S_MSG. Utilice DBCC CHECKDB en la base de datos maestra para comprobar la integridad de los metadatos de los extremos. Estado %d.|  
|28080|Error del protocolo de enlace de la conexión. El extremo %S_MSG no está configurado. Estado %d.|  
|28091|No se admite el inicio del punto de conexión de %S_MSG sin autenticación.|  
|33309|No se puede iniciar el punto de conexión del clúster porque la configuración del punto de conexión %S_MSG predeterminada aún no se ha cargado.|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--Connectivity Error Messages  
[error_number]=(35201)   
OR [error_number]=(35202)   
OR [error_number]=(35204)   
OR [error_number]=(35206)   
OR [error_number]=(35207)   
OR [error_number]=(9642)   
--OR [error_number]=(9666)   
OR [error_number]=(9691)   
OR [error_number]=(9692)   
OR [error_number]=(9693)   
OR [error_number]=(28034)   
OR [error_number]=(28036)   
OR [error_number]=(28080)   
OR [error_number]=(28091)   
OR [error_number]=(33309)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="data_movement_suspend_resume"></a><a name="BKMK_data_movement_suspend_resume"></a> data_movement_suspend_resume  
 Se produce cuando se suspende o reanuda el movimiento de la base de datos de una réplica de base de datos.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|data_movement_suspend_resume|  
|Category|Siempre activada|  
|Canal|Operativos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|-|-|-|  
|availability_group_id|guid|Identificador del grupo de disponibilidad.|  
|availability_group_name|unicode_string|Nombre del grupo de disponibilidad, si estuviera disponible.|  
|availability_replica_id|guid|Identificador de la réplica de disponibilidad.|  
|database_replica_id|guid|Identificador de la base de datos de disponibilidad.|  
|database_replica_name|unicode_string|El nombre de la base de datos de disponibilidad.|  
|database_id|uint32|Identificador de la base de datos de disponibilidad.|  
|suspend_status|suspend_status_type|Valores de estado de suspensión.<br /><br /> SUSPEND_NULL<br /><br /> RESUMED<br /><br /> SUSPENDED<br /><br /> SUSPENDED_INVALID|  
|suspend_source|suspend_source_type|Origen de la acción de suspensión o reanudación.|  
|suspend_reason|unicode_string|Motivo de suspensión capturado en el administrador de réplicas de base de datos.|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT data_movement_suspend_resume (  
WHERE (  
[suspend_status]=(SUSPENDED)or [suspend_status]=(SUSPENDED_INVALID) or   
[suspend_status]=(SUSPEND_NULL)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="alwayson_ddl_executed"></a><a name="BKMK_alwayson_ddl_executed"></a> alwayson_ddl_executed  
 Se produce cuando se ejecuta una instrucción DDL (lenguaje de definición de datos) del grupo de disponibilidad, incluidas CREATE, ALTER o DROP. El propósito principal del evento es indicar un problema con una acción del usuario en una réplica de disponibilidad, o indicar el punto inicial de una acción operativa, que va seguido de un problema de tiempo de ejecución como una conmutación por error manual, una conmutación por error forzada, un movimiento de datos suspendido o un movimiento de datos reanudado.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|alwayson_ddl_execution|  
|Category|Always On|  
|Canal|Analíticos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|----------|----------------|-----------------|  
|availability_group_id|Guid|Identificador del grupo de disponibilidad.|  
|availability_group_name|unicode_string|El nombre del grupo de disponibilidad.|  
|ddl_action|alwayson_ddl_action|Indica el tipo de acción de DDL: CREATE, ALTER o DROP.|  
|ddl_phase|ddl_opcode|Indica la fase de una operación de DDL: BEGIN, COMMIT o ROLLBACK.|  
|.|unicode_string|Texto de la instrucción que se ha ejecutado.|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT alwayson_ddl_executed  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="availability_replica_manager_state"></a><a name="BKMK_availability_replica_manager_state"></a> availability_replica_manager_state  
 Se produce cuando cambia el estado del administrador de réplicas de disponibilidad. Este evento indica el latido del administrador de réplicas de disponibilidad. Cuando el administrador de réplicas de disponibilidad no está en estado correcto, todas las réplicas de disponibilidad de la instancia de SQL Server están fuera de servicio.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|availability_replica_manager_state_change|  
|Category|Always On|  
|Canal|Operativos|  
  
#### <a name="event-fields"></a>Campos del evento  
  
|Nombre|Type_name|Descripción|  
|----------|----------------|-----------------|  
|current_state|manager_state|Estado actual del administrador de réplicas de disponibilidad.<br /><br /> En línea<br /><br /> Sin conexión<br /><br /> WaitingForClusterCommunication|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
  
ADD EVENT availability_replica_manager_state (  
WHERE ([current_state]=(OFFLINE))  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
###  <a name="error_reported-1480-database-replica-role-change"></a><a name="BKMK_error_reported_1480"></a> error_reported (1480): rol de réplica de base de datos cambiado  
 Este evento filtrado error_reported se produce de forma asincrónica después del cambio de un rol de réplica de disponibilidad. Indica qué base de datos de disponibilidad no cambia su rol esperado durante el proceso de conmutación por error.  
  
#### <a name="event-information"></a>Información del evento  
  
|Columna|Descripción|  
|------------|-----------------|  
|Nombre|error_reported<br /><br /> Número de error 1480: La base de datos REPLICATION_TYPE_MSG "NOMBRE_DE_BASE_DE_DATOS" cambia roles de "ROL_ANTERIOR" a "ROL_NUEVO" debido a REASON_MSG|  
|Category|errors|  
|Canal|Administración|  
  
#### <a name="alwayson_health-session-definition"></a>Definición de sesión alwayson_health  
  
```sql  
CREATE EVENT SESSION [alwayson_health] ON SERVER   
ADD EVENT sqlserver.error_reported(  
    WHERE   
(  
--database replica role change message  
OR [error_number] = (1480)  
  
--database replica runtime error messages  
OR [error_number]=(823)   
OR [error_number]=(824)   
OR [error_number]=(829)  
)  
)  
  
ADD TARGET package0.event_file(SET filename=N'alwayson_health.xel',max_file_size=(5),max_rollover_files=(4),metadatafile=N'alwayson_health.xem')  
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)  
GO  
```  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Ver datos de sesiones de eventos](https://msdn.microsoft.com/library/hh710068(v=sql.110).aspx)   
