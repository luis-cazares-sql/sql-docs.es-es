---
description: Crear índices filtrados
title: Creación de índices filtrados | Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- filtered indexes [SQL Server], about filtered indexes
- designing indexes [SQL Server], filtered
- filtered indexes [SQL Server]
- nonclustered indexes [SQL Server], filtered
- indexes [SQL Server], filtered
ms.assetid: 25e1fcc5-45d7-4c53-8c79-5493dfaa1c74
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 546ec1476ecae0da0fd119c7b45d06c2cc4406f6
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98764905"
---
# <a name="create-filtered-indexes"></a>Crear índices filtrados
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En este tema se describe cómo crear un índice filtrado en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Un índice filtrado es un índice no clúster optimizado, especialmente indicado para atender consultas que realizan selecciones a partir un subconjunto bien definido de datos. Utiliza un predicado de filtro para indizar una parte de las filas de la tabla. Un índice filtrado bien diseñado puede mejorar el rendimiento de las consultas, así como reducir los costos de almacenamiento y mantenimiento del índice en comparación con los índices de tabla completa.  
  
 Los índices filtrados pueden proporcionar las siguientes ventajas respecto a los índices de tabla completa:  
  
-   **Mejor rendimiento de las consultas y mayor calidad del plan**  
  
     Un índice filtrado bien diseñado mejora el rendimiento de las consultas y la calidad del plan de ejecución porque es menor que un índice no clúster de tabla completa y tiene estadísticas filtradas. Las estadísticas filtradas son más precisas que las de tabla completa porque corresponden solamente a las filas del índice filtrado.  
  
-   **Menor costo de mantenimiento de índices**  
  
     El mantenimiento de un índice se realiza únicamente cuando las instrucciones de lenguaje de manipulación de datos (DML) afectan a los datos en el índice. Un índice filtrado reduce los costos de mantenimiento del índice en comparación con un índice no clúster de tabla completa, ya que es menor y el mantenimiento se realiza únicamente cuando cambian los datos del índice. Se puede disponer de una gran cantidad de índices filtrados, sobre todo cuando contienen datos que cambian con poca frecuencia. De igual forma, si un índice filtrado contiene únicamente datos que se modifican a menudo, el tamaño menor del índice reduce el costo de actualización de las estadísticas.  
  
-   **Costos reducidos de almacenamiento de índices**  
  
     La creación de un índice filtrado puede reducir la cantidad de almacenamiento en disco de índices no clúster, cuando no sea necesario un índice de tabla completa. Puede reemplazar un índice no clúster de tabla completa con varios índices filtrados sin aumentar de forma considerable los requisitos de almacenamiento.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Consideraciones de diseño](#Design)  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para crear un índice filtrado, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="design-considerations"></a><a name="Design"></a> Consideraciones de diseño  
  
-   Cuando una columna solamente tiene un número pequeño de valores pertinentes para las consultas, puede crear un índice filtrado en el subconjunto de valores. Por ejemplo, cuando los valores en una columna son principalmente NULL y la consulta solamente selecciona entre valores distintos de NULL, puede crear un índice filtrado para las filas de datos distintos de NULL. El índice resultante será menor y tendrá costos de mantenimiento más reducidos que los de un índice no clúster de tabla completa definido en las mismas columnas de clave.  
  
-   Cuando una tabla tiene filas de datos heterogéneos, se puede crear un índice filtrado para una o varias categorías de datos. Esto puede mejorar el rendimiento de las consultas de estas filas de datos estrechando el foco de una consulta a un área concreta de la tabla. También en este caso, el índice resultante será menor y tendrá costos de mantenimiento más reducidos que los de un índice no clúster de tabla completa.  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   No se puede crear un índice filtrado en una vista. Sin embargo, el optimizador de consultas puede aprovechar un índice filtrado definido en una tabla a la que se hace referencia en una vista. El optimizador de consultas tiene en cuenta un índice filtrado para una consulta que selecciona en una vista si los resultados de la consulta serán correctos.

-   No se puede crear un índice filtrado en una tabla cuando la columna a la que se obtiene acceso en la expresión de filtro es de un tipo de datos CLR.
  
-   Los índices filtrados presentan las ventajas siguientes con respecto a las vistas indizadas:  
  
    -   Menor costo de mantenimiento de índices. Por ejemplo, el procesador de consultas utiliza menos recursos de CPU para actualizar un índice filtrado que una vista indizada.  
  
    -   Calidad del plan mejorada. Por ejemplo, durante la compilación de la consulta, el optimizador de consultas considera la posibilidad de usar un índice filtrado en más situaciones que la vista indizada equivalente.  
  
    -   El índice en línea se vuelve a generar. Puede volver a generar los índices filtrados mientras están disponibles para las consultas. Esto no es posible en el caso de las vistas indizadas. Para obtener más información, vea la opción REBUILD de [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
    -   Índices no únicos. Los índices filtrados pueden ser no únicos, mientras que las vistas indizadas deben ser únicas.  
  
-   Los índices filtrados se definen en una tabla y solamente admiten [operadores de comparación](../../t-sql/language-elements/comparison-operators-transact-sql.md) simples. Cuando necesite una expresión de filtro que haga referencia a varias tablas o que tenga lógica compleja, deberá crear una vista. Los índices filtrados no admiten operadores `LIKE`. 
  
-   Una columna de la expresión del índice filtrado no tiene por qué ser una columna incluida o de clave en la definición del índice filtrado cuando la expresión del índice filtrado es equivalente al predicado de la consulta y la consulta no devuelve la columna de la expresión del índice filtrado con los resultados de la consulta.  
  
-   Una columna de la expresión del índice filtrado debe ser una columna incluida o de clave de la definición del índice filtrado cuando el predicado de la consulta usa la columna en una comparación no equivalente a la expresión del índice filtrado.  
  
-   Una columna de la expresión del índice filtrado debe ser una columna incluida o de clave en la definición del índice filtrado si la columna está en el conjunto de resultados de la consulta.  
  
-   La clave de índice clúster de la tabla no tiene por qué ser una columna incluida o de clave de la definición del índice filtrado. La clave de índice cluster se incluye de forma automática en todos los índices no clúster, incluidos los índices filtrados.  
  
-   Si el operador de comparación especificado en la expresión del índice filtrado del índice filtrado produce una conversión de datos implícita o explícita, se producirá un error cuando la conversión se realice en el lado izquierdo de un operador de comparación. Una posible solución es escribir la expresión del índice filtrado con el operador de conversión de datos (CAST o CONVERT) en el lado derecho del operador de comparación.  

- Revise las opciones SET necesarias para la creación del índice filtrado en la sintaxis [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Requiere el permiso ALTER en la tabla o la vista. El usuario debe ser miembro del rol fijo de servidor **sysadmin** o de los roles fijos de base de datos **db_ddladmin** y **db_owner** . Para modificar la expresión de índice filtrado, utilice CREATE INDEX WITH DROP_EXISTING.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-filtered-index"></a>Para crear un índice filtrado  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir la base de datos que contiene la tabla en la que desea crear un índice filtrado.  
  
2.  Haga clic en el signo más para expandir la carpeta **Tablas** .  
  
3.  Haga clic en el signo más para expandir la tabla en la que desea crear un índice filtrado.  
  
4.  Haga clic con el botón derecho en la carpeta **Índices**, seleccione **Nuevo índice** y, luego, **Índice no agrupado...** .  
  
5.  En el cuadro de diálogo **Nuevo índice** , en la página **General** , escriba el nombre del nuevo índice en el cuadro **Nombre de índice** .  
  
6.  Debajo de **Columnas de clave de índice**, haga clic en **Agregar...** .  
  
7.  En el cuadro de diálogo **Seleccionar columnas de**_nombre\_tabla_, active las casillas de las columnas de tabla que se van a agregar al índice único.  
  
8.  Haga clic en **OK**.  
  
9. En la página **Filtro**, debajo de **Expresión de filtro**, escriba la expresión SQL que usará para crear el índice filtrado.  
  
10. Haga clic en **OK**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-filtered-index"></a>Para crear un índice filtrado  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Looks for an existing filtered index named "FIBillOfMaterialsWithEndDate"  
    -- and deletes it from the table Production.BillOfMaterials if found.   
    IF EXISTS (SELECT name FROM sys.indexes  
        WHERE name = N'FIBillOfMaterialsWithEndDate'  
        AND object_id = OBJECT_ID (N'Production.BillOfMaterials'))  
    DROP INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials  
    GO  
    -- Creates a filtered index "FIBillOfMaterialsWithEndDate"  
    -- on the table Production.BillOfMaterials   
    -- using the columms ComponentID and StartDate.  
  
    CREATE NONCLUSTERED INDEX FIBillOfMaterialsWithEndDate  
        ON Production.BillOfMaterials (ComponentID, StartDate)  
        WHERE EndDate IS NOT NULL ;  
    GO  
    ```  
  
     El índice filtrado anterior es válido para la consulta siguiente. Puede mostrar el plan de ejecución de consultas para determinar si el optimizador de consultas ha utilizado el índice filtrado.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ProductAssemblyID, ComponentID, StartDate   
    FROM Production.BillOfMaterials  
    WHERE EndDate IS NOT NULL   
        AND ComponentID = 5   
        AND StartDate > '01/01/2008' ;  
    GO  
    ```  
  
#### <a name="to-ensure-that-a-filtered-index-is-used-in-a-sql-query"></a>Para asegurarse de que un índice filtrado se usa en una consulta SQL  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT ComponentID, StartDate FROM Production.BillOfMaterials  
        WITH ( INDEX ( FIBillOfMaterialsWithEndDate ) )   
    WHERE EndDate IN ('20000825', '20000908', '20000918');   
    GO  
    ```  
  
 Para obtener más información, vea [CREATE INDEX&#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md).  
  
  
