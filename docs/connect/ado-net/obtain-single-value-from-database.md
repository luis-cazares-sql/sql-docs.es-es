---
title: Obtención de un valor único de una base de datos
description: Obtenga información sobre cómo devolver un valor único en ADO.NET. Este código de ejemplo devuelve el valor de la columna de identidad para un registro insertado.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: b38526cd-a62a-48cb-822a-e91dfa68e02d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3937c1d4617f3cecb9012d82d590f75634ab1043
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771499"
---
# <a name="obtaining-a-single-value-from-a-database"></a>Obtención de un valor único de una base de datos

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En ocasiones se debe devolver información de bases de datos consistente en un único valor, en lugar de una tabla o un flujo de datos. Por ejemplo, puede que desee devolver el resultado de una función de agregado como COUNT(\*), SUM(Price) o AVG(Quantity). El objeto **Command** permite devolver valores únicos mediante el método **ExecuteScalar**. El método **ExecuteScalar** devuelve como valor escalar el valor correspondiente a la primera columna de la primera fila del conjunto de resultados.

## <a name="example"></a>Ejemplo

El ejemplo de código siguiente inserta un valor nuevo en la base de datos utilizando <xref:Microsoft.Data.SqlClient.SqlCommand>. El método <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalar%2A> se utiliza para devolver el valor de columna de identidad para el registro insertado.

[!code-csharp[DataWorks SqlCommand.ExecuteScalar#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteScalar_Return_Id.cs#1)]

## <a name="see-also"></a>Vea también

- [Comandos y parámetros](commands-parameters.md)
- [Ejecución de un comando](execute-command.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
