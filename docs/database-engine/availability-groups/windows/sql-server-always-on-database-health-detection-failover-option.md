---
title: Detección del estado del nivel de la base de datos
description: Obtenga información sobre la característica de detección de estado de nivel de base de datos disponible para grupos de disponibilidad AlwaysOn de SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/19/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- AlwaysOn
- DB_FAILOVER
- Always On
- High Availability
- SQL Server
ms.assetid: d74afd28-25c3-48a1-bc3f-e353bee615c2
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1d0abb2a078c9dd2e7f540ee05b70b60dd756a34
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639200"
---
# <a name="availability-group-database-level-health-detection-failover-option"></a>Opción de conmutación por error de detección del estado del nivel de la base de datos de un grupo de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Desde SQL Server 2016, hay una opción disponible de detección del estado del nivel de la base de datos (DB_FAILOVER) al configurar un grupo de disponibilidad AlwaysOn. La detección del estado del nivel de la base de datos advierte cuando una base de datos deja de estar en estado en línea, cuando algo va mal, y activará la conmutación automática por error del grupo de disponibilidad.

La detección del estado de nivel de la base de datos está habilitada en el grupo de disponibilidad como un todo, de ahí que supervise todas y cada una de las bases de datos del grupo de disponibilidad. Esta opción no se puede habilitar de manera selectiva para bases de datos específicas del grupo de disponibilidad.

## <a name="benefits-of-database-level-health-detection-option"></a>Ventajas de la opción de detección del estado del nivel de la base de datos
---
La detección del estado del nivel de la base de datos de un grupo de disponibilidad es tremendamente recomendable como opción que contribuye a garantizar una elevada disponibilidad de las bases de datos. Sopese pues la posibilidad de activarla en todos los grupos de disponibilidad. Si su aplicación depende de que exista una alta disponibilidad de diversas bases de datos, reúnalas en un grupo de disponibilidad con la opción de estado de la base de datos activada.

Por ejemplo, si la opción de detección del estado del nivel de la base de datos está activada y se da el caso de que SQL Server no puede escribir en el archivo de registro de transacciones de una de las bases de datos, el estado de esa base de datos cambiaría para indicar el error, el grupo de disponibilidad conmutaría por error enseguida y la aplicación podría volver a conectarse y a seguir trabajando con una interrupción mínima cuando las bases de datos vuelvan a estar en línea.

### <a name="enabling-database-level-health-detection"></a>Habilitar la detección del estado del nivel de la base de datos

Aunque es recomendable en general, la opción de estado de la base de datos está **desactivada de forma predeterminada**, a fin de mantener la compatibilidad con la configuración predeterminada en las versiones anteriores.

Existen varias formas muy sencillas de habilitar la opción de detección del estado del nivel de la base de datos:

1. En SQL Server Management Studio, conéctese a su Motor de base de datos de SQL Server. En la ventana del Explorador de objetos, haga clic con el botón derecho en el nodo Alta disponibilidad AlwaysOn y ejecute el **Asistente para nuevo grupo de disponibilidad**. Active la casilla **Detección del estado del nivel de la base de datos** en la página Especificar nombre. Luego, complete el resto de páginas del asistente.

   ![Casilla Detección del estado del nivel de la base de datos habilitada](../../../database-engine/availability-groups/windows/media/always-on-enable-database-health-checkbox.png)

2. Vea las **Propiedades** de un grupo de disponibilidad existente en SQL Server Management Studio. Conéctese a su servidor SQL Server. En la ventana del Explorador de objetos, expanda el nodo Alta disponibilidad AlwaysOn. Expanda Grupos de disponibilidad. Haga clic con el botón derecho en el grupo de disponibilidad y elija Propiedades. Active la opción **Detección del estado del nivel de la base de datos** y haga clic en Aceptar o genere un script con el cambio.

   ![Propiedades de la detección del estado del nivel de la base de datos de un grupo de disponibilidad AlwaysOn](../../../database-engine/availability-groups/windows/media/always-on-ag-properties-database-level-health-detection.png)


3. Use la sintaxis de Transact-SQL **CREATE AVAILABILITY GROUP** para crear un grupo de disponibilidad. El parámetro DB_FAILOVER acepta valores ON u OFF.

   ```sql
   CREATE AVAILABILITY GROUP [Contoso-ag]
   WITH (DB_FAILOVER=ON)
   FOR DATABASE [AutoHa-Sample]
   REPLICA ON
       N'SQLSERVER-0' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-0.DOMAIN.COM:5022',
         FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
       N'SQLSERVER-1' WITH (ENDPOINT_URL = N'TCP://SQLSERVER-1.DOMAIN.COM:5022',
        FAILOVER_MODE = AUTOMATIC, AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
    ```

4. Use la sintaxis de Transact-SQL **ALTER AVAILABILITY GROUP** para modificar un grupo de disponibilidad. El parámetro DB_FAILOVER acepta valores ON u OFF.

   ```sql
   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = ON);

   ALTER AVAILABILITY GROUP [Contoso-ag] SET (DB_FAILOVER = OFF);
   ```

### <a name="caveats"></a>Advertencias

Es importante tener en cuenta que, actualmente, la opción de detección del estado del nivel de la base de datos no hace que SQL Server supervise el tiempo de actividad del disco y SQL Server tampoco supervisa directamente la disponibilidad del archivo de base de datos. En caso de que se produzca un error en una unidad de disco o esta deje de estar disponible, esto en sí no provocaría que el grupo de disponibilidad conmutara automáticamente por error.

Por ejemplo, cuando una base de datos está inactiva, sin ninguna transacción ni escrituras físicas, si algunos de los archivos de la base de datos dejan de estar accesibles, SQL Server no realizará ningún trabajo de E/S de lectura o escritura en los archivos ni cambiará el estado de esa base de datos inmediatamente, de modo que no se desencadenará ninguna conmutación por error. Más adelante, si tiene lugar un punto de control de la base de datos o una lectura o escritura física para realizar una consulta, SQL Server se dará cuenta del problema con los archivos y reaccionará cambiando el estado de la base de datos, lo que, en última instancia, hará que el grupo de disponibilidad que tiene activada la detección del estado del nivel de la base de datos conmute por error debido al cambio de estado de la base de datos.

Por poner otro ejemplo, cuando el Motor de base de datos de SQL Server necesita leer una página de datos para realizar una consulta, si la página de datos está almacenada en caché en la memoria del grupo de búferes, no será necesaria ninguna lectura de disco con acceso físico para cursar dicha solicitud de consulta. Por lo tanto, el hecho de que falte un archivo de datos o este no esté disponible no desencadenará inmediatamente una conmutación automática por error, incluso si está habilitada la opción de estado de la base de datos, ya que el estado de la base de datos no cambia de inmediato.


## <a name="database-failover-is-separate-from-flexible-failover-policy"></a>La conmutación por error de base de datos es independiente de la directiva de conmutación por error flexible
Con la detección del estado del nivel de la base de datos se implementa una directiva de conmutación por error flexible que configura los umbrales del estado de los procesos de SQL Server. La detección del estado del nivel de la base de datos se configura con el parámetro DB_FAILOVER, mientras que la opción de grupo de disponibilidad FAILURE_CONDITION_LEVEL es independiente y sirve para configurar la detección del estado de los procesos de SQL Server. Ambas opciones son independientes.

## <a name="managing-and-monitoring-database-level-health-detection"></a>Administración y supervisión de la detección del estado del nivel de la base de datos

### <a name="dynamic-management-views"></a>Vistas de administración dinámica

La DMV del sistema sys.availability_groups muestra una columna db_failover que indica si la opción de detección del estado del nivel de la base de datos está desactivada (0) o activada (1).

```sql
select name, db_failover from sys.availability_groups
```


Ejemplo de resultado de esta DMV:

|name  |  db_failover|
|---------|---------|
| Contoso-ag | 1  |

### <a name="errorlog"></a>ErrorLog
El registro de errores de SQL Server (o el texto recogido en sp_readerrorlog) mostrará el mensaje de error 41653 si un grupo de disponibilidad ha conmutado por error a causa de las comprobaciones de detección del estado del nivel de la base de datos.

Por ejemplo, este extracto del registro de errores pone de manifiesto que una escritura del registro de transacciones ha acabado en error debido a un problema de disco, lo que llevó a cerrar la base de datos denominada AutoHa-Sample y ello hizo que la detección del estado del nivel de la base de datos conmutara por error el grupo de disponibilidad.

>2016-04-25 12:20:21.08 spid1s      Error: 17053, Severity: 16, estado: 1.
>
>2016-04-25 12:20:21.08 spid1s      SQLServerLogMgr::LogWriter: Operating system error 21(The device is not ready.) encountered.
>2016-04-25 12:20:21.08 spid1s      Write error during log flush.
>
>2016-04-25 12:20:21.08 spid79      Error: 9001, Severity: 21, State: 4.
>
>2016-04-25 12:20:21.08 spid79      The log for database 'AutoHa-Sample' is not available. Vea los mensajes de error relacionados en el registro de eventos. Corrija los errores y reinicie la base de datos.
>
>**2016-04-25 12:20:21.15 spid79      Error: 41653, Severity: 21, State: 1.**
>
>**2016-04-25 12:20:21.15 spid79      Database 'AutoHa-Sample' encountered an error (error type: 2 'DB_SHUTDOWN') causing failure of the availability group 'Contoso-ag'.  Vea el registro de errores de SQL Server para obtener información acerca de los errores encontrados.  If this condition persists, contact the system administrator.**
>
>2016-04-25 12:20:21.17 spid79      State information for database 'AutoHa-Sample' - Hardened Lsn: '(34:664:1)'    Commit LSN: '(34:656:1)'    Commit Time: 'Apr 25 2016 12:19PM'
>
>2016-04-25 12:20:21.19 spid15s     Always On Availability Groups connection with secondary database terminated for primary database 'AutoHa-Sample' on the availability replica 'SQLServer-0' with Replica ID: {c4ad5ea4-8a99-41fa-893e-189154c24b49}. Esto es solo un mensaje informativo. No se requiere ninguna acción del usuario.
>
>2016-04-25 12:20:21.21 spid75      Always On: The local replica of availability group 'Contoso-ag' is preparing to transition to the resolving role in response to a request from the Windows Server Failover Clustering (WSFC) cluster. Esto es solo un mensaje informativo. No se requiere ninguna acción del usuario.
>
>2016-04-25 12:20:21.21 spid75      The state of the local availability replica in availability group 'ag' has changed from 'PRIMARY_NORMAL' to 'RESOLVING_NORMAL'.  The state changed because the availability group is going offline.  The replica is going offline because the associated availability group has been deleted, or the user has taken the associated availability group offline in Windows Server Failover Clustering (WSFC) management console, or the availability group is failing over to another SQL Server instance.  For more information, see the SQL Server error log, Windows Server Failover Clustering (WSFC) management console, or WSFC log.

### <a name="extended-event-sqlserveravailability_replica_database_fault_reporting"></a>Evento extendido sqlserver.availability_replica_database_fault_reporting

Desde SQL Server 2016, existe un nuevo evento extendido definido que se desencadena a través de la detección del estado del nivel de la base de datos.  El nombre del evento es **sqlserver.availability_replica_database_fault_reporting**.

Este evento extendido se desencadena únicamente en la réplica principal, y lo hace cuando se detecta un problema de estado del nivel de la base de datos en una base de datos hospedada en un grupo de disponibilidad.

Este es un ejemplo sobre cómo crear una sesión de XEvent que captura este evento. Como no se especifica ninguna ruta de acceso, el archivo de salida de XEvent se pondrá de forma predeterminada en la ruta del registro de errores de SQL Server. Ejecute lo siguiente en la réplica principal del grupo de disponibilidad:

Ejemplo de script de sesión de evento extendido

```sql
CREATE EVENT SESSION [AlwaysOn_dbfault] ON SERVER
ADD EVENT sqlserver.availability_replica_database_fault_reporting
ADD TARGET package0.event_file(SET filename=N'dbfault.xel',max_file_size=(5),max_rollover_files=(4))
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=ON)
GO
ALTER EVENT SESSION AlwaysOn_dbfault ON SERVER STATE=START
GO
```

#### <a name="extended-event-output"></a>Resultado del evento extendido
En SQL Server Management Studio, conéctese al servidor SQL Server principal, expanda el nodo Administración y, luego, Eventos extendidos. Busque la sesión (AlwaysOn_dbfault era el nombre en el ejemplo anterior) y expándala para ver los archivos de salida. Haga clic en el archivo de salida y el archivo de evento se abrirá en una pestaña nueva.

Explicación de los campos:

|Datos de columna | Descripción|
|---------|---------|
|availability_group_id |Identificador del grupo de disponibilidad.|
|availability_group_name |El nombre del grupo de disponibilidad.|
|availability_replica_id |Identificador de la réplica de disponibilidad.|
|availability_replica_name |Nombre de la réplica de disponibilidad.|
|database_name |Nombre de la base de datos que informa del error.|
|database_replica_id |Identificador de la base de datos de la réplica de disponibilidad.|
|failover_ready_replicas |Número de réplicas secundarias de conmutación automática por error que están sincronizadas.|
|fault_type  | Identificador de error notificado. Valores posibles:  <br/> 0: ninguno <br/>1: desconocido<br/>2: apagado|
|is_critical | Desde SQL Server 2016, este valor siempre debe devolver true para el XEvent.|


En esta salida de ejemplo, fault_type (tipo de error 2: apagado) refleja que se ha producido un evento crítico en el grupo de disponibilidad Contoso-ag, en la réplica llamada SQLSERVER-1, a causa del nombre de base de datos AutoHa-Sample2.

|Campo  | Value|
|---------|---------|
|availability_group_id | 24E6FE58-5EE8-4C4E-9746-491CFBB208C1|
|availability_group_name | Contoso-ag|
|availability_replica_id | 3EAE74D1-A22F-4D9F-8E9A-DEFF99B1F4D1|
|availability_replica_name | SQLSERVER-1|
|database_name | AutoHa-Sample2|
|database_replica_id | 39971379-8161-4607-82E7-098590E5AE00|
|failover_ready_replicas | 1|
|fault_type | 2|
|is_critical | True|


### <a name="related-references"></a>Referencias relacionadas

* [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)

* [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)

* [Directiva de conmutación por error flexible para conmutación automática por error de un grupo de disponibilidad (SQL Server)](./configure-flexible-automatic-failover-policy.md)

* [Enhance AlwaysOn Failover Policy to Test SQL Server Database Data and Log Drives](/archive/blogs/alwaysonpro/enhance-alwayson-failover-policy-to-test-sql-server-database-data-and-log-drives) (Mejorar la directiva de conmutación por error de AlwaysOn para probar las unidades de registro y de datos de la base de datos de SQL Server)

* [Eventos extendidos](../../../relational-databases/extended-events/extended-events.md)