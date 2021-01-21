---
title: Asistente para mapas y Asistente para capas de mapa (Generador de informes) | Microsoft Docs
description: Obtenga información sobre cómo automatizar la creación de un mapa, la incorporación de una capa de mapa o la modificación de las opciones de capas de mapa con Asistente para mapas o el Asistente para capas de mapa del Generador de informes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: reference
f1_keywords:
- sql13.rtp.rptdesigner.mapandlayerwizard.f1
- "10542"
- MICROSOFT.REPORTDESIGNER.MAPLAYER.NAME
ms.assetid: 48cbe18b-1290-4107-8a1c-ec6acd71f73b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b307966577cfd30197963263597aee7a9d06e018
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597336"
---
# <a name="map-wizard-and-map-layer-wizard-report-builder-and-ssrs"></a>Asistente para mapas y Asistente para capas de mapa (Generador de informes y SSRS)
 En los informes paginados de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , el Asistente para mapas y el Asistente para capas de mapa automatizan las tareas de crear un mapa, agregar una capa de mapa o cambiar las opciones de capas de mapa en una capa existente.  
  
 Antes de agregar un mapa a un informe o una capa de mapa a un mapa, necesita recopilar esta información:  
  
-   **Origen de datos espaciales.** La ubicación o conexión a un origen que proporcione los datos espaciales, como el nombre de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y una base de datos que contenga los datos espaciales, o bien el nombre de un archivo de forma de Environmental Systems Research Institute, Inc. (ESRI).  
  
-   **Spatial data.** Del origen de datos espaciales, un campo que contenga los conjuntos de coordenadas que especifican las ubicaciones.  
  
-   **Datos analíticos.** Los datos analíticos que se usan para variar la presentación del mapa, por ejemplo, el almacén de ventas anuales.  
  
-   **Campos coincidentes.** Definen la relación entre los datos espaciales y los datos analíticos, por ejemplo, el nombre de una región y una ciudad para identificar cada ciudad de forma única.  
  
 En las secciones siguientes se proporciona información sobre las opciones que se especifican en el Asistente para capas de mapa y en el Asistente para mapas.  
  
-   Al abrir el Generador de informes, haga clic en el icono del asistente **Mapas** , en el centro de la superficie de diseño.  
  
-   En la pestaña **Insertar** , haga clic en **Mapa** y, a continuación, haga clic en **Asistente para mapas**.  
  
 Para abrir el Asistente para capas de mapa, haga lo siguiente:  
  
-   Haga clic en el mapa para mostrar el panel de mapa y, en la barra de herramientas, haga clic en el botón **Asistente para nueva capa** .  
  
 Haga clic en el título de la página del asistente correspondiente al contenido de la Ayuda. Las páginas que se pueden ver cambian en función de las opciones de tipo de mapa, el origen de datos espaciales, y el origen de datos analíticos.  
  
1.  [Elegir un origen de datos espaciales](#SpatialDataSource). Los datos espaciales pueden proceder de la galería de mapas, de un archivo de forma de Environmental Systems Research Institute, Inc. o de los datos espaciales de una base de datos relacional de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   [¿Qué son los datos espaciales?](#SpatialData)  
  
    -   [¿Qué es la Galería de mapas?](#MapGallery)  
  
    -   [¿Qué es un archivo de forma ESRI?](#Shapefile)  
  
    -   [¿Dónde se pueden obtener archivos de forma ESRI?](#GetShapefiles)  
  
    -   [¿Qué es una consulta espacial de SQL Server?](#SqlServerSpatial)  
  
2.  [Elegir los datos espaciales y las opciones de la vista del mapa](#MapView). Establezca la vista y la resolución del mapa, y especifique si los datos espaciales se incrustan en el informe y si se incluye un fondo de mosaicos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing Maps.  
  
    -   [¿Qué es la vista o ventanilla del mapa?](#Viewport)  
  
    -   [¿Qué es la resolución u optimización del mapa?](#Resolution)  
  
    -   [¿Qué hace la incrustación de datos espaciales?](#Embed)  
  
    -   [¿Qué es un fondo de mosaicos de Bing Maps?](#Tiles)  
  
3.  [Elegir la visualización de un mapa](#Visualization). Elija el tipo de mapa que desea crear.  
  
    -   [¿Cuál es la diferencia entre un mapa básico, un mapa de burbujas y un mapa analítico?](#MapType)  
  
    -   Elegir la visualización de un mapa: Polígonos  
  
    -   Elegir la visualización de un mapa: Líneas  
  
    -   Elegir la visualización de un mapa: Puntos  
  
4.  Elegir una conexión a un origen de datos. Elegir una visualización de mapa: Puntos. Elija una conexión a un origen de datos o cree una conexión a un origen de datos externo que contenga los datos analíticos que se van a mostrar en el mapa.  
  
5.  Diseñar una consulta. Diseñe una consulta que especifique los datos analíticos.  
  
6.  [Elegir el conjunto de datos analíticos](#AnalyticalData). Especifique un origen de datos para los datos analíticos.  
  
    -   [¿Cuál es la diferencia entre los datos espaciales y los datos analíticos?](#Diff)  
  
7.  [Especificar los campos coincidentes para datos espaciales y analíticos](#SpecifyMatchFields). Genere una relación entre los datos espaciales y los datos analíticos de modo que la apariencia de los elementos de mapa pueda variar en función de los datos.  
  
    -   [¿Qué es un campo coincidente?](#MatchFields)  
  
8.  [Elegir el tema de color y la visualización de los datos](#ThemeandVisualization). Para especificar cómo visualizar los datos con el fondo del mapa, especifique el tema del mapa, los campos que visualizar y qué variar: color, tamaño y/o tipo de marcador.  
  
    -   [¿Qué hace el tema?](#Theme)  
  
    -   [¿Para qué son las leyendas y las escalas de Vista previa del mapa?](#Legends)  
  
    -   [¿Qué son las reglas?](#Rules)  
  
 Después de agregar un mapa o una capa de mapa, y obtener una vista previa del informe, puede cambiar las opciones del mapa y de la capa de mapa que estableció en los asistentes. Para más información, vea [Personalizar los datos y la presentación de un mapa o una capa de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/customize-the-data-and-display-of-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
 Para más información sobre los mapas, vea [Mapas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md). Para obtener instrucciones paso a paso para agregar un mapa a un informe, vea [Tutorial: informe de asignaciones &#40;Generador de informes&#41;](../../reporting-services/tutorial-map-report-report-builder.md)  
  
##  <a name="choose-a-source-of-spatial-data"></a><a name="SpatialDataSource"></a> Elegir un origen de datos espaciales  
 En esta página, especifique el origen de datos espaciales y qué datos espaciales se incluirán. Los datos espaciales pueden proceder de la galería de mapas, un archivo de forma ESRI o una consulta del conjunto de datos que especifica los datos espaciales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de una base de datos de versión de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] o posterior.  
  
 Puede utilizar el mismo origen o un origen diferente de datos espaciales para cada capa, pero debe especificar el origen cada vez que agrega una capa. Cuando los datos espaciales proceden de la galería de mapas o de un archivo de forma ESRI, el origen de datos espaciales no es un elemento de informe independiente. No aparece en el panel Datos de informe.  
  
###  <a name="what-is-spatial-data"></a><a name="SpatialData"></a> ¿Qué son los datos espaciales?  
 Los datos espaciales contienen las coordenadas que definen elementos geográficos o geométricos. En un mapa, los datos espaciales definen los *elementos de mapa*: polígonos que definen áreas o formas, líneas que definen rutas o rutas de acceso, y puntos que definen marcadores o pins. Los datos espaciales se almacenan en formato binario en el origen de datos y se especifican como conjuntos de coordenadas. Por ejemplo, un punto es una coordenada X e Y (X Y), una línea son dos conjuntos de coordenadas ((X1 Y1), (X2 Y2)), un polígono son cuatro o más conjuntos de coordenadas donde el primer y último conjunto de coordenadas es el mismo ((X1 Y1), (X2 Y2), (X3 Y3), (X1 Y1)).  
  
 Para obtener más información, consulte la documentación del tipo de datos espaciales que utiliza.  
  
###  <a name="what-is-the-map-gallery"></a><a name="MapGallery"></a> What is the map gallery?  
 La galería de mapas contiene mapas de los informes que se encuentran en la carpeta de la galería de mapas del entorno de creación de informes. Los mapas de la galería permiten comenzar rápidamente a agregar un mapa a un informe. Un proveedor de mapas proporciona los mapas predefinidos de la galería.  
  
> [!NOTE]  
>  Esta característica de mapas de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] usa los datos de archivos de forma TIGER/Line que se proporcionan por cortesía de la Oficina del censo estadounidense ([https://www.census.gov/](https://www.census.gov/)). Los archivos de forma TIGER/Line son un extracto de información geográfica y cartográfica seleccionada de la base de datos MAF/TIGRE del Censo. Los archivos de forma TIGER/Line están disponibles sin cargo en la Oficina del censo estadounidense. Para más información acerca de los archivos de forma TIGER/Line, vaya a [TIGER/Line Shapefiles and TIGER/Line Files Technical Documentation](https://www.census.gov/programs-surveys/geography/technical-documentation/complete-technical-documentation/tiger-geo-line.html) (Documentación técnica de los archivos de forma TIGER/Line y los archivos TIGER/Line). La información de fronteras de los archivos de forma TIGER/Line está destinada únicamente para la recopilación de datos estadísticos y tabulación; su representación y designación para fines estadísticos no constituye ninguna determinación de autoridad jurisdiccional ni derechos de propiedad ni de titularidad, y no son descripciones legales de territorios. Census TIGER and TIGER/Line son marcas registradas de la Oficina del censo estadounidense.  
  
 Para extender la galería de mapas, puede agregar o quitar informes del directorio de la galería de mapas, y agregar carpetas para organizar los mapas. Para obtener más información, vea [Mapas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md).  
  
###  <a name="what-is-an-esri-shapefile"></a><a name="Shapefile"></a> What is an ESRI shapefile?  
 Un archivo de forma ESRI es un conjunto de archivos con datos que cumplen el formato de datos espaciales de los archivos de forma del Environmental Systems Research Institute, Inc. (ESRI). El conjunto de archivos suele incluir el archivo *\<filename>* .shp, que contiene datos espaciales, y un archivo auxiliar, *\<filename>* .dbf.  
  
 Al especificar un archivo de forma como origen de datos espaciales, si se encuentra en el equipo local, los datos espaciales se incrustan automáticamente en el informe. Para usar los datos espaciales de un archivo ESRI dinámicamente, debe hacer lo siguiente:  
  
 En el Generador de informes, cargue el archivo .shp y el archivo .dbf en la misma carpeta de un servidor de informes y, a continuación, vincule el archivo .shp como el origen de datos espaciales.  
  
 En el Diseñador de informes de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], agregue el archivo .shp y el archivo .dbf al proyecto de informe y, a continuación, especifique el nombre del archivo .shp como el origen de datos espaciales.  
  
###  <a name="where-can-i-get-esri-shapefiles"></a><a name="GetShapefiles"></a> ¿Dónde se pueden obtener archivos de forma ESRI?  
 Los archivos de forma ESRI están disponibles en Internet. Para obtener más información, vea [Buscar archivos de forma ESRI para un mapa](https://go.microsoft.com/fwlink/?linkid=178814).  
  
###  <a name="what-is-a-sql-server-spatial-query"></a><a name="SqlServerSpatial"></a> ¿Qué es una consulta espacial de SQL Server?  
 Una consulta espacial de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es una consulta de conjunto de datos que especifica datos de tipo SQLGeometry o SQLGeography de una base de datos relacional de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Al definir un origen de datos en el asistente, verá diseñadores de consultas diferentes en la página Diseñar una consulta, en función del tipo de origen de datos al que se haya conectado. Para más información, vea [Herramientas de diseño de consulta &#40;SSRS&#41;](../report-data/query-design-tools-ssrs.md).  
  
 Al ejecutar la consulta en el diseñador de consultas, el conjunto de resultados muestra una columna con datos espaciales que aparece como texto. Por ejemplo, una fila podría contener datos espaciales que sean un único punto y la siguiente fila podría contener datos espaciales que definan un conjunto de puntos. Cada fila se convierte en un elemento de mapa. Puede variar la presentación de cada elemento de mapa como una unidad indivisible.  
  
 Para más información, vea [Información general de los tipos de datos espaciales](../../relational-databases/spatial/spatial-data-types-overview.md).  
  
##  <a name="choose-spatial-data-and-map-view-options"></a><a name="MapView"></a> Elegir los datos espaciales y las opciones de la vista del mapa  
 En esta página puede establecer las siguientes opciones:  
  
-   Establecer el centro de la vista y el nivel de zoom de los datos espaciales que seleccionó en la página del asistente anterior. La vista que establece se aplica a todo el mapa.  
  
-   Establecer la resolución del mapa.  
  
-   Especificar si incrustar los datos espaciales en el informe.  
  
-   Para los datos incrustados, especificar si incluir todos los datos o simplemente los de la vista actual.  
  
-   Especificar si incluir un fondo de mosaicos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Bing Maps.  
  
###  <a name="what-is-the-map-view-or-viewport"></a><a name="Viewport"></a> ¿Qué es la vista o ventanilla del mapa?  
 La ventanilla del mapa define el área del mapa que se mostrará para todas las capas del informe.  
  
 De forma predeterminada, la escala de colores y la escala de distancia aparecen dentro de la ventanilla, y la leyenda del mapa aparece fuera. Puede cambiar estas opciones para la ventanilla tras completar el asistente.  
  
###  <a name="what-is-map-resolution-and-optimization"></a><a name="Resolution"></a> ¿Qué es la resolución y la optimización del mapa?  
 Al cambiar la resolución para los datos espaciales que representan líneas o polígonos, especifica con qué grado de detalle desea dibujar el mapa. Por ejemplo, para las vistas aéreas de una región, ¿necesita la granularidad de un centenar de metros del área de la tierra o con una resolución de una milla es suficiente?  
  
 Cuando los datos espaciales se incrustan en el informe, una resolución mayor aumenta el número de elementos que se necesitan para dibujar los detalles en esa resolución. Cuando los datos espaciales no se incrustan en el informe, una resolución mayor aumenta la cantidad de tiempo requerida por el procesador de informes para calcular las líneas del mapa en esa resolución cada vez que vea el informe.  
  
 Al bajar la resolución, el procesador de informes aplica un algoritmo para reducir el número de puntos necesarios para dibujar los elementos de mapa. Cuanto menor es la resolución, menos datos se precisan para mostrar los elementos de mapa, lo que puede conducir a un mejor rendimiento de la presentación.  
  
 A medida que ajusta el control deslizante, los datos de la vista previa del panel del asistente se actualizan para darle una indicación del efecto. Después de agregar el mapa al informe, puede ajustar este valor cambiando las opciones de la ventanilla del mapa.  
  
###  <a name="what-does-embedding-spatial-data-do"></a><a name="Embed"></a> ¿Qué hace la incrustación de datos espaciales?  
 Al incrustar elementos de un mapa o mosaicos de Bing Maps en un informe, los datos espaciales se almacenan en la definición de informe.  
  
 Un informe con un mapa puede utilizar los datos espaciales o los mosaicos de Bing Maps que se recuperan dinámicamente, ya sea cuando se procesa el informe o en tiempo de diseño, y cuando se incrustan en la definición de informe. Los elementos de mapa incrustados pueden aumentar considerablemente el tamaño de la definición de informe, pero reducen el tiempo necesario para ver el mapa en el informe. Los elementos de mapa dinámicos reducen el tamaño de la definición de informe, pero aumentan el tiempo que se tarda en procesar y ver el mapa.  
  
 Para que un informe esté bien diseñado, es necesario evaluar las ventajas comparativas entre los datos de mapas dinámicos y estáticos, y encontrar un equilibrio que funcione en sus circunstancias. En general, más datos implican que la definición de informe y el informe compilado requieren más almacenamiento en el servidor de informes y tiempos de proceso más largos. Siempre es aconsejable recortar los datos espaciales, así como limitar los otros datos del informe, con el fin de incluir simplemente lo que se necesita para el informe.  
  
###  <a name="what-is-a-bing-map-tile-background"></a><a name="Tiles"></a> ¿Qué es un fondo de mosaicos de Bing Maps?  
 Para agregar un fondo de imagen geográfica a un mapa, seleccione la opción de fondo de mosaico de Bing Maps. El procesador de informes descarga los mosaicos de Servicios web de Bing Maps para el área y la resolución del mapa que especifique en esta página del asistente. Puede especificar uno de los siguientes tipos de mosaico:  
  
-   **Carretera.** Muestra un estilo de mapa de carreteras con un fondo blanco.  
  
-   **Aéreo.** Muestra solo una vista aérea. En este modo no se muestra ningún texto.  
  
-   **Híbridas.** Muestra la combinación de las vistas **Carretera** y **Aéreo** .  
  
 Para obtener más información acerca de los mosaicos, vea [Sistema de mosaicos de Bing Maps](/bingmaps/articles/bing-maps-tile-system). Para obtener más información sobre el uso de mosaicos de Bing Maps en un informe, vea [Condiciones adicionales de uso](https://go.microsoft.com/fwlink/?LinkId=151371).  
  
 Para ver un fondo de mosaico en la vista de diseño, debe tener acceso a Internet. Para ver el fondo de mosaico en una vista previa de un informe en un servidor de informes, el servidor de informes se debe configurar para ser compatible con los mosaicos de Bing Maps. Para obtener más información, consulte [Informes de solución de problemas: informes de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md) y [Planear un informe de mapa](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
 Para más información sobre otras formas de personalizar una capa de mosaico, vea [Agregar, cambiar o eliminar un mapa o una capa de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
##  <a name="choose-map-visualization"></a><a name="Visualization"></a> Elegir la visualización de un mapa  
 En esta página, elija el tipo de mapa o la capa de mapa que desea agregar a un informe. La primera vez que ejecuta el asistente, agrega al informe un mapa y la primera capa de mapa. Un mapa puede contener varias capas de mapa. Cada capa de mapa muestra un tipo específico de datos espaciales: polígonos, líneas o puntos.  
  
 El tipo de mapa que elija dependerá del propósito del mismo y de los datos de que disponga.  
  
###  <a name="what-is-the-difference-among-a-basic-map-a-bubble-map-and-an-analytical-map"></a><a name="MapType"></a> ¿Cuál es la diferencia entre un mapa básico, un mapa de burbujas y un mapa analítico?  
 Un **mapa básico** solo muestra las ubicaciones. Puede variar los colores de las áreas del mapa por la sombra, pero el color no representa los valores de los datos analíticos.  
  
 Un **mapa de burbujas** comunica el valor relativo de un único agregado de datos analíticos como el tamaño de burbuja, por ejemplo, las ventas de un almacén. Puede crear mapas de burbujas para polígonos o puntos. Para los polígonos, establezca las propiedades del punto central de un polígono; para los puntos, establezca las propiedades del marcador.  
  
 Un **mapa analítico** comunica el valor relativo de uno o varios agregados de datos analíticos para cada elemento de mapa. Por ejemplo, almacenes de ventas como tamaño del marcador, intervalo de beneficios de las categorías de producto como color del marcador y productos de mayor venta como tipo de marcador.  
  
 Para obtener más información, vea [Planear un informe de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md).  
  
##  <a name="choose-the-analytical-dataset"></a><a name="AnalyticalData"></a> Elegir el conjunto de datos analíticos  
 En esta página, especifique dónde obtener los datos analíticos para que se muestren en esta capa de mapa.  
  
 Para mostrar los datos del informe o cualquier dato analítico con el fondo del mapa, debe especificar dónde están los datos y cómo se relacionan con los datos espaciales. Los datos pueden proceder de un conjunto de datos de informes existentes o de un nuevo conjunto de datos para los que genere una consulta. Los datos analíticos existentes podrían estar incluidos en un archivo de forma ESRI que contenga datos espaciales.  
  
###  <a name="what-is-the-difference-between-spatial-data-and-analytical-data"></a><a name="Diff"></a> ¿Cuál es la diferencia entre los datos espaciales y los datos analíticos?  
 Los datos espaciales están compuestos de conjuntos de coordenadas que especifican puntos, líneas y polígonos. Los elementos de mapa se basan en los datos espaciales.  
  
 Los datos analíticos son datos numéricos o de categorías que se usan para variar la apariencia del mapa. El análisis puede provenir de un conjunto de datos de informe o podría incluirse con los datos espaciales de un mapa de la galería de mapas o de un archivo de forma ESRI.  
  
##  <a name="specify-the-match-fields"></a><a name="SpecifyMatchFields"></a> Especificar los campos coincidentes  
 En esta página, se genera una relación entre los datos espaciales y los datos analíticos.  
  
###  <a name="what-are-match-fields"></a><a name="MatchFields"></a> ¿Qué son los campos coincidentes?  
 Los campos coincidentes permiten al procesador de informes generar una relación entre los datos analíticos y los datos espaciales. Especifican valores únicos dentro de los datos analíticos. Por ejemplo, el nombre del almacén podría no ser único dentro de los datos, de modo que podría especificar tanto una ciudad como el nombre del almacén.  
  
##  <a name="choose-color-theme-and-data-visualization"></a><a name="ThemeandVisualization"></a> Elegir el tema de color y la visualización de los datos  
 En esta página, puede especificar cómo visualizar los datos con el fondo del mapa, el tema del mapa, los campos que visualizar y qué variar: color, tamaño y/o tipo de marcador.  
  
###  <a name="what-does-the-theme-do"></a><a name="Theme"></a> ¿Qué hace el tema?  
 El tema que elija establece los valores predeterminados para el color, borde y fuente. Puede cambiar estas opciones tras completar el asistente.  
  
###  <a name="what-are-the-legends-and-scales-in-map-preview-for"></a><a name="Legends"></a> ¿Para qué son las leyendas y las escalas de Vista previa del mapa?  
 Las leyendas ayudan a los usuarios a interpretar los datos que se muestran en un mapa. Un mapa proporciona un intervalo de colores, una escala de distancia y una leyenda.  
  
-   **Rango de colores.** El rango de colores muestra una barra de color con una escala que proporciona una guía para los intervalos de datos que están determinados por el procesador de informes según las reglas que se especifican para la capa.  
  
-   **Escala de distancia.** La escala de distancia proporciona una guía para las unidades de distancia en el mapa. Las unidades de distancia se determinan automáticamente según la proyección del mapa y el nivel de zoom.  
  
-   **Leyenda.** Proporciona una guía para ayudar a interpretar el significado de los colores, tamaños y tipos de marcador en un mapa. De forma predeterminada, todas las reglas de todos los niveles muestran los intervalos de datos en la primera leyenda. Puede personalizar esta leyenda y agregar leyendas después de agregar el mapa al informe.  
  
###  <a name="what-are-rules"></a><a name="Rules"></a> ¿Qué son las reglas?  
 Las reglas son cálculos que el procesador de informes utiliza para dividir los datos analíticos en intervalos. Puede especificar reglas diferentes para cada capa. El tipo de reglas que puede especificar depende del tipo de datos espaciales de la capa:  
  
-   **Polígonos.** Puede especificar reglas de color.  
  
-   **Puntos centrales de los polígonos.** Puede especificar reglas de color, reglas de tamaño y reglas de tipo de marcador.  
  
-   **Líneas.** Puede especificar reglas de color y de ancho.  
  
-   **Puntos.** Puede especificar reglas de color, reglas de tamaño y reglas de tipo de marcador.  
  
 El procesador de informes aplica las reglas que establezca y determina automáticamente la lista de elementos que se mostrarán en una leyenda. De forma predeterminada, los resultados de todas las capas se muestran en la primera leyenda. Puede ajustar esto al completar el asistente. Para obtener más información, vea [Variar la presentación de polígonos, líneas y puntos usando reglas y datos analíticos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="see-also"></a>Consulte también  
 [Solucionar problemas de los informes: informes de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/troubleshoot-reports-map-reports-report-builder-and-ssrs.md)   
 [Planear un informe de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/plan-a-map-report-report-builder-and-ssrs.md)   
 [Mapas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)  
  
