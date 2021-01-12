---
description: Destino de procesamiento de particiones
title: Destino de procesamiento de particiones | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.partitionprocessingdest.f1
- sql13.dts.designer.partprocessingtransformation.connection.f1
- sql13.dts.designer.partprocessingtransformation.mapping.f1
- sql13.dts.designer.partprocessingtransformation.advanced.f1
helpviewer_keywords:
- partitions [Analysis Services], processing
- Partition Processing destination [Integration Services]
- destinations [Integration Services], Partition Processing
ms.assetid: 36c592ff-3f78-4a58-b496-31c1c8eee131
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a3ee00006285ff8959ecba27624a491954e5a1bc
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091649"
---
# <a name="partition-processing-destination"></a>Destino de procesamiento de particiones

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El destino de procesamiento de particiones carga y procesa una partición de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Para más información sobre particiones, vea [Particiones &#40;Analysis Services - Datos multidimensionales&#41;](/analysis-services/multidimensional-models-olap-logical-cube-objects/partitions-analysis-services-multidimensional-data).  
  
 El destino de procesamiento de particiones incluye las siguientes características:  
  
-   Opciones para ejecutar procesamiento incremental, completo o de actualización.  
  
-   Configuración de errores, para especificar si el procesamiento debe omitir los errores o detenerse después de una cantidad de errores especificada.  
  
-   Asignación de columnas de entrada para columnas de particiones.  
  
 Para más información sobre el procesamiento de objetos de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], vea [Opciones y valores de procesamiento &#40;Analysis Services&#41;](/analysis-services/multidimensional-models/processing-options-and-settings-analysis-services).  
  
> [!NOTE]  
>  Las tareas aquí descritas no se aplican a los modelos tabulares de Analysis Services.  No se pueden asignar las columnas de entrada a las de partición para los modelos tabulares. En su lugar puede usar la tarea Ejecutar DDL de Analysis Services [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) para procesar la partición.  
  
## <a name="configuration-of-the-partition-processing-destination"></a>Configuración del destino de procesamiento de particiones  
 El destino de procesamiento de particiones usa un administrador de conexiones de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] para conectarse al proyecto de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] o la instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que contiene los cubos y particiones que procesa el destino. Para más información, consulte [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md).  
  
 Este destino tiene una entrada. No admite una salida de error.  
  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 El cuadro de diálogo **Editor avanzado** indica las propiedades que se pueden establecer mediante programación. Para obtener más información acerca de las propiedades que puede establecer a través del cuadro de diálogo **Editor avanzado** o mediante programación, haga clic en uno de los temas siguientes:  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Propiedades personalizadas del destino de procesamiento de particiones](../../integration-services/data-flow/partition-processing-destination-custom-properties.md)  
  
 Para más información sobre cómo establecer las propiedades, vea [Establecer las propiedades de un componente de flujo de datos](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md).  
  
## <a name="partition-processing-destination-editor-connection-manager-page"></a>Editor de destino de procesamiento de particiones (página Administrador de conexiones)
  Use la página **Administrador de conexiones** del cuadro de diálogo **Editor de destino de procesamiento de particiones** para especificar una conexión a un proyecto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] o a una instancia de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
> [!NOTE]  
>  Las tareas aquí descritas no se aplican a los modelos tabulares de Analysis Services.  No se pueden asignar las columnas de entrada a las de partición para los modelos tabulares. En su lugar puede usar la tarea Ejecutar DDL de Analysis Services [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) para procesar la partición.  
  
### <a name="options"></a>Opciones  
 **Connection manager**  
 Seleccione un administrador de conexiones de la lista o cree una conexión haciendo clic en **Nuevo**.  
  
 **Nuevo**  
 Permite crear una conexión con el cuadro de diálogo **Agregar administrador de conexiones de Analysis Services** .  
  
 **Lista de particiones disponibles**  
 Seleccione la partición para procesar.  
  
 **Método de procesamiento**  
 Seleccione el método de procesamiento. El valor predeterminado de esta opción es **Completa**.  
  
|Value|Descripción|  
|-----------|-----------------|  
|Agregar (incremental)|Realiza un procesamiento incremental de la partición.|  
|Completo|Realiza un procesamiento completo de la partición.|  
|Solo datos|Realiza un procesamiento de actualización de la partición.|  
  
## <a name="partition-processing-destination-editor-mappings-page"></a>Editor de destino de procesamiento de particiones (página Asignaciones)
  Utilice la página **Asignaciones** del cuadro de diálogo **Editor de destino de procesamiento de particiones** para asignar columnas de entrada a columnas de partición.  
  
> [!NOTE]  
>  Las tareas aquí descritas no se aplican a los modelos tabulares de Analysis Services.  No se pueden asignar las columnas de entrada a las de partición para los modelos tabulares. En su lugar puede usar la tarea Ejecutar DDL de Analysis Services [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) para procesar la partición.  
  
### <a name="options"></a>Opciones  
 **Columnas de entrada disponibles**  
 Muestra la lista de columnas de entrada disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de entrada disponibles de la tabla a columnas de destino.  
  
 **Columnas de destino disponibles**  
 Muestra la lista de columnas de destino disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de destino disponibles de la tabla a columnas de entrada.  
  
 **Columna de entrada**  
 Muestra las columnas de entrada seleccionadas de la tabla anterior. Puede cambiar las asignaciones utilizando la lista de **Columnas de entrada disponibles**.  
  
 **Columna de destino**  
 Muestra las columnas de destino disponibles, independientemente de si están asignadas o no.  
  
## <a name="partition-processing-destination-editor-advanced-page"></a>Editor de destino de procesamiento de particiones (página Avanzadas)
  Utilice la página **Avanzadas** del cuadro de diálogo **Editor de destino de procesamiento de particiones** para configurar el control de errores.  
  
> [!NOTE]  
>  Las tareas aquí descritas no se aplican a los modelos tabulares de Analysis Services.  No se pueden asignar las columnas de entrada a las de partición para los modelos tabulares. En su lugar puede usar la tarea Ejecutar DDL de Analysis Services [Analysis Services Execute DDL Task](../../integration-services/control-flow/analysis-services-execute-ddl-task.md) para procesar la partición.  
  
### <a name="options"></a>Opciones  
 **Utilizar la configuración de error predeterminada**  
 Especifica si debe utilizarse el control de errores predeterminado de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Este valor es **True** de forma predeterminada.  
  
 **Acción del error de clave**  
 Permite especificar la forma de controlar registros que tienen valores de clave no aceptables.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**ConvertToUnknown**|Convierte el valor de clave no aceptable en el valor Unknown.|  
|**DiscardRecord**|Descarta el registro.|  
  
 **Omitir errores**  
 Especifica que deben omitirse los errores.  
  
 **Detenerse ante errores**  
 Especifica que el procesamiento debe detenerse cuando se produce un error.  
  
 **Número de errores**  
 Especifica el umbral de error donde debe detenerse el procesamiento, en caso de que se haya seleccionado **Detenerse ante errores**.  
  
 **Acción ante el error**  
 Especifica la acción que debe llevarse a cabo cuando se alcanza el umbral de error, en caso de que se haya seleccionado **Detenerse ante errores**.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**StopProcessing**|Detiene el procesamiento.|  
|**StopLogging**|Detiene el registro de errores.|  
  
 **Clave no encontrada**  
 Especifica la acción que debe llevarse a cabo en caso de que se produzca un error de clave no encontrada. Este valor es **ReportAndContinue** de forma predeterminada.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**IgnoreError**|Omite el error y reanuda el procesamiento.|  
|**ReportAndContinue**|Informa del error y reanuda el procesamiento.|  
|**ReportAndStop**|Informa del error y detiene el procesamiento.|  
  
 **Clave duplicada**  
 Especifica la acción que debe llevarse a cabo en caso de que se produzca un error de clave duplicada. Este valor es **IgnoreError** de forma predeterminada.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**IgnoreError**|Omite el error y reanuda el procesamiento.|  
|**ReportAndContinue**|Informa del error y reanuda el procesamiento.|  
|**ReportAndStop**|Informa del error y detiene el procesamiento.|  
  
 **Clave NULL convertida en desconocida**  
 Especifica la acción que debe llevarse a cabo cuando una clave NULL se ha convertido al valor Unknown. Este valor es **IgnoreError** de forma predeterminada.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**IgnoreError**|Omite el error y reanuda el procesamiento.|  
|**ReportAndContinue**|Informa del error y reanuda el procesamiento.|  
|**ReportAndStop**|Informa del error y detiene el procesamiento.|  
  
 **Clave NULL no permitida**  
 Especifica la acción que debe llevarse a cabo cuando no se permiten claves NULL y se encuentra una clave NULL. Este valor es **ReportAndContinue** de forma predeterminada.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**IgnoreError**|Omite el error y reanuda el procesamiento.|  
|**ReportAndContinue**|Informa del error y reanuda el procesamiento.|  
|**ReportAndStop**|Informa del error y detiene el procesamiento.|  
  
 **Ruta de acceso del registro de errores**  
 Escriba la ruta de acceso al registro de errores o seleccione un destino mediante el botón para examinar **(…)** .  
  
 **Examinar (...)**  
 Seleccione una ruta de acceso para el registro de errores.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y mensajes de Integration Services](../../integration-services/integration-services-error-and-message-reference.md)