---
title: Generadores de cadenas de conexión
description: Obtenga información sobre las clases del generador de cadenas de conexión usadas para diferentes proveedores en ADO.NET, todas las cuales heredan de DbConnectionStringBuilder.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 8434b608-c4d3-43d3-8ae3-6d8c6b726759
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 38c1ad34fb4819460c08f00da4c1dbd31749b7cb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771441"
---
# <a name="connection-string-builders"></a>Generadores de cadenas de conexión

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En versiones anteriores de ADO.NET, no se producía la comprobación de las cadenas de conexión con valores de cadena concatenados en tiempo de compilación, por lo que, en tiempo de ejecución, una palabra clave incorrecta generaba una excepción <xref:System.ArgumentException>. El proveedor de datos SqlClient de Microsoft para SQL Server incluye la clase del generador de cadenas de conexión con establecimiento inflexible de tipos <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder?displayProperty=nameWithType> que hereda de <xref:System.Data.Common.DbConnectionStringBuilder>.

## <a name="connection-string-injection-attacks"></a>Ataques de inyección de cadenas de conexión

Cuando se utiliza la concatenación dinámica de cadenas para generar cadenas de conexión basadas en datos introducidos por el usuario, se pueden producir ataques de inyección de cadenas de conexión. Si no se valida la cadena y no se crean secuencias de escape para los caracteres o el texto malintencionado, los atacantes pueden tener acceso a datos confidenciales y a otros recursos del servidor. Por ejemplo, un atacante puede realizar un ataque si proporciona un punto y coma y anexa un valor adicional. La cadena de conexión se analiza mediante un algoritmo "**el último gana**" y la entrada hostil se sustituye por un valor legítimo.

Las clases compiladoras de cadenas de conexión se han diseñado para eliminar la adivinación y desarrollar protección ante errores de sintaxis y vulnerabilidades de seguridad. Proporcionan métodos y propiedades que corresponden a los pares clave-valor conocidos que permite el proveedor de datos. Cada clase mantiene una colección fija de sinónimos y puede convertir un sinónimo al correspondiente nombre de clave conocido. En los pares clave-valor se realizan comprobaciones y los pares no válidos inician una excepción. Además, los valores inyectados se controlan de forma segura.

En el ejemplo siguiente se muestra cómo <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> controla un valor adicional insertado en la configuración `Initial Catalog`.

[!code-csharp[SqlConnectionStringBuilder_InjectionAttack#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_InjectionAttack.cs#1)]

El resultado muestra que <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> lo controló correctamente mediante el escape del valor adicional entre comillas dobles en lugar de su anexión a la cadena de conexión como un nuevo par clave-valor.

```output
data source=(local);Integrated Security=True;
initial catalog="AdventureWorks;NewValue=Bad"
```

## <a name="build-connection-strings-from-configuration-files"></a>Creación de cadenas de conexión a partir de archivos de configuración

Si determinados elementos de una cadena de conexión se conocen de antemano, se pueden almacenar en un archivo de configuración y recuperar en tiempo de ejecución para construir una cadena de conexión completa. Por ejemplo, se puede conocer por adelantado el nombre de la base de datos, pero no el del servidor. También es posible que desee que un usuario indique un nombre y una contraseña en tiempo de ejecución sin que pueda inyectar otros valores en ella.

Uno de los constructores sobrecargados de un compilador de cadenas de conexión toma <xref:System.String> como argumento, lo que permite proporcionar una cadena de conexión parcial que se puede completar después con los datos introducidos por el usuario. La cadena de conexión parcial se puede almacenar en un archivo de configuración y recuperarse en tiempo de ejecución.

> [!NOTE]
> El espacio de nombres <xref:System.Configuration> permite el acceso mediante programación a archivos de configuración que usan <xref:System.Web.Configuration.WebConfigurationManager> en aplicaciones web y <xref:System.Configuration.ConfigurationManager> en aplicaciones Windows. Para obtener más información sobre cómo trabajar con cadenas de conexión y archivos de configuración, vea [Cadenas de conexión y archivos de configuración](connection-strings-and-configuration-files.md).

### <a name="example"></a>Ejemplo

En este ejemplo se muestra la recuperación de una cadena de conexión incluida en un archivo de configuración y cómo se completa mediante el establecimiento de las propiedades <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.DataSource%2A>, <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.UserID%2A> y <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> de <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. El archivo de configuración se define de la siguiente forma.

```xml
<connectionStrings>
  <clear/>
  <add name="partialConnectString"
    connectionString="Initial Catalog=Northwind;"
    providerName="Microsoft.Data.SqlClient" />
</connectionStrings>
```

> [!NOTE]
> Para ejecutar el código, debe establecer una referencia al archivo `System.Configuration.dll` del proyecto.

[!code-csharp[DataWorks SqlConnectionStringBuilder.UserNamePwd#1](~/../sqlclient/doc/samples/SqlConnectionStringBuilder_UserNamePwd.cs#1)]
  
## <a name="see-also"></a>Vea también

- [Cadenas de conexión](connection-strings.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
