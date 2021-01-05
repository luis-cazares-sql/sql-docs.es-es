---
title: 'Solución de problemas: el grupo de disponibilidad superó el RTO (SQL Server) | Microsoft Docs'
description: Obtenga información sobre cómo solucionar problemas de una conmutación por error en un grupo de disponibilidad Always On cuando tarda más tiempo que el objetivo de tiempo de recuperación en SQL Server.
ms.custom: ag-guide
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: e83e4ef8-92f0-406f-bd0b-dc48dc210517
author: rothja
ms.author: jroth
ms.openlocfilehash: 5847162edb0d962826931624f4d14a3d78468066
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641881"
---
# <a name="troubleshoot-availability-group-exceeded-rto"></a>Solución de problemas: El grupo de disponibilidad superó el RTO
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Después de una conmutación por error automática o manual planeada sin pérdida de datos en un grupo de disponibilidad, es posible que el tiempo de conmutación por error supere su objetivo de tiempo de recuperación (RTO). También puede ser que al estimar el tiempo de conmutación por error de una réplica secundaria de confirmación sincrónica (por ejemplo, un asociado de conmutación automática por error) usando el método que se describe en [Monitor performance for Always On Availability Groups](monitor-performance-for-always-on-availability-groups.md) (Supervisión del rendimiento de grupos de disponibilidad Always On), descubra que supera el RTO.  
  
 Si la conmutación por error automática todavía no se ha completado, consulte [Solucionar problemas de conmutación por error automática en entornos de SQL Server 2012 Always On](https://support.microsoft.com/kb/2833707).  
  
 En las siguientes secciones se describen las causas más habituales de las conmutaciones por error de tiempo que superan el RTO.  
  
1.  [La carga de trabajo de los informes impide que se ejecute el subproceso de la fase de puesta al día de ejecución](#BKMK_REDOBLOCK)  
  
2.  [El subproceso de la fase de puesta al día se retrasa debido a la contención de recursos](#BKMK_CONTENTION)  
  
##  <a name="reporting-workload-blocks-the-redo-thread-from-running"></a><a name="BKMK_REDOBLOCK"></a> La carga de trabajo de los informes impide que se ejecute el subproceso de la fase de puesta al día de ejecución  
 El subproceso de la fase de puesta al día de la réplica secundaria no puede hacer cambios en el lenguaje de definición de datos (DDL) a partir de una consulta de solo lectura de ejecución prolongada.  
  
### <a name="explanation"></a>Explicación  
 En la réplica secundaria, las consultas de solo lectura adquieren bloqueos de estabilidad del esquema (`Sch-S`). Estos bloqueos `Sch-S` pueden impedir que el subproceso de la fase de puesta al día adquiera bloqueos de modificación del esquema (`Sch-M`) para hacer cambios en el lenguaje de definición de datos. El subproceso de la fase de puesta al día no puede aplicar las entradas del registro hasta que se desbloquee. Una vez desbloqueado, puede continuar hasta llegar al final del registro y permitir que el proceso subsecuente de la fase de revisión y de la conmutación por error continúe.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Si el subproceso de la fase de puesta al día se bloquea, se genera un evento extendido denominado `sqlserver.lock_redo_blocked`. Además, puede consultar la solicitud sys.dm_exec_request de DMV en la réplica secundaria para averiguar qué sesión está bloqueando el subproceso de la fase de puesta al día para después emprender una acción correctiva. La siguiente consulta devuelve el identificador de sesión de la consulta de solo lectura que está bloqueando el subproceso de la fase de puesta al día.  
  
```sql  
select session_id, command, blocking_session_id, wait_time, wait_type, wait_resource   
from sys.dm_exec_requests where command = 'DB STARTUP'  
```  
  
 Puede esperar a que la carga de trabajo de los informes finalice, momento en que el subproceso de la fase de puesta al día se desbloqueará, o puede desbloquear inmediatamente el subproceso de la fase de puesta al día mediante la ejecución del comando [KILL &#40;Transact-SQL&#41;](~/t-sql/language-elements/kill-transact-sql.md) en el identificador de sesión de bloqueo.  
  
##  <a name="redo-thread-falls-behind-due-to-resource-contention"></a><a name="BKMK_CONTENTION"></a> El subproceso de la fase de puesta al día se retrasa debido a la contención de recursos  
 Una gran carga de trabajo de informes en la réplica secundaria ha ralentizado el rendimiento de la réplica secundaria y el subproceso de la fase de puesta al día se ha retrasado.  
  
### <a name="explanation"></a>Explicación  
 Al aplicar las entradas de registro en la réplica secundaria, el subproceso de la fase de puesta al día lee las entradas del registro desde el disco de registro y accede a las páginas de datos de cada entrada del registro para aplicar la entrada del registro. El acceso a la página puede enlazarse a E/S (accediendo al disco físico) si la página no está lista en el grupo de búferes. Si hay una carga de trabajo de informes enlazada a E/S, esta carga de trabajo compite por los recursos de E/S con el subproceso de la fase de puesta al día y puede ralentizar dicho subproceso.  
  
### <a name="diagnosis-and-resolution"></a>Diagnóstico y resolución  
 Puede usar la siguiente consulta DMV para ver hasta qué punto el subproceso de la fase de puesta al día se ha retrasado, midiendo la diferencia entre `last_redone_lsn` y `last_received_lsn`.  
  
```sql  
select recovery_lsn, truncation_lsn, last_hardened_lsn, last_received_lsn,   
   last_redone_lsn, last_redone_time  
from sys.dm_hadr_database_replica_states  
  
```  
  
 Si realmente el subproceso de la fase de puesta al día se retrasa, debe investigar la causa principal de la degradación del rendimiento en la réplica secundaria. Si se produce una contención de E/S en la carga de trabajo de los informes, puede usar [Resource Governor](~/relational-databases/resource-governor/resource-governor.md) para controlar, hasta cierto punto, los ciclos de CPU que usa la carga de trabajo de informes para controlar indirectamente los ciclos de E/S realizados. Por ejemplo, si la carga de trabajo de los informes está consumiendo un 10 % de la CPU, pero la carga de trabajo está enlazada a E/S, puede utilizar Resource Governor para limitar el uso de recursos de la CPU al 5 % y regular así la carga de trabajo de lectura. De esta forma, se reduce el impacto en la E/S.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Solucionar problemas de rendimiento en SQL Server (se aplica a SQL Server 2012)](/previous-versions/sql/sql-server-2008/dd672789(v=sql.100))  
  
