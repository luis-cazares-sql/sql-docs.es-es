---
description: DROP SYNONYM (Transact-SQL)
title: DROP SYNONYM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP SYNONYM
- DROP_SYNONYM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting synonyms
- synonyms [SQL Server], removing
- removing synonyms
- DROP SYNONYM statement
- dropping synonyms
ms.assetid: 23578932-e4de-4c39-a5a0-ce45139c4269
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a910ab091d961a48b7ff7822cb01924f1c9b1f82
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170387"
---
# <a name="drop-synonym-transact-sql"></a>DROP SYNONYM (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita un sinónimo de un esquema especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DROP SYNONYM [ IF EXISTS ] [ schema. ] synonym_name  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *IF EXISTS*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] hasta la [versión actual](https://go.microsoft.com/fwlink/p/?LinkId=299658))
  
 Quita condicionalmente el sinónimo solo si ya existe.  
  
 *schema*  
 Especifica el esquema en el que existe el sinónimo. Si no se especifica, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza el esquema predeterminado del usuario actual.  
  
 *synonym_name*  
 Es el nombre del sinónimo que se va a quitar.  
  
## <a name="remarks"></a>Observaciones  
 Las referencias a sinónimos no están enlazadas al esquema, por lo que un sinónimo se puede quitar cuando se desee. Las referencias a sinónimos quitados solo se encontrarán en tiempo de ejecución.  
  
 Es posible crear, quitar y hacer referencia a sinónimos en SQL dinámico.  
  
## <a name="permissions"></a>Permisos  
 Para quitar un sinónimo, un usuario debe cumplir al menos una de las condiciones siguientes. El usuario debe ser:  
  
-   El propietario actual del sinónimo.  
  
-   Receptor del permiso CONTROL en el sinónimo.  
  
-   Receptor del permiso ALTER SCHEMA en el esquema contenedor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente, primero se crea el sinónimo `MyProduct` y después se quita.  
  
```sql  
USE tempdb;  
GO  
-- Create a synonym for the Product table in AdventureWorks2012.  
CREATE SYNONYM MyProduct  
FOR AdventureWorks2012.Production.Product;  
GO  
-- Drop synonym MyProduct.  
USE tempdb;  
GO  
DROP SYNONYM MyProduct;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SYNONYM &#40;Transact-SQL&#41;](../../t-sql/statements/create-synonym-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
