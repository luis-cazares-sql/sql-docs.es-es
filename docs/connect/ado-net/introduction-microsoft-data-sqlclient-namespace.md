---
title: Introducción al espacio de nombres Microsoft.Data.SqlClient
description: Infórmese sobre el espacio de nombres Microsoft.Data.SqlClient y por qué es la forma preferida de conectarse a SQL en el caso de las aplicaciones .NET.
ms.date: 11/19/2020
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: e966e4f2f43ebe546d6baa0b757f682f3eca205b
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596357"
---
# <a name="introduction-to-microsoftdatasqlclient-namespace"></a>Introducción al espacio de nombres Microsoft.Data.SqlClient

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="release-notes-for-microsoftdatasqlclient-21"></a>Notas de la versión de Microsoft.Data.SqlClient 2.1

Las notas de la versión también están disponibles en el repositorio de GitHub: [Notas de la versión de 2.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.1).

### <a name="new-features"></a>Nuevas características

### <a name="cross-platform-support-for-always-encrypted"></a>Compatibilidad entre plataformas para Always Encrypted
Microsoft.Data.SqlClient v2.1 amplía la compatibilidad con Always Encrypted en las siguientes plataformas:

| Compatibilidad con Always Encrypted | Compatibilidad de Always Encrypted con enclaves seguros  | Versión de .NET Framework de destino | Microsoft.Data.SqlClient Version | Sistema operativo |
|:--|:--|:--|:--:|:--:|
| Sí | Sí | .NET Framework 4.6+ | 1.1.0 | Windows |
| Sí | Sí | .NET Core 2.1+ | 2.1.0+<sup>1</sup> | Windows, Linux, macOS |
| Yes | No<sup>2</sup> | .NET Standard 2.0 | 2.1.0+ | Windows, Linux, macOS |
| Sí | Sí | .NET Standard 2.1+ | 2.1.0+ | Windows, Linux, macOS |

> [!NOTE]
> <sup>1</sup> Antes de la versión 2.1 de Microsoft.Data.SqlClient, Always Encrypted solo se admite en Windows.
> <sup>2</sup> Always Encrypted con enclaves seguros no se admite en .NET Standard 2.0.

### <a name="azure-active-directory-device-code-flow-authentication"></a>Autenticación de flujo de código de dispositivo de Azure Active Directory
Microsoft.Data.SqlClient v2.1 proporciona compatibilidad para la autenticación "flujo de código de dispositivo" con MSAL.NET.
Documentación de referencia: [Flujo de concesión de autorización de dispositivo de OAuth 2.0](/azure/active-directory/develop/v2-oauth2-device-code)

Ejemplo de cadena de conexión:

`Server=<server>.database.windows.net; Authentication=Active Directory Device Code Flow; Database=Northwind;`

La siguiente API permite personalizar el mecanismo de devolución de llamada del flujo de código del dispositivo:

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetDeviceCodeFlowCallback(Func<DeviceCodeResult, Task> deviceCodeFlowCallbackMethod)
}
```

### <a name="azure-active-directory-managed-identity-authentication"></a>Autenticación de identidad administrada de Azure Active Directory
Microsoft.Data.SqlClient v2.1 incluye compatibilidad con la autenticación de Azure Active Directory mediante [identidades administradas](/azure/active-directory/managed-identities-azure-resources/overview).

Se admiten las siguientes palabras clave de modo de autenticación:
- Identidad administrada de Active Directory
- MSI de Active Directory (para la compatibilidad entre controladores de MS SQL)

Ejemplos de cadena de conexión:

```cs
// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; Initial Catalog={db};"

// For System Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory MSI; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"

// For User Assigned Managed Identity
"Server={serverURL}; Authentication=Active Directory Managed Identity; User Id={ObjectIdOfManagedIdentity}; Initial Catalog={db};"
```

### <a name="azure-active-directory-interactive-authentication-enhancements"></a>Mejoras en la autenticación de Azure Active Directory interactivo
Microsoft.Data.SqlClient v2.1 agrega las siguientes API para personalizar la experiencia de autenticación "Active Directory interactivo":

```csharp
public class ActiveDirectoryAuthenticationProvider
{
    // For .NET Framework targeted applications only
    public void SetIWin32WindowFunc(Func<IWin32Window> iWin32WindowFunc);

    // For .NET Standard targeted applications only
    public void SetParentActivityOrWindowFunc(Func<object> parentActivityOrWindowFunc);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void SetAcquireAuthorizationCodeAsyncCallback(Func<Uri, Uri, CancellationToken, Task<Uri>> acquireAuthorizationCodeAsyncCallback);

    // For .NET Framework, .NET Core and .NET Standard targeted applications
    public void ClearUserTokenCache();
}
```

### <a name="sqlclientauthenticationproviders-configuration-section"></a>Sección de configuración `SqlClientAuthenticationProviders`
Microsoft.Data.SqlClient v2.1 presenta una nueva sección de configuración, `SqlClientAuthenticationProviders` (un clon de `SqlAuthenticationProviders` existente). La sección de configuración existente, `SqlAuthenticationProviders`, todavía se admite por compatibilidad con versiones anteriores cuando se define el tipo adecuado.

La nueva sección permite que los archivos de configuración de la aplicación contengan una sección SqlAuthenticationProviders para System.Data.SqlClient y una sección SqlClientAuthenticationProviders section para Microsoft.Data.SqlClient.


### <a name="azure-active-directory-authentication-using-an-application-client-id"></a>Autenticación de Azure Active Directory con un identificador de cliente de aplicación
Microsoft.Data.SqlClient v2.1 incluye compatibilidad para pasar un identificador de cliente de aplicación definido por el usuario a la Biblioteca de autenticación de Microsoft. El identificador de cliente de aplicación se usa al autenticarse con Azure Active Directory.

Se introdujeron las siguientes API nuevas:

1. Se ha introducido un nuevo constructor en ActiveDirectoryAuthenticationProvider:\
_[Se aplica a todas las plataformas de .NET (.NET Framework, .NET Core y .NET Standard)]_

```csharp
public ActiveDirectoryAuthenticationProvider(string applicationClientId)
```

Uso:
```csharp
string APP_CLIENT_ID = "<GUID>";
SqlAuthenticationProvider customAuthProvider = new ActiveDirectoryAuthenticationProvider(APP_CLIENT_ID);
SqlAuthenticationProvider.SetProvider(SqlAuthenticationMethod.ActiveDirectoryInteractive, customAuthProvider);

using (SqlConnection sqlConnection = new SqlConnection("<connection_string>")
{
    sqlConnection.Open();
}
```

2. Se ha introducido una nueva propiedad de configuración en `SqlAuthenticationProviderConfigurationSection` y `SqlClientAuthenticationProviderConfigurationSection`:\
_[Se aplica a .NET Framework and .NET Core]_

```csharp
internal class SqlAuthenticationProviderConfigurationSection : ConfigurationSection
{
    ...
    [ConfigurationProperty("applicationClientId", IsRequired = false)]
    public string ApplicationClientId => this["applicationClientId"] as string;
}

// Inheritance
internal class SqlClientAuthenticationProviderConfigurationSection : SqlAuthenticationProviderConfigurationSection
{ ... }
```

Uso:
```xml
<configuration>
    <configSections>
        <section name="SqlClientAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlClientAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlClientAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>

<!--or-->

<configuration>
    <configSections>
        <section name="SqlAuthenticationProviders"
                         type="Microsoft.Data.SqlClient.SqlAuthenticationProviderConfigurationSection, Microsoft.Data.SqlClient" />
    </configSections>
    <SqlAuthenticationProviders applicationClientId ="<GUID>" />
</configuration>
```

### <a name="data-classification-v2-support"></a>Compatibilidad con la clasificación de datos v2
Microsoft.Data.SqlClient v2.1 incluye compatibilidad con la información de "rango de confidencialidad" de la clasificación de datos. Ahora están disponibles las siguientes API nuevas:

```csharp
public class SensitivityClassification
{
    public SensitivityRank SensitivityRank;
}

public class SensitivityProperty
{
    public SensitivityRank SensitivityRank;
}

public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}
```

### <a name="server-process-id-for-an-active-sqlconnection"></a>Identificador de proceso de servidor para `SqlConnection` activo
Microsoft.Data.SqlClient v2.1 introduce una nueva propiedad `SqlConnection`, `ServerProcessId`, en una conexión activa.

```csharp
public class SqlConnection
{
    // Returns the server process Id (SPID) of the active connection.
    public int ServerProcessId;
}
```

### <a name="trace-logging-support-in-native-sni"></a>Compatibilidad del registro de seguimiento en SNI nativo
Microsoft.Data.SqlClient v2.1 amplía la implementación de `SqlClientEventSource` existente para habilitar el seguimiento de eventos en SNI.dll. Los eventos deben capturarse con una herramienta como Xperf.

El seguimiento se puede habilitar mediante el envío de un comando a `SqlClientEventSource` como se muestra a continuación:

```csharp
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```


### <a name="command-timeout-connection-string-property"></a>Propiedad de la cadena de conexión "Tiempo de espera del comando"
Microsoft.Data.SqlClient v2.1 introduce la propiedad de la cadena de conexión "Tiempo de espera del comando" para reemplazar el valor predeterminado de 30 segundos. El tiempo de espera para los comandos individuales se puede invalidar mediante la propiedad `CommandTimeout` en SqlCommand.

Ejemplos de cadena de conexión:

`"Server={serverURL}; Initial Catalog={db}; Integrated Security=true; Command Timeout=60"`

### <a name="removal-of-symbols-from-native-sni"></a>Eliminación de símbolos de SNI nativo
Con Microsoft.Data.SqlClient v.2.1, hemos quitado los símbolos introducidos en [v2.0.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI/2.0.0) de NuGet [Microsoft.Data.SqlClient.SNI.runtime](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime) a partir de la [v2.1.1](https://www.nuget.org/packages/Microsoft.Data.SqlClient.SNI.runtime/2.1.1). Los símbolos públicos se publican ahora en el servidor de símbolos de Microsoft para herramientas como BinSkim que requieren acceso a símbolos públicos.

### <a name="source-linking-of-microsoftdatasqlclient-symbols"></a>Vinculación de origen de símbolos de Microsoft.Data.SqlClient
A partir de Microsoft.Data.SqlClient v2.1, los símbolos de Microsoft.Data.SqlClient se vinculan por código fuente y se publican en el servidor de símbolos de Microsoft para una experiencia de depuración mejorada sin necesidad de descargar el código fuente.


## <a name="release-notes-for-microsoftdatasqlclient-20"></a>Notas de la versión de Microsoft.Data.SqlClient 2.0

Las notas de la versión también están disponibles en el repositorio de GitHub: [Notas de la versión 2.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/2.0).

### <a name="breaking-changes"></a>Últimos cambios

- El modificador de acceso de la interfaz del proveedor de enclave `SqlColumnEncryptionEnclaveProvider` ha cambiado de `public` a `internal`.

- Las constantes de la clase `SqlClientMetaDataCollectionNames` se han actualizado para reflejar los cambios en SQL Server.

- El controlador realizará ahora la validación del certificado de servidor cuando el servidor SQL Server de destino aplique el cifrado TLS, que es el valor predeterminado para las conexiones de Azure.

- `SqlDataReader.GetSchemaTable()` ahora devuelve un elemento `DataTable` vacío en lugar de `null`.

- El controlador ahora realiza el redondeo de la escala decimal para que coincida con el comportamiento de SQL Server. Para obtener compatibilidad con versiones anteriores, el comportamiento anterior del truncamiento se puede habilitar mediante un modificador AppContext.

- En el caso de las aplicaciones .NET Framework que consumen **Microsoft.Data.SqlClient**, los archivos SNI.dll descargados previamente en las carpetas `bin\x64` y `bin\x86` ahora se denominan `Microsoft.Data.SqlClient.SNI.x64.dll` y ` Microsoft.Data.SqlClient.SNI.x86.dll`, y se descargarán en el directorio `bin`.

- Por coherencia, los sinónimos nuevos de la propiedad cadena de conexión reemplazarán a las propiedades anteriores cuando se recupere la cadena de conexión de `SqlConnectionStringBuilder`. [Más información](#new-connection-string-property-synonyms)

### <a name="new-features"></a>Nuevas características

#### <a name="dns-failure-resiliency"></a>Resistencia ante errores de DNS

Ahora, el controlador copiará en caché las direcciones IP de todas las conexiones correctas a un punto de conexión de SQL Server que admita la característica. Si se produce un error de resolución de DNS durante un intento de conexión, el controlador intentará establecer una conexión mediante una dirección IP copiada en caché para ese servidor, si existe.

#### <a name="eventsource-tracing"></a>Seguimiento de EventSource

Esta versión permite la captura de registros de seguimiento de eventos para la depuración de aplicaciones. Para capturar estos eventos, las aplicaciones cliente deben escuchar eventos de la implementación de EventSource de SqlClient:

```
Microsoft.Data.SqlClient.EventSource
```

Para obtener más información, vea cómo [habilitar el seguimiento de eventos en SqlClient](enable-eventsource-tracing.md).

#### <a name="enabling-managed-networking-on-windows"></a>Habilitación de redes administradas en Windows

Un nuevo modificador de AppContext **"Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows"** permite el uso de una implementación de SNI administrada en Windows con fines de prueba y depuración. Este modificador alternará el comportamiento del controlador a fin de usar un SNI administrado en los proyectos de .NET Core 2.1+ y .NET Standard 2.0+ en Windows, eliminando todas las dependencias de las bibliotecas nativas para la biblioteca Microsoft.Data.SqlClient.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.UseManagedNetworkingOnWindows", true);
```

Vea [Modificadores de AppContext en SqlClient](appcontext-switches.md) para obtener una lista completa de los modificadores disponibles en el controlador.

#### <a name="enabling-decimal-truncation-behavior"></a>Habilitación del comportamiento de truncamiento decimal

El controlador redondeará la escala de datos decimal de forma predeterminada, tal y como lo hace SQL Server. Para obtener la compatibilidad con versiones anteriores, puede establecer el modificador de AppContext **"Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal"** en **true**.

```csharp
AppContext.SetSwitch("Switch.Microsoft.Data.SqlClient.TruncateScaledDecimal", true);
```

#### <a name="new-connection-string-property-synonyms"></a>Sinónimos nuevos de la propiedad de cadena de conexión

Se han agregado sinónimos nuevos para las siguientes propiedades de cadena de conexión existentes a fin de evitar la confusión de espaciado en torno a las propiedades con más de una palabra. Los nombres de propiedades anteriores seguirán siendo compatibles con versiones anteriores, pero las propiedades nuevas de cadena de conexión ahora se incluirán cuando se recupere la cadena de conexión de [SqlConnectionStringBuilder](/dotnet/api/microsoft.data.sqlclient.sqlconnectionstringbuilder).

|Propiedad existente de cadena de conexión|Nuevo sinónimo|
|-----------------------------------|-----------|
| Intención de aplicaciones | Intención de aplicaciones |
| ConnectRetryCount | Recuento de reintentos de conexión |
| ConnectRetryInterval | Intervalo de reintentos de conexión |
| PoolBlockingPeriod | Período de bloqueo del grupo |
| MultipleActiveResultSets | Conjunto de resultados activo múltiple (MARS) |
| MultiSubnetFailover | Conmutación por error de varias subredes |
| TransparentNetworkIPResolution | Resolución de IP de red transparente |
| TrustServerCertificate | TrustServerCertificate |

#### <a name="sqlbulkcopy-rowscopied-property"></a>Propiedad RowsCopied de SqlBulkCopy

La propiedad RowsCopied proporciona acceso de solo lectura al número de filas que se han procesado en la operación de copia masiva en curso. Este valor no necesariamente tiene que ser igual al número final de filas agregadas a la tabla de destino.

#### <a name="connection-open-overrides"></a>Invalidaciones de apertura de la conexión

El comportamiento predeterminado de SqlConnection.Open() se puede invalidar para deshabilitar el retraso de diez segundos y los reintentos de conexión automática que desencadenan los errores transitorios.

```csharp
using SqlConnection sqlConnection = new SqlConnection("Data Source=(local);Integrated Security=true;Initial Catalog=AdventureWorks;");
sqlConnection.Open(SqlConnectionOverrides.OpenWithoutRetry);
```

> [!NOTE]
> Tenga en cuenta que esta invalidación solo se puede aplicar a SqlConnection.Open() y no a SqlConnection.OpenAsync().

#### <a name="username-support-for-active-directory-interactive-mode"></a>Compatibilidad con el nombre de usuario para el modo interactivo de Active Directory

Se puede especificar un nombre de usuario en la cadena de conexión cuando se usa el modo interactivo de autenticación de Azure Active Directory para .NET Framework y .NET Core.

Establezca un nombre de usuario mediante la propiedad de cadena de conexión **User ID** o **UID**:

```
"Server=<server name>; Database=<db name>; Authentication=Active Directory Interactive; User Id=<username>;"
```

#### <a name="order-hints-for-sqlbulkcopy"></a>Sugerencias de orden para SqlBulkCopy

Se pueden proporcionar sugerencias de orden para mejorar el rendimiento de las operaciones de copia masiva en tablas con índices agrupados. Para obtener más información, vea la sección [Operaciones de copia masiva](sql/bulk-copy-order-hints.md).

#### <a name="sni-dependency-changes"></a>Cambios de dependencia de SNI

Microsoft.Data.SqlClient (.NET Core y .NET Standard) en Windows ahora depende de **Microsoft.Data.SqlClient.SNI.runtime**, reemplazando la dependencia anterior en **runtime.native.System.Data.SqlClient.SNI**. La dependencia nueva agrega compatibilidad con la plataforma ARM junto con las plataformas ya admitidas ARM64, x64 y x86 en Windows.

## <a name="release-notes-for-microsoftdatasqlclient-110"></a>Notas de la versión de Microsoft.Data.SqlClient 1.1.0

Las notas de la versión también están disponibles en el repositorio de GitHub: [Notas de la versión 1.1](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

### <a name="new-features"></a>Nuevas características

#### <a name="always-encrypted-with-secure-enclaves"></a>Always Encrypted con enclaves seguros

Always Encrypted está disponible a partir de Microsoft SQL Server 2016. Los enclaves seguros están disponibles a partir de Microsoft SQL Server 2019. Para usar la característica de enclaves, las cadenas de conexión deben incluir el protocolo de atestación y la dirección URL de atestación necesarios. Por ejemplo:

```
Attestation Protocol=HGS;Enclave Attestation Url=<attestation_url_for_HGS>
```

Para más información, consulte:

- [Compatibilidad de SqlClient para Always Encrypted](sql/sqlclient-support-always-encrypted.md)
- [Tutorial: Desarrollo de una aplicación de .NET mediante Always Encrypted con enclaves seguros](sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)

## <a name="release-notes-for-microsoftdatasqlclient-10"></a>Notas de la versión de Microsoft.Data.SqlClient 1.0

La versión inicial del espacio de nombres Microsoft.Data.SqlClient ofrece funcionalidades adicionales del espacio de nombres System.Data.SqlClient existente.
Las notas de la versión también están disponibles en el repositorio de GitHub: [Notas de la versión 1.0](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.0).

### <a name="new-features"></a>Nuevas características

#### <a name="new-features-over-net-framework-472-systemdatasqlclient"></a>Nuevas características de System.Data.SqlClient en .NET Framework 4.7.2

- **Clasificación de datos**: disponible en Azure SQL Database y Microsoft SQL Server 2019.

- **Compatibilidad con UTF-8**: disponible en Microsoft SQL Server 2019.

#### <a name="new-features-over-net-core-22-systemdatasqlclient"></a>Nuevas características de System.Data.SqlClient en .NET Core 2.2

- **Clasificación de datos**: disponible en Azure SQL Database y Microsoft SQL Server 2019.

- **Compatibilidad con UTF-8**: disponible en Microsoft SQL Server 2019.

- **Autenticación**: modo de autenticación de contraseña de Active Directory.

### <a name="data-classification"></a>Clasificación de datos

La clasificación de datos proporciona un nuevo conjunto de API que exponen información de clasificación y confidencialidad de los datos de solo lectura de los objetos recuperados a través de SqlDataReader cuando el origen subyacente admite la característica y contiene metadatos de [confidencialidad y clasificación de datos](../../relational-databases/security/sql-data-discovery-and-classification.md). Vea la aplicación de ejemplo en [Detección y clasificación de datos en SqlClient](https://github.com/dotnet/SqlClient/tree/master/release-notes/1.1).

```csharp
public class SqlDataReader
{
    public Microsoft.Data.SqlClient.DataClassification.SensitivityClassification SensitivityClassification
}

namespace Microsoft.Data.SqlClient.DataClassification
{
    public class ColumnSensitivity
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.SensitivityProperty> SensitivityProperties
    }
    public class InformationType
    {
        public string Id
        public string Name
    }
    public class Label
    {
        public string Id
        public string Name
    }
    public class SensitivityClassification
    {
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.ColumnSensitivity> ColumnSensitivities
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.InformationType> InformationTypes
        public System.Collections.ObjectModel.ReadOnlyCollection<Microsoft.Data.SqlClient.DataClassification.Label> Labels
    }
    public class SensitivityProperty
    {
        public Microsoft.Data.SqlClient.DataClassification.InformationType InformationType
        public Microsoft.Data.SqlClient.DataClassification.Label Label
    }
}
```

### <a name="utf-8-support"></a>Compatibilidad con UTF-8

La compatibilidad con UTF-8 no requiere hacer ningún cambio en el código de la aplicación. Estos cambios de SqlClient optimizan la comunicación entre el cliente y el servidor cuando el servidor admite UTF-8 y la intercalación de columna subyacente es UTF-8. Consulte la sección UTF-8 en [Novedades de SQL Server 2019](../../sql-server/what-s-new-in-sql-server-ver15.md).

### <a name="always-encrypted-with-enclaves"></a>Always Encrypted con enclaves seguros

En general, la documentación existente que usa System.Data.SqlClient en .NET Framework **y los proveedores de almacenamiento de claves maestras de columna integradas** ahora deben trabajar también con .NET Core.

 [Desarrollar con Always Encrypted con el proveedor de datos .NET Framework](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)

 [Always Encrypted: Protección de datos confidenciales y almacenamiento de claves de cifrado en el almacén de certificados de Windows](/azure/sql-database/sql-database-always-encrypted)

### <a name="authentication"></a>Authentication

Se pueden especificar diferentes modos de autenticación mediante la opción de cadena de conexión _Autenticación_. Para obtener más información, consulte la [documentación de SqlAuthenticationMethod](/dotnet/api/system.data.sqlclient.sqlauthenticationmethod?view=netframework-4.7.2&preserve-view=true).

> [!NOTE]
> Los proveedores de almacén de claves personalizados, como el proveedor de Azure Key Vault, deberán actualizarse para admitir Microsoft.Data.SqlClient. Del mismo modo, también será necesario actualizar los proveedores de enclaves para que sean compatibles con Microsoft.Data.SqlClient.
> Always Encrypted solo se admite en los destinos de .NET Framework y .NET Core. No se admite con .NET Standard ya que en .NET Standard faltan algunas dependencias de cifrado.