---
description: sys.dm_cluster_endpoints (Transact-SQL)
title: sys.dm_cluster_endpoints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, big-data-clusters
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_cluster_endpoints
- dm_cluster_endpoints_TSQL
- dm_cluster_endpoints
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_cluster_endpoints dynamic management view
ms.assetid: ''
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-ver15||>=sql-server-linux-2017'
ms.openlocfilehash: 9f9d78015c5413c7e0ec4e827240e7465969d189
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095255"
---
# <a name="sysdm_cluster_endpoints-transact-sql"></a>sys.dm_cluster_endpoints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|`sysname`|Nombre del servicio expuesto externamente en un clúster de macrodatos de SQL. Identificador único para el extremo. Clave para esta vista. No admite valores NULL. |  
|description|`nvarchar(4000)`|Descripción del servicio. No admite valores NULL. |
|endpoint|`sysname`|Dirección URL del extremo o atributo de conexión. No admite valores NULL. |
|protocol_desc|`sysname`|Descripción del Protocolo de extremo |

## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.

## <a name="see-also"></a>Consulte también

[¿Qué [!INCLUDE[big-data-clusters-2019](../../includes/ssbigdataclusters-ss-nover.md)] son ](../../big-data-cluster/big-data-cluster-overview.md)?
