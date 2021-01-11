---
title: Ejecutar un comando
description: Describe el objeto `Command` del proveedor de datos SqlClient de Microsoft para SQL Server y cómo utilizarlo para ejecutar consultas y comandos en un origen de datos.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 40494916-c25a-4cb8-8f7c-fcb8d378464e
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 772ecef470b69a34e4bd4b0eba2ca60255657eaf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771428"
---
# <a name="executing-a-command"></a>Ejecutar un comando

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El proveedor de datos SqlClient de Microsoft para SQL Server tiene un objeto <xref:Microsoft.Data.SqlClient.SqlCommand> que hereda de <xref:System.Data.Common.DbCommand>. Este objeto expone métodos para ejecutar comandos en función del tipo de comando y el valor devuelto deseado, como se describe en la tabla siguiente.

|Get-Help|Valor devuelto|  
|-------------|------------------|  
|`ExecuteReader`|Devuelve un objeto `DataReader`.|  
|`ExecuteScalar`|Devuelve un solo valor escalar.|  
|`ExecuteNonQuery`|Ejecuta un comando que no devuelve ninguna fila.|  
|`ExecuteXMLReader`|Devuelve un valor <xref:System.Xml.XmlReader>. Solo está disponible para un objeto `SqlCommand`.|

Cada objeto command fuertemente tipado admite también una enumeración <xref:System.Data.CommandType> que especifica cómo se interpreta una cadena de comando, tal como se describe en la tabla siguiente.

|CommandType|Descripción|
|-----------------|-----------------|  
|`Text`|Comando de SQL que define las instrucciones que se van a ejecutar en el origen de datos.|  
|`StoredProcedure`|Nombre del procedimiento almacenado. Puede usar la propiedad `Parameters` de un comando para tener acceso a los parámetros de entrada y de salida y a los valores devueltos, independientemente del método `Execute` al que se llame.|  
|`TableDirect`|Nombre de una tabla.|

> [!IMPORTANT]
> Al usar `ExecuteReader`, no es posible el acceso a los valores devueltos y los parámetros de salida hasta que se cierra `DataReader`.

## <a name="example"></a>Ejemplo

En el ejemplo de código siguiente se muestra cómo se crea un objeto <xref:Microsoft.Data.SqlClient.SqlCommand> para ejecutar un procedimiento almacenado mediante el establecimiento de sus propiedades. Para especificar el parámetro de entrada del procedimiento almacenado se usa un objeto <xref:Microsoft.Data.SqlClient.SqlParameter>. El comando se ejecuta con el método <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> y el resultado de <xref:Microsoft.Data.SqlClient.SqlDataReader> se muestra en la ventana de consola.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

### <a name="troubleshooting-commands"></a>Solución de problemas de comandos

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

El proveedor de datos SqlClient de Microsoft para SQL Server agrega **contadores de rendimiento** para que pueda detectar problemas intermitentes relacionados con errores en las ejecuciones de comandos. Para más información, consulte [Contadores de rendimiento en SqlClient](performance-counters.md).

## <a name="see-also"></a>Vea también

- [Comandos y parámetros](commands-parameters.md)
- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
