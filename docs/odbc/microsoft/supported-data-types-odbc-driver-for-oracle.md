---
description: Tipos de datos admitidos (controlador ODBC para Oracle)
title: Tipos de datos admitidos (controlador ODBC para Oracle) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- data types [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], data types
ms.assetid: 21d5f8d9-a3aa-4aa4-bc37-ff8bc90c0870
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d53cdb9a270149866b2a480496f39adcb20e4d56
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88471517"
---
# <a name="supported-data-types-odbc-driver-for-oracle"></a>Tipos de datos admitidos (controlador ODBC para Oracle)
> [!IMPORTANT]  
>  Esta característica se quitará en una versión futura de Windows. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. En su lugar, utilice el controlador ODBC proporcionado por Oracle.  
  
 El controlador ODBC para Oracle admite todos los tipos de datos de Oracle 7,3; sin embargo, no es compatible con ninguno de los nuevos tipos de datos de Oracle8 que se enumeran aquí.  
  
|Tipo de datos|Oracle 7,3|Oracle8|  
|---------------|----------------|-------------|  
|BFILE|N/D|No compatible|  
|BLOB|N/D|No compatible|  
|CHAR|Compatible|Compatible|  
|CLOB|N/D|No compatible|  
|DATE|Compatible|Compatible|  
|FLOAT|Compatible|Compatible|  
|INTEGER|Compatible|Compatible|  
|LONG|Compatible|Compatible|  
|LONG RAW|Compatible|Compatible|  
|NCHAR|N/D|No compatible|  
|NCLOB|N/D|No compatible|  
|NUMBER|Compatible|Compatible|  
|NVARCHAR2|N/D|No compatible|  
|RAW|Compatible|Compatible|  
|VARCHAR2|Compatible|Compatible|  
|MLSLABEL|No compatible.|No compatible.|  
  
> [!NOTE]  
>  Para obtener más información sobre el tamaño permitido de la columna VARCHAR, vea [tamaño de columna VARCHAR](../../odbc/microsoft/varchar-column-size-odbc-driver-for-oracle.md) en esta guía.
