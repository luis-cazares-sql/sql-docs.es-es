---
title: Reporting Services en SQL Server Data Tools (SSDT) | Microsoft Docs
description: Aprenda a usar el entorno de creación Diseñador de informes de SQL Server Data Tools en Microsoft Visual Studio para crear soluciones para Reporting Services.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- Business Intelligence Development Studio, Reporting Services in
ms.assetid: 0903c7b2-ac59-45f1-b7d0-922ecd9d76f8
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 9ed7c7d204fe315280e379206d56f122606d1f03
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597306"
---
# <a name="reporting-services-in-sql-server-data-tools-ssdt"></a>Reporting Services en SQL Server Data Tools (SSDT)

  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] es un entorno [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] para crear soluciones de inteligencia empresarial. SSDT cuenta con un Diseñador de informes para crear el entorno, donde puede abrir, modificar, obtener una vista previa, guardar e implementar definiciones de informe paginadas de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , orígenes de datos compartidos, conjuntos de datos compartidos y elementos de informe. [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] no se incluye con SQL Server. Descargar [SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md). 
  
 En este tema se describen las soluciones, proyectos, plantillas de proyecto y configuraciones de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] utilizadas para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], y las vistas, menús, barras de herramientas y métodos abreviados que se pueden usar en el Diseñador de informes.  
  
 Para empezar a diseñar informes, vea [Diseñar informes con el Diseñador de informes &#40;SSRS&#41;](../../reporting-services/tools/design-reporting-services-paginated-reports-with-report-designer-ssrs.md).  
  
##  <a name="solutions-and-projects"></a><a name="bkmk_SolutionsandProjects"></a> Soluciones y proyectos  
 Un proyecto de informe actúa como contenedor de definiciones y recursos de informe. Todos los archivos del proyecto de informe se publican en el servidor de informes cuando se implementa el proyecto. Cuando se crea un proyecto por primera vez, también se crea una solución como contenedor del proyecto. Se pueden agregar varios proyectos a una solución.  
  
  
##  <a name="configurations"></a><a name="bkmk_Configurations"></a> Configuraciones  
 Para crear varios conjuntos de propiedades de proyecto para las variaciones de implementación, como servidores de informes de prueba y producción, use el Administrador de configuración. Para obtener más información, vea [Implementación y compatibilidad de versiones en las herramientas de datos de SQL Server &#40;SSRS&#41;](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md).  
  
##  <a name="report-server-projects"></a><a name="bkmk_ReportServerProjects"></a> Proyectos de servidor de informes  
 Cuando se instala [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], pasan a estar disponibles las siguientes plantillas de proyecto en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]:  
  
-   **Proyecto de servidor de informes.** Cuando se selecciona un proyecto de servidor de informes, se abre el Diseñador de informes. Un proyecto de servidor de informes es una plantilla de proyectos de Business Intelligence instalada por [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] que se encuentra disponible en el cuadro de diálogo **Nuevo proyecto** . Para más información, vea [Agregar un informe nuevo o existente a un proyecto de informe &#40;SSRS&#41;](../../reporting-services/tools/add-a-new-or-existing-report-to-a-report-project-ssrs.md). Las propiedades de proyecto del servidor de informes se aplican a todos los informes y todos los orígenes de datos compartidos de un proyecto de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Estas propiedades incluyen la dirección URL del servidor de informes y los nombres de carpeta de los informes y orígenes de datos compartidos. Use el cuadro de diálogo **Páginas de propiedades del proyecto** para ver los valores de propiedad actuales. Para abrir este cuadro de diálogo, en el menú **Proyecto** , haga clic en **Propiedades**.  
  
-   **Asistente de proyectos de servidor de informes.** Cuando se selecciona un proyecto de asistente de proyectos de servidor de informes, se crea automáticamente un proyecto de servidor de informes y se abre el Asistente para informes. En el asistente, puede crear un informe siguiendo las instrucciones de cada página para crear una cadena de conexión a un origen de datos, establecer las credenciales del origen de datos, diseñar una consulta, agregar una región de datos de tabla o matriz, especificar datos y grupos de informe, elegir un estilo de fuente y color, publicar el informe en un servidor de informes y obtener una vista previa del informe localmente. Después de crear un informe con el asistente, puede modificar los datos del informe y el diseñador de informes mediante el Diseñador de informes en el proyecto de servidor de informes.  
  
 ![Nuevas plantillas de proyecto de SSDT](/analysis-services/analysis-services/media/ssdt-biprojects.png "Nuevas plantillas de proyecto de SSDT")  
  
  
##  <a name="report-designer-windows-and-panes"></a><a name="bkmk_ReportDesignerWindowsandPanes"></a> Ventanas y paneles del Diseñador de informes  
 El Diseñador de informes admite dos vistas: **Diseño** para definir los datos del informe y el diseño del informe y **Vista previa** para mostrar una vista representada del informe. En cada vista, pueden mostrarse varias ventanas como ayuda para diseñar o ver un informe representado.  
  
###  <a name="report-data-pane"></a><a name="bkmk_ReportDataPane"></a> Panel Datos de informe  
 El panel Datos de informe muestra los campos integrados, orígenes de datos, conjuntos de datos, colecciones de campos, parámetros de informe e imágenes.  
  
 Use el panel Datos de informe para ver:  
  
-   **Campos integrados** Los datos predefinidos del informe, como el nombre del informe o la hora en que se ha procesado el informe.  
  
-   **Orígenes de datos** Un origen de datos representa un nombre y una conexión a un origen de datos.  
  
-   **Conjuntos de datos** Cada conjunto de datos incluye una consulta que especifica qué datos deben recuperarse del origen de datos. Expanda el conjunto de datos para ver la colección de campos especificada por la consulta del conjunto de datos.  
  
     En algunos diseñadores de consultas para los conjuntos de datos multidimensionales, se pueden especificar filtros en el panel Filtros e indicar si se van a crear parámetros de informe. Si especifica la opción de parámetro de informe, se crea un conjunto de datos especial automáticamente para rellenar la lista de valores válidos del parámetro.  El conjunto de datos no aparece en el panel Datos de informe de forma predeterminada. Para obtener más información, vea [Mostrar conjuntos de datos ocultos para los valores de parámetro de datos multidimensionales &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md).  
  
-   **Parámetros de informe** La lista de parámetros de informe. Los parámetros se pueden crear manual o automáticamente cuando una consulta de conjunto de datos incluye parámetros de consulta.  
  
-   **Imágenes** La lista de imágenes disponibles para incluir como elemento del informe Imagen en un informe.  
  
 Los orígenes de datos y conjuntos de datos del panel Datos de informe representan los elementos en la definición de informe. El panel Datos de informe es una característica compatible con varios entornos de creación de informes. En el Generador de informes, es el único panel disponible para administrar los orígenes de datos y conjuntos de datos. En el Diseñador de informes, el panel Datos de informe trabaja con el Explorador de soluciones, que incluye orígenes de datos compartidos y conjuntos de datos compartidos como archivos. Los orígenes de datos compartidos y los conjuntos de datos compartidos del panel Datos de informe deben apuntar a los orígenes de datos compartidos y los conjuntos de datos compartidos del Explorador de soluciones. Los elementos del panel Datos de informe contienen después una referencia a los archivos de datos en el Explorador de soluciones. Las propiedades del proyecto determinan si los orígenes de datos compartidos y los conjuntos de datos compartidos se implementan en el servidor de informes o en el sitio de SharePoint. Para más información, vea [Conversión de orígenes de datos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/convert-data-sources-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  Si no ve el panel Datos de informe, haga clic en el área de diseño y, luego, en el menú **Ver** , haga clic en **Datos de informe**. Si el panel Datos de informe está flotando, puede anclarlo. Para más información, vea [Acoplar el panel Datos de informe en el Diseñador de informes &#40;SSRS&#41;](../../reporting-services/tools/dock-the-report-data-pane-in-report-designer-ssrs.md).  
  
  
###  <a name="grouping-pane"></a><a name="bkmk_GroupingPane"></a> Panel de agrupación  
 Use el panel de agrupación para definir los grupos para una región de datos Tablix. Puede definir grupos de filas y grupos de detalles para las tablas, y grupos de filas y columnas para las matrices. No puede usar el panel de agrupación para definir grupos para los gráficos u otras regiones de datos. Para obtener más información, vea [Descripción de los grupos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/understanding-groups-report-builder-and-ssrs.md).  
  
 El panel de agrupación tiene dos modos:  
  
-   **Predeterminado.** Use el cuadro de diálogo **Predeterminado** para presentar todos los grupos de filas y columnas en un formato jerárquico que muestre la relación de grupos primarios, grupos secundarios, grupos adyacentes y grupos de detalles. Los grupos secundarios se muestra por debajo y en el siguiente nivel de sangría que los grupos primarios. Los grupos adyacentes se muestran en el mismo nivel de sangría que los grupos relacionados.  
  
     Use el modo predeterminado para agregar, modificar o eliminar grupos. Para los grupos basados en un único campo de conjunto de datos, puede arrastrar el campo al panel Grupos de filas o Grupos de columnas. Puede insertar el grupo por encima o por debajo de un grupo existente. Para agregar un grupo adyacente, haga clic con el botón secundario en el grupo relacionado y use el menú contextual. Para mostrar qué celdas de Tablix pertenecen a un grupo, seleccione el grupo en el panel de agrupación.  
  
-   **Avanzado.** Use el cuadro de diálogo **Avanzado** para mostrar los miembros de grupos de filas y columnas estáticos y dinámicos de la región de datos Tablix seleccionada.  Debe usar los miembros de grupo para establecer las propiedades que controlan la visibilidad de las filas y columnas asociadas a un grupo o miembro del grupo, o las reglas que los representadores usan para intentar mantener los grupos juntos en una sola página. Los miembros del grupo aparecen en la superficie de diseño como celdas en el grupo de filas y como áreas en el grupo de columnas.  
  
> [!NOTE]  
>  Para cambiar entre los modos **Predeterminado** y **Avanzado** , haga clic con el botón derecho en la flecha abajo, a la derecha del icono **Grupos de columnas** .  
  
 Para obtener más información, vea [Grouping Pane](../../reporting-services/tools/grouping-pane.md).  
  
  
###  <a name="toolbox"></a><a name="bkmk_Toolbox"></a> Cuadro de herramientas  
 El cuadro de herramientas contiene los elementos de informe que se pueden arrastrar a la superficie de diseño. Las regiones de datos son elementos de informe que se usan para organizar los datos en el informe. Tabla, matriz, lista, gráfico, medidor, barras de datos, minigráfico e indicador son regiones de datos. Otros elementos de informe son mapa, cuadro de texto, rectángulo, línea, imagen y subinforme. También pueden aparecer elementos de informe personalizados en esta lista si el administrador del sistema los ha instalado y registrado.  
  
###  <a name="properties-pane"></a><a name="bkmk_PropertiesPane"></a> Panel Propiedades  
 El panel de propiedades es una ventana estándar de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] que muestra nombres y valores de propiedades para el elemento de informe actualmente seleccionado en la superficie de diseño. En la mayoría de los casos, los nombres de propiedades corresponden a los elementos y atributos del archivo de lenguaje RDL (Report Definition Language). Las propiedades más utilizadas pueden establecerse utilizando el cuadro de diálogo Propiedades del elemento seleccionado. Para abrir el cuadro de diálogo correspondiente, haga clic en el botón **Páginas de propiedades** en la barra de herramientas del panel de propiedades. Los usuarios avanzados pueden establecer valores de propiedad directamente en el panel de propiedades.  
  
 Use el panel de propiedades para:  
  
-   Establecer las propiedades del elemento seleccionado en la superficie de diseño. Algunas propiedades proporcionan una lista desplegable de valores. También puede escribir directamente el valor en la celda. En algunas propiedades se incluye una colección de valores, que se indica con el valor **(Colección)** . La mayoría de las propiedades pueden aceptar una expresión; las expresiones complejas se indican mediante el valor **\<Expression>** . Haga clic en **\<Expression>** para abrir el cuadro de diálogo **Expresión**. Para obtener más información, vea [Expression Dialog Box](/previous-versions/sql/).  
  
-   Use los botones de la barra de herramientas del panel de propiedades para cambiar la cuadrícula de la vista por categorías a la vista alfabética. En la vista por categorías, es probable que tenga que expandir una categoría para ver todas las propiedades de la misma. Para abrir el cuadro de diálogo Propiedades de un elemento, haga clic en el botón **Páginas de propiedades** en la barra de herramientas, o bien haga clic con el botón derecho en el elemento y seleccione **Propiedades**.  
  
-   Establecer propiedades para el miembro del grupo seleccionado en el panel de agrupación. Las propiedades de miembro del grupo ayudan a controlar la forma en que las filas de encabezado y pie de grupo se repiten para cada grupo instancias. Para más información, vea [Mostrar encabezados y pies de página con un grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-headers-and-footers-with-a-group-report-builder-and-ssrs.md).  
  
 Para mostrar el panel de propiedades, haga clic en **Ventana de propiedades** en el menú **Ver**. Puede desacoplar este panel y moverlo a otra área de la ventana de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]o mostrarlo como una vista con pestañas en la superficie de diseño.  
  
  
###  <a name="solution-explorer"></a><a name="bkmk_SolutionExplorer"></a> Explorador de soluciones  
 El Explorador de soluciones es un componente estándar de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] que muestra todos los elementos del proyecto. Para un proyecto de servidor de informes, se incluyen las carpetas para organizar los orígenes de datos compartidos, conjuntos de datos compartidos, informes y recursos. Los elementos de la carpeta se alfabetizan al abrir el archivo de solución. Para ver las propiedades de elementos del panel de propiedades, seleccione el elemento.  
  
###  <a name="output"></a><a name="bkmk_Output"></a> Output  
 La ventana de salida muestra los errores de procesamiento cuando se obtiene la vista previa de un informe y los errores de publicación cuando se implementa un informe en un origen de datos compartido.  
  
 Use las ventanas Salida y Esquema de documento para depurar los errores en las expresiones.  
  
  
###  <a name="document-outline"></a><a name="bkmk_DocumentOutline"></a> Esquema de documento  
 La ventana Esquema de documento muestra una lista jerárquica de todos los elementos de informe de la definición de informe. Para abrir el panel Esquema de documento, en el menú **Ver** , seleccione **Otras ventanas** y haga clic en **Ventana de documento**.  
  
 Use el panel Esquema de documento para ayudar a identificar los cuadros de texto y otros elementos de informe por nombre. Cuando se selecciona un elemento en el esquema de diseño, también se selecciona el elemento en la superficie de diseño.  
  
###  <a name="task-list"></a><a name="bkmk_TaskList"></a> Lista de tareas  
 La ventana Lista de tareas muestra errores de compilación para las características no compatibles cuando se importa un informe desde otra aplicación, como [!INCLUDE[msCoName](../../includes/msconame-md.md)] Access.  
  
  
##  <a name="report-designer-design-view"></a><a name="bkmk_ReportDesignerDesignView"></a> Vista de diseño del Diseñador de informes  
 De forma predeterminada, cuando se crea un proyecto de servidor de informes, el Diseñador de informes se abre en la vista de diseño y muestra la superficie de diseño. De forma predeterminada, la superficie de diseño muestra el cuerpo y el fondo del informe.  
  
 El menú contextual del fondo ofrece opciones para agregar un encabezado y un pie de página y, en el menú Ver, para mostrar una regla y el panel de agrupación.  
  
 Use el control de zoom para aumentar o reducir la ampliación del informe.  
  
 Para diseñar un informe, arrastre los elementos del informe desde el cuadro de herramientas a la superficie de diseño y, a continuación, configure sus propiedades y modifique su organización en el informe.  
  
  
##  <a name="report-designer-preview"></a><a name="bkmk_ReportDesignerPreview"></a> Vista previa del Diseñador de informes  
 Use la vista previa para ejecutar el informe y ver el informe representado en el visor de informes. La vista previa almacena los datos del informe localmente en la memoria caché. También puede establecer las propiedades de configuración para ejecutar el informe en la vista de depuración utilizando un explorador.  
  
 Al obtener la vista previa de un informe, el Diseñador de informes se conecta a los orígenes datos del informe, ejecuta consultas de conjunto de datos, almacena los datos en la memoria caché en el equipo local, procesa el informe para combinar datos y diseño y representa el informe. Puede ver el informe en la pestaña Vista previa o configurar las propiedades del proyecto para ver el informe en modo de depuración y verlo directamente en un explorador.  
  
-   **Obtener una vista previa de informes parametrizados.** Al obtener la vista previa de un informe, el informe se procesa automáticamente si todos los parámetros de informe tienen valores predeterminados válidos. Si uno o varios parámetros de informe no tienen un valor predeterminado válido, debe elegir un valor para cada parámetro sin asignar y, a continuación, en la barra de herramientas del informe, hacer clic en **Ver informe**.  
  
-   **Información sobre la memoria caché de datos local** Al generar la vista previa de un informe, el procesador de informes ejecuta todas las consultas de los conjuntos de datos del informe con los valores predeterminados de parámetros actuales y guarda los resultados como un archivo de caché de datos local (.rdl.data). Puede continuar diseñando su informe sin incurrir en la sobrecarga que supone recuperar de nuevo estos datos si no realiza ningún cambio en las consultas del conjunto de datos de informe ni en los parámetros de informe.  
  
-   **Obtener una vista previa del informe mediante el Administrador de configuración y la depuración.** En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], las propiedades del proyecto definen la forma en que deben implementarse y depurarse los informes. Estas propiedades se aplican a todos los informes y orígenes de datos compartidos del proyecto. Para establecer las propiedades del proyecto, en el menú **Proyecto** , haga clic en **Propiedades**. Use estos valores para probar los informes y publicarlos en el servidor de informes.  
  
-   **Supervisar el panel de salida para comprobar si hay mensajes de error.** Cuando se obtiene la vista previa de un informe y el procesador de informes detecta un problema, escribe los mensajes de error en el panel de salida.  
  
  
##  <a name="report-designer-menus"></a><a name="bkmk_ReportDesignerMenus"></a> Menús del Diseñador de informes  
 Cuando un proyecto de diseñador de informes está activo en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], se agregan las siguientes barras de herramientas a la barra de herramientas principal. Los menús del Diseñador de informes solamente están visibles en la vista Diseño.  
  
###  <a name="format-menu"></a><a name="FormatMenu"></a> Menú Formato  
 Cuando se selecciona un elemento en la superficie de diseño, el menú **Formato** incluye las opciones siguientes:  
  
-   **Color de primer plano** Seleccione un color de texto. El color de texto predeterminado es el negro.  
  
-   **Color de fondo** Seleccione un color de fondo para los cuadros de texto y regiones de datos.  
  
-   **Fuente** Especifique si el texto debe mostrarse en negrita, cursiva o subrayado.  
  
-   **Justificar** Especifique si el texto se muestra alineado a la derecha, centrado o alineado a la izquierda.  
  
-   **Alinear** Especifique cómo deben alinearse entre sí los objetos seleccionados dentro del informe.  
  
-   **Igualar tamaño** Ajuste el tamaño de los objetos seleccionados dentro del informe.  
  
-   **Espaciado horizontal** Ajuste el espaciado horizontal entre los objetos seleccionados dentro del informe.  
  
-   **Espaciado vertical** Ajuste el espaciado vertical entre los objetos seleccionados dentro del informe.  
  
-   **Centrar en el formulario** Centre el objeto seleccionado vertical y horizontalmente con respecto a la ventana del Diseñador de informes.  
  
-   **Ordenar** Mueva los objetos seleccionados a un primer plano o al fondo.  
  
###  <a name="report-menu"></a><a name="ReportMenu"></a> Menú Informe  
 Cuando la superficie de diseño del informe tiene el foco, el menú **Informe** incluye las siguientes opciones:  
  
-   **Propiedades del informe** Seleccione esta opción para abrir el cuadro de diálogo **Propiedades del informe** . En este cuadro de diálogo pueden asignarse propiedades generales al informe, como el nombre de autor y el espaciado de cuadrícula, y especificar propiedades para el diseño del informe, como el número de columnas y el tamaño de página. También puede incluir código personalizado, referencias a ensamblados y clases, así como los nombres de los elementos del resultado, la transformación y los esquemas de los datos.  
  
-   **Vista** Cambie entre las dos pestañas del diseñador de informes: Diseño y Vista previa.  
  
-   **Encabezado de página** Agregue un encabezado de página al informe o elimine un encabezado de página del informe. Al eliminar un encabezado de página, se eliminan todos los elementos del encabezado de página.  
  
-   Agregar **Pie de página** Agregue un pie de página al informe o elimine un pie de página del informe. Al eliminar un pie de página, se eliminan todos los elementos del pie de página.  
  
-   **Panel de agrupación** Muestra y oculta el panel de agrupación.  
  
###  <a name="view-menu"></a><a name="ViewMenu"></a> Menú Ver  
 Use el menú **Ver** para mostrar las ventanas y las barras de herramientas del Diseñador de informes.  
  
-   **Lista de errores** Use esta opción para mostrar los errores detectados al publicar u obtener la vista previa de un informe.  
  
-   **Resultados** Use esta opción para mostrar los errores detectados al publicar o procesar un informe, o bien para obtener información sobre los errores de expresión cuando en un informe se muestra el texto "#Error".  
  
-   **Ventana de propiedades** Use esta opción para mostrar los valores de propiedades del elemento de informe actualmente seleccionado en la superficie de diseño. Para ver las propiedades de los elementos de informe anidados, debe hacer clic varias veces en un elemento de informe para recorrer la jerarquía de un elemento de informe y sus miembros anidados. Compruebe el nombre del elemento que aparece en la parte superior del panel de propiedades para ver las propiedades de qué elemento de informe se muestran.  
  
-   **Cuadro de herramientas** Use esta opción para mostrar el cuadro de herramientas.  
  
-   **Otras ventanas** Use esta opción para mostrar el panel siguiente:  
  
    -   **Esquema de documento** Use esta opción para mostrar una vista jerárquica de los elementos de informe y sus colecciones de cuadros de texto en un informe.  
  
-   **Barras de herramientas** Use esta opción para mostrar las barras de herramientas que admiten las características del Diseñador de informes, incluidas las barras de herramientas **Bordes del informe** y **Formato del informe**. Para obtener más información, vea [Barras de herramientas del Diseñador de informes](#bkmk_ReportDesignerToolbars).  
  
-   **Datos de informe** Use esta opción para mostrar el panel Datos de informe, donde puede agregar parámetros de informe, orígenes de datos, conjuntos de datos e imágenes.  
  
###  <a name="project-menu"></a><a name="ProjectMenu"></a> Menú Proyecto  
 Use el menú **Proyecto** para administrar orígenes de datos compartidos e informes en un proyecto. Al agregar o quitar elementos en el proyecto, la presentación jerárquica de los elementos del proyecto en el Explorador de soluciones se actualiza de forma automática.  
  
-   **Agregar nuevo elemento** Agregue un nuevo origen de datos compartido o un nuevo informe al proyecto.  
  
-   **Agregar elemento existente** Agregue un origen de datos compartido existente o un informe existente al proyecto.  
  
-   **Importar informes** Importe informes desde otra aplicación como, por ejemplo, Microsoft Access.  
  
-   **Excluir del proyecto** Excluya elementos del proyecto. Esta opción no elimina el elemento de su sistema de archivos.  
  
-   **Mostrar todos los archivos** Muestre todos los archivos de un proyecto.  
  
-   **Actualizar elementos del cuadro de herramientas de proyecto** Actualice la caché del cuadro de herramientas cuando instale nuevos elementos de informe personalizados en el proyecto.  
  
-   **Propiedades** Abre el cuadro de diálogo **Páginas de propiedades** de este proyecto. Para más información, vea [Páginas de propiedades del proyecto (cuadro de diálogo)](../../reporting-services/tools/project-property-pages-dialog-box.md).  
  
  
##  <a name="report-designer-toolbars"></a><a name="bkmk_ReportDesignerToolbars"></a> Barras de herramientas del Diseñador de informes  
 El Diseñador de informes proporciona las siguientes barras de herramientas especializadas que se pueden usar para diseñar informes:  
  
-   **Informe** Agregue un encabezado o pie de página, establezca propiedades de informe, alterne la regla o el panel de agrupación o use el zoom para cambiar la vista del informe.  
  
-   **Bordes del informe** Establezca el color, estilo y ancho de todas las líneas seleccionadas y de los bordes de todos los elementos de informe seleccionados.  
  
-   **Formato del informe** Establezca el formato de los elementos de informe seleccionados. En el caso de los cuadros de texto, es posible cambiar los siguientes tipos de formato utilizando la barra de herramientas: propiedades de la fuente y color del texto, color de fondo y justificación del texto.  
  
-   **Diseño** Establezca el orden de dibujo de los elementos de informe y las celdas de combinación dentro de una región de datos.  
  
-   **Estándar** Abra o guarde proyectos, muestre ventanas y seleccione la configuración de depuración.  
  
 Utilice el menú **Ver** para controlar si deben mostrarse estas barras de herramientas. Puede haber otras barras de herramientas de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] deshabilitadas si su funcionalidad no se aplica a las características del Diseñador de informes.  
  

##  <a name="source-control"></a><a name="bkmk_SourceControl"></a> Control de código fuente  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] se puede integrar con complementos de origen. Use las páginas Proyectos y Soluciones del cuadro de diálogo **Opciones** para especificar el complemento y configurar las propiedades.  
  
##  <a name="custom-report-templates"></a><a name="bkmk_CustomReportTemplates"></a> Plantillas de informe personalizadas  
 Para usar informes personalizados como plantillas para los informes nuevos, basta con copiarlas en la carpeta ReportProject del equipo en el que [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] está instalado. De forma predeterminada, esta carpeta se encuentra en la ubicación siguiente: `<drive>:\Program Files\Microsoft Visual Studio 14.0\Common7\IDE\Private Assemblies\ProjectItems\ReportProject`. Al agregar un elemento nuevo al proyecto de informe, el informe personalizado aparece en el panel de plantillas.  
  
 También puede agregar estilos personalizados al Asistente para informes.  
  
  
##  <a name="command-line-support-for-sql-server-data-tools"></a><a name="bkmk_CommandLineSupportForssdt"></a> Compatibilidad de la línea de comandos con SQL Server Data Tools  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] está basado en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] y en la aplicación devenv.exe subyacente. Para poder usar estas opciones, debe establecer valores válidos para los dos elementos siguientes:  
  
-   Propiedades de proyecto de OverwriteDataSources, TargetDataSourceFolder, TargetReportFolder y TargetServerURL.  
  
-   Al menos un conjunto de propiedades de configuración, por ejemplo, Debug o Release.  
  
 Para obtener más información, vea [Publishing Data Sources and Reports](../../reporting-services/reports/publishing-data-sources-and-reports.md).  
  
 En un proyecto de servidor de informes, puede especificar las opciones siguientes desde la línea de comandos:  
  
-   **/deploy** Se implementan los informes con las propiedades de proyecto especificadas en un archivo de configuración. Por ejemplo, el comando siguiente implementa los informes especificados por el archivo de la solución Reports.sln utilizando la opción de configuración Release que se especifica en las propiedades del proyecto:  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /deploy "Release"  
    ```  
  
-   **/build** Se genera el archivo de solución, pero no se implementa. Por ejemplo, el comando siguiente genera los informes especificados por el archivo de la solución Reports.sln utilizando la opción de configuración Debug que se especifica en las propiedades del proyecto:  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug"  
    ```  
  
-   **/out** Se redirige el resultado al archivo especificado que se genera al compilar una solución. Por ejemplo, el comando siguiente redirige la salida de la compilación del ejemplo anterior a un archivo denominado mybuildlog.txt.  
  
    ```  
    devenv.exe "C:\Users\MyUser\Documents\Visual Studio 2015\Projects\Reports\Reports.sln" /build "Debug" /out mybuildlog.txt  
    ```  
  
##  <a name="keyboard-shortcuts-in-reporting-services"></a><a name="bkmk_KeyboardShortcuts"></a> Métodos abreviados de teclado en Reporting Services  
 Use los métodos abreviados de teclado para:  
  
-   Controlar las ventanas y los modos de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]:  
  
    |Descripción|Combinación de teclas|  
    |-----------------|---------------------|  
    |Compilar el proyecto seleccionado|CTRL+MAYÚS+B|  
    |Mostrar la ventana Propiedades|F4|  
    |Ventana Mostrar datos|CTRL+Alt+D|  
    |Iniciar la depuración|F5|  
    |Pasar de una ventana abierta a la siguiente|F6|  
  
-   Controlar los elementos en la superficie de diseño de informes:  
  
    |Descripción|Combinación de teclas|  
    |-----------------|---------------------|  
    |Mover el foco de un elemento de informe al elemento de informe siguiente|TAB|  
    |Mover el elemento de informe seleccionado|Teclas de dirección|  
    |Desplazar el elemento de informe seleccionado|CTRL+teclas de dirección|  
    |Aumentar o reducir el tamaño del elemento de informe seleccionado|CTRL+MAYÚS+ teclas de dirección|  
    |En un cuadro de texto, llevar el cursor al principio del texto de presentación que está visible|CTRL+INICIO|  
    |En un cuadro de texto, llevar el cursor al final del texto de presentación que está visible|CTRL+FIN|  
    |En un cuadro de texto, seleccionar el texto desde la posición del cursor actual hasta el principio del texto de presentación que está visible|MAYÚS+INICIO|  
    |En un cuadro de texto, seleccionar el texto desde la posición del cursor actual hasta el final del texto de presentación que está visible|MAYÚS+FIN|  
    |En un cuadro de texto, seleccionar el texto desde la posición del cursor actual hasta el principio de la expresión|CTRL+MAYÚS+INICIO|  
    |En un cuadro de texto, seleccionar el texto desde la posición del cursor actual hasta el final de la expresión|CTRL+MAYÚS+FIN|  
    |Abrir el menú contextual para el elemento de informe seleccionado|MAYÚS+F10+tecla de propiedad en teclados más recientes|
  
## <a name="next-steps"></a>Pasos siguientes

[Descargar SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md)
[Explorador de soluciones](../../ssms/solution/solution-explorer.md)   
[Informes de Reporting Services](../../reporting-services/reports/reporting-services-reports-ssrs.md)   
[Lenguaje RDL (Report Definition Language)](../../reporting-services/reports/report-definition-language-ssrs.md)   
[Implementación y compatibilidad de versiones en SQL Server Data Tools](../../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).