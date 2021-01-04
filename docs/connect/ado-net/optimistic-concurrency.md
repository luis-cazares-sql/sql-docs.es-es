---
title: Simultaneidad optimista
description: Describe la simultaneidad optimista y pesimista, y cómo puede probar las infracciones de simultaneidad.
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12894591f4ee7b1db5e514b7218337d92382842b
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051372"
---
# <a name="optimistic-concurrency"></a>Simultaneidad optimista

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En un entorno multiusuario existen dos modelos para actualizar datos en una base de datos: simultaneidad optimista y simultaneidad pesimista. El objeto <xref:System.Data.DataSet> está diseñado para fomentar el uso de la simultaneidad optimista en actividades cuya ejecución tiene una larga duración, como cuando se trabaja con interacción remota y cuando los usuarios interactúan con datos.  
  
La simultaneidad pesimista implica bloquear filas en el origen de datos para impedir que otros usuarios modifiquen los datos de tal forma que el usuario actual resulte afectado. En un modelo pesimista, cuando un usuario realiza una acción que hace que se aplique un bloqueo, otros usuarios no pueden realizar acciones que entrarían en conflicto con el bloqueo hasta que el propietario del bloqueo lo libere. Este modelo se utiliza principalmente en aquellos entornos en los que hay mucha contención de datos, de manera que el costo de proteger los datos con bloqueos es menor que el costo de revertir transacciones si se producen conflictos de simultaneidad.  
  
Por tanto, en un modelo de simultaneidad pesimista, un usuario que actualiza una fila establece un bloqueo. Hasta que el usuario no haya terminado la actualización y liberado el bloqueo, nadie más podrá modificar dicha fila. Por este motivo, la simultaneidad pesimista resulta más adecuada cuando los tiempos de bloqueo son cortos, como ocurre en el procesamiento de registros mediante programación. La simultaneidad pesimista no es una opción escalable cuando los usuarios interactúan con los datos y hacen que los registros queden bloqueados durante períodos de tiempo relativamente largos.

> [!NOTE]
> Si necesita actualizar varias filas en una misma operación, entonces crear una transacción es una opción más escalable que utilizar el bloqueo pesimista.

Por el contrario, los usuarios que utilizan la simultaneidad optimista no bloquean una fila cuando la leen. Cuando un usuario desea actualizar una fila, la aplicación debe determinar si otro usuario la ha modificado o no desde que se leyó. La simultaneidad optimista suele utilizarse en entornos con poca contención de datos. Esto mejora el rendimiento porque no es necesario bloquear registros, a la vez que el bloqueo de registros requiere recursos adicionales del servidor. Además, para mantener bloqueos de registros es necesaria una conexión persistente con el servidor de bases de datos. Como éste no es el caso en un modelo de simultaneidad optimista, las conexiones con el servidor pueden atender a un mayor número de clientes en menos tiempo.

En un modelo de simultaneidad optimista, se considera que ha habido una infracción si, después de que un usuario recibe un valor de la base de datos, otro usuario modifica el valor antes de que el primer usuario haya intentado modificarlo. En el ejemplo siguiente se describe cómo el servidor resuelve una infracción de simultaneidad.

Las siguientes tablas muestran un ejemplo de simultaneidad optimista.  
  
A la 1:00 p.m., el Usuario1 lee una fila de la base de datos con los valores siguientes:  
  
**IdCliente     Apellido     Nombre**  
  
101          Martínez             Cris  
  
|Nombre de la columna|Valor original|Valor actual|Valor en la base de datos|  
|-----------------|--------------------|-------------------|-----------------------|  
|IdCliente|101|101|101|  
|Apellidos|Smith|Smith|Smith|  
|Nombre|Bob|Bob|Bob|  
  
A la 1:01 p.m., el Usuario2 lee la misma fila.  
  
A las 13:03, el Usuario2 cambia **FirstName** de "Cris" a "Cristina" y actualiza la base de datos.  
  
|Nombre de la columna|Valor original|Valor actual|Valor en la base de datos|  
|-----------------|--------------------|-------------------|-----------------------|  
|IdCliente|101|101|101|  
|Apellidos|Smith|Smith|Smith|  
|Nombre|Bob|Cristina|Bob|  
  
La actualización se realiza correctamente porque los valores contenidos en la base de datos en el momento de la actualización coinciden con los valores originales que tiene el Usuario2.  
  
A la 1:05 p.m., el Usuario1 cambia el nombre de "Cris" a "Jaime" e intenta actualizar la fila.  
  
|Nombre de la columna|Valor original|Valor actual|Valor en la base de datos|  
|-----------------|--------------------|-------------------|-----------------------|  
|IdCliente|101|101|101|  
|Apellidos|Smith|Smith|Smith|  
|Nombre|Bob|Jaime|Cristina|  
  
En este momento, el Usuario1 encuentra una infracción de la simultaneidad optimista porque los valores de la base de datos ("Jaime") ya no coinciden con los valores originales que esperaba el Usuario1 ("Cris"). La infracción de simultaneidad simplemente permite saber que se ha producido un error de actualización. Ahora hay que tomar la decisión de sobrescribir los cambios realizados por el Usuario2 con los efectuados por el Usuario1 o cancelar los cambios del Usuario1.

## <a name="testing-for-optimistic-concurrency-violations"></a>Prueba de infracciones de simultaneidad optimista

Existen varias técnicas para probar si se ha producido una infracción de la simultaneidad optimista. Una de ellas consiste en incluir una columna de marca de tiempo en la tabla.

Las bases de datos suelen ofrecer funcionalidad de marca de tiempo que puede utilizarse para identificar la fecha y la hora en que se actualizó el registro por última vez. Mediante esta técnica se incluye una columna de marca de tiempo en la definición de la tabla. Siempre que se actualiza el registro, se actualiza la marca de tiempo de manera que queden reflejadas la fecha y la hora actuales.

Al hacer una prueba para ver si hay infracciones de la simultaneidad optimista, la columna de marca de tiempo se devuelve con cualquier consulta del contenido de la tabla. Cuando se intenta realizar una actualización, se compara el valor de marca de tiempo de la base de datos con el valor de marca de tiempo original contenido en la fila modificada. Si coinciden, se realiza la actualización y se actualiza la columna de marca de tiempo con la hora actual con el fin de reflejar la actualización. Si no coinciden, se ha producido una infracción de la simultaneidad optimista.

Otra técnica para probar si hay alguna infracción relacionada con la simultaneidad optimista consiste en comprobar que todos los valores de columna originales de una fila siguen coincidiendo con los existentes en la base de datos. Por ejemplo, considere la siguiente consulta:

```sql
SELECT Col1, Col2, Col3 FROM Table1  
```  
  
Para probar si hay alguna infracción de simultaneidad optimista al actualizar una fila de **Table1**, tendría que emitir la siguiente instrucción UPDATE:  
  
```sql
UPDATE Table1 Set Col1 = @NewCol1Value,  
              Set Col2 = @NewCol2Value,  
              Set Col3 = @NewCol3Value  
WHERE Col1 = @OldCol1Value AND  
      Col2 = @OldCol2Value AND  
      Col3 = @OldCol3Value  
```
La actualización se realizará siempre y cuando los valores originales coincidan con los valores de la base de datos. Si se ha modificado algún valor, la actualización no modificará la fila porque la cláusula WHERE no encontrará ninguna coincidencia.  
  
Se recomienda devolver siempre un valor de clave principal único en la consulta. De lo contrario, la instrucción UPDATE anterior puede actualizar más de una fila, lo que quizás no sea su intención.  
  
Si una columna del origen de datos admite valores nulos, quizás sea necesario extender la cláusula WHERE para comprobar si hay alguna referencia nula coincidente en la tabla local y en el origen de datos. Por ejemplo, la siguiente instrucción UPDATE comprueba que una referencia nula de la fila local sigue coincidiendo con una referencia nula del origen de datos o que el valor de la fila local sigue coincidiendo con el valor del origen de datos.  
  
```sql
UPDATE Table1 Set Col1 = @NewVal1  
  WHERE (@OldVal1 IS NULL AND Col1 IS NULL) OR Col1 = @OldVal1  
```  
  
También se puede decidir la aplicación de criterios menos restrictivos al utilizar un modelo de simultaneidad optimista. Por ejemplo, si solo se utilizan las columnas de clave principal en la cláusula WHERE se sobrescribirán los datos, independientemente de que las otras columnas se hayan actualizado o no desde la última consulta. También se puede aplicar una cláusula WHERE solo a determinadas columnas, lo que hará que se sobrescriban los datos a menos que se hayan actualizado ciertos campos desde que se consultaron por última vez.

### <a name="the-dataadapterrowupdated-event"></a>Evento DataAdapter.RowUpdated

El evento **RowUpdated** del objeto <xref:System.Data.Common.DataAdapter> se puede usar junto con las técnicas descritas anteriormente para notificar a la aplicación las infracciones de simultaneidad optimista. **RowUpdated** se produce después de cada intento de actualizar una fila **Modified** de un objeto **DataSet**. Esto permite agregar código especial de control, incluyendo el procesamiento cuando se produce una excepción, agregar información de error personalizada, agregar lógica de reintento, etc.

El objeto <xref:System.Data.Common.RowUpdatedEventArgs> devuelve una propiedad **RecordsAffected** con el número de filas afectadas por un determinado comando de actualización para una fila modificada de una tabla. Al establecer el comando de actualización para que compruebe la simultaneidad optimista, la propiedad **RecordsAffected** devolverá un valor 0 cuando se haya producido una infracción en la simultaneidad optimista, ya que no se ha actualizado ningún registro. En tal caso se inicia una excepción.

El evento **RowUpdated** le permite controlar este caso y evitar la excepción al establecer un valor **RowUpdatedEventArgs.Status** apropiado, como **UpdateStatus.SkipCurrentRow**. Para obtener más información sobre el evento **RowUpdated**, vea [Control de eventos de objetos DataAdapter](handle-dataadapter-events.md).

De forma opcional, puede establecer **DataAdapter.ContinueUpdateOnError** en **true** antes de llamar a **Update** y responder a la información de error almacenada en la propiedad **RowError** de una fila determinada cuando **Update** se complete. Para obtener más información, vea [Información de error de fila](/dotnet/framework/data/adonet/dataset-datatable-dataview/row-error-information).

## <a name="optimistic-concurrency-example"></a>Ejemplo de simultaneidad optimista

A continuación se muestra un ejemplo sencillo que establece **UpdateCommand** de **DataAdapter** para probar la simultaneidad optimista y, después, usa el evento **RowUpdated** para probar si hay infracciones de la simultaneidad optimista. Cuando se encuentra una infracción de simultaneidad optimista, la aplicación establece el valor **RowError** de la fila para la que se ha emitido la actualización con el fin de reflejar la existencia de una infracción de la simultaneidad optimista.

Tenga en cuenta que los valores de los parámetros pasados a la cláusula WHERE del comando UPDATE se asignan a los valores **Original** de sus respectivas columnas.

[!code-csharp[SqlDataAdapter_Concurrency#1](~/../sqlclient/doc/samples/SqlDataAdapter_Concurrency.cs#1)]

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Actualizar orígenes de datos con objetos DataAdapter](update-data-sources-with-dataadapters.md)
- [Transacciones y simultaneidad](transactions-and-concurrency.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
