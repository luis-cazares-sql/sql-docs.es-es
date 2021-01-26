---
description: Ejecutar la aplicación Data Quality Client
title: Ejecutar la aplicación Data Quality Client
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.browseforservers.f1
- sql13.dqs.connecttoserver.f1
ms.assetid: 0b2aa202-7ab2-4c9d-b0f1-802588053a1e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: f83334dbbcc50642c463681614db5f23e10f6807
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765640"
---
# <a name="run-the-data-quality-client-application"></a>Ejecutar la aplicación Data Quality Client

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Ejecute [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]e inicie sesión en una instancia de [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 Debe haber completado la instalación del [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] ejecutando el archivo DQSInstaller.exe. Para obtener más información, vea [Ejecutar DQSInstaller.exe para completar la instalación del servidor de calidad de datos](../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para poder iniciar sesión en [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)], es necesario disponer de uno de los tres roles de DQS (dqs_administrator, dqs_kb_editor o dqs_kb_operator) en la base de datos DQS_MAIN.  
  
##  <a name="run-data-quality-client"></a><a name="Run"></a> Ejecutar Data Quality Client  
 Para ejecutar [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] en el equipo donde lo ha instalado:  
  
1.  En el menú **Inicio** , seleccione el **[!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]** **Data Quality Client**.  
  
2.  En el cuadro de diálogo **Conectar con el servidor**:  
  
    1.  Especifique el servidor al que desea conectar la aplicación [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Seleccione **(LOCAL)** para conectarse con [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] en el equipo local. También puede hacer clic en la flecha abajo y seleccionar **\<Browse network for more servers>** para conectarse a un servidor diferente (o para conectarse al servidor local por nombre). Se mostrará el cuadro de diálogo **Buscar servidores** . Puede seleccionar un servidor en la pestaña **Servidores locales** o en la pestaña **Servidores de redes** .  
  
    2.  Para cifrar la transferencia de datos entre [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] y [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)], haga clic en **Opciones** y luego active la casilla **Cifrar conexión** .  
  
3.  Haga clic en **Conectar**.  
  
 Aparece la página de inicio del [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Para más información, vea [Página de inicio del cliente de calidad de datos](../data-quality-services/data-quality-client-home-screen.md).  
  
  
