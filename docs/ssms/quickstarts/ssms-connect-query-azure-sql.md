---
title: Conexión a una instancia de Azure SQL Database o Azure Managed Instance y realización de consultas con SQL Server Management Studio (SSMS)
description: Conéctese a una instancia de Azure SQL Database o Azure Managed Instance en SSMS. Cree y consulte una instancia de Azure SQL Database o Azure Managed Instance en SSMS mediante la ejecución de consultas de T-SQL básicas.
ms.prod: sql
ms.technology: ssms
ms.topic: quickstart
author: markingmyname
ms.author: maghan
ms.reviewer: sstein, mikeray
ms.custom: contperf-fy21q2
ms.date: 12/15/2020
ms.openlocfilehash: bb1eeea5d336ccba441cea5c6089d326c33dbdaf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597342"
---
# <a name="quickstart-connect-and-query-an-azure-sql-database-or-an-azure-managed-instance-using-sql-server-management-studio-ssms"></a>Inicio rápido: Conexión a una instancia de Azure SQL Database o Azure Managed Instance y realización de consultas con SQL Server Management Studio (SSMS)

[!INCLUDE [asdb](../../includes/applies-to-version/asdb.md)]

Empiece a usar SQL Server Management Studio (SSMS) para conectarse a la instancia de Azure SQL Database y a ejecutar algunos comandos de Transact-SQL (T-SQL).

En el artículo se muestra cómo seguir estos pasos:

> [!div class="checklist"]
> - Conexión a una base de datos de Azure SQL
> - Crear una base de datos
> - Crear una tabla en la nueva base de datos
> - Insertar filas en la nueva tabla
> - Consultar la nueva tabla y ver los resultados
> - Usar la tabla de la ventana de consulta para comprobar las propiedades de la conexión

## <a name="prerequisites"></a>Requisitos previos

- [SQL Server Management Studio](../download-sql-server-management-studio-ssms.md) instalado.
- [Azure SQL Database](https://azure.microsoft.com/free/sql-database/search/?&ef_id=CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE:G:s&OCID=AID2100131_SEM_CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE:G:s&gclid=CjwKCAiA17P9BRB2EiwAMvwNyDTqtKIHvBKK_qCTAnj3san_fx4KFjrSR8c58InygXQX5m_G71ZoMhoCzSMQAvD_BwE) o [Azure SQL Managed Instance](https://azure.microsoft.com/services/azure-sql/sql-managed-instance/)

## <a name="connect-to-an-azure-sql-database-or-azure-sql-managed-instance"></a>Conexión a una instancia de Azure SQL Database o Azure SQL Managed Instance

[!INCLUDE[ssms-connect-azure-ad](../../includes/ssms-connect-azure-ad.md)]

1. Inicie SQL Server Management Studio. La primera vez que ejecute SSMS se abrirá la ventana **Conectarse al servidor**. Si no se abre, puede abrirla manualmente seleccionando **Explorador de objetos** > **Conectar** > **Motor de base de datos**.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-object-explorer.png" alt-text="Vínculo Conectar en el Explorador de objetos":::

2. Aparecerá el cuadro de diálogo **Conectar con el servidor** . Escriba la siguiente información:

    |   Configuración   |   Valor sugerido   |   Descripción   |
    |-------------|------------------------|-----------------|
    | **Tipo de servidor** | Motor de base de datos | En **Tipo de servidor**, seleccione **Motor de base de datos** (suele ser la opción predeterminada). |
    | **Nombre del servidor** | Nombre completo del servidor | En **Nombre del servidor**, escriba el nombre de la instancia de *Azure SQL Database* o *Azure Managed Instance*. |
    | **Autenticación** | Autenticación de SQL Server | Use **Autenticación de SQL Server** para que se conecte Azure SQL. </br> </br> No se admite el método **Autenticación de Windows** para Azure SQL. Para obtener más información, vea [Autenticación de Azure SQL](/azure/sql-database/sql-database-security-overview#access-management). |
    | **Inicio de sesión** | Identificador de usuario de la cuenta del servidor | Identificador de usuario de la cuenta del servidor que se ha usado para crear el servidor. |
    | **Contraseña** | Contraseña de la cuenta del servidor | Contraseña de la cuenta del servidor que se ha usado para crear el servidor. |

    También puede modificar otras opciones de conexión seleccionando **Opciones**. Como ejemplos de las opciones de conexión tiene la base de datos a la que se está conectando, el valor de tiempo de espera de conexión y el protocolo de red. En este artículo se usan los valores predeterminados para todas las opciones.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-to-azure-sql-object-explorer.png" alt-text="Campo Nombre del servidor para Azure SQL":::

3. Una vez cumplimentados todos los campos, seleccione **Conectar**.

    También puede modificar otras opciones de conexión seleccionando **Opciones**. Como ejemplos de las opciones de conexión tiene la base de datos a la que se está conectando, el valor de tiempo de espera de conexión y el protocolo de red. En este artículo se usan los valores predeterminados para todas las opciones.

   Si no ha configurado las opciones del firewall, aparecerá un mensaje que le indicará que lo haga. Una vez que inicie sesión, rellene la información de inicio de sesión de la cuenta de Azure y continúe con la configuración de la regla de firewall. Después, seleccione **Aceptar**. Este mensaje es una acción que solo se realiza una vez. Una vez que haya configurado el firewall, el mensaje de firewall no debería aparecer.

    :::image type="content" source="media/ssms-connect-query-azure-sql/azure-sql-firewall-sign-in-3.png" alt-text="Nueva regla de firewall de Azure SQL":::

4. Para comprobar que la conexión de Azure SQL Database o Azure Managed Instance se ha realizado correctamente, expanda y explore los objetos desde el **Explorador de objetos**, donde se muestran el nombre del servidor, la versión de SQL Server y el nombre de usuario. Estos objetos son diferentes según el tipo de servidor.

    :::image type="content" source="media/ssms-connect-query-azure-sql/connect-azure-sql.png" alt-text="Conexión a SQL Azure DB":::

## <a name="troubleshoot-connectivity-issues"></a>Solución de problemas de conectividad

Puede experimentar problemas de conexión con Azure Synapse Analytics. Para obtener más información sobre cómo solucionar problemas de conexión, visite [Solución de problemas de conectividad](/azure/azure-sql/database/troubleshoot-common-errors-issues).

Puede evitar, solucionar, diagnosticar y mitigar los errores de conexión y transitorios que encuentre cuando interactúa con Azure SQL Database o Azure SQL Managed Instance. Para obtener más información, visite [Solución de errores de conexión transitorios](/azure/azure-sql/database/troubleshoot-common-connectivity-issues).

## <a name="create-a-database"></a>Crear una base de datos

Ahora siga estos pasos para crear una base de datos denominada TutorialDB:

1. Haga clic con el botón derecho en la instancia del servidor en el Explorador de objetos y seleccione **Nueva consulta**:

   :::image type="content" source="media/ssms-connect-query-azure-sql/new-query.png" alt-text="Vínculo Nueva consulta":::

2. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta:

    ```sql
    IF NOT EXISTS (
    SELECT name
    FROM sys.databases
    WHERE name = N'TutorialDB'
    )
    CREATE DATABASE [TutorialDB]
    GO
    
    ALTER DATABASE [TutorialDB] SET QUERY_STORE=ON
    GO
    ```

3. Para ejecutar la consulta, seleccione **Ejecutar** o presione F5 en el teclado.

   :::image type="content" source="media/ssms-connect-query-azure-sql/execute.png" alt-text="Comando Ejecutar":::
  
    Una vez hecha la consulta, en la lista de bases de datos del Explorador de objetos aparecerá la nueva base de datos TutorialDB. Si no aparece, haga clic con el botón derecho en el nodo **Bases de datos** y seleccione **Actualizar**.

## <a name="create-a-table-in-the-new-database"></a>Crear una tabla en la nueva base de datos

En esta sección creará una tabla en la base de datos TutorialDB recién creada. Como el editor de consultas sigue en el contexto de la base de datos *master*, debe cambiar el contexto de la conexión a la base de datos *TutorialDB* siguiendo estos pasos:

1. En la lista desplegable de bases de datos, seleccione la base de datos que quiera, como se muestra aquí:

   :::image type="content" source="media/ssms-connect-query-azure-sql/change-db.png" alt-text="Cambiar la base de datos":::

2. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta:

    ```sql
    USE [TutorialDB]
    -- Create a new table called 'Customers' in schema 'dbo'
    -- Drop the table if it already exists
    IF OBJECT_ID('dbo.Customers', 'U') IS NOT NULL
    DROP TABLE dbo.Customers
    GO
    -- Create the table in the specified schema
    CREATE TABLE dbo.Customers
    (
       CustomerId        INT    NOT NULL   PRIMARY KEY, -- primary key column
       Name      [NVARCHAR](50)  NOT NULL,
       Location  [NVARCHAR](50)  NOT NULL,
       Email     [NVARCHAR](50)  NOT NULL
    );
    GO
    ```

3. Para ejecutar la consulta, seleccione **Ejecutar** o presione F5 en el teclado.

Una vez hecha la consulta, se mostrará la nueva tabla Customers (Clientes) en la lista de tablas del Explorador de objetos. Si la tabla no aparece, haga clic con el botón derecho en el nodo **TutorialDB** > **Tablas** en el Explorador de objetos y, después, seleccione **Actualizar**.

   :::image type="content" source="media/ssms-connect-query-azure-sql/new-table.png" alt-text="Tabla nueva":::

## <a name="insert-rows-into-the-new-table"></a>Insertar filas en la nueva tabla

Ahora se insertarán algunas filas en la tabla Customers que ha creado. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta y, después, seleccione **Ejecutar**:

   ```sql
   -- Insert rows into table 'Customers'
   INSERT INTO dbo.Customers
      ([CustomerId],[Name],[Location],[Email])
   VALUES
      ( 1, N'Orlando', N'Australia', N''),
      ( 2, N'Keith', N'India', N'keith0@adventure-works.com'),
      ( 3, N'Donna', N'Germany', N'donna0@adventure-works.com'),
      ( 4, N'Janet', N'United States', N'janet1@adventure-works.com')
   GO
   ```

## <a name="query-the-table-and-view-the-results"></a>Consultar la tabla y ver los resultados

Los resultados de una consulta aparecen debajo de la ventana de texto de la consulta. Siga estos pasos para consultar la tabla Customers y ver las filas que se han insertado:

1. Pegue el siguiente fragmento de código de T-SQL en la ventana de consulta y, después, seleccione **Ejecutar**:

   ```sql
   -- Select rows from table 'Customers'
   SELECT * FROM dbo.Customers;
   ```

    Los resultados de la consulta se muestran debajo del área en la que se ha escrito el texto.

   :::image type="content" source="media/ssms-connect-query-azure-sql/query-results.png" alt-text="Lista de resultados":::

    También puede modificar la presentación de los resultados si selecciona una de las opciones siguientes:

   ![Tres opciones para mostrar los resultados de la consulta](media/ssms-connect-query-azure-sql/results.png)

   - El primer botón muestra los resultados en una **vista de texto**, tal y como se muestra en la imagen de la siguiente sección.
   - El botón central muestra los resultados en una **vista de cuadrícula**, que es la opción predeterminada.
       - Este valor se establece como el predeterminado.
   - El tercer botón le permite guardar los resultados en un archivo cuya extensión es .rpt de forma predeterminada.

## <a name="verify-your-connection-properties-by-using-the-query-window-table"></a>Comprobar las propiedades de la conexión usando la tabla de la ventana de consulta

Puede buscar información sobre las propiedades de la conexión en los resultados de la consulta. Después de ejecutar la consulta mencionada en el paso anterior, revise las propiedades de la conexión en la parte inferior de la ventana de consulta.

- Puede determinar el servidor y la base de datos a los que se ha conectado, así como el nombre de usuario que ha utilizado.
- También puede ver la duración de la consulta y el número de filas devueltas por la consulta ejecutada.

   :::image type="content" source="media/ssms-connect-query-azure-sql/connection-properties.png" alt-text="Propiedades de la conexión":::

## <a name="additional-tools"></a>Herramientas adicionales

También puede usar [Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) para conectarse y consultar [SQL Server](../../azure-data-studio/quickstart-sql-server.md), [Azure SQL Database](../../azure-data-studio/quickstart-sql-database.md) y [Azure Synapse Analytics](../../azure-data-studio/quickstart-sql-dw.md).

## <a name="next-steps"></a>Pasos siguientes

La mejor forma de familiarizarse con SSMS es practicar. Estos artículos lo ayudan con varias características disponibles dentro de SSMS.

- [Editor de consultas de SQL Server Management Studio (SSMS)](../f1-help/database-engine-query-editor-sql-server-management-studio.md)
- [Scripting](../tutorials/scripting-ssms.md)
- [Uso de plantillas en SSMS](../template/templates-ssms.md)
- [Configuración de SSMS](../tutorials/ssms-configuration.md)
- [Otras recomendaciones y trucos al usar SSMS](../tutorials/ssms-tricks.md)