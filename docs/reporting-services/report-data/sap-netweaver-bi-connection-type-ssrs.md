---
title: Tipo de conexión de SAP NetWeaver BI | Microsoft Docs
description: Use la información de este artículo sobre el tipo de conexión SAP NetWeaver BI para obtener información sobre cómo crear un origen de datos.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: f985856b-31d5-4e56-844b-8a8ee38da67e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b376fddd69a44f9280d4c0aaec4f58880d9ff76
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596494"
---
# <a name="sap-netweaver-bi-connection-type-ssrs"></a>Tipo de conexión de SAP NetWeaver BI (SSRS)
  Para incluir en un informe datos de un origen de datos de SAP NetWeaver® Business Intelligence externo, debe tener un conjunto de datos basado en un origen de datos de informe de tipo [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)]. Este tipo de origen de datos integrado está basado en la extensión de datos del proveedor de datos [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework 1.0 para [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)].  
  
 Esta extensión de datos permite recuperar datos multidimensionales de InfoCubes, MultiProviders (InfoCubes virtual) y consultas habilitadas para web definidas en un origen de datos [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] externo.  
  
 Utilice la información de este tema para crear un origen de datos. Para obtener instrucciones paso a paso, vea [Agregar y comprobar una conexión de datos o un origen de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Cadena de conexión  
 Póngase en contacto con el administrador de la base de datos y solicite la información de conexión y las credenciales que debe usar para conectar con el origen de datos. En el siguiente ejemplo de cadena de conexión se especifica un origen de datos de [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] en un servidor que usa el puerto 8000 y XML para Analysis Services (XMLA) en Internet con el protocolo SOAP:  
  
```  
DataSource=https://mySAPNetWeaverBIServer:8000/sap/bw/xml/soap/xmla  
```  
  
 Para más ejemplos de cadenas de conexión, vea [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenciales  
 Se necesitan credenciales para ejecutar consultas y obtener una vista previa del informe localmente y desde el servidor de informes.  
  
 Después de publicar el informe, es posible que necesite cambiar las credenciales para el origen de datos de tal forma que, cuando el informe se ejecute en el servidor de informes, los permisos para recuperar los datos sean válidos.  
  
 Para más información, consulte [Especificar información de credenciales y conexión para los orígenes de datos de informes](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="queries"></a><a name="Query"></a> Consultas  
 Se puede utilizar el diseñador gráfico de consultas en modo de diseño o de consulta para crear consultas de expresiones multidimensionales (MDX) examinando las estructuras de datos subyacentes en el origen de datos. En tiempo de diseño, para ver los resultados, se pueden ejecutar consultas desde el diseñador de consultas de forma interactiva. La consulta creada define los campos del conjunto de datos. En tiempo de ejecución, se devuelven los datos reales desde el origen de datos. Utilice el diseñador gráfico de consultas para realizar las siguientes acciones:  
  
-   En modo de diseño, arrastrar dimensiones, miembros, propiedades de miembros y cifras clave desde el origen de datos hasta un panel de datos para crear una consulta MDX. Para definir más campos de conjunto de datos, se pueden arrastrar miembros calculados desde el panel Miembros calculados hasta el panel de datos.  
  
-   En modo de consulta, arrastrar dimensiones, miembros, propiedades de miembros y cifras clave hasta el panel de consulta, o escribir texto MDX directamente en el panel de consulta. Para definir más campos de conjunto de datos, se pueden arrastrar miembros calculados desde el panel Miembros calculados hasta el panel de datos.  
  
 Mientras se generan las consultas, el diseñador de consultas agrega automáticamente propiedades predeterminadas a la consulta MDX. Para incluir propiedades distintas de las predeterminadas, debe modificar manualmente la consulta MDX.  
  
 Para más información sobre el trabajo con este diseñador de consultas, vea [Interfaz de usuario del Diseñador de consultas SAP NetWeaver BI &#40;Generador de informes&#41;](/previous-versions/sql/).  
  
  
##  <a name="extended-field-properties"></a><a name="Extended"></a> Propiedades de campo extendidas  
 El origen de datos [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] admite propiedades de campo extendidas. Las propiedades de campo extendidas son propiedades (además de **Value** e **IsMissing** ) que la extensión de procesamiento de datos define para un campo de conjunto de datos. Las propiedades extendidas incluyen propiedades predefinidas y propiedades personalizadas. Las propiedades predefinidas son propiedades comunes para varios orígenes de datos. Las propiedades personalizadas son únicas para cada origen de datos.  
  
### <a name="working-with-field-properties"></a>Trabajar con propiedades de campo  
 Las propiedades de campo extendidas no aparecen en el panel Datos de informe como elementos que se puedan arrastrar al diseño del informe. En su lugar, se arrastra al informe el campo primario de la propiedad y, después, se cambia la propiedad predeterminada de **Value** a la propiedad que se desee utilizar. Por ejemplo, si se crease el nombre de campo **Calendar Year/Month Level 01** en un diseñador de consultas MDX colocando un nivel del panel Metadatos en el panel Consulta, se usaría la sintaxis siguiente para hacer referencia a la propiedad extendida personalizada **Nombre largo** en una expresión:  
  
 `=Fields!Calendar_Year_Month_Level_01("Long Name")`  
  
 El nombre de la propiedad de campo extendida aparecerá en la información sobre herramientas al mantener el mouse encima de un campo del panel Metadatos. Para obtener más información acerca de los diseñadores de consultas que se pueden utilizar para explorar los datos subyacentes, vea [SAP NetWeaver BI Query Designer User Interface](../../reporting-services/report-data/sap-netweaver-bi-query-designer-user-interface.md).  
  
> [!NOTE]  
>  Solo existirán valores para las propiedades de campo extendidas si el origen de datos ofrece estos valores cuando el informe se ejecuta y recupera los datos de sus conjuntos de datos. En ese caso, podrá hacer referencia a esos valores de propiedad **Field** desde cualquier expresión mediante la sintaxis descrita más adelante. No obstante, dado que estos campos son específicos de este proveedor de datos y no son parte del lenguaje RDL (Report Definition Language), los cambios que se realicen en estos valores no se guardarán con la definición de informe.  
  
 Para hacer referencia a propiedades extendidas predefinidas en una expresión, se utiliza la sintaxis siguiente:  
  
-   *Fields!nombreDeCampo.nombreDePropiedad*  
  
-   *Fields!nombreDeCampo("nombreDePropiedad")*  
  
 Para hacer referencia a propiedades extendidas personalizadas en una expresión, se utiliza la sintaxis siguiente:  
  
-   *Fields!nombreDeCampo("nombreDePropiedad")*  
  
  
### <a name="predefined-field-properties"></a>Propiedades de campo predefinidas  
 En la siguiente tabla se ofrece una lista de las propiedades de campo predefinidas que se pueden usar para un origen de datos [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] .  
  
|**Property**|**Type**|**Descripción o valor esperado**|  
|------------------|--------------|---------------------------------------|  
|**Valor**|**Object**|Especifica el valor de los datos del campo.|  
|**IsMissing**|**Boolean**|Indica si se ha encontrado el campo en el conjunto de datos resultante.|  
|**FormattedValue**|**String**|Devuelve un valor con formato para una cifra clave.|  
|**BackgroundColor**|**String**|Devuelve el color de fondo del campo, definido en la base de datos.|  
|**Color**|**String**|Devuelve el color de primer plano del elemento, definido en la base de datos.|  
|**Clave**|**Object**|Devuelve la clave de un nivel.|  
|**LevelNumber**|**Entero**|En jerarquías de elementos primarios y secundarios, devuelve el número de nivel o dimensión.|  
|**ParentUniqueName**|**String**|En jerarquías de elementos primarios y secundarios, devuelve el nombre completo del nivel primario.|  
|**UniqueName**|**String**|Devuelve el nombre completo de un nivel. Por ejemplo, el valor **UniqueName** de un empleado podría ser *[0D_Company].[10D_Department].[11]* .|  
  
 Para más información sobre el uso de campos y propiedades de campo en una expresión, vea [Colecciones integradas en expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md).  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> Comentarios  
 Este proveedor de datos no admite todos los modos de entrega de informes. No se admite la entrega de informes a través de suscripciones controladas por datos para esta extensión de procesamiento de datos. Para más información, vea [Usar un origen de datos externo para obtener información de los suscriptores &#40;suscripción controlada por datos&#41;](../../reporting-services/subscriptions/use-an-external-data-source-for-subscriber-data-data-driven-subscription.md).  
  
 Para obtener más información, vea el tema sobre cómo [usar SQL Server 2008 Reporting Services con SAP NetWeaver Business Intelligence](/previous-versions/sql/sql-server-2008/cc974473(v=sql.100)).  
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Temas de procedimientos  
 Esta sección contiene instrucciones paso a paso para trabajar con conexiones de datos, orígenes de datos y conjuntos de datos.  
  
 [Agregar y comprobar una conexión de datos o un origen de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [Crear un conjunto de datos compartido o un conjunto de datos incrustado &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [Agregar un filtro a un conjunto de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="related-sections"></a><a name="Related"></a> Secciones relacionadas  
 Estas secciones de la documentación proporcionan información conceptual detallada sobre los datos de informe, así como información de procedimientos acerca de cómo definir, personalizar y usar las partes de un informe que están relacionadas con datos.  
  
 [Conjuntos de datos de informe &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Proporciona información general sobre cómo obtener acceso a los datos del informe.  
  
 [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 Proporciona información sobre las conexiones de datos y los orígenes de datos.  
  
 [Conjuntos de datos incrustados y compartidos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Proporciona información sobre conjuntos de datos compartidos e incrustados.  
  
 [Colección Campos del conjunto de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Proporciona información sobre la colección de campos de conjunto de datos que genera la consulta.  
  
 [Orígenes de datos admitidos por Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 Proporciona información detallada sobre la compatibilidad de versiones y plataformas para cada extensión de datos.  
  
  
## <a name="see-also"></a>Consulte también  
 [Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrar, agrupar y ordenar datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
