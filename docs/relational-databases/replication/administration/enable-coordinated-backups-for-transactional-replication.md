---
title: Habilitación de copias de seguridad coordinadas (transaccional)
description: Obtenga información sobre cómo habilitar las copias de seguridad coordinadas en la base de datos de distribución para que el registro de transacciones de la base de datos de publicación de replicación transaccional no se trunque hasta que se haya realizado una copia de seguridad de las transacciones que se hayan propagado al distribuidor.
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- transactional replication, backup and restore
- sp_replicationdboption
- sync with backup [SQL Server replication]
- coordinated backups [SQL Server replication]
- backups [SQL Server replication], transactional replication
ms.assetid: 73a914ba-8b2d-4f4d-ac1b-db9bac676a30
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 34911188162ac5b63f5a43d510d10d503820d200
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170477"
---
# <a name="enable-coordinated-backups-for-transactional-replication"></a>Habilitar copias de seguridad coordinadas para la replicación transaccional
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Al habilitar una base de datos para la replicación transaccional, puede especificar que se tenga que realizar una copia de seguridad de todas las transacciones antes de ser entregadas a la base de datos de distribución. También puede habilitar la copia de seguridad coordinada en la base de datos de distribución para que el registro de transacciones de la base de datos de publicación no se trunque hasta que se haya realizado una copia de seguridad de las transacciones que se han propagado al distribuidor. Para más información, consulte [Estrategias para hacer copias de seguridad y restaurar replicación de instantáneas o replicación transaccional](../../../relational-databases/replication/administration/strategies-for-backing-up-and-restoring-snapshot-and-transactional-replication.md).  
  
### <a name="to-enable-coordinated-backups-for-a-database-published-with-transactional-replication"></a>Para habilitar las copias de seguridad coordinadas de una base de datos publicada con replicación transaccional  
  
1.  En el publicador, use la función `SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')` [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) para devolver la propiedad **IsSyncWithBackup** de la base de datos de publicación. Si la función devuelve **1**, las copias de seguridad coordinadas ya están habilitadas para la base de datos publicada.  
  
2.  Si la función del paso 1 devuelve **0**, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) en la base de datos de publicación del publicador. Especifique un valor de **sync with backup** para **\@optname** y **true** para **\@value**.  
  
    > [!NOTE]  
    >  Si cambia la opción **sync with backup** a **false**, el punto de la truncación de la base de datos de publicación estará actualizado después de que el Agente de registro del LOG se ejecute o después de un intervalo si el Agente de registro del LOG está ejecutando permanentemente. El parámetro del agente **–MessageInterval** , que tiene un valor predeterminado de 30 segundos, controla el intervalo máximo.  
  
### <a name="to-enable-coordinated-backups-for-a-distribution-database"></a>Para habilitar las copias de seguridad coordinadas para una base de datos de distribución  
  
1.  En el distribuidor, use la función [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../../t-sql/functions/databasepropertyex-transact-sql.md) para devolver la propiedad **IsSyncWithBackup** de la base de datos de distribución. Si la función devuelve **1**, las copias de seguridad coordinadas ya están habilitadas para la base de datos de distribución.  
  
2.  Si la función del paso 1 devuelve **0**, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) en la base de datos de distribución del distribuidor. Especifique un valor de **sync with backup** para **\@optname** y **true** para **\@value**.  
  
### <a name="to-disable-coordinated-backups"></a>Para deshabilitar las copias de seguridad coordinadas  
  
1.  En el publicador de la base de datos de publicación o en el distribuidor de la base de datos de distribución, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Especifique un valor de **sync with backup** para **\@optname** y **false** para **\@value**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-retrieve-the-issyncwithbackup-property-for-the-current-database"></a>A. Recuperación de la propiedad `IsSyncWithBackup` para la base de datos actual

En este ejemplo se devuelve la propiedad `IsSyncWithBackup` para la base de datos actual:
  
```sql
SELECT DATABASEPROPERTYEX(DB_NAME(),'IsSyncWithBackup')`
```

### <a name="b-retrieve-the-issyncwithbackup-property-for-a-specific-database"></a>B. Recuperación de la propiedad `IsSyncWithBackup` para una base de datos concreta

En este ejemplo se devuelve la propiedad `IsSyncWithBackup` para la base de datos `NameOfDatabaseToCheck`:
  
```sql
SELECT DATABASEPROPERTYEX('NameOfDatabaseToCheck','IsSyncWithBackup')`
```
