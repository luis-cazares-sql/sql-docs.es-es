---
title: Planeación del diseño y la implementación de informes | Reporting Services | Microsoft Docs
description: Obtenga información sobre cómo planear un entorno de servidor de informes y creación de informes que operen de forma conjunta en Reporting Services.
ms.date: 09/12/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1c1e265e-52a2-4de3-96fd-ca4abae01c02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ec946e008990e9cc4f75fc6a6eb0882756d25b8b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596566"
---
# <a name="plan-for-report-design-and-report-deployment--reporting-services"></a>Planeación del diseño y la implementación de informes | Reporting Services
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] proporciona varios enfoques para la creación e implementación de informes paginados. Sepa cómo planear un entorno de servidor de informes y creación de informes que operen en conjunto.

En este tema se proporciona información general sobre la compatibilidad de definición de informe por componentes de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . Una definición de informe es un archivo XML que se escribe en lenguaje RDL (Report Definition Language) o RDLC (Report Definition Language for Clients). Cada definición de informe cumple una versión de esquema específica que aparece al comienzo del archivo.  
  
 Los archivos RDL se crean en el Diseñador de informes en los proyectos de [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] y en el Generador de informes. Los archivos RDLC se crean mediante los controles ReportViewer que se incluyen en [!INCLUDE[vsprvs](../includes/vsprvs-md.md)].
  
##  <a name="rdl-schema-versions"></a><a name="bkmk_rdl_schema_versions"></a> Versiones de esquema RDL  
 En la tabla siguiente se enumera cada versión de esquema disponible y la abreviatura que se usa de aquí en adelante:  
  
|Abreviatura|Versión de esquema|  
|------------------|--------------------|  
|RDL 2016|`https://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition`|
|RDL 2010|`https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`|  
|RDL 2008|`https://schemas.microsoft.com/sqlserver/reporting/2008/01/reportdefinition`|  
|RDL 2005<br /><br /> RDLC 2005|`https://schemas.microsoft.com/sqlserver/reporting/2005/01/reportdefinition`|  
|RDL 2000|`https://schemas.microsoft.com/sqlserver/reporting/2003/10/reportdefinition`|  
  
 Para obtener más información acerca de RDL y los esquemas RDL, vea lo siguiente:  
  
-   [Esquemas XML de Microsoft SQL Server](https://go.microsoft.com/fwlink/?LinkId=31850)  
  
-   [Especificación RDL de SQL Server](/openspecs/sql_server_protocols/ms-rdl/53287204-7cd0-4bc9-a5cd-d42a5925dca1)  
  
-   [Report Definition Language &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
 Para más información sobre los controles ReportViewer, vea [Controles ReportViewer (Visual Studio)](/previous-versions/ms251671(v=vs.140)).  
  
##  <a name="report-server-and-rdl-schema-support"></a><a name="bkmk_report_server_rdl_schema_support"></a> Compatibilidad del servidor de informes y el esquema RDL  
 Un archivo de definición de informe se puede implementar en un servidor de informes de [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] de las maneras siguientes:  
  
-   **Diseñador de informes:** implemente un informe desde el Diseñador de informes de [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)].  
  
-   **Report Builder:** guarde un informe en el servidor de informes desde Report Builder.  
  
-   **Portal web:** cargue un informe en un servidor de informes en modo nativo desde el [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)].  

::: moniker range="=sql-server-2016"
  
-   **SharePoint:** cargue un informe en un sitio de SharePoint que esté configurado con un servidor de informes en modo de SharePoint.  

::: moniker-end
  
-   **Mediante programación:** publique un informe mediante programación usando las interfaces de API SOAP en un servidor de informes. Para obtener más información, vea [Report Server Web Service](../reporting-services/report-server-web-service/report-server-web-service.md).  
  
 En la tabla siguiente se indica la versión de esquema rdl admitida según la versión del servidor de informes.  
  
|Versión del servidor de informes|Versión de esquema RDL|  
|---------------------------|------------------------|  
|SQL Server 2016|RDL 2016<br /><br />RDL 2010<br /><br /> RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|RDL 2010<br /><br /> RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|RDL 2008<br /><br /> RDL 2005<br /><br /> RDL 2000|  
  
 Cuando se carga una definición de informe en el servidor de informes o se actualiza un servidor de informes que contiene informes, el servidor de informes conserva la definición de informe en el formato original. **Cuando se usa por primera vez**, el servidor de informes actualiza el informe en la base de datos del servidor de informes a un formato binario que se conserva para vistas posteriores. La definición de informe (.rdl) propiamente dicha no se actualiza.  
  
 Puede extraer del servidor de informes una copia de solo lectura del archivo de definición de informe (.rdl). En un servidor de informes en modo nativo, vaya al [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], seleccione el informe y haga clic en **Descargar**. 

::: moniker range="=sql-server-2016"

En una implementación en modo de SharePoint, vaya a la biblioteca de documentos, seleccione el informe y haga clic en **Descargar una copia**.  

::: moniker-end
  
 Para actualizar la definición de informe, se debe abrir el informe en un entorno de creación de informes, como SQL Server Data Tools o Generador de informes, y luego guardarlo.  
  
 Para más información sobre actualizaciones de informes y las versiones de esquema que se admiten, vea [Actualizar informes](../reporting-services/install-windows/upgrade-reports.md).  
  
##  <a name="report-authoring-and-deployment-support"></a><a name="bkmk_report_authoring_and_deployment"></a> Compatibilidad de creación e implementación de informes  
 Los entornos de creación de informes son el Diseñador de informes en los proyectos de [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] y el Generador de informes. Los entornos de creación de informes proporcionan una gran variedad de características de compatibilidad para la actualización de informes, el diseño de informes, la presentación de vistas previas de informes en modo local, la presentación de vistas previas de informes en el servidor de informes y la implementación de informes.  
  
 En la tabla siguiente se muestra un resumen de características de compatibilidad para la creación e implementación de definiciones de informe para diferentes versiones de esquema:  
  
|Entorno de creación|Versión RDL creada|Versión RDL de implementación|Versiones de implementación en el servidor de informes|  
|---------------------------|--------------------------|------------------------|--------------------------------------|  
|Generador de informes de SQL Server 2016|Crea RDL 2016<br /><br /> Actualizará las versiones anteriores de RDL a RDL 2016|RDL 2016|SQL Server 2016|
|Diseñador de informes en SQL Server 2016 Data Tools - Business Intelligence para Microsoft Visual Studio 2015|Crea RDL 2016<br /><br /> Actualizará las versiones anteriores de RDL a RDL 2016|RDL 2016|SQL Server 2016|
|Diseñador de informes en SQL Server 2014 Data Tools - Business Intelligence para Microsoft Visual Studio 2012<br /><br /> Or<br /><br /> Diseñador de informes en SQL Server 2012 Data Tools - Business Intelligence para Microsoft Visual Studio 2012<br /><br /> Or<br /><br /> Diseñador de informes de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Data Tools, incluido en [!INCLUDE[ssSQL11](../includes/sssql11-md.md)].|Crea RDL 2010<br /><br /> Actualizará las versiones anteriores de RDL a RDL 2010|RDL 2010|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Diseñador de informes de [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] Business Intelligence Development Studio|Crea RDL 2010<br /><br /> Actualizará las versiones anteriores de RDL a RDL 2010|RDL 2010|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|Diseñador de informes de [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] Business Intelligence Development Studio|Crea RDL 2008<br /><br /> Actualizará las versiones anteriores de RDL a RDL 2008|RDL 2008|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|
  
 Para más información sobre SQL Server Data Tools (SSDT), consulte lo siguiente:  
  
-   [Implementación y compatibilidad de versiones en las herramientas de datos de SQL Server &#40;SSRS&#41;](../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  
  
-   [SQL Server Data Tools para Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md).  
  
##  <a name="reportviewer-controls"></a><a name="bkmk_reportviewer"></a> Controles ReportViewer  
 Un control ReportViewer de [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] puede mostrar un informe .rdlc en el modo de vista previa local o, en modo remoto, el control puede mostrar un archivo .rdl hospedado en un servidor de informes de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] . En la tabla siguiente se muestra la lista de versiones RDL que admiten los controles ReportViewer para el procesamiento local (.rdlc). La compatibilidad con el lenguaje RDL de servidor se resume en la sección [Compatibilidad del servidor de informes y el esquema RDL](#bkmk_report_server_rdl_schema_support).  
  
|Control ReportViewer del producto|Versión de RDL para la vista previa local|  
|-------------------------------------|--------------------------------------|  
|[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2015 <br/><br/>Or<br/><br/>[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2013<br /><br /> Or<br /><br /> [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2012<br /><br /> Or<br /><br /> [!INCLUDE[vs_dev10_long](../includes/vs-dev10-long-md.md)]|RDL 2008|  
|[!INCLUDE[vsprvslong](../includes/vsprvslong-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[vsOrcas](../includes/vsorcas-md.md)]|RDL 2005|  
  
 Para obtener más información, vea lo siguiente:  
  
-   [Convertir archivos RDLC en archivos RDL](/previous-versions/ms252109(v=vs.140))  
  
-   [Controles ReportViewer (Visual Studio)](/previous-versions/ms251671(v=vs.140))  
  
-   [Agregar y configurar los controles ReportViewer](/previous-versions/ms252104(v=vs.140))  
  
## <a name="see-also"></a>Consulte también  
 [Informes, elementos de informe y definiciones de informe &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Herramientas de Reporting Services](../reporting-services/tools/reporting-services-tools.md)   
 [Report Definition Language &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
