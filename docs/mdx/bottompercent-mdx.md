---
description: BottomPercent (MDX)
title: BottomPercent (MDX) | Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 1b3ce341526270335564e9f67ccb89612854e098
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88494988"
---
# <a name="bottompercent-mdx"></a>BottomPercent (MDX)


  Ordena un conjunto de forma ascendente y devuelve un conjunto de tuplas con los valores más bajos con un total acumulado mayor o igual a un porcentaje especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
BottomPercent(Set_Expression, Percentage, Numeric_Expression)   
```  
  
## <a name="arguments"></a>Argumentos  
 *Set_Expression*  
 Expresión MDX (Expresiones multidimensionales) válida que devuelve un conjunto.  
  
 *Porcentaje*  
 Expresión numérica válida que especifica el porcentaje de tuplas que serán devueltas.  
  
 *Numeric_Expression*  
 Expresión numérica válida que suele ser una expresión MDX de las coordenadas de celdas que devuelven un número.  
  
## <a name="remarks"></a>Observaciones  
 La función **BottomPercent** calcula la suma de la expresión numérica especificada evaluada sobre un conjunto especificado, ordenando el conjunto en orden ascendente. A continuación, la función devuelve los elementos con los valores más bajos cuyo porcentaje acumulado del valor total sumado sea al menos el porcentaje especificado. Esta función devuelve el subconjunto más pequeño de un conjunto cuyo total acumulado sea al menos el porcentaje especificado. Los elementos devueltos se ordenan de mayor a menor.  
  
> [!IMPORTANT]  
>  La función **BottomPercent** , al igual que la función de [porcentaje](../mdx/toppercent-mdx.md) , siempre rompe la jerarquía. Para obtener más información, vea la función Order.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente devuelve, para la categoría Bike, el conjunto más pequeño de miembros del nivel City de la jerarquía Geography de la dimensión Geography para el año fiscal 2003, cuyo total acumulado mediante la medida Reseller Sales Amount sea al menos el 15% del total acumulado (empezando con los miembros de este conjunto con la cifra de ventas más baja).  
  
```  
SELECT  
[Product].[Product Categories].Bikes ON 0,  
BottomPercent  
   ({[Geography].[Geography].[City].Members}  
   , 15  
   , ([Measures].[Reseller Sales Amount],[Product].[Product Categories].Bikes)  
   ) ON 1  
FROM [Adventure Works]  
WHERE ([Measures].[Reseller Sales Amount],[Date].[Fiscal].[Fiscal Year].[FY 2003])  
```  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de funciones MDX &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
