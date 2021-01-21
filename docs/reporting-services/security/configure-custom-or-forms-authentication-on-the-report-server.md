---
description: Configurar la autenticación de formularios o personalizada en el servidor de informes
title: Configurar la autenticación de formularios o personalizada en el servidor de informes | Microsoft Docs
ms.date: 04/18/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Forms authentication, configuring
- custom authentication [Reporting Services]
ms.assetid: e8601a8f-e66d-4649-8e4d-a46ca20ec7d0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: bca083e29d0d03a5a5470f58b4e004d81cd27a36
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596634"
---
# <a name="configure-custom-or-forms-authentication-on-the-report-server"></a>Configurar la autenticación de formularios o personalizada en el servidor de informes

Reporting Services proporciona una arquitectura extensible que permite conectar módulos de autenticación personalizados o basados en formularios. Podría considerar implementar una extensión de autenticación personalizada si los requisitos de implementación no incluyen la seguridad integrada de Windows o la autenticación básica. El escenario más común para utilizar la autenticación personalizada es admitir el acceso a una extranet o a Internet en una aplicación web. Reemplazar la extensión de autenticación de Windows predeterminada con una extensión de autenticación personalizada le proporciona más control sobre cómo se concede acceso a los usuarios externos al servidor de informes.  

En la práctica, implementar una extensión de autenticación personalizada requiere varios pasos que incluyen copiar los archivos de aplicación y ensamblados, modificar los archivos de configuración y realizar pruebas. Este tema se centra simplemente en la configuración de autenticación que se especifica en los archivos de configuración.  

> [!NOTE]
>  La creación de una extensión de autenticación personalizada requiere código personalizado y conocimientos sobre la seguridad de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] . Si no desea crear una extensión de autenticación personalizada, puede utilizar grupos y cuentas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Active Directory, pero tendrá que reducir en gran parte el ámbito de implementación de un servidor de informes. Para obtener más información sobre la autenticación personalizada, vea [Implementing a Security Extension](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md).

Además, si quiere usar una extensión de autenticación personalizada o de autenticación de formularios en un entorno de SQL Server Reporting Services que esté integrado con un producto de SharePoint, debe configurar el sitio de SharePoint para usar el método de autenticación que elija. Para obtener más información sobre cómo configurar la autenticación en SharePoint, vea [Ejemplos de autenticación](/previous-versions/office/sharepoint-2007-products-and-technologies/cc262069(v=office.12)) en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Developer Network (MSDN).



### <a name="to-configure-a-report-server-to-use-custom-authentication"></a>Para configurar un servidor de informes de modo que use la autenticación personalizada

1.  Abra RSReportServer.config en un editor de texto.

2.  Busque \<**Authentication**>.

3.  Copie la estructura XML siguiente:

    ```
    <Authentication>
          <AuthenticationTypes>
                 <Custom />
          </AuthenticationTypes>
          <EnableAuthPersistence>true</EnableAuthPersistence>
    </Authentication>
    ```

4.  Péguela sobre las entradas existentes para \<**Authentication**>.

     Observe que no puede utilizar **Custom** con otros tipos de autenticación.

5.  Guarde el archivo.

6.  Abra el archivo Web.config para el servidor de informes. De forma predeterminada, se encuentra en la carpeta \Archivos de programa\Microsoft SQL Server\MSRS10_50.MSSQLSERVER\ReportServer.

7.  Busque **authentication mode** y establézcalo en **Forms**.

    ```
    <authentication mode = "Forms" />
    ```

8.  Busque **identity impersonate** y establézcalo en **False**.

    ```
    <identity impersonate = "false" />  
    ```
9. Agregue la estructura del elemento **PassThroughCookies** al archivo de configuración. Para más información, vea [Configurar el portal web para pasar cookies de autenticación personalizadas](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md).
  
10. Guarde el archivo.  
  
11. Si configuró una implementación escalada, repita todos los pasos anteriores con los demás servidores de informes de la implementación.  
  
12. Reinicie el servidor de informes para borrar las sesiones que estén abiertas en ese momento.  

## <a name="see-also"></a>Consulte también

[Implementación de una extensión de seguridad](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
[Reporting Services Custom Security Sample (GitHub) (Ejemplo de seguridad personalizada de Reporting Services en GitHub)](https://github.com/Microsoft/Reporting-Services/tree/master/CustomSecuritySample)  
[Autenticación con el servidor de informes](../../reporting-services/security/authentication-with-the-report-server.md)   
[El archivo de configuración RSReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md)   
[Configurar la autenticación básica en el servidor de informes](../../reporting-services/security/configure-basic-authentication-on-the-report-server.md)   
[Configuración de la autenticación de Windows en el servidor de informes](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)  
¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).