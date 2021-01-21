---
title: Representación de datos (Generador de informes) | Microsoft Docs
description: Descubra cómo usar representadores de datos para importar datos en una base de datos o en Excel, en transformaciones XSLT o en intercambio de datos o EDI en el Generador de informes.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a458fdf9-fb2a-4fee-9fbd-b38f36e91753
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4fe6c4989b70421bdf88b87e19bc9ad31d206379
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596151"
---
# <a name="rendering-data-report-builder-and-ssrs"></a>Representar datos (Generador de informes y SSRS)
  Cuando se usan representadores de diseño, como HTML, MHTML, Word, Excel, PDF o Image, los datos y su organización permanecen invariables. Cuando se exporta usando un formato de representador de datos, como Separado por comas (CSV) o XML, no se representa ningún elemento de diseño visual. Al representar el informe, CSV y XML aplican ciertas reglas al cuerpo del informe y a su contenido. Estas reglas determinan el modo en que se representan los datos en estos formatos.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Puede usar representadores de datos para:  
  
-   Importar a una base de datos. CSV es un formato común que pueden importar fácilmente muchas aplicaciones de base de datos, incluidas SQL Server y Microsoft Access.  
  
-   Importar a Excel. Use el representador de CSV para exportar datos a Excel sin el diseño visual. Una vez que los datos están en Excel, ya puede beneficiarse de las herramientas estándar de Excel, como los gráficos, las fórmulas y las tablas dinámicas.  
  
-   Las transformaciones XSLT. Se puede aplicar una transformación XSLT a la salida del representador de XML. Esta transformación del servidor es una técnica eficaz para transformar los datos a prácticamente cualquier formato.  
  
-   El intercambio de datos/EDI. Un proceso externo puede solicitar una representación CSV o XML de un informe y consumir estos datos.  
  
 Los formatos de los representadores de datos se controlan mediante un conjunto de propiedades distintas de las de los representadores de diseño. La lista siguiente es una relación de propiedades establecidas en el panel **Propiedades** que solo se aplican a los representadores de datos:  
  
-   La propiedad DataElementOutput controla si un elemento determinado está o no presente en los datos en el momento de la exportación.  
  
-   La propiedad DataElementName controla el nombre del elemento de datos. En CSV, esta propiedad controla el nombre del encabezado de columna de CSV. En XML, se convierte en el nombre del elemento XML o en el atributo del elemento.  
  
-   La propiedad DataElementStyle controla en XML si el elemento de informe se representa o no como un elemento o como un atributo.  
  
 La opción de exportación CSV guarda los datos del informe como archivos de texto simple delimitados por comas. De forma predeterminada, el archivo usa una coma (,) para delimitar los campos y las filas, pero este valor se puede configurar usando la Configuración de la información del dispositivo. El archivo resultante puede abrirse en un programa de hoja de cálculo como Office SharePoint Server o usarse como un formato de importación para otros programas. El archivo .csv se abre en un editor de texto como el Bloc de notas. Si se obtiene acceso al archivo .csv como dirección URL, este devuelve un tipo MIME de **text/csv**. Los archivos son MIME versión 1.0. Para más información sobre cómo representar el informe en el tipo de archivo CSV, vea [Exportar a un archivo CSV &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md).  
  
 El archivo XML con la opción de exportación de los datos del informe guarda el informe como un archivo XML. El esquema XML para el informe es específico del informe. La opción de exportación XML no guarda la información de diseño del informe. El XML que se genera mediante esta opción se puede importar a una base de datos, se puede utilizar como mensaje de datos XML o se puede enviar a una aplicación personalizada. Para más información sobre cómo representar el informe en el tipo de archivo XML, vea [Exportar a XML &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a>Consulte también  
 [Paginación en Reporting Services &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportamientos de la representación &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Funcionalidad interactiva para diferentes extensiones de representación de informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Representar elementos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tablas, matrices y listas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Configuración de información de dispositivos de Reporting Services](/previous-versions/sql/sql-server-2008/ms155397(v=sql.100))  
  
