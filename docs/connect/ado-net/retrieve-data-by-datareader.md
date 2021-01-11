---
title: Recuperación de datos mediante un objeto DataReader
description: Obtenga información sobre cómo recuperar datos mediante un objeto DataReader en ADO.NET con este código de ejemplo. DataReader proporciona un flujo de datos no almacenado en búfer.
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: dfbe41fe29a3dadc69a2a428f1bf8b606a2a7050
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771360"
---
# <a name="retrieve-data-by-a-datareader"></a>Recuperación de datos mediante un objeto DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Para recuperar datos mediante un objeto **DataReader**, cree una instancia del objeto **Command** y, después, un objeto **DataReader** mediante la llamada a **Command.ExecuteReader** para recuperar filas de un origen de datos. **DataReader** proporciona un flujo de datos no almacenado en búfer que permite a la lógica de procedimientos procesar de forma eficaz y secuencial los resultados de un origen de datos.

> [!NOTE]
> **DataReader** es la mejor opción cuando se trata de recuperar grandes cantidades de datos, ya que estos no se almacenan en la memoria caché.

En el ejemplo siguiente se muestra cómo se usa un objeto **DataReader**, donde `reader` representa una instancia válida de DataReader y `command` un objeto Command válido.  

```csharp
reader = command.ExecuteReader();  
```

Use el método **DataReader.Read** para obtener una fila de los resultados de la consulta. Para acceder a cada columna de la fila devuelta, puede pasar al objeto **DataReader** el nombre o número ordinal de la columna. Pero para obtener el mejor rendimiento, el objeto **DataReader** proporciona una serie de métodos que permiten acceder a los valores de columna en sus tipos de datos nativos (**GetDateTime**, **GetDouble**, **GetGuid**, **GetInt32**, etc.). Para obtener una lista de métodos de descriptor de acceso con tipo para objetos **DataReader** específicos del proveedores de datos, vea <xref:Microsoft.Data.SqlClient.SqlDataReader>. Al usar los métodos de descriptor de acceso con tipo cuando se conoce el tipo de datos subyacente, se reduce el número de conversiones de tipo necesarias para recuperar el valor de columna.  

En el ejemplo de código siguiente se realiza una iteración por un objeto **DataReader** y se devuelven dos columnas de cada fila.  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="close-the-datareader"></a>Cierre de DataReader  

Llame siempre al método `Close()` cuando haya terminado de usar el objeto `DataReader`.

> [!NOTE]
> Si **Command** contiene parámetros de salida o valores devueltos, no estarán disponibles hasta que se cierre el objeto **DataReader**.  

> [!IMPORTANT]
> Mientras un objeto **DataReader** está abierto, usa de forma exclusiva el objeto **Connection**. No se podrá ejecutar ningún comando para el objeto **Connection** hasta que se cierre el objeto **DataReader** original, incluida la creación de otro objeto **DataReader**.  

> [!NOTE]
> No llame a **Close** o **Dispose** para objetos **Connection** o **DataReader**, ni para ningún otro objeto administrado en el método **Finalize** de la clase. En un finalizador, libere solo los recursos no administrados que pertenezcan directamente a su clase. Si la clase no dispone de recursos no administrados, no incluya un método **Finalize** en la definición de clase. Para obtener más información, consulte [Recolección de elementos no utilizados](/dotnet/standard/garbage-collection/index).
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>Recuperación de varios conjuntos de resultados con NextResult

Si el objeto **DataReader** devuelve varios resultados, llame al método **NextResult** para realizar una iteración por los conjuntos de resultados de forma secuencial. En el siguiente ejemplo se muestra el <xref:Microsoft.Data.SqlClient.SqlDataReader> mientras procesa los resultados de las dos instrucciones SELECT mediante el método <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A>.  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>Obtención de información del esquema a partir del objeto DataReader  

Mientras un objeto **DataReader** está abierto, puede usar el método **GetSchemaTable** para recuperar información del esquema sobre el conjunto de resultados actual. **GetSchemaTable** devuelve un objeto <xref:System.Data.DataTable> rellenado con filas y columnas que contienen la información del esquema del conjunto de resultados actual. **DataTable** contiene una fila por cada una de las columnas del conjunto de resultados. Cada columna de la tabla de esquema se asocia a una propiedad de las columnas devueltas del conjunto de resultados, donde **ColumnName** es el nombre de la propiedad y el valor de la columna es el de la propiedad. En el ejemplo siguiente se escribe la información del esquema de **DataReader**.  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>Consulte también

- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Comandos y parámetros](commands-parameters.md)
- [Recuperación de la información del esquema de la base de datos](retrieving-database-schema-information.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
