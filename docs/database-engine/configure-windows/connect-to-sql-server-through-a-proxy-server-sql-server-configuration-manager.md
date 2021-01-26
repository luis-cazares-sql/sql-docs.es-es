---
title: Conectarse a SQL Server a través de un servidor proxy (Administrador de configuración de SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo utilizar el Administrador de configuración de SQL Server para conectarse a SQL Server a través de un servidor proxy. Vea cómo usar WinSock remoto (RWS) para escuchar de forma remota.
ms.custom: ''
ms.date: 12/15/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Remote WinSock
- RWS
- LATs
- proxy servers [SQL Server]
- connections [SQL Server], proxy server
- Microsoft Proxy Server [SQL Server]
- local address tables [SQL Server]
ms.assetid: 39714de0-2a1f-4179-9091-5c3fa4612545
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 325bdae16fdc8309d0f40134ba31041e5b47b547
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783270"
---
# <a name="connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager"></a>Conectarse a SQL Server a través de un servidor proxy (Administrador de configuración de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo conectarse a SQL Server mediante un servidor proxy en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizando el Administrador de configuración de SQL Server. Para escuchar de forma remota mediante WinSock remoto (RWS), defina la tabla de direcciones locales (LAT) del servidor proxy de modo que la dirección del nodo en espera de conexiones quede fuera de la lista de entradas LAT.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-enable-connections-to-sql-server-through-proxy-server"></a>Para permitir conexiones a SQL Server a través de un servidor proxy  
  
1.  Siga los pasos de [Configurar un servidor para que escuche en un puerto TCP específico &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md) para determinar qué puertos TCP/IP usa [!INCLUDE[ssDE](../../includes/ssde-md.md)], o para configurar [!INCLUDE[ssDE](../../includes/ssde-md.md)] para usar un puerto deseado.  
  
2.  Defina en su servidor proxy la tabla de direcciones locales (LAT) del servidor proxy de manera que la dirección del nodo en espera de conexiones se encuentre fuera del rango de entradas LAT. Para obtener más información, vea la documentación de su servidor proxy.  
  
> [!NOTE]
>  Este tema se aplica a [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]local. En caso de problemas de conexión relacionados con [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], consulte [Solución de problemas de conexión de Base de datos SQL de Azure](/azure/sql-database/sql-database-troubleshoot-common-connection-issues).
