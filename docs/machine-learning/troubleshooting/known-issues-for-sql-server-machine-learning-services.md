---
title: Problemas conocidos de Python y R
description: En este artículo se describen las limitaciones o los problemas conocidos de los componentes de Python y R que se proporcionan en SQL Server Machine Learning Services y SQL Server 2016 R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/13/2020
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 51d729bcdb25900af855e7429e953ee44dc1388b
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766304"
---
# <a name="known-issues-in-sql-server-machine-learning-services"></a>Problemas conocidos de SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se describen las limitaciones o los problemas conocidos de los componentes de Python y R que se proporcionan en [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) y [SQL Server 2016 R Services](../r/sql-server-r-services.md).

## <a name="setup-and-configuration-issues"></a>Problemas de instalación y configuración

Para ver una descripción de los procesos relacionados con la instalación y la configuración iniciales, consulte [Instalación de SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md). Contiene información sobre las actualizaciones, la instalación en paralelo y la instalación de nuevos componentes de R o Python.

### <a name="1-inconsistent-results-in-mkl-computations-due-to-missing-environment-variable"></a>1. Resultados incoherentes en los cálculos de MKL debido a la falta de una variable de entorno

**Se aplica a:** Binarios de R_SERVER 9.0, 9.1, 9.2 o 9.3.

R_SERVER usa la biblioteca Intel Math Kernel Library (MKL). En el caso de los cálculos que involucran a MKL, pueden producirse resultados incoherentes si el sistema no tiene una variable de entorno. 

Establezca la variable de entorno `'MKL_CBWR'=AUTO` para garantizar la reproducibilidad numérica condicional en R_SERVER. Para obtener más información, consulte [Introduction to Conditional Numerical Reproducibility (CNR)](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) [Introducción a la reproducibilidad numérica condicional (CNR)].

**Solución alternativa**

1. En el panel de control, haga clic en **Sistema y seguridad** > **Sistema** > **Configuración avanzada del sistema** > **Variables de entorno**.

2. Cree un usuario o una variable del sistema. 

   + Establezca el nombre de la variable en "MKL_CBWR".
   + Establezca "Valor de la variable" en "AUTO".

3. Reinicie R_SERVER. En SQL Server, puede reiniciar el servicio SQL Server Launchpad.

> [!NOTE]
> Si está ejecutando SQL Server 2019 en Linux, edite o cree *.bash_profile* en el directorio principal de usuario y agregue la línea `export MKL_CBWR="AUTO"`. Ejecute este archivo escribiendo `source .bash_profile` en un símbolo del sistema de Bash. Para reiniciar R_SERVER, escriba `Sys.getenv()` en el símbolo del sistema de R.

### <a name="2-r-script-runtime-error-sql-server-2017-cu5-cu7-regression"></a>2. Error de tiempo de ejecución del script de R (regresión de SQL Server 2017 CU5-CU7)

En el caso de SQL Server 2017, en las actualizaciones acumulativas de la 5 a la 7, hay una regresión en el archivo **rlauncher.config** en que la ruta de acceso del archivo de directorio temporal incluye un espacio. Esta regresión se ha corregido en CU8.

El error que verá al ejecutar el script de R incluye los siguientes mensajes:

> *No se puede comunicar con el tiempo de ejecución para el script "R". Compruebe los requisitos del tiempo de ejecución "R".*
>
> Mensaje(s) STDERR del script externo: 
>
> *Error irrecuperable: no se puede crear "R_TempDir"*

**Solución alternativa**

Instale CU8 cuando esté disponible. Como alternativa, puede volver a crear **rlauncher.config** si ejecuta **registerrext** con uninstall/install en un símbolo del sistema con privilegios elevados. 

```cmd
<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /uninstall /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>

<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /install /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>
```

En el ejemplo siguiente se muestran los comandos con la instancia predeterminada "MSSQL14.MSSQLSERVER" instalada en "C:\Archivos de programa\Microsoft SQL Server\":

```cmd
"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /uninstall /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER

"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /install /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER
```

### <a name="3-unable-to-install-sql-server-machine-learning-features-on-a-domain-controller"></a>3. No se pueden instalar las características de aprendizaje automático de SQL Server en un controlador de dominio

Si intenta instalar SQL Server 2016 R Services o SQL Server Machine Learning Services en un controlador de dominio, se producirán los siguientes errores en la instalación:

> *Error durante el proceso de instalación de la característica*
>
> *Cannot find group with identity* (No se encuentra el grupo con la identidad)
>
> *Component error code: 0x80131509* (Código de error del componente: 0x80131509)

El error se produce porque, en un controlador de dominio, el servicio no puede crear las 20 cuentas locales necesarias para ejecutar el aprendizaje automático. Como regla general, no se recomienda instalar SQL Server en un controlador de dominio. Para obtener más información, vea el [boletín de soporte técnico 2032911](https://support.microsoft.com/help/2032911/you-may-encounter-problems-when-installing-sql-server-on-a-domain-cont).

### <a name="4-install-the-latest-service-release-to-ensure-compatibility-with-microsoft-r-client"></a>4. Instalar la versión de servicio más reciente para garantizar la compatibilidad con Microsoft R Client

Si instala la versión más reciente de Microsoft R Client y la usa para ejecutar R en SQL Server en un contexto de proceso remoto, podría aparecer un error como el siguiente:

> *You are running version 9.x.x of Microsoft R Client on your computer, which is incompatible with Microsoft R Server version 8.x.x. Download and install a compatible version.* (Está ejecutando la versión 9.0.0 del Cliente de Microsoft R en el equipo, que es incompatible con Microsoft R Server versión 8.0.3. Descargue e instale una versión compatible).

SQL Server 2016 requiere que las bibliotecas de R del cliente coincidan exactamente con las bibliotecas de R del servidor. La restricción se ha quitado para versiones posteriores a R Server 9.0.1. No obstante, si se encuentra con este error, compruebe la versión de las bibliotecas de R que usan el cliente y el servidor; si es necesario, actualice el cliente para que coincida con la versión del servidor.

La versión de R que se instala con SQL Server R Services se actualiza siempre que se instala una versión del servicio SQL Server. Para garantizar que siempre tenga las versiones más actualizadas de los componentes de R, asegúrese de instalar todos los Service Pack.

Para garantizar la compatibilidad con Microsoft R Client 9.0.0, instale las actualizaciones que se describen en este [artículo de soporte técnico](https://support.microsoft.com/kb/3210262).

Para evitar problemas con los paquetes de R, también puede actualizar la versión de las bibliotecas de R que están instaladas en el servidor si cambia el contrato de servicio para que use la directiva de soporte técnico del ciclo de vida moderno, como se describe en [la siguiente sección](#bkmk_sqlbindr). Cuando lo haga, la versión de R que se instala con SQL Server se actualiza en la misma programación que se usa para las actualizaciones de Machine Learning Server (anteriormente Microsoft R Server).

**Se aplica a:** SQL Server 2016 R Services, con la versión 9.0.0 o anterior de R Server

### <a name="5-r-components-missing-from-cu3-setup"></a>5. Faltan componentes de R en la configuración de CU3

Un número limitado de máquinas virtuales de Azure se ha aprovisionado sin los archivos de instalación de R que deben incluirse con SQL Server. El problema se presenta en las máquinas virtuales aprovisionadas en el período comprendido entre el 05-01-2018 y el 23-01-2018. Este problema también podría afectar a las instalaciones locales si ha aplicado la actualización de CU3 para SQL Server 2017 durante el período del 05-01-2018 al 23-01-2018.

Se ha proporcionado una versión de servicio que incluye la versión correcta de los archivos de instalación de R.

+ [Paquete de actualización acumulativa 3 para SQL Server 2017 KB4052987](https://www.microsoft.com/download/details.aspx?id=56128).

Para instalar los componentes y reparar SQL Server 2017 CU3, debe desinstalar CU3 y reinstalar la versión actualizada:

1. Descargue el archivo de instalación de CU3 actualizado, que incluye los instaladores de R.
2. Desinstale CU3. En el panel de control, busque **Desinstalar una actualización** y después seleccione "Revisión 3015 para SQL Server 2017 (KB4052987) (64 bits)". Continúe con los pasos de desinstalación.
3. Reinstale la actualización de CU3; para ello, haga doble clic en la actualización de KB4052987 que acaba de descargar: `SQLServer2017-KB4052987-x64.exe`. Siga las instrucciones de instalación.

### <a name="6-unable-to-install-python-components-in-offline-installations-of-sql-server-2017-ctp-20-or-later"></a>6. No se pueden instalar los componentes de Python en instalaciones sin conexión de SQL Server 2017 CTP 2.0 o versiones posteriores

Si instala una versión preliminar de SQL Server 2017 en un equipo sin acceso a Internet, es posible que el instalador no muestre la página que solicita la ubicación de los componentes de Python descargados. En tal caso, puede instalar la característica Machine Learning Services, pero no los componentes de Python.

Este problema se ha corregido en la versión de lanzamiento. Además, esta limitación no afecta a los componentes de R.

**Se aplica a:** SQL Server 2017 con Python

### <a name="warning-of-incompatible-version-when-you-connect-to-an-older-version-of-sql-server-r-services-from-a-client-by-using-sssqlv14_md"></a><a name="bkmk_sqlbindr"></a> Advertencia de versión incompatible al conectarse a una versión anterior de SQL Server R Services desde un cliente mediante [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]

Al ejecutar el código de R en un contexto de proceso de SQL Server 2016, es posible que vea el siguiente error:

> *You are running version 9.0.0 of Microsoft R Client on your computer, which is incompatible with the Microsoft R Server version 8.0.3. Download and install a compatible version.* (Está ejecutando la versión 9.0.0 del Cliente de Microsoft R en el equipo, que es incompatible con Microsoft R Server versión 8.0.3. Descargue e instale una versión compatible).

Este mensaje se muestra si alguna de las dos indicaciones siguientes es verdadera:

+ Ha instalado R Server (independiente) en un equipo cliente mediante el asistente para la instalación de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
+ Ha instalado Microsoft R Server mediante el [instalador de Windows independiente](/machine-learning-server/install/r-server-install-windows).

Para asegurarse de que el servidor y el cliente usan la misma versión, es posible que tenga que usar un _enlace_, compatible con Microsoft R Server 9.0 y versiones posteriores, para actualizar los componentes de R en instancias de SQL Server 2016. Para determinar si la compatibilidad con las actualizaciones está disponible para su versión de R Services, consulte [Actualización de una instancia de R Services con SqlBindR.exe](../install/upgrade-r-and-python.md).

**Se aplica a:** SQL Server 2016 R Services, con la versión 9.0.0 o anterior de R Server

### <a name="7-setup-for-sql-server-2016-service-releases-might-fail-to-install-newer-versions-of-r-components"></a>7. La instalación de las versiones de servicio de SQL Server 2016 podrían producir un error al instalar versiones más recientes de los componentes de R

Al instalar una actualización acumulativa o un Service Pack para SQL Server 2016 en un equipo que no está conectado a Internet, el asistente para la instalación podría no mostrar el mensaje que le permite actualizar los componentes de R mediante los archivos CAB descargados. Este error suele ocurrir si se han instalado varios componentes con el motor de base de datos.

Como solución alternativa, puede instalar la versión de servicio mediante la línea de comandos. Para ello, especifique el argumento `MRCACHEDIRECTORY` tal como se muestra en este ejemplo, en el que se instalan actualizaciones de CU1:

`C:\<path to installation media>\SQLServer2016-KB3164674-x64.exe /Action=Patch /IACCEPTROPENLICENSETERMS /MRCACHEDIRECTORY=<path to CU1 CAB files>`

Para obtener los instaladores más recientes, consulte [Instalación de componentes de aprendizaje automático sin acceso a Internet](../install/sql-ml-component-install-without-internet-access.md).

**Se aplica a:** SQL Server 2016 R Services, con la versión 9.0.0 o anterior de R Server

### <a name="8-launchpad-services-fails-to-start-if-the-version-is-different-from-the-r-version"></a>8. No se puede iniciar el servicio Launchpad si la versión es diferente de la versión de R

Si instala SQL Server R Services independientemente del motor de base de datos y las versiones de compilación son diferentes, es posible que vea el siguiente error en el registro de eventos del sistema:

> *El servicio SQL Server Launchpad no pudo iniciarse debido al siguiente error: El servicio no respondió a la petición de inicio o control de manera oportuna.*

Por ejemplo, este error podría producirse si instala el motor de base de datos mediante la versión de lanzamiento, aplica una revisión para actualizar el motor de base de datos y, después, agrega la característica R Services mediante la versión de lanzamiento.

Para evitar este problema, use una utilidad como el administrador de archivos para comparar las versiones de Launchpad.exe con la versión de los archivos binarios de SQL, como sqldk.dll. Todos los componentes deben tener el mismo número de versión. Si actualiza un componente, asegúrese de aplicar la misma actualización a los demás componentes instalados.

Busque Launchpad en la carpeta `Binn` de la instancia. Por ejemplo, en una instalación predeterminada de SQL Server 2016, la ruta de acceso podría ser `C:\Program Files\Microsoft SQL Server\MSSQL.13.InstanceNameMSSQL\Binn`. 

### <a name="9-remote-compute-contexts-are-blocked-by-a-firewall-in-sql-server-instances-that-are-running-on-azure-virtual-machines"></a>9. Un firewall bloquea los contextos de proceso remoto en instancias de SQL Server que se ejecutan en máquinas virtuales de Azure

Si ha instalado [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en una máquina virtual de Azure, es posible que no pueda usar contextos de proceso que requieran el uso del área de trabajo de la máquina virtual. El motivo es que, de forma predeterminada, el firewall de las máquinas virtuales de Azure incluye una regla que bloquea el acceso a la red de las cuentas de usuario locales de R.

Como alternativa, en la máquina virtual de Azure, abra **Firewall de Windows con seguridad avanzada**, seleccione **Reglas de salida** y deshabilite la regla siguiente: **Block network access for R local user accounts in SQL Server instance MSSQLSERVER** (Bloquear el acceso a la red de las cuentas de usuario locales de R en la instancia de SQL Server MSSQLSERVER). También puede dejar la regla habilitada, pero cambiar la propiedad de seguridad a **Allow if secure** (Permitir si es seguro).

### <a name="10-implied-authentication-in-sqlexpress"></a>10. Autenticación implícita en SQLEXPRESS

Al ejecutar trabajos de R desde una estación de trabajo de ciencia de datos remota mediante la autenticación integrada de Windows, SQL Server usa la *autenticación implícita* para generar las llamadas ODBC locales que solicite el script. Pero esta característica no funcionaba en la compilación RTM de SQL Server Express Edition.

Para corregir el problema, se recomienda que actualice a una versión de servicio posterior.

Si no puede actualizar, use un inicio de sesión de SQL como solución alternativa para ejecutar los trabajos de R remotos que requieran llamadas ODBC insertadas.

**Se aplica a:** SQL Server 2016 R Services Express Edition

### <a name="11-performance-limits-when-libraries-used-by-sql-server-are-called-from-other-tools"></a>11. Límites de rendimiento cuando se llama a bibliotecas que usa SQL Server desde otras herramientas

Es posible llamar a las bibliotecas de aprendizaje automático que están instaladas para SQL Server desde una aplicación externa, como RGui. Esta puede ser la manera más cómoda de realizar ciertas tareas, como instalar nuevos paquetes o ejecutar pruebas ad hoc en ejemplos de código muy cortos. No obstante, fuera de SQL Server, el rendimiento podría ser limitado.

Por ejemplo, incluso si usa SQL Server Enterprise Edition, R se ejecuta en modo de subproceso único si ejecuta el código de R mediante herramientas externas. Para conseguir las ventajas de rendimiento en SQL Server, inicie una conexión de SQL Server y use [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) para llamar al tiempo de ejecución del script externo.

Como regla general, evite llamar a las bibliotecas de aprendizaje automático que usa SQL Server desde herramientas externas. Si necesita depurar código de R o Python, suele ser más fácil hacerlo fuera de SQL Server. Para obtener las mismas bibliotecas que están en SQL Server, puede instalar Microsoft R Client o [SQL Server 2017 Machine Learning Server (independiente)](../install/sql-machine-learning-standalone-windows-install.md).

### <a name="12-sql-server-data-tools-does-not-support-permissions-required-by-external-scripts"></a>12. SQL Server Data Tools no admite los permisos que requieren los scripts externos

Al usar Visual Studio o SQL Server Data Tools para publicar un proyecto de base de datos, si alguna entidad de seguridad tiene permisos específicos para la ejecución de scripts externos, podría recibir un error similar al siguiente:

> *TSQL Model: Error detected when reverse engineering the database. The permission was not recognized and was not imported* (Modelo TSQL: Error detectado al usar técnicas de ingeniería inversa en la base de datos. El permiso no se ha reconocido y no se ha importado).

Actualmente, el modelo DACPAC no admite los permisos que usan R Services o Machine Learning Services, como GRANT ANY EXTERNAL SCRIPT o EXECUTE ANY EXTERNAL SCRIPT. Este problema se corregirá en una versión posterior.

Como solución alternativa, ejecute las instrucciones GRANT adicionales en un script posterior a la implementación.

### <a name="13-external-script-execution-is-throttled-due-to-resource-governance-default-values"></a>13. La ejecución de scripts externos está limitada debido a los valores predeterminados de gobernanza de recursos

En Enterprise Edition, puede usar grupos de recursos para administrar procesos de script externos. En algunas compilaciones anteriores, la memoria máxima que se podía asignar a los procesos de R era del 20 %. Por lo tanto, si el servidor tenía 32 GB de RAM, los ejecutables de R (RTerm.exe y BxlServer.exe) podían usar un máximo de 6,4 GB en una única solicitud.

Si encuentra limitaciones de recursos, compruebe el valor predeterminado actual. Si el 20 % no es suficiente, consulte la documentación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para informarse sobre cómo cambiar este valor.

**Se aplica a:** SQL Server 2016 R Services, Enterprise Edition

### <a name="14-error-when-using-sp_execute_external_script-without-libcso-on-linux"></a>14. Error al usar `sp_execute_external_script` sin `libc++.so` en Linux

En un equipo Linux limpio que no tiene `libc++.so` instalado, se produce un error al ejecutar una consulta `sp_execute_external_script` (SPEES) con Java o un lenguaje externo porque `commonlauncher.so` no puede cargar `libc++.so`.

Por ejemplo:

```sql
EXECUTE sp_execute_external_script @language = N'Java'
    , @script = N'JavaTestPackage.PassThrough'
    , @parallel = 0
    , @input_data_1 = N'select 1'
WITH RESULT SETS((col1 INT NOT NULL))
GO
```

Se produce un error con un mensaje similar al siguiente:

```text
Msg 39012, Level 16, State 14, Line 0

Unable to communicate with the runtime for 'Java' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Java' runtime.
```

Los registros de `mssql-launchpadd` mostrarán un mensaje de error similar al siguiente:

```text
Oct 18 14:03:21 sqlextmls launchpadd[57471]: [launchpad] 2019/10/18 14:03:21 WARNING: PopulateLauncher failed: Library /opt/mssql-extensibility/lib/commonlauncher.so not loaded. Error: libc++.so.1: cannot open shared object file: No such file or directory
```

**Solución alternativa**

Puede usar una de las siguientes opciones:

1. Copie `libc++*` de `/opt/mssql/lib` a la ruta de acceso predeterminada del sistema `/lib64`.

1. Agregue las siguientes entradas a `/var/opt/mssql/mssql.conf` para exponer la ruta de acceso:

   ```text
   [extensibility]
   readabledirectories = /opt/mssql
   ```

**Se aplica a:** SQL Server 2019 en Linux

### <a name="15-installation-or-upgrade-error-on-fips-enabled-servers"></a>15. Error de instalación o actualización en los servidores habilitados para FIPS

Si instala SQL Server 2019 con la característica **Machine Learning Services y extensiones de lenguaje** o actualiza la instancia de SQL Server en un servidor habilitado para el [estándar federal de procesamiento de información (FIPS)](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), recibirá el siguiente error:

> *Error al instalar la característica de extensibilidad con el siguiente mensaje de error: AppContainer Creation Failed with error message NONE, state This implementation is not part of the Windows Platform FIPS validated cryptographic algorithms.* (No se pudo crear AppContainer con el mensaje de error: NINGUNO; estado: Esta implementación no forma parte de los algoritmos criptográficos validados por Windows Platform FIPS).

**Solución alternativa**

Deshabilite FIPS antes de instalar SQL Server 2019 con la característica **Microsoft Machine Learning Services y extensiones de lenguaje** o la actualización de la instancia de SQL Server. Una vez completada la instalación o la actualización, puede volver a habilitar FIPS.

**Se aplica a:** SQL Server 2019

## <a name="r-script-execution-issues"></a>Problemas con la ejecución del script de R

En esta sección se incluyen problemas conocidos que son específicos de la ejecución de R en SQL Server, así como algunos problemas relacionados con las bibliotecas y herramientas de R publicadas por Microsoft, incluida RevoScaleR.

Para consultar otros problemas conocidos que pueden afectar a las soluciones de R, vea el sitio de [Machine Learning Server](/machine-learning-server/resources-known-issues).

### <a name="1-access-denied-warning-when-executing-r-scripts-on-sql-server-in-a-non-default-location"></a>1. Advertencia de acceso denegado al ejecutar scripts de R en SQL Server en una ubicación no predeterminada

Si la instancia de SQL Server se ha instalado en una ubicación no predeterminada, como fuera de la carpeta `Program Files`, se genera la advertencia ACCESS_DENIED al intentar ejecutar scripts que instalan un paquete. Por ejemplo:

> *En `normalizePath(path.expand(path), winslash, mustWork)` : path[2]="~ExternalLibraries/R/8/1": Acceso denegado*

El motivo es que una función de R intenta leer la ruta de acceso y produce un error si el grupo de usuarios integrado **SQLRUserGroup** no tiene acceso de lectura. La advertencia que se genera no bloquea la ejecución del script de R actual, pero la advertencia puede repetirse de forma reiterada siempre que el usuario ejecute cualquier otro script de R.

Si ha instalado SQL Server en la ubicación predeterminada, este error no se produce porque todos los usuarios de Windows tienen permisos de lectura en la carpeta `Program Files`.

Este problema se soluciona en una próxima versión de servicio. Como solución alternativa, proporcione al grupo **SQLRUserGroup** acceso de lectura para todas las carpetas principales de `ExternalLibraries`.

### <a name="2-serialization-error-between-old-and-new-versions-of-revoscaler"></a>2. Error de serialización entre versiones anteriores y nuevas de RevoScaleR

Al pasar un modelo con un formato serializado a una instancia de SQL Server remota, puede obtener el error: 

> *Error in memDecompress(data, type = decompress) internal error -3 in memDecompress(2).* [Error en memDecompress(data, type = decompress); error interno -3 en memDecompress(2)].

Este error se produce si ha guardado el modelo con una versión reciente de la función de serialización [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel), pero la instancia de SQL Server donde se deserializa el modelo tiene una versión anterior de las API de RevoScaleR, de SQL Server 2017 CU2 o versiones anteriores.

Como solución alternativa, puede actualizar la instancia de SQL Server 2017 a CU3 o una versión posterior.

El error no aparece si la versión de la API es la misma ni tampoco si va a mover un modelo guardado con una función de serialización anterior a un servidor que usa una versión más reciente de la API de serialización.

En otras palabras, use la misma versión de RevoScaleR para las operaciones de serialización y deserialización.

### <a name="3-real-time-scoring-does-not-correctly-handle-the-_learningrate_-parameter-in-tree-and-forest-models"></a>3. La puntuación en tiempo real no controla correctamente el parámetro _learningRate_ en los modelos de árbol y bosque

Si crea un modelo mediante un método de árbol de decisión o de bosque de decisión y especifica la velocidad de aprendizaje, podría ver resultados incoherentes al usar `sp_rxpredict` o la función `PREDICT` de SQL, en comparación con el uso de `rxPredict`.

La causa es un error en la API que procesa modelos serializados y se limita al parámetro `learningRate`: por ejemplo, en [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees).

Este problema se soluciona en una próxima versión de servicio.

### <a name="4-limitations-on-processor-affinity-for-r-jobs"></a>4. Limitaciones de la afinidad del procesador para trabajos de R

En la compilación de versión inicial de SQL Server 2016, se podía establecer la afinidad del procesador únicamente para las CPU del primer grupo K. Por ejemplo, si el servidor es una máquina de dos sockets con dos grupos K, solo se usan los procesadores del primer grupo K para los procesos de R. La misma limitación se aplica al configurar la gobernanza de recursos para trabajos del script de R.

Este problema está corregido en SQL Server 2016 Service Pack 1. Se recomienda que actualice a la versión de servicio más reciente.

**Se aplica a:** Versión RTM de SQL Server 2016 R Services

### <a name="5-changes-to-column-types-cannot-be-performed-when-reading-data-in-a-sql-server-compute-context"></a>5. No se pueden realizar cambios en los tipos de columna al leer datos en un contexto de proceso de SQL Server

Si el contexto de proceso está establecido en la instancia de SQL Server, no puede usar el argumento _colClasses_ (u otros argumentos similares) para cambiar el tipo de datos de las columnas en el código de R.

Por ejemplo, la instrucción siguiente producirá un error si la columna CRSDepTimeStr no es ya un entero:

```R
data <- RxSqlServerData(
  sqlQuery = "SELECT CRSDepTimeStr, ArrDelay  FROM AirlineDemoSmall", 
  connectionString = connectionString, 
  colClasses = c(CRSDepTimeStr = "integer"))
```

Como solución alternativa, puede volver a escribir la consulta SQL para usar CAST o CONVERT y presentar los datos a R con el tipo de datos correcto. En general, el rendimiento es mejor si trabaja con los datos mediante SQL, en lugar de cambiar los datos en el código de R.

**Se aplica a:** SQL Server 2016 R Services

### <a name="6-limits-on-size-of-serialized-models"></a>6. Límites en el tamaño de los modelos serializados

Al guardar un modelo en una tabla de SQL Server, debe serializar el modelo y guardarlo en un formato binario. Teóricamente, el tamaño máximo de un modelo que se puede almacenar con este método es de 2 GB, que es el tamaño máximo de las columnas varbinary en SQL Server.

Si necesita usar modelos de mayor tamaño, hay disponibles las siguientes soluciones alternativas:

+ Tome medidas para reducir el tamaño del modelo. Algunos paquetes de R de código abierto incluyen una gran cantidad de información en el objeto de modelo y gran parte de esta información se puede quitar para su implementación. 
+ Use la selección de características para quitar columnas innecesarias.
+ Si usa un algoritmo de código abierto, considere la posibilidad de llevar a cabo una implementación similar con el algoritmo correspondiente en MicrosoftML o RevoScaleR. Estos paquetes se han optimizado para escenarios de implementación.
+ Después de racionalizar el modelo y reducir el tamaño con los pasos anteriores, vea si se puede usar la función [memCompress](https://www.rdocumentation.org/packages/base/versions/3.4.1/topics/memCompress) en base R para reducir el tamaño del modelo antes de pasarlo a SQL Server. Esta opción es mejor si el modelo está cerca del límite de 2 GB.
+ En el caso de modelos de mayor tamaño, puede usar la característica [FileTable](../../relational-databases/blob/filetables-sql-server.md) de SQL Server para almacenar los modelos, en lugar de usar una columna varbinary.

    Para usar FileTable, debe agregar una excepción de firewall, ya que los datos almacenados en FileTable se administran mediante el controlador de sistema de archivos de FileStream en SQL Server y las reglas de Firewall predeterminadas bloquean el acceso a archivos de red. Para obtener más información, vea [Habilitar los requisitos previos de FileTable](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md).

    Después de haber habilitado FileTable, para escribir el modelo, obtendrá una ruta de acceso de SQL mediante la API de FileTable y después escribirá el modelo en esa ubicación desde el código. Cuando tenga que leer el modelo, obtenga la ruta de acceso de SQL y después llame al modelo mediante la ruta de acceso del script. Para obtener más información, vea [Obtener acceso a FileTables con API de entrada-salida de archivo](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md).

### <a name="7-avoid-clearing-workspaces-when-you-execute-r-code-in-a-ssnoversion-compute-context"></a>7. Evitar borrar áreas de trabajo al ejecutar código de R en un contexto de proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Si usa un comando de R para borrar los objetos del área de trabajo mientras se ejecuta código de R en un contexto de proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], o bien si borra el área de trabajo como parte de un script de R llamado mediante [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), podría recibir este error: *workspace object 'revoScriptConnection' not found* (No se ha encontrado el objeto del área de trabajo "revoScriptConnection").

`revoScriptConnection` es un objeto del área de trabajo de R que contiene información sobre una sesión de R a la que se llama desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pero si su código de R incluye un comando para borrar el área de trabajo (como `rm(list=ls()))`, también se borrarán toda la información sobre la sesión y otros objetos del área de trabajo de R.

Como solución alternativa, evite borrar indiscriminadamente las variables y otros objetos mientras ejecuta R en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aunque es habitual borrar el área de trabajo cuando se trabaja en la consola de R, puede tener consecuencias no deseadas.

+ Para eliminar variables específicas, use la función `remove` de R: por ejemplo, `remove('name1', 'name2', ...)`.
+ Si quiere eliminar varias variables, guarde los nombres de las variables temporales en una lista y realice periódicamente la recolección de elementos no utilizados.

### <a name="8-restrictions-on-data-that-can-be-provided-as-input-to-an-r-script"></a>8. Restricciones en los datos que se pueden proporcionar como entrada para un script de R

No puede usar un script de R en los siguientes tipos de resultados de consulta:

- Datos de una consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] que haga referencia a columnas AlwaysEncrypted.
  
- Datos de una consulta de [!INCLUDE[tsql](../../includes/tsql-md.md)] que haga referencia a columnas enmascaradas.
  
     Si necesita usar datos enmascaradas en un script de R, una posible solución alternativa es realizar una copia de los datos en una tabla temporal y usar esos datos en su lugar.

### <a name="9-use-of-strings-as-factors-can-lead-to-performance-degradation"></a>9. El uso de cadenas como factores puede provocar una degradación del rendimiento

El uso de variables de tipo de cadena como factores puede aumentar considerablemente la cantidad de memoria que se usa para las operaciones de R. Se trata de un problema conocido en R en general y hay muchos artículos sobre el tema. Por ejemplo, vea [Factors are not first-class citizens in R (Los factores no son de primera clase en R) de John Mount en R-bloggers](https://www.r-bloggers.com/factors-are-not-first-class-citizens-in-r/) o [stringsAsFactors: An unauthorized biography (stringsAsFactors: una biografía no autorizada) de Roger Peng](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/). 

Aunque el problema no es específico de SQL Server, puede afectar en gran medida al rendimiento de la ejecución de código de R en SQL Server. Normalmente, las cadenas se almacenan como varchar o nvarchar y, si una columna de datos de cadena tiene muchos valores únicos, el proceso de que R las convierta internamente en enteros y de nuevo en cadenas puede incluso provocar errores de asignación de memoria.

Si no necesita en absoluto un tipo de datos de cadena para otras operaciones, la asignación de los valores de cadena a un tipo de datos numérico (entero) como parte de la preparación de datos resultaría beneficiosa desde una perspectiva del rendimiento y la escala.

Para obtener una explicación de este problema y otras sugerencias, vea [Rendimiento de R Services: optimización de datos](../r/r-and-data-optimization-r-services.md).

### <a name="10-arguments-varstokeep-and-varstodrop-are-not-supported-for-sql-server-data-sources"></a>10. Los argumentos *varsToKeep* y *varsToDrop* no se admiten en los orígenes de datos de SQL Server

Si usa la función rxDataStep para escribir los resultados en una tabla, utilizar los argumentos *varsToKeep* y *varsToDrop* es una forma cómoda de especificar las columnas que se van a incluir o excluir como parte de la operación. En cambio, estos argumentos no se admiten en los orígenes de datos de SQL Server.

### <a name="11-limited-support-for-sql-data-types-in-sp_execute_external_script"></a>11. Compatibilidad limitada con los tipos de datos SQL en sp\_execute\_external\_script

No todos los tipos de datos que se admiten en SQL pueden usarse en R. Como solución alternativa, considere la posibilidad de convertir el tipo de datos no admitido en un tipo de datos compatible antes de pasar los datos a sp\_execute\_external\_script.

Para obtener más información, consulte [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

### <a name="12-possible-string-corruption-using-unicode-strings-in-varchar-columns"></a>12. Posibles daños en las cadenas al usar cadenas Unicode en columnas varchar

El paso de datos Unicode en columnas varchar de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a R o Python puede producir daños en las cadenas. Esto se debe a que la codificación de estas cadenas Unicode en las intercalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede no coincidir con la codificación UTF-8 predeterminada que se usa en R o Python. 

Para enviar datos de cadena no ASCII de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a R o Python, use la codificación UTF-8 (disponible en [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) o el tipo nvarchar.

### <a name="13-only-one-value-of-type-raw-can-be-returned-from-sp_execute_external_script"></a>13. Solo se puede devolver un valor de tipo `raw` desde `sp_execute_external_script`

Cuando se devuelve desde R un tipo de datos binarios (el tipo de datos **raw** de R), el valor debe enviarse en la trama de datos de salida.

Con tipos de datos que no sean **raw**, puede devolver valores de parámetro con los resultados del procedimiento almacenado si agrega la palabra clave OUTPUT. Para obtener más información, vea [Parámetros](../../relational-databases/stored-procedures/parameters.md).

Si quiere usar varios conjuntos de salida que incluyan valores de tipo **raw**, una posible solución alternativa es realizar varias llamadas del procedimiento almacenado o enviar los conjuntos de resultados a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante ODBC.

### <a name="14-loss-of-precision"></a>14. Pérdida de precisión

Dado que [!INCLUDE[tsql](../../includes/tsql-md.md)] y R admiten varios tipos de datos, los tipos de datos numéricos pueden perder precisión durante la conversión.

Para obtener más información sobre la conversión implícita de tipos de datos, vea [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

### <a name="15-variable-scoping-error-when-you-use-the-transformfunc-parameter"></a>15. Error de ámbito de variable al usar el parámetro transformFunc

Para transformar datos durante el modelado, puede pasar un argumento *transfomFunc* en una función como `rxLinmod` o `rxLogit`. No obstante, las llamadas a funciones anidadas pueden provocar errores de ámbito en el contexto de proceso de SQL Server, incluso si las llamadas funcionan correctamente en el contexto de proceso local.

> *El conjunto de datos de ejemplo del análisis no tiene variables*

Por ejemplo, supongamos que ha definido dos funciones `f` y `g` en su entorno global local, y que `g` llama a `f`. En las llamadas remotas o distribuidas en las que participa `g`, la llamada a `g` podría generar un error porque no se encuentra `f`, incluso si ha pasado `f` y `g` a la llamada remota.

Si se produce este problema, puede solucionarlo insertando la definición de `f` en la definición de `g`, en cualquier lugar antes de que `g` llame a `f`.

Por ejemplo:

```R
f <- function(x) { 2*x * 3 }
g <- function(y) {
              a <- 10 * y
               f(a)
}
```

Para evitar el error, vuelva a escribir la definición de la siguiente manera:

```R
g <- function(y){
              f <- function(x) { 2*x +3}
              a <- 10 * y
              f(a)
}
```

### <a name="16-data-import-and-manipulation-using-revoscaler"></a>16. Importación de datos y manipulación con RevoScaleR

Al leer columnas **varchar** desde una base de datos, se eliminan los espacios en blanco. Para evitarlo, ponga las cadenas entre caracteres que no sean espacios en blanco.

Al usar funciones como `rxDataStep` para crear tablas de base de datos con columnas **varchar**, el ancho de columna se calcula en función de una muestra de los datos. Si el ancho varía, puede que sea necesario rellenar todas las cadenas con una longitud común.

El uso de una transformación para cambiar el tipo de datos de una variable no se admite cuando se usan llamadas repetidas a `rxImport` o `rxTextToXdf` para importar y anexar filas combinando varios archivos de entrada en un solo archivo .xdf.

### <a name="17-limited-support-for-rxexec"></a>17. Compatibilidad limitada con rxExec

En SQL Server 2016, la función `rxExec` que proporciona el paquete RevoScaleR solo se puede usar en el modo de un solo subproceso.

### <a name="18-increase-the-maximum-parameter-size-to-support-rxgetvarinfo"></a>18. Aumento del tamaño máximo de parámetro para admitir rxGetVarInfo

Si usa conjuntos de datos con números de variables muy grandes (por ejemplo, más de 40 000), establezca la marca `max-ppsize` al iniciar R para poder usar funciones como `rxGetVarInfo`. La marca `max-ppsize` especifica el tamaño máximo de la pila de protección del puntero.

Si usa la consola de R (por ejemplo, en RGui.exe o RTerm.exe), puede establecer el valor de _max-ppsize_ en 500000. Para ello, escriba lo siguiente:

```R
R --max-ppsize=500000
```

### <a name="19-issues-with-the-rxdtree-function"></a>19. Problemas con la función rxDTree

La función `rxDTree` no admite actualmente transformaciones en la fórmula. En particular, no se admite el uso de la sintaxis `F()` para crear factores sobre la marcha. En cambio, los datos numéricos se discretizan automáticamente.

Los factores ordenados se tratan igual que los factores de todas las funciones de análisis RevoScaleR, excepto `rxDTree`.

### <a name="20-datatable-as-an-outputdataset-in-r"></a>20. Data.table como OutputDataSet en R

No se admite el uso de `data.table` como `OutputDataSet` en R en la actualización acumulativa 13 (CU13) de SQL Server 2017 y versiones anteriores. Puede aparecer el siguiente mensaje:

``` text
Msg 39004, Level 16, State 20, Line 2
A 'R' script error occurred during execution of 
'sp_execute_external_script' with HRESULT 0x80004004.
Msg 39019, Level 16, State 2, Line 2
An external script error occurred: 
Error in alloc.col(newx) : 
  Internal error: length of names (0) is not length of dt (11)
Calls: data.frame ... as.data.frame -> as.data.frame.data.table -> copy -> alloc.col

Error in execution.  Check the output for more information.
Error in eval(expr, envir, enclos) : 
  Error in execution.  Check the output for more information.
Calls: source -> withVisible -> eval -> eval -> .Call
Execution halted
```

No se admite `data.table` como `OutputDataSet` en R en la actualización acumulativa 14 (CU14) de SQL Server 2017 y versiones posteriores.

### <a name="21-running-a-long-script-fails-while-installing-a-library"></a>21. Se produce un error al ejecutar un script largo durante la instalación de una biblioteca

Si se ejecuta una sesión de script externo de larga duración y se tiene el DBO en paralelo intentando instalar una biblioteca en otra base de datos, es posible que se finalice el script.

Por ejemplo, al ejecutar este script externo en la base de datos maestra:

```sql
USE MASTER
DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(max) = N'Sys.sleep(100)'
DECLARE @input_data_1 nvarchar(max) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1 with result sets none
go
```

Mientras que DBO en paralelo instala una biblioteca en LibraryManagementFunctional:

```sql
USE [LibraryManagementFunctional]
go

CREATE EXTERNAL LIBRARY [RODBC] FROM (CONTENT = N'/home/ani/var/opt/mssql/data/RODBC_1.3-16.tar.gz') WITH (LANGUAGE = 'R')
go

DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(14) = N'library(RODBC)'
DECLARE @input_data_1 nvarchar(8) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1
go
```

El script externo de larga duración anterior en la base de datos maestra finalizará con el siguiente mensaje de error:

> *Error de script 'R' durante la ejecución de 'sp_execute_external_script' con HRESULT 0x800704d4.*

**Solución alternativa**

No ejecute la instalación de la biblioteca en paralelo a la consulta de larga duración. También puede volver a ejecutar la consulta de larga duración una vez completada la instalación.

**Se aplica a:** SQL Server 2019 solo en clústeres de macrodatos y en Linux.

### <a name="22-sql-server-stops-responding-when-executing-r-scripts-containing-parallel-execution"></a>22. SQL Server deja de responder al ejecutar scripts de R que contienen la ejecución en paralelo

SQL Server 2019 contiene una regresión que afecta a los scripts de R que usan la ejecución en paralelo. Entre los ejemplos se incluye el uso de `rxExec` con contexto de cálculo `RxLocalPar` y scripts que usan el paquete paralelo. Este problema se debe a errores que el paquete paralelo encuentra al escribir en el dispositivo nulo mientras se ejecuta en SQL Server.

**Se aplica a:** SQL Server 2019.

### <a name="23-precision-loss-for-moneynumericdecimalbigint-data-types"></a>23. Pérdida de precisión en los tipos de datos Money/Numeric/decimal/BigInt

Si ejecuta un script de R con `sp_execute_external_script` se permiten los tipos de datos Money, Numeric, Decimal y BigInt como datos de entrada. Pero, dado que se convierten al tipo numérico de R, sufren una pérdida de precisión con valores muy altos o con valores de separador decimal.

+ **Money**: A veces, los valores de céntimos podrían ser imprecisos y, en este caso, se emitiría una advertencia: *Advertencia: No se pueden representar con precisión los valores de céntimos*.  
+ **Numeric/Decimal**: `sp_execute_external_script` con un script de R no admite el rango completo de esos tipos de datos y modificaría los últimos dígitos decimales, especialmente aquellos con fracción.
+ **BigInt**: R solo admite enteros de 53 bits y, posteriormente, empezará a tener una pérdida de precisión.

## <a name="python-script-execution-issues"></a>Problemas con la ejecución del script de Python

En esta sección se incluyen problemas conocidos que son específicos de la ejecución de Python en SQL Server, así como algunos problemas relacionados con los paquetes de Python publicados por Microsoft, incluidos [revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package) y [microsoftml](/r-server/python-reference/microsoftml/microsoftml-package).

### <a name="1-call-to-pretrained-model-fails-if-path-to-model-is-too-long"></a>1. Se produce un error en la llamada a un modelo entrenado previamente si la ruta de acceso al modelo es demasiado larga

Si ha instalado los modelos entrenados previamente en una versión anterior de SQL Server 2017, la ruta de acceso completa al archivo de modelo entrenado podría ser demasiado larga para que Python la lea. Esta limitación se corrige en una versión de servicio posterior.

Hay varias soluciones posibles:

+ Al instalar los modelos entrenados previamente, elija una ubicación personalizada.
+ Si es posible, instale la instancia de SQL Server en una ruta de instalación personalizada con una ruta de acceso más corta, como C:\SQL\MSSQL14.MSSQLSERVER.
+ Use la utilidad de Windows [Fsutil](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc788097(v=ws.11)) para crear un vínculo físico que asigne el archivo de modelo a una ruta de acceso más corta.
+ Actualice a la versión de servicio más reciente.

### <a name="2-error-when-saving-serialized-model-to-sql-server"></a>2. Error al guardar el modelo serializado en SQL Server

Al pasar un modelo a una instancia de SQL Server remota e intentar leer el modelo binario con la función `rx_unserialize` en [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), puede obtener el error: 

> *NameError: name 'rx_unserialize_model' is not defined* (NameError: no se ha definido el nombre "rx_unserialize_model")

Este error se produce si ha guardado el modelo con una versión reciente de la función de serialización, pero la instancia de SQL Server donde se deserializa el modelo no reconoce la API de deserialización.

Para solucionar el problema, actualice la instancia de SQL Server 2017 a CU3 o una versión posterior.

### <a name="3-failure-to-initialize-a-varbinary-variable-causes-an-error-in-bxlserver"></a>3. Si no se puede inicializar una variable varbinary, se produce un error en BxlServer

Si ejecuta código de Python en SQL Server mediante `sp_execute_external_script` y el código tiene variables de salida de tipo varbinary(max), varchar(max) o tipos similares, la variable se debe inicializar o establecer como parte del script. De lo contrario, el componente de intercambio de datos BxlServer genera un error y deja de funcionar.

Esta limitación se solucionará en una próxima versión de servicio. Como solución alternativa, asegúrese de que la variable se inicializa dentro del script de Python. Se puede usar cualquier valor válido, como en los ejemplos siguientes:

```sql
declare @b varbinary(max);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N'b = 0x0'
  , @params = N'@b varbinary(max) OUTPUT'
  , @b = @b OUTPUT;
go
```

```sql
declare @b varchar(30);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N' b = ""  '
  , @params = N'@b varchar(30) OUTPUT'
  , @b = @b OUTPUT;
go
```

### <a name="4-telemetry-warning-on-successful-execution-of-python-code"></a>4. Advertencia de telemetría en una ejecución correcta del código de Python

A partir de SQL Server 2017 CU2, puede aparecer el siguiente mensaje aunque el código de Python se ejecute correctamente:

> *STDERR message(s) from external script:* 
>  *~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger*
> *SyntaxWarning: telemetry_state is used prior to global declaration* (Mensaje(s) STDERR del script externo: ~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger SyntaxWarning: telemetry_state se ha usado antes de la declaración global)

Este problema se ha corregido en la actualización acumulativa 3 (CU3) de SQL Server 2017. 

### <a name="5-numeric-decimal-and-money-data-types-not-supported"></a>5. Tipos de datos numéricos, decimales y monetarios no admitidos

A partir de la actualización acumulativa 12 (CU12) de SQL Server 2017, los tipos de datos numéricos, decimales y monetarios de WITH RESULT SETS no se admiten si se usa Python con `sp_execute_external_script`. Pueden aparecer los siguientes mensajes:

> *[Código: 39004, estado de SQL: S1000] Error de script 'Python' durante la ejecución de 'sp_execute_external_script' con HRESULT 0x80004004.*
>
> *[Código: 39019, estado de SQL: S1000] Error en el script externo:*
>
> *SqlSatelliteCall error: Unsupported type in output schema. Supported types: bit, smallint, int, datetime, smallmoney, real and float. char, varchar are partially supported.* (Error de SqlSatelliteCall: tipo no compatible en el esquema de salida. Tipos compatibles: bit, smallint, int, datetime, smallmoney, real y float. char y varchar se admiten parcialmente)

Esto se ha corregido en la actualización acumulativa 14 (CU14) de SQL Server 2017.

### <a name="6-bad-interpreter-error-when-installing-python-packages-with-pip-on-linux"></a>6. Error de intérprete incorrecto al instalar paquetes de Python con pip en Linux 

En SQL Server 2019, si intenta usar **pip**. Por ejemplo:

```bash
/opt/mssql/mlservices/runtime/python/bin/pip -h
```

Recibirá este error:

> *bash: /opt/mssql/mlservices/runtime/python/bin/pip: /opt/microsoft/mlserver/9.4.7/bin/python/python: bad interpreter: no se encontró el archivo o directorio*,

**Solución alternativa**

Instale **pip** desde [Python Package Authority (PyPA)](https://www.pypa.io):

```bash
wget 'https://bootstrap.pypa.io/get-pip.py' 
/opt/mssql/mlservices/bin/python/python ./get-pip.py 
```

**Recomendación**

Vea [Instalación de paquetes de Python con sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md).

**Se aplica a:** SQL Server 2019 en Linux

### <a name="7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows"></a>7. No se pueden instalar paquetes de Python con pip después de instalar SQL Server 2019 en Windows

Después de instalar SQL Server 2019 en Windows, se producirá un error al intentar instalar un paquete de Python mediante **pip** desde una línea de comandos de DOS. Por ejemplo:

```bash
pip install quantfolio
```

Esto devolverá el siguiente error:

> *pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.* (pip se ha configurado con ubicaciones que requieren TLS/SSL, pero el módulo SSL de Python no está disponible).

Se trata de un problema específico del paquete Anaconda. Se solucionará en una próxima versión de servicio.

**Solución alternativa**

Copie los archivos siguientes:

+ `libssl-1_1-x64.dll`
+ `libcrypto-1_1-x64.dll`

de la carpeta <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\Library\bin`

a la carpeta <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\DLLs`

Después, abra un nuevo símbolo del sistema del shell de comandos de DOS.

**Se aplica a:** SQL Server 2019 en Windows

### <a name="8-error-when-using-sp_execute_external_script-without-libcaboso-on-linux"></a>8. Error al usar `sp_execute_external_script` sin `libc++abo.so` en Linux

En un equipo con Linux limpio que no tiene `libc++abi.so` instalado, la ejecución de una consulta `sp_execute_external_script` (SPEES) produce el error "No se encontró el archivo o directorio".

Por ejemplo:

```text
EXEC sp_execute_external_script
    @language = N'Python'
    , @script = N'
OutputDataSet = InputDataSet'
    , @input_data_1 = N'select 1'
    , @input_data_1_name = N'InputDataSet'
    , @output_data_1_name = N'OutputDataSet'
    WITH RESULT SETS (([output] int not null));
Msg 39012, Level 16, State 14, Line 0
Unable to communicate with the runtime for 'Python' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Python' runtime.
STDERR message(s) from external script:

Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.

SqlSatelliteCall error: Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.
STDOUT message(s) from external script:
SqlSatelliteCall function failed. Please see the console output for more information.
Traceback (most recent call last):
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/computecontext/RxInSqlServer.py", line 605, in rx_sql_satellite_call
    rx_native_call("SqlSatelliteCall", params)
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/RxSerializable.py", line 375, in rx_native_call
    ret = px_call(functionname, params)
RuntimeError: revoscalepy function failed.
Total execution time: 00:01:00.387
```

**Solución alternativa**

Ejecute el siguiente comando:

```bash
sudo cp /opt/mssql/lib/libc++abi.so.1 /opt/mssql-extensibility/lib/
```

**Se aplica a:** SQL Server 2019 en Linux

### <a name="9-cannot-install-tensorflow-package-using-sqlmlutils"></a>9. No se puede instalar el paquete **tensorflow** con **sqlmlutils**

El [paquete sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md) se usa para instalar paquetes de Python en SQL Server 2019. Debe descargar, instalar y actualizar [Microsoft Visual C++ 2015-2019 Redistributable (x64)](https://visualstudio.microsoft.com/downloads/). Pero no se puede instalar el paquete **tensorflow** con sqlmlutils. El paquete tensorflow depende de una versión más reciente de NumPy que la versión instalada en SQL Server, pero NumPy es un paquete de sistema preinstalado que sqlmlutils no puede actualizar al intentar instalar tensorflow.

**Solución alternativa**

Con el símbolo del sistema en modo de administrador, ejecute el siguiente comando y reemplace "MSSQLSERVER" por el nombre de la instancia de SQL:

   ```cmd
   "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES\python.exe" -m pip install --upgrade tensorflow
   ```

   Si recibe un error de "TLS/SSL", consulte [7. No se pueden instalar paquetes de Python con pip](#7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows), anteriormente en este artículo.

**Se aplica a:** SQL Server 2019 en Windows

## <a name="revolution-r-enterprise-and-microsoft-r-open"></a>Revolution R Enterprise y Microsoft R Open

En esta sección se enumeran los problemas específicos de la conectividad, el desarrollo y las herramientas de rendimiento de R que proporciona Revolution Analytics. Estas herramientas se han proporcionado en versiones preliminares anteriores de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

En general, se recomienda que desinstale estas versiones anteriores e instale la versión más reciente de SQL Server o Microsoft R Server.

### <a name="1-revolution-r-enterprise-is-not-supported"></a>1. No se admite Revolution R Enterprise

No se admite la instalación en paralelo de Revolution R Enterprise con ninguna versión de [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)].

Si tiene una licencia existente para Revolution R Enterprise, debe colocarla en un equipo diferente de donde se encuentran la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y las estaciones de trabajo que quiera usar para conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Algunas versiones preliminares de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] incluyen un entorno de desarrollo de R para Windows creado mediante Revolution Analytics. Esta herramienta ya no se proporciona y no es compatible.

Para asegurar la compatibilidad con [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], se recomienda instalar Microsoft R Client en su lugar. [Herramientas de R para Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) y [Visual Studio Code](https://code.visualstudio.com/) también admiten las soluciones de Microsoft R.

### <a name="2-compatibility-issues-with-sqlite-odbc-driver-and-revoscaler"></a>2. Problemas de compatibilidad con el controlador ODBC de SQLite y RevoScaleR

La revisión 0.92 del controlador ODBC de SQLite no es compatible con RevoScaleR. Las revisiones 0.88 a 0.91 y 0.93, y las versiones posteriores son compatibles.

## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas de aprendizaje automático en SQL Server](machine-learning-troubleshooting-overview.md)