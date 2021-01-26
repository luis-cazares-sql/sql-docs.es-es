---
title: Configurar un servidor para que escuche en una canalización alternativa | Microsoft Docs
description: Descubra cómo configurar la canalización con nombre en la que escucha el Motor de base de datos de SQL Server. Obtenga información sobre cómo conectar una aplicación cliente a una canalización con nombre específica.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Named Pipes [SQL Server], configuring
- listening [SQL Server], pipes
- pipes [SQL Server], alternate
- alternate pipes [SQL Server]
ms.assetid: 914f7491-e2be-4b0d-b3aa-fe5409cdbafa
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 70abda90ef4b7c047a4833baa792adeff60b8f12
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783232"
---
# <a name="configure-a-server-to-listen-on-an-alternate-pipe"></a>Configurar un servidor para que escuche en una canalización alternativa
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar un servidor para que escuche en una canalización alternativa en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de SQL Server. De forma predeterminada, la instancia predeterminada de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] escucha en la canalización con nombre \\\\.\pipe\sql\query. Las instancias con nombre de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y [!INCLUDE[ssEW](../../includes/ssew-md.md)] escuchan en otras canalizaciones.  
  
 Hay tres formas de conectarse a una canalización con nombre específica con una aplicación cliente:  
  
-   Ejecutar el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor.  
  
-   Crear un alias en el cliente, especificando la canalización con nombre.  
  
-   Programar el cliente para conectarse mediante una cadena de conexión personalizada.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-configure-the-named-pipe-used-by-the-sql-server-database-engine"></a>Para configurar la canalización con nombre utilizada por el Motor de base de datos de SQL Server  
  
1.  En el Administrador de configuración de SQL Server, en el panel de consola, expanda **Configuración de red de SQL Server** y después **Protocolos de** *\<instance name>* .  
  
2.  En el panel de detalles, haga clic con el botón derecho en **Canalizaciones con nombre** y, después, haga clic en **Propiedades**.  
  
3.  En la pestaña **Protocolo** , en el cuadro **Nombre de canalización** , escriba la canalización en la que desea que escuche [!INCLUDE[ssDE](../../includes/ssde-md.md)] y, a continuación, haga clic en **Aceptar**.  
  
4.  En el panel de la consola, haga clic en **Servicios de SQL Server**.  
  
5.  En el panel de detalles, haga clic con el botón derecho en **SQL Server (** \<instance name> **)** y, después, haga clic en **Reiniciar** para detener y reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está escuchando en una canalización alternativa, hay tres formas de conectarse a una canalización con nombre específica con una aplicación cliente:  
  
-   Ejecutar el servicio Explorador de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor.  
  
-   Crear un alias en el cliente, especificando la canalización con nombre.  
  
-   Programar el cliente para conectarse mediante una cadena de conexión personalizada.  
  
## <a name="see-also"></a>Consulte también  
 [Crear o eliminar un alias de servidor para que lo utilice un cliente &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/create-or-delete-a-server-alias-for-use-by-a-client.md)   
 [Configuración de red del servidor](../../database-engine/configure-windows/server-network-configuration.md)  
  
  
