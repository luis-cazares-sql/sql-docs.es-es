---
description: DROP SERVICE (Transact-SQL)
title: DROP SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_SERVICE_TSQL
- DROP SERVICE
dev_langs:
- TSQL
helpviewer_keywords:
- deleting services
- services [Service Broker], removing
- dropping services
- DROP SERVICE statement
- removing services
ms.assetid: 2351bba7-0f2a-4cda-b3b2-6a88b8747c53
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0aa5475984945592d968b237c1129a4f5b55d6b6
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099441"
---
# <a name="drop-service-transact-sql"></a>DROP SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita un servicio existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP SERVICE service_name  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *service_name*  
 Nombre del servicio que se va a quitar. No se pueden especificar nombres de servidor, base de datos o esquema.  
  
## <a name="remarks"></a>Comentarios  
 No se puede quitar un servicio si hay alguna prioridad de conversación que haga referencia a él.  
  
 Al quitar un servicio, se eliminan todos los mensajes para dicho servicio de la cola que utiliza. [!INCLUDE[ssSB](../../includes/sssb-md.md)] envía un error al lado remoto de cualquier conversación abierta que utilice el servicio.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, el permiso para quitar un servicio corresponde al propietario del servicio, a los miembros de los roles fijos de base de datos db_ddladmin o db_owner, y a los miembros del rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita el servicio `//Adventure-Works.com/Expenses`.  
  
```sql  
DROP SERVICE [//Adventure-Works.com/Expenses] ;  
```  
  
## <a name="see-also"></a>Vea también  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [ALTER SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
