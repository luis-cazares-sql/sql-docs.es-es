---
description: Información general sobre los índices espaciales
title: Información general sobre los índices espaciales | Microsoft Docs
ms.date: 09/12/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- spatial indexes [SQL Server]
ms.assetid: b1ae7b78-182a-459e-ab28-f743e43f8293
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bedf575888445fb0e14684d4e325b60eb8547a79
ms.sourcegitcommit: 7791bd2ba339edc5cd2078a6537c8f6bfe72a19b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2021
ms.locfileid: "98564462"
---
# <a name="spatial-indexes-overview"></a>Información general sobre los índices espaciales
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite datos espaciales e índices espaciales. Un *índice espacial* es un tipo de índice extendido que permite indizar una columna espacial. Una columna espacial es una columna de tabla que contiene datos de un tipo espacial, como **geometría** o **geografía**.  
  
> [!IMPORTANT]  
>  Para obtener una descripción detallada y ejemplos de las nuevas características espaciales de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], incluidas las características que afectan a los índices espaciales, descargue las notas del producto [Nuevas características espaciales de SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=226407).  
  
##  <a name="about-spatial-indexes"></a><a name="about"></a> Acerca de los índices espaciales  
  
###  <a name="decomposing-indexed-space-into-a-grid-hierarchy"></a><a name="decompose"></a> Descomponer espacio indizado en una jerarquía de cuadrículas  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los índices espaciales se generan usando árboles B, es decir, los índices deben representar los datos espaciales bidimensionales en el orden lineal de los árboles B. Por consiguiente, antes de leer los datos en un índice espacial, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa una descomposición uniforme jerárquica de espacio. El proceso de creación de índices *descompone* el espacio en una *jerarquía de cuadrículas* de cuatro niveles. Estos niveles se conocen como *nivel 1* (el nivel superior), *nivel 2*, *nivel 3* y *nivel 4*.  
  
 Cada nivel sucesivo descompone el nivel superior, de manera que cada celda de nivel superior contiene una cuadrícula completa en el nivel siguiente. En un nivel determinado, todas las cuadrículas tienen el mismo número de celdas a lo largo de ambos ejes (por ejemplo, 4x4 o 8x8) y las celdas son todas del mismo tamaño.  
  
 La ilustración siguiente muestra la descomposición para la celda superior derecha en cada nivel de la jerarquía de cuadrículas en una cuadrícula 4x4. En realidad, todas las celdas se descomponen de esta manera. Así, por ejemplo, la descomposición de un espacio en cuatro niveles de cuadrículas 4x4 produce realmente un total de 65.536 celdas de cuatro niveles.  
  
 ![Cuatro niveles de teselación recursiva](../../relational-databases/spatial/media/spndx-recursive-levels-telescoped.gif "Cuatro niveles de teselación recursiva")  
  
> [!NOTE]  
>  La descomposición de espacio de un índice espacial es independiente de la unidad de medida que el dato de aplicación usa.  
  
 Las celdas de una jerarquía de cuadrícula se enumeran de un modo lineal usando una variación de la curva de rellenado de espacio Hilbert. Para esta ilustración, sin embargo, esta discusión usa una numeración de modo de fila simple, en lugar de la numeración realmente generada por la curva de Hilbert. En la ilustración siguiente, ya se han colocado varios polígonos que representan edificios y líneas que representan calles en una cuadrícula de nivel 1 4x4. Las celdas de nivel 1 se numeran del 1 al 16, comenzando por la celda superior izquierda.  
  
 ![Polígonos y líneas colocados en una cuadrícula de nivel 1 4x4](../../relational-databases/spatial/media/spndx-level-1-objects.gif "Polígonos y líneas colocados en una cuadrícula de nivel 1 4x4")  
  
#### <a name="grid-density"></a>Densidad de cuadrícula  
 El número de celdas a lo largo de los ejes de una cuadrícula determina su *densidad*: cuanto mayor sea el número, más densa será la cuadrícula. Por ejemplo, una cuadrícula 8x8 (que genera 64 celdas), es más densa que una cuadrícula 4x4 (que genera 16 celdas). La densidad de cuadrícula se define por nivel.  
  
 La instrucción [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] admite una cláusula GRIDS que le permite especificar las densidades de cuadrícula diferentes en niveles diferentes. La densidad de cuadrícula para un nivel determinado se especifica con una de las palabras clave siguientes.  
  
|Palabra clave|Configuración de cuadrícula|Número de celdas|  
|-------------|------------------------|---------------------|  
|LOW|4X4|16|  
|MEDIUM|8X8|64|  
|HIGH|16X16|256|  
  
 En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cuando el nivel de compatibilidad de la base de datos se establece en 100 o menos, el valor predeterminado es MEDIUM en todos los niveles. Cuando el nivel de compatibilidad de la base de datos se establece en 110 o más, el valor predeterminado es un esquema de cuadrícula automática. (La cuadrícula automática indica una configuración de nivel 8 de HLLLLLLL). En lugar de variar la densidad de la cuadrícula de índices, puede variar las celdas por objeto y las celdas de la ventana de consulta por objeto a través de una sugerencia. 
  
 Puede controlar el proceso de descomposición especificando densidades de cuadrícula no predeterminadas. Por ejemplo, quizá sean útiles diferentes densidades de cuadrícula en niveles distintos para ajustar correctamente un índice basado en el tamaño del espacio indizado y los objetos de la columna espacial.  
  
> [!NOTE]  
>  Las densidades de cuadrícula de un índice espacial se ven en las columnas level_1_grid, level_2_grid, level_3_grid y level_4_grid de la vista de catálogo [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) cuando el nivel de compatibilidad de la base de datos se establece en 100 o menos. Las opciones del esquema de teselación **GEOMETRY_AUTO_GRID**/**GEOGRAPHY_AUTO_GRID** no rellenan estas columnas. La vista de catálogo sys.spatial_index_tessellations tiene valores **NULL** para estas columnas cuando se usan las opciones de cuadrícula automática.  
  
###  <a name="tessellation"></a><a name="tessellation"></a> Teselación  
 Después de la descomposición de un espacio indizado en una jerarquía de cuadrículas, el índice espacial lee los datos de la columna espacial, fila por fila. Después de leer los datos para un objeto espacial (o instancia), el índice espacial realiza un *proceso de teselación* para dicho objeto. El proceso de teselación ajusta el objeto en la jerarquía de cuadrículas asociándolo a un conjunto de celdas de cuadrícula que modifica (*celdas modificadas*). Comenzando por el nivel 1 de la jerarquía de cuadrículas, el proceso de teselación continúa por el nivel *primero a lo ancho* . Potencialmente, el proceso puede continuar a través de los cuatro niveles, un nivel a la vez.  
  
 El resultado del proceso de teselación es un conjunto de celdas modificadas que se graban en el índice espacial para el objeto. Haciendo referencia a estas celdas grabadas, el índice espacial puede buscar el objeto en el espacio relativo a otros objetos de la columna espacial también almacenados en el índice.  
  
#### <a name="tessellation-rules"></a>Reglas de teselación  
 Para limitar el número de celdas modificadas que se graban para un objeto, el proceso teselación aplica varias reglas de teselación. Estas reglas determinan la profundidad del proceso de teselación y cuáles de las celdas modificadas se graban en el índice.  
  
 Estas reglas son:  
  
-   La regla de cobertura  
  
     Si el objeto cubre una celda por completo, se dice que dicha celda está *cubierta* por el objeto. Se cuenta una celda cubierta y no se aplica la teselación. Esta regla se aplica a todos los niveles de la jerarquía de cuadrículas. La regla de cobertura simplifica el proceso de teselación y reduce la cantidad de datos que un índice espacial registra.  
  
-   La regla de celdas por proyecto  
  
     Esta regla exige el *límite de celdas por proyecto*, que determina el número máximo de celdas que se pueden contar para cada objeto, excepto en el nivel 1. En los niveles inferiores, la regla de celdas por proyecto controla la cantidad de información que se puede grabar sobre el objeto.  
  
-   La regla de celda más profunda  
  
     La regla de celda más profunda genera la mejor aproximación de un objeto grabando solo las únicas celdas de la parte más inferior que se han teselado para el objeto. Las celdas principales no contribuyen al recuento de celdas por objeto y no se graban en el índice.  
  
 Estas reglas de teselación se aplican en cada nivel de cuadrícula de forma recursiva. El resto de esta sección describe las reglas de teselación con más detalle.  
  
#### <a name="covering-rule"></a>Regla de cobertura  
 Si un objeto cubre una celda por completo, se dice que dicha celda está *cubierta* por el objeto. Por ejemplo, en la ilustración siguiente, una de las celdas de segundo nivel, 15.11, está cubierta completamente por la parte media de un octágono.  
  
 ![Optimización de cobertura](../../relational-databases/spatial/media/spndx-opt-covering.gif "Optimización de cobertura")  
  
 Se cuenta una celda cubierta y se graba en el índice, y la celda no se divide más en mosaicos.  
  
#### <a name="cells-per-object-rule"></a>Regla de celdas por proyecto  
 El nivel de teselación de cada objeto depende principalmente del *límite de celdas por proyecto* del índice espacial. Este límite define el número máximo de celdas que la teselación puede contar por objeto. Sin embargo, tenga en cuenta que la regla de celdas por proyecto no se exige para el nivel 1, de modo que es posible superar este límite. Si el recuento del nivel 1 alcanza, o supera, el límite de celdas por objeto, no se produce más teselación en los niveles inferiores.  
  
 Siempre que el recuento sea menor que el límite de celdas por proyecto, el proceso de teselación continúa. Empezando por la celda modificada de número inferior (por ejemplo, la celda 15.6 de la ilustración anterior), el proceso prueba todas las celdas para evaluar si contarlas o dividirlas en mosaicos. Si con la teselación de una celda se superaría el límite de celdas por proyecto, se cuenta la celda y no se realiza su teselación. De lo contrario, la celda se divide en mosaicos y se cuentan las celdas de nivel inferior modificadas por el objeto. El proceso de teselación continúa de esta manera, en toda la amplitud, por el nivel. Este proceso se repite recursivamente para las cuadrículas de nivel inferior de las celdas divididas en mosaicos hasta que se alcanza el límite o no hay más celdas que contar.  
  
 Por ejemplo, piense en la ilustración anterior, que muestra un octágono que se ajusta por completo a la celda 15 de la cuadrícula de nivel 1. En la ilustración, se ha realizado la teselación en la celda 15, diseccionando el octágono en nueve celdas de nivel 2. Esta ilustración supone que el límite de celdas por proyecto es 9 o más. Sin embargo, si el límite de celdas por proyecto fuera 8 o menos, no se realizaría la teselación en la celda 15 y solo se contaría dicha celda 15 para el objeto.  
  
 De forma predeterminada, el límite de celdas por proyecto es de 16 celdas por objeto, lo que proporciona un equilibrio satisfactorio entre espacio y precisión para la mayoría de los índices espaciales. Pero la instrucción [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] admite una cláusula CELLS_PER_OBJECT **=**  _n_ que permite especificar un límite de celdas por objeto de entre 1 y 8192, ambos incluidos.  
  
> [!NOTE]  
>  El valor **cells_per_object** de un índice espacial es visible en la vista de catálogo [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) .  
  
#### <a name="deepest-cell-rule"></a>Regla de celda más profunda  
 La regla de celda más profunda se aprovecha del hecho de que todas las celdas de nivel inferior pertenecen a la celda superior: una celda de nivel 4 pertenece a una celda de nivel 3, una celda de nivel 3 pertenece a una celda de nivel 2 y una celda de nivel 2 pertenece a una celda de nivel 1. Por ejemplo, un objeto que pertenezca a la celda 1.1.1.1, también pertenece a la celda 1.1.1, a la celda 1.1 y a la celda 1. El conocimiento de dichas relaciones de jerarquía de celdas está integrado en el procesador de consultas. Por consiguiente, solo las celdas de nivel más profundo se tienen que grabar en el índice, minimizando la información que el índice tiene que almacenar.  
  
 En la ilustración siguiente, un polígono en forma de rombo relativamente pequeño se divide en mosaicos. El índice usa el límite de celdas por proyecto predeterminado de 16, que no se alcanza para este objeto pequeño. Por lo tanto, la teselación continúa bajando hasta el nivel 4. El polígono reside en las siguientes celdas del nivel 1 hasta el nivel 3: 4, 4.4, 4.4.10 y 4.4.14. Sin embargo, si se usa la regla de celda más profunda, la teselación solo cuenta las doce celdas del nivel 4: 4.4.10.13-15 y 4.4.14.1-3, 4.4.14.5-7 y 4.4.14.9-11.  
  
 ![Optimización de celda más profunda](../../relational-databases/spatial/media/spndx-opt-deepest-cell.gif "Optimización de celda más profunda")  
  
###  <a name="tessellation-schemes"></a><a name="schemes"></a> Esquemas de teselación  
 El comportamiento de un índice espacial depende en parte de su *esquema de teselación*. El esquema de teselación es el tipo de datos concreto. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los índices espaciales admiten dos esquemas de teselación:  
  
-   *Teselación de cuadrícula de geometría*, que es el esquema para el tipo de datos **geometry** .  
  
-   *Teselación de cuadrícula de geografía*, que se aplica a las columnas del tipo de datos **geography** .  
  
> [!NOTE]  
>  El valor **tessellation_scheme** de un índice espacial es visible en la vista de catálogo [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) .  
  
#### <a name="geometry-grid-tessellation-scheme"></a>Esquema de teselación de cuadrícula de geometría  
 La teselación GEOMETRY_AUTO_GRID es el esquema de teselación predeterminado para el tipo de datos **geometry** de [!INCLUDE[ssNoVersion](../../includes/sssql11-md.md)] y versiones posteriores.  La teselación GEOMETRY_GRID es el único esquema de teselación disponible para los tipos de datos geometry de [!INCLUDE[ssNoVersion](../../includes/sskatmai-md.md)]. En esta sección se tratan los aspectos de teselación de cuadrícula de geometría relacionados con trabajar con índices espaciales: métodos compatibles y cuadros de límite.  
  
> [!NOTE]  
>  Puede especificar explícitamente este esquema de teselación con la cláusula USING (GEOMETRY_AUTO_GRID/GEOMETRY_GRID) de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
##### <a name="the-bounding-box"></a>El cuadro de límite  
 Los datos geométricos ocupan un plano que puede ser infinito. Sin embargo, en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un índice espacial requiere un espacio finito. Para establecer un espacio finito para descomposición, el esquema de teselación de cuadrícula de geometría exige un *cuadro de límite* rectangular. El cuadro de límite está definido por cuatro coordenadas, **(**_x-min_**,**_y-min_**)** y **(**_x-max_**,**_y-max_**)**, que se almacenan como propiedades del índice espacial. Estas coordenadas representan lo siguiente:  
  
-   *x-min* es la coordenada x de la esquina inferior izquierda del cuadro de límite.  
  
-   *y-min* es la coordenada y de la esquina inferior izquierda.  
  
-   *x-max* es la coordenada x de la esquina superior derecha.  
  
-   *y-max* es la coordenada y de la esquina superior derecha.  
  
> [!NOTE]  
>  Estas coordenadas están especificadas por la cláusula BOUNDING_BOX de la instrucción [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
 Las coordenadas **(**_x-min_**,**_y-min_**)** y **(**_x-max_**,**_y-max_**)** determinan la posición y las dimensiones del cuadro de límite. El espacio exterior del cuadro de límite se trata como una celda única con el número 0.  
  
 El índice espacial descompone el espacio que se encuentra dentro del cuadro de límite. La cuadrícula de nivel 1 de la jerarquía de cuadrículas rellena el cuadro de límite. Para colocar un objeto geométrico en la jerarquía de cuadrículas, el índice espacial compara las coordenadas del objeto con las coordenadas del cuadro de límite.  
  
 La ilustración siguiente muestra los puntos definidos por las coordenadas **(**_x-min_**,**_y-min_**)** y **(**_x-max_**,**_y-max_**)** del cuadro de límite. El nivel superior de la jerarquía de cuadrículas se muestra como una cuadrícula 4x4. Para la ilustración, se omiten los niveles inferiores. Un cero (0) indica el espacio exterior del cuadro de límite. Tenga en cuenta que el objeto 'A' se extiende, en parte, más allá del cuadro y el objeto 'B' permanece por completo fuera del cuadro de la celda 0.  
  
 ![Cuadro de límite que muestra las coordenadas y la celda 0.](../../relational-databases/spatial/media/spndx-bb-4x4-objects.gif "Cuadro de límite que muestra las coordenadas y la celda 0.")  
  
 Un cuadro de límite se corresponde a alguna parte de los datos espaciales de una aplicación. Depende de la aplicación si el cuadro de límite del índice contiene por completo los datos almacenados en la columna espacial, o solo contiene una parte. Solo las operaciones calculadas en los objetos que están completamente dentro del cuadro de límite se benefician del índice espacial. Por consiguiente, para aprovechar al máximo un índice espacial en una columna **geometry** , se debe especificar un cuadro de límite que contenga todos los objetos o la mayoría de ellos.  
  
> [!NOTE]  
>  Las densidades de cuadrícula de un índice espacial están visibles en las columnas bounding_box_xmin, bounding_box_ymin, bounding_box_xmax y bounding_box_ymax de la vista de catálogo [sys.spatial_index_tessellations](../../relational-databases/system-catalog-views/sys-spatial-index-tessellations-transact-sql.md) .  
  
#### <a name="the-geography-grid-tessellation-scheme"></a>El esquema de teselación de cuadrícula de geografía  
 Este esquema de teselación solo se aplica a una columna **geography** . Esta sección resume los métodos admitidos por teselación de cuadrícula de geografía y trata cómo se proyecta el espacio geodésico en un plano, que se descompone a continuación en una jerarquía de cuadrículas.  
  
> [!NOTE]  
>  Puede especificar explícitamente este esquema de teselación con la cláusula USING (GEOGRAPHY_AUTO_GRID/GEOGRAPHY_GRID) de la instrucción [CREATE SPATIAL INDEX](../../t-sql/statements/create-spatial-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
##### <a name="projection-of-the-geodetic-space-onto-a-plane"></a>Proyección del espacio geodésico en un plano  
 Los cálculos en instancias de **geography** (objetos) tratan el espacio que contiene los objetos como un elipsoide geodésico. Para descomponer este espacio, el esquema teselación de cuadrícula geography divide la superficie del elipsoide en sus hemisferios superior e inferior y, a continuación, realiza los pasos siguientes:  
  
1.  Proyecta cada hemisferio en las caras de una pirámide cuadrangular.  
  
2.  Aplana las dos pirámides.  
  
3.  Une las pirámides aplanadas para formar un plano no euclidiano.  

 La ilustración siguiente muestra una vista esquemática del proceso de descomposición de tres pasos. En las pirámides, las líneas de puntos representan los límites de las cuatro facetas de cada pirámide. En los pasos 1 y 2 se muestra el elipsoide geodésico, con una línea horizontal de color verde para representar la línea de latitud ecuatorial y una serie de líneas verticales de color verde para representar varias líneas de longitud. El paso 1 muestra las pirámides proyectándose sobre los dos hemisferios. El paso 2 muestra las pirámides aplanándose. El paso 3 muestra las pirámides aplanadas, una vez que se han combinado para formar un plano, mostrando varias líneas de longitud proyectadas. Observe que estas líneas proyectadas se ponen rectas y varían en longitud, dependiendo de dónde se encuentra en las pirámides.  
  
 ![Proyección de la elipsoide en un plano](../../relational-databases/spatial/media/spndx-geodetic-projection.gif "Proyección de la elipsoide en un plano")  
  
 Una vez que se ha proyectado el espacio en el plano, éste se descompone en la jerarquía de cuadrículas de cuatro niveles. Diferentes niveles pueden usar distintas densidades de cuadrícula. La ilustración siguiente muestra el plano una vez se ha descompuesto en una cuadrícula de nivel 1 4x4. Para esta ilustración, se omiten los niveles inferiores de la jerarquía de cuadrículas. En realidad, el plano se descompone totalmente en una jerarquía de cuadrículas de cuatro niveles. Cuando termina el proceso de descomposición, se leen los datos geográficos, fila por fila, desde la columna geography y, a su vez, se lleva a cabo el proceso de teselación para cada objeto.  
  
 ![Cuadrícula de geography de nivel 1](../../relational-databases/spatial/media/spndx-geodetic-level1grid.gif "Cuadrícula de geography de nivel 1")  
  
##  <a name="methods-supported-by-spatial-indexes"></a><a name="methods"></a> Métodos admitidos por los índices espaciales  
  
###  <a name="geometry-methods-supported-by-spatial-indexes"></a><a name="geometry"></a> Métodos de Geometry que los índices espaciales admiten  
 En ciertas condiciones, los índices espaciales son compatibles con los siguientes métodos de geometry y orientados a conjuntos: STContains(), STDistance(), STEquals(), STIntersects(), STOverlaps(), STTouches() y STWithin(). Para que un índice espacial los admita, estos métodos se deben usar dentro de la cláusula WHERE o JOIN ON de una consulta, y se deben producir dentro de un predicado con el formato general siguiente:  
  
 *geometry1*.*nombre_método*(*geometry2*)*operador_comparación**número_válido*  
  
 Para devolver un resultado no nulo, *geometry1* y *geometry2* deben tener el mismo [identificador de referencia espacial (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md). De lo contrario, el método devolverá NULL.  
  
 Los índices espaciales son compatibles con los formatos de predicado siguientes:  
  
-   *geometry1*.[STContains](../../t-sql/spatial-geometry/stcontains-geometry-data-type.md)(*geometry2*) = 1  
  
-   *geometry1*.[STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)(*geometry2*) < *number*  
  
-   *geometry1*.[STDistance](../../t-sql/spatial-geometry/stdistance-geometry-data-type.md)(*geometry2*) <= *number*  
  
-   *geometry1*.[STEquals](../../t-sql/spatial-geometry/stequals-geometry-data-type.md)(*geometry2*)= 1  
  
-   *geometry1*.[STIntersects](../../t-sql/spatial-geometry/stintersects-geometry-data-type.md)(*geometry2*)= 1  
  
-   *geometry1.* [STOverlaps](../../t-sql/spatial-geometry/stoverlaps-geometry-data-type.md) *(geometry2) = 1*  
  
-   *geometry1*.[STTouches](../../t-sql/spatial-geometry/sttouches-geometry-data-type.md)(*geometry2*) = 1  
  
-   *geometry1*.[STWithin](../../t-sql/spatial-geometry/stwithin-geometry-data-type.md)(*geometry2*)= 1  
  
###  <a name="geography-methods-supported-by-spatial-indexes"></a><a name="geography"></a> Métodos de Geography admitidos por los índices espaciales  
 En ciertas condiciones, los índices espaciales son compatibles con los siguientes métodos de geography orientados a conjuntos: STIntersects(),STEquals() y STDistance(). Para que un índice espacial los admita, estos métodos se deben usar dentro de la cláusula WHERE y se deben producir dentro de un predicado con el formato general siguiente:  
  
 *geography1*.*nombre_método*(*geography2*)*operador_comparación**número_válido*  
  
 Para devolver un resultado no nulo, *geography1* y *geography2* deben tener el mismo [identificador de referencia espacial (SRID)](../../relational-databases/spatial/spatial-reference-identifiers-srids.md). De lo contrario, el método devolverá NULL.  
  
 Los índices espaciales son compatibles con los formatos de predicado siguientes:  
  
-   *geography1*.[STIntersects](../../t-sql/spatial-geography/stintersects-geography-data-type.md)(*geography2*)= 1  
  
-   *geography1*.[STEquals](../../t-sql/spatial-geography/stequals-geography-data-type.md)(*geography2*)= 1  
  
-   *geography1*.[STDistance](../../t-sql/spatial-geography/stdistance-geography-data-type.md)(*geography2*) < *number*  
  
-   *geography1*.[STDistance](../../t-sql/spatial-geography/stdistance-geography-data-type.md)(*geography2*) <= *number*  
  
### <a name="queries-that-use-spatial-indexes"></a>Consultas que usan índices espaciales  
 Solo se admiten índices espaciales en las consultas que incluyen un operador espacial indizado en la cláusula **WHERE** . Por ejemplo, sintaxis como la siguiente:  
  
```  
[spatial object].SpatialMethod([reference spatial object]) [ = | < ] [const literal or variable]  
```  
  
 El optimizador de consultas entiende la conmutatividad de las operaciones espaciales ( `@a.STIntersects(@b) = @b.STInterestcs(@a)` ). Sin embargo, el índice espacial no se usará si el principio de una comparación no contiene el operador espacial (por ejemplo, `WHERE 1 = spatial op` no usará el índice espacial). Para usar el índice espacial, vuelva a escribir la comparación (por ejemplo, `WHERE spatial op = 1`).  
  
 Como ocurre con cualquier otro índice, cuando se admite un índice espacial, el uso del índice espacial se elige en función del costo, de modo que el optimizador de consultas no elige usar el índice espacial aunque se cumplan todos los requisitos para usarlo. Use el plan de presentación para ver si se usó el índice espacial y, si es necesario, proporcione sugerencias de consulta para forzar un plan de consulta deseado.  
  
 El tipo de vecino más cercano de consulta también admite índices espaciales; sin embargo, solo si se escribe una sintaxis de consulta específica. La sintaxis adecuada es:  
  
```  
SELECT TOP(K) [WITH TIES] *   
FROM <Table> AS T [WITH(INDEX(<SpatialIndex>))]  
WHERE <SpatialColumn>.STDistance(@reference_object) IS NOT NULL  
ORDER BY <SpatialColumn>.STDistance(@reference_object) [;]  
```  
  
## <a name="see-also"></a>Vea también  
 [Datos espaciales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
