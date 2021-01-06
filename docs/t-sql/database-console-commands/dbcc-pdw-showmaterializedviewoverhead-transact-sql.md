---
description: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
title: DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)
ms.custom: seo-dt-2019
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 87f02544512f4cd33385242101204e3d30c81ffe
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643950"
---
# <a name="dbcc-pdw_showmaterializedviewoverhead-transact-sql"></a>DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD (Transact-SQL)  

[!INCLUDE [asa](../../includes/applies-to-version/asa.md)]

Muestra el número de cambios incrementales en las tablas base que se mantienen para las vistas materializadas en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. La proporción de sobrecarga se calcula como TOTAL_ROWS / MAX (1, BASE_VIEW_ROWS).

![Icono de vínculo a temas](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis

```syntaxsql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( " [ schema_name .] materialized_view_name  " )
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos

 *schema_name*     
 Es el nombre del esquema al que pertenece la vista.

*materialized_view_name*   
Es el nombre de la vista materializada.

## <a name="remarks"></a>Comentarios

Para mantener las vistas materializadas actualizadas con cambios en los datos de las tablas base, el motor de almacenamiento de datos agrega filas de seguimiento a cada vista afectada para reflejar los cambios. La selección a partir de una vista materializada incluye el análisis del índice de almacén de columnas agrupado de la vista y la aplicación de cualquier cambio incremental.Las filas de seguimiento (TOTAL_ROWS-BASE_VIEW_ROWS) no se eliminarán hasta que los usuarios vuelvan a generar la vista materializada.  

El valor overhead_ratio se calcula como TOTAL_ROWS/MAX(1, BASE_VIEW_ROWS).  Si es alto, el rendimiento de SELECT se degradará.Los usuarios pueden volver a generar la vista materializada para reducir su proporción de sobrecarga.

## <a name="permissions"></a>Permisos  
  
Necesita el permiso VIEW DATABASE STATE.  

## <a name="examples"></a>Ejemplos  

### <a name="a-this-example-returns-the-overhead-ratio-of-a-materialized-view"></a>A. En este ejemplo se devuelve la proporción de sobrecarga de una vista materializada.

```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ( "dbo.MyIndexedView" )
```

Salida:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|1234|1|3 |3.0 |

</br>

### <a name="b-this-example-shows-how-the-materialized-view-overhead-increases-as-data-changes-in-base-tables"></a>B. Este ejemplo se muestra cómo aumenta la sobrecarga de la vista materializada a medida que cambian los datos en las tablas base

Creación de una tabla
```sql
CREATE TABLE t1 (c1 INT NOT NULL, c2 INT NOT NULL, c3 INT NOT NULL)
```
Inserción de cinco filas en t1
```sql
INSERT INTO t1 VALUES (1, 1, 1)
INSERT INTO t1 VALUES (2, 2, 2) 
INSERT INTO t1 VALUES (3, 3, 3) 
INSERT INTO t1 VALUES (4, 4, 4) 
INSERT INTO t1 VALUES (5, 5, 5) 
```
Creación de vistas materializadas MV1
```sql
CREATE MATERIALIZED VIEW MV1 
WITH (DISTRIBUTION = HASH(c1))  
AS
SELECT c1, COUNT(*) total_number 
FROM dbo.t1 WHERE c1 < 3
GROUP BY c1  
```
La selección desde la vista materializada devuelve dos filas.

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

Compruebe la sobrecarga de la vista materializada antes de realizar cualquier cambio en los datos de la tabla base.
```sql
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Salida:

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1,00000000000000000 |

Actualice la tabla base.  Esta consulta actualiza 100 veces al mismo valor la misma columna de la misma fila.  El contenido de la vista materializada no cambia.
```sql
DECLARE @p INT
SELECT @p = 1
WHILE (@p < 101)
BEGIN
UPDATE t1 SET c1 = 1 WHERE c1 = 1
SELECT @p = @p+1
END  
```

La selección de la vista materializada devuelve el mismo resultado que antes.  

|c1|total_number|
|--------|--------| 
|1|1| 
|2|1|

A continuación se muestra el resultado de DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1").  Se agregan 100 filas a la vista materializada (total_row-base_view_rows) y aumenta su valor overhead_ratio. 

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|102 |51,00000000000000000 |

Después de volver a generar la vista materializada, se eliminan todas las filas de seguimiento de los cambios de datos incrementales y se reduce la proporción de sobrecarga de la vista.  

```sql
ALTER MATERIALIZED VIEW dbo.MV1 REBUILD
GO
DBCC PDW_SHOWMATERIALIZEDVIEWOVERHEAD ("dbo.mv1")
```
Resultados

|OBJECT_ID|BASE_VIEW_ROWS|TOTAL_ROWS|OVERHEAD_RATIO|
|--------|--------|--------|--------|  
|587149137|2|2 |1,00000000000000000 |

## <a name="see-also"></a>Vea también

[Optimización del rendimiento con vista materializada](/azure/sql-data-warehouse/performance-tuning-materialized-views)   
[CREATE MATERIALIZED VIEW AS SELECT &#40;Transact-SQL&#41;](../statements/create-materialized-view-as-select-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[ALTER MATERIALIZED VIEW &#40;Transact-SQL&#41;](../statements/alter-materialized-view-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[EXPLAIN &#40;Transact-SQL&#41;](../queries/explain-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_column_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-column-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_distribution_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-distribution-properties-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[sys.pdw_materialized_view_mappings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-pdw-materialized-view-mappings-transact-sql.md?view=azure-sqldw-latest&preserve-view=true)   
[Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
[Vistas del sistema admitidas en Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-system-views)   
[Instrucciones T-SQL admitidas en Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-statements)