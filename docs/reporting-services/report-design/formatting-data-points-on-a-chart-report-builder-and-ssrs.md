---
title: Aplicación de formato a los puntos de datos de un gráfico (Generador de informes) | Microsoft Docs
description: Obtenga información sobre los distintos tipos de formato de los puntos de datos de los gráficos de los informes del Generador de informes.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- "10248"
- sql13.rtp.rptdesigner.serieslabelproperties.general.f1
ms.assetid: 08ec3818-f63a-4e89-b52c-750e47f48b85
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0d9f3036e7d8c580e770ded7bbbec6e5f29b2750
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596909"
---
# <a name="formatting-data-points-on-a-chart-report-builder-and-ssrs"></a>Aplicar formato a los puntos de datos de un gráfico (Generador de informes y SSRS)
En el informe paginado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , un punto de datos es la entidad individual más pequeña del gráfico. En los gráficos que no son de formas, los puntos de datos se representan en función del tipo de gráfico. Por ejemplo, una serie de líneas está formada por uno o más puntos de datos conectados. En los gráficos de formas, los puntos de datos se representan por sectores o segmentos individuales que se agregan al gráfico. Por ejemplo, en un gráfico circular, cada sector es un punto de datos. Para más información, vea [Tipos de gráficos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/chart-types-report-builder-and-ssrs.md).  
  
 Uno o más puntos de datos forman una serie. De forma predeterminada, todas las opciones de formato se aplican a todos los puntos de datos de la serie. Si desea especificar propiedades para puntos de datos individuales, puede especificar un campo o una expresión en la serie que dé formato al punto de datos individual en tiempo de ejecución basándose en el conjunto de datos.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="adding-tooltips-and-drillthrough-actions-to-data-points"></a>Agregar información sobre herramientas y acciones de obtención de detalles a los puntos de datos  
 Puede agregar información sobre herramientas a cada punto de datos estableciendo el valor de la propiedad **ToolTip** de la serie. Muestre información sobre herramientas si desea ofrecer a los usuarios la posibilidad de ver cualquier información relacionada con el punto de datos, como el nombre de grupo, el valor del punto de datos y el porcentaje del punto de datos con respecto al total de la serie. Para más información, vea [Mostrar la información sobre herramientas en una serie &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md).  
  
 También puede especificar una acción de obtención de detalles para que los puntos de datos de la serie muestren otro informe o una dirección URL. Puede pasar parámetros para mostrar información relacionada con el punto de datos en el que se ha hecho clic. Para más información, vea [Agregar una acción de obtención de detalles en un informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-a-drillthrough-action-on-a-report-report-builder-and-ssrs.md).  
  
## <a name="highlighting-individual-data-points-in-a-series"></a>Resaltar puntos de datos individuales en una serie  
 En cualquier gráfico que no sea de formas, puede resaltar puntos de datos individuales si especifica una expresión para la propiedad Color. Por ejemplo, para resaltar el valor del punto de datos más alto de una serie denominada `MyField` con un color diferente del de los otros puntos de datos, la expresión sería similar a la siguiente:  
  
 `=Iif(Fields!MyField.Value >= Max(Fields!MyField.Value, "MyDataSet"), "Red", "Green")`  
  
 En este ejemplo, el valor más alto para `MyField` tendrá el color rojo y el resto de puntos de datos tendrán el color verde. Al especificar un color para la serie usando la propiedad **Fill** de la serie, el gráfico invalida los colores que se especifican en la paleta. Para obtener más información, vea [Aplicar formato a los colores de serie de un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-series-colors-on-a-chart-report-builder-and-ssrs.md).  
  
## <a name="positioning-data-point-labels-on-a-chart"></a>Colocar etiquetas de puntos de datos en un gráfico  
 Para mostrar etiquetas de punto de datos en cualquier tipo de gráfico, haga clic con el botón derecho en el gráfico y seleccione **Mostrar etiquetas de datos**. La posición de las etiquetas de puntos de datos se especifica en función del tipo de gráfico:  
  
-   En un gráfico de barras, puede cambiar la posición de la etiqueta de punto de datos usando el atributo personalizado **BarLabelStyle** . Hay cuatro posiciones posibles: Externa, Izquierda, Centro y Derecha. Si el estilo de la etiqueta de la barra se establece en Externa, la etiqueta se colocará fuera de la barra, siempre y cuando quepa en el área del gráfico. Si la etiqueta no se puede colocar fuera de la barra ni dentro del área de gráfico, la etiqueta se situará dentro de la barra.  
  
-   En un gráfico circular, puede cambiar la posición de la etiqueta de punto de datos usando el atributo personalizado **PieLabelStyle** . Hay muchas consideraciones que se deben tener en cuenta al colocar etiquetas de puntos de datos alrededor de un gráfico circular, entre las que se incluyen el tamaño del gráfico, el espacio disponible entre el gráfico y su leyenda correspondiente, y el tamaño de las etiquetas. Para más información, vea [Mostrar las etiquetas de los puntos de datos fuera de un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md).  
  
-   En un gráfico de embudo o piramidal, puede cambiar de posición las etiquetas de puntos de datos usando los atributos personalizados **FunnelLabelStyle** y **PyramidLabelStyle** . Puede establecer estos atributos en el panel **Propiedades** después de seleccionar un tipo de gráfico de embudo o piramidal.  
  
-   En los gráficos apilados, las etiquetas de puntos de datos se colocan siempre dentro de la serie y se ignora la propiedad **Position** en la etiqueta de la serie.  
  
-   En el resto de tipos de gráficos, puede cambiar de posición la etiqueta de punto de datos usando la propiedad **Position** en la etiqueta de la serie. De forma predeterminada, el gráfico calcula automáticamente la posición de las etiquetas de puntos de datos con objeto de evitar colisiones entre ellas. Al establecer un valor para **Position**, todas las etiquetas de puntos de datos se colocarán de la misma manera, lo que puede ocasionar que las etiquetas se superpongan. Se recomienda usar este método solo cuando se disponga de pocos puntos de datos.  
  
 Para más información, vea [Colocar etiquetas en un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/position-labels-in-a-chart-report-builder-and-ssrs.md).  
  
## <a name="adding-keywords-for-data-point-labels-tooltips-and-legend-text"></a>Agregar palabras clave para las etiquetas de punto de datos, información sobre herramientas y texto de la leyenda  
 Puede usar palabras clave específicas del gráfico, con distinción de mayúsculas y minúsculas, para representar un elemento que existe en el gráfico. Estas palabras clave solo son aplicables a información sobre herramientas, texto de leyenda personalizado y propiedades de etiquetas de puntos de datos. En muchos casos, una palabra clave de gráfico tiene una expresión simple equivalente, pero la palabra clave se puede escribir más rápida y fácilmente. La lista siguiente es una relación de palabras clave de gráfico.  
  
|Palabra clave de gráfico|Descripción|Aplicable al tipo de gráfico|Ejemplo de una expresión simple equivalente|  
|-------------------|-----------------|------------------------------|------------------------------------------------|  
|#VALY|Valor Y del punto de datos.|All|`=Fields!MyDataField.Value`|  
|#VALY2|Valor Y nº 2 del punto de datos.|De intervalo y de burbuja|None|  
|#VALY3|Valor Y nº 3 del punto de datos.|De cotizaciones y de vela japonesa|None|  
|#VALY4|Valor Y nº 4 del punto de datos.|De cotizaciones y de vela japonesa|None|  
|#SERIESNAME|Nombre de la serie.|All|None|  
|#LABEL|Etiqueta de punto de datos.|All|None|  
|#AXISLABEL|Etiqueta de punto de datos de eje.|Forma|`=Fields!MyDataField.Value`|  
|#INDEX|Índice de punto de datos.|All|None|  
|#PERCENT|Porcentaje del valor Y del punto de datos.|All|`=FormatPercent(Fields!MyDataField.Value/Sum(Fields!MyDataField.Value, "MyDataSet"),2)`|  
|#TOTAL|Total de todos los valores Y de la serie.|All|`=Sum(Fields!MyDataField.Value)`|  
|#LEGENDTEXT|Texto correspondiente al texto del elemento de la leyenda.|All|None|  
|#AVG|Promedio de todos los valores Y de la serie.|All|`=Avg(Fields!MyDataField.Value)`|  
|#MIN|Mínimo de todos los valores Y de la serie.|All|`=Min(Fields!MyDataField.Value)`|  
|#MAX|Máximo de todos los valores Y de la serie.|All|`=Max(Fields!MyDataField.Value)`|  
|#FIRST|El primero de todos los valores Y de la serie.|All|`=First(Fields!MyDataField.Value)`|  
  
 Para dar formato a la palabra clave, incluya una cadena de formato de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] entre paréntesis. Por ejemplo, para especificar el valor del punto de datos en una información sobre herramientas como un número con dos posiciones decimales, incluya la cadena de formato "N2" entre llaves (por ejemplo, "#VALY {N2}" para la propiedad **ToolTip** de la serie). Para obtener más información sobre las cadenas de formato de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] , vea [Aplicar formato a tipos](/previous-versions/) en MSDN. Para obtener más información sobre cómo dar formato a los números en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Aplicar formato a números y fechas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-numbers-and-dates-report-builder-and-ssrs.md).  
  
 Para obtener más información sobre cómo agregar palabras clave a un gráfico, vea [Mostrar la información sobre herramientas en una serie &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md) y [Cambiar el texto de un elemento de leyenda &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/chart-legend-change-item-text-report-builder.md).  
  
## <a name="increasing-readability-in-a-chart-with-multiple-data-points"></a>Aumentar la legibilidad en un gráfico con varios puntos de datos  
 Si tiene varias series en un gráfico, puede reducir la legibilidad de los puntos de datos del gráfico. Al agregar varias series al gráfico, considere la posibilidad de usar una técnica que permita leer y entender cada serie en el gráfico de manera eficaz. Para más información, vea [Mostrar varias series en un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/multiple-series-on-a-chart-report-builder-and-ssrs.md):  
  
 Para simplificar el trabajo, cuando esté usando un gráfico de formas, considere la posibilidad de agregar un único campo de datos y un único campo de categorías. Para más información, vea [Gráficos de formas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/shape-charts-report-builder-and-ssrs.md). Si un gráfico necesita más de un campo de datos y un campo de categoría, considere cambiar el tipo de gráfico. Puede hacer clic con el botón derecho en la serie y seleccionar **Cambiar tipo de gráfico**.  
  
## <a name="inserting-data-point-markers"></a>Insertar marcadores de punto de datos  
 Un marcador de punto de datos es un indicador visual que se usa para destacar cada uno de los puntos de datos de una serie. En un gráfico de dispersión, el marcador se usa para determinar la forma y el tamaño de los puntos de datos individuales. El tamaño del marcador se especifica en función del tipo de gráfico. Puede cambiar el tamaño, el color o el estilo del marcador. Los marcadores no están disponibles para los tipos de gráficos de intervalos y de formas, ni tampoco para los subtipos de gráficos apilados.  
  
## <a name="in-this-section"></a>En esta sección  
 [Mostrar la información sobre herramientas en una serie &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/show-tooltips-on-a-series-report-builder-and-ssrs.md)  
  
 [Mostrar las etiquetas de los puntos de datos fuera de un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)  
  
 [Mostrar valores de porcentaje en un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)  
  
## <a name="see-also"></a>Consulte también  
 [Aplicar formato a un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Aplicar formato a las etiquetas de los ejes de un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md)   
 [Gráficos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Aplicar formato de fecha o de moneda a las etiquetas de los ejes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md)   
 [Tutorial: Agregar un gráfico circular a un informe &#40;Generador de informes&#41;](../../reporting-services/tutorial-add-a-pie-chart-to-your-report-report-builder.md)   
 [Ejemplos de expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
