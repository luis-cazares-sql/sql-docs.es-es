---
description: Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros
title: Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 67b74e36ff5b2872e619a4b26fabdd05428e6a16
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534844"
---
# <a name="configure-column-encryption-in-place-using-always-encrypted-with-secure-enclaves"></a>Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

[Always Encrypted con enclaves seguros](always-encrypted-enclaves.md) admite operaciones criptográficas en columnas de base de datos en contexto, dentro de un enclave seguro en [!INCLUDE[ssde-md](../../../includes/ssde-md.md)]. El cifrado en contexto elimina la necesidad de trasladar los datos fuera de la base de datos en operaciones de este tipo, lo que hace que las operaciones criptográficas sean más rápidas y confiables. 

> [!NOTE]
> A pesar de las ventajas de rendimiento del cifrado en contexto, las operaciones criptográficas en tablas muy grandes pueden tardar mucho tiempo y consumir recursos esenciales, lo que puede afectar al rendimiento y la disponibilidad de las aplicaciones.

El cifrado en contexto también permite desencadenar operaciones criptográficas con la instrucción [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md), lo que no es posible sin un enclave.

## <a name="prerequisites"></a>Requisitos previos
Estas son las operaciones criptográficas admitidas y los requisitos de las claves de cifrado de columnas que se usan en esas operaciones:
- Cifrar una columna de texto no cifrado. La clave de cifrado de columna usada para cifrar la columna debe estar habilitada para el enclave.
- Volver a cifrar una columna cifrada con un tipo de cifrado o una clave de cifrado de columna nuevos. Tanto la clave de cifrado de columna actual como la clave de cifrado de columna nueva (si es distinta de la actual) deben estar habilitadas para el enclave.
- Descifrar una columna cifrada. La clave de cifrado de columna que protege la columna debe estar habilitada para el enclave.

Vea [Administración de claves para Always Encrypted con enclaves seguros](always-encrypted-enclaves-manage-keys.md) para más información sobre cómo asegurarse de que las claves de cifrado de columnas están habilitadas para el enclave.

También debe asegurarse de que el entorno cumple los [Requisitos previos para ejecutar instrucciones con enclaves seguros](always-encrypted-enclaves-query-columns.md#prerequisites-for-running-statements-using-secure-enclaves) generales.

Un usuario o una aplicación que desencadena operaciones criptográficas debe tener permisos para realizar cambios de esquema en la tabla que contiene las columnas afectadas, así como para acceder a las claves maestras de columna implicadas en las operaciones y a los metadatos de clave relevantes en la base de datos.

El cifrado en contexto solo se puede desencadenar mediante [ALTER TABLE ALTER COLUMN (Transact-SQL)](../../../t-sql/statements/alter-table-transact-sql.md) desde SQL Server Management Studio o la aplicación personalizada. Vea [Configuración del cifrado de columnas en contexto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md).

> [!NOTE]
> Actualmente, el [Asistente de Always Encrypted](always-encrypted-wizard.md) y el cmdlet [Set-SqlColumnEncryption](/powershell/module/sqlserver/set-sqlcolumnencryption) no admiten el cifrado en contexto, y siempre descargan los datos de las operaciones criptográficas, incluso cuando la configuración cumple con los requisitos anteriores. 

## <a name="next-steps"></a>Pasos siguientes
- [Configuración del cifrado de columnas en contexto con Transact-SQL](always-encrypted-enclaves-configure-encryption-tsql.md)
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Consulte también  
- [Solución de problemas comunes de Always Encrypted con enclaves seguros](always-encrypted-enclaves-troubleshooting.md)