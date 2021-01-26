---
title: Establecer la opción de configuración del servidor Buscar procedimientos de inicio | Microsoft Docs
description: Obtenga información sobre la opción "scan for startup procs". Vea cómo especifica si SQL Server examina y ejecuta todos los procedimientos almacenados que se ejecutan automáticamente en el inicio.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- scan for startup procs option
ms.assetid: 6bf9d252-e766-458d-9dcd-23d895f032a2
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 00426f84d2f8b3c0ca4626ffd8bddc234f27cf4d
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783208"
---
# <a name="configure-the-scan-for-startup-procs-server-configuration-option"></a>Establecer la opción de configuración del servidor Buscar procedimientos de inicio
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **buscar procedimientos de inicio** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Use la opción de **buscar procedimientos de inicio** para buscar la ejecución automática de procedimientos almacenados durante el inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si el valor de esta opción se establece en 1, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] buscará y ejecutará todos los procedimientos almacenados de ejecución automática definidos en el servidor. El valor predeterminado de **Buscar procedimientos de inicio** es 0 (no buscar).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de buscar procedimientos de inicio, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de buscar procedimientos de inicio](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   El valor de esta opción puede especificarse mediante **sp_configure**. Pero se establecerá automáticamente si usa **sp_procoption**, que marca o desmarca los procedimientos almacenados de ejecución automática. Cuando se usa **sp_procoption** para marcar el primer procedimiento almacenado como procedimiento automático, esta opción se establece automáticamente en el valor 1. Cuando se usa **sp_procoption** para desmarcar el último procedimiento almacenado como procedimiento automático, esta opción se establece automáticamente en el valor 0. Si usa **sp_procoption** para marcar y desmarcar los procedimientos automáticos, y si siempre los desmarca antes de quitarlos, no hay necesidad de establecer esta opción manualmente.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>Para configurar la opción scan for startup procs  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En **Varios**, cambie la opción **Buscar procedimientos de inicio** a True o False seleccionando el valor que quiera en el cuadro de lista desplegable.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-scan-for-startup-procs-option"></a>Para configurar la opción scan for startup procs  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `scan for startup procs` en `1`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'scan for startup procs', 1 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-scan-for-startup-procs-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de buscar procedimientos de inicio  
 El servidor debe reiniciarse para que el valor surta efecto.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [sp_procoption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-procoption-transact-sql.md)  
  
  
