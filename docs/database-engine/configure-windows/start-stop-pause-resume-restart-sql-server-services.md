---
title: Inicio, detención, pausa, reanudación o reinicio de servicios de SQL Server
description: Obtenga información sobre cómo iniciar, detener, pausar, reanudar o reiniciar varios servicios SQL Server. Vea cómo utilizar Transact-SQL, PowerShell y otras herramientas para llevar a cabo estas acciones.
ms.custom: ''
ms.date: 03/05/2020
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Configuration Manager, start and stop services
- stopping SQL Server Agent
- parameters [SQL Server], startup options
- SQL Server, startup options
- Database Engine [SQL Server], starting and stopping services
- pausing SQL Server
- PowerShell [SQL Server], starting and stopping services
- single-user mode [SQL Server], starting in
- SQL Server Management Studio [SQL Server], starting or stopping services
- stopping SQL Server Browser service
- starting SQL Server Agent
- default instances [SQL Server], starting and stopping
- SQL Server Agent, starting and stopping
- command prompt [SQL Server], starting and stopping SQL Server services
- continuing SQL Server
- starting SQL Server Database Engine
- net stop commands [SQL Server]
- command prompt [SQL Server], SQL Browser service
- Configuration Manager, start and stop services
- resuming SQL Server
- startup options [SQL Server]
- named instances [SQL Server], starting and stopping
- net start commands [SQL Server]
- SQL Server, starting and stopping
- stopping SQL Server
- starting SQL Server Browser service
- SQL Server Database Engine setting startup options
- administering SQL Server, starting and stopping services
- Management Studio [SQL Server], starting or stopping services
ms.assetid: 32660a02-e5a1-411a-9e57-7066ca459df6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: 697cf043c4582796015b061c857145c214d105fb
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091778"
---
# <a name="start-stop-pause-resume-restart-sql-server-services"></a>Inicio, detención, pausa, reanudación o reinicio de servicios de SQL Server

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

En este tema se describe cómo iniciar, detener, pausar, reanudar o reiniciar el motor de base de datos de SQL Server, el Agente SQL Server o el servicio SQL Server Browser mediante el Administrador de configuración de SQL Server, SQL Server Management Studio (SSMS), comandos de net desde un símbolo del sistema, Transact-SQL o PowerShell.

## <a name="identify-the-service"></a>Identificación del servicio

Los componentes de SQL Server son programas ejecutables que se ejecutan como un servicio de Windows. Los programas que se ejecutan como servicio de Windows pueden seguir funcionando sin mostrar actividad alguna en la pantalla del equipo.

### <a name="database-engine-service"></a>Servicio Motor de base de datos

El proceso ejecutable que es el Motor de base de datos de SQL Server. El Motor de base de datos puede ser la instancia predeterminada (límite de una por equipo), o bien una de las numerosas instancias con nombre del Motor de base de datos. Use el Administrador de configuración de SQL Server para determinar las instancias del Motor de base de datos que están instaladas en el equipo. La instancia predeterminada (si la instala) aparece como **SQL Server (MSSQLSERVER)** . Las instancias con nombre (si las instala) aparecen como **SQL Server (<nombre_instancia>)** . De forma predeterminada, SQL Server Express se instala como **SQL Server (SQLEXPRESS)** .

### <a name="sql-server-agent-service"></a>servicio del Agente SQL Server

Servicio de Windows que ejecuta tareas administrativas programadas, denominadas trabajos y alertas. Para obtener más información, consulte [SQL Server Agent](../../ssms/agent/sql-server-agent.md). Agente SQL Server no está disponible en todas las ediciones de SQL Server. Para obtener una lista de las características admitidas por las ediciones de SQL Server, vea [Características compatibles con las ediciones de SQL Server 2019](../../sql-server/editions-and-components-of-sql-server-version-15.md).

### <a name="sql-server-browser-service"></a>servicio SQL Server Browser

Un servicio de Windows que escucha las solicitudes entrantes de recursos de SQL Server y proporciona información sobre las instancias de SQL Server instaladas en el equipo. Una sola instancia del servicio SQL Server Browser se usa para todas las instancias de SQL Server instaladas en el equipo.

### <a name="additional-information"></a>Información adicional

- Al pausar el servicio Motor de base de datos se impide que los nuevos usuarios se conecten al Motor de base de datos, pero los que ya estén conectados pueden seguir trabajando hasta que sus conexiones se interrumpan. Use la pausa cuando desee esperar a que los usuarios completen su trabajo antes de detener el servicio. Esto les permite completar las transacciones que están en curso. *Reanudar* permite que el Motor de base de datos vuelva a aceptar conexiones nuevas. El servicio Agente SQL Server no se puede pausar ni reanudar.  

- El Administrador de configuración de SQL Server y SSMS muestran el estado actual de los servicios mediante el uso de los iconos siguientes.  

    **Administrador de configuración de SQL Server**

  - La flecha verde del icono situado junto al nombre del servicio indica que el servicio está iniciado.

  - El cuadrado rojo del icono situado junto al nombre del servicio indica que el servicio está detenido.

  - Dos líneas verticales de color azul en el icono situado junto al nombre del servicio indican que el servicio está pausado.

  - Al reiniciar el Motor de base de datos, un cuadrado de color rojo indica que el servicio se ha detenido y una flecha de color verde que se ha iniciado correctamente.

    **SQL Server Management Studio (SSMS)**

  - La flecha blanca sobre un icono de círculo verde situado junto al nombre del servicio indica que el servicio está iniciado.  

  - El cuadrado blanco sobre un icono de círculo rojo situado junto al nombre del servicio indica que el servicio está detenido.  

  - Dos líneas verticales de color blanco sobre un icono de círculo azul situado junto al nombre del servicio indican que el servicio está pausado.  

- Al usar el Administrador de configuración de SQL Server o SSMS, solo están disponibles las opciones que son posibles. Por ejemplo, si el servicio ya está iniciado, **Iniciar** no está disponible.

- Cuando se ejecuta en un clúster, el servicio Motor de base de datos de SQL Server se administra mejor mediante el Administrador de clústeres.  

### <a name="security"></a>Seguridad

#### <a name="permissions"></a>Permisos

De forma predeterminada, solo los miembros del grupo local de administradores pueden iniciar, detener, pausar, reanudar o reiniciar un servicio. Para conceder la capacidad de administrar servicios a usuarios que no son administradores, vea [CÓMO: Conceder a los usuarios derechos para administrar servicios en la familia Windows Server 2003](https://support.microsoft.com/kb/325349). El proceso es similar en las otras versiones de Windows Server.

La detención del Motor de base de datos mediante el comando **SHUTDOWN** de Transact-SQL requiere la pertenencia a los roles fijos de servidor **sysadmin** o **serveradmin**, y no es transferible.

## <a name="sql-server-configuration-manager"></a>Administrador de configuración de SQL Server

### <a name="starting-sql-server-configuration-manager"></a>Inicio del Administrador de configuración de SQL Server

En el menú **Inicio** , seleccione **Todos los programas**, **Microsoft SQL Server**, **Herramientas de configuración** y haga clic en **Administrador de configuración de SQL Server**.

Como el Administrador de configuración de SQL Server es un complemento del programa Microsoft Management Console y no un programa independiente, el Administrador de configuración de SQL Server no aparece como aplicación en las versiones más recientes de Windows. Cuando instala Windows en la unidad C, estas son las rutas de acceso a las cuatro últimas versiones.  

|Versión|Path|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|SQL Server 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|SQL Server 2014|C:\Windows\SysWOW64\SQLServerManager12.msc|
|SQL Server 2012|C:\Windows\SysWOW64\SQLServerManager11.msc|

#### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="configmande"></a> Para iniciar, detener, pausar, reanudar o reiniciar una instancia del Motor de base de datos de SQL Server

1. Inicie el Administrador de configuración de SQL Server con las instrucciones anteriores.

2. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.

3. En el panel de la izquierda del Administrador de configuración de SQL Server, haga clic en **Servicios de SQL Server**.

4. En el panel de resultados, haga clic con el botón derecho en **SQL Server (MSSQLServer)** o en una instancia con nombre y luego haga clic en **Iniciar**, **Detener**, **Pausar**, **Reanudar** o **Reiniciar**.

5. Haga clic en **Aceptar** para cerrar la herramienta Administrador de configuración de SQL Server.

> [!NOTE]
> Para iniciar una instancia del Motor de base de datos de SQL Server con opciones de inicio, vea [Configuración de opciones de inicio del servidor &#40;Administrador de configuración de SQL Server&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  

#### <a name="to-start-stop-pause-resume-or-restart-the-sql-server-browser-or-an-instance-of-the-sql-server-agent"></a>Para iniciar, detener, pausar, reanudar o reiniciar SQL Server Browser o una instancia de Agente SQL Server

1. Inicie el Administrador de configuración de SQL Server con las instrucciones anteriores.

2. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.

3. En el panel de la izquierda del Administrador de configuración de SQL Server, haga clic en **Servicios de SQL Server**.

4. En el panel de resultados, haga clic con el botón derecho en **SQL Server Browser**, **Agente SQL Server (MSSQLServer)** , o bien en **Agente SQL Server (<nombre_de_instancia>)** para una instancia con nombre y, luego, haga clic en **Iniciar**, **Detener**, **Pausar**, **Reanudar** o **Reiniciar**.  

5. Haga clic en **Aceptar** para cerrar la herramienta Administrador de configuración de SQL Server.  

> [!NOTE]
> El Agente SQL Server no se puede pausar.

## <a name="sql-server-management-studio"></a>SQL Server Management Studio

### <a name="to-start-stop-pause-resume-or-restart-an-instance-of-the-sql-server-database-engine"></a><a name="ssmsde"></a> Para iniciar, detener, pausar, reanudar o reiniciar una instancia del Motor de base de datos de SQL Server

1. En el Explorador de objetos, conéctese a la instancia del Motor de base de datos, haga clic con el botón derecho en la instancia del Motor de base de datos que quiere iniciar y, luego, haga clic en **Iniciar**, **Detener**, **Pausar**, **Reanudar** o **Reiniciar**.

    O bien, en Servidores registrados, haga clic con el botón derecho en la instancia del Motor de base de datos que quiere iniciar, seleccione **Control de servicios** y, luego, haga clic en **Iniciar**, **Detener**, **Pausar**, **Reanudar** o **Reiniciar**.

2. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.

3. Cuando se le pregunte si quiere actuar, haga clic en **Sí**.  

#### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-agent"></a>Para iniciar, detener o reiniciar una instancia del Agente SQL Server

1. En el Explorador de objetos, conéctese a la instancia del Motor de base de datos, haga clic con el botón derecho en **Agente SQL Server** y, luego, haga clic en **Iniciar**, **Detener** o **Reiniciar**.

2. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , haga clic en **Sí**.

3. Cuando se le pregunte si quiere actuar, haga clic en **Sí**.

## <a name="command-prompt-window-using-net-commands"></a><a name="CommandPrompt"></a> Ventana del símbolo del sistema con comandos de net

Los servicios de Microsoft SQL Server se pueden iniciar, detener o pausar mediante comandos **net** de Microsoft Windows.

### <a name="to-start-the-default-instance-of-the-database-engine"></a><a name="dbDefault"></a> Para iniciar la instancia predeterminada del Motor de base de datos

- En el símbolo del sistema, escriba uno de los siguientes comandos:  
  
    **net start "SQL Server (MSSQLSERVER)"**

   O bien  

    **net start MSSQLSERVER**

### <a name="to-start-a-named-instance-of-the-database-engine"></a><a name="dbNamed"></a> Para iniciar una instancia con nombre del Motor de base de datos

- En el símbolo del sistema, escriba uno de los siguientes comandos. Sustituya *\<instancename>* por el nombre de la instancia que desea administrar.  
  
    **net start "SQL Server (** *instancename* **)"**
  
   O bien  
  
    **net start MSSQL$** *instancename*  
  
### <a name="to-start-the-database-engine-with-startup-options"></a><a name="dbStartup"></a> Para iniciar el Motor de base de datos con opciones de inicio  

- Agregue las opciones de inicio al final de la instrucción **net start "SQL Server (MSSQLSERVER)"** , separadas por un espacio. Cuando se inicia mediante **net start**, las opciones de inicio usan una barra (/) en lugar de un guión (-).  
  
    **net start "SQL Server (MSSQLSERVER)" /f /m**
  
   O bien  
  
    **net start MSSQLSERVER /f /m**
  
  > [!NOTE]
  >  Para obtener más información sobre las opciones de inicio del servicio, vea [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
###  <a name="to-start-the-sql-server-agent-on-the-default-instance-of-sql-server"></a><a name="agDefault"></a> Para iniciar Agente SQL Server en la instancia predeterminada de SQL Server
  
- En el símbolo del sistema, escriba uno de los siguientes comandos:  
  
    **net start "SQL Server Agent (MSSQLSERVER)"**
  
   O bien  
  
    **net start SQLSERVERAGENT**
  
###  <a name="to-start-the-sql-server-agent-on-a-named-instance-of-sql-server"></a><a name="agNamed"></a> Para iniciar Agente SQL Server en una instancia con nombre de SQL Server  
  
- En el símbolo del sistema, escriba uno de los siguientes comandos. Sustituya *instancename* por el nombre de la instancia que quiere administrar.  
  
    **net start "SQL Server Agent(** *instancename* **)"**
  
   O bien  
  
    **net start SQLAgent$** *instancename*  
  
 Para obtener información sobre cómo ejecutar Agente SQL Server en modo detallado para la solución de problemas, vea [sqlagent90 (aplicación)](../../tools/sqlagent90-application.md).  

### <a name="to-start-the-sql-server-browser"></a><a name="Browser"></a> Para iniciar SQL Server Browser  

- En el símbolo del sistema, escriba uno de los siguientes comandos:  
  
    **net start "SQL Server Browser"**
  
   O bien  
  
    **net start SQLBrowser**
  
### <a name="to-pause-or-stop-services-from-the-command-prompt-window"></a><a name="pauseStop"></a> Para pausar o detener los servicios desde la ventana del símbolo del sistema  

- Para pausar o detener servicios, modifique los comandos como se indica a continuación.  

- Para pausar un servicio, reemplace **net start** por **net pause**.  

- Para detener un servicio, reemplace **net start** por **net stop**.  

## <a name="transact-sql"></a><a name="TsqlProcedure"></a> Transact-SQL

El Motor de base de datos se puede detener mediante la instrucción **SHUTDOWN**.  

### <a name="to-stop-the-database-engine-using-transact-sql"></a>Para detener el Motor de base de datos mediante Transact-SQL

- Para esperar a que finalicen las instrucciones de Transact-SQL y los procedimientos almacenados en ejecución y, después, detener el Motor de base de datos, ejecute la instrucción siguiente.  
  
    ```sql
    SHUTDOWN;
    ```
  
- Para detener el Motor de base de datos de manera inmediata, ejecute la instrucción siguiente.  
  
    ```sql
    SHUTDOWN WITH NOWAIT;
    ```

Para obtener más información sobre la instrucción **SHUTDOWN**, vea [SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md).  

## <a name="powershell"></a><a name="PowerShellProcedure"></a> PowerShell

### <a name="to-start-and-stop-database-engine-services"></a>Para iniciar y detener servicios de Motor de base de datos

1. En una ventana del símbolo del sistema, inicie SQL Server PowerShell mediante la ejecución del comando siguiente.  

    ```cmd
    sqlps
    ```

2. En un símbolo del sistema de SQL Server PowerShell, ejecute el comando siguiente. Reemplace `computername` por el nombre de su equipo.  

    ```powershell
    # Get a reference to the ManagedComputer class.
    CD SQLSERVER:\SQL\computername
    $Wmi = (get-item .).ManagedComputer
    ```

3. Identifique el servicio que desea detener o iniciar. Elija una de las líneas siguientes. Reemplace `instancename` por el nombre de la instancia con nombre.

    - Para obtener una referencia a la instancia predeterminada del Motor de base de datos.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQLSERVER']
        ```

    - Para obtener una referencia a una instancia con nombre del Motor de base de datos.

        ```powershell
        $DfltInstance = $Wmi.Services['MSSQL$instancename']
        ```

    - Para obtener una referencia al servicio Agente SQL Server en la instancia predeterminada del Motor de base de datos.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLSERVERAGENT']
        ```

    - Para obtener una referencia al servicio Agente SQL Server en una instancia con nombre del Motor de base de datos.  

        ```powershell
        $DfltInstance = $Wmi.Services['SQLAGENT$instancename']
        ```

    - Para obtener una referencia al servicio SQL Server Browser.

        ```powershell
        $DfltInstance = $Wmi.Services['SQLBROWSER']
        ```

4. Complete el ejemplo para iniciar y detener el servicio seleccionado.
  
    ```powershell
    # Display the state of the service.
    $DfltInstance
    # Start the service.
    $DfltInstance.Start();
    # Wait until the service has time to start.
    # Refresh the cache.  
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    # Stop the service.
    $DfltInstance.Stop();
    # Wait until the service has time to stop.
    # Refresh the cache.
    $DfltInstance.Refresh();
    # Display the state of the service.
    $DfltInstance
    ```  
  
##  <a name="using-service-controller-class"></a><a name="ServiceController"></a> Uso de la clase de controlador de servicio

Puede utilizar la clase ServiceController para controlar el servicio SQL Server o cualquier otro servicio de Windows. Para obtener un ejemplo sobre cómo hacerlo, vea la [clase ServiceController](/dotnet/api/system.serviceprocess.servicecontroller).

## <a name="manage-the-sql-server-service-on-linux"></a>Administración del servicio SQL Server en Linux

### <a name="to-start-stop-or-restart-an-instance-of-the-sql-server-database-engine"></a>Para iniciar, detener o reiniciar una instancia del Motor de base de datos de SQL Server

A continuación se muestra cómo iniciar, detener, reiniciar y comprobar el estado del [servicio SQL Server en Linux](../../linux/sql-server-linux-troubleshooting-guide.md#manage-the-sql-server-service).

Compruebe el estado del servicio SQL Server con este comando:

   ```bash
   sudo systemctl status mssql-server
   ```

Puede detener, iniciar o reiniciar el servicio SQL Server según sea necesario mediante los comandos siguientes:

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl start mssql-server
   sudo systemctl restart mssql-server
   ```

## <a name="next-steps"></a>Pasos siguientes

- [Información general de la documentación de instalación de SQL Server](../install-windows/install-sql-server.md)
- [Ver y leer los archivos de registro de instalación de SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md)
- [Iniciar SQL Server con la configuración mínima](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md)
- [Características admitidas por las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)