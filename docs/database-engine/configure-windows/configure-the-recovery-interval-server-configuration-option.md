---
title: Establecer la opción de configuración del servidor Intervalo de recuperación | Microsoft Docs
description: Obtenga información sobre la opción "recovery interval". Vea cómo su valor afecta a la frecuencia con la que SQL Server emite puntos de control automáticos en una base de datos.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- restoring recovery interval [SQL Server]
- checkpoints [SQL Server]
- recovery interval option [SQL Server]
- default recovery interval option
- time [SQL Server], recovery interval
- interval for recovery [SQL Server]
- maximum number of minutes per database recovery
- recovery [SQL Server], recovery interval option
ms.assetid: e4734b3b-8fbe-4b65-9c48-91b5a3dd18e1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ac6d7f368a42e8a6c81fadbf151760b5bdfc8b53
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783173"
---
# <a name="configure-the-recovery-interval-server-configuration-option"></a>Establecer la opción de configuración del servidor Intervalo de recuperación
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **intervalo de recuperación** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **intervalo de recuperación** define un límite superior para el tiempo que debe tardar la recuperación de cada base de datos. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] usa el valor especificado en esta opción para determinar aproximadamente la frecuencia con la que deben emitirse los [puntos de comprobación automáticos](../../relational-databases/logs/database-checkpoints-sql-server.md) en una base de datos determinada.  
  
 El valor de intervalo de recuperación predeterminado es 0, lo que permite que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] configure automáticamente el intervalo de recuperación. Normalmente, el intervalo de recuperación predeterminado tiene como resultado que los puntos de comprobación automáticos se produzcan aproximadamente cada minuto en las bases de datos activas con un tiempo de recuperación inferior a un minuto. Los valores más altos indican el tiempo de recuperación máximo aproximado, en minutos. Por ejemplo, si el intervalo de recuperación se establece en 3, indica un tiempo de recuperación máximo de aproximadamente tres minutos.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para establecer la opción de configuración del servidor de intervalo de recuperación, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [después de configurar la opción de intervalo de recuperación](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El intervalo de recuperación afecta solo a las bases de datos que usan el tiempo de recuperación predeterminado de destino (0). Para invalidar el intervalo de recuperación de servidor en una base de datos, configure un tiempo de recuperación de destino no predeterminado en la base de datos. Para obtener más información, vea [Cambiar el tiempo de recuperación de destino de una base de datos &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Normalmente, se recomienda mantener el intervalo de recuperación en 0, a menos que experimente problemas de rendimiento. Si decide aumentar el valor de intervalo de recuperación, es recomendable que lo haga gradualmente en pequeños incrementos y que evalúe el efecto de cada aumento incremental en el rendimiento de la recuperación.  
  
-   Si usa **sp_configure** para cambiar el valor de la opción de **intervalo de recuperación** a más de 60 (minutos), especifique RECONFIGURE WITH OVERRIDE. WITH OVERRIDE deshabilita la comprobación de los valores de configuración (para los valores no válidos o que no se recomiendan).  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para establecer el intervalo de recuperación**  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en una instancia del servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Configuración de base de datos** .  
  
3.  En **Recuperación**, en el cuadro **Intervalo de recuperación (min)** , escriba o seleccione un valor entre 0 y 32767 para establecer el tiempo máximo, en minutos, que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe emplear en recuperar cada base de datos cuando se inicia. El valor predeterminado es 0, que indica que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]lo configura automáticamente. En la práctica, esto significa un tiempo de recuperación inferior a un minuto y un punto de comprobación aproximadamente cada minuto para bases de datos activas.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-set-the-recovery-interval"></a>Para establecer el intervalo de recuperación  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `recovery interval` en `3` minutos.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE ;  
GO  
EXEC sp_configure 'recovery interval', 3 ;  
GO  
RECONFIGURE;  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-recovery-internal-option"></a><a name="FollowUp"></a> Seguimiento: después de configurar la opción de intervalo de recuperación  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar el tiempo de recuperación de destino de una base de datos &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)   
 [Puntos de comprobación de base de datos &#40;SQL Server&#41;](../../relational-databases/logs/database-checkpoints-sql-server.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Opción de configuración del servidor Mostrar opciones avanzadas](../../database-engine/configure-windows/show-advanced-options-server-configuration-option.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)  
  
  
