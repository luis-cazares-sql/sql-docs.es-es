---
description: Objeto Key (ADOX)
title: Objeto Key (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Key
helpviewer_keywords:
- Key object [ADOX]
ms.assetid: 55f116fe-4d56-4892-bffe-0cdd6fc727c9
author: rothja
ms.author: jroth
ms.openlocfilehash: d8b2f37dd12f7243f2c5fcb8e577dc35e350c6a9
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88984056"
---
# <a name="key-object-adox"></a>Objeto Key (ADOX)
Representa un campo de clave principal, externa o única de una tabla de base de datos.  
  
## <a name="remarks"></a>Observaciones  
 En el código siguiente se crea una nueva **clave**:  
  
```  
Dim obj As New Key  
```  
  
 Con las propiedades y las colecciones de un objeto de **clave** , puede:  
  
-   Identifique la clave con la propiedad [Name](./name-property-adox.md) .  
  
-   Determine si la clave es principal, externa o única con la propiedad [Type](./type-property-key-adox.md) .  
  
-   Obtenga acceso a las columnas de base de datos de la clave con la colección de [columnas](./columns-collection-adox.md) .  
  
-   Especifique el nombre de la tabla relacionada con la propiedad [RelatedTable](./relatedtable-property-adox.md) .  
  
-   Determine la acción realizada al eliminar o actualizar una clave principal con las propiedades [DeleteRule](./deleterule-property-adox.md) y [UpdateRule](./updaterule-property-adox.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Key](./key-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Colección de claves (ADOX)](./keys-collection-adox.md)