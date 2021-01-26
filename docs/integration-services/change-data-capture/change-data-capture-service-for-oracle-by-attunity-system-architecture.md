---
description: Servicio de captura de datos modificados para Oracle de Attunity (Arquitectura del sistema)
title: Change Data Capture Service para Oracle de Attunity (Arquitectura del sistema) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 1db6c737-3c60-4066-a0a3-3611e1c83e4e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a8d184639991b7e7a0d5e837fabe95535fdc08fa
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765679"
---
# <a name="change-data-capture-service-for-oracle-by-attunity-system-architecture"></a>Servicio de captura de datos modificados para Oracle de Attunity (Arquitectura del sistema)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El servicio CDC para Oracle captura los cambios realizados en las tablas seleccionadas de una o varias bases de datos de Oracle de origen en bases de datos CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ubicadas en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En el diagrama siguiente se muestran los componentes que constituyen el servicio CDC para Oracle.  
  
 ![Arquitectura del servicio](../../integration-services/change-data-capture/media/service-architecture.gif "Arquitectura del servicio")  
  
 Esta ilustración muestra cuatro plataformas que se emplean. En muchos casos, estas plataformas se pueden superponer; sin embargo, este diagrama representa un caso de uso estándar. Por ejemplo, conviene que las bases de datos de Oracle y de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuten en equipos diferentes y que no se compartan con la plataforma del servicio CDC de Oracle o con la plataforma desde la que se diseñe el servicio CDC. Las plataformas mostradas en esta ilustración son:  
  
-   El servicio CDC de Oracle: puede ser cualquier equipo compatible con Windows donde se instale y ejecute el servicio CDC de Oracle. Esta plataforma también puede representar un nodo de clúster en un clúster de conmutación por error de Microsoft (las configuraciones de alta disponibilidad se describen más adelante en este documento).  
  
-   La base de datos de Oracle: puede ser cualquier equipo donde se ejecute una versión compatible de la base de datos de Oracle. Esto incluye cualquier equipo que ejecute Windows, Linux o cualquier otro sistema operativo compatible con la versión de la base de datos de Oracle instalada. Tenga en cuenta que el diagrama muestra esta plataforma en plural porque un único servicio CDC de Oracle puede capturar cambios de varias bases de datos de origen de Oracle.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: puede ser cualquier equipo donde se ejecute la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino (SKU compatible de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]). Un servicio CDC de Oracle admite un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde almacena las tablas de cambios y la configuración del servicio. La plataforma [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también puede representar una instancia en clúster de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] o una instancia reflejada de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] que use la característica **AlwaysOn** .  
  
-   El Diseñador CDC de Oracle: puede ser cualquier equipo compatible con Windows que pueda obtener acceso a la base de datos de origen de Oracle y a la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino.  
  
 En la tabla siguiente se describen los componentes que se ejecutan en las cuatro plataformas descritas anteriormente.  
  
|Componente/Descripción|El componente consta de:|  
|----------------------------|----------------------------|  
|Servicio CDC de Oracle: es un servicio de Windows donde se realiza la actividad de captura de datos modificados.|Instancia CDC de Oracle: un subproceso del servicio CDC de Oracle que controla la actividad de captura de datos modificados para una única base de datos de origen de Oracle (hay una instancia CDC de Oracle por base de datos de origen de Oracle).|  
||Registro del LOG de Oracle: lee registros de transacciones de Oracle usando el cliente de Oracle.|  
||Cliente de Oracle: el cliente Oracle Instant usado para la comunicación con Oracle. Se trata de un requisito previo que se debe obtener de Oracle e instalar antes de instalar el servicio CDC de Oracle.|  
||[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Change Writer: escribe en las tablas de cambios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]los cambios confirmados realizados en la tabla capturada de Oracle. Este componente también mantiene ese estado de captura dentro de la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino.|  
||[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: Microsoft Native Client para [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Se trata de un componente que es un requisito previo y que se debe obtener de Microsoft e instalar antes de instalar el servicio CDC de Oracle.|  
|Configuración del servicio CDC de Oracle: complemento de Microsoft Management Console que crea el servicio de Windows y establece su configuración.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : cliente SQL ADO.NET que se incluye con la versión 4 de .NET Framework.|  
|Base de datos de Oracle: base de datos de origen de Oracle de la que se capturan los cambios a las tablas seleccionadas.|Extractor de registros: componente de Oracle a través del cual se leen los registros de transacciones de Oracle.|  
||Registros de transacciones: registros de rehacer de Oracle en línea y almacenados usados por Oracle para asegurarse de que la base de datos puede revertir revertir las transacciones y recuperarse de errores (en este caso, la base de datos de Oracle debe ejecutarse en modo archive-log).|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde se hospedan las bases de datos CDC. Puede ser una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en clúster (clúster de conmutación por error) o una base de datos reflejada (AlwaysOn).|La base de datos MSXDBCDC: base de datos donde se mantiene información sobre los servicios CDC que trabajan con esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . También mantiene información sobre las instancias CDC de Oracle controladas por cada servicio CDC. Esta base de datos se crea como parte del proceso de creación del servicio CDC.|  
||Las bases de datos CDC: bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que almacenan los cambios realizados en una de las bases de datos de origen de Oracle. Las bases de datos CDC están habilitadas para CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , por lo que tienen las tablas y funciones CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , lo que simplifica el uso de cambios procedentes de Oracle.|  
|Diseñador CDC de Oracle: complemento de Microsoft Management Console que ayuda a crear instancias CDC de Oracle. Use este complemento para seleccionar las tablas y columnas que se van a capturar, proporcionar información de conexión de Oracle y administrar el ciclo de vida de las instancias CDC.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] : cliente SQL ADO.NET que se incluye con la versión 4 de .NET Framework.|  
||Cliente de Oracle: el cliente Oracle Instant usado para la comunicación con Oracle. Se trata de un componente que es un requisito previo y que se debe obtener de Oracle e instalar antes de instalar el servicio CDC de Oracle.|  
  
 El servicio CDC de Oracle y sus instancias CDC de Oracle secundarias solo pueden comunicarse con las bases de datos de origen de Oracle y con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de destino como clientes. No realizan escuchas activas en una red y otros protocolos. El servicio CDC de Oracle supervisa las bases de datos CDC para ver si hay cambios de configuración y actualiza su funcionamiento según la configuración actualizada.  
  
  
