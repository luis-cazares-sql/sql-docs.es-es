---
title: Cadenas de conexión y archivos de configuración
description: Obtenga información sobre cómo almacenar cadenas de conexión para las aplicaciones de ADO.NET en un archivo de configuración de la aplicación, como procedimiento recomendado para la seguridad y el mantenimiento.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 37df2641-661e-407a-a3fb-7bf9540f01e8
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8f1e0ac5fc52576f0e47ccb8be4b8e90899c6e68
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771467"
---
# <a name="connection-strings-and-configuration-files"></a>Cadenas de conexión y archivos de configuración

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La incrustación de cadenas de conexión en el código de la aplicación puede producir vulnerabilidades en la seguridad y problemas de mantenimiento. Las cadenas de conexión sin cifrar compiladas en el código fuente de una aplicación se pueden ver con la herramienta [Ildasm.exe (Desensamblador de IL)](/dotnet/framework/tools/ildasm-exe-il-disassembler). Además, si la cadena de conexión cambia en algún momento, será necesario compilar de nuevo la aplicación. Por estas razones, se recomienda almacenar las cadenas de conexión en un archivo de configuración de la aplicación.

## <a name="work-with-application-configuration-files"></a>Trabajo con archivos de configuración de la aplicación

Los archivos de configuración de la aplicación contienen valores específicos de una aplicación determinada. Por ejemplo, una aplicación ASP.NET puede tener uno o varios archivos **web.config** y una aplicación Windows puede tener un archivo **app.config** opcional. Los archivos de configuración comparten elementos comunes, aunque su nombre y ubicación varían en función del host de la aplicación.

### <a name="the-connectionstrings-section"></a>La sección connectionStrings

Las cadenas de conexión se pueden almacenar como pares clave-valor en la sección **connectionStrings** del elemento **configuration** en el archivo de configuración de una aplicación. Los elementos secundarios incluyen **add**, **clear** y **remove**.

El siguiente fragmento del archivo de configuración muestra el esquema y la sintaxis para almacenar una cadena de conexión. El atributo **name** es un nombre que se proporciona para identificar de forma única una cadena de conexión, de forma que se pueda recuperar en tiempo de ejecución. El valor **providerName** es **Microsoft.Data.SqlClient** con el proveedor de datos SqlClient de Microsoft para SQL Server.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
  <configuration>  
    <connectionStrings>  
      <clear />  
      <add name="Name"
       providerName="Microsoft.Data.SqlClient"
       connectionString="Valid Connection String;" />  
    </connectionStrings>  
  </configuration>  
```  

> [!NOTE]
> Puede guardar parte de la cadena de conexión en un archivo de configuración y usar la clase <xref:System.Data.Common.DbConnectionStringBuilder> para completarla en tiempo de ejecución. Esto resulta útil en escenarios en los que no se conocen los elementos de la cadena de conexión por anticipado o cuando no desea guardar información confidencial en un archivo de configuración. Para obtener más información, vea [Generadores de cadenas de conexión](connection-string-builders.md).

### <a name="use-external-configuration-files"></a>Uso de archivos de configuración externa

Los archivos de configuración externos son archivos independientes que contienen un fragmento de un archivo de configuración compuesto de una sola sección. El archivo de configuración principal hace referencia al archivo de configuración externo. El almacenamiento de la sección **connectionStrings** en un archivo físicamente independiente resulta útil en situaciones en las que es posible que se editen las cadenas de conexión después de implementar la aplicación. Por ejemplo, si se modifican los archivos de configuración, ASP.NET reinicia de forma predeterminada el dominio de la aplicación, lo que provoca la pérdida de la información de estado. Sin embargo, la modificación de un archivo de configuración externo no provoca el reinicio de la aplicación. Los archivos de configuración externos no se limitan a ASP.NET; también se pueden utilizar en aplicaciones Windows. Además, la seguridad y permisos de acceso a los archivos se pueden usar para restringir el acceso a los archivos de configuración externos. El trabajo con archivos de configuración externos en tiempo de ejecución es transparente y no requiere código especial.

Para almacenar las cadenas de conexión en un archivo de configuración externo, cree un archivo independiente que contenga únicamente la sección **connectionStrings**. No incluya elementos, secciones ni atributos adicionales. En este ejemplo se muestra la sintaxis de un archivo de configuración externo.

```xml  
<connectionStrings>  
  <add name="Name"
   providerName="Microsoft.Data.SqlClient"
   connectionString="Valid Connection String;" />  
</connectionStrings>  
```  

 En el archivo de configuración principal de la aplicación, use el atributo **configSource** para especificar el nombre completo y la ubicación del archivo externo. En este ejemplo se hace referencia a un archivo de configuración externo denominado `connections.config`.

```xml  
<?xml version='1.0' encoding='utf-8'?>  
<configuration>  
    <connectionStrings configSource="connections.config"/>  
</configuration>  
```  

## <a name="retrieving-connection-strings-at-run-time"></a>Recuperar cadenas de conexión en tiempo de ejecución

.NET Framework 2.0 incorpora nuevas clases en el espacio de nombres <xref:System.Configuration> para simplificar la recuperación de las cadenas de conexión de los archivos de configuración en tiempo de ejecución. La cadena de conexión se puede recuperar mediante programación con el nombre de la cadena o el nombre de proveedor.

> [!NOTE]
> El archivo **machine.config** también contiene una sección **connectionStrings**, donde se encuentran las cadenas de conexión que usa Visual Studio. Al recuperar cadenas de conexión mediante el nombre del proveedor del archivo **app.config** en una aplicación Windows, primero se cargan las cadenas de conexión del archivo **machine.config** y, a continuación, las entradas de **app.config**. Si se agrega **clear** inmediatamente después del elemento **connectionStrings**, se quitarán todas las referencias heredadas de la estructura de datos en memoria, de forma que solo se tendrán en cuenta las cadenas de conexión definidas en el archivo **app.config** local.

### <a name="work-with-the-configuration-files"></a>Trabajo con los archivos de configuración

A partir de .NET Framework 2.0, se usa el elemento <xref:System.Configuration.ConfigurationManager> al trabajar con archivos de configuración en el equipo local, reemplazando al elemento en desuso <xref:System.Configuration.ConfigurationSettings>. <xref:System.Web.Configuration.WebConfigurationManager> se usa para trabajar con archivos de configuración de ASP.NET. Esta característica se ha diseñado para trabajar con archivos de configuración en un servidor web y permite el acceso mediante programación a secciones del archivo de configuración como **system.web**.

> [!NOTE]
> El acceso a los archivos de configuración en tiempo de ejecución requiere la concesión de permisos al llamador; los permisos necesarios dependen del tipo de aplicación, del archivo de configuración y de la ubicación. Para obtener más información, vea [Utilizar las clases Configuration](/previous-versions/aspnet/ms228063(v=vs.100)) y <xref:System.Web.Configuration.WebConfigurationManager> para las aplicaciones ASP.NET, o bien <xref:System.Configuration.ConfigurationManager> para las aplicaciones Windows.

Puede usar <xref:System.Configuration.ConnectionStringSettingsCollection> para recuperar cadenas de conexión de archivos de configuración de aplicación. Esta clase contiene una colección de objetos <xref:System.Configuration.ConnectionStringSettings>, cada uno de los cuales representa una única entrada en la sección **connectionStrings**. Sus propiedades se asignan a los atributos de cadenas de conexión, lo que permite recuperar una cadena de conexión mediante la especificación de su nombre o del nombre del proveedor.

|Propiedad|Descripción|  
|--------------|-----------------|  
|<xref:System.Configuration.ConnectionStringSettings.Name%2A>|Nombre de la cadena de conexión. Se asigna al atributo **name**.|  
|<xref:System.Configuration.ConnectionStringSettings.ProviderName%2A>|Nombre completo del proveedor. Se asigna al atributo **providerName**.|  
|<xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A>|La cadena de conexión. Se asigna al atributo **connectionString**.|  

### <a name="example-listing-all-connection-strings"></a>Ejemplo: Enumeración de todas las cadenas de conexión

Este ejemplo recorre en iteración <xref:System.Configuration.ConnectionStringSettingsCollection> y muestra las propiedades <xref:System.Configuration.ConnectionStringSettings.Name%2A?displayProperty=nameWithType>, <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A?displayProperty=nameWithType> y <xref:System.Configuration.ConnectionStringSettings.ConnectionString%2A?displayProperty=nameWithType> en la ventana de la consola.

> [!NOTE]
> System.Configuration.dll no se incluye en todos los tipos de proyectos y es posible que deba establecer una referencia a este elemento para usar las clases de configuración. El nombre y la ubicación de un archivo de configuración de aplicación determinado varían en función del tipo de aplicación y del proceso de hospedaje.

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfig#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfig.cs#1)]

### <a name="example-retrieving-a-connection-string-by-name"></a>Ejemplo: Recuperación de una cadena de conexión por nombre

El siguiente ejemplo muestra cómo recuperar una cadena de conexión de un archivo de configuración mediante la especificación del nombre. El código crea un objeto <xref:System.Configuration.ConnectionStringSettings>, de forma que el parámetro de entrada proporcionado coincida con el nombre de <xref:System.Configuration.ConfigurationManager.ConnectionStrings%2A>. Si no se encuentra una coincidencia de nombre, la función devuelve `null` (`Nothing` en Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByName#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByName.cs#1)]

### <a name="example-retrieving-a-connection-string-by-provider-name"></a>Ejemplo: Recuperación de una cadena de conexión por nombre de proveedor

En este ejemplo se muestra cómo recuperar una cadena de conexión mediante la especificación del nombre de proveedor con el formato *Microsoft.Data.SqlClient*. El código recorre en iteración <xref:System.Configuration.ConnectionStringSettingsCollection> y devuelve la cadena de conexión del primer valor de <xref:System.Configuration.ConnectionStringSettings.ProviderName%2A> encontrado. Si no se encuentra el nombre del proveedor, la función devuelve `null` (`Nothing` en Visual Basic).

[!code-csharp[DataWorks ConnectionStringSettings.RetrieveFromConfigByProvider#1](~/../SqlClient/doc/samples/ConnectionStringSettings_RetrieveFromConfigByProvider.cs#1)]

## <a name="encrypt-configuration-file-sections-using-protected-configuration"></a>Cifrado de secciones del archivo de configuración mediante la configuración protegida

En ASP.NET 2.0 se incorporó una característica nueva denominada *configuración protegida*, que permite cifrar la información confidencial en un archivo de configuración. Si bien se ha diseñado principalmente para ASP.NET, la configuración protegida también se puede usar para cifrar secciones del archivo de configuración en aplicaciones Windows. Para obtener una descripción detallada de las funciones de configuración protegida, vea [Cifrar información de configuración mediante una configuración protegida](/previous-versions/aspnet/53tyfkaw(v=vs.100)).

En el fragmento de archivo de configuración siguiente se muestra la sección **connectionStrings** después de haberse cifrado. En la sección **configProtectionProvider** se especifica el proveedor de configuración protegida que se usa para cifrar y descifrar las cadenas de conexión. En la sección **EncryptedData** se incluye el texto cifrado.

```xml  
<connectionStrings configProtectionProvider="DataProtectionConfigurationProvider">  
  <EncryptedData>  
    <CipherData>  
      <CipherValue>AQAAANCMnd8BFdERjHoAwE/Cl+sBAAAAH2... </CipherValue>  
    </CipherData>  
  </EncryptedData>  
</connectionStrings>  
```  

Cuando se recupera la cadena de conexión cifrada en tiempo de ejecución, .NET Framework usa el proveedor especificado para descifrar **CipherValue** y que así esté disponible para la aplicación. No es necesario escribir ningún código adicional para administrar el proceso de descifrado.

### <a name="protected-configuration-providers"></a>Proveedores de configuración protegida

Los proveedores de configuración protegida se registran en la sección **configProtectedData** del archivo **machine.config** en el equipo local, como se muestra en el fragmento siguiente, donde se pueden ver los dos proveedores de configuración protegida que proporciona .NET Framework. Los valores que se muestran se han truncado para facilitar la lectura.

```xml  
<configProtectedData defaultProvider="RsaProtectedConfigurationProvider">  
  <providers>  
    <add name="RsaProtectedConfigurationProvider"
      type="System.Configuration.RsaProtectedConfigurationProvider" />  
    <add name="DataProtectionConfigurationProvider"
      type="System.Configuration.DpapiProtectedConfigurationProvider" />  
  </providers>  
</configProtectedData>  
```  

Puede configurar otros proveedores de configuración protegida si los agrega al archivo **machine.config**. Asimismo, puede crear su propio proveedor de configuración protegida heredando de la clase base abstracta <xref:System.Configuration.ProtectedConfigurationProvider>. En la tabla siguiente se describen los dos archivos de configuración incluidos en .NET Framework.

|Proveedor|Descripción|  
|--------------|-----------------|  
|<xref:System.Configuration.RsaProtectedConfigurationProvider>|Usa el algoritmo de cifrado RSA para cifrar y descifrar datos. Los algoritmos RSA se pueden usar para el cifrado de clave pública y para firmas digitales. También se conoce como cifrado de "clave pública" o asimétrico, ya que usa dos claves diferentes. Puede usar la [Herramienta de registro de IIS en ASP.NET (aspnet_regiis.exe)](/previous-versions/dotnet/netframework-3.5/k6h9cz8h(v=vs.90)) para cifrar secciones de un archivo Web.config y administrar las claves de cifrado. ASP.NET descifra el archivo de configuración cuando lo procesa. La identidad de la aplicación ASP.NET debe tener acceso de lectura a la clave de cifrado utilizada para cifrar y descifrar las secciones cifradas.|  
|<xref:System.Configuration.DpapiProtectedConfigurationProvider>|Usa la API de protección de datos (DPAPI) de Windows para cifrar y descifrar las secciones de configuración. Usa los servicios criptográficos integrados de Windows y se puede configurar para la protección específica de equipo o para la protección específica de cuenta de usuario. La protección específica de equipo resulta útil cuando varias aplicaciones del mismo servidor deben compartir información. La protección específica de cuenta de usuario se puede utilizar para los servicios que se ejecutan con una identidad de usuario concreta, como un entorno de hospedaje compartido. Cada aplicación se ejecuta en una identidad independiente, lo que restringe el acceso a recursos como archivos y bases de datos.|

Ambos proveedores proporcionan cifrado de datos de alta seguridad. No obstante, si prevé usar el mismo archivo de configuración de cifrado en varios servidores como, por ejemplo, una granja de servidores web, solo <xref:System.Configuration.RsaProtectedConfigurationProvider> permite exportar las claves de cifrado usadas para cifrar los datos e importarlas a otro servidor. Para obtener más información, vea [Importar y exportar contenedores de claves RSA con configuración protegida](/previous-versions/aspnet/yxw286t2(v=vs.100)).

### <a name="use-the-configuration-classes"></a>Uso de las clases de configuración

El espacio de nombres <xref:System.Configuration> proporciona clases para trabajar con valores de configuración mediante programación. La clase <xref:System.Configuration.ConfigurationManager> proporciona acceso a los archivos de configuración de equipo, aplicación y usuario. Si va a crear una aplicación ASP.NET, puede usar la clase <xref:System.Web.Configuration.WebConfigurationManager>, que proporciona las mismas funciones a la vez que permite tener acceso a configuración única de las aplicaciones ASP.NET, como la que se encuentra en **\<system.web>** .

> [!NOTE]
> El espacio de nombres <xref:System.Security.Cryptography> contiene clases que proporcionan opciones adicionales para cifrar y descifrar datos. Use estas clases si requiere servicios criptográficos que no están disponibles cuando se usa la configuración protegida. Algunas de estas clases son contenedores de Microsoft CryptoAPI no administrado, mientras que otras son simplemente implementaciones administradas. Para más información, vea [Servicios criptográficos](/previous-versions/visualstudio/visual-studio-2008/93bskf9z(v=vs.90)).

### <a name="appconfig-example"></a>Ejemplo de app.config

En este ejemplo se muestra cómo alternar el cifrado de la sección **connectionStrings** de un archivo **app.config** para una aplicación Windows. En este ejemplo, el procedimiento recibe el nombre de la aplicación como argumento, por ejemplo, "MyApplication.exe". Después, el archivo **app.config** se cifra y se copia en la carpeta que contiene el ejecutable con el nombre "MyApplication.exe.config".

> [!NOTE]
> La cadena de conexión solo se puede descifrar en el equipo donde se ha cifrado.

El código usa el método <xref:System.Configuration.ConfigurationManager.OpenExeConfiguration%2A> para abrir el archivo **app.config** y editarlo; el método <xref:System.Configuration.ConfigurationManager.GetSection%2A> devuelve la sección **connectionStrings**. A continuación, el código comprueba la propiedad <xref:System.Configuration.SectionInformation.IsProtected%2A> y llama a <xref:System.Configuration.SectionInformation.ProtectSection%2A> para cifrar la sección si no está cifrada. Para descifrar la sección se llama al método <xref:System.Configuration.SectionInformation.UnprotectSection%2A>. El método <xref:System.Configuration.Configuration.Save%2A> completa la operación y guarda los cambios.

> [!NOTE]
> Para ejecutar el código, debe establecer una referencia al archivo `System.Configuration.dll` del proyecto.

[!code-csharp[DataWorks ConnectionStrings.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStrings_Encrypt.cs#1)]

### <a name="webconfig-example"></a>Ejemplo de Web.config

Este ejemplo usa el método <xref:System.Web.Configuration.WebConfigurationManager.OpenWebConfiguration%2A> de `WebConfigurationManager`. Observe que en este caso puede indicar la ruta de acceso relativa al archivo **Web.config** mediante una tilde. El código requiere una referencia a la clase `System.Web.Configuration`.  
  
[!code-csharp[DataWorks ConnectionStringsWeb.Encrypt#1](~/../SqlClient/doc/samples/ConnectionStringsWeb_Encrypt.cs#1)]

Para obtener más información sobre cómo proteger las aplicaciones de ASP.NET, vea [Protección de sitios web ASP.NET](/previous-versions/aspnet/91f66yxt(v=vs.100)).

## <a name="see-also"></a>Vea también

- [Generadores de cadenas de conexión](connection-string-builders.md)
- [Protección de la información de conexión](protecting-connection-information.md)
- [Uso de las clases de configuración](/previous-versions/visualstudio/visual-studio-2008/ms228063(v=vs.90))
- [Configuración de aplicaciones](/dotnet/framework/configure-apps/index)
- [Administración de sitios web de ASP.NET](/previous-versions/aspnet/6hy1xzbw(v=vs.100))
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
