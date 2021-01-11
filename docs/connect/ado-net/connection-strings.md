---
title: Cadenas de conexión
description: Obtenga información sobre las cadenas de conexión en el proveedor de datos SqlClient de Microsoft para SQL Server, que contienen la información de inicialización pasada como un parámetro de un proveedor de datos a un origen de datos.
ms.date: 11/13/2020
ms.assetid: 745c5f95-2f02-4674-b378-6d51a7ec2490
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 257ed7d43f8ab204c7c7e7575c69251be6f2efdf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771640"
---
# <a name="connection-strings-in-adonet"></a>Cadenas de conexión en ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una cadena de conexión contiene información de inicialización que se transfiere como un parámetro desde un proveedor de datos a un origen de datos. El proveedor de datos recibe la cadena de conexión como el valor de la propiedad <xref:System.Data.Common.DbConnection.ConnectionString?displayProperty=nameWithType>. El proveedor analiza la cadena de conexión y garantiza que la sintaxis es correcta y que se admiten las palabras clave. A continuación, el método <xref:System.Data.Common.DbConnection.Open?displayProperty=nameWithType> pasa los parámetros de conexión analizados al origen de datos. El origen de datos realiza una validación adicional y establece una conexión.

## <a name="connection-string-syntax"></a>Sintaxis de cadenas de conexión

Una cadena de conexión es una lista de pares de parámetros de clave y valor delimitados por punto y coma:

```csharp
keyword1=value; keyword2=value;
```

En las palabras clave no se distingue entre mayúsculas y minúsculas. Sin embargo, los valores pueden distinguir entre mayúsculas y minúsculas, según el origen de datos. Las palabras clave y los valores pueden contener [caracteres de espacio en blanco](https://en.wikipedia.org/wiki/Whitespace_character#Unicode). Los espacios en blanco iniciales y finales se omiten en las palabras clave y los valores sin comillas.

Si un valor contiene un punto y coma, [caracteres de control Unicode](https://en.wikipedia.org/wiki/Unicode_control_characters) o un espacio en blanco inicial o final, debe incluirse entre comillas simples o dobles. Por ejemplo:

```csharp
Keyword=" whitespace  ";
Keyword='special;character';
```

Es posible que el carácter envolvente no se encuentre dentro del valor que contiene. Por lo tanto, un valor que contenga comillas simples solo se puede incluir entre comillas dobles y viceversa:

```csharp
Keyword='double"quotation;mark';
Keyword="single'quotation;mark";
```

También puede aplicar el escape al carácter envolvente utilizando dos de ellos juntos:

```csharp
Keyword="double""quotation";
Keyword='single''quotation';
```

Las comillas, así como el signo igual, no requieren caracteres de escape, por lo que las siguientes cadenas de conexión son válidas:

```csharp
Keyword=no "escaping" 'required';
Keyword=a=b=c
```

Puesto que cada valor se lee hasta el punto y coma siguiente o hasta el final de la cadena, el valor del último ejemplo es `a=b=c` y el punto y coma final es opcional.

Todas las cadenas de conexión comparten la misma sintaxis básica que se ha descrito anteriormente. El conjunto de palabras clave reconocidas depende del proveedor. El proveedor de datos *SqlClient de Microsoft*  para *SQL Server* admite muchas palabras clave de API anteriores, pero suele ser más flexible y acepta sinónimos para muchas de las palabras clave de cadena de conexión comunes.

Los errores tipográficos pueden producir errores. Por ejemplo, `Integrated Security=true` es válido, pero `IntegratedSecurity=true` provoca un error.

Las cadenas de conexión construidas manualmente en el entorno de ejecución desde la entrada de usuario no validada son vulnerables a ataques de inyección de cadenas y ponen en peligro la seguridad en el origen de datos. Para solucionar estos problemas, se ha creado la clase <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>. Este [generador de cadenas de conexión](connection-string-builders.md) expone los parámetros como propiedades con establecimiento inflexible de tipos y permite validar la cadena de conexión antes de enviarla al origen de datos.

## <a name="in-this-section"></a>En esta sección

[Generador de cadenas de conexión](connection-string-builders.md)\
Muestra cómo usar la clase `ConnectionStringBuilder` para construir cadenas de conexión válidas en el entorno de ejecución.

[Cadenas de conexión y archivos de configuración](connection-strings-and-configuration-files.md)\
Muestra cómo almacenar y recuperar cadenas de conexión en archivos de configuración.

[Sintaxis de la cadena de conexión](connection-string-syntax.md)\
Describe cómo configurar cadenas de conexión específicas del proveedor para `SqlClient`.

[Protección de la información de conexión](protecting-connection-information.md)\
Muestra técnicas de protección de la información utilizada para conectarse a un origen de datos.

## <a name="see-also"></a>Consulte también

- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
