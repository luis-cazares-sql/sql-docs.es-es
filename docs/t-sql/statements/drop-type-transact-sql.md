---
description: DROP TYPE (Transact-SQL)
title: DROP TYPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP TYPE
- DROP_TYPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- user-defined types [SQL Server], deleting
- UDTs [SQL Server], deleting
- alias data types [SQL Server], removing
- DROP TYPE statement
ms.assetid: 11bf83f9-0718-4238-a835-83d2eb14ae7b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9668d465aad45c0f3aae221e233be82bf66a2fa2
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688957"
---
# <a name="drop-type-transact-sql"></a>DROP TYPE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita de la base datos actual un tipo de datos de alias o un tipo definido por el usuario de Common Language Runtime (CLR).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP TYPE [ IF EXISTS ] [ schema_name. ] type_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
 **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] hasta la [versión actual](../../sql-server/what-s-new-in-sql-server-2016.md)).  
  
 Quita el tipo condicionalmente solo si ya existe.  
  
 *schema_name*  
 Es el nombre del esquema al que pertenece el tipo de alias o el tipo definido por el usuario.  
  
 *type_name*  
 Es el nombre del tipo de datos de alias o del tipo definido por el usuario que desea quitar.  
  
## <a name="remarks"></a>Observaciones  
 La instrucción DROP TYPE no se ejecuta si se cumple alguna de las siguientes condiciones:  
  
-   Hay tablas en la base de datos que contienen columnas del tipo de datos de alias o del tipo definido por el usuario. Encontrará más información sobre las columnas de tipo de alias o de tipo definido por el usuario si consulta las vistas de catálogo [sys.columns](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md) o [sys.column_type_usages](../../relational-databases/system-catalog-views/sys-column-type-usages-transact-sql.md).  
  
-   Hay columnas calculadas, restricciones CHECK, vistas enlazadas a esquema y funciones enlazadas a esquema cuyas definiciones hacen referencia al tipo de alias o a un tipo definido por el usuario. Encontrará más información sobre estas referencias si consulta la vista de catálogo [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md).  
  
-   Hay funciones, procedimientos almacenados o desencadenadores creados en la base de datos, y estas rutinas utilizan variables y parámetros de tipo de alias o de tipo definido por el usuario. Encontrará más información sobre los parámetros de tipo de alias o de tipo definido por el usuario si consulta las vistas de catálogo [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md) o [sys.parameter_type_usages](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Se necesita el permiso CONTROL en *type_name* o el permiso ALTER en *schema_name*.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se supone que ya se ha creado un tipo denominado `ssn` en la base de datos actual.  
  
```sql  
DROP TYPE ssn ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
