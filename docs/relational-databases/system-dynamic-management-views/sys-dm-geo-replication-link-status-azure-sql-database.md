---
description: sys.dm_geo_replication_link_status (Azure SQL Database)
title: sys.dm_geo_replication_link_status
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_link_status
- dm_geo_replication_link_status_TSQL
- sys.dm_geo_replication_link_status
- sys.dm_geo_replication_link_status_TSQL
helpviewer_keywords:
- dm_geo_replication_link_status dynamic management view
- sys.dm_geo_replication_link_status dynamic management view
ms.assetid: d763d679-470a-4c21-86ab-dfe98d37e9fd
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.custom: seo-dt-2019
ms.openlocfilehash: 2134a7890d23a7a080a576f8337e750bee2563eb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092821"
---
# <a name="sysdm_geo_replication_link_status-azure-sql-database"></a>sys.dm_geo_replication_link_status (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contiene una fila para cada vínculo de replicación entre las bases de datos principal y secundaria en una asociación de replicación geográfica. Aquí se incluyen tanto las bases de datos principales como secundarias. Si existe más de un vínculo de replicación continua para una base de datos principal dada, esta tabla contiene una fila para cada una de las relaciones. La vista se crea en todas las bases de datos, incluida la maestra lógica. Sin embargo, al consultar esta vista en la maestra lógica se devuelve un conjunto vacío.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|link_guid|**uniqueidentifier**|IDENTIFICADOR único del vínculo de replicación.|  
|partner_server|**sysname**|Nombre del servidor de SQL Database que contiene la base de datos vinculada.|  
|partner_database|**sysname**|Nombre de la base de datos vinculada en la que reside el servidor de SQL Database vinculado.|  
|last_replication|**datetimeoffset**|Marca de tiempo de la confirmación de la última transacción por la base de datos secundaria en función del reloj de la base de datos principal. Este valor solo está disponible en la base de datos principal.|  
|replication_lag_sec|**int**|Diferencia de tiempo en segundos entre el valor de last_replication y la marca de tiempo de la confirmación de la transacción en el principal basándose en el reloj de la base de datos principal.  Este valor solo está disponible en la base de datos principal.|  
|replication_state|**tinyint**|El estado de la replicación geográfica para esta base de datos, uno de los siguientes:<br /><br /> 1 = propagación. El destino de replicación geográfica se está inicializando, pero las dos bases de datos aún no se han sincronizado. Hasta que se completa la propagación, no se puede conectar a la base de datos secundaria. Al quitar la base de datos secundaria del servidor principal, se cancelará la operación de propagación.<br /><br /> 2 = puesta al día. La base de datos secundaria se encuentra en un estado transaccionalmente coherente y se está sincronizando constantemente con la base de datos principal.<br /><br /> 4 = suspendido. No es una relación activa de copia continua. Este estado suele indicar que el ancho de banda disponible para el interlink es insuficiente para el nivel de actividad de transacción en la base de datos principal. Sin embargo, la relación de copia continua sigue intacta.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|rol|**tinyint**|Rol de replicación geográfica, uno de los siguientes:<br /><br /> 0 = principal. El database_id hace referencia a la base de datos principal de la Asociación de replicación geográfica.<br /><br /> 1 = secundaria.  El database_id hace referencia a la base de datos principal de la Asociación de replicación geográfica.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|El tipo secundario, uno de los siguientes:<br /><br /> 0 = no se permiten conexiones directas a la base de datos secundaria y la base de datos no está disponible para acceso de lectura.<br /><br /> 2 = se permiten todas las conexiones a la base de datos en el REPL secundario; ication para el acceso de solo lectura.|  
|secondary_allow_connections_desc|**nvarchar(256)**|No<br /><br /> All|  
|last_commit|**datetimeoffset**|Hora de la última transacción confirmada en la base de datos. Si se recupera en la base de datos principal, indica la hora de la última confirmación en la base de datos principal. Si se recupera en la base de datos secundaria, indica la hora de la última confirmación en la base de datos secundaria. Si se recupera en la base de datos secundaria cuando la réplica principal del vínculo de replicación está inactiva, indica hasta qué punto se ha detectado el elemento secundario.|
  
> [!NOTE]  
>  Si la relación de replicación se termina quitando la base de datos secundaria (sección 4,2), la fila de esa base de datos en la vista **Sys.dm_geo_replication_link_status** desaparece.  
  
## <a name="permissions"></a>Permisos  
 Cualquier cuenta con permiso view_database_state puede consultar **Sys.dm_geo_replication_link_status**.  
  
## <a name="example"></a>Ejemplo  
 Mostrar los intervalos de replicación y la hora de la última replicación de las bases de datos secundarias.  
  
```  
SELECT   
     link_guid  
   , partner_server  
   , last_replication  
   , replication_lag_sec   
FROM sys.dm_geo_replication_link_status;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE &#40;Azure SQL Database&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.geo_replication_links &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-geo-replication-links-azure-sql-database.md)   
 [sys.dm_operation_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)   
 [sp_wait_for_database_copy_sync](../system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync.md)
