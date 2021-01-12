---
description: IHindextypes (Transact-SQL)
title: IHindextypes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- IHindextypes
- IHindextypes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHindextypes system table
ms.assetid: 5eb67d59-a19d-4dba-9d2b-657f87818f6b
author: cawrites
ms.author: chadam
ms.openlocfilehash: 22ed282e39480e98d8df7a86a499e0513ac57081
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094850"
---
# <a name="ihindextypes-transact-sql"></a>IHindextypes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHindextypes** contiene una fila por cada tipo de índice no SQL Server admitido para los publicadores que no son de SQL Server. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**type**|**nvarchar(255)**|Nombre de un tipo de índice admitido que no es de SQL Server.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
