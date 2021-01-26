---
title: Establecer la opción de configuración del servidor Tiempo de espera de consulta remota | Microsoft Docs
description: Obtenga más información sobre la opción “tiempo de espera de consulta remota”. Vea cómo determina el número de segundos que puede esperar una operación remota antes de que se agote el tiempo de espera de SQL Server.
ms.custom: ''
ms.date: 03/08/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- time limit for remote queries [SQL Server]
- remote query timeout option
ms.assetid: 888c8448-933b-41e3-8aa1-c206bc0cdb78
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4ddf0184fd7a1bd95290fa2a041d2fede09f90bf
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783212"
---
# <a name="configure-the-remote-query-timeout-server-configuration-option"></a>Establecer la opción de configuración del servidor Tiempo de espera de consulta remota
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **tiempo de espera de consultas remotas** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **tiempo de espera de consultas remotas** especifica cuánto tiempo, en segundos, puede tardar una operación remota antes de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supere el tiempo de espera. El valor predeterminado de esta opción es 600, lo que permite una espera de 10 minutos. Este valor se aplica a una conexión saliente iniciada por el [!INCLUDE[ssDE](../../includes/ssde-md.md)] como una consulta remota. Este valor no afecta a las consultas recibidas por el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Para deshabilitar el tiempo de espera, establezca el valor en 0. Una consulta esperará hasta que finalice.  
  
 En el caso de las consultas heterogéneas, la opción de **tiempo de espera de consultas remotas** especifica los segundos (inicializados en el objeto de comando mediante la propiedad de conjunto de filas DBPROP_COMMANDTIMEOUT) que un proveedor remoto debe esperar a los conjuntos de resultados antes de que se agote el tiempo de espera de la consulta. Este valor se utiliza también para establecer DBPROP_GENERALTIMEOUT, si el proveedor remoto lo admite. Esto hará que se agote el tiempo de espera de todas las demás operaciones después del número de segundos especificado.  
  
 Para los procedimientos almacenados remotos, la opción de **tiempo de espera de consultas remotas** especifica los segundos que deben transcurrir después de enviar una instrucción `EXEC` remota antes de que se agote el tiempo de espera del procedimiento almacenado remoto.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de tiempo de espera de consultas remotas, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de tiempo de espera de consultas remotas](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Deben permitirse conexiones de servidor remoto para poder establecer este valor.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>Para configurar la opción de tiempo de espera de consultas remotas  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Conexiones** .  
  
3.  En **Conexiones a servidores remotos**, en el cuadro **Tiempo de espera de consultas remotas** , escriba o seleccione un valor entre 0 y 2.147.483.647 para establecer el tiempo máximo de espera en segundos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>Para configurar la opción de tiempo de espera de consultas remotas  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `remote query timeout` en `0` , que deshabilita el tiempo de espera.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'remote query timeout', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-remote-query-timeout-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de tiempo de espera de consultas remotas  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Propiedades y comportamientos de conjuntos de filas](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
