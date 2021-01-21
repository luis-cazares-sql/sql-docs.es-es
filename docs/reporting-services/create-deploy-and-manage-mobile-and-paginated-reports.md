---
title: Qué es SQL Server Reporting Services | Microsoft Docs
description: Obtenga información sobre herramientas y servicios para informes de Reporting Services móviles y paginados en local.
ms.date: 05/06/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- reports [Reporting Services]
- SSRS
- reports [Reporting Services], about reports
- Reporting Services
- SQL Server Reporting Services
ms.assetid: b8d18d3d-9db0-43e7-8286-7b46cc3a37ed
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 08f9271c7b42e7db3d42ffadc111e9b86dd38e68
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596188"
---
# <a name="what-is-sql-server-reporting-services-ssrs"></a>¿Qué es SQL Server Reporting Services (SSRS)?

[!INCLUDE [ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../includes/ssrs-appliesto-not-pbirs.md)]

¿Busca Power BI Report Server? Vea [¿Qué es Power BI Report Server?](/power-bi/report-server/get-started).

SQL Server Reporting Services (SSRS) ofrece un conjunto de servicios y herramientas locales que permiten crear, implementar y administrar informes móviles y paginados.

![SQL Server Reporting Services todos juntos](../reporting-services/media/ss-reporting-services-all-together.png "SQL Server Reporting Services todos juntos")

## <a name="create-deploy-and-manage-mobile-and-paginated-reports"></a>Crear, implementar y administrar informes paginados y móviles

La solución SSRS ofrece la información apropiada a los usuarios correctos de manera flexible. Los usuarios pueden consumir los informes mediante un explorador web, en su dispositivo móvil o por correo electrónico.

SQL Server Reporting Services ofrece un conjunto actualizado de productos:

* **Informes paginados "tradicionales"** actualizados, de modo que pueda crear informes de aspecto moderno, con herramientas actualizadas y nuevas características para crearlos.
* **Nuevos informes móviles** con un diseño con capacidad de respuesta que se adapta a diferentes dispositivos y a las distintas formas en que los sostiene.
* **Un portal web moderno** que puede ver en cualquier explorador moderno. En el nuevo portal, puede organizar y mostrar informes paginados y móviles y KPI de Reporting Services. También puede almacenar libros de Excel en el portal.

Siga leyendo para obtener más información sobre estos productos.

### <a name="whats-new-in-reporting-services"></a>Novedades de Reporting Services

Estos orígenes le mantendrán al día sobre las nuevas características de SQL Server Reporting Services.

* [Novedades de Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)
* [Blog del equipo de SQL Server Reporting Services](/archive/blogs/sqlrsteamblog/)
* Canal de YouTube The [Guy in a Cube](https://www.youtube.com/channel/UCFp1vaKzpfvoGai0vE5VJ0w)

## <a name="paginated-reports"></a>Informes paginados

![Imagen de informes paginados en una pantalla de escritorio y en un dispositivo de tableta.](../reporting-services/media/ssrs-paginated-reports.png)

Reporting Services está asociado con los informes paginados “tradicionales”, que son ideales para documentos de diseño fijo optimizados para la impresión, como los archivos PDF y Word.

Esta carga de trabajo esencial de BI sigue existiendo en la actualidad, por lo que la hemos modernizado. Ahora puede crear informes de aspecto moderno con características nuevas y actualizadas mediante el Generador de informes o el Diseñador de informes de [SQL Server Data Tools (SSDT)](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md).

* Hemos actualizado todos los estilos y paletas de colores predeterminados, de manera que puede crear informes con un nuevo estilo moderno y minimalista de forma predeterminada.
* Hemos actualizado el panel de parámetros para que pueda organizar los parámetros como quiera.
* Puede exportar a nuevos formatos, como PowerPoint. Las visualizaciones de Reporting Services en PowerPoint son activas y se pueden editar; es decir, no son capturas de pantalla.
* Puede crear una experiencia híbrida de Power BI o de Reporting Services: en lugar de volver a crear los informes locales de Reporting Services en Power BI, puede anclar elementos visuales de estos informes en los paneles de Power BI. Luego puede supervisarlo todo desde un lugar en el panel de Power BI.

## <a name="mobile-reports"></a>Informes móviles

![Imagen de informes móviles en una pantalla de escritorio y en un dispositivo de tableta.](../reporting-services/media/ssrs-mobile-reports.png)

La informática móvil ha cambiado los dispositivos con los que tenemos que trabajar, por lo que, hoy en día, las personas tienen unas necesidades diferentes a la hora de crear informes. La experiencia de los informes de diseño fijo no funciona bien cuando se introducen tabletas y teléfonos. Algo diseñado para una pantalla ancha de PC no es una experiencia óptima en una pequeña pantalla de teléfono, no porque sea más pequeña, sino porque puede tener una orientación vertical u horizontal.

Lo que necesita con estos factores de forma de pantalla claramente diferentes es un diseño con capacidad de respuesta que se adapte a estos tamaños y orientaciones de pantalla diferentes. Por este motivo, hemos agregado un nuevo tipo de informe: los informes móviles, que se basan en la tecnología Datazen, que adquirimos aproximadamente hace un año e integramos en el producto. Puede migrar los informes existentes de Datazen a Reporting Services con el [Asistente para migración de SQL Server para Datazen](https://www.microsoft.com/download/details.aspx?id=53128).

Puede crear estos informes móviles en la nueva aplicación [Publicador de informes móviles](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md) . En las [aplicaciones nativas de Power BI para dispositivos móviles](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) para Windows 10, iOS, Android y HTML5, puede acceder a los datos que tiene en Power BI, la nube o SSRS.

A medida que cree visualizaciones, el Publicador de informes móviles genera datos de ejemplo automáticamente. Esta característica permite ver el aspecto que tendrá la visualización con sus datos y qué tipo de datos funciona bien en cada visualización.

## <a name="web-portal"></a>Portal web

![Imagen del portal web en un equipo portátil.](../reporting-services/media/ssrs-web-portal.png)

Para los usuarios finales de Reporting Services en modo nativo, la entrada principal es un portal web moderno que se puede ver en la mayoría de los exploradores. En el nuevo portal puede acceder a todos los informes paginados y móviles y KPI de Reporting Services. Los KPI pueden hacer que en el explorador aparezcan métricas empresariales clave para que pueda echarles un vistazo sin tener que abrir ningún informe.

El nuevo portal web es una versión totalmente nueva del Administrador de informes. Ahora se trata de una aplicación HTML5 de una sola página y basada en estándares, para la que están optimizados los exploradores modernos: Microsoft Edge, Internet Explorer 10 y 11, Chrome, Firefox, Safari y todos los exploradores principales.

El contenido en el portal web está organizado por tipo:

* Informes paginados
* Informes móviles 
* KPI
* Libros de Excel
* conjuntos de datos compartidos
* orígenes de datos compartidos

Puede almacenarlos y administrarlos con seguridad aquí, en la jerarquía de carpetas tradicional. Etiquete sus informes como favoritos para acceder a ellos con rapidez. Los que tengan los permisos apropiados pueden administrar el contenido de SSRS.

También puede seguir programando el procesamiento de informes, obtener acceso a los informes a petición y suscribirse a informes publicados en el nuevo portal web.

Obtenga más información sobre el [portal web](../reporting-services/web-portal-ssrs-native-mode.md).

::: moniker range="=sql-server-2016"

## <a name="reporting-services-in-sharepoint-integrated-mode"></a>Reporting Services en el modo integrado de SharePoint

Puede publicar informes en Reporting Services en el modo integrado de SharePoint. Puede programar el procesamiento de informes, el acceso a informes a petición, la suscripción a informes publicados y la exportación de informes a otras aplicaciones, como Microsoft Excel. Puede crear también alertas de datos en los informes publicados en un sitio de SharePoint y recibir mensajes de correo electrónico cuando cambien los datos del informe.  

Más información sobre el [Servidor de informes de Reporting Services en el modo integrado de SharePoint](../reporting-services/report-server-sharepoint/reporting-services-report-server-sharepoint-mode.md).

::: moniker-end

## <a name="ssrsnoversion-programming-features"></a>Características de programación de[!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]

Benefíciese de las características de programación de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] para poder ampliar y personalizar la funcionalidad de informes. Use las API de SSRS para integrar o ampliar el procesamiento de datos e informes en las aplicaciones personalizadas.

Más [documentación para desarrolladores de Reporting Services](../reporting-services/reporting-services-developer-documentation.md).

## <a name="next-steps"></a>Pasos siguientes

* [Instalar Reporting Services](../reporting-services/install-windows/install-reporting-services.md)
* [Descargar las últimas herramientas de datos SQL Server](../ssdt/download-sql-server-data-tools-ssdt.md)
* [Instalación del Generador de informes](../reporting-services/install-windows/install-report-builder.md)

* ¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).