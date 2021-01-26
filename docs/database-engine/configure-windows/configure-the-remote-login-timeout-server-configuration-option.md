---
title: Establecer la opción de configuración del servidor Tiempo de espera de inicio de sesión remoto | Microsoft Docs
description: Obtenga más información sobre la opción "Tiempo de espera de inicio de sesión remoto". Vea cómo limita el número de segundos que SQL Server asigna a la conexión a un servidor remoto.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote login timeout option
ms.assetid: 077adebe-0e3f-42a5-a75e-5e6d04847e2b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ffc46323f8f3fb783b4a06836f66c535e145a04e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783128"
---
# <a name="configure-the-remote-login-timeout-server-configuration-option"></a>Establecer la opción de configuración del servidor Tiempo de espera de inicio de sesión remoto
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo configurar la opción de configuración de **tiempo de espera de inicio de sesión remoto** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **tiempo de espera de inicio de sesión remoto** especifica el número de segundos que se esperará para volver de un error al intentar el inicio de sesión en un servidor remoto. Por ejemplo, si intenta iniciar sesión en un servidor remoto y el servidor no está activo, la opción de **tiempo de espera de inicio de sesión remoto** le ayuda a asegurarse de no tener que esperar indefinidamente hasta que el equipo deje de intentar iniciar la sesión. El valor predeterminado para esta opción es de 10 segundos. Un valor 0 permite una espera infinita.  
  
> [!NOTE]  
>  El valor predeterminado para esta opción es de 20 segundos en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de tiempo de espera de inicio de sesión remoto, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de tiempo de espera de inicio de sesión remoto](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La opción de **tiempo de espera de inicio de sesión remoto** afecta a las conexiones a proveedores OLE DB establecidas para realizar consultas heterogéneas.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-login-timeout-option"></a>Para configurar la opción de tiempo de espera de inicio de sesión remoto  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En **Red**, seleccione un valor para el cuadro **Remote Login Timeout** .  
  
     Use la opción de **tiempo de espera de inicio de sesión remoto** para especificar el número de segundos que se esperará para volver cuando se produce un error al intentar un inicio de sesión remoto.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-remote-login-timeout-option"></a>Para configurar la opción de tiempo de espera de inicio de sesión remoto  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción `remote login timeout` en `35` segundos.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'remote login timeout', 35 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-remote-login-timeout-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de tiempo de espera de inicio de sesión remoto  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
