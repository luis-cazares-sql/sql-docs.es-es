---
description: 'Lección 1-2: Adición y configuración de un administrador de conexiones de archivos planos'
title: 'Paso 2: Adición y configuración de un administrador de conexiones de archivos planos | Microsoft Docs'
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 9a77dd32-d8c2-4961-ad37-2a971f9d6043
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 57bf9bd3863e5486c890088324641a5100581a02
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099243"
---
# <a name="lesson-1-2-add-and-configure-a-flat-file-connection-manager"></a>Lección 1-2: Adición y configuración de un administrador de conexiones de archivos planos

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En esta tarea, agregará un administrador de conexiones de archivos planos al paquete que acaba de crear. Un administrador de conexiones de archivos planos permite a un paquete extraer datos de un archivo plano. Mediante el administrador de conexiones de archivos planos puede especificar el nombre y la ubicación del archivo, la configuración regional y la página de códigos, y el formato del archivo, incluyendo los delimitadores de columna, que deben aplicarse cuando el paquete extrae datos del archivo plano. Además, puede especificar de forma manual el tipo de datos para columnas individuales, o usar el cuadro de diálogo **Sugerir tipos de columna** para asignar de forma automática las columnas de datos extraídos a los tipos de datos de [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .  
  
Debe crear un administrador de conexiones de archivos planos para cada formato de archivo con el que trabaje. Como en este tutorial se extraen datos de varios archivos planos que tienen exactamente el mismo formato de datos, solo tiene que agregar y configurar un administrador de conexiones de archivos planos para el paquete de ejemplo.  
  
En esta lección, configurará las propiedades siguientes en el administrador de conexiones de archivos planos:  
  
-   **Nombres de columna:** como el archivo plano no tiene nombres de columna, el administrador de conexiones de archivos planos crea nombres de columna predeterminados. Estos nombres predeterminados no son útiles para identificar qué representa cada columna. Cambie los nombres predeterminados por nombres que coincidan con la tabla de hechos en la que se van a cargar los datos del archivo plano.  
  
-   **Asignaciones de datos:** las asignaciones de tipo de datos que especifique para el administrador de conexiones de archivos planos se usan en todos los componentes de origen de datos de archivo plano que hacen referencia al administrador de conexiones. Puede asignar los tipos de datos de forma manual mediante el administrador de conexiones de archivos planos o usar el cuadro de diálogo **Sugerir tipos de columna** . En esta tarea, verá las asignaciones sugeridas en el cuadro de diálogo **Sugerir tipos de columna** y luego creará de forma manual las asignaciones necesarias en el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**.  
  
> [!NOTE]
> El administrador de conexiones de archivos planos proporciona información de configuración regional acerca del archivo de datos. Si el equipo no está configurado para usar la opción de configuración regional **Inglés (Estados Unidos)**, debe establecer propiedades adicionales en el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**.  
  
## <a name="add-a-flat-file-connection-manager-to-the-ssis-package"></a>Adición de un administrador de conexiones de archivos planos al paquete SSIS  
  
1.  Haga clic con el botón derecho en **Administradores de conexiones** en el **Explorador de soluciones** y, después, seleccione **Nuevo administrador de conexiones**.
1. En el cuadro de diálogo **Agregar administrador de conexiones SSIS**, seleccione **FLATFILE** y, después, haga clic en **Agregar**.
  
2.  En el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, en **Nombre del administrador de conexiones**, escriba **Sample Flat File Source Data**.  
  
3.  Haga clic en **Examinar**.  
  
4.  En el cuadro de diálogo **Abrir**, busque el archivo **SampleCurrencyData.txt** en el equipo.  
  
5.  Desactive la casilla **Nombres de columna en la primera fila de datos**.  
  
### <a name="set-locale-sensitive-properties"></a>Establecimiento de propiedades sensibles a la configuración regional  
  
1.  En el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, haga clic en **General**.  
  
2.  Establezca **Configuración regional** en **Inglés (Estados Unidos)** y **Página de códigos** en **1252**.  
  
### <a name="rename-columns-in-the-flat-file-connection-manager"></a>Cambio del nombre de las columnas del administrador de conexiones de archivos planos  
  
1.  En el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, haga clic en **Avanzadas**.  
  
2.  En el panel de propiedades, realice los cambios siguientes:  
  
    -   Cambie la propiedad de nombre **Columna 0** por **AverageRate**.  
  
    -   Cambie la propiedad de nombre **Columna 1** por **CurrencyID**.  
  
    -   Cambie la propiedad de nombre **Columna 2** por **CurrencyDate**.  
  
    -   Cambie la propiedad de nombre **Columna 3** por **EndOfDayRate**.  
  
### <a name="remap-column-data-types"></a>Reasignación de los tipos de datos de columna  
  
De manera predeterminada, las cuatro columnas están inicialmente establecidas en el tipo de datos de cadena [DT_STR] con un **OutputColumnWidth** de 50.  

1.  En el cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, haga clic en **Sugerir tipos**.  
  
    [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sugiere de forma automática los tipos de datos adecuados en función de las 200 primeras filas de datos. También puede cambiar estas opciones de sugerencia para obtener más o menos datos de ejemplo, especificar el tipo de datos predeterminado para datos enteros o booleanos, o agregar espacios como relleno para las columnas de cadena.  
  
    De momento, no cambie las opciones del cuadro de diálogo **Sugerir tipos de columna** y haga clic en **Aceptar** para que [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sugiera los tipos de datos para las columnas. Esta acción le devuelve al panel **Avanzadas** del cuadro de diálogo **Editor del administrador de conexiones de archivos planos**, donde puede ver los tipos de datos de columna sugeridos por [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Como solución alternativa, si hace clic en **Cancelar**, no se realizan sugerencias de los metadatos de columna y se usa el tipo de datos predeterminado de cadena (DT_STR).  
  
    En este tutorial, [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] sugiere los tipos de datos que se muestran en la segunda columna de la siguiente tabla para los datos del archivo SampleCurrencyData.txt. En la cuarta columna se proporcionan los tipos de datos necesarios para las columnas en el destino, que se definen en un paso posterior.  
  
    |Columna de archivo plano|Tipo sugerido|Columna de destino|Tipo de destino|  
    |--------------------|------------------|----------------------|--------------------|  
    |AverageRate|float [DT_R4]|FactCurrencyRate.AverageRate|FLOAT|  
    |CurrencyID|string [DT_STR]|DimCurrency,CurrencyAlternateKey|nchar(3)|  
    |CurrencyDate|date [DT_DATE]|DimDate.FullDateAlternateKey|date|  
    |EndOfDayRate|float [DT_R4]|FactCurrencyRate.EndOfDayRate|FLOAT|  
  
    El tipo de datos sugerido para la columna **CurrencyID** no es compatible con el tipo de datos del campo de la tabla de destino. Como el tipo de datos de `DimCurrency.CurrencyAlternateKey` es nchar(3), **CurrencyID** debe cambiarse de cadena [DT_STR] a cadena Unicode [DT_WSTR]. Además, el campo `DimDate.FullDateAlternateKey` está definido como un tipo de datos de fecha, por lo que se debe cambiar **CurrencyDate** del tipo fecha [DT_Date] al tipo fecha de base de datos [DT_DBDATE].  
  
2.  En la lista, seleccione la columna **CurrencyID** y, en el panel de propiedades, cambie el tipo de datos de la columna **CurrencyID** de cadena [DT_STR] a cadena Unicode [DT_WSTR].  
  
3.  En el panel de propiedades, cambie el tipo de datos de la columna **CurrencyDate** de fecha [DT_DATE] a fecha de base de datos [DT_DBDATE].  
  
4.  Seleccione **Aceptar**.  
  
## <a name="go-to-next-task"></a>Ir a la tarea siguiente
[Paso 3: Adición y configuración de un administrador de conexiones OLE DB](../integration-services/lesson-1-3-adding-and-configuring-an-ole-db-connection-manager.md)  
  
## <a name="see-also"></a>Consulte también  
[Administrador de conexiones de archivos planos](../integration-services/connection-manager/flat-file-connection-manager.md)  
[Tipos de datos de Integration Services](../integration-services/data-flow/integration-services-data-types.md)  
  
  
  
