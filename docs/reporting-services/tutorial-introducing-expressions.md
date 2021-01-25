---
title: 'Tutorial: Introducción a las expresiones | Microsoft Docs'
description: Aprenda a usar expresiones con operadores y funciones comunes para crear informes paginados de Reporting Services eficaces y flexibles.
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 2d05ef4c-5f91-48b2-8795-f0a201a0b3cc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 938ac8fc2e006bc74aab59955374f1b012b66b68
ms.sourcegitcommit: fc24f7ecc155d97e789676fffe55e45840fcb088
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620281"
---
# <a name="tutorial-introducing-expressions"></a>Tutorial: Introducción a las expresiones
En este tutorial de [!INCLUDE[ssRBnoversion_md](../includes/ssrbnoversion.md)] , se usan expresiones con operadores y funciones comunes para crear informes paginados eficaces y flexibles de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] . 

Escribirá expresiones que concatenan valores de nombres, buscan valores en un conjunto de datos independiente, muestran diferentes colores según los valores de los campos, etc.  
  
Se trata de un informe con bandas con filas que alternan entre el blanco y un color. El informe incluye un parámetro para seleccionar el color de las filas que no son blancas.  
  
En esta ilustración se muestra un informe similar al que creará.  
  
![Captura de pantalla de un informe de Report Builder en el que se usan expresiones.](../reporting-services/media/report-builder-expression-tutorial-in-browser.png) 
  
Tiempo estimado para completar este tutorial: 30 minutos.  
  
## <a name="requirements"></a>Requisitos  
Para obtener información sobre los requisitos, vea [Requisitos previos para los tutoriales &#40;Generador de informes&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="1-create-a-table-report-and-dataset-from-the-table-or-matrix-wizard"></a><a name="Setup"></a>1. Crear un informe de tabla y un conjunto de datos con el asistente para tablas o matrices  
En esta sección, creará un informe de tabla, un origen de datos y un conjunto de datos. Cuando distribuya la tabla, incluirá solo unos cuantos campos. Después de completar el asistente, agregará manualmente las columnas. El asistente facilita la distribución de la tabla.  
  
> [!NOTE]  
> En este tutorial, la consulta contiene los valores de datos, de forma que no necesita un origen de datos externo. Esto hace que la consulta requiera bastante tiempo. En un entorno empresarial, la consulta no contendría los datos. Esto es solo con fines de aprendizaje.  
  
### <a name="to-create-a-table-report"></a>Para crear un informe de tabla  
  
1.  [Inicie el Generador de informes](../reporting-services/report-builder/start-report-builder.md) desde el equipo, el portal web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] o el modo integrado de SharePoint.  
  
    Se abre el cuadro de diálogo **Nuevo informe o conjunto de datos** .  
  
    Si no ve el cuadro de diálogo **Nuevo informe o conjunto de datos**, vaya al menú **Archivo** > **Nuevo**.  
  
2.  En el panel de la izquierda, compruebe que está seleccionada la opción **Nuevo informe** .  
  
3.  En el panel de la derecha, haga clic en **Asistente para tabla o matriz**.  
  
4.  En la página **Elegir un conjunto de datos** , haga clic en **Crear un conjunto de datos** > **Siguiente**.  
  
6.  En la página **Elegir una conexión a un origen de datos** , seleccione un origen de datos del tipo **SQL Server**. Seleccione un origen de datos en la lista o vaya al servidor de informes para seleccionar uno.  

    > [!NOTE]  
    > El origen de datos que elija no es importante mientras tenga los permisos adecuados. No está recibiendo datos del origen de datos. Para obtener más información, consulte [Maneras alternativas de obtener una conexión de datos &#40;Generador de informes&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
7.  Haga clic en **Next**.  
  
8.  En la página **Diseñar una consulta** , haga clic en **Editar como texto**.  
  
9. Pegue la siguiente consulta en el panel de consulta:  
  
    ```  
    SELECT 'Lauren' AS FirstName,'Johnson' AS LastName, 'American Samoa' AS StateProvince, 1 AS CountryRegionID,'Female' AS Gender, CAST(9996.60 AS money) AS YTDPurchase, CAST('2015-6-10' AS date) AS LastPurchase  
    UNION SELECT'Warren' AS FirstName, 'Pal' AS LastName, 'New South Wales' AS StateProvince, 2 AS CountryRegionID, 'Male' AS Gender, CAST(5747.25 AS money) AS YTDPurchase, CAST('2015-7-3' AS date) AS LastPurchase  
    UNION SELECT 'Fernando' AS FirstName, 'Ross' AS LastName, 'Alberta' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(9248.15 AS money) AS YTDPurchase, CAST('2015-10-17' AS date) AS LastPurchase  
    UNION SELECT 'Rob' AS FirstName, 'Caron' AS LastName, 'Northwest Territories' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(742.50 AS money) AS YTDPurchase, CAST('2015-4-29' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Bailey' AS LastName, 'British Columbia' AS StateProvince, 3 AS CountryRegionID, 'Male' AS Gender, CAST(1147.50 AS money) AS YTDPurchase, CAST('2015-6-15' AS date) AS LastPurchase  
    UNION SELECT  'Bridget' AS FirstName, 'She' AS LastName, 'Hamburg' AS StateProvince, 4 AS CountryRegionID, 'Female' AS Gender, CAST(7497.30 AS money) AS YTDPurchase, CAST('2015-5-10' AS date) AS LastPurchase  
    UNION SELECT 'Alexander' AS FirstName, 'Martin' AS LastName, 'Saxony' AS StateProvince, 4 AS CountryRegionID, 'Male' AS Gender, CAST(2997.60 AS money) AS YTDPurchase, CAST('2015-11-19' AS date) AS LastPurchase  
    UNION SELECT 'Yolanda' AS FirstName, 'Sharma' AS LastName ,'Micronesia' AS StateProvince, 5 AS CountryRegionID, 'Female' AS Gender, CAST(3247.95 AS money) AS YTDPurchase, CAST('2015-8-23' AS date) AS LastPurchase  
    UNION SELECT 'Marc' AS FirstName, 'Zimmerman' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1200.00 AS money) AS YTDPurchase, CAST('2015-11-16' AS date) AS LastPurchase  
    UNION SELECT 'Katherine' AS FirstName, 'Abel' AS LastName, 'Moselle' AS StateProvince, 6 AS CountryRegionID, 'Female' AS Gender, CAST(2025.00 AS money) AS YTDPurchase, CAST('2015-12-1' AS date) AS LastPurchase  
    UNION SELECT 'Nicolas' as FirstName, 'Anand' AS LastName, 'Seine (Paris)' AS StateProvince, 6 AS CountryRegionID, 'Male' AS Gender, CAST(1425.00 AS money) AS YTDPurchase, CAST('2015-12-11' AS date) AS LastPurchase  
    UNION SELECT 'James' AS FirstName, 'Peters' AS LastName, 'England' AS StateProvince, 12 AS CountryRegionID, 'Male' AS Gender, CAST(887.50 AS money) AS YTDPurchase, CAST('2015-8-15' AS date) AS LastPurchase  
    UNION SELECT 'Alison' AS FirstName, 'Nath' AS LastName, 'Alaska' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(607.50 AS money) AS YTDPurchase, CAST('2015-10-13' AS date) AS LastPurchase  
    UNION SELECT 'Grace' AS FirstName, 'Patterson' AS LastName, 'Kansas' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(1215.00 AS money) AS YTDPurchase, CAST('2015-10-18' AS date) AS LastPurchase  
    UNION SELECT 'Bobby' AS FirstName, 'Sanchez' AS LastName, 'North Dakota' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(6191.00 AS money) AS YTDPurchase, CAST('2015-9-17' AS date) AS LastPurchase  
    UNION SELECT 'Charles' AS FirstName, 'Reed' AS LastName, 'Nebraska' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8772.00 AS money) AS YTDPurchase, CAST('2015-8-27' AS date) AS LastPurchase  
    UNION SELECT 'Orlando' AS FirstName, 'Romeo' AS LastName, 'Texas' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(8578.00 AS money) AS YTDPurchase, CAST('2015-7-29' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    UNION SELECT 'Cynthia' AS FirstName, 'Randall' AS LastName, 'Utah' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(7218.10 AS money) AS YTDPurchase, CAST('2015-1-11' AS date) AS LastPurchase  
    UNION SELECT 'Rebecca' AS FirstName, 'Roberts' AS LastName, 'Washington' AS StateProvince, 7 AS CountryRegionID, 'Female' AS Gender, CAST(8357.80 AS money) AS YTDPurchase, CAST('2015-10-28' AS date) AS LastPurchase  
    UNION SELECT 'Cristian' AS FirstName, 'Petulescu' AS LastName, 'Wisconsin' AS StateProvince, 7 AS CountryRegionID, 'Male' AS Gender, CAST(3470.00 AS money) AS YTDPurchase, CAST('2015-11-30' AS date) AS LastPurchase  
    ```  

  
10. En la barra de herramientas del diseñador de consultas, haga clic en **Ejecutar** ( **!** ). El conjunto de resultados muestra 23 líneas de datos con las columnas siguientes: FirstName, LastName, StateProvince, CountryRegionID, Gender, YTDPurchase y LastPurchase.  

    ![Captura de pantalla del paso Diseñar una consulta del asistente Nueva tabla o matriz.](../reporting-services/media/report-builder-expression-tutorial-query-as-text.png)
  
11. Haga clic en **Next**.  
  
12. En la página **Organizar campos** , arrastre los campos siguientes, en el orden especificado, desde la lista **Campos disponibles** a la lista **Valores** .  
  
    -   StateProvince   
    -   CountryRegionID  
    -   LastPurchase  
    -   YTDPurchase  
  
    Dado que CountryRegionID e YTDPurchase contienen datos numéricos, el agregado SUM se aplica a ellos de manera predeterminada, pero no quiere que sean sumas.  
   
13. En la lista **Valores** , haga clic con el botón derecho en **CountryRegionID** y desactive la casilla **Sumar** .  
  
    La suma ya no se aplica a CountryRegionID.  
  
14. En la lista **Valores** , haga clic con el botón derecho en **YTDPurchase** y en la opción **Sumar** .  
  
    La suma ya no se aplica a YTDPurchase.  
    
    ![Captura de pantalla de la lista Valores en la que se muestra la opción Suma lista para borrarse.](../reporting-services/media/report-builder-expression-not-sum.png)
  
15. Haga clic en **Next**.  
  
16. En la página **Elegir el diseño** , mantenga la configuración predeterminada y haga clic en **Siguiente**.  

    ![Captura de pantalla del paso Elegir el diseño del asistente Nueva tabla o matriz.](../reporting-services/media/report-builder-expression-tutorial-choose-layout.png)
  
17. Haga clic en **Finalizar**  
  
## <a name="2-update-default-names-of-the-data-source-and-dataset"></a><a name="UpdateNames"></a>2. Actualizar nombres predeterminados del origen de datos y el conjunto de datos  
  
### <a name="to-update-the-default-name-of-the-data-source"></a>Para actualizar el nombre predeterminado del origen de datos  
  
1.  En el panel Datos de informe, expanda la carpeta **Orígenes de datos** .  
  
2.  Haga clic con el botón derecho en **OrigenDeDatos1** y, después, haga clic en **Propiedades del origen de datos.**  
  
3.  En el cuadro **Nombre** , escriba **ExpressionsDataSource**  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-update-the-default-name-of-the-dataset"></a>Para actualizar el nombre predeterminado del conjunto de datos  
  
1.  En el panel Datos de informe, expanda la carpeta **Conjuntos de datos** .  
  
2.  Haga clic con el botón derecho en **ConjuntoDeDatos1** y haga clic en **Propiedades del conjunto de datos**.  

    ![Captura de pantalla en la que se muestra cómo acceder a las propiedades del conjunto de datos en Report Builder.](../reporting-services/media/report-builder-expression-tutorial-rename-dataset.png)
  
3.  En el cuadro **Nombre** , escriba **Expresiones**  
  
4.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
## <a name="3-display-first-initial-and-last-name"></a><a name="Concatenate"></a>3. Mostrar First Initial y Last Name  
En esta sección, usará la función **Left** y el operador **Concatenate** (**&**) en una expresión que da como resultado un nombre que incluye una inicial y un apellido. Puede compilar la expresión paso a paso o avanzar en el procedimiento y copiar y pegar la expresión desde el tutorial al cuadro de diálogo **Expresión** .   
  
1.  Haga clic con el botón derecho en la columna **StateProvince** , seleccione **Insertar columna** y haga clic en **Izquierda**.  
  
    Se agrega una columna nueva a la izquierda de la columna **StateProvince** . 
    
    ![Captura de pantalla en la que se muestra cómo insertar una columna a la izquierda en un informe.](../reporting-services/media/report-builder-expression-tutorial-insert-column.png) 
  
2.  Haga clic en el encabezado de la nueva columna y escriba **Nombre**  
  
3.  Haga clic con el botón derecho en la celda de datos de la columna **Nombre** y haga clic en **Expresión**.  

    ![Captura de pantalla en la que se muestra cómo insertar una expresión en un informe.](../reporting-services/media/report-builder-expression-tutorial-insert-expression.png)
  
4.  En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y haga clic en **Texto**.  
  
5.  En la lista **Elemento** , haga doble clic en **Izquierda**.  
  
    La función **Izquierda** se agrega a la expresión.  
    
    ![Captura de pantalla en la que se muestra cómo agregar una función IZQUIERDA a una expresión.](../reporting-services/media/report-builder-expression-tutorial-left-function.png)
  
6.  En la lista **Categoría** , haga clic en **Campos (Expresiones)** .  
  
7.  En la lista **Valores** , haga doble clic en **FirstName**.  
  
8.  Escriba **, 1)**  
  
    Esta expresión extrae un carácter del valor **FirstName** , contando desde la izquierda.  
  
9. Escriba **&". "&** .  

    Esto agrega un punto y un espacio después de la expresión.
  
10. En la lista **Valores** , haga doble clic en **LastName**.  
  
    La expresión completa es: `=Left(Fields!FirstName.Value, 1) &". "& Fields!LastName.Value`  
    
    ![Captura de pantalla en la que se muestra cómo agregar un valor LastName a una expresión.](../reporting-services/media/report-builder-expression-tutorial-complete-name-expression.png)
  
11. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
12. Haga clic en **Ejecutar** para obtener la vista previa del informe.  

## <a name="optional-format-the-date-and-currency-columns-and-header-row"></a><a name="DateFormat"></a>(opcional) Dar formato a las columnas Date y Currency, y a la fila de encabezados  
En esta sección, dará formato a la columna **Last Purchase** que contiene fechas, y a la columna YTDPurchase que contiene la moneda. También puede aplicar formato a la fila de encabezados.  
  
### <a name="to-format-the-date-column"></a>Para dar formato a la columna de fecha  
  
1.  Haga clic en **Diseño** para volver a la vista de diseño.  
  
2.  Seleccione la celda de datos en la columna **Last Purchase** y, en la pestaña **Inicio** > sección **Número**, seleccione **Fecha**.  

    ![Captura de pantalla en la que se muestra cómo establecer la columna de Última compra en Fecha.](../reporting-services/media/report-builder-expression-tutorial-date-format.png)
  
3.  Además, en la sección **Número** , haga clic en la flecha junto a **Estilos de marcador de posición** y seleccione **Valores de ejemplo**. 

    ![Captura de pantalla en la que se muestra la opción Valores de ejemplo en Report Builder.](../reporting-services/media/report-builder-expression-tutorial-sample-values.png)

    Ahora puede ver un ejemplo del formato que ha seleccionado. 
  
### <a name="to-format-the-currency-column"></a>Para dar formato a la moneda

- Seleccione la celda de datos en la columna **YTDPurchase** y, en la sección **Número** , seleccione **Símbolo de moneda**.
 
### <a name="to-format-the-column-headers"></a>Para dar formato a los encabezados de columna

1. Seleccione la fila de los encabezados de columna.

2. En la pestaña **Inicio** > sección **Párrafo**, seleccione **Izquierda**. 

    ![Captura de pantalla en la que se muestra cómo dar formato a los encabezados en Report Builder.](../reporting-services/media/report-builder-expression-tutorial-format-headings.png)

3. Haga clic en **Ejecutar** para obtener la vista previa del informe. 

Aquí se muestra el informe hasta el momento con el formato en las fechas, moneda y encabezados de columna.

![Captura de pantalla en la que se muestra la vista previa del informe con formato.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted.png)

  
## <a name="4-use-color-to-display-gender"></a><a name="Gender"></a>4. Usar color para mostrar el sexo de una persona  
En esta sección, agregará color para mostrar el sexo de una persona. Agregará una nueva columna para mostrar el color y, después, determinará el color que aparece en la columna según el valor del campo Gender.  
  
Para mantener el color que ha aplicado en la celda de la tabla cuando crea un informe con bandas, agregue un rectángulo y, después, agréguele el color de fondo.  
    
 
### <a name="to-add-an-mf-column"></a>Para agregar una columna M/F  
  
1.  Haga clic con el botón derecho en la columna **Nombre** , seleccione **Insertar columna** y haga clic en **Izquierda**.  
  
    Se agrega una columna nueva a la izquierda de la columna **Nombre** .  
  
2.  Haga clic en el encabezado de la nueva columna y escriba **M/F**.  
  
### <a name="to-add-a-rectangle"></a>Para agregar un rectángulo  
  
1.   En la pestaña **Insertar** , haga clic en **Rectángulo** y, después, haga clic en la celda de datos de la columna **M/F** .  
  
     Se agregará un rectángulo a la celda.  
     
     ![Captura de pantalla en la que se muestra cómo insertar un rectángulo.](../reporting-services/media/report-builder-expression-tutorial-insert-rectangle.png)
  
2. Arrastre el divisor de columna entre **M/F** y **Nombre** para hacer más estrecha la columna **M/F** .

    ![Captura de pantalla en la que se muestra cómo hacer que una columna sea más estrecha.](../reporting-services/media/report-builder-expression-tutorial-narrow-column.png)
  
### <a name="to-use-color-to-show-gender"></a>Para usar color para mostrar el sexo de una persona  
  
1.  Haga clic con el botón derecho en el rectángulo de la celda de datos de la columna **M/F** y haga clic en **Propiedades del rectángulo**.  
  
2.  En el cuadro de diálogo **Propiedades del rectángulo** > pestaña **Relleno**, haga clic en el botón de la expresión **fx** junto a **Color de relleno**.  
  
3.  En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y, después, haga clic en **Flujo de programa**.  
  
4.  En la lista **Elemento** , haga doble clic en **Cambiar**.  
  
5.  En la lista **Categoría** , haga clic en **Campos (Expresiones)** .  
  
6.  En la lista **Valores** , haga doble clic en **Gender**.  
  
7.  Escriba **="Hombre",** (incluida la coma).

8. En la lista **Categoría** , haga clic en **Constantes** y, en el cuadro **Valores** , haga clic en **Azul aciano**.

    ![Captura de pantalla en la que se muestra cómo usar un color para indicar un sexo.](../reporting-services/media/report-builder-expression-tutorial-color-expression-cornflower-blue.png)

9. Escriba una coma al final. 
  
5.  En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **Género** de nuevo.  
  
7.  Escriba **="Mujer",** (incluida la coma). 

8. En la lista **Categoría** , haga clic en **Constantes** y, en el cuadro **Valores** , haga clic en **Rojo tomate**.

13. Escriba un paréntesis de cierre **)** al final. 
  
    La expresión completa es: `=Switch(Fields!Gender.Value ="Male", "CornflowerBlue",Fields!Gender.Value ="Female","Tomato")`  
    
    ![Captura de pantalla en la que se muestra la expresión completa en el cuadro de diálogo Expresión.](../reporting-services/media/report-builder-expression-tutorial-color-expression-complete.png)
  
12. Haga clic en **Aceptar** y, después, en **Aceptar** de nuevo para cerrar el cuadro de diálogo **Propiedades del rectángulo** .  
  
14. Haga clic en **Ejecutar** para obtener la vista previa del informe.  

    ![Captura de pantalla en la que se muestra la vista previa con la columna M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-m-f-column.png)

### <a name="to-format-the-color-rectangles"></a>Para dar formato a los rectángulos de color

1. Haga clic en **Diseño** para volver a la vista de diseño.  

16. Seleccione el rectángulo en la columna **M/F** . En el panel Propiedades, en la sección Borde, establezca estas propiedades:

    - BorderColor = White
    - BorderStyle = Solid
    - BorderWidth = 5pt
    
    ![Captura de pantalla que muestra cómo dar formato a los rectángulos de color de la columna M/F.](../reporting-services/media/report-builder-expression-tutorial-format-m-f-column.png)

18. Haga clic en **Ejecutar** para obtener la vista previa del informe de nuevo. Esta vez, los bloques de color tienen espacios en blanco a su alrededor.

    ![Captura de pantalla en la que se muestra la vista previa con los rectángulos con formato en la columna M/F.](../reporting-services/media/report-builder-expression-tutorial-preview-formatted-m-f-column.png)  
  
## <a name="5-look-up-the-countryregion-name"></a><a name="Lookup"></a>5. Buscar el nombre CountryRegion  
En esta sección, creará el conjunto de datos CountryRegion y usará la función **Lookup** para mostrar el nombre de un país o región en lugar del identificador del mismo.  
  
### <a name="to-create-the-countryregion-dataset"></a>Para crear el conjunto de datos CountryRegion  
  
1.  Haga clic en **Diseño** para volver a la vista de diseño.  
  
2.  En el panel Datos de informe, haga clic en **Nuevo** y, después, en **Conjunto de datos**.  
  
3.  En **Propiedades del conjunto de datos**, haga clic en **Usar un conjunto de datos insertado en el informe**.  
  
4.  En la lista **Origen de datos** , seleccione ExpressionsDataSource.  
  
5.  En el cuadro **Nombre** , escriba **CountryRegion**  
  
6.  Compruebe que el tipo de consulta **Texto** está seleccionado y haga clic en **Diseñador de consultas**.  
  
7.  Haga clic en **Editar como texto**.  
  
8.  Copie y pegue la siguiente consulta en el panel de consulta:  
  
    ```  
    SELECT 1 AS ID, 'American Samoa' AS CountryRegion  
    UNION SELECT 2 AS CountryRegionID, 'Australia' AS CountryRegion  
    UNION SELECT 3 AS ID, 'Canada' AS CountryRegion  
    UNION SELECT 4 AS ID, 'Germany' AS CountryRegion  
    UNION SELECT 5 AS ID, 'Micronesia' AS CountryRegion  
    UNION SELECT 6 AS ID, 'France' AS CountryRegion  
    UNION SELECT 7 AS ID, 'United States' AS CountryRegion  
    UNION SELECT 8 AS ID, 'Brazil' AS CountryRegion  
    UNION SELECT 9 AS ID, 'Mexico' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Japan' AS CountryRegion  
    UNION SELECT 10 AS ID, 'Australia' AS CountryRegion  
    UNION SELECT 12 AS ID, 'United Kingdom' AS CountryRegion  
    ```  
  
9. Haga clic en **Ejecutar** ( **!** ) para ejecutar la consulta.  
  
    Los resultados de la consulta son los identificadores del país o región y los nombres.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Haga clic en **Aceptar** de nuevo para cerrar el cuadro de diálogo **Propiedades del conjunto de datos** .  

     Ahora tiene un segundo conjunto de datos en la columna **Report Data** .
  
### <a name="to-look-up-values-in-the-countryregion-dataset"></a>Para buscar valores en el conjunto de datos CountryRegion  
  
1.  Haga clic en el encabezado de columna **Country Region ID** y elimine el texto: **ID** para que se lea **Country Region**.  
  
2.  Haga clic con el botón derecho en la celda de datos de la columna **Country Region** y haga clic en **Expresión**.  
  
3.  Elimine la expresión excepto el signo igual (=) inicial.  
  
    El resto de la expresión es: `=`  
  
4.  En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y, después, haga clic en **Varios**. En la lista **Elemento** , haga doble clic en **Lookup**.  
  
6.  En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **CountryRegionID**.  
  
8.  Coloque el cursor inmediatamente después de `CountryRegionID.Value`y escriba **,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")** .  
  
    La expresión completa es la siguiente: `=Lookup(Fields!CountryRegionID.Value,Fields!ID.value, Fields!CountryRegion.value, "CountryRegion")`  
  
    La sintaxis de la función **Lookup** especifica una búsqueda entre CountryRegionID en el conjunto de datos Expressions e ID en el conjunto de datos CountryRegion, que devuelve el valor CountryRegion del conjunto de datos CountryRegion.  
  
10. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
11. Haga clic en **Ejecutar** para obtener la vista previa del informe.  
  
## <a name="6-count-days-since-last-purchase"></a><a name="Count"></a>6. Recuento de días desde la última compra  
En esta sección, agregará una columna y, después, usará la función **Now** o la variable global integrada `ExecutionTime` para calcular el número de días desde hoy hasta la fecha en que un cliente ha realizado compras por última vez.  
  
### <a name="to-add-the-days-ago-column"></a>Para agregar la columna Días transcurridos  
  
1.  Haga clic en **Diseño** para volver a la vista de diseño.  
  
2.  Haga clic con el botón derecho en la columna **Last Purchase** , seleccione **Insertar columna** y haga clic en **Derecha**.  
  
    Se agrega una columna nueva a la derecha de la columna **Last Purchase** .  
  
3.  En el encabezado de columna, escriba **Días transcurridos**  
  
4.  Haga clic con el botón derecho en la celda de datos de la columna **Días transcurridos** y haga clic en **Expresión**.  
  
5.  En el cuadro de diálogo **Expresión**, expanda **Funciones comunes** y haga clic en **Fecha y hora**.  
  
6.  En la lista **Elemento** , haga doble clic en **DateDiff**.  
  
7.  Inmediatamente después de `DateDiff(`, escriba **"d",** (incluidas las comillas "" y la coma). 
  
9. En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **LastPurchase**.  
  
11. Inmediatamente después de `Fields!LastPurchase.Value`, escriba **,** (una coma). 
  
13. En la lista **Categoría**, haga clic en **Fecha y hora** de nuevo y, en la lista **Elemento**, haga doble clic en **Ahora**.  
  
    > [!WARNING]  
    > En los informes de producción no debe usar la función **Ahora** en las expresiones que se evalúan varias veces como representadores de informes (por ejemplo, en las filas de detalle de un informe). El valor de **Ahora** cambia de una fila a otra y los diferentes valores afectan a los resultados de evaluación de las expresiones, lo que conduce a resultados ligeramente incoherentes. En su lugar, use la variable global `ExecutionTime` que proporciona [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
15. Elimine el paréntesis de apertura después de `Now(`y, después, escriba un paréntesis de cierre **)** .  
  
    La expresión completa es: `=DateDiff("d", Fields!LastPurchase.Value, Now)`  
    
    ![Captura de pantalla en la que se muestra la expresión completa de la fecha desde la última compra.](../reporting-services/media/report-builder-expression-tutorial-date-since-last-purchase.png)
  
17. [!INCLUDE[clickOK](../includes/clickok-md.md)]  

11. Haga clic en **Ejecutar** para obtener la vista previa del informe.  
  
## <a name="7-use-an-indicator-to-show-sales-comparison"></a><a name="Indicator"></a>7. Usar un indicador para mostrar la comparación de ventas  
En esta sección agregará una nueva columna y usará un indicador para mostrar si las compras hasta la fecha de un usuario están por encima o por debajo del promedio de compras hasta la fecha. La función **Redondear** quita los decimales de los valores.  
  
Configurar el indicador y sus estados tiene muchos pasos. Si quiere, puede saltarse el procedimiento "Para configurar el indicador" y copiar o pegar las expresiones completas de este tutorial en el cuadro de diálogo **Expresión**.  
  
### <a name="to-add-the--or---avg-sales-column"></a>Para agregar la columna Promedio + o -  
  
1.  Haga clic con el botón derecho en la columna **YTD Purchase** , seleccione **Insertar columna** y haga clic en **Derecha**.  
  
    Se agrega una columna nueva a la derecha de la columna **YTD Purchase** .  
  
2.  Haga clic en el encabezado de la columna y escriba **Ventas promedio + o -** .  
  
### <a name="to-add-an-indicator"></a>Para agregar un indicador  
  
1.  En la pestaña **Insertar** , haga clic en **Indicador** y, después, haga clic en la celda de datos de la columna **Ventas promedio + o -** .  
  
    Se abrirá el cuadro de diálogo **Seleccionar tipo de indicador** .  
  
2.  En el grupo **Direccional** de los conjuntos de iconos, haga clic en el conjunto de tres flechas grises.  

    ![Captura de pantalla en la que se muestra cómo agregar un indicador.](../reporting-services/media/report-builder-expression-tutorial-select-indicator.png)
  
3.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-configure-the-indicator"></a>Para configurar el indicador  
  
1.  Haga clic con el botón derecho en el indicador, haga clic en **Propiedades de indicador** y, después, en **Valor y estados**.  
  
2.  Haga clic en el botón de expresión **fx** situado junto al cuadro de texto **Valor** .  
  
3.  En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y haga clic en **Matemáticas**.  
  
4.  En la lista **Elemento** , haga doble clic en **Redondear**.  
  
5.  En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **YTDPurchase**.  
  
7.  Inmediatamente después de `Fields!YTDPurchase.Value`, escriba  **-** (un signo menos). 
  
9. Expanda **Funciones comunes** de nuevo, haga clic en **Agregado** y, en la lista **Elemento** , haga doble clic en **Promedio**.  
  
11. En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **YTDPurchase**.  
  
13. Inmediatamente después de `Fields!YTDPurchase.Value`, escriba **, "Expresiones"))**  
  
    La expresión completa es: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions"))`  
  
15. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
16. En el cuadro **Unidad de medida de estados** , seleccione **Numérico**.  
  
17. En la fila con la flecha hacia abajo, haga clic en el botón **fx** a la derecha del cuadro de texto del valor **Inicio** .  

    ![Captura de pantalla en la que se muestra cómo seleccionar el botón FX situado junto al cuadro de texto Iniciar.](../reporting-services/media/report-builder-expression-tutorial-indicator-start.png)
  
18. En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y haga clic en **Matemáticas**.  
  
19. En la lista **Elemento** , haga doble clic en **Redondear**.  
  
20. En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **YTDPurchase**.  
  
22. Inmediatamente después de `Fields!YTDPurchase.Value`, escriba  **-** (un signo menos). 
  
24. Expanda **Funciones comunes** de nuevo, haga clic en **Agregado** y, en la lista **Elemento** , haga doble clic en **Promedio**.  
  
26. En la lista **Categoría** , haga clic en **Campos (expresiones)** y, en la lista **Valores** , haga doble clic en **YTDPurchase**.  
  
28. Inmediatamente después de `Fields!YTDPurchase.Value`, escriba **, "Expresiones")) < 0**  
  
    La expresión completa es la siguiente: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) < 0`  
  
30. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
31. En el cuadro de texto del valor **Final** , escriba **0**  
  
32. Haga clic en la fila con la flecha horizontal y haga clic en **Eliminar**.  

    ![Captura de pantalla en la que se muestra cómo eliminar un indicador.](../reporting-services/media/report-builder-expression-tutorial-delete-indicator-state.png)
    
    Ahora solo existen dos flechas, una hacia arriba y otra hacia abajo.
  
33. En la fila con la flecha hacia arriba, en el cuadro **Inicio** , escriba **0**  
  
34. Haga clic en el botón **fx** a la derecha del cuadro de texto del valor **Final** .  
  
35. En el cuadro de diálogo **Expresión** , elimine **100** y cree la expresión: `=Round(Fields!YTDPurchase.Value - Avg(Fields!YTDPurchase.Value, "Expressions")) >0`  
  
36. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
37. Haga clic en **Aceptar** de nuevo para cerrar el cuadro de diálogo **Propiedades de indicador** .  
  
38. Haga clic en **Ejecutar** para obtener la vista previa del informe.  

    ![Captura de pantalla en la que se muestra la vista previa con la columna + o - AVG Sales, incluidos todos los nuevos indicadores.](../reporting-services/media/report-builder-expression-tutorial-preview-indicator.png)
  
## <a name="8-make-a-banded-report"></a><a name="GreenBar"></a>8. Crear un informe con bandas  
Cree un parámetro de forma que los lectores del informe puedan especificar el color que se aplicará a las filas alternativas del informe, convirtiéndolo en un informe con bandas.  
  
### <a name="to-add-a-parameter"></a>Para agregar un parámetro  
  
1.  Haga clic en **Diseño** para volver a la vista de diseño.  
  
2.  En el panel **Datos de informe** , haga clic con el botón derecho en **Parámetros** y haga clic en **Agregar parámetro**.  

    ![Captura de pantalla en la que se muestra cómo agregar un parámetro.](../reporting-services/media/report-builder-expression-tutorial-add-parameter.png)
  
    Se abrirá el cuadro de diálogo **Propiedades de parámetro de informe** .  
  
3.  En **Pedir datos**, escriba **Elegir color**  
  
4.  En **Nombre**, escriba **RowColor**  
  
5.  En la pestaña **Valores disponibles** , haga clic en **Especificar valores**.  
  
7.  Haga clic en **Agregar**.  
  
8.  En el cuadro **Etiqueta** , escriba: **Amarillo**  
  
9. En el cuadro **Valor** , escriba **Amarillo**  
  
10. Haga clic en **Agregar**.  
  
11. En el cuadro **Etiqueta** , escriba **Verde**  
  
12. En el cuadro **Valor** , escriba **VerdePálido**  
  
13. Haga clic en **Agregar**.  
  
14. En el cuadro **Etiqueta** , escriba **Azul**  
  
15. En el cuadro **Valor** , escriba **AzulClaro**  
  
16. Haga clic en **Agregar**.  
  
17. En el cuadro **Etiqueta** , escriba **Rosa**  
  
18. En el cuadro **Valor** , escriba **Rosa**  

    ![Captura de pantalla del cuadro de diálogo Propiedades de parámetro de informe en la que se muestra el paso Elección de los valores disponibles para este parámetro.](../reporting-services/media/report-builder-expression-tutorial-parameter-available.png)
  
19. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="to-apply-alternating-colors-to-detail-rows"></a>Para aplicar colores alternativos a las filas de detalle.  
  
1.   Seleccione toda la celda en la fila de datos excepto la celda de la columna **M/F** que tiene su propio color de fondo.  

     ![Captura de pantalla en la que se muestran las celdas seleccionadas en una fila de datos.](../reporting-services/media/report-builder-expression-tutorial-select-banded.png)
  
4.  En el panel Propiedades, haga clic en **BackgroundColor**. 

     Si no ve el panel Propiedades, en la pestaña **Ver** , active la casilla **Propiedades** .  
  
    Si las propiedades se enumeran por categoría en el panel Propiedades, encontrará **BackgroundColor** en la categoría **Varios** .  
  
5.  Haga clic en la flecha hacia abajo y, después, en **Expresión**.  

    ![Captura de pantalla del cuadro Propiedades en el que se muestra cómo asociar una expresión a BackgroundColor.](../reporting-services/media/report-builder-expression-tutorial-banded-color-property.png)
  
6.  En el cuadro de diálogo **Expresión** , expanda **Funciones comunes** y, después, haga clic en **Flujo de programa**.  
  
7.  En la lista **Elemento** , haga doble clic en **SiInm**.  
  
8.  En **Funciones comunes**, haga clic en **Varios** y, en la lista **Elemento** , haga doble clic en **RowNumber**.  

9. Inmediatamente después de **RowNumber(** escriba **Nada) MOD 2,**
  
8. Haga clic en **Parámetros** y en la lista **Valores** , haga doble clic en **RowColor**.  
  
22. Inmediatamente después de `Parameters!RowColor.Value`, escriba **, "Blanco")** .  
  
    La expresión completa es: `=IIF(RowNumber(Nothing) MOD 2, Parameters!RowColor.Value, "White")`  
    
    ![Captura de pantalla en la que se muestra la expresión con color completo con bandas.](../reporting-services/media/report-builder-expression-tutorial-banded-color-expressn.png)
  
24. [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
### <a name="run-the-report"></a>Ejecutar el informe  
  
1.  En la pestaña **Inicio** , haga clic en **Ejecutar**.  

    Ahora, cuando ejecute el informe, no verá el informe hasta que elija un color para las bandas que no son blancas.
  
3.  En la lista **Elegir color** , seleccione un color para las bandas que no son blancas del informe.  
    
    ![Captura de pantalla en la que se muestra cómo elegir un color para las bandas que no son blancas.](../reporting-services/media/report-builder-expression-tutorial-select-color.png)
  
4.  Haga clic en **Ver informe**.  
  
    Los representadores de informes y las filas alternativas tienen el fondo que elija. 
    
    ![Captura de pantalla en la que se muestra la vista previa con filas con colores alternos.](../reporting-services/media/report-builder-expression-tutorial-preview-banded.png) 
  
## <a name="optional-add-a-report-title"></a><a name="Title"></a>(Opcional) Agregar un título de informe  
Agregue un título al informe.  
  
### <a name="to-add-a-report-title"></a>Para agregar un título de informe  
  
1.  En la superficie de diseño, haga clic en **Haga clic para agregar título**.  
  
2.  Escriba **Resumen de comparación de ventas** y seleccione el texto.  
  
3.  En la pestaña **Inicio** , en el cuadro **Fuente** , establezca:

    -  Tamaño = 18
    -  Color = gris
    -  Negrita
  
4.  En la pestaña **Inicio** , haga clic en **Ejecutar**.  
  
3.  Seleccione un color para las bandas que no son blancas del informe y haga clic en **Ver informe**.  
  
## <a name="optional-save-the-report"></a><a name="Save"></a>(Opcional) Guardar el informe  
Puede guardar los informes en un servidor de informes, en una biblioteca de SharePoint o en su equipo. Para obtener más información, consulte [Guardar informes &#40;Generador de informes&#41;](../reporting-services/report-builder/saving-reports-report-builder.md).  
  
En este tutorial, guarde el informe en un servidor de informes. Si no tiene acceso a un servidor de informes, guarde el informe en su equipo.  
  
### <a name="to-save-the-report-to-a-report-server"></a>Para guardar el informe en un servidor de informes  
  
1.  En el menú **Archivo** > **Guardar como**.  
  
2.  Haga clic en **Sitios y servidores recientes**.  
  
3.  Seleccione o escriba el nombre del servidor de informes donde tiene el permiso para guardar los informes.  
  
    Aparecerá el mensaje "Conectando con el servidor de informes". Una vez completada la conexión, verá el contenido de la carpeta de informes que el administrador del servidor de informes especificó como ubicación predeterminada para los informes.  
  
4.  Asigne un nombre al informe y haga clic en **Guardar**.  
  
El informe se guarda en el servidor de informes. El nombre del servidor de informes al que está conectado aparecerá en la barra de estado en la parte inferior de la ventana.

Ahora los lectores del informe pueden ver su informe en el portal web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] .

![Captura de pantalla del nuevo informe completado con todas las expresiones visibles.](../reporting-services/media/report-builder-expression-tutorial-final-in-browser.png)

   
## <a name="see-also"></a>Consulte también  
[Expresiones &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
[Ejemplos de expresiones &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)  
[Indicadores &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
[Imágenes, cuadros de texto rectángulos y líneas &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/images-text-boxes-rectangles-and-lines-report-builder-and-ssrs.md)  
[Tablas &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/tables-report-builder-and-ssrs.md)  
[Conjuntos de datos de informe &#40;SSRS&#41;](../reporting-services/report-data/report-datasets-ssrs.md)  
  
  
  

