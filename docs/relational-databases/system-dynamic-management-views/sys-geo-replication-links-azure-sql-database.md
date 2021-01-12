---
description: sys.geo_replication_links (Azure SQL Database)
title: sys.geo_replication_links (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- dm_geo_replication_links_TSQL
- dm_geo_replication_links
- sys.dm_geo_replication_links
- sys.dm_geo_replication_links_TSQL
helpviewer_keywords:
- sys.dm_geo_replication_links dynamic management view
- dm_geo_replication_links dynamic management view
ms.assetid: 58911798-1d60-4f28-87ab-2def2bfc3de7
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: f10f318ebba5627957f1555b5979e8993f6f5e02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093909"
---
# <a name="sysgeo_replication_links-azure-sql-database"></a>sys.geo_replication_links (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Contiene una fila para cada vínculo de replicación entre las bases de datos principal y secundaria en una asociación de replicación geográfica. Esta vista reside en la base de datos maestra lógica.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|**int**|IDENTIFICADOR de la base de datos actual en la vista sys. databases.|  
|start_date|**datetimeoffset**|Hora UTC en un centro de datos de SQL Database regional al iniciarse la replicación de base de datos.|  
|modify_date|**datetimeoffset**|Hora UTC en el centro de datos de SQL Database regional cuando se ha completado la replicación geográfica de la base de datos. La nueva base de datos se sincroniza con la base de datos principal en este momento.|  
|link_guid|**uniqueidentifier**|IDENTIFICADOR único del vínculo de replicación geográfica.|  
|partner_server|**sysname**|Nombre del servidor de SQL Database que contiene la base de datos con replicación geográfica.|  
|partner_database|**sysname**|Nombre de la base de datos con replicación geográfica en el servidor de SQL Database vinculado.|  
|replication_state|**tinyint**|El estado de la replicación geográfica para esta base de datos, uno de los siguientes:<br /><br /> 0 = pendiente. La creación de la base de datos secundaria activa está programada, pero aún no se han completado los pasos de preparación necesarios.<br /><br /> 1 = propagación. El destino de replicación geográfica se está inicializando, pero las dos bases de datos aún no se han sincronizado. Hasta que se completa la propagación, no se puede conectar a la base de datos secundaria. Al quitar la base de datos secundaria del servidor principal, se cancelará la operación de propagación.<br /><br /> 2 = puesta al día. La base de datos secundaria se encuentra en un estado transaccionalmente coherente y se está sincronizando constantemente con la base de datos principal.|  
|replication_state_desc|**nvarchar(256)**|PENDING<br /><br /> SEEDING<br /><br /> CATCH_UP|  
|rol|**tinyint**|Rol de replicación geográfica, uno de los siguientes:<br /><br /> 0 = principal. El database_id hace referencia a la base de datos principal de la Asociación de replicación geográfica.<br /><br /> 1 = secundaria.  El database_id hace referencia a la base de datos principal de la Asociación de replicación geográfica.|  
|role_desc|**nvarchar(256)**|PRIMARY<br /><br /> SECONDARY|  
|secondary_allow_connections|**tinyint**|El tipo secundario, uno de los siguientes:<br /><br /> 0 = No. No se puede obtener acceso a la base de datos secundaria hasta la conmutación por error.<br /><br /> 1 = solo lectura. Solo se puede acceder a la base de datos secundaria a las conexiones de cliente con ApplicationIntent = ReadOnly.<br /><br /> 2 = Todas. La base de datos secundaria es accesible para cualquier conexión de cliente.|  
|secondary_allow_connections _desc|**nvarchar(256)**|No<br /><br /> All<br /><br /> Solo lectura|  
  
## <a name="permissions"></a>Permisos

Esta vista solo está disponible en la base de datos **maestra** para el inicio de sesión principal de nivel de servidor.  
  
## <a name="example"></a>Ejemplo

Mostrar todas las bases de datos con vínculos de replicación geográfica.  

```sql
SELECT
     database_id  
   , start_date  
   , partner_server  
   , partner_database  
   , replication_state  
   , role_desc  
   , secondary_allow_connections_desc
FROM sys.geo_replication_links;  
```

## <a name="see-also"></a>Consulte también

 [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md)   
 [sys.dm_geo_replication_link_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database.md)   
 [sys.dm_operation_status &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database.md)
