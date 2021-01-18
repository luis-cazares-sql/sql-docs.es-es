---
title: Instalación desde un símbolo del sistema
description: Ejecute la instalación de línea de comandos de SQL Server para agregar Machine Learning Services con Python y R a una instancia del motor de base de datos de SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 01/07/2021
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9b0bf3444ab09cbbbb4aa8eca225dfa9964898c3
ms.sourcegitcommit: d681796e8c012eca2d9629d3b816749e9f50f868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98005330"
---
# <a name="install-sql-server-machine-learning-services-with-r-and-python-from-the-command-line"></a>Instalación de SQL Server Machine Learning Services con R y Python mediante la línea de comandos
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se proporcionan instrucciones para instalar [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) con Python y R mediante una línea de comandos.

Puede especificar la interacción silenciosa, básica o completa con la interfaz de usuario del programa de instalación. Este contenido complementa al artículo [Instalar SQL Server desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), el cual abarca los parámetros exclusivos de los componentes de aprendizaje automático de R y Python.

## <a name="pre-install-checklist"></a>Lista de comprobación previa a la instalación

+ Ejecute los comandos desde un símbolo del sistema con privilegios elevados. 

+ Se requiere una instancia del motor de base de datos para las instalaciones en base de datos. No se pueden instalar características solo de R o de Python, aunque se pueden [agregar incrementalmente a una instancia existente](#add-existing). Si solo quiere R y Python sin el motor de base de datos, instale el [servidor independiente](#shared-feature).

+ No realice la instalación en un clúster de conmutación por error. El mecanismo de seguridad que se usa para aislar los procesos de R y Python no es compatible con un entorno de clúster de conmutación por error de Windows Server.

+ No realice la instalación en un controlador de dominio. Se producirá un error en la parte de la instalación de Machine Learning Services.

+ Evite instalar instancias independientes y en bases de datos en el mismo equipo. Un servidor independiente competirá por los mismos recursos, lo que afectará al rendimiento de ambas instalaciones.

## <a name="command-line-arguments"></a>Argumentos de la línea de comandos

El argumento **/FEATURES** es obligatorio, igual que los contratos de términos de licencia. 

Al realizar la instalación a través del símbolo del sistema, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite el modo totalmente silencioso mediante el uso del parámetro **/Q** o el modo silencioso sencillo mediante el parámetro **/QS**. El modificador **/QS** solamente muestra el progreso, pero no acepta ninguna entrada ni muestra mensajes de error si los encuentra. El parámetro **/QS** solamente se admite cuando se ha especificado **/Action=install**.

::: moniker range="=sql-server-2016"
| Argumentos | Descripción |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Instala la versión en la base de datos: SQL Server R Services (en base de datos).  |
| /FEATURES = SQL_SHARED_MR | Instala la característica de R para la versión independiente: SQL Server R Server (independiente). Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos.|
| /IACCEPTROPENLICENSETERMS  | Indica que ha aceptado los términos de licencia para usar los componentes de R de código abierto. |
| /IACCEPTPYTHONLICENSETERMS | Indica que ha aceptado los términos de licencia para usar los componentes de Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica que ha aceptado los términos de licencia para usar SQL Server.|
| /MRCACHEDIRECTORY | Para la instalación sin conexión, establece la carpeta que contiene los archivos .cab de los componentes de R. |
::: moniker-end

::: moniker range="=sql-server-2017"
| Argumentos | Descripción |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Instala la versión en la base de datos: SQL Server Machine Learning Services (en base de datos).  |
| /FEATURES = SQL_INST_MR | Combine esto con AdvancedAnalytics. Instala la característica de R (en base de datos), incluidos Microsoft R Open y los paquetes de propiedad de R. |
| /FEATURES = SQL_INST_MPY | Combine esto con AdvancedAnalytics. Instala la característica de R (en base de datos), incluidos Anaconda y los paquetes de propiedad de Python. |
| /FEATURES = SQL_SHARED_MR | Instala la característica de R para la versión independiente: SQL Server Machine Learning Server (independiente). Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos.|
| /FEATURES = SQL_SHARED_MPY | Instala la característica de Python para la versión independiente: SQL Server Machine Learning Server (independiente). Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos.|
| /IACCEPTROPENLICENSETERMS  | Indica que ha aceptado los términos de licencia para usar los componentes de R de código abierto. |
| /IACCEPTPYTHONLICENSETERMS | Indica que ha aceptado los términos de licencia para usar los componentes de Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica que ha aceptado los términos de licencia para usar SQL Server.|
| /MRCACHEDIRECTORY | Para la instalación sin conexión, establece la carpeta que contiene los archivos .cab de los componentes de R. |
| /MPYCACHEDIRECTORY | Reservado para uso futuro. Use %TEMP% para almacenar archivos .cab de los componentes de Python para su instalación en equipos que no tienen conexión a Internet. |
::: moniker-end

::: moniker range=">=sql-server-ver15"
| Argumentos | Descripción |
|-----------|-------------|
| /FEATURES = AdvancedAnalytics | Instala la versión en la base de datos: SQL Server Machine Learning Services (en base de datos).  |
| /FEATURES = SQL_INST_MR | Combine esto con AdvancedAnalytics. Instala la característica de R (en base de datos), incluidos Microsoft R Open y los paquetes de propiedad de R. |
| /FEATURES = SQL_INST_MPY | Combine esto con AdvancedAnalytics. Instala la característica de R (en base de datos), incluidos Anaconda y los paquetes de propiedad de Python. |
| /FEATURES = SQL_INST_MJAVA | Combine esto con AdvancedAnalytics. Instala la característica de Java (en base de datos), incluido Open JRE. Se aplica a la [extensión de lenguaje Java de SQL Server](../../language-extensions/install/windows-java.md).|
| /FEATURES = SQL_SHARED_MR | Instala la característica de R para la versión independiente: SQL Server Machine Learning Server (independiente). Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos.|
| /FEATURES = SQL_SHARED_MPY | Instala la característica de Python para la versión independiente: SQL Server Machine Learning Server (independiente). Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos.|
| /IACCEPTROPENLICENSETERMS  | Indica que ha aceptado los términos de licencia para usar los componentes de R de código abierto. |
| /IACCEPTPYTHONLICENSETERMS | Indica que ha aceptado los términos de licencia para usar los componentes de Python. |
| /IACCEPTSQLSERVERLICENSETERMS | Indica que ha aceptado los términos de licencia para usar SQL Server.|
| /MRCACHEDIRECTORY | Para la instalación sin conexión, establece la carpeta que contiene los archivos .cab de los componentes de R. |
| /MPYCACHEDIRECTORY | Reservado para uso futuro. Use %TEMP% para almacenar archivos .cab de los componentes de Python para su instalación en equipos que no tienen conexión a Internet. |
::: moniker-end

## <a name="in-database-instance-installations"></a><a name="indb"></a> Instalaciones de instancias en bases de datos

El análisis en bases de datos está disponible para las instancias del motor de base de datos, y es necesario para agregar la característica **AdvancedAnalytics** a la instalación. Puede instalar una instancia del motor de base de datos con análisis avanzado o [agregarla a una instancia existente](#add-existing). 

Para ver la información de progreso sin los mensajes interactivos en pantalla, use el argumento /qs.

> [!IMPORTANT]
> Después de la instalación, quedan dos pasos de configuración más. La integración no está completa hasta que no se realizan estas tareas. Consulte [Tareas posteriores a la instalación](#post-install) para obtener instrucciones.

::: moniker range=">=sql-server-2017"
### <a name="sql-server-machine-learning-services-database-engine-advanced-analytics-with-python-and-r"></a>SQL Server Machine Learning Services: motor de base de datos y análisis avanzado con Python y R

Para una instalación simultánea de la instancia del motor de base de datos, proporcione el nombre de la instancia y las credenciales de inicio de sesión de un administrador (Windows). Incluye características para la instalación de componentes principales y de lenguaje, así como la aceptación de todos los términos de licencia.

```cmd
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Se trata del mismo comando, pero con un inicio de sesión de SQL Server en un motor de base de datos mediante el uso de autenticación mixta.

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY
/INSTANCENAME=MSSQLSERVER /SECURITYMODE=SQL /SAPWD="%password%" /SQLSYSADMINACCOUNTS="<sql-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS
```

Este ejemplo solo se aplica a Python, y muestra que se puede agregar un lenguaje si se omite una característica.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTPYTHONLICENSETERMS
```
::: moniker-end

::: moniker range="=sql-server-2016"
### <a name="sql-server-r-services-database-engine-and-advanced-analytics-with-r"></a>SQL Server R Services: motor de base de datos y análisis avanzado con R

Para una instalación simultánea de la instancia del motor de base de datos, proporcione el nombre de la instancia y las credenciales de inicio de sesión de un administrador (Windows). Incluye características para la instalación de componentes principales y de lenguaje, así como la aceptación de todos los términos de licencia.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<Windows-username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS 
```
::: moniker-end

## <a name="post-install-configuration-required"></a><a name="post-install"></a> Configuración posterior a la instalación (obligatoria)

Solo se aplica a las instalaciones en la base de datos.

Una vez finalizada la instalación, tiene una instancia del motor de base de datos con R y Python, paquetes de Microsoft de R y Python, Microsoft R Open, Anaconda, herramientas, ejemplos y scripts que forman parte de la distribución. 

Se necesitan realizar dos pasos más para completar la instalación:


::: moniker range=">=sql-server-2017"
1. Reinicie el servicio de motor de base de datos.

1. SQL Server Machine Learning Services: Habilite los scripts externos para poder usar la característica. El siguiente paso es seguir las instrucciones de [Instalación de SQL Server Machine Learning Services (en bases de datos)](sql-machine-learning-services-windows-install.md). 
::: moniker-end

::: moniker range="=sql-server-2016"
1. Reinicie el servicio de motor de base de datos.

1. SQL Server R Services: Habilite los scripts externos para poder usar la característica. El siguiente paso es seguir las instrucciones de [Instalación de SQL Server R Services (en bases de datos)](sql-r-services-windows-install.md). 
::: moniker-end

## <a name="add-advanced-analytics-to-an-existing-database-engine-instance"></a><a name="add-existing"></a> Adición de análisis avanzado a una instancia existente del motor de base de datos

Cuando agregue análisis avanzado en base de datos a una instancia existente del motor de base de datos, proporcione el nombre de la instancia. Por ejemplo, si ha instalado anteriormente un motor de base de datos SQL Server 2017 (o una versión posterior) y Python, podría usar este comando para agregar R.

```cmd  
Setup.exe /qs /ACTION=Install /FEATURES=SQL_INST_MR /INSTANCENAME=MSSQLSERVER 
/IACCEPTSQLSERVERLICENSETERMS  /IACCEPTROPENLICENSETERMS
```

## <a name="silent-install"></a><a name="silent"></a> Instalación silenciosa

Una instalación silenciosa suprime la comprobación de las ubicaciones de los archivos .cab. Por este motivo, es necesario que especifique la ubicación donde se van a desempaquetar los archivos .cab. En el caso de Python, los archivos .cab deben estar ubicados en %TEMP*. Para R, puede establecer la ruta de acceso de la carpeta con el directorio temporal.
 
```cmd  
Setup.exe /q /ACTION=Install /FEATURES=SQLEngine,ADVANCEDANALYTICS,SQL_INST_MR,SQL_INST_MPY 
/INSTANCENAME=MSSQLSERVER /SQLSYSADMINACCOUNTS="<username>" 
/IACCEPTSQLSERVERLICENSETERMS /IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS 
/MRCACHEDIRECTORY=%temp% 
```

## <a name="standalone-server-installations"></a><a name="shared-feature"></a> Instalaciones de servidor independiente

Un servidor independiente es una "característica compartida" que no está enlazada a una instancia del motor de base de datos. En los siguientes ejemplos se muestra la sintaxis válida para la instalación del servidor independiente.

::: moniker range=">=sql-server-2017"
SQL Server Machine Learning Server admite Python y R en un servidor independiente:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR,SQL_SHARED_MPY  
/IACCEPTROPENLICENSETERMS /IACCEPTPYTHONLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end
::: moniker range="=sql-server-2016"
SQL Server R Server solo admite R:

```cmd
Setup.exe /q /ACTION=Install /FEATURES=SQL_SHARED_MR 
/IACCEPTROPENLICENSETERMS /IACCEPTSQLSERVERLICENSETERMS
```
::: moniker-end

Una vez finalizada la instalación, tiene un servidor, paquetes de Microsoft, distribuciones de código abierto de R y Python, herramientas, ejemplos y scripts que forman parte de la distribución. 

Para abrir una ventana de la consola de R, vaya a `\Program files\Microsoft SQL Server\150 (or 140/130)\R_SERVER\bin\x64` y haga doble clic en **RGui.exe**. ¿No está familiarizado con R? Pruebe este tutorial: [Comandos de R y funciones de RevoScaleR básicos: 25 ejemplos comunes](/machine-learning-server/r/tutorial-r-to-revoscaler).

Para abrir un comando de Python, vaya a `\Program files\Microsoft SQL Server\150 (or 140)\PYTHON_SERVER\bin\x64` y haga doble clic en **python.exe**.

## <a name="next-steps"></a>Pasos siguientes

Los desarrolladores de Python pueden aprender a usar Python con SQL Server con estos tutoriales:

+ [Tutorial de Python: Predicción de alquileres de esquíes con regresión lineal en SQL Server Machine Learning Services](../tutorials/python-ski-rental-linear-regression-deploy-model.md)
+ [Tutorial de Python: Clasificación de clientes por categorías mediante la agrupación en clústeres k-means con SQL Server Machine Learning Services](../tutorials/python-clustering-model.md)

Los desarrolladores de R pueden empezar con algunos ejemplos sencillos y conocer los aspectos básicos del funcionamiento de R con SQL Server. Para conocer el siguiente paso, vea los vínculos siguientes:

+ [Inicio rápido: Ejecutar R en T-SQL](../tutorials/quickstart-r-create-script.md)
+ [Tutorial: Análisis en base de datos para desarrolladores de R](../tutorials/r-taxi-classification-introduction.md)
