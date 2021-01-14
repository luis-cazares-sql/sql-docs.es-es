---
description: sys.dm_exec_valid_use_hints (Transact-SQL)
title: sys.dm_exec_valid_use_hints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_exec_valid_use_hints
- sys.dm_exec_valid_use_hints_TSQL
- dm_exec_valid_use_hints
- dm_exec_valid_use_hints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_valid_use_hints management view
ms.assetid: 65d50589-39c2-4046-92b6-0c4587d8c593
author: pmasl
ms.author: pelopes
ms.openlocfilehash: de99c9372846525349df3f9f312222e322bfe751
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171397"
---
# <a name="sysdm_exec_valid_use_hints-transact-sql"></a>sys.dm_exec_valid_use_hints (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

Devuelve los nombres de sugerencia de [uso](../../t-sql/queries/hints-transact-sql-query.md#use_hint) admitidos. Muestra un nombre de sugerencia por fila.  
  
Use esta DMV para ver la lista de todas las sugerencias admitidas en la notación usar sugerencia.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre de la sugerencia.|

Vea [sugerencias de consulta](../../t-sql/queries/hints-transact-sql-query.md#use_hint) para obtener descripciones de cada sugerencia.

Introducido en [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)] SP1.
  
## <a name="see-also"></a>Consulte también  
    
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  

