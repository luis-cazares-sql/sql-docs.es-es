---
description: Método getIdentifierQuoteString (SQLServerDatabaseMetaData)
title: Método getIdentifierQuoteString (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerDatabaseMetaData.getIdentifierQuoteString
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6dea35a0-56a8-412c-8cd3-6539527ff597
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 81e1f6f6d053df8cd4a81398b9776ceecd0f46bb
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88435937"
---
# <a name="getidentifierquotestring-method-sqlserverdatabasemetadata"></a>Método getIdentifierQuoteString (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Recupera el objeto **String** que se utiliza para entrecomillar los identificadores de SQL.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public java.lang.String getIdentifierQuoteString()  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Objeto **String** que contiene los identificadores de las comillas.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método getIdentifierQuoteString especifica este método getIdentifierQuoteString en la interfaz java.sql.DatabaseMetaData.  
  
 Al utilizar el controlador JDBC de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] con una base de datos de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], este método devolverá comillas **double** ("").  
  
## <a name="see-also"></a>Consulte también  
 [Métodos SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [Miembros SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [Clase SQLServerDatabaseMetaData](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
