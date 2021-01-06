---
title: Opción de configuración del servidor xp_cmdshell
description: Obtenga información sobre la opción "xp_cmdshell". Vea cómo controla si SQL Server puede ejecutar el procedimiento almacenado extendido "xp_cmdshell". Averigüe cómo activarlo.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- xp_cmdshell
ms.assetid: c147c9e1-b81d-49c8-b800-3019f4d86a13
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.date: 06/12/2020
ms.openlocfilehash: d12ea9c15f2a07fa6fdd4bc5dd5538fd1522d267
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878838"
---
# <a name="xp_cmdshell-server-configuration-option"></a>Opción de configuración del servidor xp_cmdshell

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este artículo se describe cómo habilitar la opción de configuración **xp_cmdshell** de SQL Server. Esta opción permite a los administradores del sistema controlar si el [procedimiento almacenado extendido xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md) se puede ejecutar en un sistema. De forma predeterminada, la opción **xp_cmdshell** está deshabilitada en las nuevas instalaciones.

Antes de habilitar esta opción, es importante tener en cuenta las posibles implicaciones de seguridad.

- El código recién desarrollado no debe usar el procedimiento almacenado **xp_cmdshell** y, por lo general, se debe dejar deshabilitado.
- Algunas aplicaciones heredadas requieren la habilitación de **xp_cmdshell**. Si no se pueden modificar para evitar el uso de este procedimiento almacenado, puede habilitarlo como se describe a continuación.

> [!NOTE]  
> Si debe usar **xp_cmdshell**, como procedimiento recomendado de seguridad, se aconseja habilitarlo únicamente mientras dure la tarea que lo requiere.

Si necesita habilitar **xp_cmdshell**, puede usar la [administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) o ejecutar el procedimiento almacenado del sistema **sp_configure** como se muestra en el ejemplo de código siguiente:  
  
``` sql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```  
  
## <a name="next-steps"></a>Pasos siguientes

- [procedimiento almacenado extendido xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)
- [Opciones de configuración de servidor (SQL Server)](server-configuration-options-sql-server.md)
- [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
