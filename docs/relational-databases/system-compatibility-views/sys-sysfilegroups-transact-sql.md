---
description: sys.sysfilegroups (Transact-SQL)
title: Grupos de archivos sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysfilegroups_TSQL
- sys.sysfilegroups
- sysfilegroups
- sys.sysfilegroups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfilegroups system table
- sys.sysfilegroups compatibility view
ms.assetid: e567fa07-31cd-43cc-b8c7-ba6108baca80
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 609563f4998215367649a33cfc5136b8edd0e26b
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099121"
---
# <a name="syssysfilegroups-transact-sql"></a>sys.sysfilegroups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada grupo de archivos de una base de datos. Esta tabla contiene al menos la entrada correspondiente al grupo de archivos principal.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**GROUPID**|**smallint**|Número de identificación de grupo, único en cada base de datos.|  
|**allocpolicy**|**smallint**|Reservada|  
|**status**|**int**|0x8 = Solo lectura<br /><br /> 0x10 = Predeterminado|  
|**GroupName**|**sysname**|Nombre del grupo de archivos.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
