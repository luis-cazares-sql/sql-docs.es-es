---
description: sys.database_service_objectives (Azure SQL Database)
title: sys.database_service_objectives
titleSuffix: Azure SQL Database
ms.date: 03/21/2018
ms.service: sql-database
ms.prod_service: sql-database, sql-data-warehouse
ms.reviewer: ''
ms.topic: conceptual
keywords:
- Grupo elástico
- Grupo elástico, administración
f1_keywords:
- DATABASE_SERVICE_OBJECTIVES_TSQL
ms.assetid: cecd8c31-06c0-4aa7-85d3-ac590e6874fa
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: seo-dt-2019
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 861e8297a54cff399425e521591cd983afbb0cc6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098023"
---
# <a name="sysdatabase_service_objectives-azure-sql-database"></a>sys.database_service_objectives (Azure SQL Database)
[!INCLUDE [asdb-asdbmi-asa](../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Devuelve la edición (nivel de servicio), el objetivo de servicio (nivel de precios) y el nombre del grupo elástico, si existe, para una base de datos SQL de Azure o Azure Synapse Analytics. Si inició sesión en la base de datos maestra en un servidor de Azure SQL Database, devuelve información sobre todas las bases de datos. Para Azure Synapse Analytics, debe estar conectado a la base de datos maestra.  
  
  
 Para obtener información sobre los precios, consulte [Opciones y rendimiento de SQL Database:](https://azure.microsoft.com/pricing/details/sql-database/) precios de SQL Database y [precios de Azure Synapse Analytics](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).  
  
 Para cambiar la configuración del servicio, vea [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md) y [ALTER DATABASE (Azure Synapse Analytics)](../../t-sql/statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true).  
  
 La vista sys.database_service_objectives contiene las columnas siguientes.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|database_id|int|IDENTIFICADOR de la base de datos, único dentro de una instancia de Azure SQL Database Server. Se combina con [Sys. databases &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).|  
|edition|sysname|El nivel de servicio para la base de datos o el almacenamiento de datos: **básico**, **estándar**, **Premium** o **almacenamiento de datos**.|  
|service_objective|sysname|El plan de tarifa de la base de datos. Si la base de datos está en un grupo elástico, devuelve **ElasticPool**.<br /><br /> En el nivel **básico** , devuelve **Basic**.<br /><br /> Una **sola base de datos en un nivel de servicio estándar** devuelve uno de los siguientes: S0, S1, S2, S3, S4, S6, S7, S9 o S12.<br /><br /> **Una sola base de datos en un nivel Premium** devuelve lo siguiente: P1, P2, P4, P6, P11 o p15.<br /><br /> **Azure Synapse Analytics** devuelve DW100 a través de DW30000c.<br /><br /> Para más información, consulte bases de datos [únicas](/azure/sql-database/sql-database-dtu-resource-limits-single-databases/), [grupos elásticos](/azure/sql-database/sql-database-dtu-resource-limits-elastic-pools/), [almacenamientos de datos](/azure/sql-data-warehouse/what-is-a-data-warehouse-unit-dwu-cdwu/) .|  
|elastic_pool_name|sysname|Nombre del [Grupo elástico](/azure/azure-sql/database/elastic-pool-overview) al que pertenece la base de datos. Devuelve **null** si la base de datos es una sola base de datos o un almacenamiento de datos.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso **dbManager** en la base de datos maestra.  En el nivel de base de datos, el usuario debe ser el creador o el propietario.  
  
## <a name="examples"></a>Ejemplos  
 Este ejemplo se puede ejecutar en la base de datos maestra o en Azure SQL Database bases de datos de usuario. La consulta devuelve el nombre, el servicio y la información de nivel de rendimiento de las bases de datos.  
  
```sql  
SELECT  d.name,   
     slo.*    
FROM sys.databases d   
JOIN sys.database_service_objectives slo    
ON d.database_id = slo.database_id;  
  
```  
  
