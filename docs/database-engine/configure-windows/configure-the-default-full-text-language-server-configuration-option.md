---
title: Establecer la opción de configuración del servidor Idioma de texto completo predeterminado | Microsoft Docs
description: Obtenga información sobre la opción Idioma de texto completo predeterminado. Vea cómo configurarla a fin de especificar el idioma predeterminado que SQL Server usa para los índices de texto completo.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45e05505444d00607cae875b8dfd8bd07bf9190e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783252"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>Establecer la opción de configuración del servidor Idioma de texto completo predeterminado
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En este tema se describe cómo establecer la opción de configuración del servidor **Idioma de texto completo predeterminado** en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. La opción **Idioma de texto completo predeterminado** especifica un valor de idioma predeterminado para los índices de texto completo. Los análisis lingüísticos se realizan en todos los datos que tienen índices de texto completo y que dependen del idioma de los datos. El valor predeterminado de esta opción es el idioma del servidor. En las versiones localizadas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] establece la opción **Idioma de texto completo predeterminado** en el idioma del servidor, si existe una correspondencia apropiada. En el caso de una versión no localizada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el valor de la opción **Idioma de texto completo predeterminado** es Inglés.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
     [Seguridad](#Security)  
  
-   **Para configurar la opción de idioma de texto completo predeterminado, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Seguimiento:**  [Después de configurar la opción de idioma de texto completo predeterminado](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   El valor de la opción **Idioma de texto completo predeterminado** se usa en un índice de texto completo cuando no se especifica ningún idioma para una columna a través de la opción LANGUAGE **language_term** en las instrucciones CREATE FULLTEXT INDEX o ALTER FULLTEXT INDEX. Si el idioma predeterminado para búsqueda de texto completo no es compatible o el paquete de análisis lingüístico no está disponible, la operación CREATE o ALTER no será satisfactoria y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devolverá un mensaje de error que indica que el idioma especificado no es válido.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Esta opción es avanzada y solo debe cambiarla un administrador de base de datos con experiencia o un profesional certificado de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   La opción **Idioma de texto completo predeterminado** requiere un valor LCID. Para obtener una lista de los LCID admitidos y sus respectivos idiomas, vea [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md). Por ejemplo, es posible que haya otros idiomas disponibles de fabricantes independientes de software. Si no se detecta ningún dialecto de idioma específico, el motor de texto completo cambiará automáticamente al idioma principal.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Para configurar la opción de idioma de texto completo predeterminado  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Avanzado** .  
  
3.  En Varios, use la opción **Idioma de texto completo predeterminado** para especificar un valor de idioma predeterminado para las columnas indexadas de texto completo.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Para configurar la opción de idioma de texto completo predeterminado  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. Este ejemplo muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para establecer el valor de la opción de `default full-text` en Neerlandés (`1043`).  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a> Seguimiento: Después de configurar la opción de idioma de texto completo predeterminado  
 La configuración surte efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
