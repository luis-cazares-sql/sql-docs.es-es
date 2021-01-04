---
title: Distributed transactions
description: Se describe cómo realizar transacciones distribuidas en ADO.NET.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559237"
---
# <a name="distributed-transactions"></a>Distributed transactions

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Entre otras cosas, una transacción es un conjunto de tareas relacionadas que se ejecutan correctamente (confirman) o dan error (anulan) como una unidad. Una *transacción distribuida* es una transacción que afecta a varios recursos. Para que una transacción distribuida se confirme, todos los participantes deben garantizar que los cambios en los datos serán permanentes. Los cambios deben conservarse a pesar de bloqueos del sistema u otros eventos imprevistos. Si alguno de los participantes no cumple esta garantía, toda la transacción da error y se revertirán los cambios en los datos en el ámbito de la transacción. 

> [!NOTE]
> Si intenta confirmar o revertir una transacción al iniciar un `DataReader` mientras la transacción está activa, se produce una excepción.

## <a name="working-with-systemtransactions"></a>Trabajo con System.Transactions

En .NET, las transacciones distribuidas se administran a través de la API del espacio de nombres <xref:System.Transactions>. Cuando hay implicados varios administradores de recursos persistentes, la API <xref:System.Transactions> delegará el control de las transacciones distribuidas en un monitor de transacciones como el Coordinador de transacciones distribuidas de Microsoft (MS DTC). Para obtener más información, vea [Aspectos básicos de las transacciones](/dotnet/framework/data/transactions/transaction-fundamentals).

ADO.NET 2.0 incorporó la compatibilidad con la inscripción en una transacción distribuida mediante el método `EnlistTransaction`, que inscribe una conexión en una instancia de <xref:System.Transactions.Transaction>. En las versiones anteriores de ADO.NET, la inscripción explícita en transacciones distribuidas se realizaba mediante el método `EnlistDistributedTransaction` de una conexión que inscribía ésta en una instancia <xref:System.EnterpriseServices.ITransaction>, en la que se permitía la compatibilidad con versiones anteriores. Para obtener más información sobre las transacciones de Enterprise Services, vea [Interoperabilidad con Enterprise Services y transacciones de COM+](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions).

Cuando se utiliza una transacción <xref:System.Transactions> con el proveedor de datos SqlClient de Microsoft para SQL Server en una base de datos SQL Server, se usa automáticamente un objeto <xref:System.Transactions.Transaction> ligero. A continuación, la transacción se puede promover a una transacción distribuida completa si es necesario. Para obtener más información, vea [Integración de System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>Inscripción automática en una transacción distribuida

La inscripción automática es el método predeterminado (y preferido) de integrar conexiones ADO.NET con `System.Transactions`. Un objeto de conexión se inscribirá automáticamente en una transacción distribuida existente si se determina que hay una transacción activa, que, en términos de `System.Transaction`, significa que `Transaction.Current` no es nula. La inscripción automática en transacciones tiene lugar cuando se abre la conexión. No ocurrirá después, incluso si se ejecuta un comando dentro del ámbito de una transacción. Puede deshabilitar la inscripción automática en las transacciones existentes si especifica `Enlist=false` como un parámetro de cadena de conexión para un objeto <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType>.

## <a name="manually-enlisting-in-a-distributed-transaction"></a>Inscripción manual en una transacción distribuida

Si la inscripción automática está deshabilitada o tiene que inscribir una transacción que se ha iniciado después de abrir la conexión, puede realizar la inscripción en una transacción distribuida existente mediante el método `EnlistTransaction` del objeto <xref:Microsoft.Data.SqlClient.SqlConnection> para el proveedor de datos SqlClient de Microsoft para SQL Server. La inscripción en una transacción distribuida existente garantiza que, si la transacción se confirma o revierte, también se confirmarán o revertirán las modificaciones realizadas por el código en el origen de datos.

La inscripción en transacciones distribuidas es especialmente conveniente al agrupar objetos empresariales. Si se agrupa un objeto empresarial con una conexión abierta, la inscripción automática en transacciones sólo se produce cuando se abre esa conexión. Si se realizan varias transacciones con el objeto empresarial agrupado, la conexión abierta para ese objeto no se inscribirá automáticamente en las transacciones recién iniciadas. En este caso, puede deshabilitar la inscripción automática de la conexión en la transacción e inscribir la conexión en las transacciones mediante `EnlistTransaction`.

`EnlistTransaction` acepta un único argumento de tipo <xref:System.Transactions.Transaction> que es una referencia a la transacción existente. Después de llamar al método `EnlistTransaction` de la conexión, todas las modificaciones realizadas en el origen de datos mediante la conexión se incluyen en la transacción. Si se pasa un valor nulo, se anula la inscripción de la conexión de su inscripción actual en transacciones distribuidas. Tenga en cuenta que la conexión se debe abrir antes de llamar a `EnlistTransaction`.

> [!NOTE]
> Una vez que una conexión se inscribe explícitamente en una transacción, no se puede anular su inscripción ni inscribirse en otra transacción hasta que finaliza la primera transacción.

> [!CAUTION]
> Si la conexión ya ha comenzado una transacción mediante el método `EnlistTransaction` de la conexión, <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> inicia una excepción. No obstante, si la transacción es una transacción local iniciada en el origen de datos (por ejemplo, al ejecutar la instrucción BEGIN TRANSACTION de forma explícita mediante un <xref:Microsoft.Data.SqlClient.SqlCommand>), `EnlistTransaction` la revertirá e inscribirá en la transacción distribuida existente como se ha solicitado. No recibirá aviso de que la transacción local se ha revertido y deberá administrar todas las transacciones locales no iniciadas mediante <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A>. Si usa el proveedor de datos SqlClient de Microsoft para SQL Server con SQL Server, al intentar una inscripción se iniciará una excepción. Todos los demás casos no se detectarán.  

## <a name="promotable-transactions-in-sql-server"></a>Transacciones aptas para promoción en SQL Server

SQL Server 2005 admite transacciones que se pueden promover en las que se pueda promover automáticamente una transacción ligera local a una transacción distribuida solamente cuando es necesario. Las transacciones promovibles no invocan la sobrecarga adicional de las transacciones distribuidas a menos que sea necesario. Para obtener más información y un ejemplo de código, vea [Integración de System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md).

## <a name="configuring-distributed-transactions"></a>Configuración de transacciones distribuidas

 Puede que necesite habilitar MS DTC a través de la red para usar transacciones distribuidas. Si tiene Firewall de Windows habilitado, debe permitir que el servicio MS DTC use la red o abrir el puerto MS DTC.  
  
## <a name="see-also"></a>Consulte también

- [Transacciones y simultaneidad](transactions-and-concurrency.md)
- [Integración de System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
