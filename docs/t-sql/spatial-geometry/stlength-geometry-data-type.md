---
description: STLength (tipo de datos geometry)
title: STLength (tipo de datos geometry) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STLength_TSQL
- STLength (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STLength (geometry Data Type)
ms.assetid: e34dc620-2a65-4248-b099-fff91830ab98
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 713e423d0ef388ea63089bc2b4a203b8affa0fad
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88488131"
---
# <a name="stlength-geometry-data-type"></a>STLength (tipo de datos geometry)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Devuelve la longitud total de los elementos de una instancia de **geometry**.
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
.STLength ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 Tipo de valor devuelto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: **float**  
  
 Tipo de valor devuelto de CLR: **SqlDouble**  
  
## <a name="remarks"></a>Comentarios  
 Si se trata de una instancia de **geometry** cerrada, su longitud se calcula como la longitud total alrededor de la instancia; la longitud de cualquier polígono es su perímetro y la longitud de un punto es 0. La longitud de cualquier tipo **geometrycollection** es la suma de las longitudes de las instancias de **geometry** que contiene.  
  
 STLength() funciona con LineString válidos y no válidos. Si un LineString no es válido normalmente se debe a la superposición de segmentos, lo cual se puede deber a anomalías como seguimientos de GPS inexactos. STLength() no quita segmentos superpuestos o no válidos. Incluye los segmentos superpuestos y no válidos en el valor de longitud que devuelve. El método MakeValid() puede quitar segmentos superpuestos de un LineString.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea una instancia de `LineString` y se usa `STLength()` para averiguar la longitud de dicha instancia.  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0)', 0);  
SELECT @g.STLength();  
```  
  
## <a name="see-also"></a>Vea también  
 [Métodos de OGC en instancias de geometry](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

