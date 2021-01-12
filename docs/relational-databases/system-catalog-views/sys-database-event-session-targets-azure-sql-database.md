---
description: sys.database_event_session_targets (Azure SQL Database)
title: sys.database_event_session_targets (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
ms.assetid: 38d775ee-1fe1-4820-88c6-02b2f875a66b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4f1c04a6556407b7aa8cb0ec046b2079e2a4b166
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102788"
---
# <a name="sysdatabase_event_session_targets-azure-sql-database"></a>sys.database_event_session_targets (Azure SQL Database)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  Devuelve una fila para cada destino de evento de una sesión de eventos.  
  
||  
|-|  
|**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V12 y a las versiones posteriores.|  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|Identificador de la sesión de eventos. No admite valores NULL.|  
|target_id|**int**|Id. del destino. El Id. es único dentro del objeto de sesión de evento. No admite valores NULL.|  
|name|**sysname**|Nombre del destino del evento. No admite valores NULL.|  
|paquete|**sysname**|Nombre del paquete de eventos que contiene el destino de evento. No admite valores NULL.|  
|module|**sysname**|Nombre del módulo que contiene el destino de evento. No admite valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
## <a name="remarks"></a>Observaciones  
 Esta vista tiene las siguientes cardinalidades de relación.  
  
|From|En|Relación|  
|-|-|-|  
|Sys.database_event_session_targets sys.database_event_session_targets.event_session_id|Sys.database_event_sessions sys.database_event_sessions.event_session_id|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Eventos extendidos](../../relational-databases/extended-events/extended-events.md)  
  
  
