---
description: Crear funciones definidas por el usuario (motor de base de datos)
title: Creación de funciones definidas por el usuario (motor de base de datos) | Microsoft Docs
ms.custom: ''
ms.date: 11/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- SCHEMABINDING clause
- schema-bound functions [SQL Server]
- user-defined functions [SQL Server], creating
- CREATE FUNCTION statement
- valid statements [SQL Server]
- UDF
- TVF
ms.assetid: f0d5dd10-73fd-4e05-9177-07f56552bdf7
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3303df6ac71b7a6c92d4a1c65a8b7b4b3229cd6e
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97743682"
---
# <a name="create-user-defined-functions-database-engine"></a>Crear funciones definidas por el usuario (motor de base de datos)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo crear una función definida por el usuario en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Las funciones definidas por el usuario no se pueden utilizar para realizar acciones que modifican el estado de la base de datos.  
  
-   Las funciones definidas por el usuario no pueden tener una cláusula `OUTPUT INTO` que tenga una tabla como destino.  
  
-   Las funciones definidas por el usuario no pueden devolver varios conjuntos de resultados. Utilice un procedimiento almacenado si necesita devolver varios conjuntos de resultados.  
  
-   El control de errores está restringido en una función definida por el usuario. Una UDF no admite `TRY...CATCH`, `@ERROR` o `RAISERROR`.  
  
-   Las funciones definidas por el usuario no pueden llamar a un procedimiento almacenado, pero pueden llamar a un procedimiento almacenado extendido.  
  
-   Las funciones definidas por el usuario no pueden utilizar tablas temporales o SQL dinámicas. Se permiten las variables de tabla.  
  
-   Las instrucciones `SET` no se permiten en una función definida por el usuario.  
  
-   No se admite la cláusula `FOR XML`.  
  
-   Las funciones definidas por el usuario se pueden anidar; es decir, una función definida por el usuario puede llamar a otra. El nivel de anidamiento aumenta cuando se empieza a ejecutar la función llamada y disminuye cuando se termina de ejecutar la función llamada. Las funciones definidas por el usuario se pueden anidar hasta un máximo de 32 niveles. Si se superan los niveles máximos de anidamiento, la cadena completa de funciones de llamada produce un error. Cualquier referencia a código administrado desde una función Transact-SQL definida por el usuario cuenta como uno de los 32 niveles de anidamiento. Los métodos invocados desde el código administrado no cuentan para este límite.  
  
-   Las instrucciones de Service Broker siguientes **no se pueden incluir** en la definición de una función [!INCLUDE[tsql](../../includes/tsql-md.md)] definida por el usuario:  
  
    -   `BEGIN DIALOG CONVERSATION`  
  
    -   `END CONVERSATION`  
  
    -   `GET CONVERSATION GROUP`  
  
    -   `MOVE CONVERSATION`  
  
    -   `RECEIVE`  
  
    -   `SEND`  
  
###  <a name="permissions"></a><a name="Security"></a> Permisos 

Se requiere el permiso `CREATE FUNCTION` en la base de datos y el permiso `ALTER` en el esquema en el que se va a crear la función. Si la función especifica un tipo definido por el usuario, requiere el permiso `EXECUTE` en el tipo.  
  
##  <a name="scalar-functions"></a><a name="Scalar"></a> Funciones escalares  
 En el ejemplo siguiente se crea una **función escalar (UDF escalar)** de varias instrucciones en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La función toma un valor de entrada, `ProductID`, y devuelve un valor de devolución único, la cantidad agregada del producto especificado en el inventario.  
  
```sql  
IF OBJECT_ID (N'dbo.ufnGetInventoryStock', N'FN') IS NOT NULL  
    DROP FUNCTION ufnGetInventoryStock;  
GO  
CREATE FUNCTION dbo.ufnGetInventoryStock(@ProductID int)  
RETURNS int   
AS   
-- Returns the stock level for the product.  
BEGIN  
    DECLARE @ret int;  
    SELECT @ret = SUM(p.Quantity)   
    FROM Production.ProductInventory p   
    WHERE p.ProductID = @ProductID   
        AND p.LocationID = '6';  
     IF (@ret IS NULL)   
        SET @ret = 0;  
    RETURN @ret;  
END; 
```  
  
 En el ejemplo siguiente se utiliza la función `ufnGetInventoryStock` para devolver la cantidad de inventario actual de aquellos productos que tienen un `ProductModelID` entre 75 y 80.  
  
```sql  
SELECT ProductModelID, Name, dbo.ufnGetInventoryStock(ProductID)AS CurrentSupply  
FROM Production.Product  
WHERE ProductModelID BETWEEN 75 and 80;  
```  

> [!NOTE]  
> Para obtener más información y ejemplos de funciones escalares, vea [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

##  <a name="table-valued-functions"></a><a name="TVF"></a> Funciones con valores de tabla  
En el ejemplo siguiente se crea una **función insertada con valores de tabla(TVF)** en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La función toma un parámetro de entrada, Id. de cliente (almacén), y devuelve las columnas `ProductID`, `Name`, y el agregado de las ventas del año hasta la fecha como `YTD Total` para cada producto vendido en el almacén.  
  
```sql  
IF OBJECT_ID (N'Sales.ufn_SalesByStore', N'IF') IS NOT NULL  
    DROP FUNCTION Sales.ufn_SalesByStore;  
GO  
CREATE FUNCTION Sales.ufn_SalesByStore (@storeid int)  
RETURNS TABLE  
AS  
RETURN   
(  
    SELECT P.ProductID, P.Name, SUM(SD.LineTotal) AS 'Total'  
    FROM Production.Product AS P   
    JOIN Sales.SalesOrderDetail AS SD ON SD.ProductID = P.ProductID  
    JOIN Sales.SalesOrderHeader AS SH ON SH.SalesOrderID = SD.SalesOrderID  
    JOIN Sales.Customer AS C ON SH.CustomerID = C.CustomerID  
    WHERE C.StoreID = @storeid  
    GROUP BY P.ProductID, P.Name  
);  
```  
  
En el ejemplo siguiente se invoca la función y se especifica el identificador de cliente 602.  
  
```sql  
SELECT * FROM Sales.ufn_SalesByStore (602);  
```  
  
En el ejemplo siguiente se crea una **función con valores de tabla de varias instrucciones (MSTVF)** en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)]. La función toma un único parámetro de entrada, `EmployeeID` , y devuelve una lista de todos los empleados que dependen directa o indirectamente del empleado especificado. La función se invoca luego especificando el empleado ID 109.  
  
```sql  
IF OBJECT_ID (N'dbo.ufn_FindReports', N'TF') IS NOT NULL  
    DROP FUNCTION dbo.ufn_FindReports;  
GO  
CREATE FUNCTION dbo.ufn_FindReports (@InEmpID INTEGER)  
RETURNS @retFindReports TABLE   
(  
    EmployeeID int primary key NOT NULL,  
    FirstName nvarchar(255) NOT NULL,  
    LastName nvarchar(255) NOT NULL,  
    JobTitle nvarchar(50) NOT NULL,  
    RecursionLevel int NOT NULL  
)  
--Returns a result set that lists all the employees who report to the   
--specific employee directly or indirectly.*/  
AS  
BEGIN  
WITH EMP_cte(EmployeeID, OrganizationNode, FirstName, LastName, JobTitle, RecursionLevel) -- CTE name and columns  
    AS (  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, 0 -- Get the initial list of Employees for Manager n  
        FROM HumanResources.Employee e   
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        WHERE e.BusinessEntityID = @InEmpID  
        UNION ALL  
        SELECT e.BusinessEntityID, e.OrganizationNode, p.FirstName, p.LastName, e.JobTitle, RecursionLevel + 1 -- Join recursive member to anchor  
        FROM HumanResources.Employee e   
            INNER JOIN EMP_cte  
            ON e.OrganizationNode.GetAncestor(1) = EMP_cte.OrganizationNode  
INNER JOIN Person.Person p   
ON p.BusinessEntityID = e.BusinessEntityID  
        )  
-- copy the required columns to the result of the function   
   INSERT @retFindReports  
   SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
   FROM EMP_cte   
   RETURN  
END;  
GO  
```  
  
En el ejemplo siguiente se invoca la función y se especifica el identificador de cliente 1.  
  
```sql  
SELECT EmployeeID, FirstName, LastName, JobTitle, RecursionLevel  
FROM dbo.ufn_FindReports(1);  
```  

> [!NOTE]  
> Para obtener más información y ejemplos de funciones insertadas con valores de tabla (TVF insertadas) y funciones con valores de tabla de varias instrucciones (MSTVF), vea [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md). 

## <a name="best-practices"></a>Prácticas recomendadas  
Si una función definida por el usuario (UDF) no se crea con la cláusula `SCHEMABINDING`, los cambios que se realicen en los objetos subyacentes pueden afectar a la definición de la función y generar resultados inesperados al invocarla. Recomendamos implementar uno de los siguientes métodos para garantizar que la función no queda sin actualizar como consecuencia de los cambios realizados en sus objetos subyacentes:  
  
-   Especifique la cláusula `WITH SCHEMABINDING` cuando vaya a crear la UDF. Así se asegura de que no se pueden modificar los objetos a los que se hace referencia en la definición de la función a menos que también se modifique la función.  
  
-   Ejecute el procedimiento almacenado [sp_refreshsqlmodule](../../relational-databases/system-stored-procedures/sp-refreshsqlmodule-transact-sql.md) después de modificar cualquier objeto que se especifique en la definición de la UDF.  

Si se va a crear una UDF que no tiene acceso a datos, especifique la opción `SCHEMABINDING`. Esto impedirá que el optimizador de consultas genere operadores de cola de impresión innecesarios para planes de consultas que implican estas UDF. Para obtener más información sobre las colas de impresión, vea [Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md). Para obtener más información sobre la creación de una función enlazada a un esquema, vea [Funciones enlazadas a esquema](../../relational-databases/user-defined-functions/user-defined-functions.md#SchemaBound).

Es posible unirse a una MSTVF en una cláusula `FROM`, pero puede provocar un rendimiento incorrecto. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede usar todas las técnicas optimizadas en algunas instrucciones que se pueden incluir en una MSTVF, lo que conllevaría que el plan de consulta no alcanzase un nivel óptimo. Para lograr el mejor rendimiento, siempre que sea posible, use combinaciones entre las tablas base en lugar de funciones.  

> [!IMPORTANT]
> Las MSTVF tienen una estimación de cardinalidad fija de 100 a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], y de 1 en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
> A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], la optimización de un plan de ejecución en el que se usan MSTVF puede aprovechar la ejecución intercalada, lo que resulta en el uso de la cardinalidad real en lugar de la heurística anterior.     
> Para obtener más información, vea [Ejecución intercalada de funciones con valores de tabla de múltiples instrucciones](../../relational-databases/performance/intelligent-query-processing.md#interleaved-execution-for-mstvfs).

> [!NOTE]  
> ANSI_WARNINGS no se respeta al pasar parámetros en un procedimiento almacenado o una función definida por el usuario, ni cuando se declaran y se establecen variables en una instrucción por lotes. Por ejemplo, si una variable se define como **char(3)** y después se establece en un valor de más de tres caracteres, los datos se truncan hasta el tamaño definido y la instrucción `INSERT` o `UPDATE` se ejecuta correctamente.

## <a name="see-also"></a>Vea también  
 [Funciones definidas por el usuario](../../relational-databases/user-defined-functions/user-defined-functions.md)     
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)    
 [ALTER FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-function-transact-sql.md)    
 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)     
 [DROP PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-partition-function-transact-sql.md)    
 Obtenga más ejemplos en la [comunidad](https://www.bing.com/search?q=user%20defined%20function%20%22sql%20server%202016%22%20examples&qs=n&form=QBRE&pq=user%20defined%20function%20%22sql%20server%202016%22%20examples&sc=0-48&sp=-1&sk=&cvid=C3AD337125A840AD9EEFA3AAC36A3712)   
  
