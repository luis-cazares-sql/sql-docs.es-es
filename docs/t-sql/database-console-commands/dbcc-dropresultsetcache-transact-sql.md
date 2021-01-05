---
description: DBCC DROPRESULTSETCACHE  (Transact-SQL)
title: DBCC DROPRESULTSETCACHE (Transact-SQL)
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 22ebd9f7d52fd15d9dbd8479962312387857b233
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641261"
---
# <a name="dbcc-dropresultsetcache--transact-sql"></a>DBCC DROPRESULTSETCACHE  (Transact-SQL)

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Quita todas las entradas de la caché del conjunto de resultados de una base de datos [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] de Azure.
  
![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC DROPRESULTSETCACHE
[;]  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="permissions"></a>Permisos

Requiere pertenencia al rol fijo de servidor DB_OWNER.

## <a name="remarks"></a>Observaciones

- Este comando vacía la caché del conjunto de resultados para todas las consultas.  

- Al desactivar la característica de caché del conjunto de resultados para una base de datos, también se eliminan todos los resultados almacenados en caché.  

- Al pausar una base de datos habilitada con almacenamiento en caché del conjunto de resultados, no se eliminarán los resultados almacenados en caché.  

## <a name="see-also"></a>Consulte también

- [Ajuste del rendimiento con la copia en caché del conjunto de resultados](/azure/sql-data-warehouse/performance-tuning-result-set-caching)</br>
- [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../statements/alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)</br>
- [ALTER DATABASE &#40;Transact-SQL&#41;](../statements/alter-database-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)</br>
- [SET RESULT SET CACHING &#40;Transact-SQL&#41;](../statements/set-result-set-caching-transact-sql.md)</br>
- [DBCC SHOWRESULTCACHESPACEUSED &#40;Transact-SQL&#41;](./dbcc-showresultcachespaceused-transact-sql.md)