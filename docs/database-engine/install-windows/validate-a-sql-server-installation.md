---
title: Validar una instalación de SQL Server | Microsoft Docs
description: El informe de detección de SQL Server se puede utilizar para comprobar la versión de SQL Server y las características de SQL Server instaladas en el equipo.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- validating installations [SQL Server]
ms.assetid: 1689af50-d2b8-4aa6-8f27-cc7127157fc8
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2138be8c23aa6a9849ce19872d3c1b455223540e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170127"
---
# <a name="validate-a-sql-server-installation"></a>Validar una instalación de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  El informe de detección de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se puede utilizar para comprobar la versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instaladas en el equipo. En el **Informe de detección de características instaladas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** se muestra un informe de todos los productos y características de [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL15](../../includes/sssqlv14-md.md)] que están instalados en el servidor local. El informe de detección de características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está disponible en la página **Herramientas** del Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 ## <a name="run-ssnoversion-features-discovery-report"></a>Ejecutar el informe de detección de características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
 Inicie el centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; para ello, en el menú **Inicio**, seleccione **Todos los programas**, **[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] \<Version Name>** , **Herramientas de configuración** y haga clic en **Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** . Para ejecutar el informe de detección de características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], haga clic en **Herramientas** en el área de navegación izquierda del **Centro de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** y haga clic en **Informe de detección de características instaladas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** .  
  
 El informe de detección de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se guarda en %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<última sesión de instalación\>.  
  
 También puede generar el informe de detección a través de la línea de comandos. Ejecute "Setup.exe /Action=RunDiscovery" desde un símbolo del sistema. Si agrega "/q" a la línea de comandos anterior, no se mostrará ninguna interfaz de usuario, pero el informe se creará igualmente en %ProgramFiles%\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\*nnn*\Setup Bootstrap\Log\\<última sesión de instalación\>.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
  
