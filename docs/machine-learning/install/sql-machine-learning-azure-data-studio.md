---
title: Cuadernos de Azure Data Studio (Python, R)
description: Obtenga información sobre cómo ejecutar scripts de Python y R en un cuaderno en Azure Data Studio con Machine Learning Services de SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/09/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: b1e3d81e0229d028b9fab6a4521ee11cb20ce1b8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091558"
---
# <a name="run-python-and-r-scripts-in-azure-data-studio-notebooks-with-sql-server-machine-learning-services"></a>Ejecución de scripts de Python y R en cuadernos de Azure Data Studio con Machine Learning Services de SQL Server
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

Obtenga información sobre cómo ejecutar scripts de Python y R en cuadernos de [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) con [Machine Learning Services de SQL Server](../sql-server-machine-learning-services.md). Azure Data Studio es una herramienta de base de datos multiplataforma.

## <a name="prerequisites"></a>Prerrequisitos

- [Descargue e instale Azure Data Studio](../../azure-data-studio/download-azure-data-studio.md) en el equipo de la estación de trabajo. Azure Data Studio es multiplataforma y se ejecuta en Windows, macOS y Linux.

- Se necesita un servidor con Machine Learning Services de SQL Server instalado y habilitado. Se puede usar Machine Learning Services en Windows, Linux o clústeres de macrodatos:

  - [Instale Machine Learning Services de SQL Server en Windows](sql-machine-learning-services-windows-install.md).
  - [Instale Machine Learning Services de SQL Server en Linux](../../linux/sql-server-linux-setup-machine-learning.md).
  - [Ejecute los scripts de Python y R con Machine Learning Services en clústeres de macrodatos de SQL Server](../../big-data-cluster/machine-learning-services.md).

## <a name="create-a-sql-notebook"></a>Creación de un cuaderno de SQL

> [!IMPORTANT]
> Machine Learning Services se ejecuta como parte de SQL Server. Por lo tanto, debe usar un kernel de SQL, no uno de Python.

Puede usar Machine Learning Services en Azure Data Studio con un cuaderno de SQL. Para crear un cuaderno nuevo, siga estos pasos:

1. Haga clic en **Archivo** y **Nuevo cuaderno** para crear un cuaderno nuevo. De forma predeterminada, el cuaderno usará el **kernel de SQL**.

1. Haga clic en **Adjuntar a** y **Cambiar conexión**. 

    > [!div class="mx-imgBorder"]
    > ![Cambio de conexión en un cuaderno SQL en Azure Data Studio](media/ads-attach-to-connection.png)
    
1. Conéctese a un servidor de SQL Server nuevo o existente. Puede:

    1. Elegir una conexión existente en **Conexiones recientes** o **Conexiones guardadas**.

    1. Crear una conexión nueva en **Detalles de conexión**. Rellene los detalles de la conexión en el servidor de SQL Server y la base de datos.

    > [!div class="mx-imgBorder"]
    > ![Detalles de la conexión en un cuaderno SQL en Azure Data Studio](media/ads-connection-details.png)  

## <a name="run-python-or-r-scripts"></a>Ejecución de scripts de Python o R

Los cuadernos de SQL se componen de celdas de texto y código. Las celdas de código se usan para ejecutar scripts de Python o R mediante el procedimiento almacenado [sp_execute_external_scripts](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md). Las celdas de texto se pueden usar para documentar el código en el cuaderno.

### <a name="run-a-python-script"></a>Ejecutar un script de Python

Para ejecutar un script de Python, siga estos pasos:

1. Haga clic en **+ Código** para agregar una celda de código.

    > [!div class="mx-imgBorder"]
    > ![Adición de bloque de código en cuadernos de SQL en Azure Data Studio](media/ads-add-code.png)  

1. En la celda de código, escriba el script siguiente:

    ```sql
    EXECUTE sp_execute_external_script @language = N'Python'
        , @script = N'
    a = 1
    b = 2
    c = a/b
    d = a*b
    print(c, d)
    '
    ```

1. Haga clic en **Ejecutar celda** (flecha redonda de color negro) o presione **F5** para ejecutar la única celda.

    > [!div class="mx-imgBorder"]
    > ![Ejecución de código de Python en cuadernos de SQL en Azure Data Studio](media/ads-run-python.png)  

1. El resultado se mostrará en la celda de código.

    > [!div class="mx-imgBorder"]
    > ![Salida de código de Python en cuadernos de SQL en Azure Data Studio](media/ads-run-python-output.png)  

### <a name="run-an-r-script"></a>Ejecución de un script de R

Para ejecutar un script de R, siga estos pasos:

1. Haga clic en **+ Código** para agregar una celda de código.

    > [!div class="mx-imgBorder"]
    > ![Adición de bloque de código en cuadernos de SQL en Azure Data Studio](media/ads-add-code.png)  

1. En la celda de código, escriba el script siguiente:

    ```sql
    EXECUTE sp_execute_external_script @language = N'R'
        , @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

1. Haga clic en **Ejecutar celda** (flecha redonda de color negro) o presione **F5** para ejecutar la única celda.

    > [!div class="mx-imgBorder"]
    > ![Ejecución de código de R en cuadernos de SQL en Azure Data Studio](media/ads-run-r.png)  

1. El resultado se mostrará en la celda de código.

    > [!div class="mx-imgBorder"]
    > ![Salida de código de R en cuadernos de SQL en Azure Data Studio](media/ads-run-r-output.png)  

## <a name="next-steps"></a>Pasos siguientes

- [Uso de cuadernos en Azure Data Studio](../../azure-data-studio/notebooks/notebooks-guidance.md)
- [Creación y ejecución de un cuaderno de SQL Server](../../azure-data-studio/notebooks/notebooks-sql-kernel.md)
- [Inicio rápido: Ejecución de scripts de Python sencillos con Machine Learning Services de SQL Server](../tutorials/quickstart-python-create-script.md)
- [Inicio rápido: Ejecución de scripts de R sencillos con Machine Learning Services de SQL Server](../tutorials/quickstart-r-create-script.md)