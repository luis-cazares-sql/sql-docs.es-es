---
title: Manipular datos UDT | Microsoft Docs
description: En este artículo se describe cómo insertar, seleccionar y actualizar datos en columnas UDT de una base de datos de SQL Server.
ms.custom: ''
ms.date: 12/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- data deletions [CLR integration]
- data insertions [CLR integration]
- CONVERT function
- data updates [CLR integration]
- comparing data
- updating data [CLR integration]
- removing data
- IsByteOrdered property
- variables [CLR integration]
- data manipulation [CLR integration]
- user-defined types [CLR integration], data manipulation
- deleting data
- UDTs [CLR integration], data manipulation
- invoking UDT methods
- inserting data
ms.assetid: 51b1a5f2-7591-4e11-bfe2-d88e0836403f
author: rothja
ms.author: jroth
ms.openlocfilehash: d23880a7ea6a1e8f4c1beccc5ec82f40303b9b76
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783653"
---
# <a name="working-with-user-defined-types---manipulating-udt-data"></a>Trabajar con tipos definidos por el usuario: manipular datos UDT
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  [!INCLUDE[tsql](../../includes/tsql-md.md)] no proporciona ninguna sintaxis especializada para las instrucciones INSERT, UPDATE o DELETE al modificar datos en columnas de tipo definido por el usuario (UDT). Para convertir los tipos de datos nativos al tipo UDT, se utilizan las funciones CAST o CONVERT de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="inserting-data-in-a-udt-column"></a>Insertar datos en una columna UDT  
 Las [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones siguientes insertan tres filas de datos de ejemplo en la tabla **Points** . El tipo de datos **Point** consta de valores enteros X e y que se exponen como propiedades del UDT. Debe utilizar la función CAST o CONVERT para convertir los valores X e y delimitados por comas en el tipo de **punto** . Las dos primeras instrucciones utilizan la función CONVERT para convertir un valor de cadena en el tipo de **punto** y la tercera instrucción usa la función CAST:  
  
```sql  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '3,4'));  
INSERT INTO dbo.Points (PointValue) VALUES (CONVERT(Point, '1,5'));  
INSERT INTO dbo.Points (PointValue) VALUES (CAST ('1,99' AS Point));  
```  
  
## <a name="selecting-data"></a>Seleccionar datos  
 La siguiente instrucción SELECT selecciona el valor binario del UDT.  
  
```sql  
SELECT ID, PointValue FROM dbo.Points  
```  
  
 Para ver la salida mostrada en un formato legible, llame al método **ToString** del UDT **Point** , que convierte el valor en su representación de cadena.  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points;  
```  
  
 Esto produce el siguiente resultado.  
  
```  
ID PointValue  
-- ----------  
 1 3,4  
 2 1,5  
 3 1,99  
```  
  
 También puede utilizar las funciones CAST y CONVERT de [!INCLUDE[tsql](../../includes/tsql-md.md)] para lograr los mismos resultados.  
  
```sql  
SELECT ID, CAST(PointValue AS varchar)   
FROM dbo.Points;  
  
SELECT ID, CONVERT(varchar, PointValue)   
FROM dbo.Points;  
```  
  
 El UDT **Point** expone sus coordenadas X e y como propiedades, que puede seleccionar individualmente. La siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] selecciona las coordenadas X e Y por separado:  
  
```sql  
SELECT ID, PointValue.X AS xVal, PointValue.Y AS yVal   
FROM dbo.Points;  
```  
  
 Las propiedades X e Y devuelven un valor entero, que se muestra en el conjunto de resultados.  
  
```  
ID xVal yVal  
-- ---- ----  
 1    3    4  
 2    1    5  
 3    1   99  
```  
  
## <a name="working-with-variables"></a>Trabajar con variables  
 Puede trabajar con variables si utiliza la instrucción DECLARE para asignar una variable a un tipo UDT. Las siguientes instrucciones asignan un valor mediante la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción set y muestran los resultados llamando al método **TOSTRING** del UDT en la variable:  
  
```sql  
DECLARE @PointValue Point;  
SET @PointValue = (SELECT PointValue FROM dbo.Points  
    WHERE ID = 2);  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 El conjunto de resultados muestra el valor variable:  
  
```  
PointValue  
----------  
-1,5  
```  
  
 Las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] logran el mismo resultado utilizando SELECT en lugar de SET para la asignación de variable:  
  
```sql  
DECLARE @PointValue Point;  
SELECT @PointValue = PointValue FROM dbo.Points  
    WHERE ID = 2;  
SELECT @PointValue.ToString() AS PointValue;  
```  
  
 La diferencia entre el uso de SELECT y SET para la asignación de variable es que SELECT permite asignar varias variables en una instrucción SELECT, mientras que la sintaxis de SET exige que cada asignación de variable tenga su propia instrucción SET.  
  
## <a name="comparing-data"></a>Comparar datos  
 Puede utilizar operadores de comparación para comparar valores en el UDT si ha establecido la propiedad **IsByteOrdered** en **true** al definir la clase. Para obtener más información, vea [crear un tipo de User-Defined](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types.md).  
  
```sql  
SELECT ID, PointValue.ToString() AS Points   
FROM dbo.Points  
WHERE PointValue > CONVERT(Point, '2,2');  
```  
  
 Puede comparar los valores internos del UDT sin tener en cuenta el valor de la opción **IsByteOrdered** si los propios valores son comparables. La siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] selecciona filas donde X es mayor que Y:  
  
```sql  
SELECT ID, PointValue.ToString() AS PointValue   
FROM dbo.Points  
WHERE PointValue.X < PointValue.Y;  
```  
  
 También puede utilizar operadores de comparación con variables, tal como se muestra en esta consulta que busca un PointValue correspondiente.  
  
```sql  
DECLARE @ComparePoint Point;  
SET @ComparePoint = CONVERT(Point, '3,4');  
SELECT ID, PointValue.ToString() AS MatchingPoint   
FROM dbo.Points  
WHERE PointValue = @ComparePoint;  
```  
  
## <a name="invoking-udt-methods"></a>Invocar métodos del UDT  
 También puede invocar métodos que se definen en el UDT en [!INCLUDE[tsql](../../includes/tsql-md.md)]. La clase **Point** contiene tres métodos, **Distance**, **DistanceFrom** y **DistanceFromXY**. Para ver las listas de código que definen estos tres métodos, consulte [codificación de tipos de User-Defined](../../relational-databases/clr-integration-database-objects-user-defined-types/creating-user-defined-types-coding.md).  
  
 La siguiente [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción llama al método **PointValue. Distance** :  
  
```sql  
SELECT ID, PointValue.X AS [Point.X],   
    PointValue.Y AS [Point.Y],  
    PointValue.Distance() AS DistanceFromZero   
FROM dbo.Points;  
```  
  
 Los resultados se muestran en la columna **Distance** :  
  
```  
ID X  Y  Distance  
-- -- -- ----------------  
 1  3  4                5  
 2  1  5 5.09901951359278  
 3  1 99 99.0050503762308  
```  
  
 El método **DistanceFrom** toma un argumento de tipo de datos **Point** y muestra la distancia desde el punto especificado hasta PointValue:  
  
```sql  
SELECT ID, PointValue.ToString() AS Pnt,  
   PointValue.DistanceFrom(CONVERT(Point, '1,99')) AS DistanceFromPoint  
FROM dbo.Points;  
```  
  
 Los resultados muestran los resultados del método **DistanceFrom** para cada fila de la tabla:  
  
```  
ID Pnt DistanceFromPoint  
-- --- -----------------  
 1 3,4  95.0210502993942  
 2 1,5                94  
 3 1,9                90  
```  
  
 El método **DistanceFromXY** toma los puntos individualmente como argumentos:  
  
```sql  
SELECT ID, PointValue.X as X, PointValue.Y as Y,   
PointValue.DistanceFromXY(1, 99) AS DistanceFromXY   
FROM dbo.Points  
```  
  
 El conjunto de resultados es el mismo que el método **DistanceFrom** .  
  
## <a name="updating-data-in-a-udt-column"></a>Actualizar datos en una columna UDT  
 Para actualizar datos en una columna UDT, utilice la instrucción UPDATE de [!INCLUDE[tsql](../../includes/tsql-md.md)]. También puede utilizar un método del UDT para actualizar el estado del objeto. La siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] actualiza una única fila en la tabla:  
  
```sql  
UPDATE dbo.Points  
SET PointValue = CAST('1,88' AS Point)  
WHERE ID = 3  
```  
  
 También puede actualizar los elementos UDT por separado. La siguiente instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] solo actualiza la coordenada Y:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Si el UDT se ha definido con el orden de bytes establecido en **true**, [!INCLUDE[tsql](../../includes/tsql-md.md)] puede evaluar la columna UDT en una cláusula WHERE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = '4,5'  
WHERE PointValue = '3,4';  
```  
  
### <a name="updating-limitations"></a>Limitaciones de la actualización  
 No puede actualizar varias propiedades a la vez mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por ejemplo, la siguiente instrucción UPDATE genera un error porque no se puede usar el mismo nombre de columna dos veces en una instrucción UPDATE.  
  
```sql  
UPDATE dbo.Points  
SET PointValue.X = 5, PointValue.Y = 99  
WHERE ID = 3  
```  
  
 Para actualizar individualmente cada punto, debe crear un método mutador en el ensamblado UDT de Point. A continuación, puede invocar el método mutador para actualizar el objeto en una instrucción UPDATE de [!INCLUDE[tsql](../../includes/tsql-md.md)], como en el siguiente ejemplo:  
  
```sql  
UPDATE dbo.Points  
SET PointValue.SetXY(5, 99)  
WHERE ID = 3  
```  
  
## <a name="deleting-data-in-a-udt-column"></a>Eliminar datos en una columna UDT  
 Para eliminar datos en un UDT, utilice la instrucción DELETE de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La siguiente instrucción elimina todas las filas de la tabla que coinciden con los criterios especificadas en la cláusula WHERE. Si omite la cláusula WHERE de una instrucción DELETE, se eliminarán todas las filas de la tabla.  
  
```sql  
DELETE FROM dbo.Points  
WHERE PointValue = CAST('1,99' AS Point)  
```  
  
 Utilice la instrucción UPDATE si desea quitar los valores de una columna UDT y conservar los valores de otra fila intactos. En este ejemplo se establece PointValue en NULL.  
  
```sql  
UPDATE dbo.Points  
SET PointValue = null  
WHERE ID = 2  
```  
  
## <a name="see-also"></a>Consulte también  
 [Trabajar con tipos definidos por el usuario en SQL Server](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)  
  
  
