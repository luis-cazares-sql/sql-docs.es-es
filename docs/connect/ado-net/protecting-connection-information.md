---
title: Protección de la información de conexión
description: Obtenga información sobre las vulnerabilidades de seguridad en las cadenas de conexión, que pueden surgir debido a cómo se construyen y conservan las cadenas de conexión y el tipo de autenticación.
ms.date: 11/13/2020
ms.assetid: 1471f580-bcd4-4046-bdaf-d2541ecda2f4
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1d170d712269bf169d069ef7b93f975de855f8ec
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771417"
---
# <a name="protecting-connection-information"></a>Protección de la información de conexión

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La protección del acceso al origen de datos es uno de los objetivos más importantes a la hora de proteger una aplicación. Las cadenas de conexión presentan una posible vulnerabilidad si no se protegen. El almacenamiento de la información de conexión en texto sin formato o en la memoria ponen en riesgo el sistema completo. Las cadenas de conexión insertadas en el código fuente se pueden leer con [Ildasm.exe (IL Disassembler)](/dotnet/framework/tools/ildasm-exe-il-disassembler) para ver el lenguaje intermedio de Microsoft (MSIL) en un ensamblado compilado.

Pueden surgir vulnerabilidades de seguridad que afecten a las cadenas de conexión en función del tipo de autenticación usado, de la forma en que las cadenas de conexión se almacenan en memoria y en disco, y de las técnicas usadas para construirlas en tiempo de ejecución.

## <a name="use-windows-authentication"></a>Uso de la autenticación de Windows

Para contribuir a limitar el acceso al origen de datos, debe proteger la información de la conexión como, por ejemplo, el id. de usuario, la contraseña y el nombre de origen de datos. Para evitar la exposición de información de usuario, se recomienda usar la autenticación de Windows (suele aparecer también como *seguridad integrada*) siempre que sea posible. La autenticación de Windows se especifica en una cadena de conexión mediante las palabras clave `Integrated Security` o `Trusted_Connection`, lo que elimina la necesidad de usar un identificador de usuario y una contraseña. Cuando se usa autenticación de Windows, este sistema operativo autentica a los usuarios y el acceso a los recursos del servidor y de la base de datos se determina mediante la concesión de permisos a usuarios y grupos de Windows.

Cuando no sea posible usar la autenticación de Windows, es necesario extremar las precauciones, ya que las credenciales de usuario están expuestas en la cadena de conexión. En una aplicación ASP.NET, puede configurar una cuenta de Windows como una identidad fija que se usa para conectarse a las bases de datos y a otros recursos de red. Para ello, habilite la suplantación en el elemento de identidad del archivo **web.config** y especifique un nombre de usuario y una contraseña.

```xml  
<identity impersonate="true"
        userName="MyDomain\UserAccount"
        password="*****" />  
```  

La cuenta de identidad fija debe ser una cuenta con pocos privilegios a la que sólo se le concedan los permisos necesarios en la base de datos. Además, debe cifrar el archivo de configuración para que el nombre de usuario y la contraseña no se expongan en texto no cifrado.

## <a name="avoid-injection-attacks-with-connection-string-builders"></a>Evitar ataques de inyección con generadores de cadenas de conexión

Se pueden producir ataques de inyección de cadenas de conexión cuando se usa la concatenación dinámica de cadenas para generar cadenas de conexión basadas en la entrada del usuario. Si no se valida la entrada del usuario y no se crean secuencias de escape para los caracteres o el texto malintencionado, los atacantes pueden tener acceso a datos confidenciales y a otros recursos del servidor. Para solucionar este problema, el proveedor de datos SqlClient de Microsoft para SQL Server incluyó por primera vez una nueva clase de generador de cadenas de conexión para validar la sintaxis de las cadenas y asegurarse de que no se insertan parámetros adicionales. Para obtener más información, vea [Generadores de cadenas de conexión](connection-string-builders.md).

## <a name="use-persist-security-infofalse"></a>Uso de "Persist Security Info=false"

El valor predeterminado de `Persist Security Info` es false, y se recomienda mantener este valor predeterminado en todas las cadenas de conexión. Si `Persist Security Info` se establece en `true` o `yes`, permitirá obtener información confidencial de seguridad de la conexión, incluidos el identificador del usuario y la contraseña, una vez que esté abierta. Si `Persist Security Info` se establece en `false` o `no`, la información de seguridad se descarta tras usarla para abrir la conexión. Esto permite garantizar que los orígenes que no sean de confianza no tengan acceso a la información confidencial de seguridad.

## <a name="encrypt-configuration-files"></a>Cifrado de archivos de configuración

[!INCLUDE[appliesto-netfx-netcore-xxxx-md](../../includes/appliesto-netfx-netcore-xxxx-md.md)]

Las cadenas de conexión también se pueden almacenar en archivos de configuración, lo que elimina la necesidad de incrustarlas en el código de la aplicación. Los archivos de configuración son archivos XML estándar para los que .NET Framework ha definido un conjunto común de elementos. Generalmente, las cadenas de conexión de los archivos de configuración se almacenan en el elemento **\<connectionStrings>** del archivo **app.config** si se trata de una aplicación Windows. o del archivo **web.config** si se trata de una aplicación ASP.NET. Para obtener más información sobre los conceptos básicos de almacenamiento, recuperación y cifrado de cadenas de conexión de archivos de configuración, vea [Cadenas de conexión y archivos de configuración](connection-strings-and-configuration-files.md).

## <a name="see-also"></a>Vea también

- [Cifrar información de configuración mediante una configuración protegida](/previous-versions/aspnet/53tyfkaw(v=vs.100))
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
