---
description: Método setBinaryStream (int, java.io.InputStream, long)
title: Método setBinaryStream (int, java.io.InputStream, long) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 4ab2e2f3-eaf0-471a-8422-2cf98ce979cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e7f7b9564c6ae1c7dd1793d8c76a890807686e6d
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88432487"
---
# <a name="setbinarystream-method-int-javaioinputstream-long"></a>Método setBinaryStream (int, java.io.InputStream, long)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Establece el parámetro designado en el flujo de entrada especificado, que tendrá el número de bytes indicado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
public final void setBinaryStream(int parameterIndex,  
                                 java.io.InputStream x,  
                                          long length)  
```  
  
#### <a name="parameters"></a>Parámetros  
 *parameterIndex*  
  
 Valor **int** que indica el número de parámetro.  
  
 *x*  
  
 Un objeto java.io.InputStream.  
  
 *length*  
  
 Un valor **long** que indica el número de bytes.  
  
## <a name="exceptions"></a>Excepciones  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>Observaciones  
 El método setBinaryStream especifica este método setBinaryStream en la interfaz java.sql.PreparedStatement.  
  
 Si la longitud del flujo es distinta a la especificada en el parámetro *length*, el controlador JDBC produce una excepción cuando la fila se actualiza o se inserta.  
  
 Si se desconoce la longitud del flujo, el parámetro *length* puede establecerse en -1 para indicar que el controlador debe aceptar el flujo independientemente de su longitud. Con sqljdbc4.jar, se recomienda usar el método [setBinaryStream Method &#40;int, java.io.InputStream&#41;](../../../connect/jdbc/reference/setbinarystream-method-int-java-io-inputstream.md) de JDBC 40 si la aplicación quiere actualizar la columna a partir de un flujo cuya longitud se desconoce.  
  
## <a name="see-also"></a>Consulte también  
 [Método setBinaryStream &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setbinarystream-method-sqlserverpreparedstatement.md)   
 [Miembros de SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
