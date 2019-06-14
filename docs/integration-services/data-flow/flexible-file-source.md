---
title: Origen de archivo flexible | Microsoft Docs
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.afpextfilesrc.f1
- sql14.dts.designer.afpextfilesrc.f1
author: janinezhang
ms.author: janinez
manager: craigg
ms.openlocfilehash: 3e396e40f30571969b347c464687321b3b038212
ms.sourcegitcommit: fa2afe8e6aec51e295f55f8cc6ad3e7c6b52e042
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/03/2019
ms.locfileid: "66462537"
---
# <a name="flexible-file-source"></a>Origen de archivo flexible

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]

El componente de **origen de archivo flexible** permite que un paquete SSIS lea datos en diversos servicios de almacenamiento compatibles.
Los servicios de almacenamiento admitidos actualmente son:

- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage Gen2](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction)
  
Para ver el editor del origen de archivo flexible, arrastre y coloque el **origen de archivo flexible** en el diseñador de flujos de datos y haga doble clic en él para abrir el editor.
  
[Tarea de archivo flexible](../../integration-services/azure-feature-pack-for-integration-services-ssis.md) es un componente del **Feature Pack de SQL Server Integration Services (SSIS) para Azure**.  
  
Las propiedades siguientes están disponibles en el **Editor de origen de archivo flexible**.

- **Tipo de administrador de conexiones de archivos:** especifica el tipo del administrador de conexiones de origen. Luego elija uno existente del tipo especificado o cree uno.
- **Ruta de acceso a la carpeta:** especifica la ruta de acceso a la carpeta de origen.
- **Nombre de archivo:** especifica el nombre de archivo de origen.
- **Formato de archivo:** especifica el formato de archivo de origen. Los formatos admitidos son **Texto**, **Avro**, **ORC** y **Parquet**.
- **Carácter delimitador de columna:** especifica el carácter utilizado como delimitador de columna (no se admiten delimitadores de varios caracteres).
- **Primera fila como nombre de columna:** especifica si se debe tratar la primera línea como nombres de columna.
- **Descomprimir el archivo:** especifica si se descomprime el archivo de origen.
- **Tipo de compresión:** especifica el formato de compresión de archivo de origen. Los formatos admitidos son **GZIP**, **DEFLATE** y **BZIP2**.
  
Las propiedades siguientes están disponibles en el **Editor avanzado**.

- **rowDelimiter:** carácter utilizado para separar filas en un archivo. Solo se permite un carácter. El valor **predeterminado** es \r\n.
- **escapeChar:** carácter especial utilizado para aplicar una secuencia de escape a un delimitador de columna en el contenido de un archivo de entrada. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. No hay ningún valor predeterminado.
- **quoteChar:** carácter utilizado para citar el valor de una cadena. Los delimitadores de columna y fila de dentro de las comillas se tratan como parte del valor de la cadena. Esta propiedad es aplicable tanto al conjunto de datos de entrada como al de salida. No puede especificar ambos valores escapeChar y quoteChar para una tabla. Solo se permite un carácter. No hay ningún valor predeterminado.
- **nullValue:** uno o varios caracteres empleados para representar un valor nulo. El valor **predeterminado** es \N.
- **encodingName:** permite especificar el nombre de la codificación. Consulte la propiedad [Encoding.EncodingName](https://docs.microsoft.com/en-us/dotnet/api/system.text.encoding?redirectedfrom=MSDN&view=netframework-4.8).
- **skipLineCount:**  permite indicar el número de filas no vacías que hay que omitir al leer datos de archivos de entrada. Si se especifican ambos valores skipLineCount y firstRowAsHeader, las líneas se omiten primero y, luego, la información del encabezado se lee a partir del archivo de entrada.
- **treatEmptyAsNull:** permite especificar si hay que tratar una cadena nula o vacía como un valor nulo al leer datos de un archivo de entrada. El valor **predeterminado** es true.

Después de especificar la información de conexión, cambie a la página **Columnas** para asignar columnas de origen a columnas de destino para el flujo de datos SSIS.

**Requisito previo para el formato de archivo ORC/Parquet**

Para usar el formato de archivo ORC/Parquet se necesita Java.
La arquitectura (32 o 64 bits) de la compilación de Java debe coincidir con la del runtime de SSIS que se va a usar.
Se han probado las siguientes compilaciones de Java.

- [OpenJDK 8u192 de Zulu](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Java SE Runtime Environment 8u192 de Oracle](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

**Configuración de OpenJDK de Zulu**

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

**Configuración de Java SE Runtime Environment de Oracle**

1. Descargue y ejecute el instalador .exe.
2. Siga las instrucciones del programa de instalación para completar la instalación.