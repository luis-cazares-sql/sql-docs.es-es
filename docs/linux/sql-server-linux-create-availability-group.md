---
title: Creación y configuración de un grupo de disponibilidad para SQL Server en Linux
description: En este tutorial se muestra cómo crear y configurar grupos de disponibilidad para SQL Server en Linux, además de cómo crear puntos de conexión y certificados de grupo de disponibilidad.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/28/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: ecb634a770b4990bfbbc094b15498b03bdea4a57
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172007"
---
# <a name="create-and-configure-an-availability-group-for-sql-server-on-linux"></a>Creación y configuración de un grupo de disponibilidad para SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este tutorial se habla de cómo crear y configurar un grupo de disponibilidad para [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] en Linux. A diferencia de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] y versiones anteriores en Windows, puede habilitar grupos de disponibilidad al crear primero el clúster de Pacemaker subyacente o sin hacerlo. La integración con el clúster, si fuera necesaria, no se realiza hasta más tarde.

El tutorial incluye las siguientes tareas:
 
> [!div class="checklist"]
> * Habilitar grupos de disponibilidad.
> * Crear puntos de conexión y certificados de grupo de disponibilidad.
> * Usar [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL para crear un grupo de disponibilidad.
> * Crear el inicio de sesión de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] y los permisos para Pacemaker.
> * Crear recursos de grupo de disponibilidad en un clúster de Pacemaker (solo tipo Externo).

## <a name="prerequisite"></a>Requisito previo
- Implemente el clúster de alta disponibilidad de Pacemaker tal como se explica en [Implementar un clúster de Pacemaker para SQL Server en Linux](sql-server-linux-deploy-pacemaker-cluster.md).


## <a name="enable-the-availability-groups-feature"></a>Habilitar la característica de grupos de disponibilidad

A diferencia de como se hace en Windows, no puede usar PowerShell ni [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] Configuration Manager para habilitar la característica de grupos de disponibilidad. En Linux, debe usar `mssql-conf` para habilitar la característica. Hay dos maneras de habilitar la característica de grupos de disponibilidad: usar la utilidad `mssql-conf` o editar el archivo `mssql.conf` manualmente.

> [!IMPORTANT]
> La característica de grupos de disponibilidad debe estar habilitada para las réplicas de solo configuración, incluso en [!INCLUDE[ssexpress-md](../includes/ssexpress-md.md)].

### <a name="use-the-mssql-conf-utility"></a>Usar la utilidad mssql-conf

En un símbolo del sistema, emita lo siguiente:

```bash
sudo /opt/mssql/bin/mssql-conf set hadr.hadrenabled 1
```

### <a name="edit-the-mssqlconf-file"></a>Editar el archivo mssql.conf

También puede modificar el archivo `mssql.conf`, que se encuentra en la carpeta `/var/opt/mssql`, para agregar las siguientes líneas:

```
[hadr]

hadr.hadrenabled = 1
```

### <a name="restart-ssnoversion-md"></a>Reiniciar [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]
Después de habilitar los grupos de disponibilidad, como en Windows, debe reiniciar [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Lo puede hacer con el siguiente código:

```bash
sudo systemctl restart mssql-server
```

## <a name="create-the-availability-group-endpoints-and-certificates"></a>Crear los puntos de conexión y los certificados del grupo de disponibilidad

Un grupo de disponibilidad usa puntos de conexión TCP para la comunicación. En Linux, los puntos de conexión de un grupo de disponibilidad solo se admiten si se usan certificados para la autenticación. Esto significa que el certificado de una instancia debe restaurarse en todas las demás instancias que vayan a ser réplicas participantes en el mismo grupo de disponibilidad. El proceso de certificado es necesario incluso para una réplica de solo configuración. 

La creación de puntos de conexión y la restauración de certificados solo se pueden realizar mediante Transact-SQL. También puede usar certificados no generados por [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]. Además necesita un proceso para administrar y reemplazar los certificados que expiren.

> [!IMPORTANT]
> Si tiene previsto usar el asistente de [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] para crear el grupo de disponibilidad, debe crear y restaurar los certificados mediante Transact-SQL en Linux.

Para obtener la sintaxis completa de las opciones disponibles para los distintos comandos (como seguridad adicional), vea:

-   [BACKUP CERTIFICATE](../t-sql/statements/backup-certificate-transact-sql.md)
-   [CREATE CERTIFICATE](../t-sql/statements/create-certificate-transact-sql.md)
-   [CREATE ENDPOINT](../t-sql/statements/create-endpoint-transact-sql.md)

> [!NOTE]
> Aunque va a crear un grupo de disponibilidad, el tipo de punto de conexión usa *FOR DATABASE_MIRRORING*, ya que algunos aspectos subyacentes se han compartido en algún momento con esa característica ahora en desuso.

En este ejemplo se crean certificados para una configuración de tres nodos. Los nombres de instancia son LinAGN1, LinAGN2 y LinAGN3.

1.  Ejecute lo siguiente en LinAGN1 para crear la clave maestra, el certificado y el punto de conexión, así como para realizar una copia de seguridad del certificado. En este ejemplo se usa el puerto TCP típico de 5022 para el punto de conexión.
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN1_Cert
    WITH SUBJECT = 'LinAGN1 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN1_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN1_Cert,
        ROLE = ALL);
    
    GO
    ```
    
2.  Haga lo mismo en LinAGN2:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    WITH SUBJECT = 'LinAGN2 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN2_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN2_Cert,
        ROLE = ALL);
    
    GO
    ```
    
3.  Por último, realice la misma secuencia en LinAGN3:
    
    ```SQL
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<StrongPassword>';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    WITH SUBJECT = 'LinAGN3 AG Certificate';
    
    GO
    
    BACKUP CERTIFICATE LinAGN3_Cert
    TO FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
    CREATE ENDPOINT AGEP
    STATE = STARTED
    AS TCP (
        LISTENER_PORT = 5022,
        LISTENER_IP = ALL)
    FOR DATABASE_MIRRORING (
        AUTHENTICATION = CERTIFICATE LinAGN3_Cert,
        ROLE = ALL);
    
    GO
    ```
    
4.  Con `scp` u otra utilidad, copie las copias de seguridad del certificado en todos los nodos que vayan a formar parte del grupo de disponibilidad.
    
    En este ejemplo:
    
    - Copie LinAGN1_Cert.cer en LinAGN2 y LinAGN3.
    - Copie LinAGN2_Cert.cer en LinAGN1 y LinAGN3.
    - Copie LinAGN3_Cert.cer en LinAGN1 y LinAGN2.
    
5.  Cambie la propiedad y el grupo asociados a los archivos de certificado copiados en `mssql`.
    
    ```bash
    sudo chown mssql:mssql <CertFileName>
    ```
    
6.  Cree los inicios de sesión de nivel de instancia y los usuarios asociados a LinAGN2 y LinAGN3 en LinAGN1.
    
    ```SQL
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
7.  Restaure LinAGN2_Cert y LinAGN3_Cert en LinAGN1. El disponer de los certificados de las otras réplicas es un aspecto importante de la comunicación y la seguridad de los grupos de disponibilidad.
    
    ```SQL
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    
8.  Grant the logins associated with LinAG2 and LinAGN3 permission to connect to the endpoint on LinAGN1.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
9.  Cree los inicios de sesión de nivel de instancia y los usuarios asociados a LinAGN1 y LinAGN3 en LinAGN2.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN3_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN3_User FOR LOGIN LinAGN3_Login;
    
    GO
    ```
    
10. Restaure LinAGN1_Cert y LinAGN3_Cert en LinAGN2.
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN3_Cert
    AUTHORIZATION LinAGN3_User
    FROM FILE = '/var/opt/mssql/data/LinAGN3_Cert.cer';
    
    GO
    ```
    
11. Conceda permiso para conectarse al punto de conexión de LinAGN2 a los inicios de sesión asociados a LinAG1 y LinAGN3.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN3_Login;
    
    GO
    ```
    
12. Cree los inicios de sesión de nivel de instancia y los usuarios asociados a LinAGN1 y LinAGN2 en LinAGN3.
    
    ```SQL
    CREATE LOGIN LinAGN1_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN1_User FOR LOGIN LinAGN1_Login;
    
    GO
    
    CREATE LOGIN LinAGN2_Login WITH PASSWORD = '<StrongPassword>';
    CREATE USER LinAGN2_User FOR LOGIN LinAGN2_Login;
    
    GO
    ```
    
13. Restaure LinAGN1_Cert y LinAGN2_Cert en LinAGN3. 
    
    ```SQL
    CREATE CERTIFICATE LinAGN1_Cert
    AUTHORIZATION LinAGN1_User
    FROM FILE = '/var/opt/mssql/data/LinAGN1_Cert.cer';
    
    GO
    
    CREATE CERTIFICATE LinAGN2_Cert
    AUTHORIZATION LinAGN2_User
    FROM FILE = '/var/opt/mssql/data/LinAGN2_Cert.cer';
    
    GO
    ```
    
14. Conceda permiso para conectarse al punto de conexión de LinAGN3 a los inicios de sesión asociados a LinAG1 y LinAGN2.
    
    ```SQL
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN1_Login;
    
    GO
    
    GRANT CONNECT ON ENDPOINT::AGEP TO LinAGN2_Login;
    
    GO
    ```

## <a name="create-the-availability-group"></a>Crear el grupo de disponibilidad

En esta sección se explica cómo usar [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL para crear el grupo de disponibilidad para [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

### <a name="use-ssmanstudiofull-md"></a>Use [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]

En esta sección se muestra cómo crear un grupo de disponibilidad con un tipo de clúster Externo mediante SSMS con el Asistente para nuevo grupo de disponibilidad.

1.  En SSMS, expanda **Alta disponibilidad de Always On**, haga clic con el botón derecho en **Grupos de disponibilidad** y seleccione **Asistente para nuevo grupo de disponibilidad**.

2.  En el cuadro de diálogo Introducción, haga clic en **Siguiente**.

3.  En el cuadro de diálogo Especificar opciones de grupo de disponibilidad, escriba un nombre para el grupo de disponibilidad y seleccione un tipo de clúster EXTERNO o NINGUNO en la lista desplegable. Debe usarse Externo cuando se vaya a implementar Pacemaker. Ninguno es para escenarios especializados, como el escalado horizontal de lectura. La selección de la opción para la detección del estado del nivel de la base de datos es opcional. Para obtener más información sobre esta opción, vea [Opción de conmutación por error de detección del estado del nivel de la base de datos de un grupo de disponibilidad](../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md). Haga clic en **Next**.

    ![Creación de Grupo de disponibilidad 03](./media/sql-server-linux-create-availability-group/image3.png)

4.  En el cuadro de diálogo Seleccionar bases de datos, seleccione las bases de datos que van a participar en el grupo de disponibilidad. Cada base de datos debe tener una copia de seguridad completa para que se pueda agregar a un grupo de disponibilidad. Haga clic en **Next**.

5.  En el cuadro de diálogo Especificar réplicas, haga clic en **Agregar réplica**.

6.  En el cuadro de diálogo Conectar al servidor, escriba el nombre de la instancia de Linux de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)]que va a ser la réplica secundaria y las credenciales para conectarse. Haga clic en **Conectar**.

7.  Repita los dos pasos anteriores para la instancia que vaya a contener una réplica de solo configuración u otra réplica secundaria.

8.  Las tres instancias ahora deben aparecer en el cuadro de diálogo Especificar réplicas. Si usa un tipo de clúster Externo, en la réplica secundaria que va a ser una secundaria verdadera, asegúrese de que el modo de disponibilidad coincida con el de la réplica principal y de que el modo de conmutación por error esté establecido en Externo. En el caso de la réplica de solo configuración, seleccione un modo de disponibilidad de solo configuración.

    En el ejemplo siguiente se muestra un grupo de disponibilidad con dos réplicas, un tipo de clúster Externo y una réplica de solo configuración.

    ![Creación de grupo de disponibilidad 04](./media/sql-server-linux-create-availability-group/image4.png)

    En el ejemplo siguiente se muestra un grupo de disponibilidad con dos réplicas, un tipo de clúster Ninguno y una réplica de solo configuración.

    ![Creación de grupo de disponibilidad 05](./media/sql-server-linux-create-availability-group/image5.png)

9.  Si quiere modificar las preferencias de copia de seguridad, haga clic en la pestaña Preferencias de copia de seguridad. Para obtener más información sobre las preferencias de copia de seguridad con grupos de disponibilidad, vea [Configuración de copias de seguridad en las réplicas secundarias de un grupo de disponibilidad](../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md).

10. Si usa secundarias legibles o crea un grupo de disponibilidad con un tipo de clúster Ninguno para escalado de lectura, puede crear un cliente de escucha si selecciona la pestaña Agente de escucha. También se puede agregar un cliente de escucha más adelante. Para crear un cliente de escucha, seleccione la opción **Crear un agente de escucha del grupo de disponibilidad** y escriba un nombre, un puerto TCP/IP y si va a usar una dirección IP DHCP estática o asignada automáticamente. Recuerde que en un grupo de disponibilidad con un tipo de clúster Ninguno, la dirección IP debe ser estática y establecerse en la dirección IP del principal.

    ![Creación de grupo de disponibilidad 06](./media/sql-server-linux-create-availability-group/image6.png)

11. Si se crea un cliente de escucha para escenarios legibles, SSMS 17.3 o posterior permite la creación del enrutamiento de solo lectura en el asistente. También se puede agregar más adelante mediante SSMS o Transact-SQL. Para agregar enrutamiento de solo lectura ahora:

    a.  Seleccione la pestaña Enrutamiento de solo lectura.

    b.  Escriba las direcciones URL de las réplicas de solo lectura. Estas direcciones URL son similares a los puntos de conexión, salvo que usan el puerto de la instancia, no el punto de conexión.

    c.  Seleccione cada dirección URL y, en la parte inferior, seleccione las réplicas legibles. Para realizar una selección múltiple, mantenga presionada la tecla MAYÚS o haga clic y arrastre.

12. Haga clic en **Next**.

13. Elija cómo se van a inicializar las réplicas secundarias. El valor predeterminado es usar [propagación automática](../database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group.md), que requiere la misma ruta de acceso en todos los servidores que participan en el grupo de disponibilidad. También puede hacer que el asistente realice una copia de seguridad, copie y restaure (la segunda opción); la una si ha realizado una copia de seguridad de la base de datos, la ha copiado y restaurado manualmente en las réplicas (tercera opción); o agregue la base de datos más adelante (última opción). Al igual que con los certificados, si realiza copias de seguridad de forma manual y las copia, se deben establecer los permisos de los archivos de copia de seguridad en las otras réplicas. Haga clic en **Next**.

14. En el cuadro de diálogo Validación, si todo no se devuelve como Correcto, investigue. Algunas advertencias son aceptables y no son graves, por ejemplo si no se crea un cliente de escucha. Haga clic en **Next**.

15. En el cuadro de diálogo Resumen, haga clic en **Finalizar**. El proceso de creación del grupo de disponibilidad ya comienza.

16. Una vez finalizada la creación del grupo de disponibilidad, haga clic en **Cerrar** en los resultados. Ahora puede ver el grupo de disponibilidad en las réplicas en las vistas de administración dinámica y en la carpeta Alta disponibilidad de Always On de SSMS.

### <a name="use-transact-sql"></a>Uso de Transact-SQL

En esta sección se muestran ejemplos de creación de un grupo de disponibilidad mediante Transact-SQL. El cliente de escucha y el enrutamiento de solo lectura se pueden configurar una vez creado el grupo de disponibilidad. El propio grupo de disponibilidad se puede modificar con `ALTER AVAILABILITY GROUP`, pero el cambio de tipo de clúster no se puede realizar en [!INCLUDE[sssql17-md](../includes/sssql17-md.md)]. Si no pretende crear un grupo de disponibilidad con un tipo de clúster Externo, debe eliminarlo y volver a crearlo con un tipo de clúster Ninguno. Puede encontrar más información y otras opciones en los vínculos siguientes:

-   [CREATE AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/create-availability-group-transact-sql.md)
-   [ALTER AVAILABILITY GROUP (Transact-SQL)](../t-sql/statements/alter-availability-group-transact-sql.md)
-   [Configuración del enrutamiento de solo lectura para un grupo de disponibilidad Always On (SQL Server)](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)
-   [Configuración de un agente de escucha para un grupo de disponibilidad Always On (SQL Server)](../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)

#### <a name="example-one---two-replicas-with-a-configuration-only-replica-external-cluster-type"></a>Ejemplo uno: dos réplicas con una réplica de solo configuración (tipo de clúster Externo)

En este ejemplo se muestra cómo crear un grupo de disponibilidad de dos réplicas que use una réplica de solo configuración.

1.  Ejecute en el nodo que vaya a ser la réplica principal que contiene la copia de lectura o escritura completa de las bases de datos. En este ejemplo se usa propagación automática.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1' WITH (
       ENDPOINT_URL = N' TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       SEEDING_MODE = AUTOMATIC),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       AVAILABILITY_MODE = CONFIGURATION_ONLY);
       
    GO
    ```
    
2.  En una ventana de consulta conectada a la otra réplica, ejecute lo siguiente para unir la réplica al grupo de disponibilidad e iniciar el proceso de propagación desde la réplica principal a la secundaria.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3. En una ventana de consulta conectada a la réplica de solo configuración, únala al grupo de disponibilidad.
    
   ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
   ```

#### <a name="example-two---three-replicas-with-read-only-routing-external-cluster-type"></a>Ejemplo dos: tres réplicas con enrutamiento de solo lectura (tipo de clúster Externo)

En este ejemplo se muestran tres réplicas completas y cómo se puede configurar el enrutamiento de solo lectura como parte de la creación inicial del grupo de disponibilidad.

1.  Ejecute en el nodo que vaya a ser la réplica principal que contiene la copia de lectura o escritura completa de las bases de datos. En este ejemplo se usa propagación automática.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN2.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:1433')),
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN3.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN2.FullyQualified.Name:1433')),
    N'LinAGN3' WITH (
       ENDPOINT_URL = N'TCP://LinAGN3.FullyQualified.Name:5022',
       FAILOVER_MODE = EXTERNAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN3.FullyQualified.Name:1433'))
    LISTENER '<ListenerName>' (WITH IP = ('<IPAddress>', '<SubnetMask>'), Port = 1433);
    
    GO
    ```
    
    Algunos aspectos que se deben tener en cuenta sobre esta configuración:
    
    - *AGName* es el nombre del grupo de disponibilidad.
    - *DBName* es el nombre de la base de datos que se va a usar con el grupo de disponibilidad. También puede ser una lista de nombres separados por comas.
    - *ListenerName* es un nombre distinto a cualquiera de los servidores o nodos subyacentes. Se registra en DNS junto con *IPAddress*.
    - *IPAddress* es una dirección IP que está asociada a *ListenerName*. También es única y no es lo mismo que ninguno de los servidores o nodos. Las aplicaciones y los usuarios finales usan *ListenerName* o *IPAddress* para conectarse al grupo de disponibilidad.
    - *SubnetMask* es la máscara de subred de *IPAddress*; por ejemplo, 255.255.255.0.

2.  En una ventana de consulta conectada a la otra réplica, ejecute lo siguiente para unir la réplica al grupo de disponibilidad e iniciar el proceso de propagación desde la réplica principal a la secundaria.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```
    
3.  Repita el paso 2 para la tercera réplica.

#### <a name="example-three---two-replicas-with-read-only-routing-none-cluster-type"></a>Ejemplo tres: dos réplicas con enrutamiento de solo lectura (tipo de clúster Ninguno)

En este ejemplo se muestra la creación de una configuración de dos réplicas con un tipo de clúster Ninguno. Se usa para el escenario de escalado de lectura donde no se espera conmutación por error. Crea el cliente de escucha que es realmente la réplica principal, así como el enrutamiento de solo lectura, con la funcionalidad round robin.

1.  Ejecute en el nodo que vaya a ser la réplica principal que contiene la copia de lectura o escritura completa de las bases de datos. En este ejemplo se usa propagación automática.

    ```SQL
    CREATE AVAILABILITY GROUP [<AGName>]
    WITH (CLUSTER_TYPE = NONE)
    FOR DATABASE <DBName>
    REPLICA ON N'LinAGN1'
    WITH (
       ENDPOINT_URL = N'TCP://LinAGN1.FullyQualified.Name: <PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name'.'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL = N'TCP://LinAGN1.FullyQualified.Name:<PortOfInstance>'));
    N'LinAGN2' WITH (
       ENDPOINT_URL = N'TCP://LinAGN2.FullyQualified.Name:<PortOfEndpoint>',
       FAILOVER_MODE = MANUAL,
       SEEDING_MODE = AUTOMATIC,
       AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
       PRIMARY_ROLE (ALLOW_CONNECTIONS = READ_WRITE, READ_ONLY_ROUTING_LIST = (('LinAGN1.FullyQualified.Name', 'LinAGN2.FullyQualified.Name'))),
       SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL, READ_ONLY_ROUTING_URL =N'TCP://LinAGN2.FullyQualified.Name:<PortOfInstance>'));
    LISTENER '<ListenerName>' (WITH IP = ('<PrimaryReplicaIPAddress>', '<SubnetMask>'), Port = <PortOfListener>);
    
    GO
    ```
    
    Where
    - *AGName* es el nombre del grupo de disponibilidad.
    - *DBName* es el nombre de la base de datos que se va a usar con el grupo de disponibilidad. También puede ser una lista de nombres separados por comas.
    - *PortOfEndpoint* es el número de puerto que usa el punto de conexión creado.
    - *PortOfInstance* es el número de puerto que usa la instancia de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].
    - *ListenerName* es un nombre distinto a cualquiera de las réplicas subyacentes, pero no se usa realmente.
    - *PrimaryReplicaIPAddress* es la dirección IP de la réplica principal.
    - *SubnetMask* es la máscara de subred de *IPAddress*. Por ejemplo, 255.255.255.0.
    
2.  Una la réplica secundaria al grupo de disponibilidad e inicie la propagación automática.
    
    ```SQL
    ALTER AVAILABILITY GROUP [<AGName>] JOIN WITH (CLUSTER_TYPE = NONE);
    
    GO
    
    ALTER AVAILABILITY GROUP [<AGName>] GRANT CREATE ANY DATABASE;
    
    GO
    ```

## <a name="create-the-ssnoversion-md-login-and-permissions-for-pacemaker"></a>Crear el inicio de sesión de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] y los permisos para Pacemaker

Un clúster de alta disponibilidad de Pacemaker subyacente a [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] en Linux necesita acceso a la instancia de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], así como permisos en el propio grupo de disponibilidad. Estos pasos crean el inicio de sesión y los permisos asociados, junto con un archivo que indica a Pacemaker cómo iniciar sesión en [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)].

1.  En una ventana de consulta conectada a la réplica principal, ejecute lo siguiente:

    ```SQL
    CREATE LOGIN PMLogin WITH PASSWORD ='<StrongPassword>';
    
    GO
    
    GRANT VIEW SERVER STATE TO PMLogin;
    
    GO
    
    GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::<AGThatWasCreated> TO PMLogin;
    
    GO
    ```
    
2.  En Nodo 1, escriba el comando 
    ```bash
    sudo emacs /var/opt/mssql/secrets/passwd
    ```
    
    Así se abre el editor Emacs.
    
3.  Escriba las dos líneas siguientes en el editor:

    ```
    PMLogin
    <StrongPassword>
    ```
    
4.  Mantenga presionada la tecla CTRL y presione X; luego C, para salir y guardar el archivo.

5.  Execute 
    ```bash
    sudo chmod 400 /var/opt/mssql/secrets/passwd
    ```
    
    para bloquear el archivo.

6.  Repita los pasos 1-5 en los otros servidores que vayan a actuar como réplicas.

## <a name="create-the-availability-group-resources-in-the-pacemaker-cluster-external-only"></a>Crear recursos de grupo de disponibilidad en el clúster de Pacemaker (solo Externo)

Después de crear un grupo de disponibilidad en [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)], se deben crear los recursos correspondientes en Pacemaker si se ha especificado un tipo de clúster Externo. Hay dos recursos asociados a un grupo de disponibilidad: el propio grupo de disponibilidad y una dirección IP. La configuración del recurso de dirección IP es opcional si no se usa la funcionalidad del cliente de escucha, pero se recomienda.

El recurso de grupo de disponibilidad que se crea es un tipo especial de recurso denominado clon. El recurso de grupo de disponibilidad básicamente tiene copias en cada nodo y hay un recurso que controla denominado maestro. El maestro está asociado al servidor que hospeda la réplica principal. El resto de recursos hospedan réplicas secundarias (normales o de solo configuración) y se pueden promover a maestras en una conmutación por error.

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

1.  Cree el recurso de grupo de disponibilidad con la siguiente sintaxis:

    **Red Hat Enterprise Linux (RHEL) y Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failure-timeout=30s --master meta notify=true
    ```

    >[!NOTE]
    >En RHEL 7.4, puede aparecer una advertencia con el uso de --master. Para evitarlo, use `sudo pcs resource create <NameForAGResource> ocf:mssql:ag ag_name=<AGName> meta failover-timeout=30s master notify=true`
   
    **SUSE Linux Enterprise Server (SLES)**
    
    ```bash
    primitive <NameForAGResource> \
    ocf:mssql:ag \
    params ag_name="<AGName>" \
    meta failure-timeout=60s \
    op start timeout=60s \
    op stop timeout=60s \
    op promote timeout=60s \
    op demote timeout=10s \
    op monitor timeout=60s interval=10s \
    op monitor timeout=60s interval=11s role="Master" \
    op monitor timeout=60s interval=12s role="Slave" \
    op notify timeout=60s
    ms ms-ag_cluster <NameForAGResource> \
    meta master-max="1" master-node-max="1" clone-max="3" \
    clone-node-max="1" notify="true" \
    commit
    ```
    
    donde *NameForAGResource* es el nombre único asignado a este recurso de clúster para el grupo de disponibilidad y *AGName* es el nombre del grupo de disponibilidad que se ha creado.
 
2.  Cree el recurso de dirección IP para el grupo de disponibilidad que se va a asociar a la funcionalidad del cliente de escucha.

    **RHEL y Ubuntu**
    
    ```bash
    sudo pcs resource create <NameForIPResource> ocf:heartbeat:IPaddr2 ip=<IPAddress> cidr_netmask=<Netmask>
    ```

    **SLES**
    
    ```bash
    crm configure \
    primitive <NameForIPResource> \
       ocf:heartbeat:IPaddr2 \
       params ip=<IPAddress> \
          cidr_netmask=<Netmask>
    ```
    
    donde *NameForIPResource* es el nombre único del recurso de IP e *IPAddress* es la dirección IP estática asignada al recurso. En SLES, también debe proporcionar la máscara de red. Por ejemplo, 255.255.255.0 tendría un valor de 24 para *Netmask*.
    
3.  Para asegurarse de que la dirección IP y el recurso de grupo de disponibilidad se están ejecutando en el mismo nodo, debe configurarse una restricción de ubicación.

    **RHEL y Ubuntu**
    
    ```bash
    sudo pcs constraint colocation add <NameForIPResource> <NameForAGResource>-master INFINITY with-rsc-role=Master
    ```

    **SLES**
    
    ```bash
    crm configure <NameForConstraint> inf: \
    <NameForIPResource> <NameForAGResource>:Master 
    commit
    ```
    
    donde *NameForIPResource* es el nombre del recurso de IP, *NameForAGResource* es el nombre del recurso de grupo de disponibilidad y, en SLES, *NameForConstraint* es el nombre de la restricción.

4.  Cree una restricción de orden para asegurarse de que el recurso de grupo de disponibilidad está en funcionamiento antes que la dirección IP. Aunque la restricción de ubicación implica una restricción de orden, esto la aplica.

    **RHEL y Ubuntu**
    
    ```bash
    sudo pcs constraint order promote <NameForAGResource>-master then start <NameForIPResource>
    ```
    
    **SLES**
    
    ```bash
    crm configure \
    order <NameForConstraint> inf: <NameForAGResource>:promote <NameForIPResource>:start
    commit
    ```
    
    donde *NameForIPResource* es el nombre del recurso de IP, *NameForAGResource* es el nombre del recurso de grupo de disponibilidad y, en SLES, *NameForConstraint* es el nombre de la restricción.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a crear y configurar un grupo de disponibilidad para [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] en Linux. Ha aprendido a:
> [!div class="checklist"]
> * Habilitar grupos de disponibilidad.
> * Crear puntos de conexión y certificados de grupo de disponibilidad.
> * Usar [!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)] (SSMS) o Transact-SQL para crear un grupo de disponibilidad.
> * Crear el inicio de sesión de [!INCLUDE[ssnoversion-md](../includes/ssnoversion-md.md)] y los permisos para Pacemaker.
> * Crear recursos de grupo de disponibilidad en un clúster de Pacemaker.

Para la mayoría de las tareas de administración de grupos de disponibilidad, incluidas las actualizaciones y la conmutación por error, vea:

> [!div class="nextstepaction"]
> [Manejar un grupo de disponibilidad de alta disponibilidad para SQL Server en Linux](sql-server-linux-availability-group-failover-ha.md)

