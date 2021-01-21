---
title: Adición de parámetros en cascada a un informe (Generador de informes) | Microsoft Docs
description: Obtenga información sobre cómo usar parámetros en cascada en los informes del Generador de informes para administrar grandes cantidades de datos de informe.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 3a22eec3-57a7-478e-b6fc-102a9dbe0591
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 77a96c10d94bb27d52f8527e6822ff036e4a214e
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596760"
---
# <a name="add-cascading-parameters-to-a-report-report-builder-and-ssrs"></a>Agregar parámetros en cascada a un informe (Generador de informes y SSRS)
  Los parámetros en cascada permiten administrar grandes cantidades de datos de informe. Es posible definir un conjunto de parámetros relacionados de manera que la lista de valores de uno de ellos dependa del valor seleccionado en otro parámetro. Por ejemplo, el primer parámetro es independiente y podría presentar una lista de categorías de productos. Cuando el usuario selecciona una categoría, el segundo parámetro depende del valor del primer parámetro. Sus valores se actualizan con una lista de subcategorías para la categoría elegida. Cuando el usuario ve el informe, los valores de los parámetros de categoría y subcategoría se usan para filtrar los datos del informe.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
 Para crear parámetros en cascada, primero debe definir la consulta del conjunto de datos e incluir un parámetro de consulta para cada parámetro en cascada que necesite. También debe crear un conjunto de datos independiente para que para cada parámetro en cascada proporcione los valores disponibles. Para más información, vea [Agregar, cambiar o eliminar los valores disponibles para un parámetro de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-available-values-for-a-report-parameter.md).  
  
 El orden es importante para los parámetros en cascada, dado que la consulta del conjunto de datos de un parámetro que aparece más adelante en la lista incluye una referencia a cada parámetro que aparece antes en la lista. En tiempo de ejecución, el orden de los parámetros en el panel Datos de informe determina el orden en que aparecen las consultas de parámetros en el informe, y por consiguiente, el orden en el que el usuario elige cada uno de los valores de los parámetros sucesivos.  
  
 Para obtener información sobre la creación de parámetros en cascada con varios valores e incluso la función Select All, vea [Tener un parámetro en cascada con varios valores y Select All](/archive/blogs/psssql/).  
  
## <a name="to-create-the-main-dataset-with-a-query-that-includes-multiple-related-parameters"></a>Para crear el conjunto de datos principal con una consulta que incluye varios parámetros relacionados  
  
1.  En el panel Datos de informe, haga clic con el botón derecho en un origen de datos y, después, haga clic en **Agregar conjunto de datos**.  
  
2.  En **Nombre**, escriba el nombre del conjunto de datos.  
  
3.  En **Origen de datos**, elija el nombre del origen de datos o haga clic en **Nuevo** para crear uno.  
  
4.  En **Tipo de consulta**, elija el tipo de consulta para el origen de datos seleccionado. En este tema, se supone que el tipo de consulta es **Texto** .  
  
5.  En **Consulta**, escriba la consulta que se debe usar para recuperar los datos para este informe. La consulta debe incluir las partes siguientes:  
  
    1.  Una lista de campos del origen de datos. Por ejemplo, en una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] , la instrucción SELECT especifica una lista de nombres de columnas de la base de datos de una tabla o vista determinada.  
  
    2.  Un parámetro de consulta para cada parámetro en cascada. Un parámetro de consulta limita los datos recuperados del origen de datos especificando determinados valores para incluirlos o excluirlos de la consulta. Normalmente, los parámetros de consulta se sitúan en una cláusula de restricción de la consulta. Por ejemplo, en una instrucción SELECT de [!INCLUDE[tsql](../../includes/tsql-md.md)] , los parámetros de consulta se sitúan en la cláusula WHERE.  
  
6.  Haga clic en **Ejecutar** ( **!** ). Una vez incluidos los parámetros de la consulta y ejecutada la consulta, automáticamente se crean parámetros de informe correspondientes a los parámetros de la consulta.  
  
    > [!NOTE]  
    >  El orden que tienen los parámetros de la consulta la primera vez se ejecuta una consulta determina el orden en que se crean en el informe. Para cambiar el orden, vea [Cambiar el orden de un parámetro de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-the-order-of-a-report-parameter-report-builder-and-ssrs.md).  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 A continuación, creará un conjunto de datos que proporciona los valores para el parámetro independiente.  
  
## <a name="to-create-a-dataset-to-provide-values-for-an-independent-parameter"></a>Para crear un conjunto de datos para proporcionar los valores para un parámetro independiente  
  
1.  En el panel Datos de informe, haga clic con el botón derecho en un origen de datos y, después, haga clic en **Agregar conjunto de datos**.  
  
2.  En **Nombre**, escriba el nombre del conjunto de datos.  
  
3.  En **Origen de datos**, compruebe que el nombre es el nombre del origen de datos que eligió en el paso 1.  
  
4.  En **Tipo de consulta**, elija el tipo de consulta para el origen de datos seleccionado. En este tema, se supone que el tipo de consulta es **Texto** .  
  
5.  En **Consulta**, escriba la consulta que se debe usar para recuperar los valores para este parámetro. Las consultas para los parámetros independientes normalmente no contienen los parámetros de la consulta. Por ejemplo, para crear una consulta para un parámetro que proporciona los valores de todas las categorías, podría usar una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] similar a la siguiente:  
  
    ```  
    SELECT DISTINCT <column name> FROM <table>  
    ```  
  
     El comando SELECT DISTINCT quita los valores duplicados del conjunto de resultados para que obtenga cada valor único de la columna y la tabla especificadas.  
  
     Haga clic en **Ejecutar** ( **!** ). El conjunto de resultados muestra los valores disponibles para este primer parámetro.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 A continuación, establecerá las propiedades del primer parámetro que se debe usar en este conjunto de datos para rellenar sus valores disponibles en tiempo de ejecución.  
  
## <a name="to-set-available-values-for-a-report-parameter"></a>Para establecer los valores disponibles para un parámetro de informe  
  
1.  En la carpeta Parámetros del panel Datos de informe, haga clic con el botón derecho en el primer parámetro y, después, haga clic en **Propiedades del parámetro**.  
  
2.  En **Nombre**, compruebe que el nombre del parámetro es correcto.  
  
3.  Haga clic en **Valores disponibles**.  
  
4.  Haga clic en **Obtener valores a partir de una consulta**. Aparecen tres campos.  
  
5.  En **Conjunto de datos**, en la lista desplegable, haga clic en el nombre del conjunto de datos que creó en el procedimiento anterior.  
  
6.  En el campo **Valor** , haga clic en el nombre del campo que proporciona el valor del parámetro.  
  
7.  En el campo **Etiqueta** , haga clic en el nombre del campo que proporciona la etiqueta del parámetro.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 A continuación, creará un conjunto de datos que proporcione los valores para el parámetro dependiente.  
  
## <a name="to-create-a-dataset-to-provide-values-for-a-dependent-parameter"></a>Para crear un conjunto de datos para proporcionar los valores para un parámetro dependiente  
  
1.  En el panel Datos de informe, haga clic con el botón derecho en un origen de datos y, después, haga clic en **Agregar conjunto de datos**.  
  
2.  En **Nombre**, escriba el nombre del conjunto de datos.  
  
3.  En **Origen de datos**, compruebe que el nombre es el nombre del origen de datos que eligió en el paso 1.  
  
4.  En **Tipo de consulta**, elija el tipo de consulta para el origen de datos seleccionado. En este tema, se supone que el tipo de consulta es **Texto** .  
  
5.  En **Consulta**, escriba la consulta que se debe usar para recuperar los valores para este parámetro. Las consultas para los parámetros dependientes normalmente incluyen parámetros de consulta para cada parámetro del que depende este parámetro. Por ejemplo, para crear una consulta para un parámetro que proporciona todos los valores de subcategoría (parámetro dependiente) para una categoría (parámetro independiente), podría usar una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] similar a la siguiente:  
  
    ```  
    SELECT DISTINCT Subcategory FROM <table>   
    WHERE (Category = @Category)  
    ```  
  
     En la cláusula WHERE, Category es el nombre de un campo de \<table> y @Category es un parámetro de la consulta. Esta instrucción genera una lista de subcategorías para la categoría especificada en @Category. En tiempo de ejecución, este valor se rellenará con el valor elegido por el usuario para el parámetro de informe que tiene el mismo nombre.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
 A continuación, establecerá las propiedades del segundo parámetro que se debe usar en este conjunto de datos para rellenar sus valores disponibles en tiempo de ejecución.  
  
## <a name="to-set-available-values-for-the-second-parameter"></a>Para establecer los valores disponibles para el segundo parámetro  
  
1.  En la carpeta Parámetros del panel Datos de informe, haga clic con el botón derecho en el primer parámetro y, después, haga clic en **Propiedades del parámetro**.  
  
2.  En **Nombre**, compruebe que el nombre del parámetro es correcto.  
  
3.  Haga clic en **Valores disponibles**.  
  
4.  Haga clic en **Obtener valores a partir de una consulta**.  
  
5.  En **Conjunto de datos**, en la lista desplegable, haga clic en el nombre del conjunto de datos que creó en el procedimiento anterior.  
  
6.  En el campo **Valor** , haga clic en el nombre del campo que proporciona el valor del parámetro.  
  
7.  En el campo **Etiqueta** , haga clic en el nombre del campo que proporciona la etiqueta del parámetro.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="to-test-the-cascading-parameters"></a>Para probar los parámetros en cascada  
  
1.  Haga clic en **Ejecutar**.  
  
2.  En la lista desplegable del primer parámetro, el parámetro independiente, elija un valor.  
  
     El procesador de informes ejecuta la consulta del conjunto de datos para el parámetro siguiente y le pasa el valor que eligió para el primer parámetro. La lista desplegable del segundo parámetro se rellena con los valores disponibles basados en el valor del primer parámetro.  
  
3.  De la lista desplegable del segundo parámetro, el parámetro dependiente, elija un valor.  
  
     El informe no se ejecuta automáticamente después de elegir el último parámetro para que pueda cambiar su elección.  
  
4.  Haga clic en **Ver informe**. El informe actualiza la presentación basándose en los parámetros que ha elegido.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar, cambiar o eliminar parámetros de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-report-parameter-report-builder-and-ssrs.md)   
 [Parámetros de informe &#40;Generador de informes y Diseñador de informes&#41;](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Tutorial: Agregar un parámetro a un informe &#40;Generador de informes&#41;](../../reporting-services/tutorial-add-a-parameter-to-your-report-report-builder.md)   
 [Tutoriales del Generador de informes](../../reporting-services/report-builder-tutorials.md)   
 [Agregar filtros de conjunto de datos, filtros de región de datos y filtros de grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md)   
 [Conjuntos de datos incrustados y compartidos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
