---
description: 'Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros'
title: 'Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros | Microsoft Docs'
ms.custom: ''
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
ms.openlocfilehash: 092220a2ef2715b8d5cd414ab5a4bc3e3493acb5
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534744"
---
# <a name="tutorial-develop-a-net-framework-application-using-always-encrypted-with-secure-enclaves"></a>Tutorial: Desarrollo de una aplicación de .NET Framework mediante Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only-asdb](../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En este tutorial aprenderá a desarrollar una aplicación que emite consultas de base de datos que usan un enclave seguro del lado servidor para [Always Encrypted con enclaves seguros](encryption/always-encrypted-enclaves.md). 

## <a name="prerequisites"></a>Requisitos previos
Asegúrese de que ha completado uno de los tutoriales mostrados a continuación antes de seguir los pasos siguientes de este tutorial:

- [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)
- [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)

También necesitará Visual Studio (se recomienda la versión 2019); descárguelo desde [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). El equipo de desarrollo de la aplicación debe ejecutar .NET Framework 4.7.2 o posterior.

## <a name="step-1-set-up-your-visual-studio-project"></a>Paso 1: Configuración de su proyecto de Visual Studio

Para usar Always Encrypted con enclaves seguros en una aplicación de .NET Framework, debe asegurarse de que su aplicación se compila en .NET Framework 4.7.2 y se integra con el [NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders). Además, si almacena su clave maestra de columna en Azure Key Vault, también debe integrar su aplicación con el [NuGet Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider), versión 2.2.0 o posterior. 

1. Abra Visual Studio.

2. Cree un proyecto de aplicación de consola de C\# (.NET Framework).

3. Asegúrese de que su proyecto tenga como destino al menos .NET Framework 4.7.2. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones, seleccione **Propiedades** y establezca **Marco de destino** en .NET Framework 4.7.2.

4. Instale el siguiente paquete NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders -IncludePrerelease
   ```

5. Si usa Azure Key Vault para almacenar sus claves maestras de columna, instale los siguientes paquetes NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.SqlServer.Management.AlwaysEncrypted.AzureKeyVaultProvider -IncludePrerelease -Version 2.2.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Abra el archivo App.config del proyecto.

7. Busque la sección `<configuration>` y agregue o actualice las secciones `<configSections>`.

   1. Si la sección `<configuration>` **no** contiene la sección `<configSections>`, agregue el contenido siguiente justo después de `<configuration>`.

      ```xml
      <configSections>
        <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
      </configSections>
      ```

   2. Si la sección `<configuration>` ya contiene la sección `<configSections>`, agregue la siguiente línea desde la sección `<configSections>`:

      ```xml
      <section name="SqlColumnEncryptionEnclaveProviders"  type="System.   Data.SqlClient.   SqlColumnEncryptionEnclaveProviderConfigurationSection, System.   Data,  Version=4.0.0.0, Culture=neutral,    PublicKeyToken=b77a5c561934e089" />
      ```

8. En la sección `<configuration>`, debajo de `</configSections>`, agregue una sección nueva, que especifica un proveedor de enclave que se va a usar para la atestación de los enclaves seguros del lado servidor e interactuar con ellos.

   1. Si usa [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] y el Servicio de protección de host (HGS) (utiliza la base de datos de [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md)), agregue la sección siguiente.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Este es un ejemplo completo de un archivo app.config para una aplicación de consola simple.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="VBS"  type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.HostGuardianServiceEnclaveProvider,  Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders,    Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91"/>
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

   1. Si usa [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y Microsoft Azure Attestation (utiliza la base de datos de [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)), agregue la sección siguiente.

      ```xml
      <SqlColumnEncryptionEnclaveProviders>
        <providers>
          <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
        </providers>
      </SqlColumnEncryptionEnclaveProviders>
      ```

      Este es un ejemplo completo de un archivo app.config para una aplicación de consola simple.

      ```xml
      <?xml version="1.0" encoding="utf-8" ?>
      <configuration>
        <configSections>
          <section name="SqlColumnEncryptionEnclaveProviders" type="System.Data.SqlClient.SqlColumnEncryptionEnclaveProviderConfigurationSection, System.Data, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
        </configSections>
        <SqlColumnEncryptionEnclaveProviders>
          <providers>
            <add name="SGX" type="Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders.AzureAttestationEnclaveProvider, Microsoft.SqlServer.Management.AlwaysEncrypted.EnclaveProviders, Version=15.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" />
          </providers>
        </SqlColumnEncryptionEnclaveProviders>
        <startup> 
         <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.7.2" />
        </startup>
      </configuration>
      ```

## <a name="step-2-implement-your-application-logic"></a>Paso 2: Implementar la lógica de la aplicación

La aplicación se conectará a la base de datos **ContosoHR** del [Tutorial: Introducción a Always Encrypted con enclaves seguros en SQL Server](tutorial-getting-started-with-always-encrypted-enclaves.md) o [Tutorial: Introducción a Always Encrypted con enclaves seguros en Azure SQL Database](/azure/azure-sql/database/always-encrypted-enclaves-getting-started) y ejecutará una consulta que contiene el predicado `LIKE` en la columna **SSN** y una comparación de rangos en la columna **Salary**.

1. Reemplace el contenido del archivo Program.cs (generado por Visual Studio) por el siguiente código. Actualice la cadena de conexión de base de datos con el nombre del servidor, la configuración de autenticación de la base de datos y la dirección URL de atestación del enclave del entorno.

    ```cs
    using System;
    using System.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {
    
                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                //string connectionString = "Data Source = myserver.database.windows.net; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Enclave Attestation Url = https://myattestationprovider.uks.attest.azure.net/attest/SgxEnclave; User ID=user; Password=password";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [HR].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%9838";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 20000;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();
    
                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader[0] + ", " + reader[1] + ", " + reader[2] + ", " + reader[3]);
                    }   
                    Console.ReadKey();
                }
            }
        }
    }
    ```

2. Compile y ejecute la aplicación.  

## <a name="see-also"></a>Consulte también

- [Uso de Always Encrypted con el proveedor de datos .NET Framework para SQL Server](encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
