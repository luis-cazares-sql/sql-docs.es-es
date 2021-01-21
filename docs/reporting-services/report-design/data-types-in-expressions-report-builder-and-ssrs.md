---
title: Tipos de datos en expresiones (Generador de informes) | Microsoft Docs
description: Explore cómo los tipos de datos representan distintos tipos de datos para que se puedan almacenar y procesar en el Generador de informes.
ms.date: 08/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 94fdf921-270c-4c12-87b3-46b1cc98fae5
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 0202703dec933871d6b6a611ff1139a68ee2c938
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595361"
---
# <a name="data-types-in-expressions-report-builder-and-ssrs"></a>Tipos de datos en expresiones (Generador de informes y SSRS)
  La finalidad de los tipos de datos es permitir el almacenamiento y el procesamiento de los datos de manera eficaz. Los tipos de datos más comunes incluyen texto (también conocido como cadenas), números (con y sin decimales), fechas y horas, e imágenes. Los valores de un informe deben ser del tipo de datos de lenguaje RDL (Report Definition Language). Puede dar formato a un valor según sus preferencias al mostrarlo en un informe. Por ejemplo, un campo que representa valores de moneda se almacena en la definición del como un número de punto flotante y mostrarse en uno y otro formato en función de la propiedad de formato elegida.  
  
 Para obtener más información sobre los formatos de visualización, vea [Aplicar formato a los elementos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="report-definition-language-rdl-data-types-and-common-language-runtime-clr-data-types"></a>Tipos de datos del lenguaje RDL (Report Definition Language) y tipos de datos de Common Language Runtime (CLR)  
 Los valores que se especifican en un archivo RDL deben ser del tipo de datos RDL. Cuando se compila y procesa el informe, los tipos de datos RDL se convierten en tipos de datos CLR. En la siguiente tabla se muestra la conversión, que se marca como Predeterminada:  
  
|Tipo RDL|Tipos de CLR|  
|--------------|---------------|  
|String|Valor predeterminado: String<br /><br /> Chart, GUID, Timespan|  
|Boolean|Valor predeterminado: Boolean|  
|Entero|Valor predeterminado: Int64<br /><br /> Int16, Int32, Uint16, Uint64, Byte, Sbyte|  
|DateTime|Valor predeterminado: DateTime<br /><br /> DateTimeOffset|  
|Float|Valor predeterminado: Double<br /><br /> Single, Decimal|  
|Binary|Valor predeterminado: Byte[]|  
|Variant|Cualquiera de los anteriores excepto Byte []|  
|VariantArray|Matriz de Variant|  
|Serializable|Variant o tipos marcados con Serializable o que implementan ISerializable.|  
  
## <a name="understanding-data-types-and-writing-expressions"></a>Descripción de los tipos de datos y escritura de expresiones  
 Es importante conocer los tipos de datos para poder escribir expresiones que comparen o combinen valores, por ejemplo, para definir expresiones de grupo o de filtro, o calcular agregados. Las comparaciones y los cálculos solo son válidos entre elementos del mismo tipo de datos. Si los tipos de datos no coinciden, deberá convertir de manera explícita el tipo de datos del elemento de informe mediante una expresión.  
  
 En la lista siguiente, se describen los casos en los que puede ser necesario convertir los datos en un tipo de datos diferente:  
  
-   Comparar el valor de un parámetro de informe de un tipo de datos con un campo de conjunto de datos de un tipo de datos diferente.  
  
-   Escribir expresiones de filtro que comparen valores de tipos de datos diferentes.  
  
-   Escribir expresiones de ordenación que combinen campos de tipos de datos diferentes.  
  
-   Escribir expresiones de grupo que combinen campos de tipos de datos diferentes.  
  
-   Convertir el tipo de datos de un valor recuperado del origen de datos en un tipo de datos diferente.  
  
## <a name="determining-the-data-type-of-report-data"></a>Determinar el tipo de datos de los datos de informe  
 Para determinar el tipo de datos de un elemento de informe, puede escribir una expresión que devuelva su tipo de datos. Por ejemplo, para mostrar el tipo de datos del campo `MyField`, agregue la expresión siguiente a una celda de la tabla: `=Fields!MyField.Value.GetType().ToString()`. El resultado muestra el tipo de datos CLR usado para representar `MyField`; por ejemplo, **System.String** o **System.DateTime**.  
  
## <a name="converting-dataset-fields-to-a-different-data-type"></a>Convertir campos de conjunto de datos en un tipo de datos diferente  
 También puede convertir campos de conjunto de datos antes de usarlos en un informe. En la lista siguiente, se describen las maneras de convertir un campo de conjunto de datos existente:  
  
-   Modifique la consulta de conjunto de datos para agregar un nuevo campo de consulta con los datos convertidos. Si se trata de orígenes de datos relacionales o multidimensionales, se usan recursos de origen de datos para realizar la conversión.  
  
-   Cree un campo calculado basado en un campo de conjunto de datos de informe existente; para ello, escriba una expresión que convierta todos los datos de una columna de conjunto de resultados en una nueva columna con un tipo de datos diferente. Por ejemplo, la expresión siguiente convierte el valor entero del campo Year en un valor de cadena: `=CStr(Fields!Year.Value)`. Para obtener más información, consulte [Agregar, editar y actualizar campos en el panel Datos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Compruebe si la extensión de procesamiento de datos que está usando incluye metadatos para recuperar datos que ya tienen asignado un formato. Por ejemplo, una consulta MDX de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] incluye la propiedad extendida FORMATTED_VALUE para los valores de cubo a los que se dio formato al procesar el cubo. Para obtener más información, vea [Propiedades de campo extendidas para una base de datos de Analysis Services &#40;SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
## <a name="understanding-parameter-data-types"></a>Descripción de los tipos de datos de los parámetros  
 Los parámetros de informe deben pertenecer a uno de los cinco tipos de datos: Boolean, DateTime, Integer, Float o Text (también conocido como String). Si la consulta de conjunto de datos incluye parámetros de consulta, automáticamente se crean parámetros de informe que se vinculan a dichos parámetros de consulta. El tipo de datos predeterminado para un parámetro de informe es String. Para cambiar el tipo de datos predeterminado de un parámetro de informe, seleccione el valor adecuado en la lista desplegable **Tipo de datos** de la página **General** del cuadro de diálogo **Propiedades de parámetro de informe** .  
  
> [!NOTE]  
>  Los parámetros de informe cuyo tipo de datos es DateTime no admiten milisegundos. Aunque sí puede crear un parámetro basado en valores que incluyen milisegundos, no puede seleccionar un valor con milisegundos en una lista desplegable de valores disponibles que contiene valores de fecha o de hora.  
  
## <a name="writing-expressions-that-convert-data-types-or-extract-parts-of-data"></a>Escribir expresiones que convierten tipos de datos o que extraen partes de datos  
 Cuando se combina texto y campos de conjunto de datos con el operador de concatenación (&), el lenguaje CLR (Common Language Runtime) suele proporcionar los formatos predeterminados. Si necesita convertir de manera explícita el tipo de datos de un campo de conjunto de datos o de un parámetro en un tipo de datos concreto, deberá usar un método CLR o una función de biblioteca en tiempo de ejecución de Visual Basic.  
  
 En la tabla siguiente, se muestran ejemplos de conversión de tipos de datos.  
  
|Tipo de conversión|Ejemplo|  
|------------------------|-------------|  
|DateTime a String|`=CStr(Fields!Date.Value)`|  
|String a DateTime|`=DateTime.Parse(Fields!DateTimeinStringFormat.Value)`|  
|String a DateTimeOffset|`=DateTimeOffset.Parse(Fields!DateTimeOffsetinStringFormat.Value)`|  
|Extraer la parte Year|`=Year(Fields!TimeinStringFormat.Value)`<br /><br /> `-- or --`<br /><br /> `=Year(Fields!TimeinDateTimeFormat.Value)`|  
|Boolean a Integer|`=CInt(Parameters!BooleanField.Value)`<br /><br /> -1 es True y 0 es False.|  
|Boolean a Integer|`=System.Convert.ToInt32(Fields!BooleanFormat.Value)`<br /><br /> 1 es True y 0 es False.|  
|Solo la parte DateTime de un valor DateTimeOffset|`=Fields!MyDatetimeOffset.Value.DateTime`|  
|Solo la parte Offset de un valor DateTimeOffset|`=Fields!MyDatetimeOffset.Value.Offset`|  
  
 También puede usar la función de formato para controlar el formato de visualización del valor. Para obtener más información, vea [Funciones (Visual Basic)](/dotnet/visual-basic/language-reference/functions/).  
  
## <a name="advanced-examples"></a>Ejemplos avanzados  
 Cuando se establece conexión con un origen de datos cuyo proveedor de datos no permite convertir todos los tipos de datos existentes en dicho origen de datos, el tipo de datos predeterminado para los tipos de datos no compatibles es String. En los ejemplos siguientes, se ofrecen soluciones para tipos de datos concretos que se devuelven como String.  
  
### <a name="concatenating-a-string-and-a-clr-datetimeoffset-data-type"></a>Concatenar un tipo de datos String y un tipo de datos CLR DateTimeOffset  
 Para la mayoría de los tipos de datos, el lenguaje CLR proporciona conversiones predeterminadas que permiten concatenar valores de tipos de datos diferentes en una cadena con el operador &. Por ejemplo, la expresión siguiente concatena el texto "The date and time are:" con un campo de conjunto de datos StartDate, que es un valor <xref:System.DateTime> : `="The date and time are: " & Fields!StartDate.Value`.  
  
 Para algunos tipos de datos, puede ser necesario incluir la función ToString. Por ejemplo, la expresión siguiente muestra el mismo ejemplo con el tipo de datos CLR <xref:System.DateTimeOffset>, que incluye la fecha, la hora y un ajuste de zona horaria en relación con la zona horaria UTC: `="The time is: " & Fields!StartDate.Value.ToString()`.  
  
### <a name="converting-a-string-data-type-to-a-clr-datetime-data-type"></a>Convertir un tipo de datos String en un tipo de datos CLR DateTime  
 Si una extensión de procesamiento de datos no admite todos los tipos de datos definidos en un origen de datos, los datos se pueden recuperar como texto. Por ejemplo, un valor del tipo de datos **datetimeoffset(7)** se puede recuperar como un tipo de datos String. En Perth, Australia, el valor de cadena para la fecha 1 de julio de 2008, a las 6:05:07,9999999 a.m. sería algo parecido a:  
  
 `2008-07-01 06:05:07.9999999 +08:00`  
  
 En este ejemplo, se muestra la fecha (1 de julio de 2008) seguida de la hora con una precisión de 7 dígitos (6:05:07,9999999 a.m.) y de un ajuste de zona horaria UTC en horas y minutos (más 8 horas, 0 minutos). Para los ejemplos siguientes, este valor se ha situado en un campo con el tipo de datos **String** denominado `MyDateTime.Value`.  
  
 Puede usar una de las estrategias siguientes para convertir estos datos en uno o más valores CLR:  
  
-   En un cuadro de texto, use una expresión para extraer partes de la cadena. Por ejemplo:  
  
    -   La expresión siguiente extrae solo la parte de la hora del ajuste de zona horaria UTC y la convierte en minutos: `=CInt(Fields!MyDateTime.Value.Substring(Fields!MyDateTime.Value.Length-5,2)) * 60`  
  
         El resultado es `480`.  
  
    -   La expresión siguiente convierte la cadena en un valor de fecha y hora: `=DateTime.Parse(Fields!MyDateTime.Value)`  
  
         Si la cadena `MyDateTime.Value` tiene un ajuste UTC, la función `DateTime.Parse` primero ajusta de acuerdo con el ajuste UTC (7 a.m. - [`+08:00`] a la hora UTC de las 11 p.m. de la noche anterior). A continuación, la función `DateTime.Parse` aplica el ajuste UTC del servidor de informes local y, si fuera necesario, vuelve a ajustar la hora para adaptarla al horario de verano. Por ejemplo, en Redmond, Washington, el ajuste de la hora local adaptado al horario de verano es `[-07:00]`o 7 horas antes de las 11 p.m. El resultado es el valor **DateTime** siguiente: `2007-07-06 04:07:07 PM` (6 de julio de 2007 a las 4:07 p.m.).  
  
 Para más información acerca de la forma de convertir cadenas en tipos de datos **DateTime** , consulte [Analizar cadenas de fecha y hora](https://go.microsoft.com/fwlink/?LinkId=89703), [Aplicar formato de fecha y hora para una referencia cultural específica](https://go.microsoft.com/fwlink/?LinkId=89704)y [Choosing Between DateTime, DateTimeOffsety TimeZoneInfo](/dotnet/standard/datetime/choosing-between-datetime) en MSDN.  
  
-   Agregue un nuevo campo calculado al conjunto de datos de informe que use una expresión para extraer partes de la cadena. Para obtener más información, consulte [Agregar, editar y actualizar campos en el panel Datos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
-   Cambie la consulta de conjunto de datos de informe para usar funciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] que extraigan valores de fecha y hora de forma independiente para crear columnas independientes. En el ejemplo siguiente, se muestra cómo usar la función **DatePart** para agregar una columna para el año y una columna para la zona horaria UTC convertida en minutos:  
  
     `SELECT`  
  
     `MyDateTime,`  
  
     `DATEPART(year, MyDateTime) AS Year,`  
  
     `DATEPART(tz, MyDateTime) AS OffsetinMinutes`  
  
     `FROM MyDates`  
  
     El conjunto de resultados tiene tres columnas. La primera columna es la fecha y la hora, la segunda columna es el año y la tercera columna es el ajuste UTC en minutos. En la fila siguiente se muestran datos de ejemplo:  
  
     `2008-07-01 06:05:07             2008                   480`  
  
 Para obtener más información sobre los tipos de datos de las bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md) y [Tipos de datos y funciones de fecha y hora &#40;Transact-SQL&#41;](../../t-sql/functions/date-and-time-data-types-and-functions-transact-sql.md).  
  
 Para obtener más información sobre los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], consulte [Tipos de datos en Analysis Services](/analysis-services/multidimensional-models/olap-physical/data-types-in-analysis-services).  
  
## <a name="see-also"></a>Consulte también  
 [Aplicar formato a los elementos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)  
  
