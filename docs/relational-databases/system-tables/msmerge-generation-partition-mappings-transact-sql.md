---
title: MSmerge_generation_partition_mappings (T-SQL)
description: Describe el MSmerge_generation_partition_mappings procedimiento almacenado que se utiliza para realizar el seguimiento de los cambios en las particiones en una publicación de combinación.
ms.custom: seo-lt-2019
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_generation_partition_mappings_TSQL
- MSmerge_generation_partition_mappings
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_generation_partition_mappings system table
ms.assetid: 443a4024-ce48-4772-9ee5-95bd6fb6476b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e4bc8d5e833f5ca4c9c19c81985f744c497dfcd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102066"
---
# <a name="msmerge_generation_partition_mappings-transact-sql"></a>MSmerge_generation_partition_mappings (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_generation_partition_mappings** se utiliza para realizar el seguimiento de los cambios en las particiones en una publicación de combinación. Esta tabla se almacena en las bases de datos de publicación y scubscription.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publication_number**|**smallint**|Identifica la publicación de combinación.|  
|**última**|**bigint**|Valor de generación.|  
|**partition_id**|**int**|Identifica la partición.|  
|**changecount**|**int**|Número de veces que la partición ha cambiado.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
