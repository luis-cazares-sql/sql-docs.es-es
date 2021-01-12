---
description: sysdbmaintplans (Transact-SQL)
title: sysdbmaintplans (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysdbmaintplans_TSQL
- sysdbmaintplans
dev_langs:
- TSQL
helpviewer_keywords:
- sysdbmaintplans system table
ms.assetid: 0363296a-3082-48a9-9eb5-a1020b2f541a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2aec8e8b94dc341225a5e34e35f2f168fa83e56e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098096"
---
# <a name="sysdbmaintplans-transact-sql"></a>sysdbmaintplans (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Esta tabla se incluye en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para conservar la información existente de las instancias actualizadas de una versión anterior de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no cambia el contenido de esta tabla. Esta tabla se almacena en la base de datos **msdb** .  
  
 [!INCLUDE[ssNoteDepNextAvoid](../../includes/ssnotedepnextavoid-md.md)]  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Id. del plan de mantenimiento de bases de datos.|  
|**plan_name**|**sysname**|Nombre del plan de mantenimiento de bases de datos.|  
|**date_created**|**datetime**|Fecha de creación del plan de mantenimiento de bases de datos.|  
|**propietario**|**sysname**|Propietario del plan de mantenimiento de bases de datos.|  
|**max_history_rows**|**int**|Número máximo de filas asignadas para registrar el historial del plan de mantenimiento de bases de datos de la tabla del sistema.|  
|**remote_history_server**|**sysname**|Nombre del servidor remoto en el que se puede escribir el informe del historial.|  
|**max_remote_history_rows**|**int**|Número máximo de filas asignadas en la tabla del sistema de un servidor remoto en el que se puede escribir el informe del historial.|  
|**user_defined_1**|**int**|El valor predeterminado es NULL.|  
|**user_defined_2**|**nvarchar(100**|El valor predeterminado es NULL.|  
|**user_defined_3**|**datetime**|El valor predeterminado es NULL.|  
|**user_defined_4**|**uniqueidentifier**|El valor predeterminado es NULL.|  
|**log_shipping**|**bit**|Estado del trasvase de registros:<br /><br /> **0** = deshabilitado **1** = habilitado|  
  
  
