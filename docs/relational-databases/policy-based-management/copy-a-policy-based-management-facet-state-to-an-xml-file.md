---
title: Copia de un estado de faceta de administración basada en directivas en un archivo XML
description: Describe cómo copiar el estado de una faceta de administración basada en directivas en un archivo XML mediante SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Policy-Based Management, copy facet state to XML file
ms.assetid: 7d604ab1-6dd6-4f8e-a79c-eba99ab106fd
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 05235a6f2bc0ed2450e71397770599a5e22ea3d1
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85654732"
---
# <a name="copy-a-policy-based-management-facet-state-to-an-xml-file"></a>Copiar en un archivo XML un estado de faceta de administración basada en directivas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo copiar el estado de una faceta de administración basada en directivas en un archivo XML en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para copiar un estado de faceta en un archivo XML mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los procedimientos descritos en este tema requieren la pertenencia al rol PolicyAdministratorRole de la base de datos msdb.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-copy-a-facet-state-to-an-xml-file"></a>Para copiar un estado de faceta en un archivo XML  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], objeto de instancia, base de datos u objeto de base de datos y, después, haga clic en **Facetas**.  
  
2.  En el cuadro de diálogo **Ver facetas -** _nombre_de_objeto_, haga clic en **Exportar estado actual como directiva**.  
  
3.  En el cuadro de diálogo **Exportar como directiva** , escriba la ruta de acceso y el nombre del archivo, o use el botón Examinar ( **...** ) para buscar el archivo y, después, escriba el nombre del archivo XML. Para obtener más información acerca de las opciones disponibles en este cuadro de diálogo, vea [Export As Policy Dialog Box](../../relational-databases/policy-based-management/export-as-policy-dialog-box.md).  
  
4.  Cuando termine, haga clic en **Aceptar**.  
  
  
