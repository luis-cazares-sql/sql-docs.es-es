---
description: Rotación de claves habilitadas para el enclave
title: Rotación | Microsoft Docs
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
ms.openlocfilehash: 8ef5e2e3cbf4e00619aeb4b0d1643f0d07100aad
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534305"
---
# <a name="rotate-enclave-enabled-keys"></a>Rotación de claves habilitadas para el enclave

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En Always Encrypted, la rotación de claves es un proceso por el cual se reemplaza una clave maestra de columna existente o una clave de cifrado de columna por una clave nueva. En este artículo se describen los casos de uso y las consideraciones para la rotación de claves específica de [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) cuando la clave inicial o la clave de destino (nueva) es una clave habilitada para el enclave. Para obtener las instrucciones generales y los procesos para administrar claves de Always Encrypted, consulte [Información general de administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md). 

Es posible que necesite rotar una clave por motivos de seguridad o de cumplimiento. Por ejemplo, si se ha puesto en peligro una clave o las directivas de su organización requieren que reemplace las claves periódicamente. Además, Always Encrypted con la rotación de claves de enclaves seguros proporciona una manera de habilitar o deshabilitar la funcionalidad de los enclave seguros del lado servidor para las columnas cifradas.

- Cuando se reemplaza una clave que no está habilitada para el enclave con una clave habilitada para el enclave, se desbloquea la funcionalidad del enclave seguro para realizar consultas en columnas protegidas con la clave. Para obtener más información, vea [Habilitación de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md).
- Cuando se reemplaza una clave habilitada para el enclave con una clave que no lo está, se deshabilita la funcionalidad del enclave seguro para realizar consultas en las columnas que están protegidas con la clave.

Si va a rotar una clave solo por motivos de seguridad o cumplimiento, y no para habilitar o deshabilitar los cálculos de enclave para las columnas, asegúrese de que la clave de destino tiene la misma configuración relativa a los enclaves que la clave de origen. Por ejemplo, si la clave de origen está habilitada para el enclave, la clave de destino también debe estarlo.

En los pasos siguientes se incluyen vínculos a artículos detallados, en función del escenario de rotación:

1. Aprovisione una nueva clave (una clave maestra de columna o una clave de cifrado de columna).
    - Para aprovisionar una nueva clave habilitada para el enclave, consulte [Aprovisionar claves habilitadas para el enclave](always-encrypted-enclaves-provision-keys.md).
    - Para aprovisionar una clave que no está habilitada para el enclave, consulte [Aprovisionamiento de claves de Always Encrypted mediante SQL Server Management Studio](configure-always-encrypted-keys-using-ssms.md) y [Aprovisionamiento de claves de Always Encrypted con PowerShell](configure-always-encrypted-keys-using-powershell.md).
2. Reemplace una clave existente por la nueva clave.
    - Si está rotando una clave de cifrado de columna y tanto la clave de origen como la clave de destino están habilitadas para el enclave, puede ejecutar la rotación (que implica volver a cifrar los datos) en contexto. Para obtener más información, vea [Configuración del cifrado de columnas en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md).
    - Para obtener los pasos detallados de la rotación de claves, vea [Rotación de claves de Always Encrypted mediante SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) y [Rotación de claves de Always Encrypted con PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)
- [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Uso de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)  

## <a name="see-also"></a>Consulte también  
- [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md)