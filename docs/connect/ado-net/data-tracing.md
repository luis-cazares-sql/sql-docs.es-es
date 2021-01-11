---
title: Seguimiento de datos en SqlClient
description: Describe cómo el proveedor de datos SqlClient de Microsoft para SQL Server proporciona la funcionalidad de seguimiento de datos integrada.
ms.date: 12/04/2020
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fc8b5a7ca06af3c3e3ea83fcb747f79517e5ef7a
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744465"
---
# <a name="data-tracing-in-sqlclient"></a>Seguimiento de datos en SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET incluye la funcionalidad de seguimiento de datos integrada que es compatible con el proveedor de datos SqlClient de Microsoft para SQL Server y los protocolos de red de SQL Server.

Las llamadas API de acceso a datos de traza ayudan a diagnosticar los siguientes problemas:

- Falta de coincidencia de esquemas entre el programa cliente y la base de datos.

- Inoperabilidad de la base de datos o problemas con las bibliotecas de red.

- SQL incorrecto, tanto si está incluido en el código como si lo ha generado una aplicación.

- Lógica de programación incorrecta.

- Problemas resultantes de la interacción entre el proveedor de datos SqlClient de Microsoft para SQL Server y sus propios componentes.

Para garantizar la compatibilidad entre diferentes tecnologías de traza, éste es ampliable, de manera que un programador puede realizar un seguimiento de un problema en cualquier nivel de la pila de la aplicación. El proveedor de datos SqlClient de Microsoft para SQL Server aprovecha las ventajas de las API de instrumentación y seguimiento generalizadas.

Para obtener más información acerca de la instalación y configuración del seguimiento administrado en .NET, consulte [Traza de acceso a datos](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)).

## <a name="access-diagnostic-information-in-the-extended-events-log"></a>Acceso a información de diagnóstico en el registro de eventos extendidos

En el proveedor de datos SqlClient de Microsoft para SQL Server, [Seguimiento de acceso a datos](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10)) facilita la correlación de eventos de cliente con la información de diagnóstico, como errores de conexión, de la información del búfer en anillo de conectividad y de rendimiento de la aplicación del servidor en el registro de eventos extendidos. Para obtener información sobre la lectura del registro de eventos extendidos, vea [Ver datos de sesión de evento](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110)).

Para las operaciones de conexión, el proveedor de datos SqlClient de Microsoft para SQL Server enviará un identificador de conexión de cliente. Si se produce un error en la conexión, puede tener acceso al búfer de anillo de conectividad ([Solución de problemas de conectividad en SQL Server 2008 con el búfer de anillo de conectividad](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)) y buscar el campo `ClientConnectionID` para obtener información de diagnóstico acerca del error de conexión. Los identificadores de conexión del cliente se registran en el búfer de anillo únicamente si se produce un error. Si se produce un error en una conexión antes de enviar el paquete de inicio de sesión previo, no se generará un identificador de conexión del cliente. El identificador de conexión del cliente es un GUID de 16 bytes. También puede buscar el identificador de la conexión de cliente en la salida de destino de los eventos extendidos si la acción `client_connection_id` se agregó a los eventos en una sesión de eventos extendidos. Puede habilitar el seguimiento de acceso a datos y volver a ejecutar el comando de conexión y observar el campo `ClientConnectionID` en el seguimiento de acceso a datos, si necesita ayuda adicional de diagnóstico del controlador del cliente.

Puede obtener el identificador de la conexión de cliente mediante programación con la propiedad `SqlConnection.ClientConnectionID`.

> [!NOTE]
> El proveedor de datos SqlClient de Microsoft para SQL Server admite el identificador de proceso de servidor desde la versión 2.1.0. Puede obtenerlo mediante programación con la propiedad `SqlConnection.ServerProcessId`.

`ClientConnectionID` y `ServerProcessId` están disponibles para un objeto <xref:Microsoft.Data.SqlClient.SqlConnection> que establece correctamente una conexión. Si se produce un error en un intento de conexión, `ClientConnectionID` puede estar disponible a través de `SqlException.ToString`.

El proveedor de datos SqlClient de Microsoft para SQL Server también envía un identificador de actividad específico del subproceso. El identificador de actividad se captura en las sesiones de eventos extendidos si las sesiones se inician con la opción TRACK_CAUSAILITY habilitada. Con respecto a los problemas de rendimiento con una conexión activa, puede obtener el identificador de actividad a partir del seguimiento de acceso a datos del cliente (campo `ActivityID`) y, posteriormente, buscar el identificador de actividad en la salida de eventos extendidos. El identificador de actividad en los eventos extendidos es un GUID de 16 bytes (no es el mismo GUID que el identificador de conexión de cliente) anexado con un número de secuencia de cuatro bytes. El número de secuencia representa el orden de una solicitud en un subproceso e indica el orden relativo del lote, así como las instrucciones RPC para el subproceso. `ActivityID` se envía actualmente opcionalmente para instrucciones por lotes de SQL y solicitudes RPC cuando el seguimiento de acceso a datos está habilitado y el decimoctavo bit de la palabra de configuración de seguimiento de acceso a datos está activado.

La siguiente instrucción SQL es un ejemplo que utiliza Transact-SQL para iniciar una sesión de eventos extendidos que se va a almacenar en un búfer de anillo y que registrará el identificador de actividad que se envía desde un cliente en operaciones de RPC y por lotes.

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>Consulte también
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
