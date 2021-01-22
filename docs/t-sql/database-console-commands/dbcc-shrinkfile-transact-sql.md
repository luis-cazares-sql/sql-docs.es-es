---
description: DBCC SHRINKFILE (Transact-SQL)
title: DBCC SHRINKFILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHRINKFILE
- DBCC_SHRINKFILE_TSQL
- DBCC SHRINKFILE
- SHRINKFILE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data shrinking [SQL Server]
- TRUNCATEONLY option
- shrinking files
- NOTRUNCATE option
- shrinking databases
- decreasing database size
- file shrinking [SQL Server]
- database shrinking [SQL Server]
- logs [SQL Server], shrinking
- reducing database size
- DBCC SHRINKFILE statement
ms.assetid: e02b2318-bee9-4d84-a61f-2fddcf268c9f
author: pmasl
ms.author: umajay
ms.openlocfilehash: 478c7c784c25b1b71bcafd2279ac93bc7545c188
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151319"
---
# <a name="dbcc-shrinkfile-transact-sql"></a>DBCC SHRINKFILE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Reduce el tamaño especificado de los datos o del archivo de registro de la base de datos actual. Puede usarlo para mover datos de un archivo a otros del mismo grupo, lo que vacía el archivo y permite la eliminación de su base de datos. Puede reducir un archivo a menos de su tamaño de creación y restablecer el tamaño mínimo de archivo al nuevo valor.
  
![Icono de vínculo de artículo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DBCC SHRINKFILE   
(  
    { file_name | file_id }   
    { [ , EMPTYFILE ]   
    | [ [ , target_size ] [ , { NOTRUNCATE | TRUNCATEONLY } ] ]  
    }  
)  
[ WITH NO_INFOMSGS ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
*file_name*  
Nombre lógico del archivo que se va a reducir.
  
*file_id*  
Número de identificación (Id.) del archivo que se va a reducir. Para obtener un identificador de archivo, use la función del sistema [FILE_IDEX](../../t-sql/functions/file-idex-transact-sql.md) o consulte la vista de catálogo [sys.database_files](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md) en la base de datos actual.
  
*target_size*  
Un entero: el nuevo tamaño en megabytes del archivo. Si no se especifica o es 0, DBCC SHRINKFILE reduce hasta el tamaño de creación del archivo.
  
> [!NOTE]  
>  Puede reducir el tamaño predeterminado de un archivo vacío mediante DBCC SHRINKFILE *target_size*. Por ejemplo, si crea un archivo de 5 MB y, a continuación, reduce el archivo a 3 MB mientras el archivo todavía está vacío, el tamaño de archivo predeterminado se establece en 3 MB. Esto solo se aplica para vaciar archivos que nunca han contenido datos.  
  
Esta opción no se admite para los contenedores del grupo de archivos FILESTREAM.  
Si se especifica, DBCC SHRINKFILE intenta reducir el archivo a *target_size*. Las páginas usadas en el área del archivo que se va a liberar se mueven al espacio disponible en las áreas conservadas del archivo. Por ejemplo, con un archivo de datos de 10 MB, una operación DBCC SHRINKFILE con un valor de *target_size* de 8 mueve todas las páginas usadas en los 2 últimos MB del archivo a cualquier página sin asignar de los primeros 8 MB del archivo. DBCC SHRINKFILE no reduce un archivo más allá del tamaño necesario de los datos almacenados. Por ejemplo, si se usan 7 MB de un archivo de datos de 10 MB, una instrucción DBCC SHRINKFILE con un parámetro *target_size* de 6 reduce el archivo a 7 MB, no 6 MB.
  
EMPTYFILE  
Migra todos los datos del archivo especificado a otros archivos del **mismo grupo de archivos**. Es decir, EMPTYFILE migra los datos de un archivo especificado a otros archivos del mismo grupo de archivos. EMPTYFILE garantiza que no se agregan nuevos datos al archivo, a pesar de que este archivo no sea de solo lectura. Puede usar la instrucción [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) para quitar un archivo. Si usa la instrucción [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md) para cambiar el tamaño del archivo, la marca de solo lectura se restablece y se pueden agregar datos.

Para los contenedores del grupo de archivos FILESTREAM, no se puede usar ALTER DATABASE para quitar un archivo hasta que el recopilador de elementos no utilizados de FILESTREAM haya ejecutado y eliminado todos los archivos innecesarios del contenedor del grupo de archivos que EMPTYFILE ha copiado en otro contenedor. Para más información,vea [sp_filestream_force_garbage_collection &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/filestream-and-filetable-sp-filestream-force-garbage-collection.md).
  
> [!NOTE]  
>  Para más información sobre cómo quitar un contenedor de FILESTREAM, vea la sección correspondiente en [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).  
  
NOTRUNCATE  
Mueve las páginas asignadas desde el final de un archivo de datos hasta las páginas sin asignar de la parte delantera del archivo con o sin la especificación de *target_percent*. El espacio disponible al final del archivo no se devuelve al sistema operativo y el tamaño físico del archivo no cambia. Por tanto, si se especifica NOTRUNCATE, parecerá que el archivo no se reduce.
NOTRUNCATE solo es aplicable a archivos de datos. No afecta a los archivos de registro.   Esta opción no se admite para los contenedores del grupo de archivos FILESTREAM.
  
TRUNCATEONLY  
Libera al sistema operativo todo el espacio disponible al final del archivo, pero no realiza ningún movimiento de página dentro del archivo. El archivo de datos solo se reduce hasta el último tamaño asignado.
*target_size* se omite si se especifica con TRUNCATEONLY.  
La opción TRUNCATEONLY no mueve la información en el registro, pero quita VLF inactivos del final del archivo de registro. Esta opción no se admite para los contenedores del grupo de archivos FILESTREAM.
  
WITH NO_INFOMSGS  
Suprime todos los mensajes de información.
  
## <a name="result-sets"></a>Conjuntos de resultados  
En la siguiente tabla se describen las columnas de conjunto de resultados.
  
|Nombre de la columna|Descripción|  
|---|---|
|**DbId**|Número de identificación de la base de datos del archivo que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] intentó reducir.|  
|**FileId**|Número de identificación del archivo que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] intentó reducir.|  
|**CurrentSize**|El número de páginas de 8 KB que el archivo ocupa actualmente.|  
|**MinimumSize**|El número de páginas de 8 KB que el archivo podría ocupar, como mínimo. Este número corresponde al tamaño mínimo o tamaño de creación original de un archivo.|  
|**UsedPages**|El número de páginas de 8 KB que utiliza actualmente el archivo.|  
|**EstimatedPages**|El número de páginas de 8 KB al que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] estima que se puede reducir el archivo.|  
  
## <a name="remarks"></a>Comentarios  
DBCC SHRINKFILE se aplica a los archivos de la base de datos actual. Para más información sobre cómo cambiar la base de datos actual, vea [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md).
  
Puede detener las operaciones DBCC SHRINKFILE en cualquier momento sin que por ello se pierda el trabajo ya completado. Si usa el parámetro EMPTYFILE y cancela la operación, el archivo no se marca para evitar que se agreguen datos adicionales.
  
Cuando una operación DBCC SHRINKFILE no es correcta, se genera un error.
  
 Otros usuarios pueden trabajar en la base de datos durante la reducción de archivos: la base de datos no tiene que estar en modo de usuario único. No es necesario ejecutar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único para reducir las bases de datos del sistema.  
  
## <a name="shrinking-a-log-file"></a>Reducción de un archivo de registro  

En los archivos de registro, [!INCLUDE[ssDE](../../includes/ssde-md.md)] usa *target_size* para calcular el tamaño de destino completo del registro. Por lo tanto, *target_size* es el espacio disponible del registro después de la operación de reducción. El tamaño de destino completo del registro se convierte en el tamaño de destino de cada archivo de registro. DBCC SHRINKFILE intenta reducir cualquier archivo de registro físico a su tamaño final de forma inmediata. Sin embargo, si parte del registro lógico está en los registros virtuales más allá del tamaño final, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] libera tanto espacio como sea posible y a continuación emite un mensaje informativo. El mensaje indica las acciones que se deben llevar a cabo para mover el registro lógico de los registros virtuales al final del archivo. Una vez realizadas estas acciones, se puede usar DBCC SHRINKFILE para liberar el espacio restante.
  
Como un archivo de registro solo puede reducirse hasta el límite de un archivo de registro virtual, puede que no sea posible reducirlo a un tamaño menor que el de un archivo de registro virtual, aunque no esté siendo utilizado. [!INCLUDE[ssDE](../../includes/ssde-md.md)] elige dinámicamente el tamaño del registro de archivo virtual cuando se crean o extienden los archivos de registro.
  
## <a name="best-practices"></a>Procedimientos recomendados 
 
Tenga en cuenta la siguiente información cuando vaya a reducir un archivo:
-   Una reducción es más efectiva después de una operación con la que se crea mucho espacio no utilizado, como por ejemplo, una operación para truncar o eliminar tablas.  

-   La mayoría de las bases de datos requieren que haya espacio disponible para realizar las operaciones diarias normales. Si reduce una base de datos reiteradamente y su tamaño vuelve a aumentar, es probable que las operaciones normales requieran el espacio reducido. En estos casos, no sirve reducir la base de datos reiteradamente.  

-   La reducción no mantiene el estado de fragmentación de los índices de la base de datos y generalmente aumenta la fragmentación hasta cierto punto. Esta fragmentación es otra razón para no reducir la base de datos reiteradamente.  

-   Reduzca varios archivos en la misma base de datos de forma secuencial en lugar de simultáneamente. La contención en las tablas del sistema puede provocar bloqueo y conducir a retrasos.  
  
## <a name="troubleshooting"></a>Solución de problemas  
En esta sección se describe el modo de diagnosticar y corregir los problemas que pueden ocurrir al ejecutar el comando DBCC SHRINKFILE:
  
### <a name="the-file-doesnt-shrink"></a>El archivo no se reduce.
  
Si el tamaño del archivo no cambia después de una operación de reducción sin errores, pruebe lo siguiente para comprobar que el archivo tiene suficiente espacio libre:

- Ejecute la siguiente consulta.  
  
```sql
SELECT name ,size/128.0 - CAST(FILEPROPERTY(name, 'SpaceUsed') AS int)/128.0 AS AvailableSpaceInMB
FROM sys.database_files;
```

-   Ejecute el comando [DBCC SQLPERF](../../t-sql/database-console-commands/dbcc-sqlperf-transact-sql.md) para devolver el espacio ocupado en el registro de transacciones.  

Si no hay suficiente espacio disponible, la operación de reducción no puede reducir más el tamaño del archivo.
  
Normalmente es el archivo de registro el que parece no reducirse. Esta falta de reducción suele ser el resultado de un archivo de registro que no se ha truncado. Para truncar el registro, puede establecer el modelo de recuperación de la base de datos en SIMPLE o realizar una copia de seguridad del registro y ejecutar luego de nuevo la operación DBCC SHRINKFILE.
  
### <a name="the-shrink-operation-is-blocked"></a>La operación de reducción está bloqueada  

Una transacción que se ejecuta con un [nivel de aislamiento basado en las versiones de fila](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md) puede bloquear las operaciones de reducción. Por ejemplo, si se está ejecutando una operación de eliminación grande con un nivel de aislamiento basado en versiones de fila cuando se ejecuta una operación DBCC SHRINK DATABASE, la operación de reducción espera a que la operación de eliminación finalice antes de continuar. Cuando este bloqueo se produce, las operaciones DBCC SHRINKFILE y DBCC SHRINKDATABASE imprimen un mensaje informativo (5202 en el caso de SHRINKDATABASE y 5203 para SHRINKFILE) en el registro de errores de SQL Server. Este mensaje se registra cada cinco minutos durante la primera hora y luego cada hora. Por ejemplo, si el registro de errores contiene el siguiente mensaje de error, se producirá el siguiente error:
  
```
DBCC SHRINKFILE for file ID 1 is waiting for the snapshot   
transaction with timestamp 15 and other snapshot transactions linked to   
timestamp 15 or with timestamps older than 109 to finish.  
```  
  
Este mensaje significa que las transacciones de instantáneas con marcas de tiempo anteriores a 109 (la última transacción que realizó la operación de reducción) están bloqueando la operación de reducción. También indica que las columnas **transaction_sequence_num** o **first_snapshot_sequence_num** de la vista de administración dinámica [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) contienen el valor 15. Si las columnas de vista **transaction_sequence_num** o **first_snapshot_sequence_num** contienen un número inferior al de la última transacción realizada mediante una operación de reducción (109), la operación de reducción espera a que esas transacciones finalicen.
  
Para resolver el problema, puede llevar a cabo una de las tareas siguientes:
-   Finalizar la transacción que bloquea la operación de reducción
-   Finalizar la operación de reducción Si finaliza la operación de reducción, se mantiene todo el trabajo completado.  
-   No hacer nada y permitir que la operación de reducción espere a que finalice la transacción que la está bloqueando.  
  
## <a name="permissions"></a>Permisos  
Debe pertenecer al rol fijo de servidor **sysadmin** o al rol fijo de base de datos **db_owner** .
  
## <a name="examples"></a>Ejemplos  
  
### <a name="shrinking-a-data-file-to-a-specified-target-size"></a>Reducir un archivo de datos a un tamaño final especificado  
En el siguiente ejemplo se reduce el tamaño de un archivo de datos denominado `DataFile1` de la base de datos de usuario `UserDB` a 7 MB.
  
```sql  
USE UserDB;  
GO  
DBCC SHRINKFILE (DataFile1, 7);  
GO  
```  
  
### <a name="shrinking-a-log-file-to-a-specified-target-size"></a>Reducir un archivo de registro a un tamaño final especificado  
En el ejemplo siguiente se reduce el archivo de registro de la base de datos `AdventureWorks` a 1 MB. Para permitir que el comando DBCC SHRINKFILE reduzca el archivo, primero hay que truncar el archivo estableciendo el modelo de recuperación de la base de datos en SIMPLE.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Truncate the log by changing the database recovery model to SIMPLE.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY SIMPLE;  
GO  
-- Shrink the truncated log file to 1 MB.  
DBCC SHRINKFILE (AdventureWorks2012_Log, 1);  
GO  
-- Reset the database recovery model.  
ALTER DATABASE AdventureWorks2012  
SET RECOVERY FULL;  
GO  
```  
  
### <a name="c-truncating-a-data-file"></a>C. Truncar un archivo de datos  
En el ejemplo siguiente se trunca el archivo de datos principal en la base de datos `AdventureWorks`. Se consulta la vista de catálogo `sys.database_files` para obtener el valor de `file_id` del archivo de datos.
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT file_id, name  
FROM sys.database_files;  
GO  
DBCC SHRINKFILE (1, TRUNCATEONLY);  
```  
  
### <a name="d-emptying-a-file"></a>D. Vaciar un archivo  
En el ejemplo siguiente se ilustra cómo vaciar un archivo de forma que se pueda quitar de la base de datos. Para los fines de este ejemplo, primero se crea un archivo de datos que contiene datos.
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create a data file and assume it contains data.  
ALTER DATABASE AdventureWorks2012   
ADD FILE (  
    NAME = Test1data,  
    FILENAME = 'C:\t1data.ndf',  
    SIZE = 5MB  
    );  
GO  
-- Empty the data file.  
DBCC SHRINKFILE (Test1data, EMPTYFILE);  
GO  
-- Remove the data file from the database.  
ALTER DATABASE AdventureWorks2012  
REMOVE FILE Test1data;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md)  
[FILE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/file-id-transact-sql.md)  
[sys.database_files &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-files-transact-sql.md)  
[Reducir un archivo](../../relational-databases/databases/shrink-a-file.md)
  
  
