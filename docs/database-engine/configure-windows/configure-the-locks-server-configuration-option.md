---
title: Establecer la opción de configuración del servidor Bloqueos | Microsoft Docs
description: Obtenga información sobre la opción Bloqueos. Vea cómo usarla para limitar la cantidad de memoria que usa el Motor de base de datos de SQL Server para los bloqueos.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- locks option [SQL Server]
ms.assetid: b0cf0f86-7652-4574-a9fb-908e10d03973
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a30c2cb2955ef82380bde00714098afec153ed95
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783699"
---
# <a name="configure-the-locks-server-configuration-option"></a>Establecer la opción de configuración del servidor Bloqueos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **bloqueos** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **bloqueos** establece el número máximo de bloqueos disponibles, limitando de este modo la cantidad de memoria que el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] usa para ellos. El valor predeterminado es 0, lo que permite al [!INCLUDE[ssDE](../../includes/ssde-md.md)] asignar y cancelar la asignación de estructuras de bloqueo de manera dinámica a partir de los requisitos variables del sistema.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de bloqueos, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de bloqueos](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Cuando se inicia el servidor con un valor de 0 para **bloqueos** , el administrador de bloqueos obtendrá suficiente memoria del [!INCLUDE[ssDE](../../includes/ssde-md.md)] para un grupo inicial de 2.500 estructuras de bloqueo. A medida que se agota el grupo de bloqueos, se adquiere más memoria para el grupo.  
  
     Generalmente, si se necesita más memoria para el grupo de bloqueos que la disponible en el bloque de memoria del [!INCLUDE[ssDE](../../includes/ssde-md.md)] , y hay más memoria en el equipo (no se ha alcanzado el umbral especificado en la opción **Memoria de servidor máxima** ), el [!INCLUDE[ssDE](../../includes/ssde-md.md)] asignará memoria de manera dinámica para satisfacer la solicitud de bloqueos. No obstante, si la asignación de esa memoria puede causar la paginación en el sistema operativo (por ejemplo, si se está ejecutando otra aplicación en el mismo equipo que una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y está utilizando esa memoria), no se asignará más espacio para bloqueos. El grupo de bloqueos dinámicos no obtendrá más del 60% de la memoria asignada para el [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cuando el grupo de bloqueos alcanza el 60% de la memoria obtenida por una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)], o cuando ya no queda más memoria disponible en el equipo, se generará un error si se producen más solicitudes de bloqueos.  
  
     La configuración recomendada es permitir que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilice los bloqueos de manera dinámica. No obstante, puede establecer la opción **locks** e invalidar la capacidad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para asignar recursos de bloqueo de manera dinámica. Cuando se selecciona un valor para **locks** distinto de 0, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] no puede asignar más bloqueos que el número especificado en el valor **locks**. Aumente este valor si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] muestra un mensaje en el que se indica que se ha superado el número de bloqueos disponibles. Como cada bloqueo consume memoria (96 bytes por bloqueo), el aumento de este valor puede requerir el aumento de la cantidad de memoria dedicada al servidor.  
  
-   La opción **locks** también afecta al momento en el que se produce la ampliación de bloqueo. Cuando se selecciona el valor 0 para **locks** , la ampliación de bloqueo se produce cuando la memoria utilizada por las estructuras de bloqueo actuales alcanza el 40% del bloque de memoria del [!INCLUDE[ssDE](../../includes/ssde-md.md)] . Cuando se selecciona un valor distinto de 0 para **bloqueos** , la ampliación de bloqueo se produce cuando el número de bloqueos alcanza el 40% del valor especificado para **bloqueos**.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-locks-option"></a>Para configurar la opción locks  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En **Paralelismo**, escriba el valor deseado para la opción **locks** .  
  
     Use la opción **locks** para establecer el número máximo de bloqueos disponibles y limitar así la cantidad de memoria que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza para los mismos.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-locks-option"></a>Para configurar la opción locks  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `locks` para establecer el número de bloqueos disponibles para todos los usuarios en `20000`.  
  
```sql  
Use AdventureWorks2012 ;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'locks', 20000;  
GO  
RECONFIGURE;  
GO  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-locks-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de bloqueos  
 El servidor debe reiniciarse para que el valor surta efecto.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
