---
description: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted
title: Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted | Microsoft Docs
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
ms.openlocfilehash: 6638144576d56a0225f03021f7cf8226643a45ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771587"
---
# <a name="example-demonstrating-use-of-azure-key-vault-provider-with-always-encrypted"></a>Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

En este ejemplo se muestra el uso del proveedor de Azure Key Vault proveedor al obtener acceso a las columnas cifradas.

[!code-csharp [AKVProvider Example#1](~/../sqlclient/doc/samples/AzureKeyVaultProviderExample.cs#1)]

> [!NOTE]
> - Para usar la característica Always Encrypted sin enclaves seguros para una aplicación .NET Standard, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior. La versión de .NET Standard admitida es 2.0 o superior. 
>
> - Para usar la característica Always Encrypted en Linux y macOS, se requiere **Microsoft.Data.SqlClient** en la versión 2.1.0 o posterior.

## <a name="see-also"></a>Consulte también

- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros](azure-key-vault-enclave-example.md)
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Uso de Always Encrypted con el proveedor de datos Microsoft .NET para SQL Server](sqlclient-support-always-encrypted.md)
