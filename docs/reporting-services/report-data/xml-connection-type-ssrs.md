---
title: Tipo de conexión XML | Microsoft Docs
description: Obtenga información sobre el tipo de conexión XML para conectarse y recuperar datos de documentos XML, servicios web o código XML insertado en la consulta.
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: 5b55fff2-1b15-4156-83ef-15ad9cf9f509
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b6ca1df39a5aeed8b57de5c6eff7ab66e725062d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596536"
---
# <a name="xml-connection-type-ssrs"></a>Tipo de conexión XML (SSRS)
  Para incluir los datos de un origen de datos XML en el informe, deberá tener un conjunto de datos basado en un origen de datos de informe de tipo XML. Este tipo de origen de datos integrado se basa en la extensión de datos XML. Utilice este tipo de origen de datos para conectarse y recuperar datos de documentos XML, servicios web o XML incrustado en la consulta.  
  
 Esta extensión de datos admite parámetros y credenciales administrados independientemente de la cadena de conexión.  
  
 Utilice la información de este tema para crear un origen de datos. Para obtener instrucciones paso a paso, vea [Agregar y comprobar una conexión de datos o un origen de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="connection-string"></a><a name="Connection"></a> Cadena de conexión  
 La cadena de conexión debe ser una dirección URL que señale al servicio web, la aplicación basada en web o el documento XML disponible a través de HTTP. Los documentos XML deben tener la extensión XML. También se puede utilizar una cadena de conexión vacía si se van a incrustar datos XML en la consulta del conjunto de datos.  
  
 Los ejemplos siguientes muestran la sintaxis de cadena de conexión para un servicio web y un documento XML, respectivamente. No se admite el protocolo `file://` .  
  
|Tipo de documento XML|Ejemplo de cadena de conexión|  
|-----------------------|-------------------------------|  
|Servicio web|`https://adventure-works.com/results.aspx`|  
|Documento XML|`https://localhost/XML/Customers.xml`|  
|Documento XML incrustado|*Vacía*|  
  
 Para más información sobre ejemplos de cadenas de conexión, vea [Conexiones de datos, orígenes de datos y cadenas de conexión en el Generador de informes](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
##  <a name="credentials"></a><a name="Credentials"></a> Credenciales  
 Se necesitan credenciales para ejecutar consultas y obtener una vista previa del informe localmente y desde el servidor de informes.  
  
 Después de publicar el informe, es posible que necesite cambiar las credenciales para el origen de datos de tal forma que, cuando el informe se ejecute en el servidor de informes, los permisos para recuperar los datos sean válidos.  
  
 Desde un cliente de creación de informes, están disponibles las siguientes opciones para especificar las credenciales:  
  
-   Usuario actual de Windows (lo que se conoce también como seguridad integrada).  
  
-   No se necesitan credenciales. Si no especifica credenciales, se utilizará el acceso anónimo. Asegúrese de haber definido la cuenta de ejecución desatendida para que el servidor de informes se conecte a un origen de datos externo. La extensión de procesamiento de datos XML no pasa credenciales a la dirección URL de destino ni al servicio web; la conexión no se realizará correctamente a menos que haya definido la cuenta de ejecución desatendida. Para más información, vea [Configurar la cuenta de ejecución desatendida &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md).  
  
 Las credenciales almacenadas o solicitadas no están admitidas. Recuerde que si deshabilita la seguridad integrada de Windows, no podrá usarla para recuperar datos. Si especifica credenciales almacenadas o solicitadas, se producirá un error en tiempo de ejecución.  
  
 Para más información, consulte [Creación de cadenas de conexión de datos - Generador de informes y SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) o [Especificar información de credenciales y conexión para los orígenes de datos de informes](specify-credential-and-connection-information-for-report-data-sources.md).  
  
##  <a name="queries"></a><a name="Query"></a> Consultas  
 Una consulta especifica qué datos se van a recuperar para un conjunto de datos de informe. Las columnas del conjunto de resultados de una consulta rellenan la colección de campos de un conjunto de datos. Un informe procesa solamente el primer conjunto de resultados recuperado por una consulta.  
  
 Debe usar el diseñador de consultas basado en texto para crear la consulta. La consulta debe devolver datos XML.  
  
 Para más información sobre el diseñador de consultas basado en texto, vea [Interfaz de usuario del Diseñador de consultas basado en texto &#40;Generador de informes&#41;](../../reporting-services/report-data/text-based-query-designer-user-interface-report-builder.md).  
  
 A continuación se muestran los posibles valores de una consulta de conjunto de datos para un origen de datos de tipo XML.  
  
-   *Vacía*  
  
     Use una consulta vacía para crear un conjunto de resultados predeterminado. Para crear la consulta predeterminada se lee el origen de datos y se recorre la jerarquía de nodos XML hasta la primera colección de nodos hoja. El conjunto de resultados incluye todos los nodos con valores de texto y todos los atributos de nodo encontrados en el recorrido. Las columnas del conjunto de resultados hacen referencia a los campos del conjunto de datos.  
  
-   *Una ruta de acceso de elemento*  
  
     Especifica la secuencia de nodos que se debe utilizar al recuperar datos XML del origen de datos.  
  
-   *Un elemento de consulta XML*  
  
     Una especificación de consulta XML con los siguientes elementos opcionales:  
  
    -   **Origen de datos XML es un servicio web**  
  
         Elementos XML obligatorios:  
  
         `<Method Namespace=` *"espacio de nombres"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>`*acción soap*`</SoapAction>`  
  
         Elementos XML opcionales:  
  
         `<ElementPath>`  *ruta de acceso de elemento*  `</ElementPath>`  
  
         `<Method Namespace=` *"espacio de nombres"*  `Name="MethodName" />`  
  
         `-- or --`  
  
         `<SoapAction>` *acción soap* `</SoapAction>`  
  
    -   **Origen de datos XML es un documento XML**  
  
         Elementos XML obligatorios: None  
  
         Elementos XML opcionales:  
  
         `<ElementPath>`  *ruta de acceso de elemento*  `</ElementPath>`  
  
    -   **Origen de datos XML es un documento XML insertado**  
  
         Elementos XML obligatorios:  
  
         `<XmlData>` XML interno `</XmlData>`  
  
         Elementos XML opcionales:  
  
         `<ElementPath>`  *ruta de acceso de elemento*  `</ElementPath>`  
  
         `-- or --`  
  
         `<ElementPath IgnoreNamespaces="true">`  *ruta de acceso de elemento*  `</ElementPath>`  
  
 Para más información sobre la sintaxis de consulta, vea [Sintaxis de consulta XML para los datos de informe XML &#40;SSRS&#41;](../../reporting-services/report-data/xml-query-syntax-for-xml-report-data-ssrs.md).  
  
 Para obtener ejemplos, vea [Reporting Services: uso de XML y orígenes de datos del servicio web](/previous-versions/sql/sql-server-2005/administrator/aa964129(v=sql.90)).  
  
### <a name="requirements-for-retrieving-xml-web-service-data"></a>Requisitos para recuperar datos del servicio web XML  
 La extensión de procesamiento de datos XML no detecta el esquema de forma automática. Por tanto, debe tener alguna manera de determinar los métodos SOAP que recuperarán los datos que desea. También debe comprender el esquema de direcciones o el espacio de nombres que utiliza el servicio web para sus datos.  
  
 Para un servicio web, puede proporcionar un elemento \<**Query**> que especifique un método al que se debe llamar o una acción SOAP. Es posible dejar la consulta vacía y utilizar la consulta predeterminada si el origen de datos XML tiene una estructura jerárquica que genera los datos que se desea utilizar en el informe. Los valores y los atributos de los nodos de elemento XML recuperados cuando se ejecuta la consulta se asignan a los campos de conjunto de datos que se usan en el informe.  
  
### <a name="requirements-for-retrieving-xml-document-data"></a>Requisitos para recuperar datos de un documento XML  
 El servidor debe devolver los datos XML mediante el protocolo HTTP o los datos XML deben estar incrustados en el elemento XML **Query** . Si se hace referencia directa a un documento XML mediante el protocolo HTTP, la extensión debe ser .xml.  
  
 Debe saber cómo crear una consulta XML que recupere todos los datos que necesita. Si no especifica la ruta de un elemento, el comportamiento predeterminado para analizar un documento XML consiste en seleccionar la primera ruta disponible a una colección de nodos hoja en el documento XML. Si el documento XML incluye rutas de acceso adicionales a otras colecciones de nodos hoja relacionados, dichos nodos se omitirán a menos que se especifique una ruta de acceso en la consulta.  
  
 Puede proporcionar una ruta de acceso de elemento utilizando una sintaxis XML similar a XQuery.  
  
 Para obtener más información, consulte [Sintaxis de ruta de acceso de elemento para datos de informe XML (SSRS)](../../reporting-services/report-data/element-path-syntax-for-xml-report-data-ssrs.md).  
  
##  <a name="parameters"></a><a name="Parameters"></a> Parámetros  
 La consulta no se analiza para identificar los parámetros.  
  
 Para agregar parámetros, deberá crearlos manualmente en el cuadro de diálogo **Propiedades del conjunto de datos** de la página [Parámetro](/previous-versions/sql/) .  
  
##  <a name="remarks"></a><a name="Remarks"></a> Comentarios  
 La extensión de datos XML admite la creación de informes a partir de datos XML tabulares y no jerárquicos. Para más información, vea [Agregar datos de orígenes de datos externos &#40;SSRS&#41;](../../reporting-services/report-data/add-data-from-external-data-sources-ssrs.md).  
  
 No hay compatibilidad integrada para recuperar documentos XML desde una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
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
  
 [Orígenes de datos admitidos por Reporting Services (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md).  
 Proporciona información detallada sobre la compatibilidad de versiones y plataformas para cada extensión de datos.  
  
## <a name="see-also"></a>Consulte también  
 [Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Filtrar, agrupar y ordenar datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
