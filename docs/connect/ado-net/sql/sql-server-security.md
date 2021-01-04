---
title: Seguridad de SQL Server
description: Proporciona una introducción general a las características de seguridad de SQL Server y casos de creación de aplicaciones ADO.NET seguras dirigidas a SQL Server.
ms.date: 09/26/2019
ms.assetid: 9053724d-a1fb-4f0f-b9dc-7f6dd893e8ff
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-kaywon
ms.openlocfilehash: e70f6da7755b28c400cc80a5ee6de79a004e4e42
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97038923"
---
# <a name="sql-server-security"></a>Seguridad de SQL Server

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

SQL Server incluye muchas características que permiten crear aplicaciones de base de datos seguras.  
  
Las consideraciones comunes de seguridad, como el robo de datos o el vandalismo, se aplican independientemente de la versión de SQL Server que se use. La integridad de los datos también se debe considerar como un problema de seguridad. Si los datos no están protegidos, es posible que pierdan su valor si se permite la manipulación de datos ad hoc y los datos se modifican de forma accidental o malintencionada con valores incorrectos o se eliminan por completo. Además, a menudo se deben cumplir requisitos legales, como el almacenamiento correcto de información confidencial. El almacenamiento de algunos tipos de datos personales se prohíbe por completo, en función de las leyes que se apliquen en una determinada jurisdicción.  
  
Cada versión de SQL Server incluye diferentes características de seguridad, del mismo modo que las versiones de Windows más recientes mejoran la funcionalidad respecto a las anteriores. Es importante comprender que las características de seguridad no pueden garantizar por sí mismas una aplicación de base de datos segura. Cada aplicación de base de datos es única en sus requisitos, el entorno de ejecución, el modelo de implementación, la ubicación física y la población de usuarios. Algunas aplicaciones que son locales en ámbito pueden necesitar una seguridad mínima, mientras que otras aplicaciones locales o aplicaciones implementadas a través de Internet pueden requerir medidas de seguridad rigurosas y una supervisión y evaluación continuas.  
  
Los requisitos de seguridad de una aplicación de base de datos de SQL Server se deben tener en cuenta en el momento del diseño, no a posteriori. La evaluación de las amenazas en las primeras fases del ciclo de desarrollo le brinda la oportunidad de mitigar los posibles daños en cualquier momento en que se detecte una vulnerabilidad.  
  
Incluso si el diseño inicial de una aplicación es sólido, pueden surgir nuevas amenazas a medida que el sistema evoluciona. Al crear varias líneas de defensa en torno a la base de datos, puede minimizar los daños causados por una infracción de seguridad. La primera línea de defensa es reducir el área expuesta a ataques sin conceder más permisos de los que son absolutamente necesarios.  
  
Los temas de esta sección describen brevemente las características de seguridad de SQL Server de interés para los desarrolladores, con vínculos a temas relevantes en los Libros en pantalla de SQL Server y otros recursos que proporcionan información más detallada.  
  
## <a name="in-this-section"></a>En esta sección  
[Autenticación en SQL Server](authentication-sql-server.md)  
Describe los inicios de sesión y la autenticación en SQL Server y proporciona vínculos a recursos adicionales. 

[Autenticación con Azure Active Directory](azure-active-directory-authentication.md)  
Se describe cómo usar los modos de autenticación de Azure Active Directory compatibles para conectarse a orígenes de datos de Azure SQL con SqlClient.
  
[Escenarios de seguridad de aplicaciones en SQL Server](application-security-scenarios-sql-server.md)  
Contiene temas que describen diferentes escenarios de seguridad de aplicaciones para aplicaciones de ADO.NET y SQL Server.  
  
[Seguridad de SQL Server Express](sql-server-express-security.md)  
Describe cuestiones de seguridad relacionadas con SQL Server Express.  
  
## <a name="related-sections"></a>Secciones relacionadas  
[Centro de seguridad para el Motor de base de datos de SQL Server y Azure SQL Database](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
Describe cuestiones de seguridad relacionadas con SQL Server y Azure SQL Database.

[Consideraciones de seguridad para una instalación de SQL Server](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)  
Describe los problemas de seguridad que se deben tener en cuenta antes de instalar SQL Server.

## <a name="next-steps"></a>Pasos siguientes
- [SQL Server y ADO.NET](index.md)
