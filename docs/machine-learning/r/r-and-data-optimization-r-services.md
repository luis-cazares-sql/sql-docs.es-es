---
title: Optimización del rendimiento de los datos
description: En este artículo se describen las optimizaciones de rendimiento para scripts de R o Python que se ejecutan en SQL Server. También se describen los métodos que puede usar para actualizar el código de R, tanto para mejorar el rendimiento como para evitar problemas conocidos.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 71c8852efd538ef0a702c8f96435facf4882b7d2
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596661"
---
# <a name="performance-tuning-and-data-optimization-for-r"></a>Optimización del rendimiento y de los datos para R
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se describen las optimizaciones de rendimiento para scripts de R o Python que se ejecutan en SQL Server. Puede usar estos métodos para actualizar el código de R, tanto para mejorar el rendimiento como para evitar problemas conocidos.

## <a name="choosing-a-compute-context"></a>Elección de un contexto de cálculo

En SQL Server, puede usar el contexto de cálculo **local** o de **SQL** al ejecutar el script de R o Python.

Cuando se usa el contexto de cálculo **local**, el análisis se realiza en el equipo y no en el servidor. Por lo tanto, si está obteniendo datos de SQL Server para usar en el código, se deben recuperar los datos mediante la red. El impacto en el rendimiento en el que se incurre para esta transferencia de red depende del tamaño de los datos transferidos, la velocidad de la red y otras transferencias de red que se producen al mismo tiempo.

Al utilizar el **contexto de cálculo SQL Server**, el código se ejecuta en el servidor. Si está obteniendo datos de SQL Server, los datos deben ser locales en el servidor que ejecuta el análisis y, por lo tanto, no se introduce ninguna sobrecarga de la red. Si necesita importar datos de otros orígenes, le recomendamos que organice los ETL de antemano.

Cuando se trabaja con grandes conjuntos de datos, se debe usar siempre el contexto de cálculo SQL.

## <a name="factors"></a>Factores

El lenguaje R tiene el concepto de *factores*, que son una variable especial para los datos categóricos. Los científicos de datos suelen usar variables de factor en su fórmula, porque el control de las variables categóricas como factores garantiza que los datos se procesan correctamente mediante las funciones de aprendizaje automático.

Por diseño, las variables de factor se pueden convertir de cadenas a enteros y viceversa para el almacenamiento o el procesamiento. La función de `data.frame` de R controla todas las cadenas como variables de factor, a menos que el argumento *stringsAsFactors* se establezca en **False**. Esto significa que las cadenas se convierten automáticamente en un entero para su procesamiento y, a continuación, se asignan de nuevo a la cadena original.

Si los datos de origen de los factores se almacenan como un entero, el rendimiento puede verse afectado porque R convierte los enteros de factor en cadenas en tiempo de ejecución y, a continuación, realiza su propia conversión interna de cadena a entero.

Para evitar estas conversiones en tiempo de ejecución, le recomendamos que almacene los valores como enteros en la tabla de SQL Server y que use el argumento _colInfo_ para especificar los niveles de la columna que se usa como factor. La mayoría de los objetos de origen de datos de RevoScaleR toman el parámetro _colInfo_. Use este parámetro para asignar un nombre a las variables utilizadas por el origen de datos, especificar su tipo y definir los niveles de variables o las transformaciones en los valores de columna.

Por ejemplo, la siguiente llamada de función de R obtiene los enteros 1, 2 y 3 de una tabla, pero asigna los valores a un factor con los niveles "manzana", "naranja" y "plátano".

```R
c("fruit" = c(type = "factor", levels=as.character(c(1:3)), newLevels=c("apple", "orange", "banana")))
```

Cuando la columna de origen contiene cadenas, siempre es más eficaz especificar los niveles con anterioridad mediante el parámetro _colInfo_. Por ejemplo, el siguiente código de R trata las cadenas como factores a medida que se leen.

```R
c("fruit" = c(type = "factor", levels= c("apple", "orange", "banana")))
```

Si no hay ninguna diferencia semántica en la generación del modelo, el último enfoque puede provocar un rendimiento mejor.

## <a name="data-transformations"></a>Transformaciones de datos

Los científicos de datos suelen emplear funciones de transformación escritas en R como parte del análisis. La función de transformación se aplica a todas las filas que se recuperan de la tabla. En SQL Server, estas transformaciones se aplican a todas las filas recuperadas en un lote, lo que requiere comunicación entre el intérprete de R y el motor de análisis. Para realizar la transformación, se mueven los datos de SQL al motor de análisis y, después, al proceso del intérprete de R y viceversa.

Por esta razón, el uso de transformaciones como parte del código de R pueden tener un importante efecto adverso en el rendimiento del algoritmo, según la cantidad de datos implicados.

Es más eficaz tener todas las columnas necesarias en la tabla o vista antes de realizar el análisis, y evitar las transformaciones durante el cálculo. Si no es posible agregar más columnas a las tablas existentes, considere la posibilidad de crear otra tabla o vista con las columnas transformadas y usar una consulta adecuada para recuperar los datos.

## <a name="batch-row-reads"></a>Lecturas de filas por lotes

Si usa un origen de datos de SQL Server (`RxSqlServerData`) en el código, se recomienda intentar usar el parámetro _rowsPerRead_ para especificar el tamaño del lote. Este parámetro define el número de filas que se consultan y, a continuación, se envían al script externo para su procesamiento. En tiempo de ejecución, el algoritmo solo ve el número especificado de filas en cada lote.

La capacidad de controlar la cantidad de datos que se procesan a la vez puede ayudarle a resolver o evitar problemas. Por ejemplo, si el conjunto de datos de entrada es muy amplio (tiene muchas columnas) o si el conjunto de datos tiene algunas columnas de gran tamaño (por ejemplo, texto libre), puede reducir el tamaño del lote para evitar la paginación de los datos sin memoria.

De forma predeterminada, el valor de este parámetro se establece en 50000, para garantizar un rendimiento aceptable, incluso en equipos con poca memoria. Si el servidor tiene suficiente memoria disponible, aumentar este valor a 500 000 o incluso un millón puede mejorar el rendimiento, especialmente para tablas grandes.

Las ventajas del aumento del tamaño del lote se vuelven evidentes en un conjunto de datos grande y en una tarea que se puede ejecutar en varios procesos. Sin embargo, el aumento de este valor no siempre produce los mejores resultados. Se recomienda experimentar con los datos y el algoritmo para determinar el valor óptimo.

## <a name="parallel-processing"></a>Procesamiento paralelo

Para mejorar el rendimiento de las funciones analíticas de **rx**, puede aprovechar la capacidad de SQL Server de ejecutar tareas en paralelo mediante núcleos disponibles en el equipo servidor.

Hay dos maneras de lograr la ejecución en paralelo con R en SQL Server:

+ **Usar \@parallel.** Cuando se usa el procedimiento almacenado `sp_execute_external_script` para ejecutar un script de R, se establece el parámetro `@parallel` en `1`. Este es el mejor método si el script de R **no** usa funciones de RevoScaleR, que tienen otros mecanismos para el procesamiento. Si el script usa funciones RevoScaleR (generalmente con el prefijo "rx"), el procesamiento en paralelo se realiza automáticamente y no es necesario establecer explícitamente `@parallel` en `1`.

  Si el script de R se puede ejecutar en paralelo, y la consulta SQL también, el motor de base de datos crea varios procesos paralelos. El número máximo de procesos que se pueden crear es igual a la configuración del **grado de paralelismo máximo** (MAXDOP) de la instancia. A continuación, todos los procesos ejecutan el mismo script, pero reciben solo una parte de los datos.
  
  Por lo tanto, este método no es útil con scripts que deben ver todos los datos, como al entrenar un modelo. Pero resulta útil al realizar tareas como la predicción por lotes en paralelo. Para obtener más información sobre el uso de paralelismo con `sp_execute_external_script`, vea la sección **Sugerencias avanzadas: procesamiento en paralelo** de [Uso de código de R en Transact-SQL](../tutorials/quickstart-r-create-script.md).

+ **Usar numTasks =1.** Al usar funciones **rx** en un contexto de cálculo de SQL Server, establezca el valor del parámetro _numTasks_ en el número de procesos que desea crear. El número de procesos creados nunca puede ser más de **MAXDOP**; sin embargo, el número real de procesos creados viene determinado por el motor de base de datos y puede ser menor que el solicitado.

  Si el script de R se puede ejecutar en paralelo y la consulta SQL también, entonces el SQL Server crea varios procesos en paralelo al ejecutar las funciones rx. El número real de procesos que se crean depende de diversos factores. Estos incluyen la gobernanza de los recursos, el uso actual de los recursos, otras sesiones y el plan de ejecución de consulta correspondiente a la consulta que se usa con el script de R.

## <a name="query-parallelization"></a>Paralelización de consultas

En Microsoft R, puede trabajar con orígenes de datos de SQL Server definiendo los datos como un objeto de origen de datos RxSqlServerData.

Crea un origen de datos basado en una tabla o vista completa:

```R
RxSqlServerData(table= "airline", connectionString = sqlConnString)
```

Crea un origen de datos basado en una consulta SQL:

```R
RxSqlServerData(sqlQuery= "SELECT [ArrDelay],[CRSDepTime],[DayOfWeek] FROM  airlineWithIndex WHERE rowNum <= 100000", connectionString = sqlConnString)
```

> [!NOTE]
> Si se especifica una tabla en el origen de datos en lugar de una consulta, R Services usa la heurística interna para determinar las columnas necesarias que capturar de la tabla. Pero es poco probable que este enfoque dé como resultado la ejecución en paralelo.

Para asegurarse de que los datos se pueden analizar en paralelo, la consulta utilizada para recuperar los datos debe estar tramada de forma que el motor de base de datos pueda crear un plan de consulta paralelo. Si el código o el algoritmo usan grandes volúmenes de datos, asegúrese de que la consulta proporcionada a `RxSqlServerData` está optimizada para la ejecución en paralelo. Una consulta que no da como resultado un plan de ejecución en paralelo puede producir un único proceso para el cálculo.

Si necesita trabajar con grandes conjuntos de recursos, use Management Studio u otro analizador de consultas de SQL antes de ejecutar el código de R para analizar el plan de ejecución. A continuación, siga los pasos recomendados para mejorar el rendimiento de la consulta. Por ejemplo, un índice que falta en una tabla puede afectar al tiempo necesario para ejecutar una consulta. Para obtener más información, vea [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md).

Otro error común que puede afectar al rendimiento es que una consulta recupera más columnas de las necesarias. Por ejemplo, si una fórmula se basa solo en tres columnas, pero la tabla de origen tiene 30 columnas, el traslado de los datos no es necesario.

+ No use `SELECT *`.
+ Tómese un tiempo para revisar las columnas del conjunto de registros e identificar solo las necesarias para el análisis.
+ Elimine de las consultas las columnas que contienen tipos de datos que no son compatibles con el código de R, como GUID y ROWGUID.
+ Compruebe los formatos de fecha y hora no admitidos.
+ En lugar de cargar una tabla, cree una vista que seleccione determinados valores o convierta columnas para evitar errores de conversión.

## <a name="optimizing-the-machine-learning-algorithm"></a>Optimización del algoritmo de aprendizaje automático

En esta sección se proporcionan varias sugerencias y recursos específicos de RevoScaleR y otras opciones de Microsoft R.

> [!TIP]
> Una explicación general de la optimización de R está fuera del ámbito de este artículo. Sin embargo, si necesita que el código sea más rápido, se recomienda el artículo popular [The R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf). Trata las construcciones de programación en R y los errores comunes en un lenguaje y detalle intensos, así como proporciona muchos ejemplos específicos de técnicas de programación de R.

### <a name="optimizations-for-revoscaler"></a>Optimizaciones para RevoScaleR

Muchos algoritmos de RevoScaleR admiten parámetros para controlar cómo se genera el modelo entrenado. Aunque la precisión y la exactitud del modelo son importantes, el rendimiento del algoritmo puede ser igual de importante. Para obtener el equilibrio adecuado entre la precisión y el tiempo de entrenamiento, puede modificar los parámetros para aumentar la velocidad del cálculo y, en muchos casos, mejorar el rendimiento sin reducir la precisión o la corrección.

+ [rxDTree](/r-server/r-reference/revoscaler/rxdtree)

  `rxDTree` admite el parámetro `maxDepth`, que controla la profundidad del árbol de decisión. Cuando se aumenta `maxDepth`, el rendimiento puede disminuir, por lo que es importante analizar las ventajas de aumentar la profundidad frente al rendimiento perjudicial.

  Puede controlar el equilibrio entre la complejidad temporal y la precisión de la predicción mediante el ajuste de parámetros como `maxNumBins`, `maxDepth`, `maxComplete`, y `maxSurrogate`. Aumentar la profundidad por encima de 10 o 15 puede hacer que el cálculo sea muy costoso.

+ [rxLinMod](/r-server/r-reference/revoscaler/rxlinmod)

  Intente usar el argumento `cube` cuando la primera variable dependiente de la fórmula es una variable de factor.
  
  Cuando `cube` se establece en `TRUE`, la regresión se realiza mediante un inverso con particiones, que podría ser más rápida y usar menos memoria que el cálculo de regresión estándar. Si la fórmula tiene un gran número de variables, la ganancia de rendimiento puede ser significativa.

+ [rxLogit](/r-server/r-reference/revoscaler/rxlogit)

  Use el argumento `cube` si la primera variable dependiente es una variable de factor.
  
  Cuando `cube` se establece en `TRUE`, el algoritmo usa un inverso con particiones, que podría ser más rápido y usar menos memoria. Si la fórmula tiene un gran número de variables, la ganancia de rendimiento puede ser significativa.

Para más información sobre la optimización de RevoScaleR, consulte estos artículos:

+ Artículo de ayuda: [Opciones de rxDForest/rxDTree de optimización del rendimiento](https://support.microsoft.com/kb/3104235)

+ Los métodos para controlar el modelo se ajustan a un modelo de árbol impulsado: [Cálculo de modelos mediante la potenciación de gradiente estocástico](/r-server/r/how-to-revoscaler-boosting)

+ Información general sobre cómo RevoScaleR mueve y procesa los datos: [Escritura de algoritmos de fragmentación personalizados en ScaleR](/r-server/r/how-to-developer-write-chunking-algorithms)

+ Modelo de programación para RevoScaleR: [Administración de subprocesos en RevoScaleR](/r-server/r/how-to-developer-manage-threads)

+ Referencia de función para [rxDForest](/r-server/r-reference/revoscaler/rxdforest)

+ Referencia de función para [rxBTrees](/r-server/r-reference/revoscaler/rxbtrees)

### <a name="use-microsoftml"></a>Uso de MicrosoftML

También se recomienda que examine el nuevo paquete de **MicrosoftML**, que proporciona algoritmos de aprendizaje automático escalables que pueden usar los contextos de cálculo y las transformaciones proporcionadas por RevoScaleR.

+ [Introducción a MicrosoftML](/r-server/r/concept-what-is-the-microsoftml-package)

+ [Cómo elegir un algoritmo de MicrosoftML](/r-server/r/how-to-choose-microsoftml-algorithms-cheatsheet)

## <a name="next-steps"></a>Pasos siguientes

+ Si quiere ver las funciones de R que puede usar para mejorar el rendimiento del código de R, consulte [Uso de funciones de generación de perfiles de código de R para mejorar el rendimiento](using-r-code-profiling-functions.md).

+ Para información más completa sobre la optimización del rendimiento en SQL Server, consulte [Centro de rendimiento para el motor de base de datos de SQL Server y Base de datos SQL de Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).