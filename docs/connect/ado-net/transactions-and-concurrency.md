---
title: Transacciones y simultaneidad
description: Se describe cómo usar el proveedor de datos SqlClient de Microsoft para SQL Server con transacciones y simultaneidad.
ms.date: 11/24/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: a30a3d3184c411fc0b54c0e26330a8fb138ffdae
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051366"
---
# <a name="transactions-and-concurrency"></a>Transacciones y simultaneidad

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Una transacción consiste en un comando único o en un grupo de comandos que se ejecutan como un paquete. Las transacciones permiten combinar varias operaciones en una sola unidad de trabajo. Si en un punto de la transacción se produjera un error, todas las actualizaciones podrían revertirse y devolverse al estado que tenían antes de la transacción.

Una transacción debe ajustarse a las propiedades ACID (atomicidad, coherencia, aislamiento y durabilidad) para poder garantizar la coherencia de datos. La mayoría de los sistemas de bases de datos relacionales, como Microsoft SQL Server, admiten transacciones, al proporcionar funciones de bloqueo, registro y administración de transacciones cada vez que una aplicación cliente realiza una operación de actualización, inserción o eliminación.

> [!NOTE]
> Las transacciones que requieren varios recursos pueden reducir la simultaneidad si la duración del bloqueo es demasiado larga. Por ello, haga la transacción lo más corta posible.  

Las transacciones explícitas en procedimientos almacenados suelen dar mejores resultados cuando una transacción implica el uso de varias tablas en la misma base de datos o servidor. Se pueden crear transacciones en procedimientos almacenados de SQL Server mediante las instrucciones `BEGIN TRANSACTION`, `COMMIT TRANSACTION` o `ROLLBACK TRANSACTION` de Transact-SQL. Para obtener más información, vea Libros en pantalla de SQL Server.

Para las transacciones que implican varios administradores de recursos, como una transacción entre SQL Server y Oracle, se necesita una transacción distribuida.

## <a name="in-this-section"></a>En esta sección

 [Transacciones locales](local-transactions.md)  
 Muestra cómo realizar transacciones en una base de datos.  
  
 [Transacciones distribuidas](distributed-transactions.md)  
 Describe cómo realizar transacciones distribuidas en ADO.NET.  
  
 [Integración de System.Transactions con SQL Server](system-transactions-integration-with-sql-server.md)  
 Se describe la integración de <xref:System.Transactions> con SQL Server para trabajar con transacciones distribuidas.  
  
 [Simultaneidad optimista](optimistic-concurrency.md) Se describe la simultaneidad optimista y pesimista, y cómo puede probar si hay infracciones de simultaneidad.  

## <a name="see-also"></a>Consulte también

- [Aspectos básicos de las transacciones](/dotnet/framework/data/transactions/transaction-fundamentals)
- [Conexión al origen de datos](connecting-to-data-source.md)
- [Comandos y parámetros](commands-parameters.md)
- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
