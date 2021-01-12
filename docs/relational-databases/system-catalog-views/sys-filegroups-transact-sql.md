---
description: sys.filegroups (Transact-SQL)
title: Sys. grupos de archivos (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.filegroups_TSQL
- filegroups
- sys.filegroups
- filegroups_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filegroups catalog view
ms.assetid: 9e851f72-1f8e-4515-a25d-152ebc12ed56
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 205103ac734d4c5fe0028b85ad7e24c10cd9adea
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098415"
---
# <a name="sysfilegroups-transact-sql"></a>sys.filegroups (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila por cada espacio de datos que es un grupo de archivos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|Para obtener una lista de las columnas que hereda esta vista, vea [sys.data_spaces &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md).|  
|**filegroup_guid**|**uniqueidentifier**|GUID del grupo de archivos.<br /><br /> NULL = Grupo de archivos PRIMARY|  
|**log_filegroup_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)] En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el valor es NULL.|  
|**is_read_only**|**bit**|1 = El grupo de archivos es de solo lectura.<br /><br /> 0 = El grupo de archivos es de lectura/escritura.|  
|**is_autogrow_all_files**|**bit**|**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] a través de la [versión actual](../../sql-server/what-s-new-in-sql-server-2016.md)).<br /><br /> 1 = cuando un archivo del grupo de archivos cumple el umbral de crecimiento automático, se amplían todos los archivos del grupo de archivos.<br /><br /> 0 = cuando un archivo del grupo de archivos alcanza el umbral de crecimiento automático, solo crece ese archivo. Este es el valor predeterminado.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Espacios de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-spaces-transact-sql.md)  
  
