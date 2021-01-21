---
title: 'Informes móviles de SQL Server: tutorial completo'
description: Obtenga información sobre cómo crear informes para dispositivos móviles en el Publicador de informes móviles de SQL Server, guardar informes en el portal web de Reporting Services y verlos las aplicaciones móviles de Power BI.
ms.date: 12/07/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.custom: seodec18
ms.topic: conceptual
ms.assetid: e198575e-b154-4342-b944-2bf19ec49bfd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: df09e4168790a4607de19348f50e76a4fbd0f383
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596538"
---
# <a name="sql-server-mobile-reports-end-to-end-walk-through"></a>Informes móviles de SQL Server: tutorial completo
Recorra la creación de informes móviles para cualquier tamaño de pantalla con [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-long.md)] en el portal web de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] y véalos en las aplicaciones móviles de Power BI.

Cree informes móviles en una superficie de diseño con cuadrícula ajustable de filas y columnas y elementos flexibles de informes móviles. Conéctese a una variedad de orígenes de datos locales o cargue libros de Excel para crear informes móviles. A continuación, guarde los informes en un portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] y véalos en un explorador o en las aplicaciones móviles de Power BI.  
  
Este artículo le enseñará a:   
  
- Crear un conjunto de datos y un origen de datos compartidos en el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , con la base de datos de AdventureWorks como un origen de datos de ejemplo.  
- Crear un informe móvil de Reporting Services en el [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]  
- Publicar el informe móvil en el portal de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
- Ver el informe móvil en la aplicación móvil de Power BI.  
  
## <a name="before-we-start"></a>Antes de empezar  
Para poder continuar, necesita estos productos:  
  
* Para crear orígenes de datos y KPI, además de publicar informes móviles y conjuntos de datos, tendrá que acceder a un [servidor de informes de Reporting Services en modo nativo](../install-windows/install-reporting-services-native-mode-report-server.md).  
* Para crear conjuntos de datos compartidos, [instale el Generador de informes](../install-windows/install-report-builder.md).  
* Para crear informes móviles, [instale Publicador de informes móviles de SQL Server](../reporting-services-features-and-tasks-ssrs.md).  
* [Bases de datos de ejemplo AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases).  
*  O BIEN: Base de datos de ejemplo de World Wide Importers, disponible en la página [Ejemplos de Microsoft SQL Server](../../samples/sql-samples-where-are.md).
* Para ver el resultado: 
  *   [Suscríbase al servicio Power BI](https://go.microsoft.com/fwlink/?LinkID=513879) y
  *  [Descargue la aplicación móvil de Power BI](/power-bi/consumer/mobile/mobile-apps-for-mobile-devices) a su dispositivo móvil: dispositivo iOS, teléfono Android o dispositivo Windows 10.  

  
## <a name="create-a-shared-data-source"></a>Crear un origen de datos compartido  
  
Puede crear un origen de datos compartido para los informes móviles desde cualquiera de los orígenes de datos compatibles con Reporting Services. Vea la [lista de orígenes de datos admitidos](../report-data/data-sources-supported-by-reporting-services-ssrs.md).  
  
1. En el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , haga clic en **Nuevo** > **Origen de datos**.  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
3. Escriba la información del origen de datos > **Aceptar**.  
  
    De forma predeterminada, los orígenes de datos no se muestran en el portal.    
   
5. Para ver los orígenes de datos, haga clic en **Mostrar** > **Origen de datos**.  
  
   ![PBI_SSMRP_DisplayDataSources](../../reporting-services/mobile-reports/media/pbi-ssmrp-displaydatasources.png)  
   
6. Ahora puede ver el origen de datos en el portal de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
  
   ![PBI_SSMRP_PortlDataSource](../../reporting-services/mobile-reports/media/pbi-ssmrp-portldatasource.png)  
  
Lea más información sobre los [orígenes de datos compartidos en Reporting Services](../report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
   
## <a name=""></a><a name="shared-dataset">Creación de un conjunto de datos compartido</a>  
  
Use una herramienta de cliente de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] existente, como Diseñador de informes en [!INCLUDE[ssBIDevStudioFull_md](../../includes/ssbidevstudiofull-md.md)], para crear el conjunto de datos compartido.  Este tutorial usa [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)]. [Instale el Generador de informes](../install-windows/install-report-builder.md) o ábralo desde el portal web. Creará tres conjuntos de datos, uno para: el valor del KPI, la tendencia del KPI y otro con más campos para el informe móvil de Reporting Services.     
  
1. En el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , haga clic en **Nuevo** > **Informe paginado** para iniciar [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)   
2. Haga clic en **Nuevo conjunto de datos**.  
  
   ![PBI_SSMRP_RBNewDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-rbnewdataset.png)  
   
3. Haga clic en **Examinar otros orígenes de datos**.  
   
4. En el campo de nombre, escriba el nombre del servidor donde guardó el origen de datos, con el formato siguiente:   
   
   Nombre: https://*localhost*/ReportServer  
   Elementos de tipo: orígenes de datos (*.rsds)  
   
5. Haga clic en **Abrir** y desplácese hasta el origen de datos que creó en ese servidor.  
   
6. Seleccione el origen de datos y haga clic nuevamente en **Abrir** .    
  
7. Diseñe el conjunto de datos en [!INCLUDE[PRODUCT_NAME](../../includes/ssrbnoversion.md)].  
  
   ![PBI_SSMRP_RB_QueryDesignr600](../../reporting-services/mobile-reports/media/pbi-ssmrp-rb-querydesignr600.png)  
   
8. Cuando haya terminado, guarde el conjunto de datos en el servidor de informes de [!INCLUDE[PRODUCT_NAME](../../includes/ssrs.md)] .    
   
Ahora puede usar el conjunto de datos como base para sus KPI e informes móviles.  Puede crear varios conjuntos de datos con el mismo origen de datos. También puede crear varios KPI e informes móviles a partir de estos conjuntos de datos compartidos.   
  
## <a name=""></a><a name="create-KPI">Crear un KPI</a>  
Puede crear KPI directamente en el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .    
  
1. En la esquina superior derecha del portal web, haga clic en **Nuevo** > **Nuevo KPI**.   
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)  
      
   En la pantalla de creación de KPI, puede escribir manualmente los valores o usar un conjunto de datos compartido.    
2. Cambie **Valor** de **Establecer manualmente** a **Campo de conjunto de datos**.  
   
   ![PBI_SSMRP_KPI_DatasetField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpi-datasetfield.png)  
   
3. Haga clic en los puntos suspensivos ( **...** ) de la casilla **Seleccionar campo de conjunto de datos** y seleccione un conjunto de datos del paso anterior.  
   
   ![PBI_SSMRP_KPIPickDataset](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickdataset.png)  
   
4. Elija el campo del conjunto de datos.    
   
   ![PBI_SSMRP_KPIPickField](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipickfield.png)  
     
5. Elija la agregación de su preferencia. Los KPI solo muestran un número, por lo que se agregará el campo para que se muestre ese número.

   ![Captura de pantalla de la sección Elegir un campo de AWSalesYTD que muestra la sección Agregación promedio.](../../reporting-services/mobile-reports/media/reporting-services-kpi-pick-aggregation.png)

6. Haga clic en **OK**.

7. En la casilla **Conjunto de tendencias** , haga clic en **Tendencia de conjunto de datos**.  
  
6. En la casilla **Seleccionar tendencia de conjunto de datos** , haga clic en los puntos suspensivos ( **...** )  
   
7. Seleccione un campo y haga clic en **Aceptar**.  

   ![PBI_SSMRP_KPIPickTrend](../../reporting-services/mobile-reports/media/pbi-ssmrp-kpipicktrend.png)  
  
8. Asigne un nombre a KPI, elija un tipo de visualización y haga clic en **Crear**.   
  
   El KPI aparece en el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] .  
   
    ![PBI_SSMRP_NewKPI](../../reporting-services/mobile-reports/media/pbi-ssmrp-newkpi.png)  
    
## <a name=""></a><a name="create-mobile-report">Creación de un informe móvil de Reporting Services</a>  
   
Para crear un informe móvil de Reporting Services, [instale Publicador de informes móviles de SQL Server](../reporting-services-features-and-tasks-ssrs.md)o inícielo desde el portal web de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] . 

Cuando se abra [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)]por primera vez, verá un lienzo en blanco donde podrá crear el informe móvil. Para empezar, puede crear objetos visuales primero o puede comenzar con los datos. Si crea primero los objetos visuales, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] genera automáticamente datos simulados asociados al informe y cambia dinámicamente a medida que cambian sus selecciones visuales. Inténtelo.   
  
## <a name="start-with-the-visuals"></a>Comience con los objetos visuales  
  
1. En el portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] , haga clic en **Nuevo** > **Informe móvil** para iniciar [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)].  
  
   ![PBI_SSMRP_NewMenu](../../reporting-services/mobile-reports/media/pbi-ssmrp-newmenu.png)

   [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] se abre en la cuadrícula de diseño maestra.  
  
2. En la ficha **Diseño** , desplácese hacia abajo hasta la sección Gráficos.  
  
   ![PBI_SSMRP_LayoutTabCharts2](../../reporting-services/mobile-reports/media/pbi-ssmrp-layouttabcharts2.png)  
  
2. Arrastre el **Gráfico de rectángulos** a la cuadrícula y, luego, arrastre la esquina inferior derecha para que tenga tres columnas de ancho y tres columnas de alto.  
  
   ![PBI_SSMRP_TreeMap](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemap.png)  
  
3. Puede ver las propiedades visuales en la parte inferior. Es posible que deba desplazarse hacia los lados para verlas todas.   
  
   ![PBI_SSMRP_TreeMapVisProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapvisprops.png)  
  
4. Con el gráfico de rectángulos seleccionado, seleccione la pestaña **Datos** en la esquina superior izquierda.   
  
   Ahora puede ver los valores y campos simulados que [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] generó, además de ver qué representan el tamaño y el color del gráfico de rectángulos.  
  
   ![PBI_SSMRP_TreeMapDataProps](../../reporting-services/mobile-reports/media/pbi-ssmrp-treemapdataprops.png)  
  
6. Haga clic en la pestaña **Diseño**.  
  
7. Haga clic en el engranaje Opciones ![PBI_SSMRP_Cog](../../reporting-services/mobile-reports/media/pbi-ssmrp-cog.png) que se encuentra en la esquina superior derecha del gráfico de rectángulos para ver el menú que contiene.   
  
   ![PBI_SSMRP_OptionsWheel](../../reporting-services/mobile-reports/media/pbi-ssmrp-optionswheel.png)  
  
8. Haga clic en la flecha que aparece en el centro de la rueda para cerrarla.  
  
## <a name="add-your-own-data"></a>Agregar sus propios datos  
  
1. Cambie a la pestaña **Datos** .    
   
2. Para agregar sus propios datos, haga clic en **Agregar datos** en la esquina superior derecha y vaya a sus datos.    
  
3. Podría usar los datos de un libro de Excel local pero, en este caso, se usa el conjunto de datos compartido del portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . Aparecerá un mensaje de "Servidor agregado".  
  
4. Seleccione el servidor y, luego, el conjunto de datos que creó.  
   
3. Nuevamente en la pestaña **Datos** , en el panel **Propiedades de datos** , cambie **El tamaño representa**, **El color representa** y las otras propiedades a los campos de sus propios datos. 
   
   *  Los campos **El tamaño representa**, **El color representa** y **Valor central personalizado** deben tener valores numéricos. 
   *  **Agrupar por** es una categoría; por lo tanto, es un campo de texto.
   
   ![Captura de pantalla de la sección Propiedades de datos.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-data-properties.png)
   
6. Seleccione **Vista previa** para ver el gráfico de rectángulos actualizados con los datos.  

## <a name="add-a-gauge-to-your-mobile-report"></a>Agregar un medidor al informe móvil

Agreguemos un medidor para ver una comparación de las ventas anuales hasta la fecha y las ventas del último año con el mismo conjunto de datos.

1. En la ficha Diseño, arrastre un semicírculo al lienzo, póngalo debajo del gráfico de rectángulos y arrastre la esquina inferior derecha para tener tres cuadrados de ancho por dos de alto. 

2. Nuevamente, empieza por datos simulados. 

   Tenga en cuenta que en **Propiedades de los elementos visuales**, de manera predeterminada **los valores altos son mejores** y la **Etiqueta de tipo delta** es un **porcentaje del destino**. Tiene **detenciones de rango** predeterminadas que puede cambiar, pero no lo haremos por el momento.

   ![Captura de pantalla de la sección Establecer rangos de las propiedades visuales de anillo del informe móvil.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-visual-properties.png)
   
3. En la pestaña **Datos** , seleccione la tabla con los datos y elija el campo **Valor principal** , además del campo con el que desea compararlo en **Valor de comparación**.

4. Puede elegir agregaciones distintas para poner un número en **Valor principal** y otro en **Valor de comparación**. De manera predeterminada, es una suma.

   ![Captura de pantalla de las Opciones o Valor de comparación.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-sum.png)

5. Seleccione **Vista previa** para saber cómo se verá. 

   ![Captura de pantalla de la vista previa de anillo del informe móvil.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-donut-preview.png)

## <a name="add-a-selection-list-as-a-filter"></a>Agregar una lista de selección como filtro

Las listas de selección funcionan como segmentaciones en Power BI y Excel. Es posible agregar una para filtrar los otros objetos visuales del informe móvil.

1. En la pestaña **Diseño** , arrastre una lista de selección a la derecha del gráfico de rectángulos y, luego, arrastre la esquina inferior derecha para que tenga dos cuadrados de ancho y la misma altura del lienzo, es decir, cinco cuadrados. 

   ![Captura de pantalla de la lista de selección del informe móvil.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list.png)

2. En la pestaña **Datos** , **Propiedades de datos**, establezca **Claves** y **Etiquetas** en el campo de los datos según el cual desea establecer el filtro.

   ![Captura de pantalla de la sección de propiedades de datos en la lista de selección del informe móvil.](../../reporting-services/mobile-reports/media/ssrs-mobile-report-selection-list-data-properties.png)
   
## <a name="create-a-mobile-report-for-phones"></a>Crear un informe móvil para teléfonos  
  
Ahora que creó objetos visuales en el diseño maestro, puede crear un informe móvil con un diseño específicamente optimizado para los usuarios con teléfono.    
  
1. En la esquina superior derecha, haga clic en el icono del lienzo > **Teléfono**.  
  
2. En la pestaña Diseño, en **Instancias de control**, verá los dos gráficos que creó.   
  
3. Arrastre el gráfico de rectángulos al lienzo de teléfono para que tenga cuatro columnas de ancho y tres filas de alto.  
  

## <a name="save-your-mobile-report"></a>Guardar el informe móvil  
Puede guardar localmente el informe o en un portal web de [!INCLUDE[PRODUCT_NAME](../../includes/ssrsnoversion.md)] . Si lo guarda localmente, [!INCLUDE[PRODUCT_NAME](../../includes/ss-mobilereptpub-short.md)] lo guarda con datos en caché, por lo que puede abrirlo y seguir trabajando en él. Sin embargo, no podrá verlo en un dispositivo móvil.   
  
1. Haga clic en el icono para guardar en la esquina superior izquierda.   
   
2. Si desea compartirlo con otros usuarios y verlo en un dispositivo móvil, haga clic en **Guardar en el servidor**.  
  
3. En el servidor, vaya a la carpeta donde desea guardar el informe móvil.  
  
4. Haga clic en **Elegir carpeta** > **Guardar**.  
  
   Verá un mensaje que confirma que se guardó el informe.  
    
   Una vez que lo guarde, puede volver al portal y ver una miniatura del informe móvil.   
    
5. Pulse la miniatura para ver el informe en el portal web.  
  
## <a name="view-your-report-on-the-web-portal"></a>Ver el informe en el portal web

  
## <a name="view-your-report-on-a-mobile-device"></a>Ver el informe en un dispositivo móvil   
  
Para ver el informe de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] , debe hacer lo siguiente:

*  [Suscríbase en el servicio Power BI](https://go.microsoft.com/fwlink/?LinkID=513879), si todavía no tiene una cuenta.
*  [Descargue la aplicación móvil de Power BI](https://powerbi.microsoft.com/documentation/powerbi-power-bi-apps-for-mobile-devices/) a su dispositivo móvil.  

### <a name="view-your-mobile-report"></a>Ver el informe móvil
  
1.  Abra la aplicación de Power BI en su dispositivo móvil e inicie sesión.  
    
2.  Si desea ver los KPI y los informes móviles de Reporting Services, pulse **Reporting Services**.  
![PBI_iPad_GetStartedSm](../../reporting-services/mobile-reports/media/pbi-ipad-getstartedsm.png)  
  
3. Pulse el icono de opciones ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) que se encuentra en la esquina superior izquierda y, luego, pulse **Conectarse al servidor**.  
  
   ![PBI_iPad_SSMRP_ConnectCrop](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcrop.png)  
  
4. Asigne un nombre al servidor y rellene la dirección del servidor, su correo electrónico y contraseña según este formato:  
  
   ![PBI_iPad_SSMRP_ConnectContoso](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-connectcontoso.png)   
  
5.  Ahora verá el servidor en la barra de navegación de la izquierda.  
  
    ![PBI_iPad_SSMRP_LeftNavBiggr](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-leftnavbiggr.png)  
      
> [!TIP]
> Pulse el icono de opciones ![PBI_iPad_OptionsIcon](../../reporting-services/mobile-reports/media/pbi-ipad-optionsicon.png) en cualquier momento para pasar de los informes móviles de Reporting Services del portal web de Reporting Services a los paneles del servicio Power BI.  

  
## <a name="view-kpis-and-mobile-reports-in-the-power-bi-app"></a>Ver KPI e informes móviles en la aplicación de Power BI  
  
Pulse la pestaña **KPI** o **Informes móviles** .   
  
![PBI_iPad_SSMRP_Portal](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-portal.png)  
  
- Pulse un KPI para verlo en el modo de foco.  
  
    ![PBI_iPad_SSMRP_Tile](../../reporting-services/mobile-reports/media/pbi-ipad-ssmrp-tile.png)  
  
- Pulse un informe móvil para abrirlo e interactuar con él en la aplicación de Power BI.  
  
Los KPI y los informes móviles se muestran en las mismas carpetas en las que están en el portal web de Reporting Services.   
  
## <a name="see-also"></a>Consulte también  
 
-  Visualización [de informes y KPI locales del servidor de informes en la aplicaciones móviles de Power BI](/power-bi/consumer/mobile/mobile-app-ssrs-kpis-mobile-on-premises-reports) para dispositivos iOS y Android
-  Visualización [de informes móviles y KPI de Reporting Services (SSRS) en la aplicación móvil de Power BI para Windows 10](https://powerbi.microsoft.com/documentation/powerbi-mobile-win10-kpis-mobile-reports/)    
  
