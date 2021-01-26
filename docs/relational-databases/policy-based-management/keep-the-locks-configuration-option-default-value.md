---
description: Mantener el valor predeterminado de la opción de configuración Bloqueos
title: Mantenimiento del valor predeterminado de la opción de configuración Bloqueos | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: f214f05b-5f0b-4786-b2ad-b8b4b6e58d72
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ab27552c473419efd942cdd1c37f7dca4b64c5b6
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597294"
---
# <a name="keep-the-locks-configuration-option-default-value"></a>Mantener el valor predeterminado de la opción de configuración Bloqueos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Esta regla comprueba el valor de la opción de configuración locks. Esta opción determina el número máximo de bloqueos disponibles. Limita cuánta memoria usa [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] para los bloqueos. El valor predeterminado es 0, lo que habilita al [!INCLUDE[ssDE](../../includes/ssde-md.md)] para asignar y cancelar la asignación de estructuras de bloqueos de manera dinámica a partir de requisitos variables del sistema.  
  
 Si locks es distinto de cero, los trabajos por lotes se detendrán y se generará un mensaje de error "sin bloqueos", si se supera el valor especificado.  
  
## <a name="best-practices-recommendations"></a>Prácticas recomendadas  
 Utilice el procedimiento almacenado de sistema sp_configure para cambiar el valor de locks a su configuración predeterminada utilizando la instrucción siguiente:  
  
```  
EXEC sp_configure 'locks', 0;  
```  
  
## <a name="for-more-information"></a>Para obtener más información  
 [Establecer la opción de configuración del servidor Bloqueos](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md)  
  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
  
 [Artículo 271509 de Microsoft Knowledge Base](/troubleshoot/sql/performance/understand-resolve-blocking)  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar y aplicar las prácticas recomendadas usando la administración basada en directivas](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
