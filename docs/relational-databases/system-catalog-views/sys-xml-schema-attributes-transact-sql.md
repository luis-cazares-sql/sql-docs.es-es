---
description: sys.xml_schema_attributes (Transact-SQL)
title: sys.xml_schema_attributes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- xml_schema_attributes_TSQL
- xml_schema_attributes
- sys.xml_schema_attributes_TSQL
- sys.xml_schema_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_attributes catalog view
ms.assetid: dd0c98aa-5e72-4df6-96d9-482786c8dbb1
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cedfdf992f1dbec8eeff1ff7fa74a489b179d943
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095421"
---
# <a name="sysxml_schema_attributes-transact-sql"></a>sys.xml_schema_attributes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por componente de esquema XML que es un atributo  , symbol_space **de.**  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**|--|Hereda de [sys.xml_schema_components](../../relational-databases/system-catalog-views/sys-xml-schema-components-transact-sql.md).|  
|**is_default_fixed**|**bit**|1 = el valor predeterminado es un valor fijo. Este valor no se puede anular en una instancia de XML.<br /><br /> 0 = El valor predeterminado no es un valor fijo para el atributo (predeterminado).|  
|**must_be_qualified**|**bit**|1 = El atributo debe estar cualificado explícitamente por el espacio de nombres.<br /><br /> 0 = El atributo debe estar cualificado implícitamente por el espacio de nombres (predeterminado).|  
|**default_value**|**nvarchar**<br /><br /> **(4000)**|Valor predeterminado del atributo. Si no se ofrece otro valor predeterminado, es NULL.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Esquemas XML &#40;las vistas de catálogo del sistema de tipos XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
