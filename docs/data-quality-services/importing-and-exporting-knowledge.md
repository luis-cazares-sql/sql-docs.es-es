---
description: Importar y exportar conocimiento
title: Importar y exportar conocimiento
ms.date: 07/31/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 12537c9d-31e4-40b0-a411-cb343abbe96a
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 639e248fd938f3d2429b3d33729414224db1dc8c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88462142"
---
# <a name="importing-and-exporting-knowledge"></a>Importar y exportar conocimiento

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Puede crear bases de conocimiento y dominios directamente en la aplicación [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , o bien importar conocimiento en una base de conocimiento o exportarlo desde esta. En la aplicación [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , puede usar un archivo de datos para las operaciones de importación y exportación, o un archivo de Excel para las operaciones de importación. El archivo de datos utilizado es un archivo cifrado creado por [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) con la extensión .dqs. Los archivos creados por Microsoft Excel pueden tener la extensión .xlsx, .xls o .csv. Estas operaciones le proporcionan una mayor flexibilidad a la hora de generar y compartir el conocimiento que utiliza para realizar las operaciones de limpieza de datos y búsqueda de coincidencias.  
  
> [!IMPORTANT]  
>  Puede exportar *todas* las bases de conocimiento del [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] a un archivo de copia de seguridad de DQS (.dqsb) al mismo tiempo ejecutando el archivo DQSInstaller.exe desde el símbolo del sistema. Del mismo modo, puede importar simultáneamente *todas* las bases de conocimiento desde un archivo de copia de seguridad de DQS (.dqsb) a [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] ejecutando el archivo DQSInstaller.exe desde el símbolo del sistema. Para obtener más información acerca de cómo hacerlo, vea [Exportar e importar bases de conocimiento de DQS mediante DQSInstaller.exe](../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md) en la Guía de instalación de DQS.  
  
## <a name="in-this-section"></a>En esta sección  
 Puede realizar las operaciones de importación y exportación siguientes:  
  
|Descripción de la operación|Tema|  
|-|-|  
|Exportar un dominio de una base de conocimiento a un archivo de datos .dqs|[Exportar un dominio a un archivo .dqs](../data-quality-services/export-a-domain-to-a-dqs-file.md)|  
|Importar un dominio desde un archivo de datos .dqs a una base de conocimiento existente|[Importar un dominio desde un archivo .dqs](../data-quality-services/import-a-domain-from-a-dqs-file.md)|  
|Exportar una base de conocimiento completa a un archivo de datos .dqs|[Exportar una base de conocimiento a un archivo .dqs](../data-quality-services/export-a-knowledge-base-to-a-dqs-file.md)|  
|Importar una base de conocimiento completa desde un archivo de datos .dqs|[Importar una base de conocimiento desde un archivo .dqs](../data-quality-services/import-a-knowledge-base-from-a-dqs-file.md)|  
|Importar valores desde un archivo de Excel a un dominio|[Importar valores desde un archivo de Excel a un dominio](../data-quality-services/import-values-from-an-excel-file-into-a-domain.md)|  
|Importar dominios desde un archivo de Excel a una base de conocimiento|[Importar dominios desde un archivo de Excel a la detección del conocimiento](../data-quality-services/import-domains-from-an-excel-file-in-knowledge-discovery.md)|  
|Importar el conocimiento recopilado durante la limpieza en una base de conocimiento|[Importar valores de un proyecto de limpieza en un dominio](../data-quality-services/import-cleansing-project-values-into-a-domain.md)|  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción de la tarea|Tema|  
|----------------------|-----------|  
|Generar una base de conocimiento ejecutando la detección de conocimiento y administrando el conocimiento de forma interactiva|[Crear una base de conocimiento](../data-quality-services/building-a-knowledge-base.md)|  
|Crear un dominio individual y agregarle conocimiento.|[Administrar un dominio](../data-quality-services/managing-a-domain.md)|  
|Crear un dominio compuesto y agregarle conocimiento.|[Administrar un dominio compuesto](../data-quality-services/managing-a-composite-domain.md)|  
  
  
