---
description: Administrador de conexiones de limpieza de DQS
title: Administrador de conexiones de limpieza de DQS | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: faa1eedd-db14-41e5-8e58-8f0f6f561e42
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 33586839b3efb0c8c93786a2e020228a44dc6bbc
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726702"
---
# <a name="dqs-cleansing-connection-manager"></a>Administrador de conexiones de limpieza de DQS

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los administradores de conexiones de limpieza de DQS permiten que los paquetes se conecten a un servidor de [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] . La transformación Limpieza de DQS usa el administrador de conexiones de Limpieza de DQS.  
  
 Para obtener más información acerca de Data Quality Services, vea [Data Quality Services Concepts](../../data-quality-services/data-quality-services-concepts.md).  
  
> [!IMPORTANT]  
>  El administrador de conexiones de Limpieza de DQS solo admite la autenticación de Windows.  
  
## <a name="related-tasks"></a>Related Tasks  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación. Para más información sobre las propiedades que puede establecer en el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea [Cuadro de diálogo Editor de transformación Limpieza de DQS](../data-flow/transformations/dqs-cleansing-transformation.md).  
  
 Para obtener información sobre cómo configurar un administrador de conexiones mediante programación, vea la documentación de la clase <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> en la Guía del desarrollador.  
  
