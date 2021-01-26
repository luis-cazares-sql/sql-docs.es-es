---
title: Cambiar el modo de autenticación del servidor
description: Obtenga información sobre cómo cambiar el modo de autenticación del servidor en SQL Server. Para esta tarea, puede usar SQL Server Management Studio o Transact-SQL.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- authentication [SQL Server], changing modes
- server authentication mode [SQL Server]
- modifying server authentication mode
ms.assetid: 79babcf8-19fd-4495-b8eb-453dc575cac0
author: markingmyname
ms.author: maghan
ms.custom: ''
ms.date: 02/18/2020
ms.openlocfilehash: a4cdc32bd0514be768ac8ed1723fc88840a7e930
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783422"
---
# <a name="change-server-authentication-mode"></a>Cambio del modo de autenticación del servidor

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo cambiar el modo de autenticación del servidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Durante la instalación, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] se establece en **Modo de autenticación de Windows** o **Modo de autenticación de Windows y SQL Server**. Tras la instalación, puede cambiar el modo de autenticación en cualquier momento.

Si se selecciona **Modo de autenticación de Windows** durante la instalación, el inicio de sesión de sa está deshabilitado y el programa de instalación asigna una contraseña. Si posteriormente se cambia al **Modo de autenticación de Windows y SQL Server**, el inicio de sesión de sa permanece deshabilitado. Para usar el inicio de sesión de sa, use la instrucción ALTER LOGIN para habilitar el inicio de sesión de sa y asignar una nueva contraseña. El inicio de sesión de sa solo se puede conectar al servidor mediante la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

## <a name="before-you-begin"></a>Antes de empezar

La cuenta sa es una cuenta conocida de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y suele ser el objetivo de los usuarios malintencionados. No habilite la cuenta sa a menos que su aplicación lo requiera. Es importante que use una contraseña segura en el inicio de sesión sa.

## <a name="change-authentication-mode-with-ssms"></a>Cambio del modo de autenticación con SSMS

1. En el Explorador de objetos de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , haga clic con el botón derecho en el servidor y, después, haga clic en **Propiedades**.

2. En la página **Seguridad** , bajo **Autenticación de servidor**, seleccione el nuevo modo de autenticación del servidor y haga clic en **Aceptar**.

3. En el cuadro de diálogo [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , haga clic en **Aceptar** para confirmar el requisito de reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

4. En el Explorador de objetos, haga clic con el botón derecho en el servidor y, después, haga clic en **Reiniciar**. Si el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando, también debe reiniciarse.

## <a name="enable-sa-login"></a>Habilitación del inicio de sesión sa

Puede habilitar el inicio de sesión **sa** con SSMS o T-SQL.

### <a name="use-ssms"></a>Usar SSMS

1. En el Explorador de objetos, expanda **Seguridad**, expanda Inicios de sesión, haga clic con el botón derecho en **sa** y, después, haga clic en **Propiedades**.

2. En la página **General**, es posible que tenga que crear y confirmar una contraseña para el inicio de sesión de **sa**.

3. En la página **Estado** , en la sección **Inicio de sesión** , haga clic en **Habilitado** y, a continuación, en **Aceptar**.

### <a name="using-transact-sql"></a>Usar Transact-SQL

En el ejemplo siguiente se habilita el inicio de sesión de sa y se establece una nueva contraseña. Reemplace `<enterStrongPasswordHere>` con una contraseña segura antes de ejecutarlo.

```sql  
ALTER LOGIN sa ENABLE ;  
GO  
ALTER LOGIN sa WITH PASSWORD = '<enterStrongPasswordHere>' ;  
GO  
```

## <a name="change-authentication-mode-t-sql"></a>Cambio del modo de autenticación (T-SQL)

En el ejemplo siguiente se cambia la autenticación del servidor de modo mixto (Windows+SQL) a solo Windows.

> [!CAUTION]
> En el ejemplo siguiente se usa un procedimiento almacenado extendido para modificar el registro del servidor. Es posible que se produzcan problemas graves si el registro se modifica de forma incorrecta. Estos problemas podrían requerir volver a instalar el sistema operativo. Microsoft no puede garantizar que estos problemas se puedan resolver. Modifique el registro bajo su responsabilidad.

```sql
USE [master]
GO
EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', 
     N'Software\Microsoft\MSSQLServer\MSSQLServer',
     N'LoginMode', REG_DWORD, 1
GO
```

> [!Note]
> Los permisos necesarios para cambiar el modo de autenticación son [sysadmin](../../relational-databases/security/authentication-access/server-level-roles.md#fixed-server-level-roles) o [Control Server](../../relational-databases/security/permissions-database-engine.md)

## <a name="see-also"></a>Vea también

- [Contraseñas seguras](../../relational-databases/security/strong-passwords.md)
- [Consideraciones de seguridad para una instalación de SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)
- [Conectarse a SQL Server cuando los administradores del sistema no tienen acceso](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)
