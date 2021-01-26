---
title: Establecer la opción de configuración del servidor Umbral de cursor | Microsoft Docs
description: Obtenga información sobre la opción Umbral de cursor. Vea cómo su valor afecta a si SQL Server genera conjuntos de claves de cursor de forma asincrónica y cómo configurarlo.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- cursor threshold option
ms.assetid: 189f2067-c6c4-48bd-9bd9-65f6b2021c12
author: markingmyname
ms.author: maghan
ms.openlocfilehash: de30cb4bcf672fdf47ab926a28680baa1d9b1067
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783294"
---
# <a name="configure-the-cursor-threshold-server-configuration-option"></a>Establecer la opción de configuración del servidor Umbral de cursor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **umbral de cursor** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción de **umbral de cursor** especifica el número de filas del conjunto de cursores donde se generan de manera asincrónica los conjuntos de claves del cursor. Cuando los cursores generan un conjunto de claves para un conjunto de resultados, el optimizador de consultas calcula el número de filas que se va a devolver para ese conjunto de resultados. Si el optimizador de consultas calcula que el número de filas devuelto es superior a este umbral, el cursor se genera de manera asincrónica, lo que permite al usuario capturar las filas del cursor mientras sigue llenándose. De lo contrario, el cursor se genera de manera sincrónica y la consulta espera a que se devuelvan todas las filas.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de umbral de cursor, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de umbral de cursor](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no es compatible con la generación asincrónica de cursores de [!INCLUDE[tsql](../../includes/tsql-md.md)] estáticos o controlados por conjuntos de claves. [!INCLUDE[tsql](../../includes/tsql-md.md)] como OPEN o FETCH se procesan por lotes, por lo que la generación asincrónica de cursores de [!INCLUDE[tsql](../../includes/tsql-md.md)] no es necesaria. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sigue admitiendo los cursores de servidor de la interfaz de programación de aplicaciones (API) estáticos o controlados por conjuntos de claves asincrónicos en los que OPEN de baja latencia es un problema, debido a los recorridos de ida y vuelta al cliente para cada operación del cursor.  
  
-   La precisión del optimizador de consultas para determinar una estimación del número de filas de un conjunto de claves depende del grado de actualización de las estadísticas de cada una de las tablas del cursor.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Si establece el valor -1 para el **umbral de cursor** , todos los conjuntos de claves se generan de manera sincrónica, lo que beneficia a los conjuntos de cursores pequeños. Si establece el valor 0 para **cursor threshold** , todos los conjuntos de claves del cursor se generan de manera asincrónica. Con otros valores, el optimizador de consultas compara el número de filas esperadas del conjunto de cursores y genera asincrónicamente el conjunto de claves si supera el número establecido en **cursor threshold**. No establezca un valor demasiado bajo para el **umbral de cursor** , ya que los conjuntos de resultados pequeños se generan mejor de manera sincrónica.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-cursor-threshold-option"></a>Para configurar la opción cursor threshold  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En **Varios**, cambie la opción **Umbral de cursor** al valor que desee.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-cursor-threshold-option"></a>Para configurar la opción cursor threshold  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer la opción de `cursor threshold` en `0` con el fin de generar conjuntos de claves del cursor de forma asincrónica.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'cursor threshold', 0 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-cursor-threshold-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de umbral de cursor  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
  
  
