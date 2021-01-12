---
description: MSreplication_options (Transact-SQL)
title: MSreplication_options (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSreplication_options
- MSreplication_options_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSreplication_options system table
ms.assetid: 23cf10d7-8bc1-4368-b5eb-e5576421e776
author: cawrites
ms.author: chadam
ms.openlocfilehash: 263436f5bd3ff691191d74b29c7bced426ba4599
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098151"
---
# <a name="msreplication_options-transact-sql"></a>MSreplication_options (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En la tabla **MSreplication_options** se almacenan los metadatos que la replicación usa internamente. Esta tabla se almacena en la base de datos **maestra** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**optname**|**sysname**|Solo para uso interno.|  
|**value**|**bit**|Solo para uso interno.|  
|**major_version**|**int**|Solo para uso interno.|  
|**minor_version**|**int**|Solo para uso interno.|  
|**revision**|**int**|Solo para uso interno.|  
|**install_failures**|**int**|Solo para uso interno.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
