---
description: Azure Feature Pack para Integration Services (SSIS)
title: Azure Feature Pack para Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.SSIS.AZURE.F1
- SQL14.SSIS.AZURE.F1
ms.assetid: 31de555f-ae62-4f2f-a6a6-77fea1fa8189
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b22910f0a794925d07358fa73f3bd6d7a7c6e15a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091666"
---
# <a name="azure-feature-pack-for-integration-services-ssis"></a>Azure Feature Pack para Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


Feature Pack de SQL Server Integration Services (SSIS) para Azure es una extensión que proporciona los componentes que se muestran en esta página para que SSIS se conecte a los servicios de Azure, para transferir datos entre Azure y orígenes de datos locales, y para procesar los datos almacenados en Azure.

[![Descarga de Feature Pack de SIS para Azure](/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/download/details.aspx?id=100430) **Descargar**

- Para SQL Server 2019 - [Feature Pack de Microsoft SQL Server 2019 Integration Services para Azure](https://www.microsoft.com/download/details.aspx?id=100430)
- Para SQL Server 2017 - [Feature Pack de Microsoft SQL Server 2017 Integration Services para Azure](https://www.microsoft.com/download/details.aspx?id=54798)
- Para SQL Server 2016 - [Feature Pack de Microsoft SQL Server 2016 Integration Services para Azure](https://www.microsoft.com/download/details.aspx?id=49492)
- Para SQL Server 2014 - [Feature Pack de Microsoft SQL Server 2014 Integration Services para Azure](https://www.microsoft.com/download/details.aspx?id=47366)
- Para SQL Server 2012 - [Feature Pack de Microsoft SQL Server 2012 Integration Services para Azure](https://www.microsoft.com/download/details.aspx?id=47367)

En las páginas de descarga también se incluye información sobre los requisitos previos. Asegúrese de instalar SQL Server antes de instalar el Feature Pack de Azure en un servidor o es posible que los componentes del Feature Pack no estén disponibles al implementar los paquetes en la base de datos de catálogo de SSIS, SSISDB, en el servidor.

## <a name="components-in-the-feature-pack"></a>Componentes de Feature Pack
-   Administradores de conexión

    -   [Administrador de conexiones de Azure Data Lake Analytics](connection-manager/azure-data-lake-analytics-connection-manager.md)

    -   [Administrador de conexiones de Azure Data Lake Store](../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)
    
    -   [Administrador de conexiones de Azure HDInsight](../integration-services/connection-manager/azure-hdinsight-connection-manager.md)

    -   [Administrador de conexiones de Azure Resource Manager](../integration-services/connection-manager/azure-resource-manager-connection-manager.md)
    
    -   [Administrador de conexiones de Azure Storage](../integration-services/connection-manager/azure-storage-connection-manager.md)

    -   [Administrador de conexiones de suscripción de Azure](../integration-services/connection-manager/azure-subscription-connection-manager.md)
    
-   Tareas

    -   [Tarea de descarga de blobs de Azure](../integration-services/control-flow/azure-blob-download-task.md)

    -   [Tarea de carga de blobs de Azure](../integration-services/control-flow/azure-blob-upload-task.md)

    -   [Tarea de Azure Data Lake Analytics](control-flow/azure-data-lake-analytics-task.md)

    -   [Tarea Sistema de archivos de Azure Data Lake Store](../integration-services/control-flow/azure-data-lake-store-file-system-task.md)

    -   [Tarea de creación de clúster de HDInsight de Azure](../integration-services/control-flow/azure-hdinsight-create-cluster-task.md)

    -   [Tarea de eliminación de clúster de HDInsight de Azure](../integration-services/control-flow/azure-hdinsight-delete-cluster-task.md)
    
    -   [Tarea de Hive de Azure HDInsight](../integration-services/control-flow/azure-hdinsight-hive-task.md)

    -   [Tarea de Pig de Azure HDInsight](../integration-services/control-flow/azure-hdinsight-pig-task.md)

    -   [Tarea de carga de Azure SQL DW](../integration-services/control-flow/azure-sql-dw-upload-task.md)

    -   [Tarea de archivo flexible](../integration-services/control-flow/flexible-file-task.md)

-   Componentes de flujo de datos

    -   [Origen de blobs de Azure](../integration-services/data-flow/azure-blob-source.md)

    -   [Destino de blobs de Azure](../integration-services/data-flow/azure-blob-destination.md)
    
    -   [Origen de Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-source.md)
    
    -   [Destino de Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-destination.md)

    -   [Origen de archivo flexible](../integration-services/data-flow/flexible-file-source.md)

    -   [Destino de archivo flexible](../integration-services/data-flow/flexible-file-destination.md)

-   Enumerador de archivos para Blob de Azure, Azure Data Lake Store y Data Lake Storage Gen2. Consultar [Contenedor de bucles Para cada uno](../integration-services/control-flow/foreach-loop-container.md)

## <a name="use-tls-12"></a>Uso de TLS 1.2

La versión de TLS que usa Azure Feature Pack sigue la configuración de .NET Framework del sistema.
Para usar TLS 1.2, agregue un valor `REG_DWORD` denominado `SchUseStrongCrypto` con datos `1` en las dos claves del Registro siguientes.

1. `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319`
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319`

## <a name="dependency-on-java"></a>Dependencia en Java

Java es necesario para usar formatos de archivo ORC/Parquet con conectores de Azure Data Lake Store/archivos flexibles.  
La arquitectura (32 o 64 bits) de la compilación de Java debe coincidir con la del entorno de ejecución de SSIS que se va a usar.
Se han probado las siguientes compilaciones de Java.

- [OpenJDK 8u192 de Zulu](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Java SE Runtime Environment 8u192 de Oracle](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

### <a name="set-up-zulus-openjdk"></a>Configurar OpenJDK de Zulu

1. Descargue y extraiga el paquete ZIP de instalación.
2. Desde el símbolo del sistema, ejecute `sysdm.cpl`.
3. En la pestaña **Avanzadas**, haga clic en **Variables de entorno**.
4. En la sección **Variables del sistema**, haga clic en **Nueva**.
5. Escriba `JAVA_HOME` para el **Nombre de variable**.
6. Seleccione **Examinar directorio**, vaya a la carpeta extraída y seleccione la subcarpeta `jre`.
   Luego seleccione **Aceptar** y el campo **Valor de la variable** se rellena de forma automática.
7. Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Nueva variable del sistema**.
8. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Variables de entorno**.
9. Seleccione **Aceptar** para cerrar el cuadro de diálogo **Propiedades del sistema**.

> [!TIP]
> Si usa el formato de Parquet y recibe un error que dice que se ha producido un error al invocar Java, con el mensaje: **java.lang.OutOfMemoryError:Java heap space**, puede agregar una variable de entorno *`_JAVA_OPTIONS`* para ajustar el tamaño mínimo y máximo del montón de JVM.
>
>![montón de jvm](media/azure-feature-pack-jvm-heap-size.png)
>
> Ejemplo: establecimiento de la variable *`_JAVA_OPTIONS`* con el valor *`-Xms256m -Xmx16g`* . La marca Xms especifica el grupo de asignación de memoria inicial para una Máquina virtual Java (JVM), mientras que Xmx especifica el grupo de asignación de memoria máxima. Esto significa que JVM se iniciará con la cantidad de memoria *`Xms`* y podrá utilizar como máximo *`Xmx`* . Los valores predeterminados son 64 MB como mínimo y 1 G como máximo.

### <a name="set-up-zulus-openjdk-on-azure-ssis-integration-runtime"></a>Configuración de OpenJDK de Zulú en Azure-SSIS Integration Runtime

Esto debe hacerse a través de la [interfaz de instalación personalizada](/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) para Azure-SSIS Integration Runtime.
Supongamos que se usa `zulu8.33.0.1-jdk8.0.192-win_x64.zip`.
El contenedor de blobs se puede organizar de la manera siguiente:

~~~
main.cmd
install_openjdk.ps1
zulu8.33.0.1-jdk8.0.192-win_x64.zip
~~~

Como punto de entrada, `main.cmd` desencadena la ejecución del script `install_openjdk.ps1` de PowerShell que, a su vez, extrae `zulu8.33.0.1-jdk8.0.192-win_x64.zip` y establece `JAVA_HOME` en consecuencia.

**main.cmd**

~~~
powershell.exe -file install_openjdk.ps1
~~~

> [!TIP]
> Si usa el formato de Parquet y recibe un error que dice que se ha producido un error al invocar Java, con el mensaje: **java.lang.OutOfMemoryError:Java heap space**, puede agregar un comando en *`main.cmd`* para ajustar el tamaño mínimo y máximo del montón de JVM. Ejemplo:
> ~~~
> setx /M _JAVA_OPTIONS "-Xms256m -Xmx16g"
> ~~~
> La marca Xms especifica el grupo de asignación de memoria inicial para una Máquina virtual Java (JVM), mientras que Xmx especifica el grupo de asignación de memoria máxima. Esto significa que JVM se iniciará con la cantidad de memoria *`Xms`* y podrá utilizar como máximo *`Xmx`* . Los valores predeterminados son 64 MB como mínimo y 1 G como máximo.

**install_openjdk.ps1**

~~~
Expand-Archive zulu8.33.0.1-jdk8.0.192-win_x64.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\zulu8.33.0.1-jdk8.0.192-win_x64\jre", "Machine")
~~~

### <a name="set-up-oracles-java-se-runtime-environment"></a>Configurar Java SE Runtime Environment de Oracle

1. Descargue y ejecute el instalador .exe.
2. Siga las instrucciones del programa de instalación para completar la instalación.

## <a name="scenario-processing-big-data"></a>Escenario: Procesamiento de macrodatos
 Use el Conector Azure para completar el trabajo de procesamiento de macrodatos siguiente:

1.  Utilice la tarea de carga de blobs de Azure para cargar datos de entrada en el almacenamiento de blobs de Azure.

2.  Utilice la tarea de creación de clúster de HDInsight de Azure para crear un clúster de HDInsight de Azure. Este paso es opcional si quiere utilizar su propio clúster.

3.  Utilice la tarea de Hive de HDInsight de Azure o la tarea de Pig de HDInsight de Azure para invocar un trabajo de Pig o Hive en el clúster de HDInsight de Azure.

4.  Utilice la tarea de eliminación de clúster de HDInsight de Azure para eliminar el clúster de HDInsight tras su uso si creó un clúster de HDInsight a petición en el paso 2.

5.  Utilice la tarea de descarga de blobs de HDInsight de Azure para descargar los datos de salida de Pig o Hive del almacenamiento de blobs de Azure.

![Captura de pantalla en la que se muestra el escenario de macrodatos del conector de Azure en SSIS.](../integration-services/media/ssis-azureconnector-bigdatascenario.png)
 
## <a name="scenario-managing-data-in-the-cloud"></a>Escenario: Administración de datos en la nube
 Use el destino de blobs de Azure en un paquete de SSIS para escribir los datos de salida en Almacenamiento de blobs de Azure o para utilizar el origen de blobs de Azure para leer datos desde un Almacenamiento de blobs de Azure.

![Captura de pantalla en la que se muestra el flujo de datos desde el origen de OLE DB hasta el destino de blob de Azure.](../integration-services/media/ssis-azureconnector-cloudarchive-1.png)
 
 ![Captura de pantalla en la que se muestra el flujo de datos desde el origen de blob de Azure hasta el destino de OLE DB.](../integration-services/media/ssis-azureconnector-cloudarchive-2.png)

 Utilice el contenedor de bucles Foreach con el enumerador de blobs de Azure para procesar los datos de varios archivos de blob.

![Captura de pantalla en la que se muestra el contenedor de bucles Foreach de la pestaña Flujo de control.](../integration-services/media/ssis-azureconnector-cloudarchive-3.png)

## <a name="release-notes"></a>Notas de la versión

### <a name="version-1190"></a>Version 1.19.0

#### <a name="improvements"></a>Mejoras

1. Se ha agregado compatibilidad con la autenticación de firma de acceso compartido en el administrador de conexiones de Azure Storage.

### <a name="version-1180"></a>Versión 1.18.0

#### <a name="improvements"></a>Mejoras

1. En el caso de la tarea de archivo flexible, se han introducido tres mejoras: (1) se ha agregado compatibilidad con caracteres comodín para las operaciones de copia y eliminación; (2) el usuario puede habilitar o deshabilitar la búsqueda recursiva para la operación de eliminación; y (3) el nombre del archivo de destino para la operación de copia puede estar vacío para conservar el nombre del archivo de origen.

### <a name="version-1170"></a>Versión 1.17.0

Se trata de una versión de revisión publicada solo para SQL Server 2019.

#### <a name="bugfixes"></a>Correcciones

1. Al ejecutarse en Visual Studio 2019 y establecer como destino SQL Server 2019, podría producirse un error en la tarea, origen o destino del archivo flexible con el mensaje de error `Attempted to access an element as a type incompatible with the array.`.
1. Al ejecutarse en Visual Studio 2019 y establecer como destino SQL Server 2019, podría producirse un error en el origen o destino del archivo flexible mediante el formato ORC/Parquet con el mensaje de error `Microsoft.DataTransfer.Common.Shared.HybridDeliveryException: An unknown error occurred. JNI.JavaExceptionCheckException.`.

### <a name="version-1160"></a>Versión 1.16.0

#### <a name="bugfixes"></a>Correcciones

1. En algunos casos, la ejecución de paquetes informa de "Error: No se puede cargar el archivo o ensamblado 'Newtonsoft.Json, Version=11.0.0.0, Culture=neutral, PublicKeyToken=30ad4fe6b2a6aeed' ni una de sus dependencias".

### <a name="version-1150"></a>Versión 1.15.0

#### <a name="improvements"></a>Mejoras

1. Adición de la operación de eliminación de carpeta/archivo en la tarea de archivo flexible
1. Adición de la función de conversión de tipos de datos externos/de salida en el origen de archivo flexible

#### <a name="bugfixes"></a>Correcciones

1. En algunos casos, las conexiones de prueba no funcionan correctamente para Data Lake Storage Gen2 con el mensaje de error de tipo "Se intentó obtener acceso a un elemento como un tipo incompatible con la matriz".
1. Devolución del soporte técnico para el emulador de Azure Storage