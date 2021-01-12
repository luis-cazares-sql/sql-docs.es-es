---
description: sys.dm_os_host_info (Transact-SQL)
title: sys.dm_os_host_info (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/10/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
f1_keywords:
- sys.dm_os_host_info
- sys.dm_os_host_info_TSQL
- dm_os_host_info
- dm_os_host_info_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_host_info dynamic management view
ms.assetid: 9bb6ef86-957b-4ca1-ad20-ca2f8460a86d
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 41462fd558a30639342dc75be5bb68a44fcedf67
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097568"
---
# <a name="sysdm_os_host_info-transact-sql"></a>sys.dm_os_host_info (Transact-SQL)
[!INCLUDE[SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

Devuelve una fila que muestra información de versión del sistema operativo.  
  
|Nombre de la columna |Tipo de datos |Descripción |  
|-----------------|---------------|-----------------|  
|**host_platform** |**nvarchar(256)** |El tipo de sistema operativo: Windows o Linux |
|**host_distribution** |**nvarchar(256)** |Descripción del sistema operativo. |
|**host_release**|**nvarchar(256)**|Versión del sistema operativo [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows (número de versión). Para obtener una lista de valores y descripciones, vea [versión del sistema operativo (Windows)](/windows/desktop/SysInfo/operating-system-version). <br> Para Linux, devuelve una cadena vacía. |  
|**host_service_pack_level**|**nvarchar(256)**|Nivel de Service Pack del sistema operativo Windows. <br> Para Linux, devuelve una cadena vacía. |  
|**host_sku**|**int**|Identificador de referencia de almacén (SKU) de Windows. Para obtener una lista de identificadores y descripciones de SKU, consulte [función GetProductInfo](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getproductinfo). Acepta valores NULL. <br> Para Linux, devuelve NULL. |  
|**os_language_version**|**int**|Identificador de configuración regional (LCID) del sistema operativo Windows. Para obtener una lista de valores y descripciones de LCID, consulte ID. de [configuración regional asignados por Microsoft](/openspecs/windows_protocols/ms-lcid/a9eac961-e77d-41a6-90a5-ce1a8b0cdb9c). No puede ser NULL.|  

## <a name="remarks"></a>Observaciones  
Esta vista es similar a [Sys.dm_os_windows_info](../../relational-databases/system-dynamic-management-views/sys-dm-os-windows-info-transact-sql.md), agregando columnas para diferenciar Windows y Linux.
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
De `SELECT` `sys.dm_os_host_info` forma predeterminada, se concede el permiso para al `public` rol. Si se revoca, requiere `VIEW SERVER STATE` el permiso en el servidor.   
 
> [!CAUTION]
>  A partir de la versión [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] CTP 1,3, la [!INCLUDE[ssManStudioFull_md](../../includes/ssmanstudiofull-md.md)] versión 17 requiere el `SELECT` permiso en `sys.dm_os_host_info` para conectarse a [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] . Si `SELECT` se revoca el permiso desde `public` , solo los inicios de sesión con `VIEW SERVER STATE` permiso pueden conectarse con la versión más reciente de SSMS. (Otras herramientas, como `sqlcmd.exe` puede conectarse sin `SELECT` permiso en `sys.dm_os_host_info` ).

  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelven todas las columnas de la vista **Sys.dm_os_host_info** .  
  
```  
SELECT host_platform, host_distribution, host_release, 
    host_service_pack_level, host_sku, os_language_version  
FROM sys.dm_os_host_info;  
```  

A continuación se muestra un conjunto de resultados de ejemplo en Windows:
 
 |host_platform |host_distribution |host_release |host_service_pack_level |host_sku |os_language_version |
 |----- |----- |----- |----- |----- |----- |
 |Windows   |Windows Server 2012 R2 Standard    |6.3    |   |7  |3082 |  

A continuación se muestra un conjunto de resultados de ejemplo en Linux:
 
 |host_platform |host_distribution |host_release |host_service_pack_level |host_sku |os_language_version |
 |----- |----- |----- |----- |----- |----- |
 |Linux |Ubuntu |16.04  |   |NULL   |3082 |  

  
## <a name="see-also"></a>Vea también  
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_os_windows_info (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-windows-info-transact-sql.md)  
