---
title: Incluir servidor testigo (Asistente para la configuración de seguridad de la creación de reflejo de bases de datos)
description: Describe la página "Incluir servidor testigo" del "Asistente para configuración de seguridad de la creación de reflejo de bases de datos" en la GUI de SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.configdbmsecurwiz.inclwitness.f1
ms.assetid: f04b38a4-f4e2-4d4c-bdac-7cc70e5a5684
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: f748e8ad1c55b29ebc2db67520b51f76797f740a
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641627"
---
# <a name="include-witness-server-configure-database-mirroring-security-wizard"></a>Incluir servidor testigo (Asistente para la configuración de seguridad de la creación de reflejo de bases de datos)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilice esta página para especificar si desea incluir un servidor testigo en esta configuración de seguridad de la creación de reflejo de bases de datos.  
  
 **Para configurar la creación de reflejo de la base de datos mediante SQL Server Management Studio**  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Iniciar el Asistente para la configuración de seguridad de la creación de reflejo de la base de datos &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
## <a name="options"></a>Opciones  
 **Sí**  
 Haga clic en esta opción para incluir una instancia del servidor testigo en la configuración de seguridad. El testigo es necesario para el modo de seguridad alta con conmutación automática por error; dicho modo admite la conmutación automática por error en la instancia del servidor reflejado si se producen errores en la instancia del servidor principal.  
  
 **No**  
 Haga clic en esta opción para configurar la seguridad sin testigo.  
  
## <a name="see-also"></a>Consulte también  
 [Propiedades de la base de datos &#40;página Creación de reflejo&#41;](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Testigo de creación de reflejo de la base de datos](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
