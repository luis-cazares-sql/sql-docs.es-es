---
description: Delete (método) (colecciones ADOX)
title: Delete (método) (colecciones ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Views::Delete
- Groups::Delete
- Indexes::raw_Delete
- Columns::raw_Delete
- Tables::Delete
- Keys::Delete
- Users::Delete
- Users::raw_Delete
- Keys::raw_Delete
- Procedures::raw_Delete
- Views::raw_Delete
- Indexes::Delete
- Procedures::Delete
- Groups::raw_Delete
- Tables::raw_Delete
- Columns::Delete
helpviewer_keywords:
- delete method [ADOX]
ms.assetid: e6b6e3a4-8952-4d79-81f4-51019c338374
author: rothja
ms.author: jroth
ms.openlocfilehash: e92cbe56bb7823b5c6cfd2485d887f3e3c56a8e5
ms.sourcegitcommit: 18a98ea6a30d448aa6195e10ea2413be7e837e94
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88984626"
---
# <a name="delete-method-adox-collections"></a>Delete (método) (colecciones ADOX)
Quita un objeto de una colección.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Collection.Delete Name  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Nombre*  
 **Variante** que especifica el nombre o la posición ordinal (índice) del objeto que se va a eliminar.  
  
## <a name="remarks"></a>Observaciones  
 Se producirá un error si el *nombre* no existe en la colección.  
  
 En el caso de las colecciones de [tablas](./tables-collection-adox.md) y [usuarios](./users-collection-adox.md) , se producirá un error si el proveedor no admite la eliminación de tablas o usuarios, respectivamente. En el caso de [los procedimientos](./procedures-collection-adox.md) y las colecciones de [vistas](./views-collection-adox.md) , **Delete** producirá un error si el proveedor no admite comandos persistentes.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Colección de columnas (ADOX)](./columns-collection-adox.md)  
        [Colección de grupos (ADOX)](./groups-collection-adox.md)  
        [Colección de índices (ADOX)](./indexes-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Colección de claves (ADOX)](./keys-collection-adox.md)  
        [Colección de procedimientos (ADOX)](./procedures-collection-adox.md)  
        [Colección de tablas (ADOX)](./tables-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Colección de usuarios (ADOX)](./users-collection-adox.md)  
        [Colección de vistas (ADOX)](./views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de método Delete de procedimientos (VB)](./procedures-delete-method-example-vb.md)   
 [Ejemplo de método Delete de vistas (VB)](./views-delete-method-example-vb.md)