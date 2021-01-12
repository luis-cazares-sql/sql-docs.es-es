---
description: sys.sysdepends (Transact-SQL)
title: sys.sysdepende de (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysdepends_TSQL
- sysdepends
- sysdepends_TSQL
- sys.sysdepends
dev_langs:
- TSQL
helpviewer_keywords:
- sysdepends system table
- sys.sysdepends compatibility view
ms.assetid: f9c182cb-386f-4e72-859f-9f1115b389f9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 18abac997911fcd78de49d83d12144821c7764ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099101"
---
# <a name="syssysdepends-transact-sql"></a>sys.sysdepends (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información de dependencia entre objetos (vistas, procedimientos y desencadenadores) de la base de datos y los objetos (tablas, vistas y procedimientos) contenidos en su definición.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id**|**int**|Id. de objeto.|  
|**depid**|**int**|Id. del objeto dependiente.|  
|**número**|**smallint**|Número de procedimiento.|  
|**depnumber**|**smallint**|Número del procedimiento dependiente.|  
|**status**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deptype**|**tinyint**|Identifica el tipo de objeto dependiente:<br /><br /> 0 = Objeto o columna (solo referencias no enlazadas a esquemas)<br /><br /> 0 = Objeto o columna (referencias enlazadas a esquemas)|  
|**depdbid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**depsiteid**|**smallint**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**selall**|**bit**|1 = El objeto se utiliza en una instrucción SELECT *.<br /><br /> 0 = No.|  
|**resultobj**|**bit**|1 = Se está actualizando el objeto.<br /><br /> 0 = No.|  
|**readobj**|**bit**|1 = Se está leyendo el objeto.<br /><br /> 0 = No.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)   
 [sp_depends &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-depends-transact-sql.md)   
 [sys.sql_dependencies &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)  
  
  
