---
title: Configurar la opción de configuración del servidor Acceso remoto | Microsoft Docs
description: Obtenga información sobre alternativas a la opción "remote access" en desuso. Vea otros orígenes para solucionar problemas relacionados con las conexiones de SQL Server.
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- remote servers [SQL Server], stored procedure execution
ms.assetid: f5de748d-1c55-4714-9661-38fe62e5095f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: afe6721bef097dfa941b5578b6d4287fc370c3f2
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783143"
---
# <a name="configure-the-remote-access-server-configuration-option"></a>Configurar la opción de configuración del servidor Acceso remoto
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se abarca la característica "Acceso remoto". Esta opción de configuración es una característica de comunicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] poco clara que está en desuso y que probablemente no debería usar. Si llegó a esta página porque tiene problemas para conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consulte uno de los siguientes temas:  
  
-   [Tutorial: Introducción al motor de base de datos](../../relational-databases/tutorial-getting-started-with-the-database-engine.md)  
  
-   [Iniciar una sesión en SQL Server](../../database-engine/configure-windows/logging-in-to-sql-server.md)  
  
-   [Conectarse a SQL Server cuando los administradores del sistema no tienen acceso](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)  
  
-   [Conectarse a un servidor registrado &#40;SQL Server Management Studio&#41;](../../ssms/register-servers/connect-to-a-registered-server-sql-server-management-studio.md)  
  
-   [Conectar a un componente de SQL Server desde SQL Server Management Studio](../../ssms/f1-help/connect-to-any-sql-server-component-from-sql-server-management-studio.md)  
  
-   [Conectarse al motor de base de datos con sqlcmd](../../ssms/scripting/sqlcmd-connect-to-the-database-engine.md)  
  
-   [Cómo solucionar problemas de conexión al motor de base de datos de SQL Server](https://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx)  
  
 Puede que a los programadores les interesen los temas siguientes:  
  
-   [Cómo: Conectar con SQL Server mediante Autenticación de SQL en ASP.NET 2.0](/previous-versions/msp-n-p/ff648340(v=pandp.10))  
  
-   [Conectarse a una instancia de SQL Server](../../relational-databases/server-management-objects-smo/create-program/connecting-to-an-instance-of-sql-server.md)  
  
-   [Cómo: Crear conexiones a Bases de datos de SQL Server](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))  
  
 **Aquí comienza el cuerpo principal de este tema.**  
  
 En este tema se describe cómo establecer la opción de configuración del servidor **acceso remoto** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción **acceso remoto** controla la ejecución de los procedimientos almacenados desde servidores remotos o locales en los que se están ejecutando instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El valor predeterminado para esta opción es 1. Este valor concede el permiso para ejecutar los procedimientos almacenados locales desde servidores remotos o los procedimientos almacenados remotos desde el servidor local. Para evitar que los procedimientos almacenados locales se ejecuten desde un servidor remoto o que los procedimientos almacenados remotos se ejecuten desde un servidor local, establezca la opción en 0.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Use en su lugar [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de acceso remoto, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de acceso remoto](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La opción de **acceso remoto** solo se aplica a los servidores agregados mediante [sp_addserver](../../relational-databases/system-stored-procedures/sp-addserver-transact-sql.md)y se incluye por compatibilidad con versiones anteriores.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-access-option"></a>Para configurar la opción de acceso remoto  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Conexiones** .  
  
3.  En **Conexiones a servidores remotos**, active o desactive la casilla **Permitir conexiones remotas con este servidor** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-remote-access-option"></a>Para configurar la opción de acceso remoto  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `remote access` en `0`.  
  
```sql  
EXEC sp_configure 'remote access', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-remote-access-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de acceso remoto  
 Esta configuración no surte efecto hasta que reinicie SQL Server.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
