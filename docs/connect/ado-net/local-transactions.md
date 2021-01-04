---
title: Transacciones locales
description: Se muestra cómo realizar transacciones en una base de datos con el proveedor de datos SqlClient de Microsoft para SQL Server.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a310ab409c2300eb31d1e3c0e58b7ebe32096bfd
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051367"
---
# <a name="local-transactions"></a>Transacciones locales

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Las transacciones de ADO.NET se usan cuando se quiere enlazar varias tareas para que se ejecuten como una sola unidad de trabajo. Por ejemplo, imagine que una aplicación realiza dos tareas. Primero, actualiza una tabla con información de pedidos. Luego, actualiza una tabla que contiene la información de inventario, cargando en cuenta los elementos pedidos. Si no se puede realizar alguna de las tareas, las dos actualizaciones se revierten.  

## <a name="determining-the-transaction-type"></a>Determinación del tipo de transacción

Una transacción se considera local cuando consta de una única fase y la base de datos la controla directamente. Una transacción se considera distribuida cuando se coordina mediante un monitor de transacciones y usa mecanismos a prueba de errores (como la confirmación en dos fases) para su resolución.

El proveedor de datos SqlClient de Microsoft para SQL Server tiene un objeto <xref:Microsoft.Data.SqlClient.SqlTransaction> propio para realizar transacciones locales en bases de datos de SQL Server. Otros proveedores de datos de .NET también proporcionan objetos `Transaction` propios. Además, existe una clase <xref:System.Data.Common.DbTransaction> disponible para escribir código independiente del proveedor que requiere transacciones.

> [!NOTE]
> Las transacciones son más eficientes cuando se realizan en el servidor. Si trabaja con una base de datos de SQL Server que hace uso masivo de transacciones explícitas, debería estudiar la posibilidad de escribirlas como procedimientos almacenados mediante la instrucción BEGIN TRANSACTION de Transact-SQL.

## <a name="performing-a-transaction-using-a-single-connection"></a>Realización de una transacción con una sola conexión 

En ADO.NET, las transacciones se controlan con el objeto `Connection`. Puede iniciar una transacción local con el método `BeginTransaction`. Una vez iniciada una transacción, puede inscribir un comando en esa transacción con la propiedad `Transaction` de un objeto `Command`. Luego, puede confirmar o revertir las modificaciones realizadas en el origen de datos según el resultado correcto o incorrecto de los componentes de la transacción.

> [!NOTE]
> El método `EnlistDistributedTransaction` no se debe emplear en transacciones locales.

El ámbito de la transacción está limitado a la conexión. En el siguiente ejemplo se realiza una transacción explícita que consta de por dos comandos independientes en el bloque `try`. Los comandos ejecutan instrucciones INSERT sobre la tabla `Production.ScrapReason` de la base de datos de ejemplo AdventureWorks de SQL Server, que se confirman si no se inicia ninguna excepción. El código del bloque `catch` revierte la transacción si se lanza una excepción. Si la transacción se anula o la conexión se cierra antes de que se haya completado la transacción, ésta se revierte automáticamente.

## <a name="example"></a>Ejemplo  

 Para llevar a cabo una transacción, siga estos pasos.

1. Llame al método <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlConnection> para marcar el comienzo de la transacción. El método <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> devuelve una referencia a la transacción. Esta referencia se asigna a los objetos <xref:Microsoft.Data.SqlClient.SqlCommand> que están inscritos en la transacción.

2. Asigne el objeto `Transaction` a la propiedad <xref:Microsoft.Data.SqlClient.SqlCommand.Transaction%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlCommand> que se va a ejecutar. Si el comando se ejecuta en una conexión con una transacción activa y el objeto `Transaction` no se ha asignado a la propiedad `Transaction` del objeto `Command`, se inicia una excepción.

3. Ejecute los comandos necesarios.

4. Llame al método <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlTransaction> para completar la transacción, o al método <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A> para finalizarla. Si la conexión se cierra o elimina antes de que se hayan ejecutado los métodos <xref:Microsoft.Data.SqlClient.SqlTransaction.Commit%2A> o <xref:Microsoft.Data.SqlClient.SqlTransaction.Rollback%2A>, la transacción se revierte.

En el ejemplo de código siguiente se muestra la lógica transaccional al usar el proveedor de datos SqlClient de Microsoft para SQL Server.  

[!code-csharp[SqlTransactionLocal#1](~/../sqlclient/doc/samples/SqlTransactionLocal.cs#1)]

## <a name="see-also"></a>Consulte también

- [Transacciones y simultaneidad](transactions-and-concurrency.md)
- [Transacciones distribuidas](distributed-transactions.md)
- [Integración de System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
