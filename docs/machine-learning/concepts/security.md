---
title: Arquitectura de seguridad de extensibilidad
description: En este artículo se describe la arquitectura de seguridad del marco de extensibilidad en SQL Server Machine Learning Services. Esto engloba la seguridad de inicios de sesión y cuentas de usuario, el servicio Launchpad de SQL Server, las cuentas de trabajo, la ejecución de varios scripts y los permisos de archivo.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.custom: contperf-fy21q1, seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 63138dbd9ef2acd45dfc508bac20bd6694d49d90
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878893"
---
# <a name="security-architecture-for-the-extensibility-framework-in-sql-server-machine-learning-services"></a>Arquitectura de seguridad del marco de extensibilidad en SQL Server Machine Learning Services

[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se describe la arquitectura de seguridad que se usa para integrar el motor de base de datos de SQL Server y los componentes relacionados con el marco de extensibilidad en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md). Examina los elementos protegibles, los servicios, la identidad del proceso y los permisos. Los puntos clave que se describen en este artículo incluyen la finalidad de launchpad, SQLRUserGroup y las cuentas de trabajo, el aislamiento de procesos de scripts externos, y cómo se asignan las identidades de usuario a las cuentas de trabajo.

Para obtener más información sobre los conceptos clave y los componentes de extensibilidad en SQL Server, vea [Arquitectura de extensibilidad en SQL Server Machine Learning Services](extensibility-framework.md).

## <a name="securables-for-external-script"></a>Elementos protegibles para scripts externos

Los scripts externos se envían como parámetros de entrada a un [procedimiento almacenado del sistema](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) creado para este propósito, o bien se ajustan en un procedimiento almacenado que deberá definir personalmente. Los scripts pueden estar escritos en R, Python o lenguajes externos como Java o .NET. También puede tener en una tabla de base de datos modelos previamente entrenados y almacenados en un formato binario, a los que puede llamar en una función [PREDICT](../../t-sql/queries/predict-transact-sql.md) de T-SQL.

Dado que el script se proporciona a través de objetos de esquema de base de datos, procedimientos almacenados y tablas existentes, no hay ningún [elemento protegible](../../relational-databases/security/securables.md) nuevo para SQL Server Machine Learning Services.

Independientemente de la forma en que use el script o de lo que lo conforme, los objetos de base de datos se crearán y, probablemente, se guardarán, pero no se introducirá ningún nuevo tipo de objeto para el script de almacenamiento. Como resultado, la capacidad de consumir, crear y guardar objetos de base de datos depende en gran medida de los permisos de base de datos ya definidos para los usuarios.

<a name="permissions"></a>

## <a name="permissions"></a>Permisos

El modelo de seguridad de datos de SQL Server de inicios de sesión y roles de base de datos se extiende al script externo. Se requiere un inicio de sesión de SQL Server o una cuenta de usuario de Windows para ejecutar scripts externos que utilicen datos de SQL Server o que se ejecuten con SQL Server como contexto de proceso. Los usuarios de bases de datos que tienen permisos para ejecutar una consulta pueden acceder a los mismos datos desde el script externo.

El inicio de sesión o la cuenta de usuario identifica la *entidad de seguridad*, que podría necesitar varios niveles de acceso, dependiendo de los requisitos de los scripts externos:

+ Permiso para acceder a la base de datos donde se habilitan los scripts externos.
+ Permisos para leer datos de objetos protegidos como tablas.
+ La capacidad de escribir nuevos datos en una tabla, como un modelo o los resultados de la puntuación.
+ La capacidad de crear objetos, como tablas, procedimientos almacenados que usan el script externo o funciones personalizadas que usan el trabajo de script externo.
+ El derecho a instalar nuevos paquetes en el equipo de SQL Server o a usar paquetes proporcionados a un grupo de usuarios.

Cada persona que ejecuta un script externo mediante SQL Server como contexto de ejecución debe estar asignada a un usuario de la base de datos. En lugar de establecer individualmente los permisos de usuario de la base de datos, puede crear roles para administrar conjuntos de permisos y asignar usuarios a esos roles, en lugar de establecer individualmente los permisos de usuario.

Para obtener más información, vea [Concesión de permiso a los usuarios para SQL Server Machine Learning Services](../../machine-learning/security/user-permission.md).

## <a name="permissions-when-using-an-external-client-tool"></a>Permisos al usar una herramienta de cliente externa

Los usuarios que utilizan el script en una herramienta de cliente externa deben tener su cuenta o inicio de sesión asignada a un usuario de la base de datos si tienen que ejecutar un script externo en la base de datos, o bien acceder a datos y objetos de base de datos. Los mismos permisos son necesarios si el script externo se envía desde un cliente de ciencia de datos remoto o se ejecuta mediante un procedimiento almacenado de T-SQL.

Por ejemplo, supongamos que ha creado un script externo que se ejecuta en el equipo local y desea ejecutar ese script en SQL Server. Debe asegurarse de que se cumplan las condiciones siguientes:

+ La base de datos permite las conexiones remotas.
+ El inicio de sesión de SQL o la cuenta de Windows que ha usado para acceder a la base de datos se ha agregado al SQL Server en el nivel de instancia.
+ El inicio de sesión de SQL o el usuario de Windows deben tener permiso para ejecutar scripts externos. Por lo general, este permiso solo lo puede agregar un administrador de bases de datos.
+ El inicio de sesión de SQL o el usuario de Windows deben agregarse como usuario con los permisos adecuados en cada base de datos en la que el script externo realice cualquiera de estas operaciones:
  + Recuperar datos.
  + Escribir o actualizar datos.
  + Crear objetos como tablas o procedimientos almacenados.

Una vez que se ha aprovisionado el inicio de sesión o la cuenta de usuario de Windows y se han dado los permisos necesarios, puede ejecutar un script externo en SQL Server usando un objeto de origen de datos en R o la biblioteca **revoscalepy** en Python, o llamando a un procedimiento almacenado que contenga el script externo.

Siempre que se inicie un script externo desde SQL Server, la seguridad del motor de base de datos obtiene el contexto de seguridad del usuario que ha iniciado el trabajo y administra las asignaciones del usuario o del inicio de sesión a objetos protegibles.

Por lo tanto, todos los scripts externos que se inician desde un cliente remoto deben especificar la información de usuario o inicio de sesión como parte de la cadena de conexión.

<a name="launchpad"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="services-used-in-external-processing-launchpad"></a>Servicios usados en el procesamiento externo (launchpad)

El marco de extensibilidad agrega un nuevo servicio NT a la [lista de servicios](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) en una instalación de SQL Server: [**SQL Server Launchpad (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

El motor de base de datos usa el servicio **launchpad** de SQL Server para crear instancias de una sesión de scripts externos como un proceso independiente. 
El proceso se ejecuta con una cuenta con pocos privilegios diferente a SQL Server, Launchpad y la identidad del usuario en la que se ha ejecutado el procedimiento almacenado o la consulta de host. La ejecución de un script en un proceso independiente, en una cuenta con pocos privilegios, es la base del modelo de aislamiento y seguridad para los scripts externos en SQL Server.

SQL Server también mantiene una asignación de la identidad del usuario que realiza la llamada a la cuenta de trabajo sin privilegios que se usa para iniciar el proceso satélite. En algunos escenarios, en los que el script o el código devuelven la llamada a SQL Server para los datos y las operaciones, SQL Server puede administrar la transferencia de identidad sin problemas. El script que contiene instrucciones SELECT o funciones de llamada y otros objetos de programación normalmente se realizará correctamente si el usuario que realiza la llamada tiene permisos suficientes.

> [!NOTE]
> De forma predeterminada, [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] está configurado para ejecutarse con **NT Service\MSSQLLaunchpad**, que se aprovisiona con todos los permisos necesarios para ejecutar scripts externos. Para obtener más información sobre las opciones configurables, vea [Configuración del servicio launchpad de SQL Server](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="services-used-in-external-processing-launchpad"></a>Servicios usados en el procesamiento externo (launchpad)

El marco de extensibilidad agrega un nuevo servicio NT a la [lista de servicios](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Service_Details) en una instalación de SQL Server: [**SQL Server Launchpad (MSSSQLSERVER)**](extensibility-framework.md#launchpad).

El motor de base de datos usa el servicio **launchpad** de SQL Server para crear instancias de una sesión de scripts externos como un proceso independiente. 
El proceso se ejecuta con la identidad de usuario de launchpad, pero con la restricción agregada de estar contenida dentro de un objeto AppContainer. La ejecución de un script en un proceso independiente, bajo AppContainer, es la base del modelo de aislamiento y seguridad para los script externos en SQL Server.

SQL Server también mantiene una asignación de la identidad del usuario que realiza la llamada a la cuenta de trabajo sin privilegios que se usa para iniciar el proceso satélite. En algunos escenarios, en los que el script o el código devuelven la llamada a SQL Server para los datos y las operaciones, SQL Server puede administrar la transferencia de identidad sin problemas. El script que contiene instrucciones SELECT o funciones de llamada y otros objetos de programación normalmente se realizará correctamente si el usuario que realiza la llamada tiene permisos suficientes.

> [!NOTE]
> De forma predeterminada, [!INCLUDE[rsql_launchpad_md](../../includes/rsql-launchpad-md.md)] está configurado para ejecutarse con **NT Service\MSSQLLaunchpad**, que se aprovisiona con todos los permisos necesarios para ejecutar scripts externos. Para obtener más información sobre las opciones configurables, vea [Configuración del servicio launchpad de SQL Server](../security/sql-server-launchpad-service-account.md).

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="services-used-in-external-processing"></a>Servicios usados en el procesamiento externo

El marco de extensibilidad agrega un nuevo demonio en una instalación de SQL Server: [mssql-launchpadd](extensibility-framework.md#launchpad). mssql-launchpadd se ejecuta bajo la cuenta sin privilegios mssql_launchpadd que se crea al instalar el paquete mssql-server-extensibility.

Solo se admite una instancia del motor de base de datos, y hay un servicio launchpad enlazado a la instancia. Cuando se ejecuta un script, el servicio launchpad inicia un proceso launchpad independiente con la cuenta de usuario sin privilegios mssql_satellite en su propio PID nuevo, IPC, montaje y espacio de nombres de red. Cada proceso satélite hereda la cuenta de usuario mssql_satellite de Launchpad y la usa durante la ejecución del script.

Para obtener más información, vea [Arquitectura de extensibilidad en SQL Server Machine Learning Services](extensibility-framework.md).

::: moniker-end

<a name="sqlrusergroup"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="identities-used-in-processing-sqlrusergroup"></a>Identidades usadas durante el procesamiento (SQLRUserGroup)

**SQLRUserGroup** (grupo de usuarios restringidos de SQL) está creado por el programa de instalación SQL Server y contiene un grupo de cuentas de usuario de Windows locales con pocos privilegios. Cuando se necesita un proceso externo, launchpad toma una cuenta de trabajo disponible y la usa para ejecutar un proceso. En concreto, launchpad activa una cuenta de trabajo disponible, la asigna a la identidad del usuario que realiza la llamada y ejecuta el script en la cuenta de trabajo.

+ **SQLRUserGroup** está vinculado a una instancia específica. Para cada instancia en la que se haya habilitado el aprendizaje automático, se requiere un grupo de cuentas profesionales independiente de cuentas. Las cuentas no se pueden compartir entre instancias.

+ El tamaño del grupo de cuentas de usuario es estático y el valor predeterminado es 20, que admite 20 sesiones simultáneas. El número de sesiones del runtime externas que se puede iniciar de forma simultánea está limitado por el tamaño de este grupo de cuentas de usuario. 

+ Los nombres de las cuentas profesionales del bloque presentan el formato SQLInstanceName *nn*. Por ejemplo, en una instancia predeterminada, **SQLRUserGroup** contiene las cuentas denominadas MSSQLSERVER01, MSSQLSERVER02 y así sucesivamente hasta MSSQLSERVER20.

Las tareas en paralelo no consumen cuentas adicionales. Por ejemplo, si un usuario ejecuta una tarea de puntuación que utiliza el procesamiento en paralelo, se reutiliza la misma cuenta de trabajo para todos los subprocesos. Si piensa hacer un uso intensivo del aprendizaje automático, puede aumentar el número de cuentas que se usan para ejecutar scripts externos. Para obtener más información, vea [Escalar la ejecución simultánea de scripts externos en SQL Server Machine Learning Services](../../machine-learning/administration/scale-concurrent-execution-external-scripts.md).

### <a name="permissions-granted-to-sqlrusergroup"></a>Permisos concedidos a SQLRUserGroup

De forma predeterminada, los miembros de **SQLRUserGroup** tienen permisos de lectura y ejecución en los archivos de los directorios de SQL Server **Binn**, **R_SERVICES** y **PYTHON_SERVICES**. Esto incluye el acceso a los archivos ejecutables, las bibliotecas y los conjuntos de archivos integrados en las distribuciones de R y Python instaladas con SQL Server. 

Para proteger los recursos confidenciales en SQL Server, puede optar por definir una lista de control de acceso (ACL) que deniegue el acceso a **SQLRUserGroup**. Asimismo, también puede conceder permisos a los recursos de datos locales que existen en el equipo host, además del mismo SQL Server. 

De forma predeterminada, **SQLRUserGroup** no tiene ningún permiso ni inicio de sesión de base de datos para ningún dato. En determinadas circunstancias, es posible que le interese crear un inicio de sesión para permitir las conexiones de bucle invertido, especialmente cuando una identidad de Windows de confianza se corresponde con el usuario que realiza la llamada. Esta capacidad se denomina [*autenticación implícita*](#implied-authentication). Para obtener más información, vea [Agregar SQLRUserGroup como usuario de base de datos](../../machine-learning/security/create-a-login-for-sqlrusergroup.md).

## <a name="identity-mapping"></a>Asignación de identidades

Cuando se inicia una sesión, launchpad asigna la identidad del usuario que realiza la llamada a una cuenta de trabajo. Solo se puede asignar un usuario externo de Windows o un inicio de sesión de SQL válido a una cuenta profesional mientras dure el procedimiento almacenado de SQL que ejecuta el script externo. Las consultas en paralelo desde el mismo inicio de sesión se asignan a la misma cuenta de trabajo de usuario.

Durante la ejecución, launchpad crea carpetas temporales para almacenar datos de la sesión y los elimina cuando finaliza la sesión. Los directorios tienen acceso restringido. En R, RLauncher realiza esta tarea. En Python, PythonLauncher realiza esta tarea. Cada cuenta profesional individual está restringida a su propia carpeta y no puede acceder a los archivos contenidos en carpetas situadas por encima de su propio nivel. Sin embargo, las cuentas profesionales pueden leer, escribir o eliminar elementos secundarios en la carpeta de trabajo de la sesión que se ha creado. Si es administrador del equipo, puede ver los directorios creados para cada proceso. Cada directorio se identifica mediante su GUID de sesión.

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="appcontainer-isolation"></a>Aislamiento de AppContainer

El aislamiento se consigue a través de instancias de [AppContainer](/windows/desktop/secauthz/appcontainer-isolation). En tiempo de ejecución, cuando se detecta un script externo en un procedimiento almacenado o una consulta, SQL Server llama a launchpad con una solicitud para un iniciador específico de la extensión. Launchpad invoca el entorno de runtime adecuado en un proceso bajo su identidad y crea una instancia de AppContainer para que lo contenga. Este cambio es beneficioso porque ya no se requiere la administración de cuentas y contraseñas locales. Además, en las instalaciones en las que las cuentas de usuario locales están prohibidas, la eliminación de la dependencia de la cuenta de usuario local comporta la posibilidad de usar esta característica.

Según la implementación de SQL Server, los contenedores AppContainer son un mecanismo interno. Aunque no verá ninguna evidencia física de los contenedores AppContainer en el monitor de procesos, podrá encontrarlos en las reglas de firewall de salida creadas por el programa de instalación para evitar que los procesos realicen llamadas de red. Para obtener más información, vea [Configuración de firewall para SQL Server Machine Learning Services](../../machine-learning/security/firewall-configuration.md).

## <a name="identity-mapping"></a>Asignación de identidades

Cuando se inicia una sesión, launchpad asigna la identidad del usuario que realiza la llamada a un objeto **AppContainer**.

> [!Note]
> En SQL Server 2019 y versiones posteriores, **SQLRUserGroup** solo tiene un miembro que ahora es la única cuenta de servicio de SQL Server Launchpad, en lugar de varias cuentas de trabajo.

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="identity-mapping"></a>Asignación de identidades

El demonio de **Launchpadd** (con doble "D": [mssql-launchpadd](extensibility-framework.md#launchpad)) asigna la identidad del usuario que realiza la llamada a un proceso **launchpad** (con una sola "D") independiente con una carpeta "launchpad GUID" y un certificado satélite. Estas carpetas launchpad GUID se crean en `/var/opt/mssql-extensibility/data/`. El proceso de launchpad usa este certificado para volver a autenticarse en SQL y, después, crea carpetas temporales para cada GUID de sesión en la carpeta launchpad GUID. El proceso satélite (R, Python o ExtHost) puede acceder a la carpeta launchpad GUID, el certificado que contiene y su carpeta GUID de sesión.

El script de SQL siguiente imprime el contenido de las carpetas launchpad.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    ,@script = N'
print("Contents of /var/opt/mssql-extensibility/data :");
print(system("ls -al /var/opt/mssql-extensibility/data"));
print("Contents of Launchpad GUID folder:");
print(system("ls -al /var/opt/mssql-extensibility/data/*"));
print(system("ls -al /var/opt/mssql-extensibility/data/*/*"))
'
    ,@input_data_1 = N'SELECT 1 AS hello'
```

::: moniker-end

<a name="implied-authentication"></a>

::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="implied-authentication-loopback-requests"></a>Autenticación implícita (solicitudes de bucle invertido)

La *autenticación implícita* describe el comportamiento de la solicitud de conexión en la que los procesos externos que funcionan como cuentas de trabajo sin privilegios se presentan como una identidad de usuario de confianza para SQL Server en solicitudes de bucle invertido para datos u operaciones. Como concepto, la autenticación implícita es exclusiva de la autenticación de Windows, en cadenas de conexión de SQL Server que especifican una conexión de confianza, en las solicitudes que se originan en procesos externos como el script de R o Python. A veces también se denomina *bucle invertido*.

Las conexiones de confianza se pueden ejecutar desde el script externo, pero solo con una configuración adicional. En la arquitectura de extensibilidad, los procesos externos se ejecutan en cuentas de trabajo y heredan los permisos del elemento **SQLRUserGroup** principal. Cuando una cadena de conexión especifica `Trusted_Connection=True`, la identidad de la cuenta profesional se presenta en la solicitud de conexión, que SQL Server desconoce de forma predeterminada.

Para que las conexiones de confianza se realicen correctamente, debe crear un inicio de sesión de base de datos para **SQLRUserGroup**. Después de hacerlo, cualquier conexión de confianza de cualquier miembro de **SQLRUserGroup** tiene derechos de inicio de sesión en SQL Server. Para obtener instrucciones paso a paso, vea [Add SQLRUserGroup to a database login](../../machine-learning/security/create-a-login-for-sqlrusergroup.md) (Agregar SQLRUserGroup a un inicio de sesión de base de datos).

Las conexiones de confianza no son la formulación más habitual para una solicitud de conexión. Cuando el script externo especifica una conexión, puede ser más común usar un inicio de sesión de SQL o un nombre de usuario y una contraseña completos si la conexión es a un origen de datos ODBC.

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Cómo funciona la autenticación implícita para sesiones de script externas

En el diagrama siguiente se muestra la interacción de los componentes de SQL Server con el runtime del lenguaje y cómo realiza la autenticación implícita en Windows.

![Autenticación implícita en Windows](../security/media/implied-auth-windows-2017.png)

::: moniker-end

::: moniker range=">=sql-server-ver15"

## <a name="implied-authentication-loopback-requests"></a>Autenticación implícita (solicitudes de bucle invertido)

La *autenticación implícita* describe el comportamiento de la solicitud de conexión en la que los procesos externos que se ejecutan en objetos AppContainer se presentan como una identidad de usuario de confianza para SQL Server en solicitudes de bucle invertido para datos u operaciones. Como concepto, la autenticación implícita ya no es exclusiva de la autenticación de Windows, en cadenas de conexión de SQL Server que especifican una conexión de confianza, en las solicitudes que se originan en procesos externos como el script de R o Python. A veces también se denomina *bucle invertido*.

Mediante la administración de la identidad y las credenciales, AppContainer impide el uso de las credenciales de usuario para acceder a recursos o iniciar sesión en otros entornos. El entorno de AppContainer crea un identificador que usa las identidades combinadas del usuario y la aplicación, por lo que las credenciales son únicas para cada emparejamiento de usuario y aplicación, y la aplicación no puede suplantar al usuario. Para más información, vea [Aislamiento de AppContainer](/windows/win32/secauthz/appcontainer-isolation).

Para obtener más información sobre las conexiones de bucle invertido, vea [Conexión de bucle invertido a SQL Server desde un script de Python o R](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Cómo funciona la autenticación implícita para sesiones de script externas

En el diagrama siguiente se muestra la interacción de los componentes de SQL Server con el runtime del lenguaje y cómo realiza la autenticación implícita en Windows.

![Autenticación implícita en Windows](../security/media/implied-auth-windows.png)

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

## <a name="implied-authentication-loopback-requests"></a>Autenticación implícita (solicitudes de bucle invertido)

La *autenticación implícita* describe el comportamiento de la solicitud de conexión en la que los procesos externos que funcionan como usuarios mssql_satellite sin privilegios en sus propios espacios de nombres se presentan como una identidad de usuario de confianza para SQL Server en solicitudes de bucle invertido para datos u operaciones. A veces también se denomina bucle invertido.

Una conexión de bucle invertido se consigue mediante el certificado satélite de la carpeta de launchpad GUID para volver a autenticarse en SQL Server con el proceso satélite. La identidad del usuario que realiza la llamada se asigna a este certificado y, por tanto, el proceso satélite que se vuelve a conectar a SQL Server con el certificado se puede asignar de nuevo al usuario que realiza la llamada.

Para obtener más información, vea [Conexión de bucle invertido a SQL Server desde un script de Python o R](../connect/loopback-connection.md).

### <a name="how-implied-authentication-works-for-external-script-sessions"></a>Cómo funciona la autenticación implícita para sesiones de script externas

En el diagrama siguiente se muestra la interacción de los componentes de SQL Server con el runtime del lenguaje y cómo realiza la autenticación implícita en Linux.

![Autenticación implícita en Linux](../security/media/implied-auth-linux.png)

::: moniker-end

## <a name="no-support-for-transparent-data-encryption-at-rest"></a>No se admite el cifrado de datos transparente en reposo

El [cifrado de datos transparente (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md) no se admite para los datos enviados al runtime del script externo o que se reciban de este. La razón es que el proceso externo se ejecuta fuera del proceso de SQL Server. Por lo tanto, los datos utilizados por el runtime externo no están protegidos por las características de cifrado del motor de base de datos. Este comportamiento no se diferencia en nada de cualquier otro cliente que se ejecute en el equipo de SQL Server, que lea los datos de la base de datos y haga una copia.

Por este motivo, el TDE **no** se aplica a ningún dato que se use en los scripts externos, a ningún dato que se guarde en el disco ni a ningún resultado intermedio persistente. Sin embargo, se siguen aplicando otros tipos de cifrado, como el cifrado de BitLocker de Windows o cifrado de terceros en el nivel de archivo o carpeta.

En el caso de [Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md), los runtimes externos no tienen acceso a las claves de cifrado. Por lo tanto, no se pueden enviar datos a los scripts.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido los componentes y el modelo de interacción de la arquitectura de seguridad integrada en el [marco de extensibilidad](../../machine-learning/concepts/extensibility-framework.md). Los puntos clave que se describen en este artículo incluyen la finalidad de launchpad, SQLRUserGroup y las cuentas de trabajo, el aislamiento de procesos de scripts externos, y cómo se asignan las identidades de usuario a las cuentas de trabajo. 

Como paso siguiente, revise las instrucciones para [conceder permisos](../../machine-learning/security/user-permission.md). En el caso de los servidores que usan la autenticación de Windows, también debe revisar [Add SQLRUserGroup to a database login](../../machine-learning/security/create-a-login-for-sqlrusergroup.md) (Agregar SQLRUserGroup a un inicio de sesión de base de datos).