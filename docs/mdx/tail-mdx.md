---
description: Tail (MDX)
title: Cola (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 2f9cd5c397ac87826db20509e665b2215bd8db91
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88466487"
---
# <a name="tail-mdx"></a>Tail (MDX)


  Devuelve un subconjunto del final de un conjunto.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Tail(Set_Expression [ ,Count ] )  
```  
  
## <a name="arguments"></a>Argumentos  
 *Set_Expression*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
 *Recuento*  
 Expresión numérica válida que especifica el número de tuplas que serán devueltas.  
  
## <a name="remarks"></a>Observaciones  
 La función **tail** devuelve el número especificado de tuplas desde el final del conjunto especificado. Se conserva el orden de los elementos. El valor predeterminado de *Count* es 1. Si el número especificado de tuplas es menor que 1, la función devuelve un conjunto vacío. Si el número especificado de tuplas supera al número de tuplas del conjunto, la función devuelve el conjunto original.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente devuelve la medida Reseller Sales de las cinco subcategorías de productos más vendidos, independientemente de su jerarquía, de acuerdo con Reseller Gross Profit. La función **tail** se utiliza para devolver solo los cinco últimos conjuntos del resultado después de que el resultado se ordene de forma inversa mediante la función **Order** .  
  
```  
SELECT Tail  
   (Order   
      ([Product].[Product Categories].[SubCategory].members  
         ,[Measures].[Reseller Gross Profit]  
         ,BASC  
      )  
   ,5  
   ) ON 0  
FROM [Adventure Works]  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
