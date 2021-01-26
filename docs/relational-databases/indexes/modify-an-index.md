---
description: Modificar un índice
title: Modificación de un índice | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], modifying
- modifying indexes
- index changes [SQL Server]
ms.assetid: 97e3110d-fde7-4f5d-9309-dc1697960aeb
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 0b653936dd08b5c124fd1249697dec35e9ae7ba9
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766319"
---
# <a name="modify-an-index"></a>Modificar un índice
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx_md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  En este tema se describe cómo modificar un índice en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!IMPORTANT]  
>  Los índices creados como resultado de una restricción PRIMARY KEY o UNIQUE no se pueden modificar con este método. En su lugar, se debe modificar la restricción.  
  
 **En este tema**  
  
-   **Para modificar un índice, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-modify-an-index"></a>Para modificar un índice  
  
1.  En el Explorador de objetos, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y expándala.  
  
2.  Expanda **Bases de datos**, a continuación, la base de datos a la que pertenece la tabla y, por último, **Tablas**.  
  
3.  Expanda la tabla a la que pertenece el índice y, a continuación, **Índices**.  
  
4.  Haga clic con el botón derecho en el índice que quiera modificar y, después, haga clic en **Propiedades**.  
  
5.  En el cuadro de diálogo **Propiedades del índice** , realice los cambios deseados. Por ejemplo, puede agregar o quitar una columna de la clave de índice, o cambiar el valor de una opción de índice.  
  
#### <a name="to-modify-index-columns"></a>Para modificar columnas de índices  
  
1.  Para agregar, quitar o cambiar la posición de una columna de índice, seleccione la página **General** del cuadro de diálogo **Propiedades del índice** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-modify-an-index"></a>Para modificar un índice  
  
En el ejemplo siguiente se quita y se vuelve a crear un índice existente en la columna `ProductID` de la tabla `Production.WorkOrder` en la base de datos de AdventureWorks mediante la opción `DROP_EXISTING`. También se establecen las opciones `FILLFACTOR` y `PAD_INDEX` .  
  
[!code-sql[IndexDDL#CreateIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_1.sql)]  
  
En el ejemplo siguiente se usa ALTER INDEX para establecer varias opciones del índice `AK_SalesOrderHeader_SalesOrderNumber`.  
  
[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/modify-an-index_2.sql)]  
  
#### <a name="to-modify-index-columns"></a>Para modificar columnas de índices  
  
1.  Para agregar, quitar o cambiar la posición de una columna de índice, debe quitar y volver a crear el índice.  
  
## <a name="see-also"></a>Vea también  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)   
 [Establecer opciones de índice](../../relational-databases/indexes/set-index-options.md)   
 [Cambiar el nombre a los índices](../../relational-databases/indexes/rename-indexes.md)  
  
  
