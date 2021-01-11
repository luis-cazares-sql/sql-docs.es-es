---
title: Exportación a Microsoft Excel (Generador de informes) | Microsoft Docs
description: En el Generador de informes, la extensión de representación en Excel representa un informe paginado en el formato Office Open XML para su uso con Microsoft Excel.
ms.date: 12/23/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 74f726fc-2167-47af-9093-1644e03ef01f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4e8b43d3e9e772b06961602bf2d8bd69aa51216e
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878858"
---
# <a name="exporting-to-microsoft-excel-report-builder-and-ssrs"></a>Exportar a Microsoft Excel (Generador de informes y SSRS)
  La extensión de representación de Excel [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] representa un informe paginado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en el formato de [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (.xlsx). Con la extensión de representación de Excel, el ancho de las columnas de Excel refleja más exactamente el ancho de las columnas de los informes.  
  
 El formato es Office Open XML. El tipo de contenido de los archivos generados por este representador es **application/vnd.openxmlformats-officedocument.spreadsheetml.sheet** , y la extensión de los archivos es .xlsx.  
  
 Puede cambiar parte de la configuración predeterminada de este representador cambiando los valores de configuración de la información del dispositivo. Para más información, vea [Excel Device Information Settings](../../reporting-services/excel-device-information-settings.md).  
  
 Vea [Exportar informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md) para obtener más información sobre cómo exportar a Excel.  
  
> [!IMPORTANT]  
>  Cuando se define un parámetro de tipo **String**, el usuario ve un cuadro de texto que admite cualquier valor. Si un parámetro de informe no está vinculado a un parámetro de consulta y los valores del parámetro se incluyen en el informe, un usuario del informe puede escribir sintaxis de expresión, un script o una dirección URL en el valor del parámetro y representar el informe en Excel. Si, posteriormente, otro usuario visualiza el informe y hace clic en el contenido del parámetro representado, el usuario podría ejecutar accidentalmente el script o el vínculo malintencionados.  
>   
>  Para reducir el riesgo de ejecución accidental de scripts malintencionados, abra los informes representados exclusivamente desde orígenes de confianza. Para obtener más información sobre cómo proteger informes, vea [Proteger informes y recursos](../../reporting-services/security/secure-reports-and-resources.md).  
  
##  <a name="excel-limitations"></a><a name="ExcelLimitations"></a> Limitaciones de Excel  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] impone limitaciones a los informes exportados debido a las funciones de Excel y sus formatos de archivo. A continuación se indican las más importantes:  
  
-   El ancho máximo de las columnas en Excel es de 255 caracteres o 1726,5 puntos. El representador no comprueba que el ancho de columna es menor que el límite.  
  
-   El número máximo de caracteres de una celda es de 32.767. Si se supera este límite, el representador muestra un mensaje de error.  
  
-   El alto máximo de las filas es de 409 puntos. Si el contenido de la fila provoca que su alto supere los 409 puntos, la celda de Excel muestra una cantidad parcial de texto de hasta 409 puntos. El resto del contenido de la celda está aún dentro de la celda (hasta el número máximo de caracteres de Excel de 32.767).

-  Dado que el alto de fila máximo es de 409 puntos, si el alto definido de la celda en el informe es algo mayor de 409 puntos, Excel divide el contenido de la celda en varias filas.
  
-   En Excel no se define un número máximo de hojas de cálculo, pero hay una serie de factores externos, como la memoria y el espacio en disco, que pueden hacer que se apliquen limitaciones.  
  
-   En los esquemas, Excel permite hasta 7 niveles de anidamiento.  
  
-   Si el elemento de informe que controla si la visualización de otro elemento va activarse o desactivarse no está en la fila o en la columna anterior o siguiente al elemento controlado, el esquema también se deshabilita.  
  
 Para obtener más información sobre las limitaciones de Excel, vea [Especificaciones y límites de Excel](https://support.microsoft.com/en-us/office/excel-specifications-and-limits-1672b34d-7043-467e-8e27-269d656771c3).  
  
### <a name="sizes-of-excel-2003-xls-files"></a>Tamaño de los archivos de Excel 2003 (.xls)  
  
> [!IMPORTANT]  
>  La extensión de representación de [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 ha quedado obsoleta. Para más información, vea [Características obsoletas de SQL Server Reporting Services en SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 Cuando los informes se exportan y guardan en Excel 2003, no se benefician de la optimización de archivos que Excel aplica automáticamente a sus archivos de libro *.xls. El mayor tamaño del archivo puede producir problemas al enviar por correo electrónico suscripciones y datos adjuntos. Para reducir el tamaño de los archivos \*.xls de los informes exportados, abra los archivos \*.xls y vuelva a guardar los libros. Al guardar de nuevo los libros se suele reducir el tamaño de los archivos entre un 40 y un 50%.  
  
> [!NOTE]  
>  En Excel 2003, en una celda de una hoja de cálculo de Excel se muestran aproximadamente 1000 caracteres, pero el resto de los caracteres hasta alcanzar el máximo permitido pueden editarse en la barra de fórmulas. Esta limitación no se aplica a los archivos actuales de Excel (.xlsx).  
  
### <a name="text-boxes-and-text"></a>Cuadros de texto y texto  
 Las siguientes limitaciones se aplican a los cuadros de texto y al texto:  
  
-   Los valores de cuadro de texto que son expresiones no se convierten en fórmulas de Excel. El valor de cada cuadro de texto se evalúa durante el procesamiento del informe. La expresión evaluada se exporta como el contenido de cada celda de Excel.  
  
-   Los cuadros de texto se presentan dentro de una celda de Excel. El tamaño de fuente, la fuente, la decoración y el estilo de fuente son los únicos formatos que se admiten en el texto individual dentro de una celda de Excel.  
  
-   El efecto de texto "Suprarrayado" no está admitido en Excel.  
  
-   Excel agrega un relleno predeterminado de aproximadamente 3,75 puntos en los lados izquierdo y derecho de las celdas. Si la configuración de relleno de un cuadro de texto es menor que 3,75 puntos, y el texto cabe muy justo en su interior, es posible que se produzca el ajuste del texto en Excel.  
  
    > [!NOTE]  
    >  Para evitar este problema, aumente el ancho del cuadro de texto en el informe.  
  
### <a name="images"></a>Imágenes  
 Las siguientes limitaciones se aplican a las imágenes:  
  
-   Las imágenes de fondo de los elementos de informe se omiten porque Excel no admite imágenes de fondo en celdas individuales.  
  
-   La extensión de presentación en Excel solamente admite la imagen de fondo del cuerpo del informe. Si se muestra una imagen de fondo del cuerpo del informe en el informe, la imagen se presenta como una imagen de fondo de la hoja de cálculo.  
  
### <a name="rectangles"></a>Rectángulos  
 La siguiente limitación se aplica a los rectángulos.  
  
-   Los rectángulos de los pies de página del informe no se exportan a Excel. Sin embargo, los rectángulos del cuerpo del informe, las celdas de Tablix, etc., se representa como un intervalo de celdas de Excel.  
  
### <a name="report-headers-and-footers"></a>Encabezados y pies de página del informe  
 Las siguientes limitaciones se aplican a los encabezados y pies de página del informe:  
  
-   Los encabezados y pies de página de Excel admiten un máximo de 256 caracteres, incluido el marcado. La extensión de presentación trunca la cadena en los 256 caracteres.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no admite márgenes en los encabezados y pies de página del informe. Cuando se exportan a Excel, estos valores de margen se configuran en cero y cualquier encabezado o pie de página que contenga varias filas de datos no podría imprimir varias filas, dependiendo de la configuración de la impresora.  
  
-   Los cuadros de texto de un encabezado o pie de página mantienen el formato, pero no su alineación, cuando se exportan a Excel. Esto se produce porque los espacios iniciales y finales se recortan cuando el informe se representa en Excel.  
  
### <a name="merging-cells"></a>Combinar celdas  
 La siguiente limitación se aplica a la combinación de celdas:  
  
-   Si se combinan celdas, el ajuste de línea no funcionará correctamente. Si hay celdas combinadas en una fila donde se ha representado un cuadro de texto con la propiedad Ajustar tamaño automáticamente, el ajuste automático de tamaño no funcionará.  
  
 El representador de Excel es principalmente un presentador de diseño. Su objetivo es replicar el diseño del informe representado tanto como sea posible en una hoja de cálculo de Excel y, por consiguiente, las celdas podrían estar combinadas en la hoja de cálculo para conservar el diseño del informe. Las celdas combinadas pueden producir problemas porque la funcionalidad de ordenación de Excel requiere que las celdas se combinen de una manera muy concreta para que la ordenación funcione correctamente. Por ejemplo, Excel requiere que los intervalos de celdas combinadas tengan el mismo tamaño para ser ordenadas.  
  
 Si es importante que los informes exportados a las hojas de cálculo de Excel puedan estar ordenados, entonces siguiente puede ayudarle a reducir el número de celdas combinadas en las hojas de cálculo de Excel, que es la causa común de las dificultades de la funcionalidad de ordenación de Excel.  
  
-   No alinear los elementos a derecha y a izquierda es la causa más común de las celdas combinadas. Asegúrese de que los bordes derechos e izquierda de todos los elementos de informe se alinean unos con otros. La alineación de los elementos y que tengan el mismo ancho resolverá el problema en la mayoría de los casos.  
  
-   Aunque alinee todos los elementos con precisión, podría encontrar en algunos casos raros en los que algunas columnas siguen estando combinadas. Esto podría deberse al redondeo y a la conversión interna de unidades cuando se representa la hoja de cálculo de Excel. En el lenguaje de definición de informes (RDL), puede especificar la posición y el tamaño de diferentes unidades de medida, como pulgadas, píxeles, centímetros y puntos. Internamente Excel utiliza los puntos. Para minimizar la conversión y la posible imprecisión del redondeo al convertir pulgadas y centímetros a puntos, considere la posibilidad de especificar todas las mediciones en puntos enteros para obtener los resultados más directos. Una pulgada es 72 puntos.  
  
### <a name="report-row-groups-and-column-groups"></a>Grupos de filas y grupos de columna de informes  
 Los informes con grupos de filas o grupos de columnas contienen celdas vacías cuando se exportan a Excel. Imagine un informe que agrupa las filas según la distancia a la que se encuentran los clientes. Cada distancia puede contener más de un cliente. El informe se muestra en la siguiente imagen.  
  
 ![Informe en el portal web de Reporting Services](../../reporting-services/report-builder/media/ssrb-excelexportssrs.png "Informe en el portal web de Reporting Services")  
  
 Cuando el informe se exporta a Excel, la distancia de viaje solo aparece en una celda de la columna Commute Distance. En función de la alineación del texto en el informe (arriba, en el centro o abajo), el valor está en la celda primera, central o última. Las demás celdas están vacías. La columna Name que contiene los nombres de los clientes no tiene ninguna celda vacía. En la siguiente imagen se muestra el informe una vez exportado a Excel. Los bordes de celdas rojos se han agregado por motivos de claridad. Los cuadros grises son las celdas vacías. (Ni las líneas rojas ni los cuadros grises forman parte del informe exportado).  
  
 ![Informe exportado a Excel, con líneas](../../reporting-services/report-builder/media/ssrb-exportedexcellines.png "Informe exportado a Excel, con líneas")  
  
 Esto significa que es necesario modificar los informes con grupos de filas o grupos de columnas después de exportarlos a Excel y antes de poder mostrar los datos exportados en una tabla dinámica. Debe agregar el valor de grupo a las celdas en que faltan para que la hoja de cálculo sea una tabla plana con valores en todas las celdas. En la siguiente imagen se muestra la hoja de cálculo actualizada.  
  
 ![Informe exportado a Excel, plano](../../reporting-services/report-builder/media/ssrb-excelexportnomatrix.png "Informe exportado a Excel, plano")  
  
 Así pues, si crea un informe y va a exportarlo a Excel para realizar análisis más exhaustivos de los datos, considere la posibilidad de no agrupar las filas ni las columnas del informe.  
  
## <a name="excel-renderer"></a>Representador de Excel  
  
### <a name="current-xlsx-excel-file-renderer"></a>Representador de los archivos de Excel actuales (.xlsx)  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], el representador predeterminado de Excel es la versión compatible con los archivos actuales de [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (.xlsx). Es la opción de **Excel** que aparece en los menús de **Exportar** del portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y la lista de SharePoint.  
  
 Si va a usar el representador de Excel predeterminado, en lugar del representador de Excel 2003 (.xls) anterior, instale el Paquete de compatibilidad de Microsoft Office para Word, Excel y PowerPoint con el fin de que las versiones anteriores de Excel puedan abrir los archivos que se exportan.  
  
### <a name="excel-2003-xls-renderer"></a>Representador de Excel 2003 (.xls)  
  
> [!IMPORTANT]  
>  La extensión de representación de [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2003 ha quedado obsoleta. Para más información, vea [Características obsoletas de SQL Server Reporting Services en SQL Server 2016](../../reporting-services/deprecated-features-in-sql-server-reporting-services-ssrs.md).  
  
 La versión anterior del representador de Excel, compatible con Excel 2003, se denomina ahora Excel 2003 y aparece en los menús con ese nombre. El tipo de contenido de los archivos generados por este representador es **application/vnd.ms-excel** y la extensión del nombre de los archivos es .xls.  
  
 De forma predeterminada, la opción de menú **Excel 2003** no está visible. Los administradores pueden hacer que esté visible en ciertas circunstancias si actualizan el archivo de configuración RSReportServer. Si desea exportar informes de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] con el representador de Excel 2003, debe actualizar el archivo de configuración RSReportDesigner.  
  
 La extensión de la opción de menú **Excel 2003** nunca está visible en los escenarios siguientes:  
  
-   El Generador de informes está en modo sin conexión y se obtiene la vista previa de un informe en el Generador de informes. El archivo de configuración RSReportServer reside en el servidor de informes, por lo que las herramientas o productos desde los que exporte informes se deben conectar con un servidor de informes para leer el archivo de configuración.  
  
-   El elemento web Visor de informes está en modo local y la granja de servidores de SharePoint no está integrada en un servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para más información, vea [Informes en modo local frente al modo conectado en el Visor de informes &#40;Reporting Services en modo de SharePoint&#41;](../../reporting-services/report-server-sharepoint/local-mode-vs-connected-mode-reports-in-the-report-viewer.md)  
  
 Si el representador de la opción de menú **Excel 2003** se configura para que esté visible, las opciones de Excel y de Excel 2003 están disponibles en los escenarios siguientes:  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Portal web en modo nativo.  
  
-   Sitio de SharePoint si Reporting Services está instalado en modo integrado de SharePoint.  
  
-   [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] y obtiene una vista previa del informe.  
  
-   Generador de informes está conectado a un servidor de informes.  
  
-   El elemento web Visor de informes en modo remoto.  
  
 En el siguiente XML se muestran los elementos para las dos extensiones de representación de Excel en los archivos de configuración RSReportServer y RSReportDesigner:  
  
 `<Extension Name="EXCELOPENXML" Type="Microsoft.ReportingServices.Rendering.ExcelOpenXmlRenderer.ExcelOpenXmlRenderer,Microsoft.ReportingServices.ExcelRendering"/>`  
  
 `<Extension Name="EXCEL" Type="Microsoft.ReportingServices.Rendering.ExcelRenderer.ExcelRenderer,Microsoft.ReportingServices.ExcelRendering" Visible="false"/>`  
  
 La extensión EXCELOPENXML define el representador de Excel para los archivos actuales de Excel (.xlsx). La extensión EXCEL define la versión de Excel 2003. `Visible = "false"` indica que el representador de Excel 2003 está oculto. Para más información, vea [El archivo de configuración RsReportServer.config](../../reporting-services/report-server/rsreportserver-config-configuration-file.md) y [Archivo de configuración RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md).  
  
### <a name="differences-between-the-current-xlsx-excel-and-excel-2003-renderers"></a>Diferencias entre el representador actual de Excel (.xlsx) y el representador de Excel 2003  
 Los informes, representados con el representador actual de Excel (.xlsx) o el representador de Excel 2003, suelen ser idénticos y solo en raras ocasiones observará diferencias entre los dos formatos. En la tabla siguiente se comparan los representadores de Excel y de Excel 2003.  
  
|Propiedad|Excel 2003|Excel actual|  
|--------------|----------------|-------------------|  
|Número máximo de columnas por hoja de cálculo|256|16 384|  
|Número máximo de filas por hoja de cálculo|65 536|1,048,576|  
|Número de colores permitido en una hoja de cálculo|56 (paleta)<br /><br /> Si en el informe se usan más de 56 colores, la extensión de representación hace coincidir el color requerido con uno de los 56 colores disponibles en la paleta personalizada.|Aproximadamente 16 millones (color de 24 bits)|  
|Archivos comprimidos ZIP|None|Compresión ZIP|  
|Familia de fuentes predeterminada|Arial|Calibri|  
|Tamaño de fuente predeterminado|10 pto|11 pto|  
|Alto de fila predeterminado|12,75 pto|15 pto|  
  
 Puesto que el informe establece explícitamente el alto de fila, el alto de fila predeterminado afecta solo a las filas cuyo tamaño se configura automáticamente al exportar a Excel.  
  
##  <a name="report-items-in-excel"></a><a name="ReportItemsExcel"></a> Elementos de informe en Excel  
 Los rectángulos, los subinformes, el cuerpo de los informes y las regiones de datos se representan como un rango de celdas de Excel. Los cuadros de texto, las imágenes, los gráficos, las barras de datos, los minigráficos, los mapas, los medidores y los indicadores se deben representar dentro de una celda de Excel, que podría combinarse en función del diseño del resto del informe.  
  
 Las imágenes, gráficos, minigráficos, barras de datos, mapas, medidores, indicadores y líneas se colocan dentro de una celda de Excel, pero se sitúan encima de la cuadrícula de la celda. Las líneas se representan como bordes de las celdas.  
  
 Los gráficos, minigráficos, barras de datos, mapas, medidores e indicadores se exportan como imágenes. Los datos que describen, como el valor y las etiquetas miembros de un gráfico, no se exportan con ellos y no están disponibles en el libro de Excel a menos que se incluya en una columna o fila de una región de datos dentro de un informe.  
  
 Si desea trabajar con datos de gráficos, minigráficos, barras de datos, mapas, medidores e indicadores, exporte el informe a un archivo .csv o genere fuentes de distribución de datos conformes a Atom a partir del informe. Para más información, vea [Exportar a un archivo CSV &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md) y [Generar fuentes de distribución de datos a partir de informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).  
  
## <a name="page-sizing"></a>Tamaño de página  
 La extensión de representación en Excel usa la configuración del ancho y del alto de la página para determinar la configuración del papel en la hoja de cálculo Excel. Excel intenta hacer coincidir los valores de las propiedades PageHeight y PageWidth con uno de los tamaños de papel más comunes.  
  
 Si no encuentra ninguna coincidencia, Excel usa el tamaño de página predeterminado para la impresora. La orientación se establece en Vertical si el ancho de página es menor que el alto; en caso contrario, se establece en Horizontal.  
  
##  <a name="worksheet-tab-names"></a><a name="WorksheetTabNames"></a> Nombres de las pestañas de las hojas de cálculo  
 Al exportar un informe a Excel, las páginas del mismo que se crearon con saltos de página se exportan a hojas de cálculo diferentes. Si proporcionó un nombre de página inicial para el informe, cada hoja de cálculo del libro de Excel tendrá este nombre de forma predeterminada. El nombre aparece en la pestaña de la hoja de cálculo. Sin embargo, dado que cada hoja de cálculo de un libro debe tener un nombre único, un número entero a partir de 1 que se incrementa en 1 se anexa al nombre de página inicial para cada hoja de cálculo adicional. Por ejemplo, si el nombre de la página inicial es **Informe de ventas por año fiscal**, la segunda hoja de cálculo se denominaría **Informe de ventas por año fiscal1**, la tercera **Informe de ventas por año fiscal2** y así sucesivamente.  
  
 Si todas las páginas del informe creadas con saltos de página proporcionan nombres de página nuevos, cada hoja de cálculo tendrá el nombre de página asociado. Sin embargo, estos nombres de página podrían no ser únicos. Si los nombres de página no son únicos, las hojas de cálculo se denominan de la misma manera que los iniciales de las páginas. Por ejemplo, si el nombre de página de dos grupos es **Ventas de NW**, una pestaña de la hoja de cálculo tendrá el nombre **Ventas de NW** y la otra **Ventas de NW1**.  
  
 Si el informe no proporciona un nombre de página inicial ni nombres de página relacionados con los saltos de página, las pestañas de la hoja de cálculo tendrán los nombres predeterminados **Hoja1**, **Hoja2** y así sucesivamente.  
  
 Reporting Services proporciona propiedades para establecer en los informes, regiones de datos, grupos y rectángulos con el fin de ayudarle a crear informes que se puedan exportar a Excel de la manera que desee. Para más información, vea [Paginación en Reporting Services &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
##  <a name="document-properties"></a><a name="DocumentProperties"></a> Propiedades de documento  
 El presentador de Excel escribe los metadatos siguientes en el archivo de Excel.  
  
|Propiedades del elemento de informe|Descripción|  
|-------------------------------|-----------------|  
|Creado|Fecha y hora de ejecución del informe como un valor de fecha y hora de ISO.|  
|Autor|Autor del informe|  
|Descripción|Descripción del informe|  
|LastSaved|Fecha y hora de ejecución del informe como un valor de fecha y hora de ISO.|  
  
##  <a name="page-headers-and-footers"></a><a name="PageHeadersFooters"></a> Encabezados y pies de página  
 En función del valor de SimplePageHeaders en la información del dispositivo, el encabezado de página se puede representar de dos maneras: en la parte superior de cada cuadrícula de celda de la hoja de cálculo o en la propia sección de encabezado de la hoja de cálculo de Excel. De forma predeterminada, el encabezado se representa en la cuadrícula de celda en la hoja de cálculo de Excel.  
  
 El pie de página siempre se representa en la propia sección de pie de página de la hoja de cálculo de Excel, sin tener en cuenta el valor del parámetro SimplePageHeaders.  
  
 Las secciones de encabezados y pies de página de Excel admiten un máximo de 256 caracteres, incluido el marcado. Si se supera este límite, el representador de Excel quita caracteres de marcado comenzando al final de la cadena de encabezado, la cadena de pie de página o ambas cadenas; de este modo, reduce el número total de caracteres. Si se quitan todos los caracteres de marcado y la longitud todavía supera el máximo, la cadena se trunca comenzando por la derecha.  
  
### <a name="simplepageheader-settings"></a>Configuración de SimplePageHeader  
 De forma predeterminada, el parámetro SimplePageHeaders de información del dispositivo se establece en **False**; por consiguiente, los encabezados de página se representan como filas en el informe en la superficie de la hoja de cálculo de Excel. Las filas de la hoja de cálculo que contienen los encabezados se convierten en filas bloqueadas. Puede inmovilizar o movilizar el panel en Excel. Si está seleccionada la opción **Imprimir títulos** , estos encabezados se imprimen en cada página de la hoja de cálculo de manera automática.  
  
 El encabezado de página se repite en la parte superior de cada hoja de cálculo del libro, excepto en la portada del mapa del documento si la opción **Imprimir títulos** está seleccionada en la pestaña Diseño de página de Excel. Si la opción **Imprimir en la primera página** o **Imprimir en la última página** no está seleccionada en los cuadros de diálogo Editar propiedades del encabezado de informe o Propiedades de pie de informe, el encabezado no se agregará a la primera o a la última página respectivamente.  
  
 Los pies de página se representan en la sección de pie de página de Excel.  
  
 Debido a las limitaciones de Excel, los cuadros de texto son el único tipo de elemento de informe que se puede representar en la sección de encabezado y pie de página de Excel.  
  
##  <a name="interactivity"></a><a name="Interactivity"></a> Interactividad  
 En Excel se admiten algunos elementos interactivos. A continuación se describen sus comportamientos específicos.  
  
### <a name="show-and-hide"></a>Mostrar u ocultar  
 [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] tiene limitaciones en el modo de administrar los elementos de informe ocultos y visibles cuando éstos se exportan. Los grupos, filas y columnas que contienen elementos de informe que pueden mostrarse y ocultarse se representan como esquemas de Excel. Excel crea esquemas que expanden y contraen filas y columnas completas, lo que puede provocar que se colapsen elementos de informe que no se deberían contraer. Además, los símbolos de esquema de Excel pueden mostrarse desordenados con esquemas superpuestos. Para solucionar estos problemas, al usar la extensión de representación en Excel se aplican las reglas de esquema siguientes:  
  
-   El elemento de informe que se puede mostrar y ocultar y que está situado en la esquina superior izquierda se podrá seguir mostrando y ocultando en Excel. Los elementos de informe que se pueden mostrar y ocultar y que comparten un espacio vertical u horizontal con el elemento de informe que se puede mostrar y ocultar y que está situado en la esquina superior izquierda no se podrán mostrar ni ocultar en Excel.  
  
-   Para determinar si las filas o las columnas podrán contraer una región de datos, se determinan la posición del elemento de informe que controla la activación o desactivación de la visualización y la posición del elemento de informe que se muestra o se oculta. Si el elemento que controla la activación o desactivación de la visualización aparece antes que el elemento que se va a mostrar u ocultar, las filas contraerán el elemento. De lo contrario, son las columnas las que contraen el elemento. Si el elemento que controla la activación o desactivación de la visualización aparece por igual al lado y encima del área que se va a mostrar y ocultar, el elemento se representa con filas que son contraídas por filas.  
  
-   Para determinar dónde se sitúan los subtotales en el informe representado, la extensión de representación examina la primera instancia de un miembro dinámico. Si un miembro estático del mismo nivel aparece justo encima, se da por hecho que el miembro dinámico equivale a los subtotales. Los esquemas se establecen para indicar que se trata de datos de resumen. Si no hay ningún elemento estático del mismo nivel de un miembro dinámico, la primera instancia de la instancia es el subtotal.  
  
-   Debido a una limitación de Excel, los esquemas se pueden anidar 7 niveles como máximo.  
  
### <a name="document-map"></a>Mapa del documento  
 Si existe alguna etiqueta de mapa del documento en el informe, se representa un mapa del documento. El mapa del documento se representa como una portada de Excel que se inserta en la posición de la primera pestaña del libro. La hoja de cálculo se denomina **Mapa del documento**.  
  
 La propiedad DocumentMapLabel del elemento de informe o grupo determina el texto mostrado en el mapa del documento. Las etiquetas del mapa del documento aparecen en el orden en que aparecen en el informe, comenzando en la primera fila y en la primera columna. A cada celda de etiqueta de mapa del documento se le aplica una sangría equivalente al número de niveles que aparece en el informe. Para representar cada nivel de sangría se sitúa la etiqueta en la columna siguiente. Excel admite hasta 256 niveles de anidamiento de esquema.  
  
 El esquema del mapa del documento se representa como un esquema de Excel que se puede contraer. La estructura de esquema coincide con la estructura anidada del mapa del documento. El estado de expansión y contracción del esquema se inicia en el segundo nivel.  
  
 El nodo raíz del mapa es el nombre del informe, \<*reportname*>.rdl, y no es interactivo. La fuente de los vínculos del mapa del documento es Arial, 10pt.  
  
### <a name="drillthrough-links"></a>Vínculos de obtención de detalles  
 Los vínculos de obtención de detalles que aparecen en cuadros de texto se representan como hipervínculos de Excel en la celda en la que se representa el texto. Los vínculos de obtención de detalles para imágenes y gráficos se representan como hipervínculos de Excel en la imagen cuando se representa ésta. Al hacer clic, el vínculo de obtención de detalles abre el explorador predeterminado del cliente y navega hasta la vista HTML del destino.  
  
### <a name="hyperlinks"></a>Hipervínculos  
 Los hipervínculos que aparecen en cuadros de texto se representan como hipervínculos de Excel en la celda en la que se representa el texto. Los hipervínculos para imágenes y gráficos se representan como hipervínculos de Excel en la imagen cuando se representa ésta. Al hacer clic, el hipervínculo abre el explorador predeterminado del cliente y navega hasta la dirección URL de destino.  
  
### <a name="interactive-sorting"></a>Ordenación interactiva  
 Excel no admite la ordenación interactiva.  
  
### <a name="bookmarks"></a>Marcadores  
 Los vínculos de marcador que aparecen en cuadros de texto se representan como hipervínculos de Excel en la celda en la que se representa el texto. Los vínculos de marcador para imágenes y gráficos se representan como hipervínculos de Excel en la imagen cuando se representa ésta. Cuando se hace clic, el marcador va a la celda de Excel en la que se presenta el elemento de informe marcado.  
  
##  <a name="changing-reports-at-run-time"></a><a name="ConditionalFormat"></a> Cambiar informes en tiempo de ejecución  
 Si un informe se debe representar en varios formatos y no es posible crear un diseño del informe que se represente de la manera que desea en todos los formatos necesarios, podría considerar la utilización del valor del RenderFormat integrado global, para cambiar condicionalmente la apariencia del informe en tiempo de ejecución. De este modo puede ocultar o presentar los elementos de informe dependiendo del representador usado para obtener los mejores resultados en cada formato. Para obtener más información, vea [Referencias a campos globales y de usuario integrados &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).  
  
## <a name="see-also"></a>Consulte también  
 [Paginación en Reporting Services &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md)   
 [Comportamientos de la representación &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/rendering-behaviors-report-builder-and-ssrs.md)   
 [Funcionalidad interactiva para diferentes extensiones de representación de informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/interactive-functionality-different-report-rendering-extensions.md)   
 [Representar elementos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/rendering-report-items-report-builder-and-ssrs.md)   
 [Tablas, matrices y listas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)  
  
  
