---
title: Mejora del rendimiento mediante la función de generación de perfiles de código de R
description: Recopile la información útil para mejorar el rendimiento y obtenga resultados más rápidos en los cálculos de R en SQL Server mediante el uso de funciones de generación de perfiles de R. La función *rprof* recopila y devuelve información acerca de las llamadas de función internas.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/30/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 511a0d73bbc0e8df5c19ee563dca9c7eef8e53eb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597046"
---
# <a name="use-r-code-profiling-functions-to-improve-performance"></a>Uso de funciones de generación de perfiles de código de R para mejorar el rendimiento
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se describen las herramientas de rendimiento que los paquetes de R proporcionan para obtener información sobre las llamadas de funciones internas. Puede usar esta información para mejorar el rendimiento del código.

> [!TIP]
> En este artículo se ofrecen recursos básicos para comenzar. Para obtener orientación experta, se recomienda la sección sobre *rendimiento* del libro ["Advanced R"](http://adv-r.had.co.nz) de Hadley Wickham.

## <a name="using-rprof"></a>Uso de RPROF

[*rprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/Rprof) es una función incluida en el paquete base [**utils**](https://www.rdocumentation.org/packages/utils/versions/3.5.1), que se carga de manera predeterminada. 

Por lo general, la función *rprof* escribe la pila de llamadas en un archivo, a intervalos especificados. Después, se puede usar la función [*summaryRprof*](https://www.rdocumentation.org/packages/utils/versions/3.5.1/topics/summaryRprof) para procesar el archivo de salida. Una ventaja de *rprof* es que realiza el muestreo, lo que, por tanto, disminuye la carga de rendimiento de la supervisión.

Para usar la generación de perfiles de R en el código, llame a esta función y especifique sus parámetros, incluido el nombre de la ubicación del archivo de registro que se va a escribir. La generación de perfiles se puede activar y desactivar en el código. La sintaxis siguiente muestra el uso básico: 

```R
# Specify profiling output file.
varOutputFile <- "C:/TEMP/run001.log")
Rprof(varOutputFile)

# Turn off profiling
Rprof(NULL)
    
# Restart profiling
Rprof(append=TRUE)
```

> [!NOTE]
> El uso de esta función requiere que Windows Perl esté instalado en el equipo donde se ejecuta el código. Por consiguiente, se recomienda generar perfiles del código durante el desarrollo en un entorno de R e implementar después el código depurado en SQL Server.  


## <a name="r-system-functions"></a>Funciones del sistema de R

El lenguaje R incluye muchas funciones del paquete base que devuelven el contenido de variables del sistema. Por ejemplo, como parte del código de R, podría usar `Sys.timezone` para obtener la zona horaria actual o `Sys.Time` para obtener la hora del sistema desde R. 

Para obtener información sobre funciones individuales del sistema de R, escriba el nombre de la función como argumento en la función `help()` de R desde un símbolo del sistema.

```R
help("Sys.time")
```

## <a name="debugging-and-profiling-in-r"></a>Depuración y generación de perfiles de R

La documentación de Microsoft R Open, que se instala de manera predeterminada, incluye un manual sobre el desarrollo de extensiones para el lenguaje R que analiza minuciosamente la [generación de perfiles y la depuración](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Debugging).

## <a name="next-steps"></a>Pasos siguientes

+ Para más información sobre la optimización de scripts de R en SQL Server, consulte [Optimización del rendimiento y de los datos para R](r-and-data-optimization-r-services.md).
+ Para información más completa sobre la optimización del rendimiento en SQL Server, consulte [Centro de rendimiento para el motor de base de datos de SQL Server y Base de datos SQL de Azure](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md).
+ Para más información sobre el paquete de utilidades, consulte el artículo sobre [el paquete de utilidades de R](https://www.rdocumentation.org/packages/utils/versions/3.5.1).
+ Para un análisis detallado de la programación R, consulte ["Advanced R" de Hadley Wickham](http://adv-r.had.co.nz).