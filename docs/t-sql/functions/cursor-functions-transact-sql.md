---
description: Funciones del cursor (Transact-SQL)
title: Funciones del cursor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], cursors
- cursor functions
ms.assetid: 7d9daa10-4c50-4212-9400-42120222b2b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 3433a39caeb0a1e951eba18ccdd3d1658c108252
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98087844"
---
# <a name="cursor-functions-transact-sql"></a>Funciones del cursor (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Estas funciones escalares devuelven información sobre los cursores:
  
- [@@CURSOR_ROWS](../../t-sql/functions/cursor-rows-transact-sql.md)
- [@@FETCH_STATUS](../../t-sql/functions/fetch-status-transact-sql.md)
- [CURSOR_STATUS](../../t-sql/functions/cursor-status-transact-sql.md)
  
Todas las funciones del cursor son no deterministas. En otras palabras, estas funciones no siempre devuelven el mismo resultado cada vez que se ejecutan, incluso con el mismo conjunto de valores de entrada. Vea [Funciones deterministas y no deterministas](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md) para obtener más información sobre el determinismo de las funciones.
  
## <a name="see-also"></a>Consulte también

[Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)
