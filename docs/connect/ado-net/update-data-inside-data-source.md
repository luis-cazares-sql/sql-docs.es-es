---
title: Actualización de datos en un origen de datos
description: Describe la forma de ejecutar comandos o procedimientos almacenados que modifican datos en una base de datos.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771487"
---
# <a name="updating-data-in-a-data-source"></a>Actualización de datos en un origen de datos

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Las instrucciones SQL que modifican datos (por ejemplo INSERT, UPDATE o DELETE) no devuelven ninguna fila. De la misma forma, muchos procedimientos almacenados realizan alguna acción pero no devuelven filas. Para ejecutar comandos que no devuelvan filas, cree un objeto **Command** con el comando SQL adecuado y otro **Connection**, incluidos las propiedades de **Parameters** necesarias. Ejecute el comando con el método <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> del objeto <xref:Microsoft.Data.SqlClient.SqlCommand>.

> [!NOTE]
> El método **ExecuteNonQuery** devuelve un entero que representa el número de filas que se ven afectadas por la instrucción o por el procedimiento almacenado que se haya ejecutado. Si se ejecutan varias instrucciones, el valor devuelto es la suma de los registros afectados por todas las instrucciones ejecutadas.

## <a name="example"></a>Ejemplo

En el ejemplo de código siguiente se ejecuta una instrucción INSERT para insertar un registro en una base de datos mediante el método **ExecuteNonQuery**.
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

En el ejemplo de código siguiente se ejecuta el procedimiento almacenado que se creó con el ejemplo de código de la sección [Realización de operaciones de catálogo](perform-catalog-operations.md). El procedimiento almacenado no devuelve ninguna fila, por lo que se utiliza el método **ExecuteNonQuery**, aunque el procedimiento almacenado reciba un parámetro de entrada y devuelva un parámetro de salida y un valor devuelto.

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>Vea también

- [Uso de comandos para modificar datos](use-commands-to-modify-data.md)
- [Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md)
- [Comandos y parámetros](commands-parameters.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
