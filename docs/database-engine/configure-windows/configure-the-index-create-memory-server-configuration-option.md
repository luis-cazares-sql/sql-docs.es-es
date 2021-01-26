---
title: Establecer la opción de configuración del servidor Memoria para creación de índices | Microsoft Docs
description: Vea cómo usar la opción de memoria para crear índices para establecer la cantidad máxima de memoria que SQL Server asigna inicialmente para las operaciones de ordenación durante la creación de índices.
ms.custom: ''
ms.date: 11/24/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- index create memory option
ms.assetid: 3d722d9b-bada-4bf5-a9d7-bfc556bb4915
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 66fa81d56ed0b3ebebd7a95ddea0b12356da5c1d
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783691"
---
# <a name="configure-the-index-create-memory-server-configuration-option"></a>Establecer la opción de configuración del servidor Memoria para creación de índices
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **memoria para creación de índices** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **memoria para creación de índices** controla la cantidad máxima de memoria asignada inicialmente para las operaciones de ordenación durante la creación de índices. El valor predeterminado para esta opción es 0 (configuración automática). Si más adelante se necesita más memoria para la creación de índices y hay memoria disponible, el servidor la utilizará; por lo tanto, se excederá el valor de esta opción. Si no hay más memoria disponible, la creación de índices continuará utilizando la asignada.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de memoria para creación de índices, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de memoria para creación de índices](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El valor de la opción de **[memoria mínima por consulta](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)** tiene prioridad sobre la opción **memoria para creación de índices**. Si cambia ambas opciones y el valor de **memoria para creación de índices** es inferior al de **memoria mínima por consulta**, aparecerá un mensaje de advertencia, pero se establecerá el valor. Durante la ejecución de consultas, recibirá una advertencia similar.  
  
-   Al usar tablas e índices con particiones, los requisitos de memoria mínima para la creación de índices pueden aumentar de forma significativa si hay índices con particiones no alineados con un alto grado de paralelismo. Esta opción controla la cantidad inicial total de memoria asignada para todas las particiones de índice en una sola operación de creación de índices. La consulta se terminará con un mensaje de error si la cantidad establecida por esta opción es inferior al mínimo necesario para ejecutar la consulta.  
  
-   El valor de ejecución de esta opción no excederá la cantidad real de memoria que se puede usar para el sistema operativo y la plataforma de hardware en los que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   La opción **Memoria para creación de índices** se configura automáticamente y, por lo general, funciona sin necesidad de ajuste alguno. No obstante, si tiene dificultades para crear índices, puede probar a aumentar el valor de esta opción a partir del valor de ejecución.  

-   Crear un índice en un sistema de producción suele ser una tarea poco frecuente y que, a menudo, se programa como un trabajo que se ejecutará en horas de poca actividad. Por tanto, si se crean índices con poca frecuencia y durante las horas de poca actividad, el incremento de la **memoria para creación de índices** puede mejorar el rendimiento de la creación de índices. Sin embargo, mantenga la opción de configuración **[memoria mínima por consulta](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)** con un número bajo para que el trabajo de creación de índices se inicie aunque no esté disponible toda la memoria solicitada.
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-index-create-memory-option"></a>Para configurar la opción index create memory  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Memoria** .  
  
3.  En **Memoria de creación de índice**, escriba o seleccione el valor que desee para la opción index create memory.  
  
     Utilice la opción **index create memory** para controlar la cantidad de memoria que se utiliza para ordenaciones de creación de índices. La opción **Memoria para creación de índices** se configura automáticamente y, en la mayoría de los casos, debería funcionar sin necesidad de ajuste alguno. No obstante, si tiene dificultades para crear índices, puede probar a aumentar el valor de esta opción a partir del valor de ejecución. Las ordenaciones de consultas se controlan mediante la opción de **memoria mínima por consulta** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-index-create-memory-option"></a>Para configurar la opción index create memory  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `index create memory` en `4096`.  
  
```sql  
USE AdventureWorks2012 ;  
EXEC sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
EXEC sp_configure 'index create memory', 4096  
GO  
RECONFIGURE;  
GO  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-index-create-memory-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de memoria para creación de índices  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.configurations&#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de memoria del servidor](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
