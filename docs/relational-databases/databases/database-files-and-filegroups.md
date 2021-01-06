---
title: Archivos y grupos de archivos de base de datos | Microsoft Docs
description: Obtenga información sobre los archivos de base de datos y cómo crear grupos de archivos en SQL Server con fines administrativos y de asignación. Vea ejemplos, reglas y recomendaciones.
ms.custom: contperf-fy20q4
ms.date: 05/29/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- databases [SQL Server], files
- filegroups [SQL Server]
- transaction logs [SQL Server], about
- transaction logs [SQL Server], files
- .mdf files
- data files [SQL Server]
- default filegroups
- files [SQL Server], about files and filegroups
- secondary files [SQL Server]
- log files [SQL Server]
- .ndf files
- files [SQL Server]
- .ldf files
- database files [SQL Server]
- databases [SQL Server], filegroups
- filegroups [SQL Server], types
- primary filegroups [SQL Server]
- user-defined filegroups [SQL Server]
- filegroups [SQL Server], about filegroups
- primary files [SQL Server]
- file types [SQL Server]
ms.assetid: 9ca11918-480d-4838-9198-cec221ef6ad0
author: stevestein
ms.author: sstein
ms.openlocfilehash: f14eaec691218c4f7a9cad82dabb2f31f36b6245
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878763"
---
# <a name="database-files-and-filegroups"></a>Archivos y grupos de archivos de base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Como mínimo, todas las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen dos archivos del sistema operativo: un archivo de datos y un archivo de registro. Los archivos de datos contienen datos y otros objetos, como tablas, índices, procedimientos almacenados y vistas. Los archivos de registro contienen la información necesaria para recuperar todas las transacciones de la base de datos. Los archivos de datos se pueden agrupar en grupos de archivos para su asignación y administración.  
  
## <a name="database-files"></a>Archivos de la base de datos  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen tres tipos de archivos, tal como se muestra en la tabla siguiente.  
  
|Archivo|Descripción|  
|----------|-----------------|  
|Principal|Incluye información de inicio de la base de datos y apunta a los demás archivos de la misma. Cada base de datos tiene un archivo de datos principal. La extensión recomendada para los nombres de archivos de datos principales es .mdf.|  
|Secundario|Archivos de datos opcionales definidos por el usuario. Los datos se pueden distribuir en varios discos colocando cada archivo en una unidad de disco distinta. La extensión de nombre de archivo recomendada para los archivos de datos secundarios es .ndf.|  
|Registro de transacciones|El registro contiene la información que se utiliza para recuperar la base de datos. Cada base de datos debe tener al menos un archivo de registro. La extensión recomendada para los nombres de archivos de registro es .ldf.|  
  
 Por ejemplo, una base de datos sencilla denominada **Ventas** tiene un archivo principal que contenga todos los datos y objetos y un archivo de registro con la información del registro de transacciones. Puede crearse una base de datos más compleja, **Pedidos**, compuesta por un archivo principal y cinco archivos secundarios. Los datos y objetos de la base de datos se reparten entre los seis archivos, y cuatro archivos de registro adicionales contienen la información del registro de transacciones.  
  
 De forma predeterminada, los datos y los registros de transacciones se colocan en la misma unidad y ruta de acceso para manipular los sistemas de disco único, pero puede que esta opción no resulte óptima para los entornos de producción. Se recomienda colocar los archivos de datos y de registro en distintos discos.  

### <a name="logical-and-physical-file-names"></a>Nombres de archivo lógico y físico
Los archivos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen dos tipos de nombres de archivo:

**logical_file_name:**  logical_file_name es el nombre que se usa para hacer referencia al archivo físico en todas las instrucciones Transact-SQL. El nombre de archivo lógico tiene que cumplir las reglas de los identificadores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y tiene que ser único entre los nombres de archivos lógicos de la base de datos.

**os_file_name:** os_file_name es el nombre del archivo físico que incluye la ruta de acceso al directorio. Debe seguir las reglas para nombres de archivos del sistema operativo.

 Para obtener más información sobre `NAME` y el argumento `FILENAME`, consulte las [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md).

> [!IMPORTANT]
> Los archivos de datos y de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pueden colocar en sistemas de archivos FAT o NTFS. En sistemas Windows, se recomienda usar el sistema de archivos NTFS por las características de seguridad que ofrece. 

> [!WARNING]
> No se admiten grupos de archivos de datos de lectura o escritura ni archivos de registro en un sistema de archivos NTFS comprimido. Solo las bases de datos de solo lectura y los grupos de archivos secundarios de solo lectura se pueden colocar en un sistema de archivos NTFS comprimido.
> Para ahorrar espacio, se recomienda encarecidamente usar [compresión de datos](../../relational-databases/data-compression/data-compression.md) en lugar de compresión del sistema de archivos.

Cuando se ejecutan varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un único equipo, cada instancia recibe un directorio predeterminado diferente para albergar los archivos de las bases de datos creadas en la instancia. Para obtener más información, vea [Ubicaciones de archivos para las instancias predeterminadas y con nombre de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md).

### <a name="data-file-pages"></a>Páginas de archivo de datos
Las páginas de un archivo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están numeradas secuencialmente, comenzando por cero (0) para la primera página del archivo. Cada archivo de una base de datos tiene un número de identificador único. Para identificar de forma única una página de una base de datos, se requiere el identificador del archivo y el número de la página. El siguiente ejemplo muestra los números de página de una base de datos que tiene un archivo de datos principal de 4 MB y un archivo de datos secundario de 1 MB.

![data_file_pages](../../relational-databases/databases/media/data-file-pages.gif)

Una página de encabezado de archivo es la primera página que contiene información acerca de los atributos del archivo. Algunas de las otras páginas del comienzo del archivo también contienen información de sistema, como mapas de asignación. Una de las páginas de sistema almacenadas en el archivo de datos principal y en el archivo de registro principal es una página de arranque de la base de datos que contiene información acerca de los atributos de la base de datos.
### <a name="file-size"></a>Tamaño de archivo
Los archivos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden crecer de forma automática a partir del tamaño especificado inicialmente. Cuando se define un archivo, se puede especificar un incremento de crecimiento. Cada vez que se llena el archivo, el tamaño aumenta en la cantidad especificada. Si hay varios archivos en un grupo de archivos, no crecerán automáticamente hasta que todos los archivos estén llenos.

 Para obtener más información sobre las páginas y tipos de páginas, vea [Guía de arquitectura de páginas y extensiones](../..//relational-databases/pages-and-extents-architecture-guide.md).


Cada archivo también puede tener un tamaño máximo especificado. Si no se especifica un tamaño máximo, el archivo puede crecer hasta utilizar todo el espacio disponible en el disco. Esta característica es especialmente útil cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza como una base de datos incrustada en una aplicación para la que el usuario no dispone fácilmente de acceso a un administrador del sistema. El usuario puede dejar que los archivos crezcan automáticamente cuando sea necesario y evitar así las tareas administrativas de supervisar la cantidad de espacio disponible en la base de datos y asignar más espacio manualmente.  

Para obtener más información sobre la administración del archivo de registro de transacciones, vea [Administrar el tamaño del archivo de registro de transacciones](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations).   

## <a name="database-snapshot-files"></a>Archivos de instantáneas de bases de datos
La forma de archivo que utiliza una instantánea de base de datos para almacenar sus datos de copia por escritura depende de si la instantánea la ha creado un usuario o se utiliza internamente:

* Una instantánea de base de datos que crea un usuario almacena sus datos en uno o más archivos dispersos. La tecnología de archivos dispersos es una característica del sistema de archivos NTFS. Al principio, un archivo disperso no incluye datos de usuario y no se le asigna espacio en disco. Para obtener información general sobre el uso de los archivos dispersos en instantáneas de bases de datos y el crecimiento de estas, vea [Ver el tamaño del archivo disperso de una instantánea de base de datos (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md). 
* Las instantáneas de bases de datos las utilizan internamente algunos comandos DBCC. Entre estos comandos se incluyen: DBCC CHECKDB, DBCC CHECKTABLE, DBCC CHECKALLOC y DBCC CHECKFILEGROUP. Una instantánea de base de datos interna utiliza flujos de datos alternativos dispersos de los archivos de base de datos originales. Como los archivos dispersos, los flujos de datos alternativos son una característica del sistema de archivos NTFS. El uso de los flujos de datos alternativos dispersos permite que varias asignaciones de datos se asocien a un único archivo o carpeta sin afectar a las estadísticas de tamaño o volumen. 
  
## <a name="filegroups"></a>Grupos de archivos  
* El grupo de archivos contiene el archivo de datos principal y cualquier otro archivo secundario que no se encuentre en otro grupo de archivos. 
* Se pueden crear grupos de archivos definidos por el usuario para agrupar archivos con fines administrativos y de asignación y ubicación de datos.  
  
 Por ejemplo, `Data1.ndf`, `Data2.ndf` y `Data3.ndf` pueden crearse en tres unidades de disco, respectivamente, y asignarse al grupo de archivos `fgroup1`. Se puede crear una tabla específicamente para el grupo de archivos `fgroup1`. Las consultas de datos de la tabla se distribuirán por los tres discos, con lo que mejorará el rendimiento. Puede obtenerse la misma mejora del rendimiento con un solo archivo creado en un conjunto de bandas RAID (matriz redundante de discos independientes). No obstante, los archivos y grupos de archivos permiten agregar fácilmente nuevos archivos a discos nuevos.  
  
 Todos los archivos de datos se almacenan en los grupos de archivos que se indican en la tabla siguiente.  
  
|Grupo de archivos|Descripción|  
|---------------|-----------------|  
|Principal|Grupo de archivos que contiene el archivo principal. Todas las tablas del sistema forman parte del grupo de archivos principal.|  
|Tabla optimizada para memoria|Un grupo de archivos optimizados para memoria está basado en un grupo de archivos de FILESTREAM.|  
|Secuencia de archivos||    
|Definidas por el usuario|Cualquier grupo de archivos creado por el usuario al crear la base de datos o al modificarla.|  
  
### <a name="default-primary-filegroup"></a>Grupo de archivos predeterminado (principal)  
 Cuando se crean objetos en la base de datos sin especificar a qué grupo de archivos pertenecen, se asignan al grupo de archivos predeterminado. Siempre existe un grupo de archivos designado como predeterminado. Los archivos del grupo de archivos predeterminado deben ser lo suficientemente grandes como para dar cabida a todos los objetos nuevos no asignados a otros grupos de archivos.  
  
 El grupo de archivos PRINCIPAL es el predeterminado, a menos que se cambie mediante la instrucción ALTER DATABASE. Los objetos y las tablas del sistema no se asignan al nuevo grupo de archivos predeterminado, sino que siguen asignados al grupo de archivos PRIMARY.  
 
### <a name="memory-optimized-data-filegroup"></a>Grupo de archivos de datos optimizados para memoria

Para obtener más información sobre los grupos de archivos optimizados para memoria, vea [Grupo de archivos optimizados para memoria](../../relational-databases/in-memory-oltp/the-memory-optimized-filegroup.md).

### <a name="filestream-filegroup"></a>Grupo de archivos FILESTREAM

Para obtener más información sobre los grupos de archivos FILESTREAM, consulte [FILESTREAM](../../relational-databases/blob/filestream-sql-server.md#filestream-storage) y [Crear una base de datos habilitada para FILESTREAM](../../relational-databases/blob/create-a-filestream-enabled-database.md).

### <a name="file-and-filegroup-example"></a>Ejemplo de archivos y grupos de archivos
 En el siguiente ejemplo se crea una base de datos en una instancia de SQL Server. La base de datos tiene un archivo de datos principal, un grupo de archivos definido por el usuario y el archivo de registro. El archivo de datos principal está en el grupo de archivos principal y el grupo de archivos definido por el usuario tiene dos archivos de datos secundarios. Una instrucción ALTER DATABASE hace que el grupo de archivos definido por el usuario sea el grupo predeterminado. A continuación, se crea una tabla que especifica el grupo de archivos definido por el usuario. (En este ejemplo se usa una ruta de acceso genérica `c:\Program Files\Microsoft SQL Server\MSSQL.1` para evitar que se especifique una versión de SQL Server).

```sql
USE master;
GO
-- Create the database with the default data
-- filegroup, filestream filegroup and a log file. Specify the
-- growth increment and the max size for the
-- primary data file.
CREATE DATABASE MyDB
ON PRIMARY
  ( NAME='MyDB_Primary',
    FILENAME=
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_Prm.mdf',
    SIZE=4MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
FILEGROUP MyDB_FG1
  ( NAME = 'MyDB_FG1_Dat1',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_FG1_1.ndf',
    SIZE = 1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
  ( NAME = 'MyDB_FG1_Dat2',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB_FG1_2.ndf',
    SIZE = 1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB),
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM
  ( NAME = 'MyDB_FG_FS',
    FILENAME = 'c:\Data\filestream1')
LOG ON
  ( NAME='MyDB_log',
    FILENAME =
       'c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\data\MyDB.ldf',
    SIZE=1MB,
    MAXSIZE=10MB,
    FILEGROWTH=1MB);
GO
ALTER DATABASE MyDB 
  MODIFY FILEGROUP MyDB_FG1 DEFAULT;
GO

-- Create a table in the user-defined filegroup.
USE MyDB;
CREATE TABLE MyTable
  ( cola int PRIMARY KEY,
    colb char(8) )
ON MyDB_FG1;
GO

-- Create a table in the filestream filegroup
CREATE TABLE MyFSTable
(
    cola int PRIMARY KEY,
  colb VARBINARY(MAX) FILESTREAM NULL
)
GO
```

La siguiente ilustración resume los resultados del ejemplo anterior, excepto para datos FILESTREAM.

![filegroup_example](../../relational-databases/databases/media/filegroup-example.gif)

## <a name="file-and-filegroup-fill-strategy"></a>Estrategia para el relleno de archivos y grupos de archivos
Los grupos de archivos utilizan una estrategia de relleno proporcional entre todos los archivos de cada grupo de archivos. A medida que se escriben datos en el grupo de archivos, [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] escribe una cantidad proporcional al espacio disponible del archivo en cada archivo del grupo, en lugar de escribir todos los datos en el primer archivo hasta que esté completo. A continuación, escribe en el siguiente archivo. Por ejemplo, si el archivo f1 tiene 100 MB de espacio disponible y el archivo f2 tiene 200 MB de espacio disponible, se dará una extensión del archivo f1, dos extensiones del archivo f2, etc. De ese modo, los dos archivos se llenarán aproximadamente al mismo tiempo y se conseguirá crear una banda simple.

Por ejemplo, un grupo de archivos consta de tres archivos configurados para crecer automáticamente. Cuando se agota el espacio de todos los archivos del grupo de archivos, solo se expande el primer archivo. Cuando se llena el primer archivo y no se pueden escribir más datos en el grupo de archivos, se expande el segundo archivo. Cuando se llena el segundo archivo y no pueden escribirse más datos en el grupo de archivos, se expande el tercer archivo. Si se llena el tercer archivo y no se pueden escribir más datos en el grupo de archivos, se vuelve a expandir el primer archivo, y así sucesivamente.

## <a name="rules-for-designing-files-and-filegroups"></a>Reglas para el diseño de archivos y grupos de archivos
Las siguientes reglas afectan a los archivos y grupos de archivos:
- Un archivo o un grupo de archivos no puede ser utilizado por más de una base de datos. Por ejemplo, los archivos sales.mdf y sales.ndf, que contienen datos y objetos de la base de datos sales, no pueden ser utilizados por otra base de datos.
- Un archivo puede pertenecer únicamente a un grupo de archivos.
- Los archivos del registro de transacciones nunca pueden formar parte de un grupo de archivos.

## <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones
Recomendaciones para trabajar con archivos y grupos de archivos: 
- La mayor parte de las bases de datos funcionarán correctamente con un solo archivo de datos y un solo archivo de registro de transacciones.
- Si usa varios archivos de datos, cree un segundo grupo de archivos para el archivo adicional y conviértalo en el grupo de archivos predeterminado. De ese modo, el archivo principal solo contendrá objetos y tablas del sistema.
- Para maximizar el rendimiento, cree archivos o grupos de archivos en tantos discos disponibles como sea posible y distribuya en grupos de archivos distintos los objetos que compitan de forma intensa por el espacio.
- Utilice grupos de archivos para permitir la colocación de los objetos en determinados discos físicos.
- Disponga en grupos de archivos distintos las diferentes tablas que se utilicen en las mismas consultas de combinación. Este paso mejorará el rendimiento debido a la búsqueda de datos combinados que realizan las operaciones de E/S en paralelo en los discos.
- Distribuya en grupos de archivos distintos las tablas de acceso frecuente y los índices no clúster que pertenezcan a esas tablas. Utilizar grupos de archivos distintos mejorará el rendimiento debido a las operaciones de E/S en paralelo que se realizan si los archivos se encuentran en discos físicos distintos.
- No coloque los archivos de registro de transacción en el mismo disco físico con los demás archivos y grupos de archivos.
- Si necesita extender un volumen o una partición en la que residan los archivos de base de datos mediante herramientas como [Diskpart](/windows-server/administration/windows-commands/diskpart), debe realizar una copia de seguridad de todas las bases de datos de usuario y del sistema y detener los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] primero. Además, una vez que los volúmenes de disco se extiendan correctamente, considere la posibilidad de ejecutar el comando [`DBCC CHECKDB`](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md) para garantizar la integridad física de todas las bases de datos que residen en cada volumen.

Para obtener más recomendaciones sobre la administración del archivo de registro de transacciones, vea [Administrar el tamaño del archivo de registro de transacciones](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md#Recommendations).   

## <a name="related-content"></a>Contenido relacionado  
 [CREATE DATABASE &#40;Transact-SQL de SQL Server&#41;](../../t-sql/statements/create-database-transact-sql.md)    
 [Opciones File y Filegroup de ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md)      
 [Adjuntar y separar bases de datos &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)  
 [Guía de arquitectura y administración de registros de transacciones de SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)    
 [Guía de arquitectura de páginas y extensiones](../../relational-databases/pages-and-extents-architecture-guide.md)    
 [Administrar el tamaño del archivo de registro de transacciones](../../relational-databases/logs/manage-the-size-of-the-transaction-log-file.md)