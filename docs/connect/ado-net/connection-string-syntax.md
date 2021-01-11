---
title: Sintaxis de cadenas de conexión
description: Obtenga información sobre la sintaxis de las cadenas de conexión en el proveedor de datos SqlClient de Microsoft para SQL Server. La sintaxis de cada proveedor se documenta en la propiedad ConnectionString.
ms.date: 11/13/2020
ms.assetid: 0977aeee-04d1-4cce-bbed-750c77fce06e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 42b1f8704be721c0b52b3c42946c9abbf7efd7db
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771638"
---
# <a name="connection-string-syntax"></a>Sintaxis de cadenas de conexión

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient> tiene un objeto `Connection` que hereda de <xref:System.Data.Common.DbConnection>, así como una propiedad <xref:System.Data.Common.DbConnection.ConnectionString%2A> específica del proveedor. La sintaxis de la cadena de conexión específica del proveedor SqlClient se documenta en su propiedad `ConnectionString`. Para obtener más información acerca de la sintaxis de la cadena de conexión, vea <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A>.

## <a name="connection-string-builders"></a>Generadores de cadenas de conexión

 El proveedor de datos SqlClient de Microsoft para SQL Server incluyó por primera vez el siguiente generador de cadenas de conexión.

- <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>

Los compiladores de cadenas de conexión permiten crear cadenas de conexión sintácticamente válidas en tiempo de ejecución. Por tanto, el usuario no tiene que concatenar manualmente los valores de las cadenas de conexión del código. Para obtener más información, vea [Generadores de cadenas de conexión](connection-string-builders.md).

## <a name="windows-authentication"></a>Autenticación de Windows

Recomendamos usar la autenticación de Windows (en ocasiones denominada *seguridad integrada*) para conectarse a orígenes de datos que sean compatibles. En la tabla siguiente se muestra la sintaxis de autenticación de Windows que se usa con el proveedor de datos SqlClient de Microsoft para SQL Server.

|Proveedor|Syntax|  
|--------------|------------|  
|`SqlClient`|`Integrated Security=true;`<br /><br /> `-- or --`<br /><br /> `Integrated Security=SSPI;`|  

## <a name="sqlclient-connection-strings"></a>Cadenas de conexión de SqlClient

La sintaxis de una cadena de conexión de <xref:Microsoft.Data.SqlClient.SqlConnection> se documenta en la propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>. Puede usar la propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> para obtener o establecer una cadena de conexión para una base de datos de SQL Server. Las palabras clave de la cadena de conexión también se asignan a las propiedades en <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

> [!IMPORTANT]
> La configuración predeterminada de la palabra clave `Persist Security Info` es `false`. Si se establece en `true` o `yes`, permite obtener información de seguridad confidencial de la conexión, incluidos el identificador de usuario y la contraseña, una vez abierta la conexión. Mantenga el valor `Persist Security Info` establecido en `false` para garantizar que los orígenes que no sean de confianza no puedan tener acceso a información confidencial de la cadena de conexión.

### <a name="windows-authentication-with-sqlclient"></a>Autenticación de Windows con SqlClient

En cada una de las siguientes formas de sintaxis se utiliza autenticación de Windows para conectarse a la base de datos **AdventureWorks** en un servidor local.

```csharp  
"Persist Security Info=False;Integrated Security=true;  
    Initial Catalog=AdventureWorks;Server=MSSQL1"  
"Persist Security Info=False;Integrated Security=SSPI;  
    database=AdventureWorks;server=(local)"  
"Persist Security Info=False;Trusted_Connection=True;  
    database=AdventureWorks;server=(local)"  
```  

### <a name="sql-server-authentication-with-sqlclient"></a>Autenticación de SQL Server con SqlClient

Es preferible utilizar la autenticación de Windows para conectarse a SQL Server. No obstante, si se requiere autenticación de SQL Server, deberá utilizar la siguiente sintaxis para especificar un nombre de usuario y una contraseña. En este ejemplo, los asteriscos representan un nombre de usuario y una contraseña válidos.

```csharp  
"Persist Security Info=False;User ID=*****;Password=*****;Initial Catalog=AdventureWorks;Server=MySqlServer"  
```  

Cuando se conecte a Azure SQL Database o a Azure Synapse Analytics y proporcione un inicio de sesión con el formato `user@servername`, asegúrese de que el valor `servername` del inicio de sesión coincida con el valor proporcionado para `Server=`.

> [!NOTE]
> La autenticación de Windows tiene prioridad sobre los inicios de sesión de SQL Server. Si especifica Integrated Security=true junto con un nombre de usuario y una contraseña, se ignorarán el nombre de usuario y la contraseña, y se usará la autenticación de Windows.

### <a name="connect-to-a-named-instance-of-sql-server"></a>Conexión a una instancia con nombre de SQL Server

Para conectarse a una instancia con nombre de SQL Server, utilice la sintaxis *nombre de servidor\nombre de instancia*.

```csharp  
"Data Source=MySqlServer\MSSQL1;"  
```  

A la hora de crear una cadena de conexión, también puede establecer la propiedad <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A> del `SqlConnectionStringBuilder` en el nombre de instancia. La propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.DataSource%2A> de un objeto <xref:Microsoft.Data.SqlClient.SqlConnection> es de solo lectura.

### <a name="type-system-version-changes"></a>Cambios en la versión de sistema de tipo

La palabra clave `Type System Version` en un objeto <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> especifica la representación del lado cliente de los tipos de SQL Server. Para obtener más información sobre la palabra clave <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>, vea `Type System Version`.

## <a name="connect-and-attach-to-sql-server-express-user-instances"></a>Conexión y asociación a instancias de usuario de SQL Server Express

Las instancias de usuario son una característica de SQL Server Express. Permiten que un usuario que ejecuta una cuenta de Windows local y con privilegios mínimos adjunte y ejecute una base de datos de SQL Server sin necesidad de tener privilegios administrativos. Una instancia de usuario se ejecuta con las credenciales de Windows del usuario, no como un servicio.

Para obtener más información sobre cómo trabajar con instancias de usuario, vea [Instancias de usuario de SQL Server Express](./sql/sql-server-express-user-instances.md).

## <a name="use-trustservercertificate"></a>Use TrustServerCertificate

La palabra clave `TrustServerCertificate` solo es válida al conectar a una instancia de SQL Server con un certificado válido. Cuando `TrustServerCertificate` se establece en `true`, la capa de transporte usará SSL para cifrar el canal y omitir el paso de la cadena de certificados para validar la confianza.

```csharp  
"TrustServerCertificate=true;"
```  

> [!NOTE]
> Si `TrustServerCertificate` se establece en `true` y se activa el cifrado, se utilizará el nivel de cifrado especificado en el servidor aunque `Encrypt` esté establecido en `false` en la cadena de conexión. De lo contrario, se producirá un error en la conexión.

### <a name="enable-encryption"></a>Habilitación del cifrado

Si desea habilitar el cifrado porque no se ha incluido un certificado en el servidor, deben estar habilitadas las opciones **Forzar cifrado de protocolo** y **Confiar en certificado de servidor** en el Administrador de configuración de SQL Server. En ese caso, el cifrado utilizará un certificado de servidor autofirmado sin validación si no se ha proporcionado ningún certificado comprobable en el servidor.

La configuración de las aplicaciones no puede reducir el nivel de seguridad configurado en SQL Server, sino que en todo caso puede reforzarlo. Una aplicación puede solicitar cifrado estableciendo las palabras clave `TrustServerCertificate` y `Encrypt` en `true`, con lo que se garantiza el cifrado aunque no se haya proporcionado un certificado de servidor y no se haya configurado **Forzar cifrado de protocolo para el cliente**. Sin embargo, si en la configuración del cliente no está habilitado `TrustServerCertificate`, seguirá siendo necesario un certificado de servidor.

En la siguiente tabla se describen todos los casos.

|Cliente configurado con Forzar cifrado de protocolo|Cliente configurado con Confiar en certificado de servidor|Cadena de conexión/atributo Cifrar/Utilizar cifrado para los datos|Cadena de conexión/atributo Confiar en certificado de servidor|Resultado|  
|----------------------------------------------|---------------------------------------------|-------------------------------------------------------------------|-----------------------------------------------------------|------------|  
|No|N/D|No (valor predeterminado)|Omitido|No se produce el cifrado.|  
|No|N/D|Sí|No (valor predeterminado)|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|No|N/D|Sí|Sí|El cifrado se produce siempre, pero puede que se utilice un certificado de servidor autofirmado.|  
|Sí|No|Omitido|Omitido|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|Sí|Sí|No (valor predeterminado)|Omitido|El cifrado se produce siempre, pero puede que se utilice un certificado de servidor autofirmado.|  
|Sí|Sí|Sí|No (valor predeterminado)|El cifrado solamente se produce si hay un certificado de servidor comprobable; de lo contrario, se produce un error en el intento de conexión.|  
|Sí|Sí|Sí|Sí|El cifrado se produce siempre, pero puede que se utilice un certificado de servidor autofirmado.|  

Para obtener más información, vea [Usar el cifrado sin validación](/sql/relational-databases/native-client/features/using-encryption-without-validation).

## <a name="see-also"></a>Vea también

- [Cadenas de conexión](connection-strings.md)
- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
