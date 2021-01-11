---
title: Recuperación y modificación de datos
description: En .NET, el proveedor de datos SqlClient de Microsoft para SQL Server sirve como puente entre una aplicación y un origen de datos para leer y actualizar datos.
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771561"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>Recuperación y modificación de datos en ADO.NET

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La principal función de cualquier aplicación de base de datos es conectarse a un origen de datos y recuperar los datos que contiene. Los proveedores de datos SqlClient sirven como puente entre una aplicación y un origen de datos, permitiéndole ejecutar comandos y recuperar datos mediante un elemento **DataReader** o **DataAdapter**. Una función clave de cualquier aplicación de base de datos es la capacidad de actualizar los datos almacenados en la misma. En el proveedor de datos SqlClient de Microsoft para SQL Server, la actualización de los datos implica el uso de los objetos **DataAdapter**, <xref:System.Data.DataSet> y **Command**; y también puede implicar el uso de transacciones.

## <a name="in-this-section"></a>En esta sección

[Conexión a un origen de datos](connecting-to-data-source.md)  
Describe cómo se establece una conexión con un origen de datos y cómo trabajar con eventos de conexión.

[Cadenas de conexión](connection-strings.md)  
Contiene temas en los que se describen varios aspectos del uso de cadenas de conexión, entre los que se incluyen las palabras clave de cadena de conexión, información sobre seguridad y el almacenamiento y recuperación de cadenas de conexión.

[Agrupación de conexiones](connection-pooling.md)  
Se describe la agrupación de conexiones para el proveedor de datos SqlClient de Microsoft para SQL Server.

[Comandos y parámetros](commands-parameters.md)  
Contiene temas en los que se describe cómo crear comandos y generadores de comandos, configurar parámetros y cómo ejecutar comandos para recuperar y modificar datos.

[Objetos DataAdapter y DataReader](dataadapters-datareaders.md)  
Contiene temas en los que se describen DataReaders, DataAdapters, los parámetros, el control de eventos DataAdapter y la ejecución de operaciones por lotes.

[Transacciones y simultaneidad](transactions-and-concurrency.md)  
Contiene temas en los que se describe cómo realizar transacciones locales y transacciones distribuidas, y cómo trabajar con la simultaneidad optimista.

[Recuperación de la información del esquema de la base de datos](retrieving-database-schema-information.md)  
Describe cómo obtener de un origen de dstos las bases de datos o catálogos disponibles, las tablas y vistas de una base de datos, las restricciones que existen para las tablas y otra información de esquema.

[Objetos DbProviderFactory](dbproviderfactories.md)  
Describe el modelo de generador de proveedor y demuestra cómo usar las clases base del espacio de nombres `System.Data.Common`.  

[Recuperación de valores de identidad o autonuméricos](retrieve-identity-or-autonumber-values.md)  
Proporciona un ejemplo de asignación de los valores generados para una columna **identity** de una tabla de SQL Server a una columna de una fila insertada en una tabla. Describe la combinación de valores de identidad en una `DataTable`.  
  
[Recuperación de datos binarios](retrieve-binary-data.md)  
Describe cómo recuperar datos binarios o grandes estructuras de datos mediante `CommandBehavior`.`SequentialAccess` para modificar el comportamiento predeterminado de un `DataReader`.  
  
[Modificación de datos con procedimientos almacenados](modify-data-with-stored-procedures.md)  
Describe cómo utilizar parámetros de entrada y parámetros de salida de procedimientos almacenados para insertar una fila en una base de datos y devolver un nuevo valor de identidad.  

[Seguimiento de datos en SqlClient](data-tracing.md)  
Describe cómo el proveedor de datos SqlClient de Microsoft para SQL Server proporciona la funcionalidad de seguimiento de datos integrada.  
  
[Contadores de rendimiento en SqlClient](performance-counters.md)  
Describe los contadores de rendimiento disponibles para el proveedor de datos SqlClient de Microsoft para SQL Server.  
  
[Programación asincrónica](asynchronous-programming.md)  
Describe el proveedor de datos SqlClient de Microsoft para la compatibilidad de SQL Server con la programación asincrónica.  
  
[Compatibilidad con streaming de SqlClient](sqlclient-streaming-support.md)  
Explica cómo escribir aplicaciones que transmiten por secuencias datos de SQL Server sin cargarlos totalmente en memoria.  

## <a name="see-also"></a>Vea también

- [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md)
- [SQL Server y ADO.NET](./sql/index.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
