---
title: Registrar un nombre de entidad de seguridad de servicio (SPN) para un servidor de informes | Microsoft Docs
description: Obtenga información sobre cómo crear un SPN para el servicio del servidor de informes si se ejecuta como un usuario de dominio, si en la red se usa Kerberos para la autenticación.
ms.date: 09/24/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
ms.assetid: dda91d4f-77cc-4898-ad03-810ece5f8e74
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 38985454ae83a73fd27dac886fd0f4ee10e5ad55
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125594"
---
# <a name="register-a-service-principal-name-spn-for-a-report-server"></a>Registrar un nombre principal de servicio (SPN) para un servidor de informes
  Si está implementando [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en una red que usa el protocolo Kerberos para la autenticación mutua, debe crear un nombre principal de servicio (SPN) para el servicio Servidor de informes si lo configura para que se ejecute como una cuenta de usuario de dominio.  
  
## <a name="about-spns"></a>Acerca de los nombres principales de servicio  
 Un SPN es un identificador único para un servicio en una red que utiliza la autenticación Kerberos. Está compuesto de una clase de servicio, un nombre de host y, en ocasiones, un puerto. Los SPN de HTTP no requieren un puerto. En una red que utiliza la autenticación Kerberos, un SPN para el servidor se debe registrar en una cuenta de equipo integrada (como NetworkService o LocalSystem) o en una cuenta de usuario. Los SPN se registran automáticamente para las cuentas integradas. Sin embargo, al ejecutar un servicio en una cuenta de usuario de dominio, debe registrar manualmente el SPN para la cuenta que desea utilizar.  
  
 Para crear un SPN, puede emplear la utilidad de línea de comandos **SetSPN** . Para obtener más información, vea lo siguiente:  
  
-   [Setspn](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)) (https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)).  
  
-   [Sintaxis de nombres SetSPN de los nombres de entidad de servicio (SPN) (Setspn.exe)](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx) (https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spns-setspn-syntax-setspn-exe.aspx).  
  
 Debe ser administrador de dominio si desea ejecutar la utilidad en el controlador de dominio.  
  
## <a name="syntax"></a>Sintaxis  

Al manipular los SPN con setspn, el SPN se debe escribir en el formato correcto. El formato de un SPN HTTP es `http/host`. La sintaxis de comandos para utilizar la utilidad SetSPN con el fin de crear un SPN para el servidor de informes es similar a la siguiente:  
  
```  
Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
```  
  
 **SetSPN** está disponible con Windows Server. El argumento **-s** agrega un SPN después de validar que no existe ningún duplicado. **NOTA: -s** está disponible en Windows Server a partir de Windows Server 2008.  
  
 **HTTP** es la clase de servicio. El servicio web del servidor de informes se ejecuta en HTTP.SYS. Una consecuencia de la creación de un SPN para HTTP es que a todas las aplicaciones web del mismo equipo que se ejecutan en HTTP.SYS (incluidas las que se hospedan en IIS) se les concederán vales en función de la cuenta de usuario de dominio. Si esos servicios se ejecutan en una cuenta diferente, se producirá un error en las solicitudes de autenticación. Para evitar este problema, asegúrese de configurar todas las aplicaciones HTTP para ejecutarse en la misma cuenta, o considere la posibilidad de crear los encabezados de host para cada aplicación y crear después SPN independientes para cada encabezado de host. Al configurar los encabezados de host, se requieren cambios de DNS con independencia de la configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Los valores que especifique para \<*computername*> y \<*domainname*> identifican la dirección de red única del equipo en el que se hospeda el servidor de informes. Puede ser un nombre de host local o un nombre de dominio completo (FQDN). Si solo tiene un dominio, puede omitir \<*domainname*> de la línea de comandos. \<*domain-user-account*> es la cuenta de usuario en la que se ejecuta el servicio del servidor de informes y para la que se debe registrar el SPN.  
  
## <a name="register-an-spn-for-domain-user-account"></a>Registrar un nombre principal de servicio para la cuenta de usuario de dominio  
  
### <a name="to-register-an-spn-for-a-report-server-service-running-as-a-domain-user"></a>Para registrar un SPN para un servicio Servidor de informes que se ejecute como un usuario de dominio  
  
1.  Instale [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y configure el servicio Servidor de informes para ejecutarse como una cuenta de usuario de dominio. Observe que los usuarios no podrán conectarse al servidor de informes hasta que complete los pasos siguientes.  
  
2.  Inicie sesión en el controlador de dominio como administrador de dominio.  
  
3.  Abra una ventana de símbolo del sistema.  
  
4.  Copie el comando siguiente, reemplazando los valores de los marcadores de posición con valores reales que sean válidos para su red:  
  
    ```  
    Setspn -s http/<computer-name>.<domain-name> <domain-user-account>  
    ```  
  
    Por ejemplo: `Setspn -s http/MyReportServer.MyDomain.com MyDomainUser`  
  
5.  Ejecute el comando.  
  
6.  Abra el archivo **RsReportServer.config** y busque la sección `<AuthenticationTypes>` .  
  
7.  Agregue `<RSWindowsNegotiate />` como primera entrada en esta sección para habilitar Kerberos.  
  
## <a name="see-also"></a>Consulte también  
 [Configurar una cuenta de servicio &#40;Administrador de configuración del servidor de informes&#41;](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Configurar la cuenta de servicio del servidor de informes &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)   
 [Administración de un servidor de informes en modo nativo de Reporting Services](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)  
  
