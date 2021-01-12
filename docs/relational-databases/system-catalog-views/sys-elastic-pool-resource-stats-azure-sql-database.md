---
description: sys.elastic_pool_resource_stats (Azure SQL Database)
title: sys.elastic_pool_resource_stats
titleSuffix: Azure SQL Database
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.elastic_pool_resource_stats catalog view
helpviewer_keywords:
- sys.elastic_pool_resource_stats_TSQL
- sys.elastic_pool_resource_stats
- elastic_pool_resource_stats_TSQL
- elastic_pool_resource_stats
ms.assetid: f242c1bd-3cc8-4c8b-8aaf-c79b6a8a0329
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1b97fe07119f28211186e2efa3986308af477947
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102790"
---
# <a name="syselastic_pool_resource_stats-azure-sql-database"></a>sys.elastic_pool_resource_stats (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  Devuelve estadísticas de uso de recursos de todos los grupos elásticos de un servidor de SQL Database. Para cada grupo elástico hay una fila por cada ventana de informe de 15 segundos (cuatro filas por minuto). Esto incluye uso de CPU, E/S, registro, almacenamiento y empleo simultáneo de solicitudes o sesiones por parte de todas las bases de datos del grupo. Estos datos se conservan durante 14 días. 
  
||  
|-|  
|**Se aplica a**:  [!INCLUDE[ssSDS](../../includes/sssds-md.md)] V12.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**start_time**|**datetime2**|Hora UTC que indica el inicio del intervalo de informes de 15 segundos.|  
|**end_time**|**datetime2**|Hora UTC que indica el final del intervalo de informes de 15 segundos.|  
|**elastic_pool_name**|**nvarchar(128)**|Nombre del grupo de bases de datos elásticas.|  
|**avg_cpu_percent**|**decimal (5, 2)**|Uso de proceso medio en porcentaje del límite del grupo.|  
|**avg_data_io_percent**|**decimal (5, 2)**|Uso de E/S medio en porcentaje basado en el límite del grupo.|  
|**avg_log_write_percent**|**decimal (5, 2)**|Uso de recursos de escritura medio en porcentaje del límite del grupo.|  
|**avg_storage_percent**|**decimal (5, 2)**|Uso de almacenamiento medio en porcentaje del límite de almacenamiento del grupo.|  
|**max_worker_percent**|**decimal (5, 2)**|Cantidad máxima de trabajos simultáneos (solicitudes) en porcentaje basado en el límite del grupo.|  
|**max_session_percent**|**decimal (5, 2)**|Cantidad máxima de sesiones simultáneas en porcentaje basado en el límite del grupo.|  
|**elastic_pool_dtu_limit**|**int**|Configuración de cantidad máxima de DTU de grupos elásticos actual para este grupo elástico durante este intervalo.|  
|**elastic_pool_storage_limit_mb**|**bigint**|Configuración de límite máximo de almacenamiento de grupos elásticos actual para este grupo elástico en megabytes durante este intervalo.|
|**avg_allocated_storage_percent**|**decimal (5, 2)**|El porcentaje de espacio de datos asignado por todas las bases de datos del grupo elástico.  Esta es la proporción de espacio de datos asignado al tamaño máximo de los datos para el grupo elástico.  Para obtener más información, vea [Administración del espacio de archivo en SQL Database](/azure/sql-database/sql-database-file-space-management)|  
  
## <a name="remarks"></a>Observaciones

 Esta vista existe en la base de datos maestra del servidor de SQL Database. Debe estar conectado a la base de datos maestra para consultar **Sys.elastic_pool_resource_stats**.  
  
## <a name="permissions"></a>Permisos

 Requiere la pertenencia al rol **dbmanager** .  
  
## <a name="examples"></a>Ejemplos

 En el ejemplo siguiente se devuelven los datos de uso de recursos ordenados por la hora más reciente para todos los grupos de bases de datos elásticas en el servidor de SQL Database actual.  
  
```sql
SELECT * FROM sys.elastic_pool_resource_stats
ORDER BY end_time DESC;  
```

 En el ejemplo siguiente se calcula el consumo medio de porcentaje de DTU para un grupo determinado.  

```sql
SELECT start_time, end_time,
  (SELECT Max(v)
FROM (VALUES (avg_cpu_percent), (avg_data_io_percent), (avg_log_write_percent)) AS value(v)) AS [avg_DTU_percent]
FROM sys.elastic_pool_resource_stats
WHERE elastic_pool_name = '<your pool name>'
ORDER BY end_time DESC;  
```

## <a name="see-also"></a>Consulte también

 [Dominar el crecimiento explosivo con bases de datos elásticas](/azure/azure-sql/database/elastic-pool-overview)   
 [Creación y administración de un grupo de bases de datos elásticas SQL Database](/azure/azure-sql/database/elastic-pool-overview)   
 [sys.resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database.md)   
 [sys.dm_db_resource_stats &#40;Azure SQL Database&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database.md)  
  
