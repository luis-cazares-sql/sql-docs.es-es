---
description: Aprovisionar claves habilitadas para el enclave
title: Aprovisionamiento de claves habilitadas para el enclave | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: e28b6d18b5fe466aa239164b18ebdfe5fef0895c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534764"
---
# <a name="provision-enclave-enabled-keys"></a>Aprovisionar claves habilitadas para el enclave

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En este artículo se describe cómo aprovisionar claves habilitadas para el enclave que admiten cálculos dentro de los enclaves seguros del lado servidor que se usan en [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md). 

Los procesos y las instrucciones generales para [administrar claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) son válidos también para aprovisionar claves habilitadas para el enclave. En este artículo se abordan detalles específicos de Always Encrypted con enclaves seguro.

Para aprovisionar una clave maestra de columna habilitada para el enclave con SQL Server Management Studio o PowerShell, asegúrese de que la nueva clave admite cálculos de enclave. Esto hará que la herramienta (SSMS o PowerShell) genere la instrucción `CREATE COLUMN MASTER KEY`, que establece `ENCLAVE_COMPUTATIONS` en los metadatos de la clave maestra de columnas en la base de datos. Para más información, vea [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md). 

La herramienta también firmará digitalmente las propiedades maestras de columna con la clave maestra de columna, y almacenará la firma en los metadatos de la base de datos. La firma evita la alteración malintencionada con la configuración `ENCLAVE_COMPUTATIONS`. Los controladores cliente SQL comprueban las firmas antes de permitir el uso de enclaves. Esto proporciona a los administradores de seguridad control sobre qué datos de la columna se pueden calcular en el enclave.

`ENCLAVE_COMPUTATIONS` es inmutable, lo que significa que no se puede cambiar una vez definida la clave maestra de columna en los metadatos. Para posibilitar los cálculos de enclave con una clave de cifrado de columna que cifra una clave maestra de columna determinada, hay que rotar la clave maestra de columna y reemplazarla por una clave maestra de columna habilitada para el enclave. Vea [Rotación de claves habilitadas para el enclave](always-encrypted-enclaves-rotate-keys.md).

> [!NOTE]
> Actualmente, tanto SSMS como PowerShell admiten claves maestras de columna habilitadas para el enclave almacenadas en Azure Key Vault o en el almacén de certificados de Windows. No se admiten módulos de seguridad de hardware (mediante CNG o CAPI).

Para crear una clave de cifrado de columna habilitada para el enclave, debe asegurarse de que selecciona una clave maestra de columna habilitada para el enclave para cifrar la nueva clave. 

En las siguientes secciones se proporcionan más detalles sobre cómo aprovisionar claves habilitadas para el enclave por medio de SSMS y PowerShell.

## <a name="provision-enclave-enabled-keys-using-sql-server-management-studio"></a>Aprovisionamiento de claves habilitadas para el enclave mediante SQL Server Management Studio
En SQL Server Management Studio puede aprovisionar lo siguiente:
- Una clave maestra de columna habilitada para el enclave, mediante el cuadro de diálogo **Nueva clave maestra de columna**.
- Una clave de cifrado de columna habilitada para el enclave, mediante el cuadro de diálogo **Nueva clave de cifrado de columnas**.

> [!NOTE]
> Actualmente, el [asistente de Always Encrypted](always-encrypted-wizard.md) no permite generar claves habilitadas para el enclave, pero sí puede crear primero claves habilitadas para el enclave mediante los cuadros de diálogo anteriores y, después, cuando ejecute el asistente, seleccionar un cifrado de columnas habilitado para el enclave ya existente en las columnas que quiera cifrar.

Requisitos mínimos de versión de SSMS:

- SSMS 18.3 cuando se usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SSMS 18.8 cuando se usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="provision-enclave-enabled-column-master-keys-with-the-new-column-master-key-dialog"></a>Aprovisionamiento de claves maestras de columna habilitadas para el enclave con el cuadro de diálogo Nueva clave maestra de columna
Para aprovisionar una clave maestra de columna habilitada para el enclave, siga los pasos descritos en [Aprovisionamiento de claves maestras de columna con el cuadro de diálogo Nueva clave maestra de columna](configure-always-encrypted-keys-using-ssms.md#provision-column-master-keys-with-the-new-column-master-key-dialog). Asegúrese de seleccionar **Permitir cálculos de enclave**. Vea la siguiente captura de pantalla:

![Permitir cálculos de enclave](./media/always-encrypted-enclaves/allow-enclave-computations.png)

> [!NOTE]
> La casilla **Permitir cálculos de enclave** solo aparece si se ha configurado un enclave seguro para la base de datos. Si usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea [Configuración del enclave seguro en SQL Server](always-encrypted-enclaves-configure-enclave-type.md). Si usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)], vea [Habilitación de Intel SGX para la base de datos de Azure SQL](/azure/azure-sql/database/always-encrypted-enclaves-enable-sgx).

> [!TIP]
> Para saber si una clave maestra de columna está habilitada para el enclave, haga clic con el botón derecho en ella en el Explorador de objetos y seleccione **Propiedades**. Si la clave está habilitada para el enclave, verá **Cálculos de enclave: permitidos** en la ventana que muestra las propiedades de la clave. También puede usar la vista [sys.column_master_keys (Transact-SQL)](../../system-catalog-views/sys-column-master-keys-transact-sql.md).

### <a name="provision-enclave-enabled-column-encryption-keys-with-the-new-column-encryption-key-dialog"></a>Aprovisionamiento de claves de cifrado de columna habilitadas para el enclave con el cuadro de diálogo Nueva clave de cifrado de columnas
Para aprovisionar una clave de cifrado de columna habilitada para el enclave, siga los pasos descritos en [Aprovisionamiento de claves de cifrado de columna con el cuadro de diálogo Nueva clave de cifrado de columnas](configure-always-encrypted-keys-using-ssms.md#provision-column-encryption-keys-with-the-new-column-encryption-key-dialog). Cuando seleccione una clave de cifrado de columna, asegúrese de que está habilitada para el enclave.

> [!TIP]
> Para saber si una clave de cifrado de columna está habilitada para el enclave, haga clic con el botón derecho en ella en el Explorador de objetos y seleccione **Propiedades**. Si la clave está habilitada para el enclave, verá **Cálculos de enclave: permitidos** en la ventana que muestra las propiedades de la clave.

## <a name="provision-enclave-enabled-keys-using-powershell"></a>Aprovisionamiento de claves habilitadas para el enclave mediante PowerShell
Si quiere aprovisionar claves habilitadas para el enclave mediante PowerShell, necesitará el módulo de PowerShell SqlServer versión 21.1.18179 o posterior.

En general, los flujos de trabajo de aprovisionamiento de claves de PowerShell (con y sin separación de roles) para Always Encrypted (descritos en [Aprovisionamiento de claves de Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md)) son válidos también con las claves habilitadas para el enclave. En esta sección se describen detalles específicos de las claves habilitadas para el enclave.

El módulo de PowerShell SqlServer amplía los cmdlets [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) y [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings) con el parámetro `-AllowEnclaveComputations`, que permite especificar una clave maestra de columna habilitada para el enclave durante el proceso de aprovisionamiento. Cualquiera de estos cmdlets crea un objeto local que contiene las propiedades de una clave maestra de columna (almacenada en Azure Key Vault o en el almacén de certificados de Windows). Si se especifica, la propiedad `-AllowEnclaveComputations` marca la clave como habilitada para el enclave en el objeto local. También hace que el cmdlet acceda a la clave maestra de columna referida (en Azure Key Vault o en el almacén de certificados de Windows) para firmar las propiedades de la clave digitalmente. Una vez creado el objeto de configuración de una nueva clave maestra de columna habilitada para el enclave, se puede usar en una invocación posterior del cmdlet [**New-SqlColumnMasterKey**](/powershell/module/sqlserver/new-sqlcolumnmasterkey) para crear un objeto de metadatos que describa la nueva clave en la base de datos.

El aprovisionamiento de claves de cifrado de columna habilitadas para el enclave es igual que el aprovisionamiento de claves de cifrado de columna no habilitadas para el enclave: solo hay que procurar que la clave maestra de columna usada para cifrar la nueva clave de cifrado de columna esté habilitada para el enclave.

> [!NOTE]
> Actualmente, el módulo de PowerShell SqlServer no permite aprovisionar claves habilitadas para el enclave que estén almacenadas en módulos de seguridad de hardware (mediante CNG o CAPI).

### <a name="example---provision-enclave-enabled-keys-using-windows-certificate-store"></a>Ejemplo: aprovisionar claves habilitadas para el enclave con el almacén de certificados de Windows
En el siguiente ejemplo completo se muestra cómo aprovisionar claves habilitadas para el enclave, almacenando la clave maestra de columna en el almacén de certificados de Windows. El script se basa en el ejemplo de [Almacén de certificados de Windows sin separación de roles (ejemplo)](configure-always-encrypted-keys-using-powershell.md#windows-certificate-store-without-role-separation-example). Es importante tener en cuenta el uso del parámetro `-AllowEnclaveComputations` en el cmdlet [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings), que es la única diferencia entre los flujos de trabajo de los dos ejemplos.

```powershell
# Create a column master key in Windows Certificate Store.
$cert = New-SelfSignedCertificate -Subject "AlwaysEncryptedCert" -CertStoreLocation Cert:CurrentUser\My -KeyExportPolicy Exportable -Type DocumentEncryptionCert -KeyUsage DataEncipherment -KeySpec KeyExchange

# Import the SqlServer module.
Import-Module "SqlServer"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Create a SqlColumnMasterKeySettings object for your column master key
# using the -AllowEnclaveComputations parameter.
$cmkSettings = New-SqlCertificateStoreColumnMasterKeySettings -CertificateStoreLocation "CurrentUser" -Thumbprint $cert.Thumbprint -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName  -InputObject $database -ColumnMasterKey $cmkName
```

### <a name="example---provision-enclave-enabled-keys-using-azure-key-vault"></a>Ejemplo: aprovisionar claves habilitadas para el enclave mediante Azure Key Vault
En el siguiente ejemplo completo se muestra cómo aprovisionar claves habilitadas para el enclave, almacenando la clave maestra de columna en Azure Key Vault. El script se basa en el ejemplo de [Azure Key Vault sin separación de roles (ejemplo)](configure-always-encrypted-keys-using-powershell.md#azure-key-vault-without-role-separation-example). Es importante tener en cuenta dos diferencias entre el flujo de trabajo de las claves habilitadas para el enclave en comparación con las claves no habilitadas para el enclave. 
- En el siguiente script, [**New-SqlCertificateStoreColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlcertificatestorecolumnmasterkeysettings) usa el parámetro `-AllowEnclaveComputations` para que la nueva clave maestra de columna esté habilitada para el enclave. 
- El siguiente script llama al cmdlet [**Add-SqlAzureAuthenticationContext**](/powershell/module/sqlserver/add-sqlazureauthenticationcontext) para iniciar sesión en Azure antes de llamar al cmdlet [**New-SqlAzureKeyVaultColumnMasterKeySettings**](/powershell/module/sqlserver/new-sqlazurekeyvaultcolumnmasterkeysettings). Es necesario iniciar sesión en Azure en primer lugar, ya que el parámetro `-AllowEnclaveComputations` hace que **New-SqlAzureKeyVaultColumnMasterKeySettings** llame a Azure Key Vault para firmar las propiedades de la clave maestra de columna.

```powershell
# Create a column master key in Azure Key Vault.
Import-Module Az
Connect-AzAccount
$SubscriptionId = "<Azure SubscriptionId>"
$resourceGroup = "<resource group name>"
$azureLocation = "<datacenter location>"
$akvName = "<key vault name>"
$akvKeyName = "<key name>"
$azureCtx = Set-AzConteXt -SubscriptionId $SubscriptionId # Sets the context for the below cmdlets to the specified subscription.
New-AzResourceGroup -Name $resourceGroup -Location $azureLocation # Creates a new resource group - skip, if your desired group already exists.
New-AzKeyVault -VaultName $akvName -ResourceGroupName $resourceGroup -Location $azureLocation # Creates a new key vault - skip if your vault already exists.
Set-AzKeyVaultAccessPolicy -VaultName $akvName -ResourceGroupName $resourceGroup -PermissionsToKeys get, create, delete, list, wrapKey,unwrapKey, sign, verify -UserPrincipalName $azureCtx.Account
$akvKey = Add-AzureKeyVaultKey -VaultName $akvName -Name $akvKeyName -Destination "Software"

# Connect to your database.
$serverName = "<server name>"
$databaseName = "<database name>"
# Change the authentication method in the connection string, if needed.
$connStr = "Server = " + $serverName + "; Database = " + $databaseName + "; Integrated Security = True"
$database = Get-SqlDatabase -ConnectionString $connStr

# Authenticate to Azure - it is required before calling the next cmdlet.
Add-SqlAzureAuthenticationContext -Interactive

# Create a SqlColumnMasterKeySettings object for your column master key. 
$cmkSettings = New-SqlAzureKeyVaultColumnMasterKeySettings -KeyURL $akvKey.ID -AllowEnclaveComputations

# Create column master key metadata in the database.
$cmkName = "CMK1"
New-SqlColumnMasterKey -Name $cmkName -InputObject $database -ColumnMasterKeySettings $cmkSettings

# Generate a column encryption key, encrypt it with the column master key and create column encryption key metadata in the database. 
$cekName = "CEK1"
New-SqlColumnEncryptionKey -Name $cekName -InputObject $database -ColumnMasterKey $cmkName
```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)
- [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Uso de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md) 

## <a name="see-also"></a>Consulte también  
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
- [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)