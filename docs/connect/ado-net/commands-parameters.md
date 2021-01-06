---
title: Comandos y parámetros
description: Obtenga información sobre cómo usar objetos de comando para el proveedor de datos SqlClient de Microsoft para SQL Server para ejecutar comandos y devolver resultados de un origen de datos.
ms.date: 11/25/2020
ms.assetid: b623f810-d871-49a5-b0f5-078cc3c34db6
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 07f23aa6545636fd8cbbf1717f6c8ee81ebe9294
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771479"
---
# <a name="commands-and-parameters"></a>Comandos y parámetros

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una vez establecida una conexión a un origen de datos, puede ejecutar comandos y devolver resultados desde el mismo mediante un objeto <xref:System.Data.Common.DbCommand>. Puede crear un comando mediante uno de los constructores de comandos para el proveedor de datos SqlClient de Microsoft para SQL Server. Los constructores pueden aceptar argumentos opcionales, como una instrucción SQL para ejecutar en el origen de datos, un objeto <xref:System.Data.Common.DbConnection> o un objeto <xref:System.Data.Common.DbTransaction>.

También puede configurar dichos objetos como propiedades del comando. También puede crear un comando para una determinada conexión mediante el método <xref:System.Data.Common.DbConnection.CreateCommand%2A> de un objeto `DbConnection`. La instrucción SQL que ejecuta el comando se puede configurar mediante la propiedad <xref:System.Data.Common.DbCommand.CommandText%2A>. El proveedor de datos SqlClient de Microsoft para SQL Server tiene el objeto <xref:Microsoft.Data.SqlClient.SqlCommand>.

## <a name="in-this-section"></a>En esta sección

[Ejecutar un comando](execute-command.md)  
Describe el objeto `Command` de ADO.NET, así como la forma de utilizarlo para ejecutar consultas y comandos con respecto a un origen de datos.

[Configuración de parámetros](configure-parameters.md)  
Describe el trabajo con parámetros `Command`, incluidos dirección, tipo de datos y sintaxis de parámetros.

[Generación de comandos con objetos CommandBuilder](generate-commands-with-commandbuilders.md)  
Describe cómo utilizar generadores de comandos para generar automáticamente comandos INSERT, UPDATE y DELETE para un `DataAdapter` que tiene un comando SELECT de tabla única.

[Obtención de un valor único de una base de datos](obtain-single-value-from-database.md)  
Describe cómo utilizar el método `ExecuteScalar` de un objeto `Command` para devolver un único valor desde una consulta de base de datos.

[Uso de comandos para modificar datos](use-commands-to-modify-data.md)  
Se describe cómo usar el proveedor de datos SqlClient de Microsoft para SQL Server a fin de ejecutar procedimientos almacenados o instrucciones de lenguaje de definición de datos (DDL).

## <a name="see-also"></a>Vea también

- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
