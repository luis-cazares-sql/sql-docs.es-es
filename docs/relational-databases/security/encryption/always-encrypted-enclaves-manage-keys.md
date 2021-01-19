---
description: Administración de claves para Always Encrypted con enclaves seguros
title: Administración de claves para Always Encrypted con enclaves seguros | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.reviewer: vanto
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 68cc07fdf37dc358d0de024c3227f7225ebea27c
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534429"
---
# <a name="manage-keys-for-always-encrypted-with-secure-enclaves"></a>Administración de claves para Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) amplía la administración de claves de [Always Encrypted](always-encrypted-database-engine.md) mediante la introducción de claves habilitadas para el enclave: 

- **Clave maestra de columna habilitada para el enclave**: clave maestra de columna que se crea con la propiedad `ENCLAVE_COMPUTATIONS` especificada en el objeto de metadatos de clave maestra de columna dentro de la base de datos. 
- **Clave de cifrado de columna habilitada para el enclave**: una clave de cifrado de columna cifrada con una clave maestra de columna habilitada para el enclave. Para los cálculos en un enclave seguro del lado servidor, solo se pueden usar claves de cifrado de columnas habilitadas para el enclave. 

Los procesos y las instrucciones generales para [administrar claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md) se aplican también a la administración de claves habilitadas para el enclave. 

## <a name="managing-keys"></a>Administración de claves

En los artículos siguientes se describen los aspectos específicos de la administración de claves habilitadas para el enclave.

- [Aprovisionamiento de claves habilitadas para el enclave](always-encrypted-enclaves-provision-keys.md)
- [Rotación de claves habilitadas para el enclave](always-encrypted-enclaves-rotate-keys.md)

## <a name="next-steps"></a>Pasos siguientes
- [Aprovisionamiento de claves habilitadas para el enclave](always-encrypted-enclaves-provision-keys.md)

## <a name="see-also"></a>Consulte también  
- [Información general sobre la administración de claves de Always Encrypted](overview-of-key-management-for-always-encrypted.md)
- [CREATE COLUMN MASTER KEY (Transact-SQL)](../../../t-sql/statements/create-column-master-key-transact-sql.md)
