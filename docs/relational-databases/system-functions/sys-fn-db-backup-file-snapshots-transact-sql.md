---
description: sys.fn_db_backup_file_snapshots (Transact-SQL)
title: sys.fn_db_backup_file_snapshots (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2015
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 45010ff2-219f-4086-9ea4-016a6c17cddd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4976e0ae436fe1c9c59742b6580ae083b3e3c5c4
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094949"
---
# <a name="sysfn_db_backup_file_snapshots-transact-sql"></a>sys.fn_db_backup_file_snapshots (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve instantáneas de Azure asociadas a los archivos de base de datos. Si no se encuentra la base de datos especificada o si los archivos de base de datos no están almacenados en el servicio Microsoft Azure BLOB Storage, no se devuelve ninguna fila. Use esta función del sistema junto con el **Sys.sp_delete_backup_file_snapshot** procedimiento almacenado del sistema para identificar y eliminar instantáneas de copia de seguridad huérfanas. Para obtener más información, vea [Copias de seguridad de instantánea de archivos para archivos de base de datos de Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.fn_db_backup_file_snapshots   
   [ ( database_name ) ]  
```  
  
## <a name="arguments"></a>Argumentos  
 *Database_name*  
 Nombre de la base de datos que se consulta. Si es NULL, esta función se ejecuta en el ámbito de la base de datos actual.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|file_id|**int**|IDENTIFICADOR de archivo de la base de datos. No admite valores NULL.|  
|snapshot_time|**nvarchar(260)**|Marca de tiempo de la instantánea tal como la devuelve la API de REST. Devuelve NULL si no existe ninguna instantánea.|  
|snapshot_url|**nvarchar(360)**|Dirección URL completa de la instantánea del archivo. Devuelve NULL si no existe ninguna instantánea.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos.  
  
## <a name="see-also"></a>Consulte también  
 [sp_delete_backup_file_snapshot &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup-file-snapshot.md)   
 [sp_delete_backup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/snapshot-backup-sp-delete-backup.md)  
  
  
