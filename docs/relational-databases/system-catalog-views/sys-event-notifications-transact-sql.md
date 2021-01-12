---
description: sys.event_notifications (Transact-SQL)
title: sys.event_notifications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- event_notifications_TSQL
- event_notifications
- sys.event_notifications_TSQL
- sys.event_notifications
dev_langs:
- TSQL
helpviewer_keywords:
- sys.event_notifications catalog view
ms.assetid: 136a76ee-2b35-4418-ab46-fda2d51f7d99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ac0c2f356f713581b582d64a38e2db1bd2fb5ec5
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095602"
---
# <a name="sysevent_notifications-transact-sql"></a>sys.event_notifications (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada objeto que es una notificación de eventos, con **Sys. Objects. Type** = en.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre de la notificación de eventos.|  
|**object_id**|**int**|Número de identificación del objeto. Es único en una base de datos.|  
|**parent_class**|**tinyint**|Clase del elemento primario.<br /><br /> 0 = Base de datos<br /><br /> 1 = objeto o columna|  
|**parent_class_desc**|**nvarchar(60)**|DATABASE<br /><br /> OBJECT_OR_COLUMN|  
|**parent_id**|**int**|Id. distinto de cero del objeto primario.<br /><br /> 0 = La clase primaria es la base de datos.|  
|**create_date**|**datetime**|Fecha de creación.|  
|**modify_date**|**datetime**|Siempre es igual a **create_date**.|  
|**service_name**|**nvarchar(256)**|Nombre del servicio de destino al que se envía la notificación.|  
|**broker_instance**|**nvarchar(128)**|Instancia de agente a la que se envía la notificación.|  
|**principal_id**|**int**|El Id. de la entidad de seguridad de la base de datos a la que pertenece esta notificación de eventos.|  
|**creator_sid**|**varbinary(85)**|SID del inicio de sesión que ha creado la notificación de eventos.<br /><br /> Es NULL si no se especifica la opción FAN_IN.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
