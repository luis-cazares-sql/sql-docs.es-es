---
description: sysopentapes (Transact-SQL)
title: sysopentapes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sysopentapes
- sysopentapes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- backup media [SQL Server], sysopentapes system table
- sysopentapes system table
ms.assetid: c066ca9b-9cfd-46b1-90a3-5c8dc9e7b6ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0cb05c0ac88f7f362651b872fa84598a656ff25d
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096066"
---
# <a name="sysopentapes-transact-sql"></a>sysopentapes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada dispositivo de cinta abierto. Esta vista se almacena en la base de datos **maestra** .  
  
> [!IMPORTANT]  
>  Esta tabla del sistema se incluye como una vista para la compatibilidad con versiones anteriores. En su lugar, use el sys.dm_io_backup_tapes &#40;la vista de administración dinámica [de Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-io-backup-tapes-transact-sql.md) .  
  
> [!NOTE]  
>  No se puede quitar la vista **sysopentapes** .  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**openTape**|**nvarchar (64)**|Nombre de archivo físico del dispositivo de cinta abierto. Para obtener más información acerca de la apertura y liberación de dispositivos de cinta, vea [BACKUP &#40;Transact-sql&#41;](../../t-sql/statements/backup-transact-sql.md) y [restore &#40;transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).|  
  
## <a name="permissions"></a>Permisos  
 El usuario necesita tener el permiso VIEW SERVER STATE en el servidor.  
  
  
