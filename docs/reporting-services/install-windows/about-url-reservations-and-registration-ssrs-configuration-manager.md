---
title: Registro y reservas de URL (Configuration Manager) | Microsoft Docs
description: Las direcciones URL de las aplicaciones de Reporting Services se definen como reservas de direcciones URL en HTTP.SYS.
ms.date: 01/16/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.custom: seo-lt-2019, seo-mmd-2019
ms.topic: conceptual
helpviewer_keywords:
- URL reservations
- URL registration
- Report Server service, URL reservations
ms.assetid: c2c460c3-e749-4efd-aa02-0f8a98ddbc76
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1203c1190b0c14a4d5d9c3f7218adc8d6ab5d57a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595291"
---
# <a name="about-url-reservations-and-registration--report-server-configuration-manager"></a>Acerca de las reservas y el registro de direcciones URL (Administrador de configuración del servidor de informes)
  Las direcciones URL de las aplicaciones de Reporting Services se definen como reservas de direcciones URL en HTTP.SYS. Una reserva de direcciones URL define la sintaxis de un extremo de dirección URL para una aplicación web. Las reservas de direcciones URL se definen tanto para el servicio web del servidor de informes como para el portal web al configurar las aplicaciones en el servidor de informes. Las reservas de direcciones URL se crean automáticamente al configurar direcciones URL a través del programa de instalación o de la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   El programa de instalación creará las reservas de direcciones URL utilizando los valores predeterminados. Si el programa instala la configuración predeterminada, reservará dos direcciones URL: una para el servicio web del servidor de informes y otra para el portal web. Puede utilizar la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para agregar más direcciones URL o modificar las predeterminados que el programa de instalación crea.  
  
-   La herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] creará una reserva de direcciones URL según la dirección URL que especifique en las páginas **Dirección URL del servicio web** o **Dirección URL del Administrador de informes** de la herramienta.  
  
 Tanto el programa de instalación como la herramienta también asignarán permisos en la dirección URL al servicio del servidor de informes, comprobarán si hay instancias duplicadas y agregarán la reserva de direcciones URL a HTTP.SYS. No cree ni modifique nunca una reserva de direcciones URL de Reporting Services directamente mediante HttpCfg.exe u otra herramienta. Si omite un paso o establece un valor no válido, encontrará problemas que podrían ser difíciles de diagnosticar o corregir.  
  
> [!NOTE]  
> HTTP.SYS es un componente del sistema operativo que escucha las solicitudes de red y las enruta a una cola de solicitudes. En esta versión de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], HTTP.SYS establece y mantiene la cola de solicitudes para el servicio web del servidor de informes y el portal web. Internet Information Services (IIS) ya no se utiliza para hospedar o tener acceso a aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para más información sobre la funcionalidad de HTTP.SYS, vea [HTTP Server API](/windows/win32/http/http-api-start-page).  
  
##  <a name="urls-in-reporting-services"></a><a name="ReportingServicesURLs"></a> Direcciones URL en Reporting Services  
 En una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , puede tener acceso a las herramientas, aplicaciones y elementos siguientes a través de direcciones URL:  
  
-   servicio web del servidor de informes  
  
-   Portal web  
  
-   Informes publicados en un servidor de informes  
  
 No se debería tener acceso a otros elementos publicados con direcciones URL (por ejemplo, a orígenes de datos compartidos) a través de direcciones URL como elementos independientes. El servidor de informes no muestra esos elementos en un formato significativo cuando se ven en una ventana del explorador.  
  
> [!NOTE]  
> En este artículo no se describe el acceso con direcciones URL a informes específicos que se almacenan en el servidor de informes. Para más información sobre el acceso con direcciones URL a estos elementos, vea [Acceder a elementos del servidor de informes mediante el acceso URL](../../reporting-services/access-report-server-items-using-url-access.md).  
  
##  <a name="url-reservation-and-registration"></a><a name="URLreservation"></a> Reserva y registro de direcciones URL  
 Una reserva de direcciones URL define las direcciones URL que se pueden usar para tener acceso a una aplicación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] reservará una o más direcciones URL para el servicio web del servidor de informes y para el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] en HTTP.SYS, y las registrará cuando se inicie el servicio. Si anexa parámetros a la dirección URL, puede abrir los informes a través del servicio web. HTTP.SYS proporciona las reservas y permite el registro. Para más información, vea [Namespace Reservations, Registration, and Routing](/windows/win32/http/namespace-reservations-registrations-and-routing).  
  
 La *reserva de direcciones URL* es el proceso por el que se crea un extremo de dirección URL para una aplicación web y se almacena en HTTP.SYS. HTTP.SYS es el repositorio común de todas las reservas de direcciones URL que se definen en un equipo y define un conjunto de reglas comunes que garantizan que las reservas sean únicas.  
  
 El *registro de direcciones URL* se produce cuando el servicio se inicia. Se crea la cola de solicitudes y HTTP.SYS empieza a enrutar las solicitudes a esa cola. Un extremo de la dirección URL se debe registrar antes de agregar a la cola las solicitudes que se dirijan a ese extremo. Cuando el servicio Servidor de informes se inicie, registrará todas las direcciones URL que haya reservado para todas las aplicaciones habilitadas. Esto significa que el servicio web debe estar habilitado para que el registro tenga lugar. Si establece la propiedad **WebServiceAndHTTPAccessEnabled** en **False** en la faceta Configuración de área expuesta para Reporting Services de Administración basada en directivas, la dirección URL del servicio web no se registrará cuando se inicie el servicio.  
  
 Las direcciones URL se eliminan del Registro si detiene el servicio o recicla el servicio web o el dominio de aplicación del [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] . Si modifica una reserva de direcciones URL mientras el servicio se está ejecutando, el servidor de informes reciclará el dominio de aplicación inmediatamente para que se pueda eliminar del Registro la dirección URL anterior y empezar a usar la nueva.  
  
 Unos sencillos ejemplos ilustran el concepto de reserva de direcciones URL y cómo se relaciona con las direcciones URL que se usan para las aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Un punto clave que tener en cuenta es que la reserva de direcciones URL tiene una sintaxis diferente a la que la dirección URL utiliza para tener acceso a la aplicación:  
  
|Reserva de direcciones URL en HTTP.SYS|URL|Explicación|  
|---------------------------------|---------|-----------------|  
|`https://+:80/reportserver`|`https://<computername>/reportserver`<br /><br /> `https://<IPAddress>/reportserver`<br /><br /> `https://localhost/reportserver`|La reserva de direcciones URL especifica un carácter comodín (+) en el puerto 80. Esto coloca en la cola del servidor de informes cualquier solicitud entrante que especifique un host que se resuelva como el equipo del servidor de informes en el puerto 80. Observe que con esta reserva de direcciones URL se puede usar una cantidad cualquiera de direcciones URL para tener acceso al servidor de informes.<br /><br /> Ésta es la reserva de direcciones URL predeterminada para un servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para la mayoría de los sistemas operativos.|  
|`https://123.45.67.0:80/reportserver`|`https://123.45.67.0/reportserver`|Esta reserva de direcciones URL especifica una dirección IP y es mucho más restrictiva que la que tiene un carácter comodín. Solo las direcciones URL que incluyen la dirección IP se pueden utilizar para conectarse al servidor de informes. Dada esta reserva de dirección URL, se produciría un error en una solicitud a un servidor de informes en `https://<computername>/reportserver` o `https://localhost/reportserver`.|  
  
##  <a name="default-urls"></a><a name="DefaultURLs"></a> Direcciones URL predeterminadas  
 Si instala [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en la configuración predeterminada, el programa de instalación reservará direcciones URL para el servicio web del servidor de informes y el [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]. También puede aceptar estos valores predeterminados al definir las reservas de direcciones URL en la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Las direcciones URL predeterminadas incluirán un nombre de instancia si instala [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] o [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] como una instancia con nombre.  
  
> [!IMPORTANT]  
> El carácter de la instancia es el carácter de subrayado ( **_** ).  
  
 Las reservas de direcciones URL incluyen un número de puerto. Los sistemas operativos siguientes permitirán que varias aplicaciones web compartan un puerto.  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)] R2  
  
-   [!INCLUDE[win8srv](../../includes/win8srv-md.md)]  
  
-   [!INCLUDE[winserver2008r2](../../includes/winserver2008r2-md.md)]  
  
-   [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)]  
  
-   [!INCLUDE[win7](../../includes/win7-md.md)]  
  
-   [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)]  
  
|Tipo de instancia|Application|Dirección URL predeterminada|Reserva de direcciones URL real en HTTP.SYS|  
|-------------------|-----------------|-----------------|----------------------------------------|  
|Instancia predeterminada|servicio web del servidor de informes|`https://<servername>/reportserver`|`https://<servername>:80/reportserver`|  
|Instancia predeterminada|Portal web|`https://<servername>/reports`|`https://<servername>:80/reports`|  
|Instancia con nombre|servicio web del servidor de informes|`https://<servername>/reportserver_<instancename>`|`https://<servername>:80/reportserver_<instancename>`|  
|Instancia con nombre|Portal web|`https://<servername>/reports_<instancename>`|`https://<servername>:80/reports_<instancename>`|  
|SQL Server Express|servicio web del servidor de informes|`https://<servername>/reportserver_SQLExpress`|`https://<servername>:80/reportserver_SQLExpress`|  
|SQL Server Express|Portal web|`https://<servername>/reports_SQLExpress`|`https://<servername>:80/reports_SQLExpress`|  
  
##  <a name="authentication-and-service-identity-for-reporting-services-urls"></a><a name="URLPermissionsAccounts"></a> Autenticación e identidad de servicio de las direcciones URL de Reporting Services  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Las reservas de direcciones URL muestran la cuenta de la reserva. La cuenta del servicio virtual se utiliza para todas las direcciones URL que se crean para las aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que se ejecutan en la misma instancia.
  
 
 El acceso anónimo está deshabilitado porque la seguridad predeterminada es **RSWindowsNegotiate**. Para el acceso en una intranet, las direcciones URL del servidor de informes usan nombres de equipo de red. Si desea configurar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para las conexiones a Internet, debe utilizar valores diferentes. Para más información sobre la autenticación, vea [Autenticación con el servidor de informes](../../reporting-services/security/authentication-with-the-report-server.md).  
  
##  <a name="urls-for-local-administration"></a><a name="URLlocalAdmin"></a> Direcciones URL para administración local  
 Puede usar `https://localhost/reportserver` o `https://localhost/reports` si ha especificado un carácter comodín fuerte o débil para la reserva de direcciones URL.  
  
 La dirección URL `https://localhost` se interpreta como `https://127.0.0.1`. Si asoció la reserva de direcciones URL a un único nombre de equipo o dirección IP, no puede utilizar el host local a menos que cree una reserva adicional para 127.0.0.1 en el equipo local. De igual forma, si localhost o 127.0.0.1 se deshabilitan en el equipo, no puede utilizar esa dirección URL.  
  
 [!INCLUDE[wiprlhlong](../../includes/wiprlhlong-md.md)], [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] y versiones posteriores incluyen nuevas características de seguridad para reducir el riesgo de ejecutar accidentalmente programas con privilegios elevados. Se necesitan pasos adicionales para habilitar la administración local en estos sistemas operativos. Para más información, vea [Configurar un servidor de informes en modo nativo para la administración local &#40;SSRS&#41;](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md).  
  
## <a name="see-also"></a>Consulte también  
 [Configuración de una URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)   
 [Sintaxis de reserva de URL &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/url-reservation-syntax-ssrs-configuration-manager.md)  

