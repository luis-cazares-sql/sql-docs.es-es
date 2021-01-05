---
title: Administrador de conexiones ADO.NET | Microsoft Docs
description: Un administrador de conexiones ADO.NET permite que un paquete acceda a orígenes de datos mediante un proveedor .NET.
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.adonetconnection.f1
helpviewer_keywords:
- connection managers [Integration Services], ADO.NET
- ADO.NET connection manager [Integration Services]
- connections [Integration Services], ADO.NET
ms.assetid: fc5daa2f-0159-4bda-9402-c87f1035a96f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8c2efe0145643f3f9e969c298e76115967477019
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637676"
---
# <a name="adonet-connection-manager"></a>Administrador de conexiones de ADO.NET

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Un administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] permite a un paquete tener acceso a orígenes de datos mediante un proveedor .NET. Normalmente, se usa este administrador de conexiones para acceder a orígenes de datos como [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. También puede acceder a orígenes de datos expuestos a través de OLE DB y XML en tareas personalizadas escritas en código administrado mediante un lenguaje como C#.  
  
Cuando se agrega un administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a un paquete, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crea un administrador de conexiones que se resuelve como una conexión [!INCLUDE[vstecado](../../includes/vstecado-md.md)] en tiempo de ejecución. Establece las propiedades del administrador de conexiones y agrega este a la colección **Connections** del paquete.  
  
La propiedad `ConnectionManagerType` del administrador de conexiones se establece en `ADO.NET`. El valor de `ConnectionManagerType` se califica para incluir el nombre del proveedor .NET que usa el administrador de conexiones.  
  
## <a name="adonet-connection-manager-troubleshooting"></a>Solución de problemas del administrador de conexiones ADO.NET  
Puede registrar las llamadas realizadas por el administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] a proveedores de datos externos. Luego, puede solucionar los problemas relacionados con las conexiones que el administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] establece con orígenes de datos externos. Para registrar las llamadas que el administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] realiza a proveedores de datos externos, habilite el registro de paquetes y seleccione el evento **Diagnostic** en el nivel de paquete. Para más información, vea [Herramientas para solucionar problemas con la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).  
  
Al ser leídos por un administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)], los datos de ciertos tipos de datos de fecha de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generan los resultados que se muestran en la tabla siguiente.  
  
|Tipos de datos de SQL Server|Resultado|  
|--------------------------|------------|  
|**time**, **datetimeoffset**|Se produce un error en el paquete a menos que el paquete utilice comandos SQL parametrizados. Para utilizar comandos SQL parametrizados, utilice la tarea Ejecute SQL en el paquete. Para más información, vea [Tarea Ejecutar SQL](../../integration-services/control-flow/execute-sql-task.md) y [Parámetros y códigos de retorno en la tarea Ejecutar SQL](../control-flow/execute-sql-task.md).|  
|**datetime2**|El administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] trunca el valor de milisegundos.|  
  
> [!NOTE]  
>  Para más información sobre los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y cómo se asignan a los tipos de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) y [Tipos de datos de Integration Services](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="adonet-connection-manager-configuration"></a>Configuración del administrador de conexiones ADO.NET  
  
Puede establecer propiedades a través del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
-   Proporcionar una cadena de conexión específica configurada para cumplir con los requisitos del proveedor .NET seleccionado.  
  
-   Según el proveedor, incluir el nombre del origen de datos al cual conectarse.  
  
-   Proporcionar credenciales de seguridad según resulte apropiado para el proveedor seleccionado.  
  
-   Indique si la conexión creada desde el administrador de conexiones se conserva en tiempo de ejecución.  
  
Muchas de las opciones de configuración del administrador de conexiones [!INCLUDE[vstecado](../../includes/vstecado-md.md)] dependen del proveedor .NET que usa el administrador de conexiones.  
  
Para más información sobre las propiedades que puede configurar en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)], consulte [Configuración del administrador de conexiones ADO.NET]().  
  
 Para obtener información sobre la configuración de un administrador de conexiones mediante programación, vea <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> y [Agregar conexiones mediante programación](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
### <a name="configure-adonet-connection-manager"></a>Configuración del administrador de conexiones ADO.NET
Utilice el cuadro de diálogo **Configurar el administrador de conexiones ADO.NET** para agregar una conexión a un origen de datos al que se puede tener acceso mediante un proveedor de datos de .NET Framework. Por ejemplo, un proveedor de este tipo es el proveedor SqlClient. El administrador de conexiones puede utilizar una conexión existente o puede crear una nueva.  
  
 Para obtener más información acerca del administrador de conexiones ADO .NET, vea [ADO.NET Connection Manager](../../integration-services/connection-manager/ado-net-connection-manager.md).  
  
#### <a name="options"></a>Opciones  
**Conexiones de datos**  
Seleccione una conexión de datos ADO.NET de la lista.  
  
**Propiedades de conexión de datos**  
Vea las propiedades y los valores de la conexión de datos ADO.NET seleccionada.  
  
**Nuevo**  
Cree una conexión de datos ADO.NET mediante el cuadro de diálogo **Administrador de conexiones** .  
  
**Eliminar**  
Seleccione una conexión y, luego, **Eliminar** para eliminarla.  
  
#### <a name="managed-identities-for-azure-resources-authentication"></a>Identidades administradas para la autenticación de los recursos de Azure
Al ejecutar paquetes SSIS en el [entorno de ejecución de integración de Azure-SSIS en Azure Data Factory](/azure/data-factory/concepts-integration-runtime#azure-ssis-integration-runtime), puede usar la [identidad administrada](/azure/data-factory/connector-azure-sql-database#managed-identity) asociada a su factoría de datos para la autenticación en Azure SQL Database o en Azure SQL Managed Instance. Puede acceder a la factoría designada y copiar datos desde la base de datos o en la base de datos usando esta identidad.

> [!NOTE]
>  Al usar la autenticación de Azure Active Directory (Azure AD) (incluida la autenticación de identidad administrada) para conectarse a Azure SQL Database o a Azure SQL Managed Instance, podría encontrarse con un problema relacionado con un error en la ejecución de paquetes o un cambio de comportamiento inesperado. Consulte [Características y limitaciones de Azure AD](/azure/sql-database/sql-database-aad-authentication#azure-ad-features-and-limitations) para más información.

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

Por último, configure la autenticación de la identidad administrada para el administrador de conexiones ADO.NET. Estas son las opciones para hacerlo:
    
- **Configuración durante su diseño.** En el Diseñador SSIS, haga clic con el botón derecho en el administrador de conexiones ADO.net y seleccione **Properties** (Propiedades). Actualice la propiedad `ConnectUsingManagedIdentity` a `True`.
    > [!NOTE]
    >  Actualmente, la propiedad del administrador de conexiones `ConnectUsingManagedIdentity` no surte efecto (lo que indica que la autenticación de identidad administrada no funciona) al ejecutar el paquete SSIS en el Diseñador SSIS o en [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server.
    
- **Configuración en tiempo de ejecución.** Al ejecutar el paquete a través de [SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md) o de la [actividad para ejecutar paquetes SSIS de Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity), busque el administrador de conexiones ADO.NET. Actualice su propiedad `ConnectUsingManagedIdentity` a `True`.
    > [!NOTE]
    >  En el entorno de ejecución de integración de Azure-SSIS, todos los demás métodos de autenticación (por ejemplo, autenticación integrada y contraseña) preconfigurados en el administrador de conexiones ADO.NET se invalidarán cuando se use la autenticación de la identidad administrada para establecer una conexión de base de datos.

> [!NOTE]
>  Para configurar la autenticación de identidad administrada en paquetes existentes, la manera preferida es volver a generar el proyecto SSIS con el [Diseñador SSIS más reciente](../../ssdt/download-sql-server-data-tools-ssdt.md) al menos una vez. Vuelva a implementar el proyecto SSIS en su entorno de ejecución de integración de Azure SSIS, de modo que la nueva propiedad del administrador de conexiones `ConnectUsingManagedIdentity` se agregue automáticamente a todos los administradores de conexiones ADO.NET del proyecto SSIS. La manera alternativa es usar directamente la invalidación de propiedades con la ruta de acceso a la propiedad **\Package.Connections[{el nombre del administrador de conexiones}].Properties[ConnectUsingManagedIdentity]** en tiempo de ejecución.

## <a name="see-also"></a>Consulte también  
 [Conexiones de Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
