---
description: 'Insertar: comando SQL'
title: Comando INSERT-SQL | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- INSERT [ODBC]
ms.assetid: 9b648198-349f-46f6-b869-13d129945971
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 92c4b2068149164716d52fd3693e56164ab788ab
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449507"
---
# <a name="insert---sql-command"></a>Insertar: comando SQL
Anexa un registro al final de una tabla que contiene los valores de campo especificados.  
  
 El controlador ODBC de Visual FoxPro admite la sintaxis nativa del lenguaje Visual FoxPro para este comando. Para obtener información específica del controlador, consulte la sección Comentarios.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
INSERT INTO dbf_name [(fname1 [, fname2, ...])]  
   VALUES (eExpression1 [, eExpression2, ...])  
```  
  
## <a name="arguments"></a>Argumentos  
 INSERTAR en *dbf_name*  
 Especifica el nombre de la tabla a la que se anexa el nuevo registro. *dbf_name* puede incluir una ruta de acceso y puede ser una expresión de nombre.  
  
 Si la tabla que especifica no está abierta, se abre exclusivamente en un nuevo área de trabajo y el nuevo registro se anexa a la tabla. El área de trabajo nuevo no está seleccionada; el área de trabajo actual permanece seleccionada.  
  
 Si la tabla especificada está abierta, INSERT anexa el nuevo registro a la tabla. Si la tabla está abierta en un área de trabajo diferente del área de trabajo actual, no se selecciona una vez anexado el registro. el área de trabajo actual permanece seleccionada.  
  
 [( *fname1*[, *fname2*[,...]])]  
 Especifica en el nuevo registro los nombres de los campos en los que se insertan los valores.  
  
 VALORES ( *eExpression1*[, *eExpression2*[,...]])  
 Especifica los valores de campo insertados en el nuevo registro. Si omite los nombres de campo, debe especificar los valores de campo en el orden definido por la estructura de la tabla.  
  
## <a name="remarks"></a>Observaciones  
 El nuevo registro contiene los datos enumerados en la cláusula VALUEs.  
  
## <a name="driver-remarks"></a>Notas del controlador  
 Cuando la aplicación envía la instrucción SQL de ODBC INSERT al origen de datos, el controlador ODBC de Visual FoxPro convierte el comando en el comando visual FoxProINSERT sin conversión.  
  
## <a name="see-also"></a>Consulte también  
 [Comando CREATE TABLE-SQL](../../odbc/microsoft/create-table-sql-command.md)   
 [Seleccione - comando SQL](../../odbc/microsoft/select-sql-command.md)
