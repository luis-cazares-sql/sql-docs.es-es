---
title: Uso de comandos para modificar datos
description: Describe cómo se utiliza un proveedor de datos para ejecutar procedimientos almacenados o instrucciones de lenguaje de definición de datos (DDL).
ms.date: 11/25/2020
ms.assetid: f4160389-b9ff-4b74-b655-437c76dcd586
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 98127e41b5b07c38030ef27214c9c92bf7c4b4be
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2020
ms.locfileid: "96761483"
---
# <a name="using-commands-to-modify-data"></a>Uso de comandos para modificar datos

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Con el proveedor de datos SqlClient de Microsoft para SQL Server, puede ejecutar procedimientos almacenados o instrucciones de lenguaje de definición de datos (por ejemplo, CREATE TABLE y ALTER COLUMN) para manipular el esquema en una base de datos o un catálogo. Estos comandos no devuelven filas de la forma en que lo haría una consulta, por lo que el objeto <xref:Microsoft.Data.SqlClient.SqlCommand> proporciona <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> para procesarlos.

Además de utilizar **ExecuteNonQuery** para modificar esquemas, este método se puede usar también para procesar instrucciones SQL que modifican datos sin devolver ninguna fila, como INSERT, UPDATE y DELETE.

Aunque el método **ExecuteNonQuery** no devuelve ninguna fila, mediante la colección **Parameters** del objeto **Command** se pueden pasar y devolver parámetros de entrada y de salida, así como valores devueltos.

## <a name="in-this-section"></a>En esta sección

[Actualización de datos en un origen de datos](update-data-inside-data-source.md)  
Describe la forma de ejecutar comandos o procedimientos almacenados que modifican datos en una base de datos.

[Realización de operaciones de catálogo](perform-catalog-operations.md)  
Describe la forma de ejecutar comandos que modifican esquemas de la base de datos.

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Comandos y parámetros](commands-parameters.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
