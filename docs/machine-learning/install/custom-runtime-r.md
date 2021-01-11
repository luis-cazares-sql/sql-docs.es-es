---
title: Instalación de un entorno de ejecución personalizado de R
description: Obtenga información sobre cómo instalar un entorno de ejecución personalizado de R para SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 09/20/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 9029dd87712ee7128504a9b495b699c9c58e000d
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804343"
---
# <a name="install-an-r-custom-runtime-for-sql-server"></a>Instalación de un entorno de ejecución personalizado de R para SQL Server

[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe cómo instalar un entorno de ejecución personalizado para ejecutar scripts de R con SQL Server. El tiempo de ejecución personalizado usa la tecnología de extensión de lenguaje basada en un marco de extensibilidad para ejecutar código externo. El entorno de ejecución personalizado para R se puede usar en los escenarios siguientes:

+ Una instalación de SQL Server con el marco de extensibilidad.

+ Una instalación de Machine Learning Services con SQL Server 2019. La extensión de lenguaje se puede usar con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) después de completar algunos pasos de configuración adicionales.

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> En este artículo se describe cómo instalar un entorno de ejecución personalizado para R en Windows. Para instalar en Linux, consulte [Instalación de un entorno de ejecución personalizado de R para SQL Server en Linux](custom-runtime-r.md?view=sql-server-linux-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Lista de comprobación previa a la instalación

Antes de instalar un entorno de ejecución personalizado de R, instale lo siguiente:

+ [SQL Server 2019 para Windows (actualización acumulativa 3 o posterior)](../../database-engine/install-windows/install-sql-server.md).

+ [Extensiones de lenguaje de SQL Server en Windows con el marco de extensibilidad](../../language-extensions/install/windows-java.md).

+ [R versión 3.3 o posterior](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-windows"></a>Adición de las extensiones de lenguaje de SQL Server para Windows

> [!NOTE]
> Si ha instalado Machine Learning Services en SQL Server 2019, el marco de extensibilidad para las extensiones de lenguaje con el servicio Launchpad ya está instalado y puede omitir este paso.

Las extensiones de lenguaje usan el marco de extensibilidad para ejecutar código externo. La ejecución del código está aislada de los procesos principales del motor, pero está totalmente integrada con la ejecución de consultas de SQL Server.

1. Inicie el asistente para la instalación de SQL Server 2019.

1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

    ![Instalación de SQL Server 2019 CU3 o posterior](../install/media/2019setup-installation-page-mlsvcs.png)

1. En la página **Selección de características** , seleccione estas opciones:

    - **Servicios de Motor de base de datos**

        Para usar las extensiones de lenguaje con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia predeterminada o una con nombre.

    - **Machine Learning Services y extensiones de lenguaje**

        Seleccione **Machine Learning Services y extensiones de lenguaje**. No es necesario seleccionar R.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Instalación de Extensiones de lenguaje de SQL Server 2019.":::

1. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.

    + Servicios de Motor de base de datos
    + Machine Learning Services y extensiones de lenguaje

1. Cuando finalice la instalación, si el programa indica que se reinicie el equipo, hágalo. Es importante leer el mensaje del Asistente para la instalación tras finalizar el programa de instalación. Para obtener más información, vea [View and Read SQL Server Setup Log Files](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).

## <a name="install-r"></a>Instalar R

> [!NOTE]
>En el caso de SQL Server Machine Learning Services, R ya está instalado en la carpeta **R_SERVICES** de la instancia de SQL Server. Por ejemplo, "C:\Archivos de programa\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES". Si desea seguir usando esta ruta de acceso como R_HOME, vaya al siguiente paso de la instalación de Rcpp. En caso contrario, si desea usar otro entorno de ejecución de R, siga aquí para instalarlo.

Instale [R (3.3 o posterior)](https://cran.r-project.org/bin/windows/base/) y anote la ruta de acceso donde está instalado. Esta ruta de acceso es **R_HOME**. Por ejemplo, como se muestra aquí, R_HOME es "C:\Archivos de programa\R\R-4.0.2".

![Instalación de R personalizada](../install/media/custom-r-installation.png)

> [!NOTE]
>En las siguientes instrucciones, %R_HOME% es la ruta de acceso a la instalación de R y se debe reemplazar por ese valor.

## <a name="install-rcpp-package"></a>Instalación del paquete Rcpp

+ Busque el archivo ejecutable de R en %R_HOME%\bin. De forma predeterminada, está en `C:\Program Files\R\R-4.0.2\bin`.

+ Inicie R desde un símbolo del sistema con privilegios *elevados*:

```CMD
%R_HOME%\bin\R.exe
```

En este símbolo del sistema de R con privilegios *elevados* (%R_HOME%\bin\R.exe), ejecute el siguiente script para instalar el paquete Rcpp en la carpeta %R_HOME%\library.

```R
install.packages("Rcpp", lib="%R_HOME%/library");
```

## <a name="update-the-system-environment-variables"></a>Actualización de las variables de entorno del sistema

1. Agregue o modifique **R_HOME** como variable de entorno del sistema.
    + En el cuadro de búsqueda de Windows, escriba "entorno" y seleccione **Editar las variables de entorno del sistema**.
    + En la pestaña **Opciones avanzadas**, seleccione **Variables de entorno**.

    + En **Variables del sistema**, seleccione **Nueva** para crear R_HOME.
    Para modificarla, seleccione **Editar**. Modifique su valor para que apunte a la ruta de instalación personalizada de R.

    ![Crear la variable de entorno del sistema R_HOME.](../install/media/sys-env-r-home.png)

2. Actualice la variable de entorno **PATH**.
    Anexe la ruta de acceso a **R.dll** a la variable de entorno del sistema **PATH**. Seleccione **PATH**, seleccione **Editar** y agregue `%R_HOME%\bin\x64` a la lista de rutas de acceso.

    ![Anexar a la variable de entorno del sistema PATH.](../install/media/sys-env-path-r.png)

3. Seleccione **Aceptar** para cerrar las ventanas restantes.

    Como alternativa, para establecer estas variables de entorno desde un símbolo del sistema con privilegios *elevados*, ejecute los siguientes comandos. Asegúrese de usar la ruta de instalación personalizada de R.

```CMD
setx /m R_HOME "path\to\installation\of\R"
setx /m PATH "path\to\installation\of\R\bin\x64;%PATH%"
```

## <a name="grant-access-to-the-custom-r-installation-folder"></a>Concesión de acceso a la carpeta de instalación personalizada de R

> [!NOTE]
>Si ha instalado R en la ubicación predeterminada **C:\Archivos de programa\R\R-version**, puede omitir este paso.

Ejecute los comandos **icacls** desde un nuevo símbolo del sistema con privilegios *elevados* para conceder acceso de lectura y ejecución al **nombre de usuario del servicio SQL Server Launchpad** y SID **S-1-15-2-1** (**Todos los paquetes de aplicación**). El nombre de usuario del servicio Launchpad tiene el formato `NT Service\MSSQLLAUNCHPAD$INSTANCENAME`, donde `INSTANCENAME` es el nombre de la instancia de SQL Server.

Estos comandos concederán acceso de forma recursiva a todos los archivos y carpetas en la ruta de acceso de directorio especificada.

Anexe el nombre de la instancia a `MSSQLLAUNCHPAD` (`MSSQLLAUNCHPAD$INSTANCENAME`). En este ejemplo, `INSTANCENAME` es la instancia predeterminada `MSSQLSERVER`.

1. Conceda permisos al **nombre de usuario del servicio SQL Server Launchpad**.

    ```cmd
    icacls "%R_HOME%" /grant "NT Service\MSSQLLAUNCHPAD$MSSQLSERVER":(OI)(CI)RX /T
    ```
2. Conceda permisos al **SID S-1-15-2-1**.

    ```cmd
    icacls "%R_HOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T

>[!NOTE]
>The preceding command grants permissions to the computer **SID S-1-15-2-1**, which is equivalent to ALL APPLICATION PACKAGES on an English version of Windows. Alternatively, you can use `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` on an English version of Windows.

## Restart SQL Server Launchpad service

From an *elevated* command prompt, run the following commands. Replace "MSSQLSERVER" with the name of your SQL Server instance.


```CMD
net stop MSSQLLAUNCHPAD$MSSQLSERVER
net start MSSQLLAUNCHPAD$MSSQLSERVER
```

Como alternativa, haga clic con el botón derecho en el servicio SQL Server Launchpad en la aplicación **Servicios** del sistema y seleccione el comando **Reiniciar**. O bien, utilice el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md) para reiniciar el servicio.

## <a name="download-r-language-extension"></a>Descarga de la extensión del lenguaje R

Descargue el [archivo ZIP que contiene la extensión del lenguaje R para Windows](https://github.com/microsoft/sql-server-language-extensions/releases). Se recomienda usar la versión de lanzamiento en producción. Use la versión de depuración en desarrollo o pruebas, ya que proporciona información de registro detallada para investigar los errores.

## <a name="register-external-language"></a>Registrar lenguaje externo

Registre esta extensión del lenguaje R con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) para cada base de datos en la que desea usarla. Use [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) para conectarse a SQL Server y ejecute el siguiente comando T-SQL.
Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP de la extensión de lenguaje descargado (R-lang-extension.zip).

> [!NOTE]
>**R** es una palabra reservada. Use un nombre diferente para el lenguaje externo, por ejemplo, "myR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.dll');
GO
```

::: moniker-end

::: moniker range=">=sql-server-linux-ver15"

Puede instalar SQL Server en Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) y Ubuntu. Para obtener más información, vea [la sección Plataformas admitidas de las instrucciones de instalación de SQL Server en Linux](../../linux/sql-server-linux-setup.md#supportedplatforms).

> [!NOTE]
> En este artículo se describe cómo instalar un entorno de ejecución personalizado para R en Linux. Para instalar en Windows, consulte [Instalación de un entorno de ejecución personalizado de R para SQL Server en Windows](custom-runtime-r.md?view=sql-server-ver15&preserve-view=true).

## <a name="pre-install-checklist"></a>Lista de comprobación previa a la instalación

Antes de instalar un entorno de ejecución personalizado de R, instale lo siguiente:

+ [SQL Server 2019 para Linux (actualización acumulativa 3 o posterior)](../../linux/sql-server-linux-setup.md).
Antes de instalar SQL Server en Linux, debe configurar un repositorio de Microsoft. Para más información, consulte [Configuración de repositorios](../../linux/sql-server-linux-change-repo.md).

+ [Extensiones de lenguaje de SQL Server en Linux con el marco de extensibilidad](../../linux/sql-server-linux-setup-language-extensions-java.md).

+ [R versión 3.3 o posterior](https://cran.r-project.org/).

## <a name="add-sql-server-language-extensions-for-linux"></a>Adición de las extensiones de lenguaje de SQL Server para Linux

> [!NOTE]
> Si ha instalado Machine Learning Services en SQL Server 2019, el paquete **mssql-server-extensibility** para las extensiones de lenguaje ya está instalado y puede omitir este paso.

Las extensiones de lenguaje usan el marco de extensibilidad para ejecutar código externo. La ejecución del código está aislada de los procesos principales del motor, pero está totalmente integrada con la ejecución de consultas de SQL Server.

Use los siguientes comandos para instalar las extensiones de lenguaje, en función de la versión de Linux.

### <a name="ubuntu"></a>Ubuntu
> [!Tip]
> Si es posible, ejecute `sudo apt-get update` para actualizar los paquetes en el sistema antes de la instalación. Ubuntu podría no tener la opción de transporte de apt para https. Para instalarla, use `sudo apt-get install apt-transport-https`.

```bash
# Install as root or sudo
sudo apt-get install mssql-server-extensibility
```

### <a name="red-hat"></a>Red Hat
```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

### <a name="suse"></a>Suse
```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Instalar R

>[!NOTE]
>For SQL Server Machine Learning Services, R ya está instalado en `/opt/microsoft/ropen/3.5.2/lib64/R`. Si desea seguir usando esta ruta de acceso como R_HOME, vaya al siguiente paso, **Instalación de Rcpp**. 

Si desea usar otro entorno de ejecución de R, primero debe eliminar `microsoft-r-open-mro` antes de continuar con la instalación de una nueva versión. Ejemplo para Ubuntu:

```bash
sudo apt remove microsoft-r-open-mro-3.5.2
```

Siga las [instrucciones](https://cran.r-project.org/bin/linux/) para completar la instalación de R (3.3 o posterior) para la plataforma de Linux correspondiente. De forma predeterminada, R se instala en **/usr/lib/R**. Esta ruta de acceso es **R_HOME**. Si instala R en una ubicación diferente, tome nota de la ruta de acceso como R_HOME.

Instrucciones de ejemplo para Ubuntu. Cambie la dirección URL del repositorio siguiente para su versión de R.

```bash
export DEBIAN_FRONTEND=noninteractive
sudo apt-get update
sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6

# Add R CRAN repository. This repository works for R 4.0.x.
#
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
sudo apt-get update

# Install R runtime.
#
sudo apt-get -y install r-base-core
```

## <a name="install-rcpp-package"></a>Instalación del paquete Rcpp

En las siguientes instrucciones, ${R_HOME} es la ruta de acceso a la instalación de R. 

+ Busque el archivo binario de R en ${R_HOME}/bin. De forma predeterminada, se encuentra en **/usr/lib/R/bin**.

+ Iniciar R

  ```bash
  sudo ${R_HOME}/bin/R
  ```

+ En este símbolo del sistema de R con privilegios *elevados* (${R_HOME}/bin/R), ejecute el siguiente script para instalar el paquete **Rcpp** en la carpeta ${R_HOME}/library.

  ```R
  install.packages("Rcpp", lib = "${R_HOME}/library");
  ```

## <a name="using-a-custom-installation-of-r"></a>Uso de una instalación personalizada de R

> [!NOTE]
>Si ha instalado R en la ubicación predeterminada **/usr/lib/R**, puede omitir esta sección.

### <a name="update-the-environment-variables"></a>Actualización de las variables de entorno

1. Edite el servicio Microsoft mssql-launchpadd para agregar la variable de entorno R_HOME al archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

    + Inserte el texto siguiente en el archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` que se abre. Establezca el valor de R_HOME en la ruta de instalación personalizada de R.

    ```text
    [Service]
    Environment="R_HOME=/path/to/installation/of/R"
    ```

    + Guarde y cierre el archivo.

2. Asegúrese de que se puede cargar **libR.so**.

    + Cree el archivo custom-r.conf en **/etc/ld.so.conf.d**.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-r.conf
    ```

    + En el archivo que se abre, agregue la ruta de acceso a **libR.so** desde la instalación personalizada de R.

    ```vi editor
    /path/to/installation/of/R/lib
    ```

    + Guarde y cierre el nuevo archivo.

    + Ejecute `ldconfig` y compruebe que se puede cargar **libR.so**; para ello, ejecute el comando siguiente y compruebe que se pueden encontrar todas las bibliotecas dependientes.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/R/lib/libR.so
    ```

### <a name="grant-access-to-the-custom-r-installation-folder"></a>Concesión de acceso a la carpeta de instalación personalizada de R

Establezca la opción `datadirectories` de la sección de extensibilidad del archivo /var/opt/mssql/mssql.conf en la instalación personalizada de R.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/R
```

### <a name="restart-mssql-launchpadd-service"></a>Reinicio del servicio mssql-launchpadd

> [!NOTE]
>Si ha instalado R en la ubicación predeterminada **/usr/lib/R**, puede omitir este paso.

```bash
sudo systemctl restart mssql-launchpadd
```

## <a name="download-r-language-extension"></a>Descarga de la extensión del lenguaje R

Descargue el [archivo ZIP que contiene la extensión del lenguaje R para Linux](https://github.com/microsoft/sql-server-language-extensions/releases). Se recomienda usar la versión de lanzamiento en producción. Use la versión de depuración en desarrollo o pruebas, ya que proporciona información de registro detallada para investigar los errores.

## <a name="register-external-language"></a>Registrar lenguaje externo

Registre esta extensión del lenguaje R con [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md) para cada base de datos en la que desea usarla. Use [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio) para conectarse a SQL Server y ejecute el siguiente comando T-SQL. 
Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP de la extensión de lenguaje descargado (r-lang-extension.zip).


> [!NOTE]
>**R** es una palabra reservada. Use un nombre diferente para el lenguaje externo, por ejemplo, "myR".

```sql
CREATE EXTERNAL LANGUAGE [myR]
FROM (CONTENT = N'/path/to/R-lang-extension.zip', FILE_NAME = 'libRExtension.so.1.0');
GO
```

::: moniker-end

## <a name="enable-external-script-execution-in-sql-server"></a>Habilitación de la ejecución de scripts externos en SQL Server

Se puede ejecutar un script externo en R mediante la ejecución del procedimiento almacenado [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) en SQL Server. 

Para habilitar los scripts externos, ejecute los siguientes comandos SQL con [Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/download-azure-data-studio), conectado a SQL Server.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="verify-language-extension-installation"></a>Comprobación de la instalación de la extensión de lenguaje

Este script de SQL comprueba la instalación correcta de la extensión personalizada del lenguaje R. La salida de este script debe mostrar R_HOME, la ruta de acceso a R y la versión del entorno de ejecución personalizado de R. Confirma que el script utiliza el entorno de ejecución personalizado.

```sql
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(R.home());
print(file.path(R.home("bin"), "R"));
print(R.version);
print("Hello RExtension!");'
```

## <a name="verify-parameters-and-datasets-of-different-data-types"></a>Comprobación de los parámetros y los conjuntos de datos de diferentes tipos de datos

Este script prueba diferentes tipos de datos para los parámetros de entrada y salida y los conjuntos de datos.

```sql
DECLARE @sumVal INT = 12;
DECLARE @charVal VARCHAR(30) = N'Hello';
EXEC sp_execute_external_script
    @language =N'myR',
    @script=N'
print(sumVal);
print(charVal);
sumVal <- sumVal + 300;
OutputDataSet <- InputDataSet;'
    ,@input_data_1 = N'select 1, cast(1.4 as real), ''Hi'', cast(''1'' as bit)'
    ,@params = N'@sumVal int OUTPUT, @charVal VARCHAR(30)'
    ,@sumVal = @sumVal OUTPUT
    ,@charVal =  @charVal
WITH RESULT SETS ((intCol INT, doubleCol REAL, charCol CHAR(2), logicalCol BIT));
PRINT @sumVal;
```

## <a name="see-also"></a>Consulte también

+ [Marco de extensibilidad en SQL Server](../concepts/extensibility-framework.md)
+ [Introducción a las extensiones de lenguaje](../../language-extensions/language-extensions-overview.md)