---
title: Minigráficos y barras de datos (Generador de informes) | Microsoft Docs
description: Descubra las ventajas de usar minigráficos y barras de datos en el Generador de informes. Estos gráficos compactos transmiten mucha información en muy poco espacio.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.sparklines.f1
- "10544"
ms.assetid: b287436b-fa48-4970-a1a7-1dbcb86e7411
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b8eb3019b347427b2b24a8fb48083a77935f75f8
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595156"
---
# <a name="sparklines-and-data-bars-report-builder-and-ssrs"></a>Minigráficos y barras de datos (Generador de informes y SSRS)
  Los minigráficos y las barras de datos son gráficos simples y pequeños que contienen mucha información en poco espacio, a menudo conjuntamente con texto.   
    
  En los informes paginados de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , los minigráficos y las barras de datos se usan a menudo en las tablas y matrices. Su importancia radica en que permiten ver muchos datos juntos y compararlos rápidamente uno encima de otro, en lugar de verlos de forma individual. Facilitan la visualización de los valores atípicos, las filas que no se muestran como las demás. Aunque son pequeños, cada minigráfico suele representar varios puntos de datos, con frecuencia, a lo largo del tiempo. Las barras de datos representan varios puntos de datos, pero normalmente solo muestran uno. Cada minigráfico suele presentar una única serie. No puede agregar un minigráfico a un grupo de detalles en una tabla. Dado que los minigráficos presentan los datos agregados, deben estar en una celda que esté asociada a un grupo. Los minigráficos y las barras de datos tienen los mismos elementos de gráfico básicos de las categorías, series y valores, pero carecen de leyendas, líneas de eje, etiquetas o marcas de graduación.  
  
 ![rs_SparklineExample](../../reporting-services/report-design/media/rs-sparklineexample.gif "rs_SparklineExample")  
  
 Para empezar a trabajar rápidamente con elementos de informe, vea [Tutorial: Agregar un minigráfico a un informe &#40;Generador de informes&#41;](../../reporting-services/tutorial-add-a-sparkline-to-your-report-report-builder.md) y los vídeos [How to: Create a Sparkline in a Table](/SharePoint/sharepoint-server) (Cómo crear un minigráfico en una tabla) y [Sparklines, Bar Charts, and Indicators in Report Builder](/previous-versions/dn912438(v=msdn.10)) (Minigráficos, gráficos de barras e indicadores del Generador de informes).  
  
> [!NOTE]  
>  Puede publicar minigráficos y barras de datos con su lista, matriz o tabla primaria por separado de un informe como elementos de informe. Para más información, vea [Elementos de informe](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
##  <a name="types-of-sparklines"></a><a name="KindsofSparklines"></a> Tipos de minigráficos  
 Puede crear casi tantos tipos de minigráficos como gráficos normales. En general, no puede crear minigráfico 3D. Puede crear versiones de minigráfico de estos gráficos completos:  
  
-   [Gráficos de columnas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/column-charts-report-builder-and-ssrs.md): los gráficos de columnas básicas, apiladas y 100 % apiladas.  
  
-   [Gráficos de líneas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/line-charts-report-builder-and-ssrs.md): todos los gráficos de líneas excepto el de 3D.  
  
-   [Gráficos de áreas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/area-charts-report-builder-and-ssrs.md): todos los gráficos de áreas excepto el de 3D.  
  
-   [Gráficos circulares &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md): gráficos de anillos, planos y 3D, pero no las demás formas, por ejemplo, gráficos de embudo y de pirámide.  
  
-   [Rangos de intervalos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/range-charts-report-builder-and-ssrs.md): los gráficos de cotizaciones, de vela, de barras de error y los diagramas de caja.  
  
##  <a name="data-bars"></a><a name="DataBars"></a> Barras de datos  
 Las barras de datos suelen representar un único punto de datos, aunque pueden representar varios, al igual que los gráficos de barras normales. A menudo contienen varias series sin categoría o tienen agrupaciones de serie.  
  
 ![rs_DataBars](../../reporting-services/report-design/media/rs-databars.gif "rs_DataBars")  
  
 En este ejemplo que usa las barras de datos apiladas, cada una, aunque solo sea una barra, muestra más de un punto de datos. Por ejemplo, los tres colores diferentes de la barra podrían representar tareas de tres niveles de prioridad donde la longitud de la barra representa el número total de tareas asignado a cada persona. Si realizara barras de datos apiladas al 100% en su lugar, cada barra rellenaría la celda y los diferentes colores representarían el porcentaje del todo para cada nivel de prioridad.  
  
 Puede crear versiones de barras de datos de estos gráficos completos:  
  
-   [Gráficos de barras &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/bar-charts-report-builder-and-ssrs.md): gráficos de barras básicas, apiladas y 100 % a piladas.  
  
-   [Gráficos de columnas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/column-charts-report-builder-and-ssrs.md): gráficos de columnas básicas, apiladas y 100 % apiladas. Los gráficos de columnas pueden ser minigráficos o barras de datos.  
  
##  <a name="aligning-sparkline-data-in-a-table-or-matrix"></a><a name="AlignDatainTableMatrix"></a> Alinear los datos de minigráficos en una tabla o matriz  
 Al insertar un minigráfico en una tabla o matriz, suele ser importante que los puntos de datos de cada minigráfico se alineen con los puntos de datos de los demás minigráficos de esa columna. De lo contrario, es difícil comparar los datos de las distintas filas. Por ejemplo, al comparar los datos de ventas por mes para distintos vendedores de la empresa, sería aconsejable que los meses estuvieran alineados. Si un empleado no trabajó durante el mes de abril, no habría datos para ese empleado durante ese mes. Desearía ver un hueco durante ese mes y vería los datos de los meses subsiguientes alineados con los datos de los demás empleados. Puede hacer esto alineando el eje horizontal. Para más información, vea la sección sobre minigráficos en [Ámbito de expresión para los totales, agregados y colecciones integradas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md) y también [Alinear los datos en un gráfico en una tabla o una matriz &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/align-the-data-in-a-chart-in-a-table-or-matrix-report-builder-and-ssrs.md).  
  
 De la misma forma, para que se puedan comparar por filas, los datos deben alinearse también verticalmente. Dicho de otro modo, el alto de las barras o líneas de un minigráfico o barra de datos debe estar en relación con el alto de las barras y líneas en todos los demás minigráficos o barras de datos. En caso contrario, no podrá comparar dos filas entre sí.  
  
 ![rs_SparklineAlignData](../../reporting-services/report-design/media/rs-sparklinealigndata.gif "rs_SparklineAlignData")  
  
 En esta imagen, el gráfico de columnas muestra las ventas cotidianas para cada empleado. Tenga en cuenta que durante los días que un empleado no tiene ninguna venta, el gráfico deja un espacio en blanco y alinea los días subsiguientes. Es un ejemplo de alineación horizontal. Observe también que para algunos empleados, todas las barras son cortas y ninguna barra alcanza la parte superior de la celda. El siguiente es un ejemplo de alineación vertical; sin ella, en las filas sin barras altas, las barras cortas se expandirían para rellenar el alto de la celda.  
  
##  <a name="understanding-the-data-supplied-to-a-sparkline-or-data-bar"></a><a name="UnderstandScope"></a> Descripción de los datos proporcionados a un minigráfico o barra de datos  
 La adición de un minigráfico o barra de datos a una tabla o matriz se conoce como *anidar* una región de datos en otra. La anidación significa que los datos proporcionados a un minigráfico o barra de datos son controlados por el conjunto de datos en el que se basa la tabla o la matriz y por su ubicación en la tabla o matriz. Para más información, vea [Anidar regiones de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md).  
  
##  <a name="converting-a-sparkline-or-data-bar-to-a-full-chart"></a><a name="ConvertSparklinetoChart"></a> Convertir un minigráfico o una barra de datos en un gráfico completo  
 Dado que los minigráficos y las barras de datos son un tipo de gráfico, si decide que prefiere tener toda la funcionalidad de un gráfico completo, puede realizar la conversión a un gráfico completo haciendo clic con el botón derecho en el gráfico y seleccionando **Convertir a gráfico completo**. Al hacerlo, las líneas de ejes, las etiquetas, las marcas de graduación y la leyenda se agregan automáticamente.  
  
> [!NOTE]  
>  No puede convertir un gráfico completo en minigráfico o barra de datos con un clic. Sin embargo, puede crear un minigráfico o barra de datos a partir de un gráfico completo eliminando todos los elementos de gráfico que no se encuentran en los minigráficos y barras de datos.  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Temas de procedimientos  
 [Agregar minigráficos y barras de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-sparklines-and-data-bars-report-builder-and-ssrs.md)  
  
 [Alinear los datos en un gráfico en una tabla o una matriz &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/align-the-data-in-a-chart-in-a-table-or-matrix-report-builder-and-ssrs.md)  
  
### <a name="other-how-to-topics-for-charts"></a>Otros temas de procedimientos para los gráficos  
 Puesto que los minigráficos y las barras de datos son un tipo de gráfico, los siguientes temas de procedimientos para gráficos también podrían resultarle útiles:  
  
 [Agregar un gráfico a un informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)  
  
 [Agregar puntos vacíos al gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)  
  
 [Agregar o quitar márgenes de un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-or-remove-margins-from-a-chart-report-builder-and-ssrs.md)  
  
 [Cambiar un tipo de gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-a-chart-type-report-builder-and-ssrs.md)  
  
 [Definir los colores de un gráfico mediante una paleta &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/define-colors-on-a-chart-using-a-palette-report-builder-and-ssrs.md)  
  
 [Mostrar la información sobre herramientas en una serie &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [Especificar una escala logarítmica &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)  
  
 [Especificar un intervalo de eje &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/specify-an-axis-interval-report-builder-and-ssrs.md)  
  
 [Especificar colores coherentes en varios gráficos de formas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Consulte también  
 [Gráficos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Tutorial: Agregar un minigráfico a un informe &#40;Generador de informes&#41;](../../reporting-services/tutorial-add-a-sparkline-to-your-report-report-builder.md)   
  
