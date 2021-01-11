---
title: Instalación de un entorno de ejecución personalizado de Python
description: Aprenda a instalar un entorno de ejecución personalizado de Python para SQL Server mediante Extensiones de lenguaje. El entorno de ejecución personalizado de Python se puede usar para el aprendizaje automático.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
zone_pivot_groups: python-custom-runtime-platform
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 91aace4333b4496338b782344e64cdfea2b886bd
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804312"
---
# <a name="install-a-python-custom-runtime-for-sql-server"></a>Instalación de un entorno de ejecución personalizado de Python para SQL Server
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe cómo instalar un entorno de ejecución personalizado de Python para ejecutar scripts externos de Python con SQL Server. El entorno de ejecución personalizado usa las [Extensiones de lenguaje de SQL Server](../../language-extensions/language-extensions-overview.md) y se puede usar para ejecutar scripts de aprendizaje automático.

El entorno de ejecución personalizado de Python le permite usar su propia versión del entorno de ejecución de Python con SQL Server en lugar de la versión del entorno de ejecución predeterminada instalada con [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md).

::: zone pivot="python-custom-runtime-windows"

## <a name="prerequisites"></a>Prerrequisitos

Antes de instalar un entorno de ejecución personalizado de Python, instale lo siguiente:

+ Instale la [actualización acumulativa (CU) 3 o posterior](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) para SQL Server 2019.

+ Instale [Python 3.7](https://www.python.org/downloads/) en el servidor.

    La extensión del lenguaje Python que se usa para el entorno de ejecución de Python actualmente admite solo Python 3.7. Si quiere usar una versión distinta de Python, siga la instrucción que aparece en el [repositorio de GitHub sobre Extensiones de lenguaje de Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) para modificar y recompilar la extensión.

    > [!IMPORTANT]
    > Durante la instalación de Python, compruebe **Agregar Python 3.7 a PATH**.

## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../sql-server-machine-learning-services.md) en SQL Server 2019, las Extensiones de lenguaje ya están instaladas y puede omitir este paso.

Siga los pasos que aparecen a continuación para instalar [Extensiones de lenguaje de SQL Server](../../language-extensions/language-extensions-overview.md), que se usa para el entorno de ejecución personalizado de Python.

1. Inicie el asistente para la instalación de SQL Server 2019.
  
1. En la pestaña **Instalación**, seleccione **Nueva instalación independiente de SQL Server o agregar características a una instalación existente**.

1. En la página **Selección de características** , seleccione estas opciones:
  
    + **Servicios de Motor de base de datos**
  
        Para usar las extensiones de lenguaje con SQL Server, debe instalar una instancia del motor de base de datos. Puede usar una instancia nueva o una existente.
  
    + **Machine Learning Services y extensiones de lenguaje**

        Seleccione **Machine Learning Services y extensiones de lenguaje**. No seleccione Python, ya que va a instalar el entorno de ejecución de Python personalizado más adelante.

        :::image type="content" source="media/2019-setup-language-extensions.png" alt-text="Instalación de Extensiones de lenguaje de SQL Server 2019.":::

1. En la página **Listo para instalar**, confirme que estas selecciones se han realizado y haga clic en **Instalar**.
  
    + Servicios de Motor de base de datos
    + Machine Learning Services y extensiones de lenguaje

1. Una vez que se completa la instalación, reinicie la máquina si se le pide hacerlo.

> [!IMPORTANT]
> Si instala una instancia nueva de SQL Server 2019 con Extensiones de lenguaje, instale la [actualización acumulativa (CU) 3 o posterior](../../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) antes de ir al paso siguiente.

## <a name="install-pandas"></a>Instalación de pandas

Instale el paquete para Python [pandas](https://pandas.pydata.org/) desde un símbolo del sistema con privilegios *elevados*:

```bash
python.exe -m pip install pandas
```

## <a name="add-environment-variable"></a>Incorporación de una variable de entorno

Agregue o modifique la variable de entorno del sistema **PYTHONHOME**.

1. En el cuadro de búsqueda de Windows, escriba *entorno* y seleccione **Editar las variables de entorno del sistema**.
1. En la pestaña **Opciones avanzadas**, seleccione **Variables de entorno**.
1. En **Variables del sistema**, seleccione **Nueva** para crear **PYTHONHOME** para que apunte a la ubicación de instalación de Python 3.7. Si PYTHONHOME ya existe, seleccione **Editar** para que apunte a la ubicación de instalación de Python 3.7.
1. Seleccione **Aceptar** para cerrar todas las ventanas.

    :::image type="content" source="media/pythonhome-env-variable.png" alt-text="Variable de entorno PYTHONHOME.":::

## <a name="grant-access-to-python-folder"></a>Concesión de acceso a la carpeta de Python

Ejecute los comandos **icacls** siguientes desde un símbolo del sistema con privilegios *elevados* nuevos para conocer a **PYTHONHOME** acceso de **LECTURA Y EJECUCIÓN** al **servicio SQL Server Launchpad** y SID **S-1-15-2-1** (**ALL_APPLICATION_PACKAGES**).

1. Conceda permisos al **nombre de usuario del servicio SQL Server Launchpad**.

    ```cmd
    icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD":(OI)(CI)RX /T
    ```

    En el caso de una instancia con nombre, el comando será `icacls "%PYTHONHOME%" /grant "NT Service\MSSQLLAUNCHPAD$SQL01":(OI)(CI)RX /T` para una instancia denominada **SQL01**.

2. Conceda permisos al **SID S-1-15-2-1**.

    ```cmd
    icacls "%PYTHONHOME%" /grant *S-1-15-2-1:(OI)(CI)RX /T
    ```

    El comando anterior concede permisos al SID del equipo **S-1-15-2-1**, que es equivalente a **TODOS LOS PAQUETES DE APLICACIONES** en una versión en inglés de Windows. Como alternativa, puede usar `icacls "%R_HOME%" /grant "ALL APPLICATION PACKAGES":(OI)(CI)RX /T` en una versión en inglés de Windows.

## <a name="restart-sql-server-launchpad"></a>Reinicio de SQL Server Launchpad

Siga estos pasos para reiniciar el servicio SQL Server Launchpad.

1. Abra el [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md).

1. En **Servicios de SQL Server**, haga clic con el botón derecho en **SQL Server Launchpad (MSSQLSERVER)** y seleccione **Reiniciar**. Si usa una instancia con nombre, se mostrará el nombre de la instancia en lugar de **(MSSQLSERVER)** .

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje Python, que se usa para el entorno de ejecución personalizado de Python.

1. Descargue el archivo **python-lang-extension-windows.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**python-lang-extension-windows-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje Python con [CREAR UN LENGUAJE EXTERNO](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP de la extensión del lenguaje descargado (**python-lang-extension-windows.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-windows.zip', FILE_NAME = 'pythonextension.dll');
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje Python.

    > [!NOTE]
    > **Python** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, la instrucción anterior usa **myPython**.

::: zone-end

::: zone pivot="python-custom-runtime-linux"

## <a name="prerequisites"></a>Prerrequisitos

Antes de instalar un entorno de ejecución personalizado de Python, instale lo siguiente:

+ Instale SQL Server 2019 para Linux. Puede instalar SQL Server en Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) y Ubuntu. Para más información, consulte la [guía de instalación de SQL Server en Linux](../../linux/sql-server-linux-setup.md).

+ Instale la actualización acumulativa (CU) 3 o posterior para SQL Server 2019. Siga estos pasos:
    1. Configure los repositorios para las actualizaciones acumulativas. Para más información, consulte [Configuración de repositorios para instalar y actualizar SQL Server en Linux](../../linux/sql-server-linux-change-repo.md).

    1. Actualice el paquete **mssql-server** a la actualización acumulativa más reciente. Para más información, consulte la [sección de actualización de SQL Server en la guía de instalación de SQL Server en Linux](../../linux/sql-server-linux-setup.md#upgrade).

+ Instale [Python 3.7](https://www.python.org/downloads/) en el servidor.

    La extensión del lenguaje Python que se usa para el entorno de ejecución de Python actualmente admite solo Python 3.7. Si quiere usar una versión distinta de Python, siga la instrucción que aparece en el [repositorio de GitHub sobre Extensiones de lenguaje de Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) para modificar y recompilar la extensión.

## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../sql-server-machine-learning-services.md) en SQL Server 2019, el paquete **mssql-server-extensibility** para las Extensiones de lenguaje ya está instalado y puede omitir este paso.

Siga los comandos que aparecen a continuación para instalar [Extensiones de lenguaje de SQL Server](../../language-extensions/language-extensions-overview.md) en Linux, que se usa para el entorno de ejecución personalizado de Python.

#### <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

1. Si es posible, ejecute este comando para actualizar los paquetes en el sistema antes de la instalación.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu podría no tener la opción de transporte de apt para https. Para realizar la instalación, ejecute este comando.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Instale **mssql-server-extensibility** con este comando.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

#### <a name="red-hat-enterprise-linux-rhel"></a>[Red Hat Enterprise Linux (RHEL)](#tab/rhel)

```bash
# Install as root or sudo
sudo yum install mssql-server-extensibility
```

#### <a name="suse-linux-enterprise-server-sles"></a>[SUSE Linux Enterprise Server (SLES)](#tab/sles)

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

---

## <a name="install-python-37-and-pandas"></a>Instalación de Python 3.7 y pandas

Instale Python 3.7, la biblioteca libpython3.7 y el paquete pandas. 

Los siguientes son comandos de ejemplo para Ubuntu:

```bash
# Install python3.7 and the corresponding library:
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.7 python3-pip libpython3.7

# Install pandas to /usr/lib:
sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
```

## <a name="custom-installation-of-python"></a>Instalación personalizada de Python

> [!NOTE]
> Si instaló Python 3.7 en la ubicación predeterminada de `/usr/lib/python3.7`, puede omitir esta sección y pasar a la sección [Registro de la extensión del lenguaje](#register-language-extension-linux).

Si compiló su propia versión de Python 3.7, use los comandos siguientes para que SQL Server sepa de su instalación personalizada.

### <a name="add-environment-variable"></a>Incorporación de una variable de entorno

Primero, edite el servicio **mssql-launchpadd** para agregar la variable de entorno **PYTHONHOME** al archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf`.

1. Abra el archivo con systemctl.

    ```bash
    sudo systemctl edit mssql-launchpadd
    ```

1. Inserte el texto siguiente en el archivo `/etc/systemd/system/mssql-launchpadd.service.d/override.conf` que se abre. Establezca el valor de **PYTHONHOME** en la ruta de instalación personalizada de Python.

    ```
    [Service]
    Environment="PYTHONHOME=/path/to/installation/of/python3.7"
    ```

1. Guarde el archivo y cierre el editor.

A continuación, asegúrese de que se puede cargar `libpython3.7m.so.1.0`.

1. Cree el archivo custom-python.conf en `/etc/ld.so.conf.d`.

    ```bash
    sudo vi /etc/ld.so.conf.d/custom-python.conf
    ```

1. En el archivo que se abre, agregue la ruta de acceso a **libpython3.7m.so.1.0** de la instalación personalizada de Python.

    ```
    /path/to/installation/of/python3.7/lib
    ```

1. Guarde el archivo nuevo y cierre el editor.

1. Ejecute `ldconfig` y compruebe que se puede cargar `libpython3.7m.so.1.0`; para ello, ejecute los comandos siguientes y compruebe que se pueden encontrar todas las bibliotecas dependientes.

    ```bash
    sudo ldconfig
    ldd /path/to/installation/of/python3.7/lib/libpython3.7m.so.1.0
    ```

### <a name="grant-access-to-python-folder"></a>Concesión de acceso a la carpeta de Python

Establezca la opción `datadirectories` de la sección de extensibilidad del archivo /var/opt/mssql/mssql.conf en la instalación personalizada de Python.

```bash
sudo /opt/mssql/bin/mssql-conf set extensibility.datadirectories /path/to/installation/of/python3.7
```

### <a name="restart-mssql-launchpadd"></a>Reinicio de mssql-launchpadd

```bash
sudo systemctl restart mssql-launchpadd
```

<a name="register-language-extension-linux"></a>

## <a name="register-language-extension"></a>Registro de la extensión del lenguaje

Siga estos pasos para descargar y registrar la extensión del lenguaje Python, que se usa para el entorno de ejecución personalizado de Python.

1. Descargue el archivo **python-lang-extension-linux.zip** del [repositorio de GitHub de Extensiones de lenguaje de SQL Server](https://github.com/microsoft/sql-server-language-extensions/releases).

    También puede usar la versión de depuración (**python-lang-extension-linux-debug.zip**) en un entorno de desarrollo o prueba. La versión de depuración proporciona información de registro detallada para investigar los errores y no se recomienda para los entornos de producción.

1. Use [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) para conectarse a la instancia de SQL Server y ejecute el comando de T-SQL siguiente para registrar la extensión del lenguaje Python con [CREAR UN LENGUAJE EXTERNO](../../t-sql/statements/create-external-language-transact-sql.md). 

    Modifique la ruta de acceso de esta instrucción para reflejar la ubicación del archivo ZIP de la extensión del lenguaje descargado (**python-lang-extension-linux.zip**).

    ```sql
    CREATE EXTERNAL LANGUAGE [myPython]
    FROM (CONTENT = N'/path/to/python-lang-extension-linux.zip', FILE_NAME = 'libPythonExtension.so.1.0');
    GO
    ```

    Ejecute la instrucción para cada base de datos en la que quiera usar la extensión del lenguaje Python.

    > [!NOTE]
    > **Python** es una palabra reservada y no se puede usar como nombre de un lenguaje externo nuevo. Use otro nombre. Por ejemplo, la instrucción anterior usa **myPython**.

::: zone-end

## <a name="enable-external-script"></a>Habilitación de scripts externos

Puede ejecutar un script externo de Python con el procedimiento almacenado [sp_execute_external script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

Si quiere habilitar los scripts externos, use [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) para ejecutar la instrucción siguiente.

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

## <a name="verify-installation"></a>Comprobación de la instalación

Use el script de SQL siguiente para comprobar la instalación y la funcionalidad del entorno de ejecución personalizado de Python.

```sql
EXEC sp_execute_external_script
@language =N'myPython',
@script=N'
import sys
print(sys.path)
print(sys.version)
print(sys.executable)'
```

## <a name="next-steps"></a>Pasos siguientes

+ [Instalación de un entorno de ejecución personalizado de R para SQL Server](custom-runtime-r.md)
+ [Marco de extensibilidad en SQL Server](../concepts/extensibility-framework.md)
+ [Introducción a las extensiones de lenguaje](../../language-extensions/language-extensions-overview.md)
