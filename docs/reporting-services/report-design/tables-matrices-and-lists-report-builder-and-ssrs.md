---
title: Tablas, matrices y listas (Generador de informes) | Microsoft Docs
description: Obtenga información sobre las tablas, matrices y listas, las regiones de datos Tablix en las que se muestran los datos del informe paginado organizados en filas y columnas en el Generador de informes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.tablixgroup.f1
- "10045"
- sql13.rtp.rptdesigner.tablix.visibility.f1
- "10039"
- sql13.rtp.rptdesigner.groupproperties.visibility.f1
- "10104"
- "10047"
- sql13.rtp.rptdesigner.groupproperties.advanced.f1
- "10044"
- sql13.rtp.rptdesigner.groupproperties.filters.f1
- sql13.rtp.rptdesigner.tablix.sort.f1
- sql13.rtp.rptdesigner.tablix.general.f1
- sql13.rtp.rptdesigner.groupproperties.general.f1
- sql13.rtp.rptdesigner.groupproperties.variables.f1
- "10046"
- "10101"
- sql13.rtp.rptdesigner.tablix.filter.f1
- sql13.rtp.rptdesigner.groupproperties.sort.f1
- "10042"
- "10041"
- "10102"
- "10103"
- "10043"
- sql13.rtp.rptdesigner.groupproperties.pagebreaks.f1
ms.assetid: 9dcf3fc8-bf9c-4a14-a03d-e78254aa4098
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f9276557c2c2d5d3f733d6f37d243eaee0eec68a
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534296"
---
# <a name="tables-matrices-and-lists-report-builder-and-ssrs"></a>Tablas, matrices y listas (Generador de informes y SSRS)
 En [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)], las tablas, matrices y listas son *regiones de datos* en las que se muestran los datos del informe paginado en celdas organizadas en filas y columnas. Normalmente, las celdas contienen datos como texto, fechas y números, pero también pueden contener medidores, gráficos o elementos de informe como imágenes. Colectivamente, las tablas, matrices y listas se denominan a menudo regiones de datos *Tablix* .  
  
 La plantillas de tabla, matriz y lista se generan en la región de datos Tablix, que es una cuadrícula flexible que puede mostrar datos en celdas. En las plantillas para matrices y tablas, las celdas se organizan en filas y columnas. Como las plantillas son variaciones de la región de datos Tablix genérica subyacente, los datos se pueden mostrar en combinaciones de formatos de plantilla; la tabla, matriz o lista se pueden cambiar al diseñar el informe, de forma que contengan características de otra región de datos. Por ejemplo, si agrega una tabla y se da cuenta de que no le resulta útil, puede agregar grupos de columnas para convertir la tabla en una matriz.  
  
 Las regiones de datos de matriz y tabla pueden mostrar relaciones complejas entre los datos mediante la inclusión de tablas, matrices, listas, gráficos y medidores anidados. Las tablas y matrices tienen un diseño tabular y sus datos proceden de un conjunto de datos único, generado a partir de un origen de datos único. La diferencia clave entre las tablas y las matrices es que las tablas solo pueden tener grupos de filas, mientras que las matrices tienen grupos de filas y grupos de columnas.  
  
 Las listas son un poco diferentes. Admiten un diseño libre que puede incluir varias tablas o matrices del mismo nivel, cada una de las cuales usa los datos de un conjunto de datos diferente. Las listas también se pueden utilizar para los formularios, como facturas.  
  
 Las siguientes imágenes ilustran informes sencillos con una tabla, matriz o lista.  
  
 ![RS_TableMatrixList](../../reporting-services/report-design/media/rs-tablematrixlist.gif "RS_TableMatrixList")  
  
 Para empezar a trabajar rápidamente con tablas, matrices y listas, vea [Tutorial: Crear un informe de tabla básico &#40;Generador de informes&#41;](../../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md), [Tutorial: Crear un informe de matriz &#40;Generador de informes&#41;](../../reporting-services/tutorial-creating-a-matrix-report-report-builder.md) y [Tutorial: Crear un informe de forma libre &#40;Generador de informes&#41;](../../reporting-services/tutorial-creating-a-free-form-report-report-builder.md).  
  
> [!NOTE]  
>  Puede publicar tablas, matrices y listas por separado de un informe como elementos de informe. Para más información, vea [Elementos de informe](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
##  <a name="table"></a><a name="Table"></a> Table  
 use una tabla para mostrar datos detallados, para organizar los datos en grupos de filas, o para ambas cosas. La plantilla Tabla contiene tres columnas con una fila de encabezado de tabla y una fila de detalles para los datos. En la ilustración siguiente, se muestra la plantilla de tabla inicial, seleccionada en la superficie de diseño:  
  
 ![Plantilla de tabla en la superficie de diseño, seleccionada](../../reporting-services/report-design/media/rs-tabletemplatenewselected.gif "Plantilla de tabla en la superficie de diseño, seleccionada")  
  
 Puede agrupar los datos por un solo campo, por varios campos o escribiendo su propia expresión. Puede crear grupos anidados o independientes, grupos adyacentes y presentar valores para datos agrupados, o agregar totales a los grupos. Por ejemplo, si la tabla tiene un grupo de filas llamado [Categoría], puede agregar un subtotal para cada grupo, así como un total general para el informe. Para mejorar la apariencia de tabla y resaltar los datos a los que desee dar énfasis, puede combinar celdas y aplicar formato a los datos y encabezados de tabla.  
  
 Puede ocultar inicialmente los datos detallados o agrupados, e incluir controles de alternancia de obtención de detalles para permitir a los usuarios elegir interactivamente cuántos datos se van a mostrar.  
  
 Para más información, vea [Tablas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md).  
  
##  <a name="matrix"></a><a name="Matrix"></a> Matriz  
 use una matriz para mostrar resúmenes de los datos agregados agrupados en filas y en columnas; algo similar a una tabla dinámica o a una tabla de referencias cruzadas. El número de valores únicos por cada grupo de filas y columnas determina el número de filas y de columnas de los grupos. En la ilustración siguiente, se muestra la plantilla de matriz inicial, seleccionada en la superficie de diseño:  
  
 ![Nueva matriz agregada desde el cuadro de herramientas, seleccionada](../../reporting-services/report-design/media/rs-matrixtemplatenewselected.gif "Nueva matriz agregada desde el cuadro de herramientas, seleccionada")  
  
 Puede agrupar datos por varios campos o expresiones en grupos de filas y de columnas. En tiempo de ejecución, cuando se combinan las regiones de datos y los datos del informe, una matriz crece en horizontal y vertical en la página al irse agregando columnas a los grupos de columnas y filas a los grupos de filas. Las celdas de la matriz muestran valores agregados cuyo ámbito es la intersección de los grupos de filas y de columnas a los que pertenece la celda. Por ejemplo, si la matriz tiene un grupo de filas (Categoría) y dos grupos de columnas (Territorio y Año) que muestran la suma de las ventas, el informe muestra dos celdas con las sumas de ventas de cada valor del grupo de categorías. El ámbito de las celdas son las dos intersecciones: Category y Territory, y Category y Year. La matriz puede tener grupos anidados y adyacentes. Los grupos anidados tienen una relación primario-secundario y los adyacentes una relación del mismo nivel. Puede agregar los subtotales a cualquiera de los niveles de grupos anidados de filas y columnas de la matriz.  
  
 Para que los datos de la matriz sean más legibles y resaltar los datos a los que desea dar énfasis, puede combinar celdas, dividir los datos en horizontal y en vertical, o aplicar formato a los datos y encabezados de grupo.  
  
 También puede incluir controles de alternancia de obtención de detalles que ocultan inicialmente los datos detallados; de esta forma, el usuario podrá hacer clic en dichos controles para mostrar más o menos detalles, según sea necesario.  
  
 Para más información, vea [Crear una matriz](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md).  
  
##  <a name="list"></a><a name="List"></a> Lista  
 use una lista para crear un diseño de forma libre. Con una lista, no está limitado a un diseño de cuadrícula, sino que puede colocar libremente los campos dentro de la lista. Use una lista para diseñar un formulario que permita mostrar muchos campos de conjunto de datos, o como contenedor para mostrar en paralelo varias regiones de datos para los datos agrupados. Por ejemplo, puede definir un grupo para una lista; agregar una tabla, un gráfico y una imagen; y mostrar los valores en forma de tabla y de gráfico para cada valor del grupo, tal y como lo haría con un registro de un empleado o de un paciente.  
  
 ![Nueva lista agregada desde el cuadro de herramientas, seleccionada](../../reporting-services/report-design/media/rs-listtemplatenewselected.gif "Nueva lista agregada desde el cuadro de herramientas, seleccionada")  
  
 Para obtener más información, consulte [Crear facturas y formularios con listas](../../reporting-services/report-design/create-invoices-and-forms-with-lists-report-builder-and-ssrs.md).  
  
##  <a name="preparing-data"></a><a name="PreparingData"></a> Preparar los datos  
 Una región de datos de tabla, matriz y lista muestra datos de un conjunto de datos. Puede preparar los datos en la consulta que recupera los datos para el conjunto de datos, o establecer las propiedades en la tabla, matriz o lista.  
  
 Los lenguajes de consulta como [!INCLUDE[tsql](../../includes/tsql-md.md)], que se usan para recuperar los datos para los conjuntos de datos de informe, pueden preparar los datos aplicando filtros que incluyan solo un subconjunto de los datos, reemplazando valores nulos o en blanco con constantes que hagan el informe más legible, y ordenando y agrupando los datos.  
  
 Si decide preparar los datos de la región de datos de tabla, matriz o lista de un informe, debe establecer las propiedades en la región de datos o las celdas de la región de datos. Si desea filtrar u ordenar los datos, debe establecer las propiedades en la región de datos. Por ejemplo, para ordenar los datos debe especificar las columnas por las que se va a ordenar y el sentido de la ordenación. Si desea proporcionar un valor alternativo para un campo, debe establecer los valores del texto de la celda en que se muestra el campo. Por ejemplo, para mostrar En blanco cuando un campo esté vacío o tenga un valor nulo, debe usar una expresión para establecer el valor.  
  
 Para más información, vea [Preparar datos para su presentación en una región de datos Tablix &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/preparing-data-for-display-in-a-tablix-data-region-report-builder-and-ssrs.md).  
  
##  <a name="building-and-configuring-a-table-matrix-or-list"></a><a name="BuildingConfiguringTableMatrixList"></a> Generar y configurar un tabla, matriz o lista  
 Al agregar tablas o matrices a un informe, puede usar el Asistente para tabla o matriz, o generarlas manualmente a partir de las plantillas proporcionadas por el Generador de informes y el Diseñador de informes. Las listas se generan manualmente a partir de la plantilla de lista.  
  
 El asistente indica todos los pasos para generar y configurar rápidamente una tabla o una matriz. Después de completar el asistente o generar las regiones de datos Tablix desde cero, puede configurarlas y refinarlas. Los cuadros de diálogo, disponible en los menús contextuales en las regiones de datos, facilitan el establecimiento de las propiedades más utilizadas para los saltos de página, repeticiones y visibilidad de encabezados y pies de página, opciones de pantalla, filtros y orden. La región de datos Tablix ofrece muchas otras propiedades, que solo puede establecer en el panel Propiedades de Generador de informes. Por ejemplo, si quiere mostrar un mensaje cuando el conjunto de datos de una tabla, matriz o lista esté vacío, debe especificar el texto del mensaje en la propiedad de Tablix NoRowsMessage en el panel Propiedades.  
  
##  <a name="changing-between-tablix-templates"></a><a name="ChangingBetweenTablixTemplates"></a> Cambiar entre las plantillas de Tablix  
 La plantilla inicial de Tablix que elija no es necesariamente definitiva. Mientras agrega grupos, totales y etiquetas, es posible que decida modificar el diseño de Tablix. Por ejemplo, puede comenzar con una tabla y, a continuación, eliminar la fila de detalles y agregar grupos de columnas. Para más información, vea [Explorar la flexibilidad de una región de datos Tablix &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/exploring-the-flexibility-of-a-tablix-data-region-report-builder-and-ssrs.md).  
  
 Para continuar el desarrollo de una tabla, matriz o lista, puede agregar cualquier característica de Tablix. Entre las características de Tablix se incluye la visualización de datos detallados o agregados para los datos agrupados en filas y columnas. Puede crear grupos anidados, grupos adyacentes independientes o grupos recursivos. Puede filtrar y ordenar datos agrupados, y combinar grupos fácilmente mediante la inclusión de varias expresiones de grupo en una definición de grupo.  
  
 También puede agregar totales para un grupo o totales generales para la región de datos. Puede ocultar filas o columnas para simplificar un informe y permitir al usuario alternar entre mostrar u ocultar los datos, como en un informe detallado. Para más información, vea [Controlar la presentación de la región de datos Tablix en una página de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md).  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> Temas de procedimientos  
 En esta sección se describen procedimientos que muestran, paso a paso, cómo trabajar con tablas, matrices y listas en los informes; cómo mostrar los datos en filas y columnas, agregar y eliminar columnas, combinar celdas, e incluir subtotales para los grupos de filas y columnas.  
  
-   [Agregar un grupo de detalles &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-a-details-group-report-builder-and-ssrs.md)  
  
-   [Agregar un total a un grupo o a una región de datos Tablix &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-a-total-to-a-group-or-tablix-data-region-report-builder-and-ssrs.md)  
  
-   [Cambiar un elemento de una celda &#40;Diseñador de informes y SSRS&#41;](../../reporting-services/report-design/change-an-item-within-a-cell-report-builder-and-ssrs.md)  
  
-   [Cambiar el alto de fila o el ancho de columna &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-row-height-or-column-width-report-builder-and-ssrs.md)  
  
-   [Insertar o eliminar una columna &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md)  
  
-   [Insertar o eliminar una fila &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-row-report-builder-and-ssrs.md)  
  
-   [Combinar celdas en una región de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/merge-cells-in-a-data-region-report-builder-and-ssrs.md)  
  
-   [Crear un grupo de jerarquía recursiva &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/create-a-recursive-hierarchy-group-report-builder-and-ssrs.md)  
  
-   [Agregar o eliminar un grupo en una región de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-or-delete-a-group-in-a-data-region-report-builder-and-ssrs.md)  
  
-   [Mostrar encabezados y pies de página con un grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md)  
  
-   [Crear un informe escalonado &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/create-a-stepped-report-report-builder-and-ssrs.md)  
  
-   [Agregar, mover o eliminar una tabla, una matriz o una lista &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-move-or-delete-a-table-matrix-or-list-report-builder-and-ssrs.md)  
  
##  <a name="in-this-section"></a><a name="InThisSection"></a> En esta sección  
 En los siguientes temas se proporciona información adicional acerca de cómo trabajar con la región de datos Tablix.  
  
 [Región de datos Tablix &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tablix-data-region-report-builder-and-ssrs.md)  
 Explica los conceptos clave relacionados con la región de datos Tablix, por ejemplo las áreas de Tablix, datos detallados y agrupados, grupos de filas y columnas, filas y columnas dinámicas y estáticas.  
  
 [Agregar datos a una región de datos Tablix &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/adding-data-to-a-tablix-data-region-report-builder-and-ssrs.md)  
 Proporciona información detallada sobre cómo agregar a una región de datos Tablix datos detallados y agrupados, subtotales y totales, y etiquetas.  
  
 [Controlar la presentación de la región de datos Tablix en una página de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/controlling-the-tablix-data-region-display-on-a-report-page.md)  
 Se describen las propiedades que puede modificar en una región de datos Tablix para cambiar la manera en la que aparece dicha región al verla en un informe.  
  
 [Controlar los encabezados de fila y de columna &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/controlling-row-and-column-headings-report-builder-and-ssrs.md)  
 Se describe cómo controlar los encabezados de filas y columnas cuando región de datos de tabla, matriz o lista abarca varias páginas horizontal o verticalmente.  
  
 [Crear grupos de jerarquía recursiva &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/creating-recursive-hierarchy-groups-report-builder-and-ssrs.md)  
 Se describe cómo mostrar datos recursivos donde la relación entre el elemento primario y el elemento secundario se representa mediante campos en el conjunto de datos.  
  
 [Descripción de los grupos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md)  
 Se explica qué son los grupos y cuándo utilizarlos, y se describen los grupos disponibles para las distintas regiones de datos Tablix.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar filtros de conjunto de datos, filtros de región de datos y filtros de grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Anidar regiones de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/nested-data-regions-report-builder-and-ssrs.md)   
 [Vincular varias regiones de datos al mismo conjunto de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/linking-multiple-data-regions-to-the-same-dataset-report-builder-and-ssrs.md)   
 [Expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Filtrar, agrupar y ordenar datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Gráficos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Medidores &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/gauges-report-builder-and-ssrs.md)  
  
  
