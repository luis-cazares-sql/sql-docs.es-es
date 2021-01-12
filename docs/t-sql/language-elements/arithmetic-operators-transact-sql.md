---
description: Operadores aritméticos (Transact-SQL)
title: Operadores aritméticos (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], arithmetic
- arithmetic operators
- math operations [Transact-SQL]
ms.assetid: a41b92a5-1061-4e4d-bb3b-a180b73c88fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1b2743817df1bcc49f9f42e0fe3b8b608f9a0fa1
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101023"
---
# <a name="arithmetic-operators-transact-sql"></a>Operadores aritméticos (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Los operadores aritméticos ejecutan operaciones matemáticas con dos expresiones de uno o varios tipos de datos. Se ejecutan en la categoría de tipos de datos numéricos. Para obtener más información sobre las categorías de tipos de datos, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  
  
|Operator|Significado|  
|--------------|-------------|  
|[+ (Sumar)](../../t-sql/language-elements/add-transact-sql.md)|Suma|  
|[- (Restar)](../../t-sql/language-elements/subtract-transact-sql.md)|Resta|  
|[* (Multiplicar)](../../t-sql/language-elements/multiply-transact-sql.md)|Multiplicación|  
|[/ (Dividir)](../../t-sql/language-elements/divide-transact-sql.md)|División|  
|[% (Módulo)](../../t-sql/language-elements/modulo-transact-sql.md)|Devuelve el resto entero de una división. Por ejemplo, 12 % 5 = 2 porque el resto de 12 dividido entre 5 es 2.|  
  
También se pueden usar los operadores más (+) y menos (-) para ejecutar operaciones aritméticas sobre valores **datetime** y **smalldatetime**.  
  
Para obtener más información sobre la precisión y la escala del resultado de una operación aritmética, vea [Precisión, escala y longitud &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
[Funciones matemáticas &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
[Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
