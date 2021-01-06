---
title: Objetos DataAdapter y DataReader
description: Obtenga información sobre el proveedor de datos SqlClient de Microsoft para DataReader de SQL Server, que recupera datos de una base de datos, y DataAdapter, que recupera datos de un origen de datos y rellena un objeto DataSet.
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 3a252ca44683c062e531081f0b625a5e2359d236
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771397"
---
# <a name="dataadapters-and-datareaders"></a>Objetos DataAdapter y DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Puede usar el proveedor de datos SqlClient de Microsoft para el objeto **DataReader** de SQL Server a fin de recuperar un flujo de datos de solo lectura y solo avance de una base de datos. Los resultados se devuelven a medida que se ejecuta la consulta y se almacenan en el búfer de red del cliente hasta que se solicitan con el método **Read** del objeto **DataReader**. Mediante el objeto **DataReader** puede aumentar el rendimiento de la aplicación si recupera los datos en cuanto están disponibles y, de forma predeterminada, almacena una sola fila cada vez en memoria, lo que reduce la sobrecarga del sistema.

Un <xref:System.Data.Common.DataAdapter> se utiliza para recuperar datos de un origen de datos y llenar tablas con un <xref:System.Data.DataSet>. `DataAdapter` también resuelve los cambios realizados en `DataSet` de vuelta al origen de datos. En `DataAdapter` se usa el objeto `Connection` del proveedor de datos SqlClient de Microsoft para SQL Server a fin de conectarse a un origen de datos, y se usan objetos `Command` para recuperar datos del origen de datos y resolver los cambios en él.

.NET tiene una instancia de <xref:System.Data.Common.DbDataReader> y un objeto <xref:System.Data.Common.DbDataAdapter>: el proveedor de datos SqlClient de Microsoft para SQL Server incluye una instancia de <xref:Microsoft.Data.SqlClient.SqlDataReader> y un objeto <xref:Microsoft.Data.SqlClient.SqlDataAdapter>.

## <a name="in-this-section"></a>En esta sección

[Recuperación de datos mediante un objeto DataReader](retrieve-data-by-datareader.md)  
Se describe el objeto **DataReader** de ADO.NET, así como la forma de usarlo para devolver una secuencia de resultados desde un origen de datos.

[Rellenar un conjunto de datos a partir de un objeto DataAdapter](populate-dataset-from-dataadapter.md)  
Describe cómo llenar un `DataSet`  de tablas, columnas y filas mediante un `DataAdapter`.

[Parámetros de objetos DataAdapter](dataadapter-parameters.md)  
Describe cómo utilizar parámetros con las propiedades de comando de `DataAdapter`, lo que incluye cómo asignar el contenido de una columna de `DataSet` a un parámetro de comando.

[Adición de las restricciones existentes a un conjunto de datos](add-existing-constraints-to-dataset.md)  
Describe cómo agregar restricciones existentes a un `DataSet`.

[Asignaciones de objetos DataAdapter, DataTable y DataColumn](dataadapter-datatable-datacolumn-mappings.md)  
Describe cómo configurar `DataTableMappings` y `ColumnMappings` para `DataAdapter`.

[Paginación de resultados de consulta](paging-through-query-result.md)  
Proporciona un ejemplo de cómo ver los resultados de una consulta como páginas de datos.

[Actualización de orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md)  
Describe cómo se utiliza `DataAdapter` para resolver modificaciones en `DataSet` en la base de datos.

[Control de eventos de objetos DataAdapter](handle-dataadapter-events.md)  
Describe los eventos de `DataAdapter` y cómo utilizarlos.

[Operaciones por lotes con objetos DataAdapter](batch-operations-using-dataadapters.md)  
Describe cómo mejorar el rendimiento de la aplicación mediante la reducción del número de viajes de ida y vuelta (round trip) al servidor SQL Server al aplicar las actualizaciones desde el `DataSet`.

## <a name="see-also"></a>Vea también

- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Comandos y parámetros](commands-parameters.md)
- [Transacciones y simultaneidad](transactions-and-concurrency.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
