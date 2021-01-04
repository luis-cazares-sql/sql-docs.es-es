---
title: Integración de System.Transactions con SQL Server
description: Se describe la integración de System.Transactions con SQL Server para trabajar con transacciones distribuidas.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: af0ba2865719a5388314a4ca695e09191cb56173
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051378"
---
# <a name="systemtransactions-integration-with-sql-server"></a>Integración de System.Transactions con SQL Server

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En .NET se incluye un marco de transacciones al que se puede acceder a través del espacio de nombres <xref:System.Transactions>. Este marco expone las transacciones de forma que se integran totalmente en .NET, incluido ADO.NET.  
  
Además de las mejoras de programación, <xref:System.Transactions> y ADO.NET pueden funcionar conjuntamente para coordinar las optimizaciones al trabajar con transacciones. Una transacción promovible es una transacción ligera (local) que, en caso necesario, se puede promover automáticamente a una transacción completamente distribuida.

El proveedor de datos SqlClient de Microsoft para SQL Server admite transacciones aptas para promoción cuando se trabaja con SQL Server. Las transacciones promovibles no invocan la sobrecarga adicional de las transacciones distribuidas a menos que sea necesario. Las transacciones aptas para promoción son automáticas y no necesitan la intervención del desarrollador.

## <a name="creating-promotable-transactions"></a>Creación de transacciones aptas para promoción

El proveedor de datos SqlClient de Microsoft para SQL Server ofrece compatibilidad con transacciones aptas para promoción, que se controlan a través de las clases del espacio de nombres <xref:System.Transactions>. Las transacciones promocionadas optimizan las transacciones distribuidas ya que aplazan la creación de las mismas hasta que es necesario. Si solo se necesita un administrador de recursos, no tiene lugar ninguna transacción distribuida.

> [!NOTE]
> En un caso que no es de plena confianza, se requiere <xref:System.Transactions.DistributedTransactionPermission> cuando la transacción se promueve al nivel de transacción distribuida.

## <a name="promotable-transaction-scenarios"></a>Escenarios de transacciones aptas para promoción

Normalmente, las transacciones distribuidas consumen muchos recursos del sistema, siendo el encargado de administrarlas Microsoft DTC (Coordinador de transacciones distribuidas), que integra todos los administradores de recursos a los que se tiene acceso en la transacción. Una transacción apta para promoción es una forma especial de una transacción <xref:System.Transactions> que delega eficazmente el trabajo en una transacción simple de SQL Server. <xref:System.Transactions>, <xref:Microsoft.Data.SqlClient> y SQL Server coordinan el trabajo relacionado con la administración de la transacción, y la promueven a una transacción distribuida completa, según sea necesario.

La ventaja de utilizar transacciones promocionadas es que cuando se abre una conexión utilizando una transacción <xref:System.Transactions.TransactionScope> activa, y no hay ninguna otra conexión abierta, la transacción se confirma como una transacción ligera, en lugar de incurrir en la sobrecarga adicional de una transacción completamente distribuida.

### <a name="connection-string-keywords"></a>Palabras clave de cadena de conexión

La propiedad <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> admite una palabra clave, `Enlist`, que indica si <xref:Microsoft.Data.SqlClient> detectará contextos transaccionales e inscribirá automáticamente la conexión en una transacción distribuida. Si `Enlist=true`, la conexión se inscribe automáticamente en el contexto de transacción actual del subproceso de apertura. Si `Enlist=false`, la conexión `SqlClient` no interactúa con una transacción distribuida. El valor predeterminado de `Enlist` es true. Si no se especifica `Enlist` en la cadena de conexión, la conexión se da de alta automáticamente en una transacción distribuida si se detecta una al abrirse la conexión.

Las palabras clave `Transaction Binding` en una cadena de conexión <xref:Microsoft.Data.SqlClient.SqlConnection> controlan la asociación de la conexión con una transacción `System.Transactions` dada de alta. También está disponible mediante la propiedad <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder.TransactionBinding%2A> de <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder>.

La siguiente tabla describe los posibles valores.
  
|Palabra clave|Descripción|  
|-------------|-----------------|  
|Desenlace implícito|El valor predeterminado. La conexión se separa de la transacción cuando termina, y vuelve a cambiar al modo de confirmación automática.|
|Desenlace explícito|La conexión sigue adjuntada a la transacción hasta que ésta se cierra. La conexión producirá errores si no está activa o no coincide con <xref:System.Transactions.Transaction.Current%2A>.|

## <a name="using-transactionscope"></a>Uso de TransactionScope

La clase <xref:System.Transactions.TransactionScope> crea un bloque de código transaccional dando de alta implícitamente las conexiones en una transacción distribuida. Debe llamar al método <xref:System.Transactions.TransactionScope.Complete%2A> al final del bloque <xref:System.Transactions.TransactionScope> antes de abandonarlo. Al salir del bloque se invoca el método <xref:System.Transactions.TransactionScope.Dispose%2A> . Si se ha producido una excepción que ocasiona que el código salga del ámbito, la transacción se considera anulada.

Se recomienda el uso de un bloque `using` para asegurarse de que se llama a <xref:System.Transactions.TransactionScope.Dispose%2A> en el objeto <xref:System.Transactions.TransactionScope> cuando se sale de dicho bloque. Si no se confirman ni revierten las transacciones pendientes, el rendimiento puede verse seriamente afectado ya que el tiempo de espera predeterminado de <xref:System.Transactions.TransactionScope> es un minuto. Si no utiliza una instrucción `using`, debe realizar todo el trabajo de un bloque `Try` y llamar explícitamente al método <xref:System.Transactions.TransactionScope.Dispose%2A> del bloque `Finally`.

Si se produce una excepción en <xref:System.Transactions.TransactionScope>, la transacción se marca como incoherente y se abandona. Se revertirá cuando se elimine el <xref:System.Transactions.TransactionScope> . Si no se produce ninguna excepción, las transacciones participantes se confirman.

> [!NOTE]
> La clase `TransactionScope` crea una transacción con <xref:System.Transactions.Transaction.IsolationLevel%2A> de `Serializable` de forma predeterminada. Dependiendo de la aplicación, puede considerar la opción de reducir el nivel de aislamiento para evitar conflictos en ella.

> [!NOTE]
> Se recomienda que solo realice actualizaciones, inserciones y eliminaciones en transacciones distribuidas, ya que consumen una cantidad considerable de recursos de base de datos. Las instrucciones SELECT pueden bloquear los recursos de base de datos de forma innecesaria y, en algunas situaciones, es posible que tengan que utilizarse transacciones para las selecciones. Todo el trabajo que no sea de base de datos debe realizarse fuera del ámbito de la transacción, a menos que estén implicados otros administradores de recursos de transacción.
Aunque una excepción en el ámbito de la transacción impide que se confirme la misma, la clase <xref:System.Transactions.TransactionScope> no deja revertir los cambios que haya realizado el código fuera del ámbito de la propia transacción. Si es necesario realizar alguna acción cuando se revierta la transacción, deberá escribir su propia implementación de la interfaz <xref:System.Transactions.IEnlistmentNotification> y darla de alta explícitamente en la transacción.

## <a name="example"></a>Ejemplo

Trabajar con <xref:System.Transactions> requiere disponer de una referencia a System.Transactions.dll.

La siguiente función muestra cómo crear una transacción promocionada en dos instancias de SQL Server diferentes, representadas por dos objetos <xref:Microsoft.Data.SqlClient.SqlConnection> diferentes, que se incluyen en un bloque <xref:System.Transactions.TransactionScope> .

El código crea el bloque <xref:System.Transactions.TransactionScope> con una instrucción `using` y abre la primera conexión, que automáticamente lo inscribe en <xref:System.Transactions.TransactionScope>.

La transacción se da de alta inicialmente como una transacción ligera, no como una transacción distribuida completa. La segunda conexión se inscribe en <xref:System.Transactions.TransactionScope> únicamente si el comando de la primera conexión no produce una excepción. Cuando se abre la segunda conexión, la transacción se promociona automáticamente a una transacción completamente distribuida.

Más adelante, se invoca el método <xref:System.Transactions.TransactionScope.Complete%2A>, que confirma la transacción solo si no se ha iniciado ninguna excepción. Si en algún punto del bloque <xref:System.Transactions.TransactionScope> se ha producido una excepción, no se llamará a `Complete` y, cuando se elimine <xref:System.Transactions.TransactionScope> al final de su bloque `using` , se revertirá la transacción distribuida.

[!code-csharp[SqlTransactionScope#1](~/../sqlclient/doc/samples/SqlTransactionScope.cs#1)]

## <a name="see-also"></a>Consulte también

- [Transacciones y simultaneidad](transactions-and-concurrency.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
