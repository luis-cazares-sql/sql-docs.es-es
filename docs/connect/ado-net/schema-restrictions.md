---
title: Restricciones de esquema
description: Se describen las restricciones de esquema que se pueden utilizar con GetSchema al usar el proveedor de datos SqlClient de Microsoft para SQL Server.
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 8d72e2dd020f1345ceb0b68249cb3d3acd1024dc
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051387"
---
# <a name="schema-restrictions"></a>Restricciones de esquema

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El segundo parámetro opcional del método **GetSchema** son las restricciones que se usan para limitar la cantidad de información de esquema devuelta. Este parámetro se pasa al método **GetSchema** como una matriz de cadenas. La posición en la matriz determina los valores que puede pasar, y es equivalente al número de restricciones.  
  
Por ejemplo, en la tabla siguiente se describen las restricciones que admite la colección de esquemas "Tables" mediante el proveedor de datos SqlClient de Microsoft para SQL Server. Las restricciones adicionales para las colecciones de esquemas de SQL Server se muestran al final de este tema.  

|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|TABLE_CATALOG|1|  
|Propietario|@Owner|TABLE_SCHEMA|2|  
|Tabla|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
 
## <a name="specifying-restriction-values"></a>Especificación de valores de restricción  

Para utilizar una de las restricciones de la colección de esquemas "Tables", basta con crear una matriz de cadenas con cuatro elementos y, después, colocar un valor en el elemento que coincida con el número de restricción. Por ejemplo, para restringir las tablas devueltas por el método **GetSchema** solo a las del esquema "Sales", establezca el segundo elemento de la matriz en "Sales" antes de pasarlo al método **GetSchema**.  
  
> [!NOTE]
> - Las colecciones de restricciones para `SqlClient` tienen una columna `ParameterName` adicional. La columna de valor predeterminado de restricción sigue ahí para la compatibilidad con versiones anteriores, pero actualmente se omite. Para reducir el riesgo de un ataque de inyección de SQL al especificar valores de restricción, es necesario utilizar consultas parametrizadas en lugar de sustitución de cadenas.  
> - El número de elementos de la matriz debe ser menor o igual que el número de restricciones admitidas en la colección de esquemas especificada o se iniciará una <xref:System.ArgumentException>. Puede haber un número de restricciones inferior al máximo. Se supone que las restricciones que faltan serán nulas (sin restricciones).  
  
Puede consultar el proveedor de datos SqlClient de Microsoft para SQL Server a fin de determinar la lista de restricciones admitidas mediante la llamada al método **GetSchema** con el nombre de la colección de esquemas con restricciones, que es "Restrictions". Esto devolverá una <xref:System.Data.DataTable> con una lista de los nombres de colecciones, los nombres de restricciones, los valores predeterminados de restricción y los números de restricciones.  
  
### <a name="example"></a>Ejemplo  

En los ejemplos siguientes se muestra cómo usar el método <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> de la clase <xref:Microsoft.Data.SqlClient.SqlConnection> del proveedor de datos SqlClient de Microsoft para SQL Server a fin de recuperar información de esquema de todas las tablas contenidas en la base de datos de ejemplo **AdventureWorks**, y para restringir la información devuelta solo a las tablas del esquema "Sales":  

[!code-csharp[SqlClient GetSchema with restrictions#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Restriction.cs#1)]  

## <a name="sql-server-schema-restrictions"></a>Restricciones de esquema de SQL Server  

 En la tabla siguiente se muestran las restricciones de las colecciones de esquemas de SQL Server.  
  
### <a name="users"></a>Usuarios  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>Bases de datos  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Nombre|@Name|Nombre|1|  
  
### <a name="tables"></a>Tablas  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|TABLE_CATALOG|1|  
|Propietario|@Owner|TABLE_SCHEMA|2|  
|Tabla|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>Columnas  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|TABLE_CATALOG|1|  
|Propietario|@Owner|TABLE_SCHEMA|2|  
|Tabla|@Table|TABLE_NAME|3|  
|Columna|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|TABLE_CATALOG|1|  
|Propietario|@Owner|TABLE_SCHEMA|2|  
|Tabla|@Table|TABLE_NAME|3|  
|Columna|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>Vistas  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|TABLE_CATALOG|1|  
|Propietario|@Owner|TABLE_SCHEMA|2|  
|Tabla|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|VIEW_CATALOG|1|  
|Propietario|@Owner|VIEW_SCHEMA|2|  
|Tabla|@Table|VIEW_NAME|3|  
|Columna|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|SPECIFIC_CATALOG|1|  
|Propietario|@Owner|SPECIFIC_SCHEMA|2|  
|Nombre|@Name|SPECIFIC_NAME|3|  
|Parámetro|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>Procedimientos  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|SPECIFIC_CATALOG|1|  
|Propietario|@Owner|SPECIFIC_SCHEMA|2|  
|Nombre|@Name|SPECIFIC_NAME|3|  
|Tipo|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|db_name()|1|  
|Propietario|@Owner|user_name()|2|  
|Tabla|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|Columna|@Column|c.name|5|  
  
### <a name="indexes"></a>Índices  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|db_name()|1|  
|Propietario|@Owner|user_name()|2|  
|Tabla|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|Nombre de la restricción|Nombre de parámetro|Valor predeterminado de la restricción|Número de restricciones|  
|----------------------|--------------------|-------------------------|------------------------|  
|Catálogo|@Catalog|CONSTRAINT_CATALOG|1|  
|Propietario|@Owner|CONSTRAINT_SCHEMA|2|  
|Tabla|@Table|TABLE_NAME|3|  
|Nombre|@Name|CONSTRAINT_NAME|4|  
  
## <a name="see-also"></a>Consulte también

- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
