---
description: Instalar SQL Server Reporting Services
title: Instalar SQL Server Reporting Services | Microsoft Docs
ms.date: 12/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: a6c2dc6ae1a8711c3e78403b539a603c0cb36dcf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596988"
---
# <a name="install-sql-server-reporting-services"></a>Instalar SQL Server Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

La instalación de SQL Server Reporting Services implica componentes de servidor para almacenar elementos de informe, informes de representación y procesamiento de suscripción, así como otros servicios de informes. 

::: moniker range=">=sql-server-ver15"
Descargue [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) desde el Centro de descarga de Microsoft.

::: moniker-end

::: moniker range="=sql-server-2017"
Descargue [SQL Server 2017 Reporting Services](https://www.microsoft.com/download/details.aspx?id=55252) desde el Centro de descarga de Microsoft.

::: moniker-end

> [!NOTE]
> ¿Busca Power BI Report Server? Vea [Instalar Power BI Report Server](https://powerbi.microsoft.com/documentation/reportserver-install-report-server/).
> 
> ¿Actualizar o realizar una migración desde una instancia de SQL Server 2016 o una versión anterior de Reporting Services? Vea el artículo sobre la [actualización y migración de Reporting Services](upgrade-and-migrate-reporting-services.md).

## <a name="before-you-begin"></a>Antes de empezar

Antes de instalar Reporting Services, lea los [Requisitos de hardware y software para instalar SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="install-your-report-server"></a>Instalar el servidor de informes

La instalación de un servidor de informes es sencilla. Solo se necesitan unos cuantos pasos para instalar los archivos.

> [!NOTE]
> No es necesario que haya ningún servidor de motor de base de datos de SQL Server disponible en el momento de la instalación. Necesitará uno para configurar Reporting Services después de la instalación.

1. Busque la ubicación de SQLServerReportingServices.exe e inicie el programa de instalación.

2. Seleccione **Instalar Reporting Services**.

3. Elija una edición para instalar y luego seleccione **Siguiente**.

    Si quiere una edición gratuita, elija Evaluation o Developer en la lista desplegable.

    ![Ediciones Evaluation o Developer](media/install-reporting-services/report-server-install-edition-select.png)

    De lo contrario, especifique una clave de producto. [Busque la clave de producto de SQL Server Reporting Services](find-reporting-services-product-key-ssrs.md).

4. Lea y acepte los términos y condiciones de licencia y luego seleccione **Siguiente**.

5. Debe tener un motor de base de datos disponible para almacenar la base de datos del servidor de informes. Seleccione **Siguiente** para instalar solo el servidor de informes.

6. Especifique la ubicación de instalación del servidor de informes. Seleccione **Instalar** para continuar.

    > [!NOTE]
    > La ruta de acceso predeterminada es C:\Archivos de programa\Microsoft SQL Server Reporting Services.

7. Después de una instalación correcta, seleccione **Configurar el servidor de informes** para iniciar el Administrador de configuración del servidor de informes.

## <a name="configure-your-report-server"></a>Configuración del servidor de informes

Después de seleccionar **Configurar el servidor de informes** en el programa de instalación, aparece el **Administrador de configuración del servidor de informes**. Para más información, vea [Administrador de configuración del servidor de informes](reporting-services-configuration-manager-native-mode.md).

Tiene que [crear una base de datos del servidor de informes](ssrs-report-server-create-a-report-server-database.md) para completar la configuración inicial de Reporting Services. Se necesita un servidor de Base de datos de SQL Server para completar este paso.

### <a name="creating-a-database-on-a-different-server"></a>Crear una base de datos en otro servidor

Si va a crear la base de datos del servidor de informes en un servidor de base de datos de otro equipo, debe cambiar la cuenta de servicio del servidor de informes a una credencial que se reconozca en el servidor de base de datos.

De forma predeterminada, el servidor de informes usa la cuenta de servicio virtual. Si intenta crear una base de datos en otro servidor, puede recibir el siguiente error en el paso de aplicación de derechos de conexión.

`System.Data.SqlClient.SqlException (0x80131904): Windows NT user or group '(null)' not found. Check the name again.`

Para solucionar el error, puede cambiar la cuenta de servicio al servicio de red o una cuenta de dominio. El cambio de la cuenta de servicio al servicio de red aplica derechos en el contexto de la cuenta de equipo del servidor de informes.

Para más información, vea [Configurar la cuenta del servicio del servidor de informes](configure-the-report-server-service-account-ssrs-configuration-manager.md).

## <a name="windows-service"></a>Servicio de Windows

Se crea un servicio de Windows como parte de la instalación. Aparece como **SQL Server Reporting Services**. El nombre de servicio es **SQLServerReportingServices**.

## <a name="default-url-reservations"></a>Reservas de direcciones URL predeterminadas

Las reservas de direcciones URL están compuestas de un prefijo, un nombre de host, un puerto y un directorio virtual:

|Parte|Descripción|
|----------|-----------------|
|Prefijo|El prefijo predeterminado es HTTP. Si instaló anteriormente un certificado de Seguridad de la capa de transporte (TLS), conocida anteriormente como Capa de sockets seguros (SSL), el programa de instalación intenta crear reservas de direcciones URL que usen el prefijo HTTPS.|
|Nombre de host|El nombre de host predeterminado es un carácter comodín (+) seguro. Especifica que el servidor de informes acepta cualquier solicitud HTTP en el puerto designado para cualquier nombre de host que se resuelva en el equipo, incluidos `https://<computername>/reportserver`, `https://localhost/reportserver` o `https://<IPAddress>/reportserver.`.|
|Port|El puerto predeterminado es 80. Si usa cualquier puerto distinto del 80, tiene que agregarlo explícitamente a la dirección URL cuando abra el portal web en una ventana del explorador.|
|Directorio virtual|De forma predeterminada, los directorios virtuales se crean en el formato de ReportServer para el servicio web del servidor de informes y de Reports para el portal web. Para el servicio web del servidor de informes, el nombre del directorio virtual predeterminado es **reportserver**. Para el portal web, el directorio virtual predeterminado es **reports**.|

Un ejemplo de cadena de dirección URL completa podría ser el siguiente:

- `https://+:80/reportserver`, proporciona acceso al servidor de informes.

- `https://+:80/reports`, proporciona acceso al portal web.

## <a name="firewall"></a>Firewall

Si accede al servidor de informes desde un equipo remoto, querrá asegurarse de que ha configurado las reglas de firewall, si hay un firewall presente.

Tiene que abrir el puerto TCP que ha configurado para la dirección URL del servicio web y la dirección URL del portal web. De forma predeterminada, están configuradas en el puerto TCP 80.

## <a name="additional-configuration"></a>Configuración adicional

- Para configurar la integración con el servicio Power BI, de modo que pueda anclar elementos de informe a un panel de Power BI, vea [Integrar con el servicio Power BI](power-bi-report-server-integration-configuration-manager.md).

- Para configurar el correo electrónico para el procesamiento de suscripciones, vea [Configuración de correo electrónico](e-mail-settings-reporting-services-native-mode-configuration-manager.md) y [Entrega por correo electrónico en Reporting Services](../subscriptions/e-mail-delivery-in-reporting-services.md).

- Para configurar el portal web de modo que se pueda acceder a él en un equipo remoto para ver y administrar informes, vea [Configurar un firewall para el acceso al servidor de informes](../report-server/configure-a-firewall-for-report-server-access.md) y [Configurar un servidor de informes para la administración remota](../report-server/configure-a-report-server-for-remote-administration.md).

## <a name="related-information"></a>Información relacionada

Para obtener información sobre cómo instalar SQL Server Reporting Services en modo nativo, vea [Instalar el servidor de informes en modo nativo de Reporting Services](install-reporting-services-native-mode-report-server.md). 

::: moniker range="=sql-server-2016"

Para obtener información sobre cómo instalar SQL Server 2016 Reporting Services (y versiones anteriores) en modo integrado de SharePoint, vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md).

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

Con el servidor de informes instalado, empiece a crear informes e impleméntelos en el servidor de informes. Para más información sobre cómo iniciar el Generador de informes, vea [Instalar el Generador de informes](../../reporting-services/install-windows/install-report-builder.md).

Para crear informes mediante SQL Server Data Tools, vaya a [Descargar SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).