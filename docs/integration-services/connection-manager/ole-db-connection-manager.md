---
title: Administrador de conexiones OLE DB | Microsoft Docs
description: Un administrador de conexiones OLE DB permite a un paquete conectarse a un origen de datos mediante un proveedor OLE DB.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.oledbconnection.f1
helpviewer_keywords:
- OLE DB connection manager
- data sources [Integration Services], connections
- connection managers [Integration Services], OLE DB
- connections [Integration Services], OLE DB
ms.assetid: 91e3622e-4b1a-439a-80c7-a00b90d66979
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c50a2e201bcba99299998dc232e1246005eb168
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97639504"
---
# <a name="ole-db-connection-manager"></a>Administrador de conexiones OLE DB

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Un administrador de conexiones OLE DB permite a un paquete conectarse a un origen de datos mediante un proveedor OLE DB. Por ejemplo, un administrador de conexiones OLE DB que se conecta a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede usar el proveedor [!INCLUDE[msCoName](../../includes/msconame-md.md)] OLE DB para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].    
    
> [!NOTE]
>  El proveedor OLE DB de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 11.0 no admite las nuevas palabras clave de cadena de conexión (MultiSubnetFailover=True) para la agrupación en clústeres de conmutación por error de varias subredes. Para más información, consulte las [notas de la versión de SQL Server](https://go.microsoft.com/fwlink/?LinkId=247824).    
> 
> [!NOTE]
>  Si el origen de datos es [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007 o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, el origen de datos requiere un proveedor de datos distinto al de las versiones anteriores de Excel o Access. Para obtener más información, vea [Conectarse a un libro de Excel](../load-data-to-from-excel-with-ssis.md) y [Conectarse a una base de datos de Access](./integration-services-ssis-connections.md).    
    
Varias tareas y componentes de flujo de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usan un administrador de conexiones OLE DB. Por ejemplo, el origen y el destino de OLE DB usan este administrador de conexiones para extraer y cargar datos. La tarea Ejecutar SQL puede usar este administrador de conexiones para conectarse a una base de datos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y ejecutar consultas.    
    
El administrador de conexiones OLE DB también se puede usar para acceder a orígenes de datos OLE DB en tareas personalizadas escritas en código no administrado que usa un lenguaje como C++.    
    
Cuando se agrega un administrador de conexiones OLE DB a un paquete, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea un administrador de conexiones que se resuelve como una conexión OLE DB en tiempo de ejecución, establece las propiedades del administrador de conexiones y agrega el administrador de conexiones a la colección **Connections** del paquete.    
    
La propiedad `ConnectionManagerType` del administrador de conexiones se establece en `OLEDB`.    
    
Configure el administrador de conexiones OLE DB de las maneras siguientes:    
    
-   Proporcionar una cadena de conexión específica configurada para cumplir con los requisitos del proveedor seleccionado.    
    
-   Según el proveedor, incluir el nombre del origen de datos al cual conectarse.    
    
-   Proporcionar credenciales de seguridad según resulte apropiado para el proveedor seleccionado.    
    
-   Indique si la conexión creada desde el administrador de conexiones se conserva en tiempo de ejecución.    
    
## <a name="log-calls-and-troubleshoot-connections"></a>Registro de llamadas y solución de problemas de conexiones    
 Puede registrar las llamadas realizadas por el administrador de conexiones OLE DB a proveedores de datos externos. Luego, puede solucionar los problemas relacionados con las conexiones que el administrador de conexiones OLE DB establece con orígenes de datos externos. Para registrar las llamadas que el administrador de conexiones OLE DB realiza a proveedores de datos externos, habilite el registro de paquetes y seleccione el evento **Diagnostic** en el nivel de paquete. Para más información, vea [Herramientas para solucionar problemas con la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).    
    
## <a name="configure-the-ole-db-connection-manager"></a>Configuración del administrador de conexiones OLE DB    
 Puede establecer propiedades a través del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación. Para obtener más información sobre las propiedades que puede configurar en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea [Configurar el administrador de conexiones OLE DB](). Para obtener información acerca de cómo configurar un administrador de conexiones mediante programación, vea la documentación de la clase **T:Microsoft.SqlServer.Dts.Runtime.ConnectionManager** en la Guía del desarrollador.    
    
### <a name="configure-ole-db-connection-manager"></a>Configuración del administrador de conexiones OLE DB

Use el cuadro de diálogo **Configurar el administrador de conexiones OLE DB** para agregar una conexión a un origen de datos. Esta conexión puede ser nueva o una copia de una conexión existente.  
  
> [!NOTE]  
>  Si el origen de datos es [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Excel 2007, el origen de datos requiere un administrador de conexiones distinto al de las versiones anteriores de Excel. Para más información, vea [Conectarse a un libro de Excel](../load-data-to-from-excel-with-ssis.md).  
>   
>  Si el origen de datos es [!INCLUDE[msCoName](../../includes/msconame-md.md)] Office Access 2007, el origen de datos requiere un proveedor OLE DB diferente que las versiones anteriores de Access. Para más información, vea [Conectarse a una base de datos de Access](./integration-services-ssis-connections.md).  
  
 Para obtener más información acerca del administrador de conexiones OLE DB, vea [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
#### <a name="options"></a>Opciones  
 **Conexiones de datos**  
 Seleccione en la lista una conexión de datos OLE DB existente.  
  
 **Propiedades de conexión de datos**  
 Vea las propiedades y los valores de la conexión de datos OLE DB seleccionada.  
  
 **Nuevo**  
 Permite crear una conexión de datos OLE DB con el cuadro de diálogo **Administrador de conexiones** .  
  
 **Eliminar**  
 Seleccione una conexión y, luego, **Eliminar** para eliminarla.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Identidades administradas para la autenticación de los recursos de Azure
Al ejecutar paquetes SSIS en el [entorno de ejecución de integración de Azure-SSIS en Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime), use la [identidad administrada](/azure/data-factory/connector-azure-sql-database#managed-identity) asociada a su factoría de datos para la autenticación en Azure SQL Database o Managed Instance. Puede acceder a la factoría designada y copiar datos desde la base de datos o en la base de datos usando esta identidad.

> [!NOTE]
>  Al usar la autenticación de Azure Active Directory (Azure AD) (incluida la autenticación de identidad administrada) para conectarse a Azure SQL Database o Managed Instance, podría encontrarse con un problema relacionado con un error en la ejecución de paquetes o un cambio de comportamiento inesperado. Consulte [Características y limitaciones de Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations) para más información.

Para usar la autenticación de la identidad administrada en Azure SQL Database, siga estos pasos para configurar la base de datos:

1. [Aprovisione un administrador de Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) para Azure SQL Server en Azure Portal, si aún no lo ha hecho. El administrador de Azure AD puede ser un usuario o un grupo de Azure AD. Si concede un rol de administrador al grupo con identidad administrada, omita los pasos 2 y 3. El administrador tendrá acceso completo a la base de datos.

1. [Cree usuarios de bases de datos independientes](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) para la identidad administrada de Data Factory. Conéctese a la base de datos en la que va a copiar o pegar datos mediante herramientas como SSMS, con una identidad de Azure AD que tenga al menos el permiso ALTER ANY USER. Ejecute el T-SQL siguiente: 
    
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Conceda a la identidad administrada de Data Factory los permisos necesarios, tal como lo haría normalmente para los usuarios de SQL y otros usuarios. Consulte [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md) para conocer los roles adecuados. Ejecute el código siguiente: Para más opciones, consulte [este documento](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md).

    ```sql
    EXEC sp_addrolemember [role name], [your data factory name];
    ```

Para usar la autenticación de identidad administrada en Azure SQL Managed Instance, siga estos pasos para configurar la base de datos:
    
1. [Aprovisione un administrador de Azure Active Directory](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance) para la instancia administrada en Azure Portal, si aún no lo ha hecho. El administrador de Azure AD puede ser un usuario o un grupo de Azure AD. Si concede un rol de administrador al grupo con identidad administrada, omita los pasos 2-4. El administrador tendrá acceso completo a la base de datos.

1. [Cree inicios de sesión](../../t-sql/statements/create-login-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true) para la identidad administrada de Data Factory. En SQL Server Management Studio (SSMS), conéctese a la instancia administrada con una cuenta de SQL Server que sea **sysadmin**. En la base de datos **maestra**, ejecute el siguiente script T-SQL:

    ```sql
    CREATE LOGIN [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. [Cree usuarios de bases de datos independientes](/azure/sql-database/sql-database-aad-authentication-configure#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities) para la identidad administrada de Data Factory. Conéctese a la base de datos de origen o destino de copia de los datos y ejecute el siguiente script T-SQL: 
  
    ```sql
    CREATE USER [your data factory name] FROM EXTERNAL PROVIDER;
    ```

1. Conceda a la identidad administrada de Data Factory los permisos necesarios, tal como lo haría normalmente para los usuarios de SQL y otros usuarios. Ejecute el código siguiente: Para más opciones, consulte [este documento](../../t-sql/statements/alter-role-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true).

    ```sql
    ALTER ROLE [role name e.g., db_owner] ADD MEMBER [your data factory name];
    ```

Después, configure el proveedor OLE DB para el administrador de conexiones OLE DB. Estas son las opciones para hacerlo:
    
- **Configuración durante su diseño.** En el Diseñador SSIS, haga doble clic en el administrador de conexiones OLE DB para abrir la ventana del **administrador de conexiones**. En la lista desplegable **Proveedor**, seleccione [**Microsoft OLE DB Driver for SQL Server**](https://go.microsoft.com/fwlink/?linkid=871294).
    > [!NOTE]
    >  Puede ser que otros proveedores de la lista desplegable no admitan la autenticación de la identidad administrada.
    
- **Configuración en tiempo de ejecución.** Al ejecutar el paquete a través de [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) o de la [actividad para ejecutar paquetes SSIS de Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity), busque la propiedad del administrador de conexiones **ConnectionString** del administrador de conexiones de OLE DB. Actualice la propiedad de conexión `Provider` a `MSOLEDBSQL` (es decir, Microsoft OLE DB Driver for SQL Server).
    ```vb
    Data Source=serverName;Initial Catalog=databaseName;Provider=MSOLEDBSQL;...
    ```

Por último, configure la autenticación de la identidad administrada del administrador de conexiones OLE DB. Estas son las opciones para hacerlo:
    
- **Configuración durante su diseño.** En el Diseñador SSIS, haga clic con el botón derecho en el administrador de conexiones OLE DB y seleccione **Properties** (Propiedades). Actualice la propiedad `ConnectUsingManagedIdentity` a `True`.
    > [!NOTE]
    >  Actualmente, la propiedad del administrador de conexiones `ConnectUsingManagedIdentity` no surte efecto (lo que indica que la autenticación de identidad administrada no funciona) al ejecutar el paquete SSIS en el Diseñador SSIS o en [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.

- **Configuración en tiempo de ejecución.** Al ejecutar el paquete a través de SSMS o de una **actividad de ejecución de paquetes SQL**, busque el administrador de conexiones OLE DB y actualice su propiedad `ConnectUsingManagedIdentity` a `True`.
    > [!NOTE]
    >  En el entorno de ejecución de integración de Azure-SSIS, todos los demás métodos de autenticación (por ejemplo, seguridad integrada y contraseña) preconfigurados en el administrador de conexiones OLE DB se invalidarán cuando se use la autenticación de identidad administrada para establecer una conexión de base de datos.

> [!NOTE]
>  Para configurar la autenticación de identidad administrada en paquetes existentes, la manera preferida es volver a generar el proyecto SSIS con el [Diseñador SSIS más reciente](../../ssdt/download-sql-server-data-tools-ssdt.md) al menos una vez. Vuelva a implementar el proyecto SSIS en su entorno de ejecución de integración de Azure SSIS, de modo que la nueva propiedad del administrador de conexiones `ConnectUsingManagedIdentity` se agregue automáticamente a todos los administradores de conexiones OLE DB del proyecto SSIS. La manera alternativa es usar directamente la invalidación de propiedades con la ruta de acceso a la propiedad **\Package.Connections[{el nombre del administrador de conexiones}].Properties[ConnectUsingManagedIdentity]** en tiempo de ejecución.

## <a name="see-also"></a>Consulte también    
 [Origen de OLE DB](../../integration-services/data-flow/ole-db-source.md)     
 [Destino de OLE DB](../../integration-services/data-flow/ole-db-destination.md)     
 [Tarea Ejecutar SQL](../../integration-services/control-flow/execute-sql-task.md)     
 [Conexiones de Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)    
    
