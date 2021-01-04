---
title: Recuperación de la información del esquema de la base de datos
description: Obtenga información sobre el uso del proveedor de datos SqlClient de Microsoft para SQL Server a fin de recuperar información del esquema de la base de datos.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051392"
---
# <a name="retrieving-database-schema-information"></a>Recuperación de la información del esquema de la base de datos

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La obtención de información de esquema de una base de datos se efectúa con el proceso de detección de esquemas. La detección de esquemas permite que las aplicaciones soliciten a los proveedores administrados que busquen y devuelvan información sobre el esquema de la base de datos, también conocido como los *metadatos*, de una base de datos concreta. Los diferentes elementos del esquema de base de datos, como tablas, columnas y procedimientos almacenados, se exponen a través de colecciones de esquemas. Cada colección de esquemas contiene diversa información de esquema relativa al proveedor que se está utilizando.

El proveedor de datos SqlClient de Microsoft para SQL Server implementa el método **GetSchema** de la clase **SqlConnection**, y la información de esquema que devuelve el método **GetSchema** tiene el formato de un objeto <xref:System.Data.DataTable>. El método **GetSchema** es un método sobrecargado que proporciona parámetros opcionales para especificar la colección de esquemas que se devolverá y restringe la cantidad de información devuelta. El proveedor de datos SqlClient también proporciona un método **GetSchemaTable** que devuelve un objeto DataTable que describe los metadatos de columna del objeto **SqlDataReader**.

## <a name="in-this-section"></a>En esta sección

[GetSchema y colecciones de esquema](getschema-and-schema-collections.md)  
Se describe el método **GetSchema**, y cómo se puede usar para recuperar y restringir la información del esquema de una base de datos.

[Restricciones de esquema](schema-restrictions.md)  
Se describen las restricciones de esquema que se pueden usar con **GetSchema**. 

[Colecciones de esquemas comunes](common-schema-collections.md)  
Se describen todas las colecciones de esquemas comunes que admiten todos los proveedores administrados de .NET.  
  
[Colecciones de esquemas de SQL Server](sql-server-schema-collections.md)  
Se describen las colecciones de esquemas adicionales admitidas por el proveedor de datos SqlClient de Microsoft para SQL Server. 

## <a name="reference"></a>Referencia

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
Se describe el método **GetSchema** de la clase <xref:System.Data.Common.DbConnection>.

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
Se describe el método **GetSchema** de la clase <xref:Microsoft.Data.SqlClient.SqlConnection>.

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
Se describe el método **GetSchemaTable** de la clase <xref:System.Data.Common.DbDataReader>. 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
Se describe el método **GetSchemaTable** de la clase <xref:Microsoft.Data.SqlClient.SqlDataReader>.

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
