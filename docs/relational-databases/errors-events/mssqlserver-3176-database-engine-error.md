---
description: MSSQLSERVER_3176
title: MSSQLSERVER_3176 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 3176 (Database Engine error)
ms.assetid: 4be24c64-2d52-4cb4-b4d7-36efbe4555b6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d4061c4da9243a945f2c299b6561f2a11f399638
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88476239"
---
# <a name="mssqlserver_3176"></a>MSSQLSERVER_3176
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|3176|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LDDB_FILE_CLAIM|  
|Texto del mensaje|El archivo '%ls' fue reclamado por '%ls'(%d) y '%ls'(%d). Se puede utilizar la cláusula WITH MOVE para cambiar la ubicación de uno o más archivos.|  
  
## <a name="explanation"></a>Explicación  
Se ha intentado utilizar un archivo para más de un propósito.  
  
### <a name="possible-causes"></a>Causas posibles  
Otra base de datos ya está utilizando el nombre de archivo.  
  
## <a name="user-action"></a>Acción del usuario  
Restaure los archivos de la base de datos en otra ubicación. En una instrucción RESTORE, utilice una cláusula WITH MOVE para mover cada archivo. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], cambie las ubicaciones de los archivos en la cuadrícula **Restaurar los archivos de base de datos como** del cuadro de diálogo **Opciones de Restaurar base de datos**.  
  
## <a name="see-also"></a>Consulte también  
[Restaurar una base de datos a una nueva ubicación &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
[Restaurar archivos en una nueva ubicación &#40;SQL Server&#41;](~/relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
[RESTORE &#40;Transact-SQL&#41;](~/t-sql/statements/restore-statements-transact-sql.md)  
  
