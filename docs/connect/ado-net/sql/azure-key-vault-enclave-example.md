---
description: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros
title: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 548c5e8716f4ba15de675de856a5e5bd8206da35
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534477"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted-enabled-with-secure-enclaves"></a>Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

En este ejemplo se muestra el uso del proveedor de Azure Key Vault proveedor al obtener acceso a las columnas cifradas.

[!code-csharp [Azure Key Vault Provider with Enclave Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderWithEnclaveProviderExample.cs#1)]

> [!NOTE]
> - Para usar Always Encrypted con enclave seguros para una aplicación .NET Standard, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior. La versión de .NET Standard admitida es 2.1 o superior. 
>
> - Para usar Always Encrypted con enclaves seguro en Linux y macOS, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior.

## <a name="see-also"></a>Consulte también

- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso de Always Encrypted con el proveedor de datos Microsoft .NET para SQL Server](sqlclient-support-always-encrypted.md)
