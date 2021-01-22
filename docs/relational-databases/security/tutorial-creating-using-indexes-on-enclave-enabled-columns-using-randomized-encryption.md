---
title: Índices en columnas habilitadas para enclave con cifrado aleatorio (Tutorial)
description: En este tutorial se muestra cómo crear y usar índices en columnas habilitadas para el enclave mediante el cifrado aleatorio admitido en Always Encrypted con enclaves seguros para SQL Server y Azure SQL Database.
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 5e08a42f9112379951c9fcff27693ab3700b69fd
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534824"
---
# <a name="tutorial-create-and-use-indexes-on-enclave-enabled-columns-using-randomized-encryption"></a>Tutorial: Creación y uso de índices en columnas habilitadas para enclave que usan cifrado aleatorio

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

Este tutorial le enseña a crear y usar índices en columnas habilitadas para enclave que usan el cifrado aleatorio admitido en [Always Encrypted con enclaves seguros](encryption/always-encrypted-enclaves.md). En él encontrará:

> [!div class="checklist"]
> - Cómo crear un índice cuando tiene acceso a las claves (la clave maestra de columna y la clave de cifrado de columna) que protegen la columna.
> - Cómo crear un índice cuando no tiene acceso a las claves que protegen la columna.

## <a name="prerequisites"></a>Prerrequisitos

Asegúrese de que ha completado uno de los tutoriales mostrados a continuación antes de seguir los pasos siguientes de este tutorial:

- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

## <a name="step-1-enable-accelerated-database-recovery-adr-in-your-database"></a>Paso 1: Habilitación de la recuperación de base de datos acelerada (ADR) en la base de datos

> [!NOTE]
> Este paso solo se aplica a [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]. Si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], omita este paso. ADR se habilita de forma automática en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y no se puede deshabilitar.

Microsoft recomienda encarecidamente habilitar ADR en la base de datos antes de crear el primer índice en una columna habilitada para enclave que usa cifrado aleatorio. Vea la sección [Recuperación de la base de datos](./encryption/always-encrypted-enclaves.md#database-recovery) del tutorial [Always Encrypted con enclaves seguros](./encryption/always-encrypted-enclaves.md).



1. Cierre todas las instancias SSMS que usó en el tutorial anterior. Las conexiones abiertas de base de datos se cerrarán, lo que es necesario para habilitar ADR.
1. Abra una nueva instancia de SSMS y conéctese a la instancia de [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] como sysadmin **sin** Always Encrypted habilitado para la conexión de base de datos.
    1. Inicie SSMS.
    1. En el cuadro de diálogo **Conectar a servidor**, especifique el nombre del servidor, seleccione un método de autenticación e indique sus credenciales.
    1. Haga clic en **Opciones >>** y seleccione la pestaña **Always Encrypted**.
    1. Asegúrese de que la casilla **Habilitar Always Encrypted (cifrado de columna)** **no** esté activada.
    1. Seleccione **Conectar**.
1. Abra una nueva ventana de consulta y ejecute la siguiente instrucción para habilitar ADR.

   ```sql
   ALTER DATABASE ContosoHR SET ACCELERATED_DATABASE_RECOVERY = ON;
   ```

## <a name="step-2-create-and-test-an-index-without-role-separation"></a>Paso 2: Creación y prueba de un índice sin separación de roles

En este paso, creará y probará un índice en una columna cifrada. Actuará como un usuario que asume los roles de un administrador de base de datos, que administra la base de datos, y de un propietario de datos, que tiene acceso a las claves que protegen los datos.

1. Abra una nueva instancia de SSMS y conéctese a la instancia de [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] **con** Always Encrypted habilitado para la conexión de base de datos.
   1. Inicie una nueva instancia de SSMS.
   1. En el cuadro de diálogo **Conectar a servidor**, especifique el nombre del servidor, seleccione un método de autenticación e indique sus credenciales.
   1. Haga clic en **Opciones >>** y seleccione la pestaña **Always Encrypted**.
   1. Active la casilla **Habilitar Always Encrypted (cifrado de columna)** y especifique la dirección URL de atestación del enclave (por ejemplo, `http://hgs.bastion.local/Attestation` o `https://MyAttestationProvider.us.attest.azure.net/attest/SgxEnclave`).
   1. Seleccione **Conectar**.
   1. Si se le pide habilitar la parametrización para las consultas de Always Encrypted, haga clic en **Habilitar**.
1. Si no se le solicita que habilite la parametrización de Always Encrypted, compruebe que está habilitada.
   1. Seleccione **Herramientas** en el menú principal de SSMS.
   2. Seleccione **Opciones...** .
   3. Vaya a **Ejecución de consulta** > **SQL Server** > **Avanzadas**.
   4. Asegúrese de que la opción **Habilitar parametrización para Always Encrypted** esté activada.
   5. Seleccione **Aceptar**.
1. Abra una ventana de consulta y ejecute las siguientes instrucciones para cifrar la columna **LastName** en la tabla **Empleados**. En pasos posteriores, deberá crear y utilizar un índice en esa columna.

   ```sql  
   ALTER TABLE [HR].[Employees]
   ALTER COLUMN [LastName] [nvarchar](50) COLLATE Latin1_General_BIN2 
   ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL;
   GO   
   ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
   GO
   ```

1. Cree un índice en la columna **LastName**. Puesto que está conectado a la base de datos con Always Encrypted habilitado, el controlador cliente dentro de SSMS proporciona de modo transparente **CEK1** (la clave de cifrado de columna que protege la columna **LastName**) al enclave, que es necesario para crear el índice.

   ```sql
   CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
   INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
   GO
   ```

1. Ejecute una consulta completa en la columna **LastName** y compruebe que SQL Server usa el índice al ejecutar la consulta.
   1. En la misma ventana de consulta o en otra nueva, asegúrese de que el botón **Incluir estadísticas de consultas dinámicas** de la barra de herramientas esté activado.
   1. Ejecute la siguiente consulta.

       ```sql
       DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
       SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
       GO
      ```

   1. En la pestaña **Estadísticas de consultas dinámicas** (en la parte inferior de la ventana de consulta), observe que la consulta utiliza el índice.

## <a name="step-3-create-an-index-with-role-separation"></a>Paso 3: Creación de un índice con separación de roles

En este paso, creará un índice en una columna cifrada, que simula ser dos usuarios distintos. Un usuario es un administrador de base de datos que necesita crear un índice, pero que no tiene acceso a las claves. El otro usuario es un propietario de datos, que tiene acceso a las claves.

1. Con la instancia de SSMS **sin** Always Encrypted habilitado, ejecute la siguiente instrucción para quitar el índice de la columna **LastName**.

   ```sql
   DROP INDEX IX_LastName ON [HR].[Employees]; 
   GO
   ```

1. Actúe como propietario de datos (o una aplicación que tenga acceso a las claves) y llene la caché dentro del enclave con **CEK1**.

   > [!NOTE]
   > A menos que haya reiniciado la instancia de SQL Server después del **Paso 2: Creación y prueba de un índice sin separación de roles**, este paso es redundante ya que **CEK1** ya está presente en la caché. Lo hemos agregado para demostrar cómo un propietario de datos puede proporcionar una clave al enclave, si aún no está presente en él.

   1. En la instancia de SSMS **con** Always Encrypted habilitado, ejecute las siguientes instrucciones en una ventana de consulta. La instrucción envía todas las claves de cifrado de columna habilitadas para enclave al enclave. Consulte [sp_enclave_send_keys](../system-stored-procedures/sp-enclave-send-keys-sql.md) para obtener más información.

        ```sql
        EXEC sp_enclave_send_keys;
        GO
        ```

   1. Como alternativa a ejecutar el procedimiento almacenado anterior, puede ejecutar una consulta DML que usa el enclave en la columna **LastName**. Esta acción rellenará el enclave solo con **CEK1**.

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

1. Actúe como un administrador de base de datos y cree el índice.
    1. En la instancia de SSMS **sin** Always Encrypted habilitado, ejecute las siguientes instrucciones en una ventana de consulta.

        ```sql
        CREATE INDEX IX_LastName ON [HR].[Employees] ([LastName])
        INCLUDE ([EmployeeID], [FirstName], [SSN], [Salary]);
        GO
        ```

1. Como propietario de datos, ejecute una consulta completa en la columna **LastName** y compruebe que SQL Server usa el índice al ejecutarla.
   1. En la instancia de SSMS **con** Always Encrypted habilitado, seleccione una ventana de consulta existente o abra una nueva ventana de consulta y asegúrese de que el botón **Incluir estadísticas de consultas dinámicas** de la barra de herramientas esté activado.
   1. Ejecute la siguiente consulta. 

        ```sql
        DECLARE @LastNamePrefix NVARCHAR(50) = 'Aber%';
        SELECT * FROM [HR].[Employees] WHERE [LastName] LIKE @LastNamePrefix;
        GO
        ```

   1. En **Estadísticas de consultas dinámicas** (en la parte inferior de la ventana de consulta), observe que la consulta utiliza el índice.

## <a name="next-steps"></a>Pasos siguientes
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)

## <a name="see-also"></a>Consulte también
- [Creación y uso de índices en columnas mediante Always Encrypted con enclaves seguros](encryption/always-encrypted-enclaves-create-use-indexes.md)
