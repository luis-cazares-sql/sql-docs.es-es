---
description: Método WriteText
title: Método WriteText | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _Stream::raw_WriteText
- _Stream::WriteText
helpviewer_keywords:
- WriteText method [ADO]
ms.assetid: 7a669048-13f4-4574-a2b1-985e089729d5
author: rothja
ms.author: jroth
ms.openlocfilehash: d38d493fb57e8147f882056d07514ba9f405ecf9
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88987727"
---
# <a name="writetext-method"></a>Método WriteText
Escribe una cadena de texto especificada en un objeto de [secuencia](./stream-object-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Stream.WriteText Data, Options  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Data*  
 Valor de **cadena** que contiene el texto en caracteres que se va a escribir.  
  
 *Opciones*  
 Opcional. Valor [StreamWriteEnum](./streamwriteenum.md) que especifica si se debe escribir un carácter separador de línea al final de la cadena especificada.  
  
## <a name="remarks"></a>Observaciones  
 Las cadenas especificadas se escriben en el objeto de **secuencia** sin espacios ni caracteres intermedios entre cada cadena.  
  
 La [posición](./position-property-ado.md) actual se establece en el carácter que sigue a los datos escritos. El método **WRITETEXT** no trunca el resto de los datos de una secuencia. Si desea truncar estos caracteres, llame a [seteos](./seteos-method.md).  
  
 Si escribe más allá de la posición actual de [EOS](./eos-property.md) , se aumentará el [tamaño](./size-property-ado-stream.md) de la **secuencia** para que contenga cualquier carácter nuevo y **EOS** se moverá al nuevo último byte de la **secuencia**.  
  
> [!NOTE]
>  El método **WRITETEXT** se usa con secuencias de texto (el[tipo](./type-property-ado-stream.md) es **adTypeText**). En el caso de secuencias binarias (el**tipo** es **adTypeBinary**), utilice [Write](./write-method.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de secuencia (ADO)](./stream-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Método Write](./write-method.md)