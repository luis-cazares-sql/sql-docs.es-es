---
title: Límites de tamaño de informes e instantáneas | Microsoft Docs
description: Obtenga información sobre los límites de tamaño del informe cuando se publica en el servidor de informes, se representa en tiempo de ejecución y se guarda en el sistema de archivos.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- large reports
- maximum report size
- size [SQL Server], reports
- report size [Reporting Services]
- reports [Reporting Services], size
- denial of service attacks [Reporting Services]
ms.assetid: 1e3be259-d453-4802-b2f5-6b81ef607edf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5b5e8dbaee71c53f8f9491f8a1abcd15559e4800
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596840"
---
# <a name="report-and-snapshot-size-limits"></a>Límites de tamaño de informes e instantáneas
  Los responsables de administrar una implementación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pueden usar la información de este tema para conocer los límites de tamaño de los informes cuando se publican en un servidor de informes, cuando se representan en tiempo de ejecución y cuando se guardan en el sistema de archivos. En este tema también se incluyen directrices prácticas para medir el tamaño de una base de datos del servidor de informes y se describe cómo afecta el tamaño de las instantáneas al rendimiento del servidor.  
  
## <a name="maximum-size-for-published-reports"></a>Tamaño máximo de los informes publicados  
 En el servidor de informes, el tamaño de los informes y modelos se basa en el tamaño de los archivos de definición de informe (.rdl) y modelo de informe (.smdl) publicados. El servidor de informes no limita el tamaño de un informe que haya publicado. Sin embargo, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] impone un tamaño máximo para los elementos que se publican en el servidor. El valor predeterminado de este límite es de 4 megabytes (MB). Si se carga o publica un archivo que supera este límite en un servidor de informes, se generará una excepción HTTP. En este caso, para modificar el valor predeterminado, se puede aumentar el valor del elemento **maxRequestLength** en el archivo Machine.config.  
  
 Aunque un modelo de informe puede ser muy grande, las definiciones de informe no suelen ser superiores a 4 MB. El tamaño más habitual de un informe suele ser del orden de kilobytes (KB). Sin embargo, si se incluyen imágenes incrustadas, su codificación puede dar lugar a definiciones de informe de gran tamaño, que superan el valor predeterminado de 4 MB.  
  
 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] impone un límite máximo en los archivos publicados para reducir la amenaza de ataques de denegación de servicio en el servidor. Al aumentar el valor del límite superior, se elimina parte de la protección que ofrece este límite. Aumente el valor solo si está seguro de que las ventajas de hacerlo compensan los riesgos de seguridad adicionales.  
  
 Tenga en cuenta que el valor establecido para el elemento **maxRequestLength** debe ser mayor que los límites de tamaño reales que desea aplicar. Tiene que usar el valor más grande para tener en cuenta el aumento inevitable del tamaño de la solicitud HTTP que se produce una vez encapsulados todos los parámetros en un sobre SOAP y aplicada la codificación Base64 a ciertos parámetros. La codificación Base64 aumenta el tamaño de los datos originales en un 33 % aproximadamente. Por tanto, el valor que especifique para el elemento **maxRequestLength** debe ser aproximadamente un 33 % mayor que el tamaño del elemento utilizable real. Por ejemplo, si especifica un valor de 64 MB para **maxRequestLength**, puede esperar de forma realista que el tamaño máximo de los archivos de informe que se envían al servidor de informes sea aproximadamente 48 MB.  
  
## <a name="report-size-in-memory"></a>Tamaño de informes en memoria  
 Cuando se ejecuta un informe, el tamaño del informe es igual a la cantidad de datos que se devuelven en el informe más el tamaño del flujo de salida. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no impone un límite máximo en cuanto al tamaño de un informe representado. La memoria del sistema determina el límite superior del tamaño (de forma predeterminada, un servidor de informes usa toda la memoria configurada disponible al representar un informe), pero puede especificar los ajustes de configuración para establecer umbrales de memoria y directivas de administración de memoria. Para obtener más información, vea [Configurar la memoria disponible para las aplicaciones del servidor de informes](../../reporting-services/report-server/configure-available-memory-for-report-server-applications.md).  
  
 El tamaño de un informe puede variar considerablemente en función de la cantidad de datos devueltos y del formato de representación que se utilice para el informe. Un informe con parámetros puede ser mayor o menor en función de cómo afectan los valores de los parámetros a los resultados de la consulta. El formato de salida que se elija para el informe influye en su tamaño de la manera siguiente:  
  
-   HTML procesa el informe página por página. Dado que el informe se procesa en unidades más pequeñas, se requiere menos memoria para procesar fragmentos específicos.  
  
-   PDF, Excel, TIFF, XML y CSV procesan el informe completo en memoria antes de mostrarlo al usuario.  
  
 Para medir el tamaño de un informe representado se puede ver el registro de la ejecución de informes. Para obtener más información, vea [Registro de ejecución del servidor de informes y la vista ExecutionLog3](../../reporting-services/report-server/report-server-executionlog-and-the-executionlog3-view.md).  
  
 Para calcular el tamaño de un informe representado en el disco, se puede exportar el informe y, después, guardarlo en el sistema de archivos (el archivo guardado incluye los datos y la información de formato del informe).  
  
 Existen límites claros para el tamaño de un informe cuando se representa en formato Excel. Las hojas de cálculo no pueden tener más de 65536 filas o 256 columnas. Los demás formatos de representación no tienen estos límites, por lo que su tamaño solamente depende de la cantidad de recursos del servidor.  
  
> [!NOTE]  
>  El procesamiento y la representación de los informes se producen en memoria. Si los informes son grandes o hay muchos usuarios, asegúrese de planear la capacidad de alguna manera para garantizar que la implementación del servidor de informes funciona en un grado que sea satisfactorio para los usuarios. Para obtener más información sobre herramientas e instrucciones, vea las publicaciones siguientes en MSDN: [Planning for Scalability and Performance with Reporting Services](/previous-versions/sql/sql-server-2005/administrator/cc966418(v=technet.10)) y [Using Visual Studio 2005 to Perform Load Testing on a SQL Server 2005 Reporting Services Report Server](/previous-versions/sql/sql-server-2005/administrator/aa964139(v=sql.90)).  
  
## <a name="measuring-snapshot-storage"></a>Medir el almacenamiento de instantáneas  
 El tamaño de una instantánea es directamente proporcional a la cantidad de datos del informe. Las instantáneas suelen ser mucho mayores que otros elementos que se almacenan en un servidor de informes. El tamaño de una instantánea puede ser de algunos megabytes a decenas de megabytes. Si los informes son muy grandes, es de esperar que haya instantáneas incluso mayores. En función de la frecuencia con la que se utilicen las instantáneas y de cómo se haya configurado el historial de informes, la cantidad de espacio de disco que requiere la base de datos del servidor de informes puede crecer rápidamente en poco tiempo.  
  
 De manera predeterminada, las bases de datos **reportserver** y **reportservertempdb** están configuradas para el crecimiento automático. Aunque el tamaño de la base de datos puede aumentar automáticamente, nunca se reduce de forma automática. Si la base de datos **reportserver** tiene capacidad de sobra debido a que se han eliminado instantáneas, ésta deberá reducirse manualmente para recuperar el espacio en el disco. De manera similar, si **reportservertempdb** ha crecido para admitir un volumen excepcionalmente alto de informes interactivos, la asignación de espacio de disco se mantendrá en ese valor hasta que se reduzca.  
  
 Para medir el tamaño de las bases de datos del servidor de informes, se pueden ejecutar los siguientes comandos [!INCLUDE[tsql](../../includes/tsql-md.md)] . Si el tamaño total de una base de datos se calcula a intervalos regulares, esto puede ayudar a obtener una estimación razonable de cómo asignar el espacio para la base de datos del servidor de informes a lo largo del tiempo. Las instrucciones siguientes miden la cantidad de espacio que se utiliza actualmente (se asume que se utilizan los nombres de base de datos predeterminados):  
  
```  
USE ReportServer  
EXEC sp_spaceused  
```  
  
## <a name="snapshot-size-and-report-server-performance"></a>Tamaño de instantáneas y rendimiento del servidor de informes  
 El tamaño de las instantáneas afecta al rendimiento del servidor cuando se procesa y representa el informe. El rendimiento del servidor se ve principalmente afectado por las operaciones de representación, por lo que, si tiene una instantánea grande, cabe esperar un cierto retardo cuando los usuarios soliciten el informe. En función del número de usuarios, se pueden prever retardos cuando el tamaño de instantánea sea superior a 100 megabytes.  
  
 Para reducir al mínimo los retardos de rendimiento debidos a instantáneas grandes, se pueden realizar los pasos siguientes:  
  
-   Implemente el servidor de informes y el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en equipos independientes.  
  
-   Agregar más memoria de sistema.  
  
-   En el documento en el que se explica cómo planear la escalabilidad y el rendimiento con Reporting Services ("Planning for Scalability and Performance with Reporting Services"), disponible en el sitio web de MSDN, encontrará las prácticas recomendadas para configurar un servidor de informes para una empresa.  
  
 El número de instantáneas que se almacenan en una base de datos del servidor de informes no es de por sí un factor de rendimiento. Se pueden almacenar muchas instantáneas sin que el rendimiento del servidor se vea afectado. Las instantáneas se pueden conservar de manera indefinida. No obstante, debe tenerse en cuenta que el historial de informes es configurable. Si un administrador del servidor de informes reduce el límite del historial de informes, pueden perderse informes históricos que se pensaba conservar. Si se elimina el informe, se elimina también el historial.  
  
## <a name="see-also"></a>Consulte también  
 [Establecer las propiedades del procesamiento de informes](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Base de datos del servidor de informes &#40;Modo nativo de SSRS&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
 [Procesamiento de informes grandes](../../reporting-services/report-server/process-large-reports.md)  
  
