---
title: Introducción a diagramas (SQLXML)
description: Obtenga información sobre SQLXML 4,0 diagramas que puede usarse para insertar, actualizar o eliminar datos en una base de datos.
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a416950247208c682d0fdc923db71357405fd0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170627"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Introducción a los diagramas de actualización (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  Puede modificar (insertar, actualizar o eliminar) una base de datos de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] desde un documento XML existente mediante una diagrama o la función OPENXML [!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
 La función OPENXML modifica una base de datos dividiendo el documento XML existente y proporcionando un conjunto de filas que puede pasarse a una instrucción INSERT, UPDATE o DELETE. Con OPENXML, las operaciones se realizan directamente en las tablas de la base de datos. Por lo tanto, el uso de OPENXML resulta más adecuado siempre que los proveedores de conjuntos de filas, como una tabla, puedan aparecer como un origen.  
  
 Al igual que OPENXML, un diagrama de actualización permite insertar, actualizar o eliminar datos en la base de datos; sin embargo, un diagrama de actualización funciona con las vistas XML proporcionadas por el esquema XSD (o XDR) anotado; por ejemplo, las actualizaciones se aplican a la vista XML proporcionada por el esquema de asignación. El esquema de asignación, a su vez, incluye la información necesaria para asignar elementos y atributos XML a las columnas y tablas de base de datos correspondientes. El diagrama de actualización usa esta información de asignación para actualizar las columnas y tablas de base de datos.  
  
> [!NOTE]  
>  En esta documentación se asume que está familiarizado con la compatibilidad de las plantillas y el esquema de asignación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Introducción a los esquemas XSD anotados &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). En el caso de las aplicaciones heredadas que usan XDR, consulte [esquemas XDR anotados &#40;en desuso en SQLXML 4,0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Espacios de nombres necesarios en el diagrama de actualización  
 Las palabras clave de un diagrama, como **\<sync>** , **\<before>** y, **\<after>** existen en el espacio de nombres **urn: schemas-microsoft-com: XML-diagrama** . Se usa un prefijo de espacio de nombres arbitrario. En esta documentación, el prefijo **atributo updg** denota el espacio de nombres **Diagrama** .  
  
## <a name="reviewing-syntax"></a>Revisar la sintaxis  
 Un diagrama es una plantilla con **\<sync>** **\<before>** bloques, y **\<after>** que forman la sintaxis de diagrama. El código siguiente muestra esta sintaxis en su forma más simple:  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Las definiciones siguientes describen el rol de cada uno de estos bloques:  
  
 **\<before>**  
 Identifica el estado existente (que también recibe el nombre de "estado before") de la instancia de registro.  
  
 **\<after>**  
 Identifica el nuevo estado al que van a cambiarse los datos.  
  
 **\<sync>**  
 Contiene los **\<before>** **\<after>** bloques y. Un **\<sync>** bloque puede contener más de un conjunto de **\<before>** **\<after>** bloques y. Si hay más de un conjunto de **\<before>** bloques y **\<after>** , estos bloques (incluso si están vacíos) deben especificarse como pares. Además, un diagrama puede tener más de un **\<sync>** bloque. Cada **\<sync>** bloque es una unidad de transacción (lo que significa que se realiza todo el **\<sync>** bloque o no se hace nada). Si especifica varios **\<sync>** bloques en un diagrama, el error de un **\<sync>** bloque no afectará a los demás **\<sync>** bloques.  
  
 El hecho de que un diagrama elimine, inserte o actualice una instancia de registro depende del contenido de **\<before>** los **\<after>** bloques y:  
  
-   Si una instancia de registro aparece solo en el **\<before>** bloque sin ninguna instancia correspondiente en el **\<after>** bloque, el diagrama realiza una operación de eliminación.  
  
-   Si una instancia de registro aparece solo en el **\<after>** bloque sin ninguna instancia correspondiente en el **\<before>** bloque, se trata de una operación de inserción.  
  
-   Si una instancia de registro aparece en el **\<before>** bloque y tiene una instancia correspondiente en el **\<after>** bloque, se trata de una operación de actualización. En este caso, diagrama actualiza la instancia de registro a los valores que se especifican en el **\<after>** bloque.  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Especificar un esquema de asignación en el diagrama de actualización  
 En un diagrama de actualización, la abstracción XML que se proporciona con un esquema de asignación (se admiten tanto esquemas XSD como esquemas XDR) puede ser implícita o explícita (es decir, un diagrama de actualización puede funcionar con o sin un esquema de asignación especificado). Si no se especifica un esquema de asignación, el diagrama asume una asignación implícita (la asignación predeterminada), donde cada elemento del **\<before>** bloque o **\<after>** bloque se asigna a una tabla y el atributo o elemento secundario de cada elemento se asigna a una columna de la base de datos. Si especifica explícitamente un esquema de asignación, los elementos y atributos del diagrama de actualización deben coincidir con los elementos y los atributos del esquema de asignación.  
  
### <a name="implicit-default-mapping"></a>Asignación implícita (predeterminada)  
 En la mayoría de los casos, es posible que un diagrama de actualización que realiza actualizaciones simples no requiera un esquema de asignación. En este caso, el diagrama de actualización se basa en el esquema de asignación predeterminado.  
  
 En el diagrama de actualización siguiente se muestra una asignación implícita. En este ejemplo, el diagrama de actualización inserta un nuevo cliente en la tabla Sales.Customer. Dado que este diagrama usa la asignación implícita, el \<Sales.Customer> elemento se asigna a la tabla sales. Customer y los atributos CustomerID y SalesPersonID se asignan a las columnas correspondientes de la tabla sales. Customer.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>Asignación explícita  
 Si especifica un esquema de asignación (ya sea XSD o XDR), el diagrama de actualización usa dicho esquema para determinar las columnas y las tablas de base de datos que van a actualizarse.  
  
 Si el diagrama realiza una actualización compleja (por ejemplo, insertando registros en varias tablas en base a la relación de elementos primarios y secundarios que se especifica en el esquema de asignación), debe proporcionar explícitamente el esquema de asignación utilizando el atributo **de esquema de asignación** en el que se ejecuta diagrama.  
  
 Dado que un diagrama de actualización es una plantilla, la ruta de acceso especificada para el esquema de asignación en el diagrama de actualización es relativa a la ubicación del archivo de plantilla (con respecto al lugar donde se almacena el diagrama de actualización). Para obtener más información, vea [especificar un esquema de asignación anotado en un diagrama &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Asignación centrada en elementos y centrada en atributos en diagramas de actualización  
 Con la asignación predeterminada (cuando el esquema de asignación no se especifica en el diagrama de actualización), los elementos del diagrama de actualización se asignan a tablas y los elementos secundarios (en el caso de la asignación centrada en elementos) y los atributos (en el caso de asignación centrada en atributos) se asignan a columnas.  
  
### <a name="element-centric-mapping"></a>Asignación centrada en elementos  
 En un diagrama de actualización centrado en elementos, un elemento contiene elementos secundarios que denotan las propiedades del elemento. Como ejemplo, consulte el diagrama de actualización siguiente. El **\<Person.Contact>** elemento contiene los **\<FirstName>** **\<LastName>** elementos secundarios y. Estos elementos secundarios son propiedades del **\<Person.Contact>** elemento.  
  
 Dado que este diagrama no especifica un esquema de asignación, diagrama usa la asignación implícita, donde el **\<Person.Contact>** elemento se asigna a la tabla person. contact y sus elementos secundarios se asignan a las columnas FirstName y LastName.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>asignación centrada en atributos  
 En una asignación centrada en atributos, los elementos tienen atributos. El diagrama de actualización siguiente usa la asignación centrada en atributos. En este ejemplo, el **\<Person.Contact>** elemento consta de los atributos **FirstName** y **LastName** . Estos atributos son las propiedades del **\<Person.Contact>** elemento. Como en el ejemplo anterior, este diagrama no especifica ningún esquema de asignación, por lo que se basa en la asignación implícita para asignar el **\<Person.Contact>** elemento a la tabla person. contact y los atributos del elemento a las columnas respectivas de la tabla.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>Usar asignaciones centradas en elementos y centradas en atributos  
 Puede especificar una combinación de asignaciones centradas en elementos y asignaciones centradas en atributos, tal y como se muestra en el diagrama de actualización siguiente. Observe que el **\<Person.Contact>** elemento contiene un atributo y un elemento secundario. Asimismo, este diagrama de actualización se basa en la asignación implícita. Por lo tanto, el atributo **FirstName** y el **\<LastName>** elemento secundario se asignan a las columnas correspondientes de la tabla person. contact.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>Trabajar con caracteres que son válidos en SQL Server pero que no son válidos en XML  
 En [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], los nombres de tabla pueden incluir un espacio. Sin embargo, este tipo de nombre de tabla no es válido en XML.  
  
 Para codificar caracteres que son [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificadores válidos pero que no son identificadores XML válidos, use ' __xHHHH \_ \_ ' como valor de codificación, donde HHHH representa el código UCS-2 hexadecimal de cuatro dígitos para el carácter en el orden más significativo en primer lugar. Con este esquema de codificación, un carácter de espacio se reemplaza por x0020 (el código hexadecimal de cuatro dígitos para un carácter de espacio); por lo tanto, el nombre de la tabla [Order Details] en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se _x005B_Order_x0020_Details_x005D \_ en XML.  
  
 Del mismo modo, es posible que necesite especificar nombres de elementos de tres partes, como \<[database].[owner].[table]> . Dado que los caracteres de corchete ([y]) no son válidos en XML, debe especificarse como \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> , donde _x005B \_ es la codificación del corchete de apertura ([) y _x005D \_ es la codificación del corchete de cierre (]).  
  
## <a name="executing-updategrams"></a>Ejecutar diagramas de actualización  
 Dado que un diagrama de actualización es una plantilla, todos los mecanismos de procesamiento de una plantilla se aplican al diagrama de actualización. Para SQLXML 4.0, puede ejecutar un diagrama de actualización de cualquiera de las siguientes formas:  
  
-   Enviándolo en un comando ADO.  
  
-   Enviándolo como un comando OLE DB.  
  
## <a name="see-also"></a>Consulte también  
 [Consideraciones de seguridad de diagrama &#40;SQLXML 4,0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
