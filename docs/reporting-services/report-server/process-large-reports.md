---
title: Procesar informes grandes | Microsoft Docs
description: Obtenga información sobre cómo ejecutar correctamente informes grandes en el Servidor de informes siguiendo las recomendaciones de configuración, representación, implementación y distribución.
ms.date: 06/10/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- report processing [Reporting Services], large reports
- page breaks [Reporting Services]
- large reports
- size [SQL Server], reports
- distributing reports [Reporting Services], large reports
ms.assetid: c5275a9f-c95b-46d7-bc62-633879a8a291
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 022751a3ec4b47a8e7a3a5f5295306265f659512
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594981"
---
# <a name="process-large-reports"></a>Procesar informes grandes
  Los informes de gran tamaño presentan determinados problemas de procesamiento y requieren determinadas configuraciones para que se ejecuten correctamente. Estos informes no deben ejecutarse a petición a menos que estén configurados para admitir paginación.  
  
> [!NOTE]  
>  Los saltos de página se habilitan de forma predeterminada. No deshabilite los saltos de página si cree que el informe tendrá gran cantidad de datos. El formato de representación en HTML que se utiliza para representar inicialmente un informe abre un informe en un explorador. Si el informe no está paginado, todos los datos se incluyen en una sola página, que no es válida para la mayoría de los exploradores. Por ejemplo, es casi seguro que un informe que contenga 5.000 filas de datos no pueda visualizarse en un explorador en una sola página.  
  
 Si trabaja con un informe de gran tamaño, debe elegir opciones de ejecución, representación y entrega de informe válidas para documentos de gran tamaño. El tamaño del informe está determinado en gran medida por el conjunto de filas que devuelve la consulta y la extensión de representación que se utiliza para presentar el informe.  
  
 En el caso de informes que incluyen datos volátiles, el tamaño puede variar drásticamente de una ejecución a otra del informe. En este caso, debe supervisar el origen de datos para determinar cómo afecta la volatilidad de los datos al informe y si es preciso seguir los pasos que se exponen en este tema.  
  
 Para obtener más información y sugerencias sobre cómo diagnosticar errores de tiempo de espera y errores de memoria insuficiente, vea el artículo sobre [cómo diagnosticar problemas cuando se ejecutan informes en el servidor de informes](/archive/blogs/lukaszp/) en blogs.msdn.microsoft.com.  
  
## <a name="configuration-recommendations"></a>Recomendaciones para la configuración  
 Entre las recomendaciones para la ejecución de informes, la representación de informes y el acceso a los informes, se contemplan los siguientes aspectos:  
  
-   El informe debe diseñarse para que admita paginación. El servidor de informes envía los informes página por página. Si el informe incluye paginación, podrá controlar el volumen de datos que se envía al explorador. Para más información, vea [Cargar previamente la memoria caché (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md).  
  
-   El informe debe configurarse para que se ejecute como una instantánea de informe programado y evitar que se ejecute a petición. No establezca un valor de tiempo de espera para la ejecución del informe. Ejecute el informe durante las horas de menor actividad.  
  
-   El informe debe configurarse para que utilice un origen de datos compartido cuando se desee controlar si se va a procesar el informe. Una de las ventajas de utilizar un origen de datos compartido es que el usuario puede deshabilitarlo. Deshabilitar el origen de datos también evita el procesamiento del informe.  
  
-   El historial del informe debe deshabilitarse para conservar espacio en disco. Para deshabilitar el historial del informe, desactive todas las casillas de la página de propiedades Historial.  
  
-   El acceso al informe debe limitarse. Configure el informe para que utilice seguridad de nivel de elemento y reemplace las asignaciones de roles predeterminados por otras nuevas que solo permitan el acceso a los usuarios que lo necesitan.  
  
     De forma predeterminada, los usuarios pueden abrir cualquier informe que vean en la jerarquía de carpetas. Incluso si configura un informe para que se ejecute como una instantánea, los usuarios que puedan ver el elemento de informe en una carpeta también podrán abrir el informe. Si el informe es muy grande, podría hacer que el explorador deje de responder cuando un usuario abra el informe con el portal web.  
  
## <a name="rendering-recommendations"></a>Recomendaciones para la representación  
 Antes de configurar la distribución de los informes es importante saber qué clientes de representación pueden trabajar con documentos de gran tamaño. El formato recomendado es la extensión de representación en HTML predeterminada con salto de página automático, pero puede elegir cualquier formato que admita paginación.  
  
 El rendimiento y el consumo de memoria varían según cada uno de los formatos de representación. El mismo informe se representará a diferentes velocidades y requerirá distintas cantidades de memoria en función del formato que seleccione. Los formatos más rápidos y que requieren menor cantidad de memoria son CSV, XML y HTML. PDF y Excel tienen el rendimiento más bajo, pero por diferentes motivos. PDF hace un uso importante de CPU, mientras que Excel utiliza una gran cantidad de RAM. Las representaciones de imágenes se encuentra entre los dos grupos. Puede especificar el formato al definir el modo de distribución del informe.  
  
## <a name="deployment-and-distribution-recommendations"></a>Recomendaciones para la implementación y la distribución  
 Si utiliza saltos de página para controlar la representación de informe, puede implementar un informe de gran tamaño del mismo modo que implementaría cualquier informe. Puede proporcionar acceso al informe mediante el portal web, un elemento web de SharePoint o una dirección URL que se agrega a un portal o sitio web. Todas estas opciones de implementación admiten acceso a petición, así como también una instantánea de informe ejecutada con anterioridad.  
  
 Una estrategia de implementación alternativa es distribuir informes a usuarios individuales. Puede distribuir informes de gran tamaño mediante suscripciones si configura las opciones de entrega con cuidado. Puede utilizar una suscripción estándar o una controlada por datos para efectuar la entrega del informe. Entre las recomendaciones de suscripción y entrega, se incluyen los siguientes aspectos:  
  
-   Debe configurarse una suscripción que utilice un archivo web (MHTML), PDF o Excel.  
  
-   Debe configurarse una suscripción que utilice la entrega a recursos compartidos de archivos si utiliza PDF o Excel. Una vez entregado el informe, puede utilizar una aplicación de escritorio para trabajar con el informe. Debe establecer los permisos para el recurso compartido de archivos a fin de determinar quién puede ver el informe.  
  
     Tenga en cuenta que, después de ubicar el informe en el recurso compartido de archivos, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]deja de controlarlo o mantener su seguridad. Si desea recibir una notificación cuando se actualice el informe, cree una segunda suscripción que utilice la entrega por correo electrónico para enviar solo una notificación.  
  
 Si desea utilizar la entrega de informes por correo electrónico, configure la suscripción para que incluya un vínculo. Evite enviar el informe como datos adjuntos.  
  
## <a name="see-also"></a>Consulte también  
 [Suscripciones y entrega &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)   
 [Establecer las propiedades del procesamiento de informes](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Especificar información de credenciales y conexión para los orígenes de datos de informes](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
 [Administración de contenido del servidor de informes &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Carga previa de la memoria caché (SSRS)](../../reporting-services/report-server/preload-the-cache-report-manager.md)