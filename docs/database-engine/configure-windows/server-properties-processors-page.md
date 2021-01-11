---
title: Propiedades del servidor (página de procesadores)
description: Familiarícese con la configuración de procesador en SQL Server. Obtenga información sobre qué opciones controlan el número de subprocesos de trabajo, la asignación del procesador y otras propiedades.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684217"
---
# <a name="server-properties-processors-page"></a>Propiedades del servidor (página Procesadores)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilice esta página para ver o modificar las opciones del procesador. Los valores de configuración de la afinidad del procesador solo se habilitan si hay más de un procesador instalado.  

## <a name="options"></a>Opciones

### <a name="processor-affinity"></a>Afinidad del procesador
Asigna procesadores a subprocesos específicos para eliminar las recargas de procesador y reducir la migración de subprocesos entre los procesadores. Para obtener más información, vea [affinity mask (opción de configuración del servidor)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

### <a name="io-affinity"></a>Afinidad de E/S
Enlaza las E/S de disco de [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server a un subconjunto especificado de CPU. Para obtener más información, vea [affinity I/O mask (opción de configuración del servidor)](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>Establecer automáticamente máscara de afinidad de procesador para todos los procesadores
Permite a SQL Server establecer la afinidad de los procesadores.

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>Establecer automáticamente máscara de afinidad de E/S para todos los procesadores
Permite a SQL Server establecer la afinidad de E/S.

### <a name="maximum-worker-threads"></a>Número máximo de subprocesos de trabajo
0 permite a SQL Server establecer de forma dinámica el número de subprocesos de trabajo. Este valor es el más adecuado para la mayor parte de los sistemas. No obstante, según la configuración del sistema, definir esta opción en un valor específico puede mejorar, a veces, el rendimiento. Para más información, consulte [Establecer la opción de configuración del servidor Máximo de subprocesos de trabajo](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  

### <a name="boost-sql-server-priority"></a>Aumentar la prioridad de SQL Server
Especifica si SQL Server debe ejecutarse con una prioridad de programación de Microsoft Windows más alta en lugar de otros procesos en el mismo equipo. Para más información, consulte [Establecer la opción de configuración del servidor Aumento de prioridad](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  

> [!Note]
> Esta opción no está disponible con SSMS 18.x y versiones posteriores.

### <a name="use-windows-fibers-lightweight-pooling"></a>Usar fibras de Windows (agrupación ligera)
Utilice fibras de Windows en lugar de subprocesos para el servicio SQL Server. Esta opción no está disponible en Windows 2003 Server Edition. Para obtener más información, consulte [lightweight pooling (opción de configuración del servidor)](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

> [!Note]
> Esta opción no está disponible con SSMS 18.x y versiones posteriores.

### <a name="configured-values"></a>Valores configurados
Muestra los valores configurados para las opciones de este panel. Si cambia estos valores, seleccione **Valores actuales** para comprobar si los cambios han surtido efecto. Si no han surtido efecto, deberá reiniciarse primero la instancia de SQL Server.

### <a name="running-values"></a>Valores actuales
Presenta los valores actuales de las opciones de este panel. Estos valores son de solo lectura.

## <a name="see-also"></a>Consulte también
[Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


