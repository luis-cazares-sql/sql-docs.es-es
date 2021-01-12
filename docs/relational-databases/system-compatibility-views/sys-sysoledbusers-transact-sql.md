---
description: sys.sysoledbusers (Transact-SQL)
title: sys.sysoledbusers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.sysoledbusers
- sys.sysoledbusers_TSQL
- sysoledbusers
- sysoledbusers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysoledbusers system table
- sys.sysoledbusers compatibility view
ms.assetid: fe924c17-9cad-4b2b-8124-1e0fd82931e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 442ca09761a75a5ca53b028bf8ac0a7f58c59314
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095462"
---
# <a name="syssysoledbusers-transact-sql"></a>sys.sysoledbusers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

    
> [!IMPORTANT]  
>  Esta tabla del sistema de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] se incluye en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como una vista para la compatibilidad con versiones anteriores. En su lugar, se recomienda usar [vistas de catálogo](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md) .  
  
 Contiene una fila por cada correspondencia entre usuario y contraseña en el servidor vinculado especificado. **sysoledbusers** se almacena en la base de datos **maestra** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**rmtsrvid**|**smallint**|SID (número de identificación de seguridad) del servidor.|  
|**rmtloginame**|**nvarchar (** 128 **)**|Nombre del inicio de sesión remoto al que se asigna **loginsid** para **rmtservid** vinculado.|  
|**rmtpassword**|**nvarchar (** 128 **)**|Devuelve NULL.|  
|**loginsid**|**varbinary (** 85 **)**|SID del inicio de sesión local que se va a asignar.|  
|**status**|**smallint**|Si este valor es 1, la asignación debe utilizar las credenciales del usuario.|  
|**changedate**|**datetime**|Fecha en que cambió por última vez la información de asignación.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
