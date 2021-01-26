---
title: Difundir un mensaje de cierre del sistema (símbolo del sistema) | Microsoft Docs
description: Descubra cómo usar el comando net send para difundir un mensaje en SQL Server. Vea cómo determinar qué usuarios están conectados actualmente a SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server, stopping
- named instances [SQL Server], broadcasting shutdown messages
- shutdown message broadcast
- broadcasting shutdown message
- command prompt [SQL Server], broadcasting shutdown messages
- default instances [SQL Server], broadcasting shutdown messages
- stopping SQL Server
ms.assetid: 9f20ccd5-d952-431d-ba12-339911af9430
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 44a2ecf5405ceda7e85b0f7b0831759dc2313b63
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783392"
---
# <a name="broadcast-a-shutdown-message-command-prompt"></a>Difundir un mensaje de cierre del sistema (símbolo del sistema)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo difundir un mensaje de cierre en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el comando **net send** . En el mensaje, incluya la hora a la que se va a detener la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que los usuarios puedan finalizar sus tareas.  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-broadcast-a-shutdown-message"></a>Para difundir un mensaje de cierre  
  
1.  Desde el símbolo del sistema, escriba:  
  
     **net send /users "message"**  
  
     La opción **/users** especifica que el mensaje se enviará a todos los usuarios conectados al servidor  
  
> [!NOTE]  
>  El comando **net send** requiere que se esté ejecutando el servicio de mensajería tanto en el equipo emisor como en el receptor. El servicio de mensajería se deshabilita de forma predeterminada en Windows Server 2003. Para obtener información sobre **net send**, vea la documentación de Windows.  
  
 Es posible que en su red sea más conveniente ponerse en contacto con los usuarios por correo electrónico o teléfono. Para determinar qué usuarios están conectados actualmente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilice el Monitor de actividad. Para obtener información sobre el Monitor de actividad, vea [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md) y [Abrir el Monitor de actividad &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md).  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
  
