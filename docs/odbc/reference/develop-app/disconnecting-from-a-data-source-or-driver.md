---
description: Desconectarse de datos de un origen o el controlador
title: Desconectando de un origen de datos o un controlador | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- disconnecting from driver [ODBC]
- data sources [ODBC], disconnecting
- disconnecting from data source [ODBC]
- connecting to data source [ODBC], disconnecting
- connecting to driver [ODBC], disconnecting
- ODBC drivers [ODBC], disconnecting
ms.assetid: 83dbf0bf-b400-41fb-8537-9b016050dc3c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fc14ca0ebf29a2ab203a4408db4b5681ad667497
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88476697"
---
# <a name="disconnecting-from-a-data-source-or-driver"></a>Desconectarse de datos de un origen o el controlador
Cuando una aplicación ha terminado de usar un origen de datos, llama a **SQLDisconnect**. **SQLDisconnect** libera todas las instrucciones que se asignan en la conexión y desconecta el controlador del origen de datos. Devuelve un error si una transacción está en curso.  
  
 Después de desconectarse, la aplicación puede llamar a **SQLFreeHandle** para liberar la conexión. Después de liberar la conexión, se trata de un error de programación de la aplicación para utilizar el identificador de la conexión en una llamada a una función ODBC. Si lo hace, tendrá consecuencias indefinidas pero probablemente graves. Cuando se llama a **SQLFreeHandle** , el controlador libera la estructura utilizada para almacenar información sobre la conexión.  
  
 La aplicación también puede volver a usar la conexión, ya sea para conectarse a un origen de datos diferente o volver a conectarse al mismo origen de datos. La decisión de seguir conectado, en lugar de desconectarse y volver a conectarse más tarde, requiere que el escritor de la aplicación tenga en cuenta los costos relativos de cada opción. la conexión a un origen de datos y la conexión restante puede ser relativamente costosa según el medio de conexión. Al realizar un equilibrio correcto, la aplicación también debe realizar suposiciones sobre la probabilidad y el tiempo de operaciones posteriores en el mismo origen de datos.
