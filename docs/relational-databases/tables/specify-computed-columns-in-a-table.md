---
description: Especificar columnas calculadas en una tabla
title: Especificar columnas calculadas en una tabla | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- computed columns, define
ms.assetid: 731a4576-09c1-47f0-a8f6-edd0b55679f4
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb4b3f8a0efb8f62a94177c5a4546e3a30697ccb
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766068"
---
# <a name="specify-computed-columns-in-a-table"></a>Especificar columnas calculadas en una tabla

[!INCLUDE[tsql-appliesto-ss-asdb-xxxx-xxx-md](../../includes/applies-to-version/sql-asdb.md)]

Una columna calculada es una columna virtual que no está almacenada físicamente en la tabla, a menos que la columna esté marcada con PERSISTED. Las expresiones de columnas calculadas pueden utilizar datos de otras columnas al calcular un valor para la columna a la que pertenecen. Puede especificar una expresión para una columna calculada en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].

**En este tema**

- **Antes de empezar:**

   [Limitaciones y restricciones](#Limitations)

   [Seguridad](#Security)

- **Para especificar una columna calculada con:**

   [SQL Server Management Studio](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar

### <a name="limitations-and-restrictions"></a><a name="Limitations"></a> Limitaciones y restricciones

- Una columna calculada no puede utilizarse como definición de restricción DEFAULT o FOREIGN KEY ni como NOT NULL. No obstante, si el valor de columna calculada lo define una expresión determinista y se permite el tipo de datos del resultado en columnas de índice, se puede utilizar una columna calculada como columna de clave en un índice o como parte de cualquier restricción PRIMARY KEY o UNIQUE. Por ejemplo, si la tabla tiene las columnas de tipo entero a y b, la columna calculada a + b se puede indizar, pero la columna calculada a + DATEPART(dd, GETDATE()) no, porque el valor podría variar en llamadas posteriores.
- Una columna calculada no puede ser el destino de una instrucción INSERT o UPDATE.

### <a name="security"></a><a name="Security"></a> Seguridad

#### <a name="permissions"></a><a name="Permissions"></a> Permisos

Requiere el permiso ALTER en la tabla.

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio

### <a name="to-add-a-new-computed-column"></a><a name="NewColumn"></a> Para agregar una nueva columna calculada

1. En el **Explorador de objetos**, expanda la tabla para la que desea agregar la nueva columna calculada. Haga clic con el botón derecho en **Columnas** y seleccione **Nueva columna**.
2. Escriba el nombre de columna y acepte el tipo de datos predeterminado (**nchar**(10)). El [!INCLUDE[ssDE](../../includes/ssde-md.md)] determina el tipo de datos de la columna calculada aplicando las reglas de prioridad de tipo de datos a las expresiones especificadas en la fórmula. Por ejemplo, si la fórmula hace referencia a una columna de tipo **money** y una columna de tipo **int**, la columna calculada será de tipo **money** porque ese tipo de datos tiene mayor prioridad. Para obtener más información, vea [Prioridad de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).
3. En la pestaña **Propiedades de columna**, expanda la propiedad **Especificación de columna calculada**.
4. En la propiedad secundaria **(Fórmula)** , escriba la expresión de esta columna en la celda de la cuadrícula situada a la derecha. Por ejemplo, en una columna `SalesTotal` , la fórmula que escribe puede ser `SubTotal+TaxAmt+Freight`, que suma el valor de estas columnas para cada fila de la tabla.

   > [!IMPORTANT]
   > Cuando una fórmula combina dos expresiones de tipos de datos distintos, las reglas de prioridad de tipo de datos especifican que el tipo de datos con la prioridad menor se convierta al tipo de datos con la prioridad mayor. Si la conversión no es una conversión implícita admitida, se devuelve el error "`Error validating the formula for column column_name.`". Use la función CAST o CONVERT para resolver el conflicto de tipos de datos. Por ejemplo, si una columna de tipo **nvarchar** se combina con una de tipo **int**, el tipo entero debe convertirse a **nvarchar** como se muestra en esta fórmula: `('Prod'+CONVERT(nvarchar(23),ProductID))`. Para obtener más información, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md).

5. Indique si los datos se van a conservar; para ello, elija **Sí** o **No** en el menú desplegable de la propiedad secundaria **Es persistente** .

6. En el menú **Archivo**, haga clic en ***Guardar** _nombre de tabla_.

#### <a name="to-add-a-computed-column-definition-to-an-existing-column"></a>Para agregar una definición de columna calculada a una columna existente

1. En el **Explorador de objetos**, haga clic con el botón derecho en la tabla que contenga la columna que quiera cambiar y expanda la carpeta **Columnas** .
2. Haga clic con el botón derecho en la columna para la que quiera especificar una fórmula de columna calculada y haga clic en **Eliminar**. Haga clic en **Aceptar**.
3. Agregue una nueva columna y especifique la fórmula de columna calculada siguiendo el procedimiento anterior para agregar una nueva columna calculada.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL

### <a name="to-add-a-computed-column-when-creating-a-table"></a>Para agregar una columna calculada al crear una tabla

En el ejemplo siguiente se crea una tabla con una columna calculada que multiplica el valor de la columna `QtyAvailable` tantas veces como indique el valor de la columna `UnitPrice`.

```sql
CREATE TABLE dbo.Products
   (
      ProductID int IDENTITY (1,1) NOT NULL
      , QtyAvailable smallint
      , UnitPrice money
      , InventoryValue AS QtyAvailable * UnitPrice
    )
;
-- Insert values into the table.
INSERT INTO dbo.Products (QtyAvailable, UnitPrice)
   VALUES (25, 2.00), (10, 1.5)
;
-- Display the rows in the table.
SELECT ProductID, QtyAvailable, UnitPrice, InventoryValue
FROM dbo.Products
;
```

### <a name="to-add-a-new-computed-column-to-an-existing-table"></a>Para agregar una nueva columna calculada a una tabla existente

En el ejemplo siguiente se agrega una columna nueva a la tabla creada en el ejemplo anterior.

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Si lo desea, agregue el argumento PERSISTED para almacenar físicamente los valores calculados en la tabla:

```sql
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5) PERSISTED
;
```

### <a name="to-change-an-existing-column-to-a-computed-column"></a>Para cambiar una columna existente a una columna calculada

En el ejemplo siguiente se modifica la columna agregada en el ejemplo anterior.

```sql
ALTER TABLE dbo.Products DROP COLUMN RetailValue
;
GO
ALTER TABLE dbo.Products ADD RetailValue AS (QtyAvailable * UnitPrice * 1.5)
;
```

Para obtener más información, vea [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md).
