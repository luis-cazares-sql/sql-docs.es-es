---
description: Informe de evaluación (DB2ToSQL)
title: Informe de evaluación (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9e13eba0-e3cf-4205-974f-c00f982061de
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bea3b1c684457bc794d1b35d3a7985b79da41bef
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88418521"
---
# <a name="assessment-report-db2tosql"></a>Informe de evaluación (DB2ToSQL)
La ventana Informe de evaluación muestra los resultados de la conversión de objetos de base de datos a [!INCLUDE[tsql](../../includes/tsql-md.md)] sintaxis y también puede ayudarle a calcular la complejidad y el costo de los proyectos de migración.  
  
Para tener acceso al informe de evaluación, seleccione los objetos que desea convertir en el explorador de metadatos de origen, haga clic con el botón derecho en **esquemas** o **sinónimos**y, a continuación, seleccione **crear informe**.  
  
## <a name="options"></a>Opciones  
  
|Término|Definición|  
|-|-|  
|**Estadísticas de conversión**|Muestra las estadísticas de conversión por tipo de instrucción. Este panel está visible cuando se selecciona un objeto de grupo, como un esquema, o un objeto sin código en el panel izquierdo.|  
|**Objetos por categorías**|Muestra el número de objetos por categoría. Este panel solo es visible cuando se selecciona un objeto de grupo, como un esquema, o un objeto sin código en el panel izquierdo.|  
|**estadísticas**|Muestra las estadísticas de conversión del objeto seleccionado. Este panel solo es visible cuando se selecciona un objeto individual con código en el panel izquierdo. Es posible que tenga que expandir **estadísticas**, que se encuentra justo encima del panel **origen** , para ver este panel.|  
|**Origen**|Muestra el código DB2 para el objeto seleccionado y resalta el código que no se convirtió en [!INCLUDE[tsql](../../includes/tsql-md.md)] . Este panel solo es visible cuando se selecciona un objeto individual con código en el panel izquierdo.<br /><br />Haga clic en los números de línea para establecer o Borrar marcadores. Use los botones situados en la parte superior del panel para navegar por el código.|  
|**Destino**|Muestra el código resultante de [!INCLUDE[tsql](../../includes/tsql-md.md)] la conversión para el objeto seleccionado y los mensajes de error para el código que no se ha convertido. Este panel solo es visible cuando se selecciona un objeto individual con código en el panel izquierdo.<br /><br />Haga clic en los números de línea para establecer o Borrar marcadores. Use los botones situados en la parte superior del panel para navegar por el código.|  
|**Panel Mensajes**|Muestra los errores, las advertencias y los mensajes informativos que se generaron al crear el informe de evaluación. Los mensajes se agrupan por número. Para ver el código que causó el error, haga clic en **errores**, **advertencias**o **información**, expanda la categoría de mensajes y, a continuación, haga clic en un mensaje.|  
  
