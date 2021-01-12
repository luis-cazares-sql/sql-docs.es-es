---
description: MSsubscriber_info (Transact-SQL)
title: MSsubscriber_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- MSsubscriber_info_TSQL
- MSsubscriber_info
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscriber_info system table
ms.assetid: 5ca22f41-6020-4f72-8110-e69baf3447cb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 05ea16f0f6687a82a04ab04039edf3223bb8ef70
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099689"
---
# <a name="mssubscriber_info-transact-sql"></a>MSsubscriber_info (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscriber_info** contiene una fila por cada par de publicador y suscriptor que se va a insertar suscripciones del distribuidor local. Esta tabla se almacena en la base de datos de distribución.  
  
 **Nota:** Esta tabla del sistema ha quedado desusada y se mantiene para admitir versiones anteriores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="definition"></a>Definición  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|El nombre del publicador.|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**type**|**tinyint**|El tipo de suscriptor:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suscriptor.<br /><br /> **1** = origen de datos ODBC.|  
|**Inicio**|**sysname**|Inicio de sesión para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se almacena en formato cifrado si se agrega el suscriptor con el modo de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**nvarchar (524)**|Contraseña para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se almacena en formato cifrado si se agrega el suscriptor con el modo de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**description**|**nvarchar(255)**|La descripción del suscriptor.|  
|**security_mode**|**int**|Modo de seguridad implementado:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
