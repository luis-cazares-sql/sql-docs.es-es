---
title: Establecer la opción de configuración del servidor Opciones de usuario | Microsoft Docs
description: Obtenga información sobre la opción "user options". Vea cómo cambia los valores predeterminados de las opciones de procesamiento de consultas que SQL Server establece para las sesiones de trabajo del usuario.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- global default for all users [SQL Server]
- users [SQL Server], global defaults
- user options option [SQL Server]
ms.assetid: cfed8f86-6bcf-4b90-88eb-9656e22d5dc5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 71dcf7e52b1c58c7df868f502bc8de5da9892c6b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783447"
---
# <a name="configure-the-user-options-server-configuration-option"></a>Establecer la opción de configuración del servidor Opciones de usuario
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración de servidor para **opciones de usuario** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción para **opciones de usuario** especifica valores predeterminados globales para todos los usuarios. Hay establecida una lista de opciones de procesamiento de consultas predeterminadas para la duración de la sesión de trabajo de un usuario. La opción **opciones de usuario** permite cambiar los valores predeterminados de las opciones SET si no resultan adecuados los valores predeterminados del servidor.  
  
 El usuario puede suplantar estos valores predeterminados con la instrucción SET. Puede configurar la opción **user options** de manera dinámica para nuevos inicios de sesión. Después de cambiar el valor de la opción para **opciones de usuario**, los nuevos inicios de sesión utilizarán el nuevo valor, pero el cambio no afectará a los inicios de sesión actuales.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de configuración para opciones de usuario, utilizando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de configuración para opciones de usuario](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   En la tabla siguiente se muestran y describen los valores de configuración para **opciones de usuario**. No todos los valores de configuración son compatibles entre sí. Por ejemplo, ANSI_NULL_DFLT_ON y ANSI_NULL_DFLT_OFF no se pueden establecer al mismo tiempo.  
  
    |Value|Configuración|Descripción|  
    |-----------|-------------------|-----------------|  
    |1|DISABLE_DEF_CNST_CHK|Controla la comprobación de restricciones provisionales o diferidas.|  
    |2|IMPLICIT_TRANSACTIONS|Para las conexiones de biblioteca de red dblib, controla si una transacción se inicia de manera implícita al ejecutar una instrucción. El valor IMPLICIT_TRANSACTIONS no tiene ningún efecto en las conexiones ODBC u OLEDB.|  
    |4|CURSOR_CLOSE_ON_COMMIT|Controla el comportamiento de los cursores después de realizarse una operación de confirmación.|  
    |8|ANSI_WARNINGS|Controla el truncamiento y los valores NULL en las advertencias de agregados.|  
    |16|ANSI_PADDING|Controla los valores de relleno de las variables de longitud fija.|  
    |32|ANSI_NULLS|Controla el tratamiento de los valores NULL cuando se utilizan operadores de igualdad.|  
    |64|ARITHABORT|Cancela una consulta cuando se produce un error de desbordamiento o división por cero durante su ejecución.|  
    |128|ARITHIGNORE|Devuelve un valor NULL cuando se produce un error de desbordamiento o de división por cero durante una consulta.|  
    |256|QUOTED_IDENTIFIER|Diferencia entre las comillas simples o dobles al evaluar una expresión.|  
    |512|NOCOUNT|Desactiva el mensaje que se devuelve al final de cada instrucción, que indica el número de filas afectadas.|  
    |1024|ANSI_NULL_DFLT_ON|Altera el comportamiento de la sesión para que utilice la compatibilidad con ANSI para la nulabilidad. Se permite la nulabilidad para las nuevas columnas definidas sin la aceptación explícita de estos valores.|  
    |2048|ANSI_NULL_DFLT_OFF|Altera el comportamiento de la sesión para que no utilice la compatibilidad con ANSI para la nulabilidad. Las nuevas columnas definidas sin la nulabilidad no aceptan estos valores.|  
    |4096|CONCAT_NULL_YIELDS_NULL|Devuelve un valor NULL al concatenar un valor NULL con una cadena.|  
    |8192|NUMERIC_ROUNDABORT|Genera un error cuando se produce una pérdida de precisión en una expresión.|  
    |16384|XACT_ABORT|Revierte una transacción si una instrucción Transact- SQL produce un error en tiempo de ejecución.|  
  
-   Las posiciones de bits de **opciones de usuario** son las mismas que las de la función @@OPTIONS. Cada conexión tiene su propia función @@OPTIONS, que representa el entorno de configuración. Cuando se inicia una sesión en una instancia de \ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el usuario recibe un entorno de configuración predeterminado que asigna el valor actual para **opciones de usuario** a la función @@OPTIONS. La ejecución de instrucciones SET para **user options** afecta al valor correspondiente de la función @@OPTIONS de la sesión. Todas las conexiones que se crean después de modificar esta opción recibirán el nuevo valor.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Para configurar la opción de configuración para opciones de usuario  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Conexiones** .  
  
3.  En el cuadro **Opciones predeterminadas de conexión** , seleccione uno o más atributos para configurar las opciones predeterminadas del procesamiento de consultas para todos los usuarios conectados.  
  
     De manera predeterminada, no hay ninguna opción de usuario configurada.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Para configurar la opción de configuración para opciones de usuario  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para configurar `user options` para cambiar el valor de la opción de servidor ANSI_WARNINGS.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'user options', 8 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-user-options-configuration-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de configuración para opciones de usuario  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
