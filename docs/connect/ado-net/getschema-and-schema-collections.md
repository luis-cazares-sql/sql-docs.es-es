---
title: Colecciones GetSchema y Schema
description: Se describe el método GetSchema para recuperar y restringir la información del esquema de una base de datos.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051400"
---
# <a name="get-schema-and-schema-collections"></a>Colecciones GetSchema y Schema

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Las clases **SqlConnection** del proveedor de datos SqlClient de Microsoft para SQL Server implementan un método **GetSchema** que se usa para recuperar la información del esquema sobre la base de datos que está actualmente conectada, y la información del esquema que devuelve el método **GetSchema** tiene el formato de un objeto <xref:System.Data.DataTable>. El método **GetSchema** es un método sobrecargado que proporciona parámetros opcionales para especificar la colección de esquemas que se devolverá y restringe la cantidad de información devuelta.

## <a name="specifying-the-schema-collections"></a>Especificación de las colecciones de esquemas

El primer parámetro opcional del método **GetSchema** es el nombre de la colección que se especifica como una cadena. Existen dos tipos de colecciones de esquemas: comunes, que son comunes a todos los proveedores, y específicas, que son específicas de cada proveedor.  

Puede consultar el proveedor de datos SqlClient de Microsoft para SQL Server para determinar la lista de colecciones de esquemas admitidas mediante la llamada al método **GetSchema** sin argumentos, o bien con el nombre de colección de esquemas "MetaDataCollections". Esto devolverá una <xref:System.Data.DataTable> con una lista de colecciones de esquemas admitidas, el número de restricciones que admite cada una y el número de partes de identificador que emplean.  

### <a name="retrieving-schema-collections-example"></a>Ejemplo de recuperación de colecciones de esquemas

En los ejemplos siguientes se muestra cómo usar el método <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> de la clase <xref:Microsoft.Data.SqlClient.SqlConnection> del proveedor de datos SqlClient de Microsoft para SQL Server para recuperar información del esquema de todas las tablas contenidas en la base de datos de ejemplo **AdventureWorks**:  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>Consulte también

- [Recuperación de la información del esquema de la base de datos](retrieving-database-schema-information.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
