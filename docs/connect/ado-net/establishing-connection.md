---
title: Establecimiento de conexión
description: Instrucciones para la conexión a un servidor SQL Server por el proveedor SqlClient.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: b3d92f49fb4eae6184a673ffc25ff6bbdf521626
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771597"
---
# <a name="establishing-connection"></a>Establecimiento de conexión

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Para conectarse a Microsoft SQL Server, use el objeto <xref:Microsoft.Data.SqlClient.SqlConnection> del proveedor de datos SqlClient de Microsoft para SQL Server. Para almacenar y recuperar de forma segura las cadenas de conexión, vea [Proteger la información de conexión](protecting-connection-information.md).

## <a name="closing-connections"></a>Cierre de conexiones

Recomendamos cerrar siempre la conexión cuando termine de utilizarla, para que la conexión pueda regresar al grupo. El bloque `Using` de Visual Basic o C# elimina automáticamente la conexión cuando el código sale del bloque, incluso en el caso de una excepción no controlada. Vea [Instrucción using](/dotnet/csharp/language-reference/keywords/using-statement) e [Instrucción using](/dotnet/visual-basic/language-reference/statements/using-statement) para obtener más información.

También puede utilizar los métodos `Close` o `Dispose` del objeto de conexión. Es posible que las conexiones que no se cierran explícitamente no se puedan agregar ni puedan regresar al grupo. Por ejemplo, una conexión que se ha salido del ámbito pero que no se ha cerrado explícitamente solo se devolverá al grupo de conexión si se ha alcanzado el tamaño máximo del grupo y la conexión aún es válida.

> [!NOTE]
> No llame a `Close` o a `Dispose` en un objeto **Connection**, un objeto **DataReader** o cualquier otro objeto administrado en el método `Finalize` de la clase. En un finalizador, libere solo los recursos no administrados que pertenezcan directamente a su clase. Si la clase no dispone de recursos no administrados, no incluya un método `Finalize` en la definición de clase. Para obtener más información, consulte [Recolección de elementos no utilizados](/dotnet/standard/garbage-collection/index).

> [!NOTE]
> Los eventos de inicio y cierre de sesión no se provocarán en el servidor cuando se busque una conexión desde el grupo de conexiones o se devuelva a éste, puesto que la conexión no está cerrada realmente cuando se devuelve al grupo de conexiones. Para obtener más información, vea [Agrupación de conexiones de SQL Server (ADO.NET)](sql-server-connection-pooling.md).

## <a name="connecting-to-sql-server"></a>Conectarse a SQL Server

Para consultar los nombres y valores válidos de formato de cadena, vea la propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlConnection>. También puede usar la clase <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> para crear cadenas de conexión sintácticamente válidas en tiempo de ejecución. Para obtener más información, vea [Generadores de cadenas de conexión](connection-string-builders.md).

El siguiente código de ejemplo demuestra cómo crear y abrir una conexión a una base de datos SQL Server.

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>Seguridad integrada y ASP.NET

La seguridad integrada de SQL Server (también conocida como conexiones de confianza) ayuda a proporcionar protección al conectarse a SQL Server, ya que no expone un identificador de usuario ni una contraseña en la cadena de conexión y es el método recomendado para autenticar una conexión. La seguridad integrada utiliza la identidad de seguridad actual, o símbolo (token), del proceso en ejecución, que En el caso de las aplicaciones de escritorio, esta identidad suele ser la identidad del usuario que tiene la sesión actualmente iniciada.

La identidad de seguridad para aplicaciones ASP.NET se puede establecer en una de varias opciones diferentes. Para comprender mejor la identidad de seguridad que usa una aplicación de ASP.NET al conectarse a SQL Server, consulte [Suplantación de ASP.NET](/previous-versions/aspnet/xh507fc5(v=vs.100)), [Autenticación de ASP.NET](/previous-versions/aspnet/eeyk640h(v=vs.100))y [Procedimiento de acceso a SQL Server mediante la seguridad integrada de Windows](/previous-versions/aspnet/bsz5788z(v=vs.100)).

## <a name="see-also"></a>Vea también

- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Cadenas de conexión](connection-strings.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
