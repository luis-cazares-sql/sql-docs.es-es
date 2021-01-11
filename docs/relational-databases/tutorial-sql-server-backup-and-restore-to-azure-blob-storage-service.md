---
title: 'Inicio rápido: Copia de seguridad y restauración en el servicio Azure Blob Storage'
description: 'Inicio rápido: información sobre cómo escribir copias de seguridad en el servicio Azure Blob Storage y cómo restaurar desde este servicio. Cree un contenedor de blobs de Azure, escriba una copia de seguridad y, después, lleve a cabo la restauración.'
ms.custom: seo-dt-2019
ms.date: 12/21/2020
ms.prod: sql
ms.technology: backup-restore
ms.prod_service: backup-restore
ms.reviewer: ''
ms.topic: quickstart
author: cawrites
ms.author: chadam
ms.openlocfilehash: d27f53f80c8f987106f90816a4566339d777e6f6
ms.sourcegitcommit: bb54e4c9dd8c97365b7a96dfcd557b8b86d06978
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2020
ms.locfileid: "97736903"
---
# <a name="quickstart-sql-backup-and-restore-to-azure-blob-storage-service"></a>Inicio rápido: Copia de seguridad y restauración de SQL en el servicio Azure Blob Storage

[!INCLUDE [sqlserver2016-asdbmi](../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Con este inicio rápido, entenderá mejor cómo escribir copias de seguridad en el servicio Azure Blob Storage y cómo restaurar desde este servicio.  En el artículo se explica cómo crear un contenedor de blobs de Azure, escribir una copia de seguridad en el Blob service y, luego, realizar una restauración.

> [!NOTE]
> SQL Server 2012 SP1 CU2 incorporó la compatibilidad con la copia de seguridad en Azure Blob Storage. SQL Server 2014 y versiones anteriores no admiten la firma de acceso compartido (SAS) que se describe en este artículo de inicio rápido.
>
> Para SQL Server 2014 y versiones anteriores, use [Tutorial: Copia de seguridad y restauración de SQL Server en el servicio Azure Blob Storage](/previous-versions/sql/2014/relational-databases/backup-restore/sql-server-backup-to-url).
>
  
## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido, debe estar familiarizado con los conceptos de copias de seguridad y restauración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y con la sintaxis de T-SQL.  Necesita una cuenta de Azure Storage, SQL Server Management Studio (SSMS) y acceso a un servidor que ejecute SQL Server o Azure SQL Managed Instance. Además, la cuenta que se usa para emitir comandos BACKUP o RESTORE debe tener el rol de base de datos **db_backupoperator** con permisos **Modificar cualquier credencial**. 

- Obtenga una [cuenta de Azure](https://azure.microsoft.com/offers/ms-azr-0044p/) gratis.
- Cree una [cuenta de almacenamiento de Azure](/azure/storage/common/storage-quickstart-create-account?tabs=portal).
- Instale [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md).
- Instale [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads) o implemente [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-get-started) con conectividad establecida mediante una [máquina virtual de Azure SQL](/azure/sql-database/sql-database-managed-instance-configure-vm) o de [punto a sitio](/azure/sql-database/sql-database-managed-instance-configure-p2s).
- Asigne la cuenta de usuario al rol [db_backupoperator](./security/authentication-access/database-level-roles.md) y conceda permisos [Modificar cualquier credencial](../t-sql/statements/alter-credential-transact-sql.md). 

## <a name="create-azure-blob-container"></a>Creación de un contenedor de blobs de Azure
Los contenedores proporcionan una agrupación de un conjunto de blobs. Todos los blobs deben estar en un contenedor. Una cuenta de almacenamiento puede contener un número ilimitado de contenedores, pero debe tener al menos uno. Un contenedor puede almacenar un número ilimitado de blobs. 

Para crear un contenedor, siga estos pasos:

1. Abra Azure Portal. 
1. Navegue a la cuenta de almacenamiento. 
1. Seleccione la cuenta de almacenamiento y baje hasta **Blob Services** (Servicios de blob).
1. Seleccione **Blobs** y después **+ Contenedor** para agregar un nuevo contenedor. 
1. Escriba el nombre del contenedor y tome nota del nombre de contenedor especificado. Esta información se usa en la dirección URL (ruta de acceso al archivo de copia de seguridad) en las instrucciones T-SQL más adelante en este inicio rápido. 
1. Seleccione **Aceptar**. 
    
    ![Nuevo contenedor](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/new-container.png)

  > [!NOTE]
  > La autenticación en la cuenta de almacenamiento es necesaria para las copias de seguridad y la restauración de SQL Server, incluso aunque elija crear un contenedor público. También puede crear un contenedor mediante programación con las API de REST. Para más información, vea [Create container](/rest/api/storageservices/Create-Container) (Creación de contenedor)

## <a name="create-a-test-database"></a>Creación de una base de datos de prueba 
En este paso, cree una base de datos de prueba con SQL Server Management Studio (SSMS). 

1. Inicie [SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md) y conéctese a la instancia de SQL Server.
1. Abra una ventana de **nueva consulta**. 
1. Ejecute el siguiente código Transact-SQL (T-SQL) para crear la base de datos de prueba. Actualice el nodo **Bases de datos** en el **Explorador de objetos** para ver la nueva base de datos. Las bases de datos recién creadas en SQL Managed Instance tienen habilitado TDE de manera automática, por lo que deberá deshabilitarlo para continuar. 

```sql
USE [master]
GO

-- Create database
CREATE DATABASE [SQLTestDB]
GO

-- Create table in database
USE [SQLTestDB]
GO
CREATE TABLE SQLTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO

-- Populate table 
USE [SQLTestDB]
GO

INSERT INTO SQLTest (ID, c1) VALUES (1, 'test1')
INSERT INTO SQLTest (ID, c1) VALUES (2, 'test2')
INSERT INTO SQLTest (ID, c1) VALUES (3, 'test3')
INSERT INTO SQLTest (ID, c1) VALUES (4, 'test4')
INSERT INTO SQLTest (ID, c1) VALUES (5, 'test5')
GO

SELECT * FROM SQLTest
GO

-- Disable TDE for newly-created databases on SQL Managed Instance 
USE [SQLTestDB];
GO
ALTER DATABASE [SQLTestDB] SET ENCRYPTION OFF;
GO
```

## <a name="create-credential"></a>Crear una credencial

Use la GUI de SQL Server Management Studio para crear la credencial siguiendo estos pasos. De manera alternativa, también puede crear la credencial [mediante programación](tutorial-use-azure-blob-storage-service-with-sql-server-2016.md#2---create-a-sql-server-credential-using-a-shared-access-signature). 

1. Expanda el nodo **Bases de datos** dentro del **Explorador de objetos** de [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Haga clic con el botón derecho en la base de datos `SQLTestDB` nueva, mantenga el puntero del mouse sobre **Tareas** y seleccione **Copia de seguridad…** para iniciar el Asistente de **copia de seguridad de base de datos**. 
1. Seleccione **Dirección URL** en el menú desplegable **Copia de seguridad en** de destino y seleccione **Agregar** para abrir el cuadro de diálogo **Seleccionar destino de la copia de seguridad**. 

   ![Copia de seguridad en dirección URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Seleccione **Nuevo contenedor** en el cuadro de diálogo **Seleccionar destino de la copia de seguridad** para iniciar la ventana **Conectarse a una suscripción de Microsoft**. 

   ![Captura de pantalla del cuadro de diálogo Seleccionar destino de la copia de seguridad con la opción Nuevo contenedor resaltada.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-backup-destination.png)

1. Para iniciar sesión en Azure Portal, seleccione **Iniciar sesión…** y continúe con el proceso de inicio de sesión. 
1. Seleccione la **suscripción** en el menú desplegable. 
1. Seleccione la **cuenta de almacenamiento** en el menú desplegable. 
1. Seleccione el contenedor que creó anteriormente en el menú desplegable. 
1. Seleccione **Crear credencial** para generar la *firma de acceso compartido (SAS)* .  **Guarde este valor, ya que lo necesitará para la restauración.**

   ![Crear una credencial](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/create-credential.png)

1. Haga clic en **Aceptar** para cerrar la ventana **Conectarse a una suscripción de Microsoft**. Con esto se rellena el valor del *contenedor de almacenamiento de Azure* en el cuadro de diálogo **Seleccionar destino de copia de seguridad**. Seleccione **Aceptar** para elegir el contenedor de almacenamiento seleccionado y cierre el cuadro de diálogo. 
1. En este momento, puede ir directamente al paso 4 de la sección siguiente para realizar la copia de seguridad de la base de datos, o bien cerrar el Asistente para la **copia de seguridad de base de datos** si en su lugar quiere seguir usando Transact-SQL para crear la copia de seguridad de la base de datos. 


## <a name="back-up-database"></a>Copia de seguridad de base de datos
En este paso, cree una copia de seguridad de la base de datos `SQLTestDB` en la cuenta de Azure Blob Storage con la GUI dentro de SQL Server Management Studio o con Transact-SQL (T-SQL). 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Si el Asistente para la **copia de seguridad de base de datos** todavía no está abierto, expanda el nodo **Bases de datos** dentro del **Explorador de objetos** de [SQL Server Management Studio(SSMS)](../ssms/download-sql-server-management-studio-ssms.md).
1. Haga clic con el botón derecho en la base de datos `SQLTestDB` nueva, mantenga el puntero del mouse sobre **Tareas** y seleccione **Copia de seguridad…** para iniciar el Asistente de **copia de seguridad de base de datos**. 
1. Seleccione **Dirección URL** en el menú desplegable **Copia de seguridad en** y seleccione **Agregar** para abrir el cuadro de diálogo **Seleccionar destino de la copia de seguridad**. 

   ![Copia de seguridad en dirección URL](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/back-up-to-url.png)

1. Seleccione el contenedor que creó en el paso anterior en el menú desplegable **Contenedor de almacenamiento de Azure**. 

   ![Contenedor de almacenamiento de Windows Azure](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/azure-storage-container.png)

1. Seleccione **Aceptar** en el Asistente para la **copia de seguridad de base de datos** para crear una copia de seguridad de su base de datos. 
1. Seleccione **Aceptar** una vez que la copia de seguridad de la base de datos se haya creado correctamente para cerrar todas las ventanas relacionadas con la copia de seguridad. 

   > [!TIP]
   > Puede generar un script de la instancia de Transact-SQL detrás de este comando si selecciona **Script** en la parte superior del Asistente para la **copia de seguridad de base de datos**: ![Comando de script](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/script-backup-command.png)


# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Cree una copia de seguridad de la base de datos con Transact-SQL mediante la ejecución del comando siguiente: 


```sql
USE [master]

BACKUP DATABASE [SQLTestDB] 
TO  URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak' 
WITH  COPY_ONLY, CHECKSUM
GO
```

---

## <a name="delete-database"></a>Eliminar base de datos
En este paso, elimine la base de datos antes de realizar la restauración. Este paso solo es necesario para este tutorial, pero no es probable que se use en procedimientos normales de administración de bases de datos. Puede omitir este paso, pero tendrá que cambiar el nombre de la base de datos durante la restauración en una instancia administrada, o bien ejecutar el comando de restauración `WITH REPLACE` para restaurar correctamente la base de datos en el entorno local. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Expanda el nodo **Bases de datos** en el **Explorador de objetos**, haga clic con el botón derecho en la base de datos `SQLTestDB` y seleccione Eliminar para iniciar el Asistente para **eliminar el objeto**. 
1. En una instancia administrada, seleccione **Aceptar** para eliminar la base de datos. En el entorno local, active la casilla junto a **Cerrar conexiones existentes** y seleccione **Aceptar** para eliminar la base de datos. 

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Para eliminar la base de datos, ejecute el comando de Transact-SQL siguiente:

```sql
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO

-- If connections currently exist on-premises, you'll need to set the database into single user mode first
USE [master]
GO
ALTER DATABASE [SQLTestDB] SET  SINGLE_USER WITH ROLLBACK IMMEDIATE
GO
USE [master]
GO
DROP DATABASE [SQLTestDB]
GO
```

---


## <a name="restore-database"></a>Restaurar base de datos 
En este paso, restaure la base de datos con la GUI de SQL Server Management Studio o con Transact-SQL. 

# <a name="ssms"></a>[SSMS](#tab/SSMS)

1. Haga clic con el botón secundario en el nodo **Bases de datos** en el **Explorador de objetos** dentro de SQL Server Management Studio y seleccione **Restaurar base de datos**. 
1. Seleccione **Dispositivo** y, luego, los puntos suspensivos (…) para elegir el dispositivo. 

   ![Selección de restaurar el dispositivo](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-device.png)

1. Seleccione **Dirección URL** en el menú desplegable **Tipo de medio de copia de seguridad** y seleccione **Agregar** para agregar el dispositivo. 

   ![Agregar dispositivo de copia de seguridad](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/add-backup-device.png)

1. Seleccione el contenedor en el menú desplegable y pegue la firma de acceso compartido (SAS) que guardó cuando creó la credencial. 

   ![Captura de pantalla del cuadro de diálogo Seleccionar ubicación de archivo de copia de seguridad con el campo Firma de acceso compartido rellenado.](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/restore-from-container.png)

1. Seleccione **Aceptar** para seleccionar la ubicación del archivo de copia de seguridad. 
1. Expanda **Contenedores** y seleccione el contenedor en el que se encuentra el archivo de copia de seguridad. 
1. Seleccione el archivo de copia de seguridad que quiere restaurar y, luego, seleccione **Aceptar**. Si no hay archivos visibles, es posible que esté usando una clave SAS incorrecta. Para regenerar la clave SAS, siga los mismos pasos de antes para agregar el contenedor. 

   ![Selección de restaurar el archivo](media/tutorial-sql-server-backup-and-restore-to-azure-blob-storage-service/select-restore-file.png)

1. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Seleccionar dispositivos de copia de seguridad**.
1. Seleccione **Aceptar** para restaurar la base de datos.

# <a name="transact-sql"></a>[Transact-SQL](#tab/tsql)

Para restaurar la base de datos local desde Azure Blob Storage, modifique el comando de Transact-SQL siguiente para usar su propia cuenta de almacenamiento y, luego, ejecútelo dentro de una ventana de consulta nueva.

```sql
USE [master]
RESTORE DATABASE [SQLTestDB] FROM 
URL = N'https://msftutorialstorage.blob.core.windows.net/sql-backup/sqltestdb_backup_2020_01_01_000001.bak'
```

---


## <a name="see-also"></a>Consulte también 
Estas son algunas lecturas recomendadas para comprender mejor los conceptos y los procedimientos recomendados al usar el servicio Azure Blob Storage para copias de seguridad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
-   [Copia de seguridad y restauración de SQL Server con el servicio Microsoft Azure Blob Storage](../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md)   
-   [Procedimientos recomendados y solución de problemas de Copia de seguridad en URL de SQL Server](../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)  
