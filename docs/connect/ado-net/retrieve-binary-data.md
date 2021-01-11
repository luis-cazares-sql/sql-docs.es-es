---
title: Recuperación de datos binarios
description: Describe cómo recuperar datos binarios o grandes estructuras de datos mediante `CommandBehavior`.`SequentialAccess` para modificar el comportamiento predeterminado de un `DataReader`.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1033a6b5394d92a45cd19d70f4dd6c250ec37b62
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744464"
---
# <a name="retrieve-binary-data"></a>Recuperación de datos binarios

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El objeto **DataReader** carga de forma predeterminada los datos que recibe en una fila, siempre que hay disponibles suficientes datos para llenarla. Sin embargo, los objetos binarios grandes (BLOB) se deben tratar de otra forma, ya que pueden llegar a contener grandes cantidades de datos (del orden de gigabytes) que no pueden almacenarse en una sola fila. El método **Command.ExecuteReader** se puede sobrecargar para aceptar un argumento <xref:System.Data.CommandBehavior> y modificar el comportamiento predeterminado de **DataReader**. Puede pasar <xref:System.Data.CommandBehavior.SequentialAccess> al método **ExecuteReader** para modificar el comportamiento predeterminado de **DataReader** de forma que en lugar de cargar los datos por filas, los vaya cargando secuencialmente a medida que los vaya recibiendo. Este sistema es idóneo para cargar BLOB y otras estructuras de datos grandes.

> [!NOTE]
> Al configurar **DataReader** para que utilice **SequentialAccess** debe tener en cuenta la secuencia en que va a tener acceso a los campos devueltos. El comportamiento predeterminado de **DataReader**, consistente en cargar una fila completa de datos en cuanto hay datos suficientes, permite tener acceso a los campos devueltos en cualquier orden hasta que se lee la fila siguiente. Sin embargo, al utilizar **SequentialAccess** es necesario tener acceso a los campos que devuelve **DataReader** en el orden adecuado. Por ejemplo, si la consulta devuelve tres columnas y la tercera es un BLOB, debe devolver los valores de los campos primero y segundo antes de tener acceso a los datos BLOB del tercer campo. Si trata de tener acceso al tercer campo antes que al primero o el segundo, puede que éstos dejen de estar disponibles. Esto se debe a que **SequentialAccess** cambia la forma en que el **DataReader** devuelve los datos, haciendo que lo haga de forma secuencial con lo que los datos dejan de estar disponibles en el momento en que el **DataReader** lee datos posteriores.

Cuando intente obtener acceso a los datos del campo BLOB, utilice los descriptores de acceso con información de tipos **GetBytes** o **GetChars** del **DataReader**, que llenan una matriz con los datos. En el caso de los datos de caracteres, puede utilizar también **GetString**; no obstante, si desea conservar los recursos del sistema, es mejor que no cargue un valor BLOB completo en una sola variable de cadena. En lugar de ello, puede especificar un tamaño determinado de búfer para los datos que se van a devolver, así como la ubicación de comienzo para leer el primer byte o carácter de los datos devueltos. **GetBytes** y **GetChars** devuelven un valor de tipo `long` que representa el número de bytes o caracteres devueltos. Si pasa una matriz con valores null a **GetBytes** o **GetChars**, el valor de tipo long devuelto contiene el número total de bytes o caracteres del BLOB. También puede especificar un índice de la matriz como posición de comienzo para la lectura de datos.

## <a name="example"></a>Ejemplo

En el siguiente ejemplo se devuelve el identificador y el logotipo del publicador desde la base de datos de ejemplo [**pubs**](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs). El identificador del publicador (`pub_id`) es un campo de caracteres, mientras que el logotipo es una imagen de BLOB. Como el campo **logo** es un mapa de bits, el ejemplo devuelve datos binarios mediante **GetBytes**. Tenga en cuenta que la necesidad de tener acceso a los datos de forma secuencial hace que en la fila actual de datos se tenga acceso al identificador de publicador antes que al logotipo.

[!code-csharp[SqlCommand_ExecuteReader_SequentialAccess#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteReader_SequentialAccess.cs#1)]

## <a name="see-also"></a>Consulte también

- [Datos binarios y datos de valores grandes de SQL Server](./sql/sql-server-binary-large-value-data.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
