---
description: sys.sysindexkeys (Transact-SQL)
title: sys.sysindexkeys (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysindexkeys
- sys.sysindexkeys_TSQL
- sysindexkeys_TSQL
- sys.sysindexkeys
dev_langs:
- TSQL
helpviewer_keywords:
- sysindexkeys system table
- sys.sysindexkeys compatibility view
ms.assetid: 53a33c8d-e5f0-430d-a712-b65f43d64318
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fd333ea7ea5761732eefb96db902b7dbb1840834
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099064"
---
# <a name="syssysindexkeys-transact-sql"></a>sys.sysindexkeys (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre las claves o columnas de un índice de la base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de la tabla.|  
|**indid**|**smallint**|Id. del índice.|  
|**colid**|**smallint**|Identificador de la columna.|  
|**keyno**|**smallint**|Posición de la columna en el índice.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  
