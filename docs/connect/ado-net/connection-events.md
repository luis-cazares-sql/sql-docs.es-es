---
title: Eventos de conexión
description: Los eventos de conexión para recuperar mensajes informativos de un origen de datos y determinar si se cambia su estado.
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8151915dc6c16c6225fec9ab90cb5a88e86b992f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771455"
---
# <a name="connection-events"></a>Eventos de conexión

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Todos los proveedores de datos SqlClient de Microsoft tienen objetos **Connection** con dos eventos que se pueden utilizar para recuperar mensajes informativos de un origen de datos o para determinar si ha cambiado el estado de un objeto **Connection**. En la tabla siguiente se enumeran los eventos del objeto **Connection**.

|Evento|Descripción|  
|-----------|-----------------|  
|**InfoMessage**|Se produce cuando se devuelve un mensaje informativo desde un origen de datos. Los mensajes informativos son aquellos procedentes de orígenes de datos que no inician una excepción.|  
|**StateChange**|Se produce cuando cambia el estado del objeto **Connection**.|  

## <a name="work-with-the-infomessage-event"></a>Trabajo con el evento InfoMessage

Con el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> del objeto <xref:Microsoft.Data.SqlClient.SqlConnection> puede recuperar advertencias o mensajes informativos de un origen de datos de SQL Server. Si se devuelven errores desde el origen de datos con un nivel de seguridad entre 11 y 16, se inicia una excepción. Sin embargo, el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> se puede utilizar para obtener mensajes del origen de datos que no estén asociados a un error. En el caso de Microsoft SQL Server, cualquier error que tenga la gravedad 10, como máximo, se considera de tipo informativo y se captura mediante el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>. Para obtener más información, consulte el artículo [Niveles de gravedad de error del motor de base de datos](/sql/relational-databases/errors-events/database-engine-error-severities).

El evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> recibe un objeto <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs> que contiene en su propiedad **Errors** una colección de los mensajes del origen de datos. Puede consultar los objetos **Error** de esa colección para conocer el número de error y el texto del mensaje, así como el origen del error. El proveedor de datos SqlClient de Microsoft para SQL Server también incluye detalles acerca de la base de datos, el procedimiento almacenado y el número de línea del que procede el mensaje.

### <a name="example"></a>Ejemplo

En el ejemplo de código siguiente se muestra cómo se puede agregar un controlador de eventos para el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handle-errors-as-infomessages"></a>Gestión de errores como eventos InfoMessage

Normalmente, el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> solo se activa para mensajes informativos y de advertencia enviados desde el servidor. Sin embargo, cuando se produce un error real, se detiene la ejecución de los métodos **ExecuteNonQuery** o **ExecuteReader** que iniciaron la operación del servidor y se inicia una excepción.

Si desea seguir procesando el resto de las instrucciones de un comando, independientemente de los errores producidos en el servidor, establezca la propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A> de <xref:Microsoft.Data.SqlClient.SqlConnection> como `true`. De esta forma, la conexión activa el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> para errores, en lugar de iniciar una excepción e interrumpir el procesamiento. La aplicación cliente puede controlar el evento y reaccionar ante las situaciones de error.

> [!NOTE]
> Los errores con un nivel de gravedad de 17, como mínimo, que hacen que el servidor interrumpa el procesamiento de comandos, deben controlarse como excepciones. En este caso, se inicia una excepción, independientemente del modo en que se controle el error en el evento <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage>.

## <a name="work-with-the-statechange-event"></a>Trabajo con el evento StateChange

El evento **StateChange** se produce cuando cambia el estado de un objeto **Connection**. El evento **StateChange** recibe la clase <xref:System.Data.StateChangeEventArgs>, que permite determinar el cambio de estado de **Connection** por medio de las propiedades **OriginalState** y **CurrentState**. La propiedad **OriginalState** es una enumeración <xref:System.Data.ConnectionState> que indica el estado del objeto **Connection** antes del cambio. **CurrentState** es una enumeración <xref:System.Data.ConnectionState> que indica el estado del objeto **Connection** después del cambio.

En el ejemplo de código siguiente se utiliza el evento **StateChange** para escribir un mensaje en la consola cuando cambia el estado del objeto **Connection**.

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>Vea también

- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
