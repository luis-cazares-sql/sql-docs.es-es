---
title: Habilitar TDE en SQL Server con EKM | Microsoft Docs
description: Habilite el cifrado de datos transparente en SQL Server para proteger una clave de base de datos mediante una clave asimétrica en un módulo de administración extensible de claves con Transact-SQL.
ms.custom: ''
ms.date: 07/25/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- encryption [SQL Server], TDE using an EKM
- TDE, EKM how to
- EKM, TDE how to
- Transparent Data Encryption, using EKM
ms.assetid: b892e7a7-95bd-4903-bf54-55ce08e225af
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: e455b8b27537a3f7fedd7ca0c9cad356b9d49df4
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764897"
---
# <a name="enable-tde-on-sql-server-using-ekm"></a>Habilitar TDE en SQL Server con EKM
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este artículo se describe cómo habilitar el cifrado de datos transparente (TDE) en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] para proteger una clave de cifrado de base de datos mediante una clave asimétrica almacenada en un módulo EKM (Administración extensible de claves) con [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 TDE cifra el almacenamiento de una base de datos completa mediante el uso de una clave simétrica denominada clave de cifrado de base de datos. La clave de cifrado de base de datos también se puede proteger utilizando un certificado que se protege mediante la clave maestra de base de datos de la base de datos maestra. Para obtener más información sobre cómo proteger la clave de cifrado de base de datos usando la clave maestra de base de datos, vea [Cifrado de datos transparente &#40;TDE&#41;](../../../relational-databases/security/encryption/transparent-data-encryption.md). Para obtener más información sobre cómo configurar TDE cuando [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se está ejecutando en una máquina virtual de Azure, vea [Administración extensible de claves con el Almacén de claves de Azure &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md). Para obtener más información sobre cómo configurar TDE con una clave del Almacén de claves de Azure, vea [Usar el Conector de SQL Server con características de cifrado de SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md). 

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Debe ser un usuario con muchos privilegios (como un administrador del sistema) para crear una clave de cifrado de base de datos y cifrar una base de datos. Ese usuario debe poder ser autenticado por el módulo EKM.  
  
-   Tras iniciarse, [!INCLUDE[ssDE](../../../includes/ssde-md.md)] debe abrir la base de datos. Para ello, debe crear una credencial que será autenticada por EKM y agregarla a un inicio de sesión que se basa en una clave asimétrica. Los usuarios no pueden iniciar sesión mediante ese inicio de sesión, pero [!INCLUDE[ssDE](../../../includes/ssde-md.md)] podrá autenticarse con el dispositivo EKM.  
  
-   Si la clave asimétrica almacenada en el módulo EKM se pierde, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]no podrá abrir la base de datos. Si el proveedor EKM permite hacer una copia de seguridad de la clave asimétrica, debería crear una y almacenarla en una ubicación segura.  
  
-   Las opciones y los parámetros requeridos por el proveedor de EKM pueden diferir de lo que se proporciona en el ejemplo de código siguiente. Para obtener más información, consulte al proveedor de EKM.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 En este artículo se utilizan los permisos siguientes:  
  
-   Para cambiar una opción de configuración y ejecutar la instrucción RECONFIGURE, debe tener el permiso ALTER SETTINGS de nivel de servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
-   Requiere el permiso ALTER ANY CREDENTIAL.  
  
-   Requiere el permiso ALTER ANY LOGIN.  
  
-   Requiere el permiso CREATE ASYMMETRIC KEY.  
  
-   Requiere el permiso CONTROL en la base de datos para cifrarla.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-enable-tde-using-ekm"></a>Para habilitar TDE usando EKM  
  
1.  Copie los archivos proporcionados por el proveedor EKM a una ubicación adecuada en el equipo de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . En este ejemplo, usamos la carpeta **C:\EKM** .  
  
2.  Instale los certificados en el equipo tal y como requiera el proveedor EKM.  
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] no proporciona un proveedor EKM. Cada proveedor EKM puede tener procedimientos diferentes para instalar, configurar y autorizar a los usuarios.  Consulte la documentación del proveedor EKM para completar este paso.  
  
3.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
4.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
5.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Enable advanced options.  
    sp_configure 'show advanced options', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Enable EKM provider  
    sp_configure 'EKM provider enabled', 1 ;  
    GO  
    RECONFIGURE ;  
    GO  
    -- Create a cryptographic provider, which we have chosen to call "EKM_Prov," based on an EKM provider  
  
    CREATE CRYPTOGRAPHIC PROVIDER EKM_Prov   
    FROM FILE = 'C:\EKM_Files\KeyProvFile.dll' ;  
    GO  
  
    -- Create a credential that will be used by system administrators.  
    CREATE CREDENTIAL sa_ekm_tde_cred   
    WITH IDENTITY = 'Identity1',   
    SECRET = 'q*gtev$0u#D1v'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
    GO  
  
    -- Add the credential to a high privileged user such as your   
    -- own domain login in the format [DOMAIN\login].  
    ALTER LOGIN Contoso\Mary  
    ADD CREDENTIAL sa_ekm_tde_cred ;  
    GO  
    -- create an asymmetric key stored inside the EKM provider  
    USE master ;  
    GO  
    CREATE ASYMMETRIC KEY ekm_login_key   
    FROM PROVIDER [EKM_Prov]  
    WITH ALGORITHM = RSA_512,  
    PROVIDER_KEY_NAME = 'SQL_Server_Key' ;  
    GO  
  
    -- Create a credential that will be used by the Database Engine.  
    CREATE CREDENTIAL ekm_tde_cred   
    WITH IDENTITY = 'Identity2'   
    , SECRET = 'jeksi84&sLksi01@s'   
    FOR CRYPTOGRAPHIC PROVIDER EKM_Prov ;  
  
    -- Add a login used by TDE, and add the new credential to the login.  
    CREATE LOGIN EKM_Login   
    FROM ASYMMETRIC KEY ekm_login_key ;  
    GO  
    ALTER LOGIN EKM_Login   
    ADD CREDENTIAL ekm_tde_cred ;  
    GO  
  
    -- Create the database encryption key that will be used for TDE.  
    USE AdventureWorks2012 ;  
    GO  
    CREATE DATABASE ENCRYPTION KEY  
    WITH ALGORITHM  = AES_128  
    ENCRYPTION BY SERVER ASYMMETRIC KEY ekm_login_key ;  
    GO  
  
    -- Alter the database to enable transparent data encryption.  
    ALTER DATABASE AdventureWorks2012   
    SET ENCRYPTION ON ;  
    GO  
    ```  
  
 Para obtener más información, vea lo siguiente:  
  
-   [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
-   [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
-   [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
-   [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
-   [CREATE DATABASE ENCRYPTION KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-database-encryption-key-transact-sql.md)  
  
-   [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
-   [ALTER DATABASE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-database-transact-sql.md)  
  
## <a name="see-also"></a>Consulte también  
 [Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview)  
  
