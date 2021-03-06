---
title: Adición o eliminación de un indicador (Generador de informes) | Microsoft Docs
description: Obtenga información sobre cómo agregar o eliminar un indicador en los informes paginados para transmitir el estado de un único valor de datos en el Generador de informes.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a8b1aac1-53ef-47a4-afc0-8fa866c6c480
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 154faecf24a142578fc236a2cae6330ea7d89a76
ms.sourcegitcommit: 5c7634b007f6808c87094174b80376cb20545d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2020
ms.locfileid: "84881259"
---
# <a name="add-or-delete-an-indicator-report-builder-and-ssrs"></a>Agregar o eliminar un indicador (Generador de informes y SSRS)
  En un informe paginado de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)], los indicadores son medidores mínimos que comunican el estado de un único valor de datos de un vistazo. Para obtener más información sobre ellos, vea [Indicadores &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md).  
  
 Los indicadores normalmente se colocan en celdas en una tabla o matriz, pero también puede utilizarlos solos, uno al lado de otro con medidores o incrustados en medidores.  
  
 Al agregar por primera vez un indicador, de forma predeterminada se configura para utilizar los porcentajes como unidades de medida. Los intervalos de porcentaje se distribuyen uniformemente a través de los miembros del indicador establecido, y el ámbito de valores mostrado por el indicador es el elemento primario del indicador como una tabla o matriz.  
  
 Puede actualizar los valores y los estados de los indicadores. Para obtener más información, vea los temas siguientes:  
  
-   [Cambiar iconos de indicador y conjuntos de indicadores &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-indicator-icons-and-indicator-sets-report-builder-and-ssrs.md)  
  
-   [Establecer y configurar unidades de medida &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/set-and-configure-measurement-units-report-builder-and-ssrs.md)  
  
-   [Establecer el ámbito de sincronización &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/set-synchronization-scope-report-builder-and-ssrs.md)  
  
 Dado que un indicador se coloca dentro del panel de medidor, necesita seleccionar el indicador en lugar del panel si desea configurar el indicador utilizando el cuadro de diálogo **Propiedades de los indicadores** o el panel **Propiedades** . La imagen siguiente muestra un indicador seleccionado en su panel de medidor.  
  
 ![rs_GaugePanelWithIndicator](../../reporting-services/report-design/media/rs-gaugepanelwithindicator.gif "rs_GaugePanelWithIndicator")  
  
> [!NOTE]  
>  En función del ancho de columna y la longitud de los valores de los datos, el texto de la tabla o las celdas de la matriz, podría ajustar el texto y mostrarlo en varias líneas. Cuando esto ocurre, el icono de indicador podría ajustarse y cambiar de forma. Esto puede hacer que el icono del indicador sea menos legible. Coloque el indicador dentro de un rectángulo para asegurarse de que el icono nunca se ajusta.  
  
## <a name="to-add-an-indicator-to-a-table-or-matrix"></a>Agregar un indicador a una tabla o matriz  
  
1.  Abra un informe existente o cree un informe que contenga una tabla y matriz con los datos que desea mostrar. Para más información, vea [Tablas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tables-report-builder-and-ssrs.md) y [Matrices](../../reporting-services/report-design/create-a-matrix-report-builder-and-ssrs.md).  
  
2.  Inserte una columna en la tabla o matriz. Para más información, vea [Insertar o eliminar una columna &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/insert-or-delete-a-column-report-builder-and-ssrs.md).  
  
3.  Si lo desea, en la pestaña **Insertar** , haga clic en **Rectángulo**y, a continuación, haga clic en una celda en la nueva columna.  
  
4.  En la pestaña **Insertar** , haga clic en **Indicador**y, a continuación, haga clic en una celda en la nueva columna.  
  
     Si agregó un rectángulo a una celda, haga clic en ella.  
  
5.  En el cuadro de diálogo **Seleccionar estilo de indicador** , en el panel izquierdo, haga clic en el tipo de indicador que desea y, a continuación, haga clic en el indicador establecido.  
  
6.  Haga clic en **OK**.  
  
7.  Haga clic en el indicador. El panel **Medidor de datos** se abre.  
  
8.  En el área **Valores** , en la lista desplegable **(No especificado)** , haga clic en el campo cuyos valores quiera mostrar como un indicador.  
  
     El indicador se configura para utilizar los valores predeterminados. De forma predeterminada, los indicadores son los porcentajes de uso configurados como unidades de medida, los intervalos de porcentaje se distribuyen uniformemente a través de los miembros del indicador y el valor que el indicador comunica utiliza el ámbito del grupo más cercano.  
  
## <a name="to-delete-an-indicator-to-a-table-or-matrix"></a>Eliminar un indicador para una tabla o matriz  
  
1.  Haga clic con el botón derecho en el indicador y, después, haga clic en **Eliminar**.  
  
    > [!NOTE]  
    >  Un indicador se podría colocar dentro de un panel de medidor que contenga otros indicadores o medidores. Si los paneles de medidor contienen varios elementos, asegúrese de hacer clic en el indicador para eliminarlo, no el panel de medidor. Si hace clic y, a continuación, elimina el panel de medidor, se eliminan todos los paneles de medidor y sus elementos.  
  
2.  Haga clic en **Eliminar**.  
  
## <a name="see-also"></a>Consulte también  
 [Indicadores &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/indicators-report-builder-and-ssrs.md)  
  
  
