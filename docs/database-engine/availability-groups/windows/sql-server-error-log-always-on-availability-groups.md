---
title: Registro de errores de SQL Server (grupos de disponibilidad)
description: Obtenga información sobre los eventos de registro de errores de SQL Server que afectan a un grupo de disponibilidad Always On y los síntomas que deben conducir a la revisión del registro de errores.
ms.custom: seo-lt-2019
ms.date: 06/13/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
ms.assetid: 39d0c98d-75af-4dd1-b908-30d31af56f2a
author: rothja
ms.author: jroth
ms.openlocfilehash: a538b0530202a56cb1682d97946ec06d86f19a36
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97638972"
---
# <a name="sql-server-error-log-always-on-availability-groups"></a>Registro de errores de SQL Server (Grupos de disponibilidad Always On)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  El registro de errores de SQL Server informa de eventos que afectan a los grupos de disponibilidad Always On, por ejemplo:  
  
-   Comunicación con el clúster de clústeres de conmutación por error de Windows Server    
-   Transiciones de estado de réplicas de disponibilidad    
-   Transiciones de estado de bases de datos de disponibilidad    
-   Estado de conectividad de bases de datos de disponibilidad entre réplicas principales y secundarias    
-   Estado de los puntos de conexión de grupos de disponibilidad    
-   Estado de los agentes de escucha de grupos de disponibilidad    
-   Estado de concesión entre la DLL del recurso de SQL Server (en ejecución en el clúster WSFC) y la instancia de SQL Server (para obtener más información, vea [How It Works: SQL Server Always On lease timeout](/archive/blogs/psssql/how-it-works-sql-server-alwayson-lease-timeout) [Cómo funciona: tiempo de espera de concesión de Always On de SQL Server]).    
-   Eventos de error del grupo de disponibilidad  

Los síntomas siguientes deben dar lugar a una revisión del registro de errores de SQL Server:  

-   No se puede acceder a las bases de datos de disponibilidad    
-   Conmutación por error inesperada del grupo de disponibilidad    
-   Grupo de disponibilidad en estado Resolviendo de forma inesperada    
-   Grupo de disponibilidad en un estado indeterminado  
  
Para obtener más información, vea [Ver el registro de errores de SQL Server &#40;SQL Server Management Studio&#41;](~/relational-databases/performance/view-the-sql-server-error-log-sql-server-management-studio.md).  
  
