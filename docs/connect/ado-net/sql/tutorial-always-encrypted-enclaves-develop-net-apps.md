---
description: 'Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros'
title: 'Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros | Microsoft Docs'
ms.custom: ''
ms.date: 07/09/2020
ms.reviewer: v-kaywon
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: karinazhou
ms.author: v-jizho2
ms.openlocfilehash: 59c377d5055e8eb3858e1005c50d9c4dfb8334ab
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771573"
---
# <a name="tutorial-develop-a-net-application-using-always-encrypted-with-secure-enclaves"></a>Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

[!INCLUDE [appliesto-netfx-netcore-xxxx-md](../../../includes/appliesto-netfx-netcore-xxxx-md.md)]

En este tutorial aprenderá a desarrollar una aplicación sencilla que emite consultas de base de datos que usan un enclave seguro de servidor para [Always Encrypted con enclaves seguros](../../../relational-databases/security/encryption/always-encrypted-enclaves.md).

> [!NOTE]
> Always Encrypted con enclaves seguros solo se admite en Windows.

## <a name="prerequisites"></a>Requisitos previos

Este tutorial es la continuación del [Tutorial: Introducción a Always Encrypted con enclaves seguros con SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md). Asegúrese de haberlo completado antes de seguir los pasos siguientes.

Además, se necesita Visual Studio (se recomienda la versión 2019); puede descargarlo desde [https://visualstudio.microsoft.com/](https://visualstudio.microsoft.com). El entorno de desarrollo de la aplicación debe usar .NET Framework 4.6 o posterior o .NET Core 2.1 o posterior.

## <a name="step-1-set-up-your-visual-studio-project"></a>Paso 1: Configuración de su proyecto de Visual Studio

Para usar Always Encrypted con enclaves seguros en una aplicación .NET Framework, debe asegurarse de que la aplicación tiene como destino .NET Framework 4.6 o superior. Para usar Always Encrypted con enclaves seguros en una aplicación .NET Core, debe asegurarse de que la aplicación tiene como destino .NET Core 2.1 o superior.

Además, si almacena su clave maestra de columna en Azure Key Vault, también debe integrar su aplicación con el [NuGet Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider](https://www.nuget.org/packages/Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider).

1. Abra Visual Studio.

2. Cree un proyecto de aplicación de consola de C\# (.NET Framework/Core).

3. Asegúrese de que su proyecto tenga como destino al menos .NET Framework 4.6 o .NET Core 2.1. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones, seleccione Propiedades y establezca el marco de destino.

4. Instale el siguiente paquete NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.Data.SqlClient -Version 1.1.0
   ```

5. Si usa Azure Key Vault para almacenar sus claves maestras de columna, instale los siguientes paquetes NuGet yendo a **Herramientas** (menú principal) > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**. Ejecute el siguiente código en la Consola del Administrador de paquetes.

   ```powershell
   Install-Package Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider -Version 1.0.0
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   ```

6. Especifique `Attestation Protocol` y `Enclave Attestation Url` en la cadena de conexión, que se utilizarán en la aplicación para comunicarse con SQL Server.

  ```cs
   Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Column Encryption Setting = Enabled
   ```

## <a name="step-2-implement-your-application-logic"></a>Paso 2: Implementar la lógica de la aplicación

La aplicación se conectará a la base de datos **ContosoHR** del [Tutorial: Introducción a Always Encrypted con enclaves seguros con SSMS](../../../relational-databases/security/tutorial-getting-started-with-always-encrypted-enclaves.md) y ejecutará una consulta que contiene el predicado `LIKE` en la columna **SSN** y una comparación de rangos en la columna **Salary**.

1. Reemplace el contenido del archivo Program.cs (generado por Visual Studio) por el siguiente código. Actualice la cadena de conexión de base de datos con el nombre de servidor y la dirección URL de atestación del enclave de su entorno. También puede actualizar la configuración de la autenticación de base de datos.

    ```cs
    using System;
    using Microsoft.Data.SqlClient;
    using System.Data;

    namespace ConsoleApp1
    {
        class Program
        {
            static void Main(string[] args)
            {

                string connectionString = "Data Source = myserver; Initial Catalog = ContosoHR; Column Encryption Setting = Enabled;Attestation Protocol = HGS; Enclave Attestation Url = http://hgs.bastion.local/Attestation; Integrated Security = true";

                using (SqlConnection connection = new SqlConnection(connectionString))
                {
                    connection.Open();

                    SqlCommand cmd = connection.CreateCommand();
                    cmd.CommandText = @"SELECT [SSN], [FirstName], [LastName], [Salary] FROM [dbo].[Employees] WHERE [SSN] LIKE @SSNPattern AND [Salary] > @MinSalary;";

                    SqlParameter paramSSNPattern = cmd.CreateParameter();

                    paramSSNPattern.ParameterName = @"@SSNPattern";
                    paramSSNPattern.DbType = DbType.AnsiStringFixedLength;
                    paramSSNPattern.Direction = ParameterDirection.Input;
                    paramSSNPattern.Value = "%1111";
                    paramSSNPattern.Size = 11;

                    cmd.Parameters.Add(paramSSNPattern);

                    SqlParameter MinSalary = cmd.CreateParameter();

                    MinSalary.ParameterName = @"@MinSalary";
                    MinSalary.DbType = DbType.Currency;
                    MinSalary.Direction = ParameterDirection.Input;
                    MinSalary.Value = 900;

                    cmd.Parameters.Add(MinSalary);
                    cmd.ExecuteNonQuery();

                    SqlDataReader reader = cmd.ExecuteReader();
                    while (reader.Read())

                    {
                        Console.WriteLine(reader);
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

- [Uso de Always Encrypted con el proveedor de datos Microsoft .NET para SQL Server](sqlclient-support-always-encrypted.md)
- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted](azure-key-vault-example.md)
- [Ejemplo que muestra el uso del proveedor de Azure Key Vault con Always Encrypted habilitado con enclaves seguros](azure-key-vault-enclave-example.md)
