---
title: Tablas e índices (OLE DB Driver)
description: Obtenga información sobre las interfaces IIndexDefinition y ITableDefinition de OLE DB Driver, que permiten a los consumidores crear, modificar y quitar tablas e índices de SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB, indexes
- OLE DB, tables
- ITableDefinition interface
- tables [OLE DB]
- IIndexDefinition interface
- OLE DB Driver for SQL Server, tables
- OLE DB Driver for SQL Server, indexes
- indexes [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: af39d73d6e542702f5798e07de78fb69fcfd0fda
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861861"
---
# <a name="tables-and-indexes"></a>Tablas e índices
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  El controlador OLE DB para SQL Server expone las interfaces **IIndexDefinition** e **ITableDefinition**, que permiten a los consumidores crear, modificar y quitar tablas e índices de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Las definiciones de tabla e índice válidas dependen de la versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 La capacidad de crear o quitar tablas e índices depende de los derechos de acceso a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] del usuario de la aplicación de consumidor. La eliminación de una tabla se puede restringir en mayor medida mediante la presencia de restricciones de integridad referencia declarativas u otros factores.  
  
 La mayoría de las aplicaciones que tienen como destino [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] usan SQL-DMO en lugar de estas interfaces de OLE DB Driver for SQL Server. SQL-DMO es una colección de objetos de OLE Automation que admite todas las funciones administrativas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Las aplicaciones con destino en varios proveedores OLE DB utilizan estas interfaces OLE DB genéricas que admiten los diferentes proveedores OLE DB.  
  
 En el conjunto de propiedades específico de proveedor DBPROPSET_SQLSERVERCOLUMN, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] define la propiedad siguiente.  
  
|Id. de propiedad|Descripción|  
|-----------------|-----------------|  
|SSPROP_COL_COLLATIONNAME|Tipo: VT_BSTR<br /><br /> L/E: escritura<br /><br /> Valor predeterminado: NULL<br /><br /> Descripción: esta propiedad solo se usa en **ITableDefinition**. La cadena especificada en esta propiedad se usa al crear una instrucción [CREATE TABLE](../../../t-sql/statements/create-table-transact-sql.md).<br /><br /> .|  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Crear tablas de SQL Server](../../oledb/ole-db-tables-indexes/creating-sql-server-tables.md)  
  
-   [Agregar una columna a una tabla de SQL Server](../../oledb/ole-db-tables-indexes/adding-a-column-to-a-sql-server-table.md)  
  
-   [Quitar una columna de una tabla de SQL Server](../../oledb/ole-db-tables-indexes/removing-a-column-from-a-sql-server-table.md)  
  
-   [Quitar una tabla de SQL Server](../../oledb/ole-db-tables-indexes/dropping-a-sql-server-table.md)  
  
-   [Crear índices de SQL Server](../../oledb/ole-db-tables-indexes/creating-sql-server-indexes.md)  
  
-   [Quitar un índice de SQL Server](../../oledb/ole-db-tables-indexes/dropping-a-sql-server-index.md)  
  
## <a name="see-also"></a>Consulte también  
 [Programación del controlador OLE DB para SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)   
 [DROP TABLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-table-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../../t-sql/statements/create-index-transact-sql.md)   
 [DROP INDEX &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-index-transact-sql.md)  
  
  
