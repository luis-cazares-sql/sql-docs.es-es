---
title: MSmerge_conflict_publication_article (T-SQL)
description: Describe el MSmerge_conflict_publication_article procedimiento almacenado que contiene información sobre las filas en conflicto o los cambios de fila que se deshicieron para lograr la convergencia de datos.
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSmerge_conflict_publication_article_TSQL
- MSmerge_conflict_publication_article
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_conflict_publication_article system table
ms.assetid: dc4490b4-02d8-4dfc-98f5-0cf8de8e11be
author: cawrites
ms.author: chadam
ms.openlocfilehash: 741aeb7e38463c738862b78cfb8140d54368b1ab
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096260"
---
# <a name="msmerge_conflict_publication_article-transact-sql"></a>MSmerge_conflict_publication_article (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_conflict_publication_article** contiene información sobre las filas que han producido conflictos o cambios en las filas que se han deshecho para lograr la convergencia de datos. En una publicación, cada tabla replicada posee una tabla de conflictos; el nombre de esta tabla de conflictos se anexa al nombre del artículo y la publicación. Estas tablas de conflictos específicos del artículo se encuentran en la base de datos utilizada para registrar los conflictos, que normalmente es la base de datos de publicaciones, pero que puede ser la base de datos de suscripciones si el registro de conflictos está descentralizado.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**_\_nombre de columna del artículo \__**|**variable**|Representa una columna en una tabla replicada. Esta tabla del sistema contiene una columna para cada columna del artículo de la tabla.|  
|**rowguid**|**uniqueidentifier**|El identificador de fila para la fila de conflicto.|  
|**ModifiedDate**|**datetime**|La fecha en que ocurrió el conflicto.|  
|**identificador del origen de origen \_ \_**|**uniqueidentifier**|La suscripción en la que se deshizo el cambio de fila o se perdió el conflicto.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
