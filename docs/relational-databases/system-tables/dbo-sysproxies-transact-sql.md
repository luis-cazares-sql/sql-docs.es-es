---
description: dbo.sysproxies (Transact-SQL)
title: dbo.sysproxies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dbo.sysproxies_TSQL
- sysproxies_TSQL
- dbo.sysproxies
- sysproxies
dev_langs:
- TSQL
helpviewer_keywords:
- sysproxies system table
ms.assetid: a73da875-be22-45fc-b5e2-ea7ebd48e2d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3d04cd1822a9c7e0edc309a3500e800b2df58fd0
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098325"
---
# <a name="dbosysproxies-transact-sql"></a>dbo.sysproxies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Define atributos de una cuenta de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta tabla se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Id. de la cuenta de proxy.|  
|**name**|**sysname**|Nombre de la cuenta de proxy.|  
|**credential_id**|**int**|Id. de la credencial que utiliza la cuenta de proxy.|  
|**enabled**|**tinyint**|Estado de la cuenta de proxy:<br /><br /> **0** = deshabilitado. **1** = habilitada.|  
|**description**|**nvarchar(512)**|Descripción proporcionada por el usuario cuando se creó la cuenta de proxy.|  
|**user_sid**|**varbinary(85)**|*Security_identifier* de Microsoft Windows del usuario o grupo asociado a la credencial del proxy.|  
|**credential_date_created**|**datetime**|Es la fecha y hora en que se creó la credencial.|  
  
## <a name="remarks"></a>Observaciones  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden tener acceso a la tabla **sysproxies** .  
  
## <a name="see-also"></a>Consulte también  
 [dbo.sysProxyLogin &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)   
 [dbo.sysproxysubsystem &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)   
 [dbo.syssubsistemas &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-syssubsystems-transact-sql.md)  
  
  
