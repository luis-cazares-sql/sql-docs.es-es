---
title: Ajuste automático | Microsoft Docs
description: Más información sobre el ajuste automático en SQL Server y Azure SQL Database
ms.custom: fasttrack-edit
ms.date: 09/28/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- performance tuning [SQL Server]
- automatic tuning
- aprc
- automatic plan regression correction
- last known good plan
ms.assetid: ''
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9c01a9e02576d666c39df13dc6e7e01f6d622a7d
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98169877"
---
# <a name="automatic-tuning"></a>Ajuste automático
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

El ajuste automático es una característica de base de datos que proporciona información de los posibles problemas de rendimiento de las consultas, recomienda soluciones y corrige automáticamente los problemas identificados.

El ajuste automático, introducido en [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , le notifica cada vez que se detecta un posible problema de rendimiento y le permite aplicar las acciones correctivas o permite que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] corrija automáticamente los problemas de rendimiento. El ajuste automático [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le permite identificar y corregir problemas de rendimiento causados por **regresiones de elección del plan de ejecución de consultas**. El ajuste automático en [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] también crea los índices necesarios y quita los índices no usados. Para obtener más información sobre los planes de ejecución de consultas, vea [planes de ejecución](../../relational-databases/performance/execution-plans.md).

[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Supervisa las consultas que se ejecutan en la base de datos y mejora automáticamente el rendimiento de la carga de trabajo. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]Tiene un mecanismo integrado de inteligencia que puede ajustar y mejorar automáticamente el rendimiento de las consultas mediante la adaptación dinámica de la base de datos a la carga de trabajo. Hay dos características de ajuste automático que están disponibles:

-   La **corrección automática del plan** identifica los planes de ejecución de consultas problemáticos, como una [sensibilidad de parámetros o](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) problemas de examen de parámetros, y corrige los problemas de rendimiento relacionados con el plan de ejecución de consulta forzando el último plan bueno conocido antes de que se produjera la regresión. **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

-   La **Administración automática de índices** identifica los índices que se deben agregar a la base de datos y los índices que se deben quitar. **Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

## <a name="why-automatic-tuning"></a>¿Por qué el ajuste automático?

Tres de las principales tareas de la administración de bases de datos clásicas son la supervisión de la carga de trabajo, la identificación de [!INCLUDE[tsql_md](../../includes/tsql-md.md)] consultas críticas e identificación de índices que se deben agregar para mejorar el rendimiento, o índices que rara vez se usan y que se pueden quitar para mejorar el rendimiento. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]Proporciona información detallada sobre las consultas y los índices que necesita supervisar. Sin embargo, supervisar constantemente una base de datos es una tarea ardua y tediosa, sobre todo cuando se trabaja con muchas bases de datos. La administración de un gran número de bases de datos podría ser imposible de realizar eficazmente. En lugar de supervisar y ajustar la base de datos manualmente, puede considerar la posibilidad de delegar algunas de las acciones de supervisión y optimización en la [!INCLUDE[ssde_md](../../includes/ssde_md.md)] característica de ajuste automático de uso.

### <a name="how-does-automatic-tuning-work"></a>¿Cómo funciona el ajuste automático?

El ajuste automático es un proceso de supervisión y análisis continuo que aprende constantemente sobre las características de la carga de trabajo e identifica posibles problemas y mejoras.

![Proceso de ajuste automático](./media/tuning-process.png)

Este proceso permite a la base de datos adaptarse dinámicamente a la carga de trabajo al encontrar qué índices y planes pueden mejorar el rendimiento de las cargas de trabajo y qué índices afectan a las cargas de trabajo. En función de estos hallazgos, el ajuste automático aplica acciones de ajuste que mejoran el rendimiento de la carga de trabajo. Además, el ajuste automático supervisa continuamente el rendimiento de la base de datos después de implementar los cambios para asegurarse de que mejora el rendimiento de la carga de trabajo. Cualquier acción que no mejore el rendimiento se revierte automáticamente. Este proceso de comprobación es una característica clave que garantiza que cualquier cambio realizado por el ajuste automático no reduce el rendimiento general de la carga de trabajo.

## <a name="automatic-plan-correction"></a>Corrección automática del plan

La corrección automática del plan es una característica de ajuste automático que identifica la **regresión de elección del plan de ejecución** y corrige automáticamente el problema forzando el último plan bueno conocido. Para obtener más información acerca de los planes de ejecución de consultas y el optimizador de consultas, consulte la [Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md).

> [!IMPORTANT]
> La corrección automática del plan depende de Almacén de consultas habilitada en la base de datos para el seguimiento de la carga de trabajo. 

### <a name="what-is-execution-plan-choice-regression"></a>¿Qué es la regresión de elección del plan de ejecución?

[!INCLUDE[ssdenoversion_md](../../includes/ssdenoversion_md.md)]Puede utilizar planes de ejecución diferentes para ejecutar las [!INCLUDE[tsql_md](../../includes/tsql-md.md)] consultas. Los planes de consulta dependen de las estadísticas, los índices y otros factores. El plan óptimo que se debe usar para ejecutar una [!INCLUDE[tsql_md](../../includes/tsql-md.md)] consulta puede cambiar con el tiempo en función de los cambios en estos factores. En algunos casos, el nuevo plan podría no ser mejor que el anterior y el nuevo plan podría provocar una regresión del rendimiento, como un problema relacionado con la [distinción de parámetros o el examen de parámetros](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing) . 

 ![Regresión de elección del plan de ejecución de consulta](media/plan-choice-regression.png "Regresión de elección del plan de ejecución de consulta") 

Siempre que observe que se ha producido una regresión de elección del plan, debería encontrar un plan bueno anterior y forzarlo para que se use en lugar del actual. Esto puede hacerse mediante el `sp_query_store_force_plan` procedimiento. [!INCLUDE[ssde_md](../../includes/ssde_md.md)]En [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] proporciona información sobre los planes con regresión y las acciones correctivas recomendadas. Además, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] le permite automatizar completamente este proceso y dejar que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] solucione cualquier problema encontrado relacionado con el cambio del plan.

> [!IMPORTANT]
> La corrección automática del plan debe usarse en el ámbito de una actualización del nivel de compatibilidad de la base de datos, después de haber capturado una línea de base, para mitigar automáticamente los riesgos de actualización de la carga de trabajo. Para obtener más información acerca de este caso de uso, consulte [mantener la estabilidad del rendimiento durante la actualización a más reciente SQL Server](../../relational-databases/performance/query-store-usage-scenarios.md#CEUpgrade). 

### <a name="automatic-plan-choice-correction"></a>Corrección automática de la selección de plan

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Puede cambiar automáticamente al último plan bueno conocido cada vez que se detecta una regresión de elección del plan.

![Corrección de elección del plan de ejecución de consulta](media/force-last-good-plan.png "Corrección de elección del plan de ejecución de consulta") 

[!INCLUDE[ssde_md](../../includes/ssde_md.md)]Detecta automáticamente cualquier posible regresión de elección del plan, incluido el plan que se debe usar en lugar del plan equivocado. Cuando el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] aplica el último plan bueno conocido antes de que se produjera la regresión, supervisa automáticamente el rendimiento del plan forzado. Si el plan forzado no es mejor que el plan con regresión, el nuevo plan no se aplicará y [!INCLUDE[ssde_md](../../includes/ssde_md.md)] se compilará un nuevo plan. Si [!INCLUDE[ssde_md](../../includes/ssde_md.md)] comprueba que el plan forzado es mejor que el plan con regresión, se conservará el plan forzado. Se conservará hasta que se produzca una nueva compilación (por ejemplo, en la siguiente actualización de estadísticas o en el cambio de esquema). Para obtener más información sobre cómo forzar el plan y los tipos de planes que se pueden forzar, vea [limitaciones para forzar el plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md#plan-forcing-limitations).

> [!NOTE]
> Si la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia se reinicia antes de comprobar la acción de forzar un plan, ese plan se desobligará automáticamente. De lo contrario, el forzado del plan se conservará en los [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reinicios.

### <a name="enabling-automatic-plan-choice-correction"></a>Habilitar la corrección automática de la elección del plan

Puede habilitar el ajuste automático por base de datos y especificar que se debe forzar el último plan bueno cada vez que se detecta una regresión de cambios en el plan. El ajuste automático se habilita con el siguiente comando:

```sql   
ALTER DATABASE <yourDatabase>
SET AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = ON ); 
```

Una vez habilitada esta opción, el aplicará [!INCLUDE[ssde_md](../../includes/ssde_md.md)] automáticamente las recomendaciones en las que la ganancia de CPU estimada sea superior a 10 segundos, o el número de errores en el nuevo plan sea mayor que el número de errores del plan recomendado, y comprobará que el plan forzado es mejor que el actual.

### <a name="alternative---manual-plan-choice-correction"></a>Alternativa: corrección manual de elección del plan

Sin el ajuste automático, los usuarios deben supervisar periódicamente el sistema y buscar las consultas con regresión. Si se ha realizado la regresión de algún plan, el usuario debe encontrar un plan bueno anterior y forzarlo en lugar del actual mediante el uso del `sp_query_store_force_plan` procedimiento. El procedimiento recomendado sería forzar el último plan bueno conocido porque es posible que los planes más antiguos no sean válidos debido a cambios en los índices o estadísticas. El usuario que fuerza el último plan bueno conocido debe supervisar el rendimiento de la consulta que se ejecuta con el plan forzado y comprobar que el plan forzado funciona según lo previsto. En función de los resultados de la supervisión y el análisis, se debe forzar el plan o el usuario debe encontrar otra manera de optimizar la consulta, como volver a escribirla. Los planes forzados manualmente no se deben forzar de manera permanente, ya que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] debe ser capaz de aplicar los planes óptimos. El usuario o DBA no debe forzar el plan con el `sp_query_store_unforce_plan` procedimiento y dejar que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] encuentre el plan óptimo. 

> [!TIP]
> Como alternativa, puede usar las **consultas con planes forzados** almacén de consultas vista para buscar y no aplicar planes.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona todas las vistas y los procedimientos necesarios para supervisar el rendimiento y solucionar problemas en Almacén de consultas.

En [!INCLUDE[sssql15-md](../../includes/sssql16-md.md)] , puede buscar regresiones de elección del plan mediante almacén de consultas vistas del sistema. A partir de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)] , [!INCLUDE[ssde_md](../../includes/ssde_md.md)] detecta y muestra las posibles regresiones de elección del plan y las acciones recomendadas que se deben aplicar en el [sys.dm_db_tuning_recommendations &#40;DMV&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md) . La DMV muestra información sobre el problema, la importancia del problema y detalles como la consulta identificada, el identificador del plan con regresión, el ID. del plan que se usó como línea base para la comparación y la [!INCLUDE[tsql_md](../../includes/tsql-md.md)] instrucción que se puede ejecutar para corregir el problema.

| type | description | datetime | score | detalles | ... |
| --- | --- | --- | --- | --- | --- |
| `FORCE_LAST_GOOD_PLAN` | Tiempo de CPU cambiado de 4 MS a 14 MS | 3/17/2017 | 83 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |
| `FORCE_LAST_GOOD_PLAN` | Tiempo de CPU cambiado de 37 MS a 84 MS | 3/16/2017 | 26 | `queryId` `recommendedPlanId` `regressedPlanId` `T-SQL` |   |

Algunas columnas de esta vista se describen en la lista siguiente:
 - Tipo de la acción recomendada `FORCE_LAST_GOOD_PLAN` .
 - Descripción que contiene información sobre por qué [!INCLUDE[ssde_md](../../includes/ssde_md.md)] considera que este cambio de plan es una posible regresión de rendimiento.
 - Fecha y hora en que se detecta la posible regresión.
 - Puntuación de esta recomendación.
 - Detalles acerca de los problemas, como el identificador del plan detectado, el identificador del plan con regresión, el ID. del plan que se debe forzar para corregir el problema, el [!INCLUDE[tsql_md](../../includes/tsql-md.md)] script que se puede aplicar para corregir el problema, etc. Los detalles se almacenan en [formato JSON](../json/json-data-sql-server.md).

Use la siguiente consulta para obtener un script que corrija el problema e información adicional sobre la ganancia estimada:

```sql   
SELECT reason, score,
      script = JSON_VALUE(details, '$.implementationDetails.script'),
      planForceDetails.*,
      estimated_gain = (regressedPlanExecutionCount + recommendedPlanExecutionCount)
                  * (regressedPlanCpuTimeAverage - recommendedPlanCpuTimeAverage)/1000000,
      error_prone = IIF(regressedPlanErrorCount > recommendedPlanErrorCount, 'YES','NO')
FROM sys.dm_db_tuning_recommendations
CROSS APPLY OPENJSON (Details, '$.planForceDetails')
    WITH (  [query_id] int '$.queryId',
            regressedPlanId int '$.regressedPlanId',
            recommendedPlanId int '$.recommendedPlanId',
            regressedPlanErrorCount int,
            recommendedPlanErrorCount int,
            regressedPlanExecutionCount int,
            regressedPlanCpuTimeAverage float,
            recommendedPlanExecutionCount int,
            recommendedPlanCpuTimeAverage float
          ) AS planForceDetails;
```

[!INCLUDE[ssresult-md](../../includes/ssresult-md.md)]     

| reason | score | script | \_ID. de consulta | ID. del plan actual \_ | identificador de plan recomendado \_ | ganancia estimada \_ | propenso a errores \_
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Tiempo de CPU cambiado de 3 MS a 46 MS | 36 | \_ \_ Plan de fuerza del almacén de consultas de exec Sp \_ \_ 12, 17; | 12 | 28 | 17 | 11,59 | 0

La columna `estimated_gain` representa el número estimado de segundos que se guardarían si el plan recomendado se usara para la ejecución de la consulta en lugar del plan actual. Se debe forzar el plan recomendado en lugar del plan actual si la ganancia es superior a 10 segundos. Si hay más errores (por ejemplo, tiempos de espera o ejecuciones anuladas) en el plan actual que en el plan recomendado, la columna `error_prone` se establecería en el valor `YES` . Un plan propenso a errores es otro motivo por el que se debe forzar el plan recomendado en lugar de la actual.

Aunque [!INCLUDE[ssde_md](../../includes/ssde_md.md)] proporciona toda la información necesaria para identificar las regresiones de elección del plan, los problemas de rendimiento de la supervisión continua y la corrección pueden ser un proceso tedioso. El ajuste automático facilita este proceso.

> [!NOTE]
> Los datos de la `sys.dm_db_tuning_recommendations` DMV no se conservan entre los reinicios de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia.

## <a name="automatic-index-management"></a>Administración automática de índices

En [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] , la administración de índices es fácil porque [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] aprende sobre la carga de trabajo y garantiza que los datos siempre se indexan de forma óptima. El diseño adecuado de índices es fundamental para un rendimiento óptimo de la carga de trabajo y la administración automática de índices puede ayudar a optimizar los índices. La administración automática de índices puede corregir problemas de rendimiento en bases de datos indexadas incorrectamente o mantener y mejorar los índices en el esquema de base de datos existente. El ajuste automático en [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] realiza las siguientes acciones:

 - Identifica los índices que podrían mejorar el rendimiento de las [!INCLUDE[tsql_md](../../includes/tsql-md.md)] consultas que leen datos de las tablas.
 - Identifica índices redundantes o índices que no se usaron en un período de tiempo más largo que se podría quitar. La eliminación de índices innecesarios mejora el rendimiento de las consultas que actualizan los datos de las tablas.

### <a name="why-do-you-need-index-management"></a>¿Por qué se necesita la administración de índices?

Los índices aceleran algunas de las consultas que leen datos de las tablas, pero pueden ralentizar las consultas que actualizan los datos. Debe analizar cuidadosamente cuándo se debe crear un índice y qué columnas debe incluir en él. Es posible que algunos índices no sean necesarios pasado un tiempo. Por lo tanto, debe identificar y quitar periódicamente estos índices que no aportan ninguna ventaja. Si omite los índices no usados, el rendimiento de las consultas que actualizan los datos se reduciría sin ninguna ventaja en las consultas que leen los datos. Además, los índices no usados afectan al rendimiento general del sistema porque las actualizaciones adicionales necesitan tareas de registro innecesarias.

Para encontrar el conjunto óptimo de índices que mejore el rendimiento de las consultas que leen datos de las tablas y tenga un impacto mínimo en las actualizaciones, es posible que se requiera un análisis complejo y continuado.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] usa inteligencia integrada y reglas avanzadas que analizan las consultas, identifican los índices que serían óptimos para las cargas de trabajo actuales e identifican los índices que es posible que deban quitarse. Azure SQL Database garantiza que tiene un conjunto mínimo necesario de índices que optimizan las consultas que leen los datos, con un impacto mínimo en las demás consultas.

### <a name="automatic-index-management"></a>Administración automática de índices

Además de la detección, [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] puede aplicar automáticamente las recomendaciones identificadas. Si observa que las reglas integradas mejoran el rendimiento de la base de datos, puede permitir que [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] Administre automáticamente los índices.

Para habilitar el ajuste automático en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y permitir que la característica de ajuste automático administre totalmente la carga de trabajo, consulte [habilitación del ajuste automático en Azure SQL Database mediante Azure portal](/azure/sql-database/sql-database-automatic-tuning-enable).

Cuando [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] aplica una recomendación Create index o Drop index, supervisa automáticamente el rendimiento de las consultas que se ven afectadas por el índice. Solo se conservará el nuevo índice si se han mejorado los rendimientos de las consultas afectadas. El índice quitado se volverá a crear automáticamente si hay algunas consultas que se ejecutan más lentamente debido a la ausencia del índice.

### <a name="automatic-index-management-considerations"></a>Consideraciones sobre la administración automática de índices

Las acciones necesarias para crear los índices necesarios en [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] pueden consumir recursos y afectar al rendimiento de la carga de trabajo. Para minimizar el impacto de la creación de índices en el rendimiento de la carga de trabajo, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  busca una ventana de tiempo adecuada para cualquier operación de administración de índices. La acción de optimización se pospone si la base de datos necesita recursos para ejecutar la carga de trabajo y se reinicia cuando la base de datos tiene suficientes recursos no usados que se pueden usar para la tarea de mantenimiento. Una característica importante de la administración automática de índices es la comprobación de las acciones. Cuando [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  crea o quita un índice, un proceso de supervisión analiza el rendimiento de la carga de trabajo para comprobar que la acción ha mejorado el rendimiento general. Si no suponen una mejora significativa, la acción se revierte inmediatamente. De este modo, se [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  asegura de que las acciones de ajuste automático no afecten negativamente al rendimiento de la carga de trabajo. Los índices creados por el ajuste automático son transparentes para la operación de mantenimiento en el esquema subyacente. Los cambios de esquema, como quitar columnas o cambiarlas de nombre, no quedan bloqueados por la presencia de índices creados de forma automática. Los índices creados automáticamente por [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  se quitan inmediatamente cuando se quitan las columnas o tablas relacionadas.

### <a name="alternative---manual-index-management"></a>Administración de índices alternativa: manual

Sin la administración automática de índices, un usuario o DBA tendría que consultar manualmente la [sys.dm_db_missing_index_details &#40;vista de&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md) o usar el informe del panel de rendimiento en [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)] para buscar índices que podrían mejorar el rendimiento, crear índices con los detalles proporcionados en esta vista y supervisar manualmente el rendimiento de la consulta. Para encontrar los índices que deben quitarse, los usuarios deben supervisar las estadísticas de uso operativo de los índices para encontrar índices usados con poca frecuencia.

[!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] simplifica este proceso. [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] analiza la carga de trabajo, identifica las consultas que se pueden ejecutar más rápido con un nuevo índice e identifica los índices no usados o duplicados. Para más información sobre la identificación de los índices que deben cambiarse, consulte cómo [buscar recomendaciones de índices en Azure Portal](/azure/sql-database/sql-database-advisor-portal).

## <a name="see-also"></a>Consulte también  
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_automatic_tuning_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-automatic-tuning-options-transact-sql.md)  
 [sys.dm_db_tuning_recommendations &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 [sys.dm_db_missing_index_details &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-missing-index-details-transact-sql.md)   
 [sp_query_store_force_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)     
 [sp_query_store_unforce_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)           
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [Funciones JSON](../json/json-data-sql-server.md)    
 [Planes de ejecución](../../relational-databases/performance/execution-plans.md)    
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Herramientas de supervisión y optimización del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Supervisar el rendimiento mediante el Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)  
 [Asistente para la optimización de consultas](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)