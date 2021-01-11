---
title: Programación asincrónica
description: Aprenda sobre la programación asincrónica en el proveedor de datos SqlClient de Microsoft para SQL Server.
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804300"
---
# <a name="asynchronous-programming"></a>Programación asincrónica

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En este tema se analiza la compatibilidad del proveedor de datos SqlClient de Microsoft para SQL Server (SqlClient) con la programación asincrónica.

## <a name="legacy-asynchronous-programming"></a>Programación asincrónica heredada

El proveedor de datos SqlClient de Microsoft para SQL Server incluye métodos de **System.Data.SqlClient** para mantener la compatibilidad con versiones anteriores de las aplicaciones que migran a <xref:Microsoft.Data.SqlClient>. No se recomienda usar los métodos de programación asincrónica heredados siguientes para un desarrollo nuevo:

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> En el proveedor de datos SqlClient de Microsoft para SQL Server, estos métodos heredados ya no requieren `Asynchronous Processing=true` en la cadena de conexión.

## <a name="asynchronous-programming-features"></a>Características de la programación asincrónica

Estas características de la programación asincrónica proporcionan una técnica sencilla para hacer que el código sea asincrónico.

Para más información sobre la programación asincrónica en .NET, consulte:

- [Programación asincrónica en C#](/dotnet/csharp/async)

- [Programación asincrónica con Async y Await (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

Cuando la interfaz de usuario no responde o el servidor no escala, es probable que necesite que el código sea más asincrónico. La escritura de código asincrónico ha implicado tradicionalmente la instalación de una devolución de llamada (también denominada continuación) para expresar la lógica que tiene lugar después de que la operación asincrónica finalice. Esto complica la estructura del código asincrónico con respecto al código sincrónico.

Puede llamar a métodos asincrónicos sin usar devoluciones de llamada y sin dividir el código en varios métodos o expresiones lambda.

El modificador `async` especifica que un método es asincrónico. Al llamar a un método `async`, se devuelve una tarea. Cuando se aplica el operador `await` a una tarea, el método actual finaliza de inmediato. Cuando la tarea finaliza, la ejecución se reanuda en el mismo método.

> [!TIP]
> En el proveedor de datos SqlClient de Microsoft para SQL Server, no se requieren llamadas asincrónicas para establecer la palabra clave de la cadena de conexión `Context Connection`.

Al llamar a un método `async` no se asigna ningún subproceso adicional. Puede usar el subproceso existente de finalización de E/S momentáneamente al final.

Los métodos siguientes en el proveedor de datos SqlClient de Microsoft para SQL Server admiten la programación asincrónica:

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

Otros miembros asincrónicos admiten la [compatibilidad con streaming de SqlClient](sqlclient-streaming-support.md).

> [!TIP]
> Los métodos asincrónicos no requieren `Asynchronous Processing=true` en la cadena de conexión. Esta propiedad es obsoleta en el proveedor de datos SqlClient de Microsoft para SQL Server.

### <a name="synchronous-to-asynchronous-connection-open"></a>Apertura de conexión sincrónica a asincrónica

Puede actualizar una aplicación existente para usar la característica asincrónica. Por ejemplo, suponga que una aplicación tiene un algoritmo de conexión sincrónico y bloquea el subproceso de la interfaz de usuario cada vez que se conecta a la base de datos y, una vez que está conectada, la aplicación llama a un procedimiento almacenado que señala otros usuarios en vez de al que acaba de iniciar sesión.

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

Cuando se convierte para usar la funcionalidad asincrónica, el programa tendría el aspecto siguiente:

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>Incorporación de una característica asincrónica a una aplicación existente (mediante la combinación de modelos existentes y nuevos)

También es posible agregar una funcionalidad asincrónica (SqlConnection::OpenAsync) sin cambiar la lógica asincrónica existente. Por ejemplo, si una aplicación usa actualmente:

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

Puede empezar a usar el modelo asincrónico sin cambiar sustancialmente el algoritmo existente.

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>Uso del modelo de proveedor base y la característica asincrónica

Puede que tenga que crear una herramienta que pueda conectarse a bases de datos diferentes y ejecutar consultas. Puede usar el modelo de proveedor base y la característica asincrónica.

El Controlador de transacciones distribuidas de Microsoft (MSDTC) debe estar habilitado en el servidor para usar transacciones distribuidas. Para información sobre cómo habilitar MSDTC, consulte el artículo sobre cómo [habilitar MSDTC en un servidor web](/previous-versions/commerce-server/dd327979(v=cs.90)).

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>Uso de transacciones SQL y la característica asincrónica nueva

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>Uso de transacciones distribuidas y la característica asincrónica nueva

En una aplicación empresarial, es posible que tenga que agregar **transacciones distribuidas** en algunos escenarios para habilitar las transacciones entre varios servidores de bases de datos. Puede usar el espacio de nombres System.Transactions e inscribir una transacción distribuida, del siguiente modo:

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>Cancelación de una operación asincrónica

Puede cancelar una solicitud asincrónica mediante <xref:System.Threading.CancellationToken>.

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>Operaciones asincrónicas con SqlBulkCopy

Las funcionalidades asincrónicas también están en <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> con <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>.

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>Uso asincrónico de varios comandos con MARS

En el ejemplo se abre una única conexión a la base de datos **AdventureWorks**. Con un objeto <xref:Microsoft.Data.SqlClient.SqlCommand>, se crea <xref:Microsoft.Data.SqlClient.SqlDataReader>. Cuando se utiliza el lector, se abre un segundo <xref:Microsoft.Data.SqlClient.SqlDataReader>, utilizando los datos del primer <xref:Microsoft.Data.SqlClient.SqlDataReader> como entrada de la cláusula WHERE para el segundo lector.

> [!NOTE]
> En el ejemplo siguiente se usa la base de datos [**AdventureWorks** de ejemplo](../../samples/adventureworks-install-configure.md). La cadena de conexión proporcionada en el código de ejemplo da por sentado que la base de datos está instalada y disponible en el equipo local. Modifique la cadena de conexión según sea necesario para el entorno.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>Lectura y actualización de datos de manera asincrónica con MARS

MARS permite usar una conexión para operaciones de lectura y de lenguaje de manipulación de datos (DML) con más de una operación pendiente. Esta característica elimina la necesidad de que una aplicación se ocupe de los errores de conexión ocupada. Además, MARS puede reemplazar el uso de los cursores del lado servidor, que suelen consumir más recursos. Finalmente, dado que es posible realizar varias operaciones con una sola conexión, pueden compartir el mismo contexto de transacción, lo que elimina la necesidad de usar los procedimientos almacenados del sistema **sp_getbindtoken** y **sp_bindsession**.

La siguiente aplicación de consola muestra cómo usar dos objetos <xref:Microsoft.Data.SqlClient.SqlDataReader> con tres objetos <xref:Microsoft.Data.SqlClient.SqlCommand> y un único objeto <xref:Microsoft.Data.SqlClient.SqlConnection> con MARS habilitado. El primer objeto de comando recupera una lista de proveedores cuya clasificación crediticia es 5. El segundo objeto de comando utiliza el identificador de proveedor proporcionado a partir de <xref:Microsoft.Data.SqlClient.SqlDataReader> para cargar el segundo <xref:Microsoft.Data.SqlClient.SqlDataReader> con todos los productos de ese proveedor en particular. El segundo <xref:Microsoft.Data.SqlClient.SqlDataReader> visita cada registro del producto. Para determinar cuál debe ser la nueva **OnOrderQty**, se realiza un cálculo. Luego, se usa el tercer objeto de comando para actualizar la tabla **ProductVendor** con el nuevo valor. Este proceso completo tiene lugar dentro de una única transacción, que se revierte al final.

> [!NOTE]
> En el ejemplo siguiente se usa la base de datos [**AdventureWorks** de ejemplo](../../samples/adventureworks-install-configure.md). La cadena de conexión proporcionada en el código de ejemplo da por sentado que la base de datos está instalada y disponible en el equipo local. Modifique la cadena de conexión según sea necesario para el entorno.

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
