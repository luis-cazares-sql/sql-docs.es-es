---
description: Tablas temporales con control de versiones del sistema con tablas con optimización para memoria
title: Tablas temporales con control de versiones del sistema con tablas con optimización para memoria | Microsoft Docs
ms.custom: ''
ms.date: 07/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 23274522-e5cf-4095-bed8-bf986d6342e0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 05ed5ec0d34d2a1a612b76c0117d0bf66675ad7a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171897"
---
# <a name="system-versioned-temporal-tables-with-memory-optimized-tables"></a>Tablas temporales con control de versiones del sistema con tablas con optimización para memoria


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


Las tablas temporales con control de versiones del sistema para [tablas con optimización para memoria](../in-memory-oltp/sample-database-for-in-memory-oltp.md) están diseñadas con el objetivo de ofrecer una solución rentable para escenarios donde se requieren [auditorías de los datos y análisis puntales](./temporal-table-usage-scenarios.md) , además de los datos recopilados con cargas de trabajo de OLTP en memoria. Ofrecen gran rendimiento transaccional, simultaneidad sin bloqueo y, al mismo tiempo, la capacidad de almacenar grandes cantidades de datos de historial en los que se pueden realizar consultas fácilmente.

## <a name="overview"></a>Información general

Las tablas temporales con control de versiones del sistema mantienen automáticamente un historial completo de los cambios de datos y exponen prácticas extensiones de Transact-SQL para los análisis puntuales. En un escenario típico, se conserva el historial de datos durante un periodo muy largo (varios meses, incluso años), aunque no se realicen consultas en él con regularidad.

Puede que se exijan auditorías de datos y análisis basados en el tiempo en distintos entornos, especialmente en sistemas OLTP que procesen un número de solicitudes muy elevado y donde se utilice la tecnología OLTP en memoria. Sin embargo, el uso de tablas optimizadas para memoria en escenarios temporales resulta difícil porque la enorme cantidad de datos históricos generados suele superar el límite de memoria RAM disponible. Al mismo tiempo, la utilización de RAM para almacenar datos históricos de solo lectura a los que se accede como menos frecuencia a medida que van adquiriendo más antigüedad no constituye una solución óptima.

Las tablas temporales con control de versiones del sistema para tablas optimizadas para memoria ofrecen un alto rendimiento transaccional, simultaneidad sin bloqueo y, al mismo tiempo, la capacidad de almacenar granes cantidades de datos de historial mediante tablas en memoria para guardar los datos actuales (la tabla temporal) y tablas basadas en disco para los históricos. Se minimiza el impacto en las operaciones DML mediante el uso de una tabla de almacenamiento provisional interna optimizada para memoria y generada automáticamente que almacena el historial reciente y permite que las DML se ejecuten desde el código compilado de manera nativa.

El siguiente diagrama muestra esta arquitectura. ![Arquitectura en memoria temporal](../../relational-databases/tables/media/temporal-in-memory-architecture.png "Arquitectura en memoria temporal")

## <a name="implementation-details"></a>Detalles de la implementación

Los siguientes datos sobre las tablas temporales con control de versiones del sistema con tablas optimizadas para memoria son consideraciones que debe tener en cuenta a la hora de crear una tabla con control de versiones del sistema y optimizada para memoria. Para ver las opciones de sintaxis y obtener un ejemplo, consulte [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md).

- Solo se puede usar el control de versiones del sistema en tablas optimizadas para memoria duraderas (**DURABILITY = SCHEMA_AND_DATA**).
- La tabla de historial para la tabla con control de versiones del sistema optimizado para memoria debe ser basada en disco, al margen de si la creó el usuario final o el sistema.
- Las consultas que afectan solo a la tabla actual (en memoria) pueden usarse en [módulos T-SQL compilados de forma nativa](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md). No se admiten consultas temporales con la cláusula FOR SYSTEM TIME en módulos compilados de forma nativa. Se admite el uso de la cláusula FOR SYSTEM TIME con tablas optimizadas para memoria en consultas ad hoc y módulos no nativos.
- Cuando **SYSTEM_VERSIONING = ON**, se crea automáticamente una tabla de almacenamiento provisional interna optimizada para memoria a fin de aceptar los cambios con control de versiones del sistema más recientes, que son consecuencia de operaciones de actualización y eliminación en la tabla actual optimizada para memoria.
- La tarea de vaciado de datos asincrónica mueve con regularidad los datos de la tabla de almacenamiento provisional interna optimizada para memoria a la tabla de historial basada en disco. Este mecanismo de vaciado de datos tiene el objetivo de mantener los búferes de memoria interna a menos del 10 % del consumo de memoria de sus objetos primarios. Puede efectuar un seguimiento del consumo de memoria total de la tabla temporal con control de versiones del sistema y optimizado para memoria si realiza una consulta a [sys.dm_db_xtp_memory_consumers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-memory-consumers-transact-sql.md) y resume los datos de la tabla de almacenamiento provisional interna optimizada para memoria y la tabla temporal actual.
- Puede forzar un vaciado de datos invocando [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md).
- Cuando **SYSTEM_VERSIONING = OFF** o cuando se modifica el esquema de una tabla con control de versiones del sistema agregando, quitando o alterando columnas, todo el contenido del búfer de almacenamiento provisional interno se mueve a la tabla de historial basada en disco.
- La consulta de los datos de historial se produce en el nivel de aislamiento SNAPSHOT y siempre devuelve una unión entre el búfer de almacenamiento provisional en memoria y la tabla basada en disco sin duplicados.
- Las operaciones **ALTER TABLE** que cambian el esquema de tabla internamente deben realizar un vaciado de datos, lo que podría prolongar la duración de la operación.

## <a name="the-internal-memory-optimized-staging-table"></a>Tabla de almacenamiento provisional interna con optimización para memoria

La tabla de almacenamiento provisional interna optimizada para memoria es un objeto interno que crea el sistema con la finalidad de optimizar las operaciones DML.

- El nombre de tabla se genera en el formato siguiente: **Memory_Optimized_History_Table_<object_id>** , donde *<object_id>* es el identificador de la tabla temporal actual.
- La tabla replica el esquema de la tabla temporal actual, más una columna BIGINT. Esta columna adicional garantiza la exclusividad de las filas trasladadas al búfer interno de historial.
- La columna adicional tiene el formato de nombre siguiente: **Change_ID[_< suffix>]** , donde *_\<suffix>* se agrega de manera opcional en caso de que la tabla ya cuente con una columna *Change_ID*.
- El tamaño máximo de fila de una tabla con control de versiones del sistema y optimizada para memoria se reduce en 8 bytes debido a la columna BIGINT adicional de la tabla de almacenamiento provisional. Ahora, el nuevo máximo es 8052 bytes.
- La tabla de almacenamiento provisional interna optimizada para memoria no se representa en el Explorador de objetos de SQL Server Management Studio.
- En [sys.internal_tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md), es posible encontrar metadatos sobre esta tabla, así como su conexión con la tabla temporal actual.

## <a name="the-data-flush-task"></a>La tarea de vaciado de datos

El vaciado de datos es una tarea que se activa con regularidad y que comprueba si la tabla optimizada para memoria cumple una condición basada en el tamaño de la memoria para el movimiento de los datos. El movimiento de los datos empieza cuando el consumo de memoria de la tabla de almacenamiento provisional interna alcanza el 8 % del de la tabla temporal.

La tarea de vaciado de datos se activa periódicamente con una programación que varía según la carga de trabajo existente. Con una gran carga de trabajo, tendrá una frecuencia de cada 5 segundos y con una carga de trabajo ligera, de cada 1 minuto. Se genera un subproceso para cada tabla de almacenamiento provisional optimizada para memoria que necesite limpieza.

El vaciado de datos elimina todos los registros del búfer interno en memoria que sean posteriores a la transacción más antigua en ejecución en ese momento para mover dichos registros a la tabla de historial basada en disco.

Puede forzar un vaciado de los datos si invoca [sp_xtp_flush_temporal_history](../../relational-databases/system-stored-procedures/temporal-table-sp-xtp-flush-temporal-history.md) y especifica el esquema y el nombre de tabla: **sys.sp_xtp_flush_temporal_history \@nombre_esquema, \@nombre_objeto**. Con este comando ejecutado por el usuario, se invoca el mismo proceso de movimiento de datos que cuando el sistema invoca la tarea de vaciado de datos según la programación interna.

## <a name="see-also"></a>Consulte también

- [Tablas temporales](../../relational-databases/tables/temporal-tables.md)
- [Introducción a las tablas temporales con versión del sistema](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [Escenarios de uso de tablas temporales](../../relational-databases/tables/temporal-table-usage-scenarios.md)
- [Comprobaciones de coherencia del sistema de la tabla temporal](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [Creación de particiones con tablas temporales](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [Limitaciones y consideraciones de las tablas temporales](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [Seguridad de la tabla temporal](../../relational-databases/tables/temporal-table-security.md)
- [Administración de la retención de datos históricos en las tablas temporales con control de versiones del sistema](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [Funciones y vistas de metadatos de la tabla temporal](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
