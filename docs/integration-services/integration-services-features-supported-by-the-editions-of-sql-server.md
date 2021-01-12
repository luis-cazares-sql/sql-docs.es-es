---
description: Características de Integration Services compatibles con las ediciones de SQL Server
title: Características de Integration Services compatibles con las ediciones de SQL Server| Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: e5018225-68bb-4f34-ae4a-ead79d8ad13a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2a8b60c36831622713103786727c4225ac0c351c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095677"
---
# <a name="integration-services-features-supported-by-the-editions-of-sql-server"></a>Características de Integration Services compatibles con las ediciones de SQL Server

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


 En este tema, encontrará información detallada sobre las características de SQL Server Integration Services (SSIS) admitidas por las diversas ediciones de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)].  

Para obtener las características compatibles con las ediciones Developer y Evaluation, consulte las características enumeradas para Enterprise Edition en las tablas siguientes.
  
Para consultar las notas de la versión más reciente e información sobre las novedades, consulte los artículos siguientes:
-   [Notas de la versión de SQL Server 2016](../sql-server/sql-server-2016-release-notes.md)
-   [Novedades de Integration Services en SQL Server 2016](../integration-services/what-s-new-in-integration-services-in-sql-server-2016.md)
-   [Novedades de Integration Services en SQL Server 2017](../integration-services/what-s-new-in-integration-services-in-sql-server-2017.md)
    
**Pruebe SQL Server 2016.**    

La edición de evaluación de SQL Server está disponible durante un período de prueba de 180 días.  
    
> [![Descargar desde el Centro de evaluación](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016) **[Descargar SQL Server 2016 desde el Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**    
    
## <a name="new-integration-services-features-in-sql-server-2017"></a><a name="ISNew"></a> Nuevas características de Integration Services en SQL Server 2017
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Patrón de escalabilidad horizontal|Sí|||||
|Trabajo de escalabilidad horizontal|Sí|Sí <sup>1</sup>|TBD|TBD|TBD|
|Compatibilidad con Microsoft Dynamics AX y Microsoft Dynamics CRM en componentes de OData <sup>2</sup>|Sí|Sí||||
|Compatibilidad de Linux|Sí|Sí|||Sí|

<sup>1</sup> Si ejecuta paquetes que solo requieren características de la versión Enterprise en la escalabilidad horizontal, los trabajos de escalabilidad horizontal también deben ejecutarse en instancias de SQL Server Enterprise.

<sup>2</sup> Esta característica también se admite en SQL Server 2016 con Service Pack 1.

## <a name="sql-server-import-and-export-wizard"></a><a name="IEWiz"></a> Asistente para importación y exportación de SQL Server

|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Asistente para importación y exportación de SQL Server|Sí|Sí|Sí|Sí<sup>1</sup>|Sí<sup>1</sup>|

<sup>1</sup> DTSWizard.exe no se proporciona con SQL en Linux. Pero se puede usar dtexec en Linux para ejecutar un paquete creado por DTSWizard en Windows.


## <a name="integration-services"></a><a name="IS"></a> Integration Services  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Conectores de origen de datos integrados|Sí|Sí|||| 
|Tareas y transformaciones integradas|Sí|Sí||||  
|Origen y destino de ODBC |Sí|Sí|||| 
|Tareas y conectores de origen de datos de Azure|Sí|Sí||||  
|Conectores y tareas de Hadoop/HDFS|Sí|Sí||||  
|Herramientas de generación de perfiles de datos básicos|Sí|Sí|||| 

## <a name="integration-services---advanced-sources-and-destinations"></a><a name="ISAA"></a> Integration Services: orígenes y destinos avanzados  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Origen y de destino de Oracle de alto rendimiento de Attunity|Sí|||||  
|Origen y de destino de Teradata de alto rendimiento de Attunity|Sí|||||  
|Origen y destino de SAP BW|Sí|||||  
|Destino de entrenamiento del modelo de minería de datos|Sí|||||  
|Destino de procesamiento de dimensiones|Sí|||||  
|Destino de procesamiento de particiones|Sí|||||  
  
## <a name="integration-services---advanced-tasks-and-transformations"></a><a name="ISAT"></a> Integration Services: transformaciones y tareas avanzadas  
  
|Característica|Enterprise|Estándar|Web|Express con Advanced Services|Express|  
|-------------|----------------|--------------|---------|------------------------------------|------------------------|  
|Componentes de Change Data Capture de Attunity <sup>1</sup>|Sí|||||  
|Transformación de consulta de minería de datos|Sí|||||  
|Transformaciones de búsqueda y agrupación aproximadas|Sí|||||  
|Transformaciones de extracción y búsqueda de términos|Sí|||||  

<sup>1</sup> Los componentes de Change Data Capture de Attunity necesitan la edición Enterprise. Sin embargo, Change Data Capture Service y Change Data Capture Designer no necesitan la edición Enterprise. Puede usar el diseñador y el servicio en un equipo donde SSIS no esté instalado.