---
description: sys.fn_virtualservernodes (Transact-SQL)
title: sys.fn_virtualservernodes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_virtualservernodes
- fn_virtualservernodes_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- nodes [Faillover Clustering], virtual servers
- nodes [Faillover Clustering]
- virtual servers [Faillover Clustering]
- failover clustering [SQL Server], nodes
- fn_virtualservernodes function
- sys.fn_virtualservernodes function
ms.assetid: 257f3b8d-93c0-4444-87f1-ea211bd8cad0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2096c3d2c7a73ef76598037a24780b9fe577760f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098769"
---
# <a name="sysfn_virtualservernodes-transact-sql"></a>sys.fn_virtualservernodes (Transact-SQL)
[!INCLUDE [sql-asdbmi-pdw](../../includes/applies-to-version/sql-asdbmi-pdw.md)]

  Devuelve una lista de nodos de la instancia en clúster de conmutación por error en los que se puede ejecutar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Esta información es útil en entornos de clústeres de conmutación por error.  
  
> [!IMPORTANT]
>  Esta [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] función del sistema se incluye por motivos de compatibilidad con versiones anteriores. En su lugar, se recomienda usar [sys.dm_os_cluster_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
fn_virtualservernodes()  
```  
  
## <a name="tables-returned"></a>Tablas devueltas  
 Si el servidor actual es un servidor en clúster, **fn_virtualservernodes** devuelve una lista de nodos de instancia en clúster de conmutación por error en los que se ha definido esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Si la instancia del servidor actual no es un servidor en clúster, **fn_virtualservernodes** devuelve un conjunto de filas vacío.  
  
## <a name="permissions"></a>Permisos  
 El usuario debe tener el permiso VIEW SERVER STATE para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se utiliza `fn_virtualservernodes` para realizar una consulta en una instancia de servidor en clúster:  
  
```  
SELECT * FROM fn_virtualservernodes();  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 NodeName  
  
 -------\-  
  
 SS3-CLUSN1  
  
 SS3-CLUSN2  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_os_cluster_nodes &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)   
 [sys.fn_servershareddrives &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-servershareddrives-transact-sql.md)  
  
  
