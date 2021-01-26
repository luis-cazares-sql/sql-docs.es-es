---
description: Analizar datos
title: Analizar datos | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- parsing [Integration Services]
- data parsing [Integration Services]
ms.assetid: 8893ea9d-634c-4309-b52c-6337222dcb39
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 905c5988860aef1a2e495bb17d98a48eaf91c5ce
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594827"
---
# <a name="parsing-data"></a>Analizar datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los flujos de datos de paquetes extraen y cargan datos entre almacenes de datos heterogéneos, que pueden usar diversos tipos de datos estándar y personalizados. En un flujo de datos, los orígenes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] realizan el trabajo de extraer datos, analizar datos de cadenas y convertir datos en un tipo de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Las transformaciones posteriores pueden analizar datos para convertirlos en un tipo de datos diferentes, o bien crear copias de columnas con diferentes tipos de datos. Las expresiones usadas en los componentes también pueden convertir argumentos y operandos en diferentes tipos de datos. Finalmente, cuando se cargan los datos en un almacén de datos, el destino puede analizar los datos para convertirlos en un tipo de datos que usa el destino. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="two-types-of-parsing"></a>Dos tipos de análisis  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona dos tipos de análisis para convertir los datos: el análisis rápido y el análisis estándar.  
  
-   El análisis rápido es un conjunto rápido y simple de rutinas de análisis que no admite conversiones de tipos de datos específicos de la configuración regional sino solo los formatos de fecha y hora usados con mayor frecuencia. 
  
-   El análisis estándar es un conjunto completo de rutinas de análisis que admite todas las conversiones de tipos de datos proporcionadas por las API de conversión de tipo de datos de automatización disponibles en Oleaut32.dll y Ole2dsip.dll.   
  
## <a name="fast-parse"></a>Fast Parse
El análisis rápido ofrece un conjunto rápido y simple de rutinas para analizar datos. Estas rutinas no son dependientes de la configuración regional y solo admiten un subconjunto de formatos de fecha, hora y número entero.  
  
### <a name="requirements-and-limitations"></a>Limitaciones y requisitos  
 Al implementar el análisis rápido, un paquete arriesga su capacidad de interpretar los datos numéricos y de fecha y hora en formatos específicos de configuración regional, así como varios formatos ISO 8601 básicos y extendidos utilizados con frecuencia, pero el paquete mejora su rendimiento. Por ejemplo, el análisis rápido admite únicamente las representaciones de formato de fecha usadas con más frecuencia, como DDMMYYYY y DD-MM-YYYY, no realiza ningún análisis específico de una configuración regional, no reconoce caracteres especiales en los datos de moneda y no puede convertir una representación hexadecimal o científica de números enteros.  
  
 El análisis rápido solamente está disponible cuando se utiliza el origen de archivo plano o la transformación Conversión de datos. El aumento del rendimiento puede ser considerable, por lo que debería tener en cuenta la posibilidad de utilizar el análisis rápido en estos componentes de flujo de datos, si es posible.  
  
 Si el flujo de datos en el paquete exige un análisis sensible a la configuración regional, se recomienda el análisis estándar en lugar del rápido. Por ejemplo el análisis rápido no reconoce los datos relacionados con la configuración regional que incluyen símbolos decimales como la coma, formatos de fecha que no sean los formatos año-mes-fecha y símbolos de moneda.  
  
 Las representaciones truncadas que implican una o más partes de una fecha, como el siglo, año o mes, no son reconocidas por el análisis rápido. Por ejemplo, el análisis rápido no reconoce el formato "**-YYMM**", que especifica un año y mes en un siglo específico, ni "**--MM**", que especifica un mes en un año implícito. Sin embargo, se reconocen algunas representaciones con precisión reducida. Por ejemplo, el análisis rápido reconoce el formato 'hhmm;', que indica solamente horas y minutos, y '**YYYY**', que indica el año solamente.  
  
 El análisis rápido se especifica en el nivel de columna. En el origen de archivo plano y en la transformación Conversión de datos, puede especificarse el análisis rápido en las columnas de salida. Las entradas y salidas pueden incluir columnas sensibles y no sensibles a la configuración regional.  
 
## <a name="numeric-data-formats-fast-parse"></a>Formatos de datos numéricos (análisis rápido)
El análisis rápido ofrece un conjunto rápido y simple de rutinas, que no distinguen la configuración regional, para analizar datos. El análisis rápido admite solo un conjunto limitado de formatos para tipos de datos enteros.  
  
### <a name="integer-data-type"></a>Tipo de datos Integer
 Los tipos de datos enteros que proporciona [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] son DT_I1, DT_UI1, DT_I2, DT_UI2, DT_I4, DT_UI4, DT_I8 y DT_UI8. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
 El análisis rápido admite los siguientes formatos para tipos de datos enteros:  
  
-   Espacios iniciales y finales de cero o superiores o tabulaciones. Por ejemplo, el valor " 123  " es válido. El valor que todo él es espacios equivale a cero.  
  
-   Un signo más o menos inicial, o ningún signo inicial. Por ejemplo, los valores +123, -123 y 123 son válidos.  
  
-   Uno o más numerales indo-árabes (0-9). Por ejemplo, el valor 345 es válido. No se admiten numerales de otros idiomas.  
  
 Entre los formatos de datos no admitidos se incluyen los siguientes:  
  
-   Caracteres especiales. Por ejemplo, el carácter de moneda $ no se admite y el valor $20 no puede analizarse.  
  
-   Los caracteres de espacios en blanco como avance de línea, retorno de carro y espacios de no separación. Por ejemplo, el valor " 123" no puede analizarse.  
  
-   Representaciones hexadecimales de números enteros. Por ejemplo, el valor 2EE no puede analizarse.  
  
-   Representaciones en notación científica de números enteros. Por ejemplo, el valor 1E+10 no puede analizarse.  
  
 Los siguientes formatos son datos de salida para números enteros:  
  
-   Un signo menos para números negativos y nada para números positivos.  
  
-   Ningún espacio en blanco.  
  
-   Uno o más numerales indo-árabes (0-9).  

## <a name="date-and-time-formats-fast-parse"></a>Formatos de fecha y hora (análisis rápido)
El análisis rápido ofrece un conjunto rápido y simple de rutinas para analizar datos. El análisis rápido admite los siguientes formatos para los tipos de datos de fecha y hora.  
  
### <a name="date-data-type"></a>Tipo de datos de fecha 
 El análisis rápido admite los siguientes formatos de cadena para tipos de datos de fecha:  
  
-   Formatos de fecha con espacios en blanco iniciales. Por ejemplo, el valor " 2004- 02-03" es válido.  
  
-   Formatos ISO 8601, que se muestran en la tabla siguiente:  
  
    |Formato|Descripción|  
    |------------|-----------------|  
    |YYYYMMDD<br /><br /> YYYY-MM-DD|Los formatos básico y extendido para un año de cuatro dígitos, un mes de dos dígitos y un día de dos dígitos. En el formato extendido, las partes de la fecha se encuentran separadas por guiones (-).|  
    |YYYY-MM|Formatos básico y extendido de precisión reducida para un año de cuatro dígitos y un mes de dos dígitos. En el formato extendido, las partes de la fecha se encuentran separadas por guiones (-).|  
    |AAAA|El formato de precisión reducida es un año de cuatro dígitos.|  
  
 El análisis rápido no admite los siguientes formatos para datos de fecha:  
  
-   Valores alfabéticos de meses. Por ejemplo, el formato de fecha Oct-31-2003 no es válido.  
  
-   Formatos ambiguos como DD-MM-YYYY y MM-DD-YYYY. Por ejemplo, las fechas 03-04-1995 y 04-03-1995 no son válidas.  
  
-   Formatos truncados básico y extendido para un año natural de cuatro dígitos y un día de tres dígitos dentro de un año, YYYYDDD y YYYY-DDD.  
  
-   Formatos básico y extendido para un año de cuatro dígitos, un número de dos dígitos para la semana del año y un número de un dígito para el día de la semana, YYYYWwwD y YYYY-Www-D.  
  
-   Los formatos truncados básico y extendido para un año y una semana son un año de cuatro dígitos y un número de dos dígitos para la semana, YYYWww y YYYY-Www.  
  
 El análisis rápido genera los datos como DT_DBDATE. Los valores de fechas en formatos truncados se rellenan. Por ejemplo, YYYY pasa a ser YYYY0101.  
  
 Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="time-data-type"></a>Tipo de datos Time
 El análisis rápido admite los siguientes formatos de cadena para tipos de datos de hora:  
  
-   Formatos de hora con espacios en blanco iniciales. Por ejemplo, el valor " 10:24" es válido.  
  
-   Formato de 24 horas El análisis rápido no admite la notación AM y PM.  
  
-   Formatos de hora ISO 8601, que se muestran en la tabla siguiente:  
  
    |Formato|Descripción|  
    |------------|-----------------|  
    |HHMISS<br /><br /> HH:MI:SS|Los formatos básico y extendido para una hora de dos dígitos, minutos de dos dígitos y segundos de dos dígitos. En el formato extendido, las partes de la hora se encuentran separadas por dos puntos (:).|  
    |HHMI<br /><br /> HH:MI|Formatos truncados básico y extendido para una hora de dos dígitos y minutos de dos dígitos. En el formato extendido, las partes de la hora se encuentran separadas por dos puntos (:).|  
    |HH|Formato truncado para una hora de dos dígitos.|  
    |00:00:00<br /><br /> 000000<br /><br /> 0000<br /><br /> 00<br /><br /> 240000<br /><br /> 24:00:00<br /><br /> 2400<br /><br /> 24|Formato para medianoche.|  
  
-   Formatos de hora que especifican una zona horaria, como se muestra en la tabla siguiente:  
  
    |Formato|Descripción|  
    |------------|-----------------|  
    |+HH:MI<br /><br /> +HHMI|Formatos básicos y extendidos que indican el número de horas y minutos que se agregan a la hora universal coordinada (UTC) para obtener la hora local.|  
    |-HH:MI<br /><br /> -HHMI|Formatos básicos y extendidos que indican el número de horas y minutos que se restan a la UTC para obtener la hora local.|  
    |+HH|Formato truncado que indica el número de horas que se agregan a la UTC para obtener la hora local.|  
    |-HH|Formato truncado que indica el número de horas que se restan a la UTC para obtener la hora local.|  
    |Z|Un valor de 0 que indica que la hora se representa en UTC.|  
  
     Los formatos para todos los datos de fecha/hora pueden incluir un elemento de zona horaria. Sin embargo, el sistema omite el valor de zona horaria, excepto cuando los datos son del tipo DT_DBTIMESTAMPOFFSET. Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
     En formatos que incluyen un elemento de zona horaria, no hay ningún espacio entre el elemento de tiempo y el elemento de zona horaria, como se muestra en el ejemplo siguiente:  
  
     HH:MI:SS[+HH:MI]  
  
     Los corchetes del ejemplo anterior indican que el valor de zona horaria es opcional.  
  
-   Formatos de hora que incluyen una fracción decimal, como se muestra en la tabla siguiente:  
  
    |Formato|Descripción|  
    |------------|-----------------|  
    |HH[.nnnnnnn]|n es un valor entre 0 y 9999999 que representa una fracción de horas. Los corchetes indican que este valor es opcional.<br /><br /> Por ejemplo, el valor 12.750 indica 12:45.|  
    |HHMI[.nnnnnnn]<br /><br /> HH:MI[.nnnnnnn]|n es un valor entre 0 y 9999999 que representa una fracción de minutos. Los corchetes indican que este valor es opcional.<br /><br /> Por ejemplo, el valor 1220.500 indica 12:20:30.|  
    |HHMISS[.nnnnnnn]<br /><br /> HH:MI:SS[.nnnnnnn]|n es un valor entre 0 y 9999999 que representa una fracción de segundos. Los corchetes indican que este valor es opcional.<br /><br /> Por ejemplo, el valor 122040.250 indica 12:20:40.15.|  
  
    > [!NOTE]  
    >  El separador decimal para los formatos de hora de la tabla anterior puede ser un punto o una coma.  
  
-   Valores de hora que incluyen un segundo intercalar, como se muestra en los ejemplos siguientes:  
  
     23:59:60[.0000000]  
  
     235960[.0000000]  
  
 El análisis rápido genera cadenas como DT_DBTIME y DT_DBTIME2. Los valores de horas en formatos truncados se rellenan. Por ejemplo, HH:MI pasa a ser HH:MM:00.000.  
  
 Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
### <a name="datetime-data-type"></a>Tipo de datos de fecha/hora  
 El análisis rápido admite los siguientes formatos de cadena para tipos de datos de fecha/hora:  
  
-   Formatos con espacios en blanco iniciales. Por ejemplo, el valor "  2003-01/10T203910" es válido.  
  
-   Combinaciones de formatos de fecha válidos y los formatos de hora válidos separados por una T mayúscula, y formatos de zona horaria válidos, como YYYYMMDDT[HHMISS][+HH:MI]. Los valores de hora y zona horaria no son obligatorios. Por ejemplo, "2003-10-14" es válido.  
  
 El análisis rápido no admite intervalos de tiempo. Por ejemplo, un intervalo de tiempo identificado con una fecha y hora de inicio y fin en el formato YYYYMMDDThhmmss/YYYYMMDDThhmmss no se puede analizar.  
  
 El análisis rápido genera cadenas como DT_DATE, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 y DT_DBTIMESTAMPOFFSET. Los valores de fecha/hora en formatos truncados se rellenan. En la siguiente tabla se muestran los valores que se agregan para las partes de fecha y hora que faltan.  
  
|Parte de fecha y hora|Relleno|  
|---------------------|-------------|  
|Segundos|Agregar 00.|  
|Minutos|Agregar 00:00.|  
|Hora|Agregar 00:00:00.|  
|Día|Agregar 01 para el día del mes.|  
|Month (Mes)|Agregar 01 para el mes del año.|  
  
 Para obtener más información, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
## <a name="enable-fast-parse"></a>Habilitar el análisis rápido
La propiedad de análisis rápido debe configurarse para cada columna del origen o la transformación que utilice el análisis rápido. Para configurar la propiedad, utilice el Editor avanzado del origen de archivo plano y la transformación Conversión de datos.  
  
1.  Haga clic con el botón derecho en el origen de archivo plano o en la transformación Conversión de datos y, después, haga clic en **Mostrar editor avanzado**.  
  
2.  En el cuadro de diálogo **Editor avanzado** haga clic en la pestaña **Propiedades de entrada y salida** .  
  
3.  En el panel **Entradas y salidas** , haga clic en la columna para la cual desee habilitar el análisis rápido.  
  
4.  En la ventana Propiedades, expanda el nodo **Propiedades personalizadas** y establezca la propiedad **FastParse** en **True**.  
  
5.  Haga clic en **Aceptar**.  

## <a name="standard-parse"></a>Standard Parse
El análisis estándar es un conjunto de rutinas de análisis dependientes de la configuración regional y que admiten todas las conversiones de tipo de datos proporcionadas por las API de conversión de tipo de datos de Automation disponibles en Oleaut32.dll y Ole2dsip.dll. El análisis estándar es equivalente a las API de análisis de OLE DB.  
  
 El análisis estándar proporciona conversión de tipo de datos internacionales y se debe usar si el análisis rápido no admite el formato de datos. Para obtener más información sobre la conversión API de los tipos de automatización de datos, vea "Los tipos de conversión de datos API" en la [biblioteca MSDN](/documentation/). 
