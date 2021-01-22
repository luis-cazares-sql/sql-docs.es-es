---
description: Configuración del cifrado de columnas en contexto con Transact-SQL
title: Configuración del cifrado de columnas en contexto con Transact-SQL | Microsoft Docs
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
ms.openlocfilehash: ab59eec637bd5afc127227b09445417ffa1fe4eb
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534854"
---
# <a name="configure-column-encryption-in-place-with-transact-sql"></a>Configuración del cifrado de columnas en contexto con Transact-SQL

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En este artículo se describe cómo realizar operaciones criptográficas en contexto en columnas mediante Always Encrypted con enclaves seguros con la [instrucción ALTER TABLE](../../../odbc/microsoft/alter-table-statement.md)/`ALTER COLUMN`. Para obtener información básica sobre el cifrado en contexto y los requisitos previos generales, consulte [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md).

Con la instrucción `ALTER TABLE` o `ALTER COLUMN`, puede establecer la configuración de cifrado de destino de una columna. Al ejecutar la instrucción, el enclave seguro del lado servidor cifrará, volverá a cifrar o descifrará los datos almacenados en la columna, en función de la configuración de cifrado actual y de destino que se ha especificado en la definición de columna de la instrucción. 
- Si la columna no está cifrada actualmente, se cifrará si especifica la cláusula `ENCRYPTED WITH` en la definición de columna.
- Si la columna está cifrada actualmente, se descifrará (es decir, se convertirá en una columna de texto no cifrado) si no se especifica la cláusula `ENCRYPTED WITH` en la definición de columna.
- Si la columna está cifrada actualmente, se volverá a cifrar si se especifica la cláusula `ENCRYPTED WITH` y el tipo de cifrado de columna indicado o la clave de cifrado de columna son diferentes del tipo de cifrado usado actualmente o de la clave de cifrado de columna. 

> [!NOTE]
> No se pueden combinar operaciones criptográficas con otros cambios en una sola instrucción `ALTER TABLE`/`ALTER COLUMN`, salvo al cambiar la columna a `NULL` o `NOT NULL`, o bien al cambiar una intercalación. Por ejemplo, no puede cifrar una columna y cambiar un tipo de datos de la columna en una sola instrucción `ALTER TABLE`/`ALTER COLUMN` de Transact-SQL. Use dos instrucciones independientes.

Como todas las consultas que usan un enclave seguro del lado servidor, se debe enviar una instrucción `ALTER TABLE`/`ALTER COLUMN` que desencadene el cifrado en contexto a través de una conexión con Always Encrypted y los cálculos de enclave habilitados. 

En el resto de este artículo se describe cómo desencadenar el cifrado en contexto mediante la instrucción `ALTER TABLE`/`ALTER COLUMN` de SQL Server Management Studio. Como alternativa, puede emitir `ALTER TABLE`/`ALTER COLUMN` desde Azure Data Studio o la aplicación. 

> [!NOTE]
> Actualmente, el cmdlet [Invoke-Sqlcmd](/powershell/module/sqlserver/invoke-sqlcmd) del módulo SqlServer de PowerShell y [sqlcmd](../../../tools/sqlcmd-utility.md), no admiten el uso de `ALTER TABLE`/`ALTER COLUMN` para operaciones criptográficas en contexto.

## <a name="perform-in-place-encryption-with-transact-sql-in-ssms"></a>Realización del cifrado en contexto con Transact-SQL en SSMS
### <a name="pre-requisites"></a>Requisitos previos
- Los requisitos previos se describen en [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md).
- SQL Server Management Studio 18.3 o una versión superior cuando se usa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].
- SQL Server Management Studio 18.8 o una versión superior cuando se usa [!INCLUDE[ssSDSfull](../../../includes/sssdsfull-md.md)].

### <a name="steps"></a>Pasos
1. Abra una ventana de consulta con Always Encrypted y los cálculos de enclave habilitados en la conexión de base de datos. Para obtener más información, consulte [Habilitación y deshabilitación de Always Encrypted para una conexión de base de datos](always-encrypted-query-columns-ssms.md#en-dis).
2. En la ventana de consulta, emita la instrucción `ALTER TABLE`/`ALTER COLUMN`, y especifique la configuración de cifrado de destino de una columna que quiera cifrar, descifrar o volver a cifrar. Si va a cifrar o volver a cifrar la columna, use la cláusula `ENCRYPTED WITH`. Si su columna es una columna de cadena (por ejemplo, `char`, `varchar`, `nchar` o `nvarchar`), es posible que también tenga que cambiar la intercalación a una intercalación BIN2. 
    
    > [!NOTE]
    > Si su clave maestra de columna se almacena en Azure Key Vault, es posible que se le pida que inicie sesión en Azure.

3. Borre la caché de planes de todos los lotes y procedimientos almacenados que tienen acceso a la tabla para actualizar la información de cifrado de los parámetros. 
 
    ```sql
    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```
    > [!NOTE]
    > Si no quita el plan para la consulta afectada de la memoria caché, la primera ejecución de la consulta tras el cifrado puede producir un error.

    > [!NOTE]
    > Use `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE``DBCC FREEPROCCACHE` para borrar la caché de planes con cuidado, ya que puede dar lugar a una degradación temporal del rendimiento de las consultas. Para minimizar el impacto negativo de haber borrado la memoria caché, únicamente puede quitar de forma selectiva los planes para las consultas afectadas.

4.  Llame a [sp_refresh_parameter_encryption](../../system-stored-procedures/sp-refresh-parameter-encryption-transact-sql.md) para actualizar los metadatos de los parámetros de cada módulo (procedimiento almacenado, función, vista, desencadenador) que persisten en [sys.parameters](../..//system-catalog-views/sys-parameters-transact-sql.md) y que pueden haberse invalidado al cifrar las columnas.

### <a name="examples"></a>Ejemplos
#### <a name="encrypting-a-column-in-place"></a>Para cifrar una columna en contexto
En el ejemplo siguiente se da por sentado que:
- `CEK1` es una clave de cifrado de columna habilitada para el enclave.
- La columna `SSN` es texto no cifrado y actualmente usa la intercalación de base de datos predeterminada, por ejemplo, una intercalación Latin1 distinta de BIN2 (como `Latin1_General_CI_AI_KS_WS`).

La instrucción cifra la columna `SSN` mediante el cifrado aleatorio y la clave de cifrado de columna habilitada para el enclave en contexto. También sobrescribe la intercalación de base de datos predeterminada con la intercalación BIN2 correspondiente (en la misma página de código).

La operación se realiza en línea (`ONLINE = ON`). Tenga también en cuenta la llamada a `ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE`, que vuelve a crear los planes de las consultas afectadas por el cambio en el esquema de la tabla.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char] COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
WITH
(ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-change-encryption-type"></a>Para volver a cifrar una columna en contexto a fin de cambiar el tipo de cifrado
En el ejemplo siguiente se da por sentado que:
- La columna `SSN` está cifrada mediante cifrado determinista y una clave de cifrado de columna habilitada para el enclave, `CEK1`.
- La intercalación actual, establecida en el nivel de columna, es `Latin1_General_BIN2`.

La siguiente instrucción vuelve a cifrar la columna mediante el cifrado aleatorio y la misma clave (`CEK1`).

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

#### <a name="re-encrypt-a-column-in-place-to-rotate-a-column-encryption-key"></a>Para volver a cifrar una columna en contexto a fin de girar una clave de cifrado de columna
En el ejemplo siguiente se da por sentado que:
- La columna `SSN` está cifrada mediante cifrado aleatorio y una clave de cifrado de columna habilitada para el enclave, `CEK1`.
- `CEK2` es una clave de cifrado de columna habilitada para el enclave (diferente de `CEK1`).
- La intercalación actual, establecida en el nivel de columna, es `Latin1_General_BIN2`.

La siguiente instrucción vuelve a cifrar la columna con `CEK2`.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK2]
, ENCRYPTION_TYPE = Randomized
, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```
#### <a name="decrypt-a-column-in-place"></a>Descifrado de una columna en contexto
En el ejemplo siguiente se da por sentado que:
- La columna `SSN` está cifrada mediante una clave de cifrado de columna habilitada para el enclave.
- La intercalación actual, establecida en el nivel de columna, es `Latin1_General_BIN2`.

La instrucción siguiente descifra la columna y no modifica la intercalación. Como alternativa, puede optar por cambiar la intercalación. Por ejemplo, cambie la intercalación por una que no sea BIN2 en la misma instrucción.

```sql
ALTER TABLE [dbo].[Employees]
ALTER COLUMN [SSN] [char](11) COLLATE Latin1_General_BIN2
WITH (ONLINE = ON);
GO
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
GO
```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-create-use-indexes.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)

## <a name="see-also"></a>Consulte también  
- [Solución de problemas comunes de Always Encrypted con enclaves seguros](always-encrypted-enclaves-troubleshooting.md)
- [Configuración del cifrado de columna en contexto mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-configure-encryption.md)
- [Uso de Always Encrypted con enclaves seguros para las columnas cifradas existentes](always-encrypted-enclaves-enable-for-encrypted-columns.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](../tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)