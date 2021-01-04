---
title: Contadores de rendimiento - Servicio ReportServer, objetos de rendimiento | Microsoft Docs
description: Obtenga información sobre los contadores de rendimiento de los objetos de rendimiento ReportServer:Service y ReportServerSharePoint:Service, que forman parte de una implementación de SQL Server 2012.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- Report Server service, performance counters
ms.assetid: 2bcacab2-3a4f-4aae-b123-19d756b9b9ed
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 8224c126d9f23cb5f13c517400ec79e04d127ff5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466616"
---
# <a name="performance-counters---reportserver-service--performance-objects"></a>Contadores de rendimiento - Servicio ReportServer, objetos de rendimiento
  En este tema se describen los contadores de rendimiento de los objetos de rendimiento **ReportServer:Service** y **ReportServerSharePoint:Service** que forman parte de una implementación de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] .  
  
> [!NOTE]  
>  Los objetos de rendimiento se utilizan para supervisar eventos en el servidor de informes local. Si ejecuta un servidor de informes en una implementación escalada, los recuentos se aplican al servidor actual y no a la implementación escalada completa.  
  
 Los objetos de rendimiento están disponibles en el Monitor de rendimiento de Windows (**Perfmon.exe**). Para obtener más información, consulte la documentación de Windows. [Generar perfiles en tiempo de ejecución](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx).  
  
 En este tema:  
  
-   [Contadores de rendimiento de ReportServer:Service (servidor de informes de modo nativo)](#bkmk_ReportServer)  
  
-   [ReportServerSharePoint:Service (servidor de informes en modo de SharePoint)](#bkmk_ReportServerSharePoint)  
  
-   [Usar cmdlets de PowerShell para devolver listas](#bkmk_powershell)  
  
 [!INCLUDE[applies](../../includes/applies-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
##  <a name="reportserverservice-performance-counters-native-mode-report-server"></a><a name="bkmk_ReportServer"></a> Contadores de rendimiento de ReportServer:Service (servidor de informes de modo nativo)  
 El objeto de rendimiento **ReportServer:Service** incluye una colección de contadores para realizar el seguimiento de eventos relacionados con HTTP y eventos relativos a la memoria para una instancia del servidor de informes. Este objeto de rendimiento aparece una vez para cada instancia de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el equipo; puede agregar o quitar los contadores del objeto de rendimiento para cada instancia. Los contadores de la instancia predeterminada aparecen con el formato **ReportServer:Service**. Los contadores para las instancias con nombre aparecen con el formato **ReportServer$\<**_instance_name_*_>:Service_* .  
  
 El objeto de rendimiento **ReportServer:Service** era nuevo en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y proporciona un subconjunto de contadores que se incluyeron con Internet Information Services (IIS) y [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] en versiones anteriores de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Estos nuevos contadores son específicos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]y realizan el seguimiento de los eventos relacionados con HTTP para el servidor de informes, como solicitudes, conexiones e intentos de inicio de sesión. Además, este objeto de rendimiento incluye los contadores para realizar el seguimiento de los eventos de administración de memoria.  
  
 En la tabla siguiente se enumeran los contadores que se incluyen con el objeto de rendimiento **ReportServer:Service** .  
  
 ![Contenido relacionado con PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenido relacionado con PowerShell") El siguiente script de Windows PowerShell devolverá la lista de contadores de rendimiento de CounterSetName  
  
```  
(get-counter -listset "ReportServer:Service").paths  
```  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Las conexiones activas**|El número de conexiones actualmente activas en el servidor.|  
|**Total de bytes recibidos**|Número de bytes recibidos por el servidor. Este contador cuenta los bytes sin formato recibidos en total por el Administrador de informes y el servidor de informes.|  
|**Bytes recibidos/seg.**|El número de bytes recibidos por segundo por el servidor. Este contador solamente está actualizado cuando se completa una transferencia. Esto significa que el contador permanece en 0 y a continuación, el valor aumenta después de que una transferencia haya finalizado.|  
|**Total de bytes enviados**|El número de bytes enviado desde el servidor. Este contador cuenta los bytes sin formato enviados en total por el Administrador de informes y el servidor de informes.|  
|**Bytes enviados/seg.**|El número de bytes enviado por segundo desde el servidor. Este contador solamente está actualizado cuando se completa una transferencia. Esto significa que el contador permanece en 0 y a continuación, el valor aumenta después de que una transferencia haya finalizado.|  
|**Total de errores**|El número total de errores no controlados que ocurrieron durante el procesamiento de solicitudes HTTP. Estos errores incluyen los códigos de estado HTTP en los 400s y 500s.|  
|**Errores/seg.**|El número total de errores que ocurren por segundo durante el procesamiento de solicitudes HTTP. Estos errores incluyen los códigos de estado HTTP en los 400s y 500s.|  
|**Total de intentos de inicio de sesión**|El número de intentos de inicio de sesión realizados desde los tipos de autenticación de RSWindows. Los tipos de autenticación de RSWindows incluyen RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos y RSWindowsB asic. El valor cero (0) representa la autenticación personalizada.|  
|**Intentos de inicio de sesión/s**|La tasa de intentos de inicio de sesión.|  
|**Total de inicios de sesión correctos**|El número de inicios de sesión correctos para los tipos de autenticación de RSWindows. Los tipos de autenticación de RSWindows incluyen RSWindowsNegotiate, RSWindowsNTLM, RSWindowsKerberos y RSWindowsB asic. El valor cero (0) representa la autenticación personalizada.|  
|**Inicios de sesión correctos/s**|La tasa de inicio de sesión correctos.|  
|**Estado de presión de memoria**|Uno de los números siguientes, del 1 al 5, que indican el estado de la memoria actual del servidor:<br /><br /> 1: Ninguna presión<br /><br /> 2: Baja presión<br /><br /> 3: Presión media<br /><br /> 4: Alta presión<br /><br /> 5: Presión superada|  
|**Cantidad de reducción de memoria**|El número de bytes que el servidor solicitó reducir la memoria en uso.|  
|**Notificaciones de reducción de memoria/s**|El número de notificaciones que el servidor emitió en el último segundo para reducir la memoria en uso. Este valor indica la frecuencia con la que el servidor experimenta la presión de memoria.|  
|**Solicitudes desconectadas**|El número de solicitudes que se han desconectado debido a errores en la comunicación.|  
|**Solicitudes en ejecución**|El número de solicitudes que están procesando actualmente.|  
|**Solicitudes no autorizadas**|El número de solicitudes ejecutadas incorrectamente con un código de estado 401.|  
|**Solicitudes rechazadas**|El número total de solicitudes no ejecutadas debido a que no hay suficientes recursos en el servidor para procesarlas. Este contador representa el número de solicitudes que encuentran el código de estado 503 HTTP, que indica que el servidor está ocupado.|  
|**Total de solicitudes**|El número total de solicitudes que son recibidas por el servicio del servidor de informes desde el inicio. Este contador cuenta solicitudes que se envían al Administrador de informes y solicitudes que se envían desde el Administrador de informes al servidor de informes.|  
|**Solicitudes/s**|El número de solicitudes que se procesan por segundo. Este valor representa el rendimiento actual de la aplicación.|  
|**Tareas en cola**|El número de tareas que están esperando para que un subproceso esté disponible para su proceso. Cada solicitud realizada al servidor de informes corresponde a una o más tareas. Este contador representa únicamente el número de tareas listas para su procesamiento; no incluye el número de tareas que se está ejecutando actualmente.|  
  
##  <a name="reportserversharepointservice-sharepoint-mode-report-server"></a><a name="bkmk_ReportServerSharePoint"></a> ReportServerSharePoint:Service (servidor de informes en modo de SharePoint)  
 El objeto de rendimiento **ReportServerSharePoint:Service** se agregó en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 ![Contenido relacionado con PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenido relacionado con PowerShell") El siguiente script de Windows PowerShell devolverá la lista de contadores de rendimiento de CounterSetName  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Estado de presión de memoria**||  
|**Cantidad de reducción de memoria**||  
|**Memory Shrink Notifications/Sec**||  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> Usar cmdlets de PowerShell para devolver listas  
 ![Contenido relacionado con PowerShell](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "Contenido relacionado con PowerShell") El script de Windows PowerShell siguiente devolverá la lista de contadores de rendimiento de CounterSetName "ReportServerSharePoint:Service":  
  
```  
(get-counter -listset "ReportServerSharePoint:Service").paths  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el rendimiento del servidor de informes](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [Contadores de rendimiento para los objetos de rendimiento del Servicio web de MSRS 2011 y el Servicio de Windows de MSRS 2011 &#40;modo nativo&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [Contadores de rendimiento para los objetos de rendimiento en modo SharePoint de MSRS 2011 Web Service y SharePoint de MSRS 2011 Windows Service &#40;modo SharePoint&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)  
  
