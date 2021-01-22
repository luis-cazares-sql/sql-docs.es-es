---
description: Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros
title: Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros | Microsoft Docs
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
ms.openlocfilehash: 4799cd725dce4eb8300717b8c89d601e9915f7d2
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534834"
---
# <a name="create-and-use-indexes-on-columns-using-always-encrypted-with-secure-enclaves"></a>Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En este artículo se describe cómo crear y usar índices en columnas cifradas mediante claves de cifrado de columna habilitadas para el enclave con [Always Encrypted con enclaves seguros](always-encrypted-enclaves.md).

Always Encrypted con enclaves seguros admite:
- Índices agrupados y no en clúster en columnas cifradas mediante cifrado determinista y claves habilitadas para el enclave.
  - Estos índices se ordenan en función del texto cifrado. No se aplica ninguna consideración especial a dichos índices. Puede administrarlos y usarlos de la misma manera que los índices de las columnas cifradas mediante cifrado determinista y claves no habilitadas para el enclave (como con Always Encrypted). 
- Índices no en clúster en columnas cifradas mediante cifrado aleatorio y claves habilitadas para el enclave.
  - Los valores de clave de la estructura de datos de índice (árbol B) se cifran y se ordenan en función de sus valores de texto no cifrado. Para obtener más información, vea [Índices en columnas habilitadas para el enclave](always-encrypted-enclaves.md#indexes-on-enclave-enabled-columns).

> [!NOTE]
> El resto de este artículo trata sobre los índices no en clúster en columnas cifradas mediante cifrado aleatorio y claves habilitadas para el enclave.

Como un índice en una columna que usa cifrado aleatorio y una clave de cifrado de columna habilitada para el enclave contiene datos cifrados (texto cifrado) ordenados en función de texto sin cifrar, el motor de SQL Server debe usar el enclave en cualquier operación que suponga la creación o la actualización de un índice o la búsqueda en este, por ejemplo:

- Creación o recompilación de un índice.
- Inserción, actualización o eliminación de una fila de una tabla (que contiene una columna indexada o cifrada), que desencadena la inserción o la eliminación de una clave de índice hacia y desde el índice.
- Ejecución de comandos `DBCC` que implican la comprobación de la integridad de los índices, por ejemplo [DBCC CHECKDB (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) o [DBCC CHECKTABLE (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md).
- Recuperación de base de datos (por ejemplo, después de que se produzca un error en SQL Server y se reinicie), si SQL Server necesita deshacer los cambios realizados en el índice (consulte más detalles a continuación).

Para todas las operaciones anteriores, es necesario que el enclave tenga la clave de cifrado de columna de la columna indexada. Se necesita la clave para descifrar las claves de índice. En general, el enclave puede obtener una clave de cifrado de columna de una de dos maneras:
- Directamente desde la aplicación cliente.
- Desde la caché de claves de cifrado de columna.

## <a name="invoke-indexing-operations-with-column-encryption-keys-provided-directly-by-the-client"></a>Invocación de operaciones de indexación con claves de cifrado de columna proporcionadas directamente por el cliente
Con este método, para que la invocación de operaciones de indexación funcione, la aplicación (incluida una herramienta, como SQL Server Management Studio, o SSMS) que emite una consulta que desencadena una operación en un índice debe hacer lo siguiente:

- Conectarse a la base de datos con Always Encrypted y cálculos de enclave habilitados para la conexión de base de datos.
- La aplicación debe tener acceso a la clave maestra de columna que protege la clave de cifrado de columna de la columna indexada.

Una vez que el Motor de SQL Server analiza la consulta de la aplicación y determina que será necesario actualizar un índice en una columna cifrada para ejecutar la consulta, indica al controlador cliente que libere la clave de cifrado de columna necesaria al enclave a través de un canal seguro. Este mecanismo es exactamente el mismo que se usa para proporcionar al enclave las claves de cifrado de columna para el procesamiento de todas las demás consultas que no usen índices. Por ejemplo, las consultas o el cifrado en contexto mediante la coincidencia de patrones y las comparaciones de intervalos.

Este método es útil para garantizar que la presencia de índices en columnas cifradas es transparente para las aplicaciones que ya están conectadas a la base de datos con Always Encrypted y cálculos de enclave habilitados. La conexión de la aplicación puede usar el enclave para el procesamiento de consultas. Después de crear un índice en una columna, el controlador dentro de la aplicación proporcionará de forma transparente claves de cifrado de columna al enclave para las operaciones de indexación. La creación de índices puede aumentar el número de consultas en las que es necesario que la aplicación envíe las claves de cifrado de columna al enclave.

Para usar este método, siga las instrucciones generales para ejecutar instrucciones mediante un enclave seguro de [Ejecución de instrucciones de Transact-SQL mediante enclaves seguros](always-encrypted-enclaves-query-columns.md).

Para obtener instrucciones paso a paso sobre cómo usar este método, consulte el [Tutorial: Creación y uso de índices en columnas basadas en enclave mediante el cifrado aleatorio](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).

## <a name="invoke-indexing-operations-using-cached-column-encryption-keys"></a>Invocación de operaciones de indexación mediante claves de cifrado de columna almacenadas en caché

Una vez que una aplicación cliente envía una clave de cifrado de columna al enclave para el procesamiento de las consultas que requieran cálculos de enclave, el enclave almacena en caché esta clave en una caché interna. Dicha caché está ubicada en el enclave y no se puede acceder a ella desde el exterior.

Si esta u otra aplicación cliente, usada por el mismo usuario u otro distinto, desencadena una operación en un índice sin proporcionar directamente el cifrado de columna exigido, el enclave buscará la clave de cifrado de columna en la caché. Como resultado, la operación en el índice se realiza correctamente, aunque la aplicación cliente no haya proporcionado la clave.

Para que este método de invocación de operaciones de indexación funcione, la aplicación debe conectarse a la base de datos sin Always Encrypted habilitado para la conexión y la clave de cifrado de columna necesaria debe estar disponible en la caché dentro del enclave.

Este método de invocación de operaciones solo se admite para consultas que no requieran claves de cifrado de columna para otras operaciones no relacionadas con los índices. Por ejemplo, una aplicación que inserta una fila mediante una instrucción `INSERT` en una tabla que contiene una columna cifrada debe conectarse a la base de datos con Always Encrypted habilitado en la cadena de conexión y debe tener acceso a las claves, independientemente de si la columna cifrada tiene o no un índice.

Este método es útil en los siguientes casos:
 - Para garantizar que la presencia de índices en columnas habilitadas para enclave que usan cifrado aleatorio sea transparente para las aplicaciones y los usuarios que no tienen acceso a las claves y los datos en texto no cifrado. 
 - Para garantizar que, al crear un índice en una columna cifrada, no se frenan las consultas existentes. Si una aplicación emite una consulta en una tabla que contiene columnas cifradas sin necesidad de tener acceso a las claves, la aplicación puede seguir ejecutándose sin tener acceso a las claves después de que un administrador de base de datos crea un índice. Por ejemplo, considere una aplicación que ejecuta la consulta siguiente en la tabla **Empleados** que contiene columnas cifradas. El administrador de base de datos no ha creado un índice en ninguna columna cifrada.

   ```sql
   DELETE FROM [dbo].[Employees] WHERE [EmployeeID] = 1;
   GO
   ```

   Si la aplicación envía la consulta a través de una conexión sin Always Encrypted y cálculos de enclave habilitados, la consulta se realizará correctamente. La consulta no desencadena ningún cálculo en las columnas cifradas. Después de que un administrador de base de datos cree un índice en cualquier columna cifrada, la consulta desencadenará la eliminación de claves de índice de los índices. En esta situación, el enclave necesita las claves de cifrado de columna. Sin embargo, la aplicación podrá continuar ejecutando esta consulta a través de la misma conexión, siempre que un propietario de datos haya proporcionado las claves de cifrado de columna al enclave.

 - Para conseguir la separación de roles al administrar los índices, ya que permite que los administradores de base de datos creen y modifiquen los índices en columnas cifradas sin tener acceso a datos confidenciales. 

> [!TIP] 
> [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md) permite enviar fácilmente todas las claves de cifrado de columna habilitadas para el enclave usadas para los índices del enclave y rellenar la caché de claves.

Para obtener instrucciones paso a paso sobre cómo usar este método, consulte el [Tutorial: Creación y uso de índices en columnas basadas en enclave mediante el cifrado aleatorio](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md). 

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)

## <a name="see-also"></a>Vea también  
- [Tutorial: Creación y uso de índices en columnas basadas en enclave mediante el cifrado aleatorio](../tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md).
- [sp_enclave_send_keys (Transact-SQL)](../../system-stored-procedures/sp-enclave-send-keys-sql.md)
