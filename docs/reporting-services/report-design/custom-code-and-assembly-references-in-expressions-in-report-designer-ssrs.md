---
title: Referencias a ensamblados y código personalizado en expresiones en el Diseñador de informes | Microsoft Docs
description: Obtenga información sobre cómo agregar referencias a un código personalizado insertado en un informe. Compile y guarde en el equipo, e implemente en el servidor de informes en el Generador de informes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- items [Reporting Services], expressions
- data [Reporting Services], expressions
- expressions [Reporting Services], about expressions
- expressions [Reporting Services]
- SSRS, expressions
- formulas [Reporting Services]
- data manipulation [Reporting Services]
- SQL Server Reporting Services, expressions
ms.assetid: ae8a0166-2ccc-45f4-8d28-c150da7b73de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31e92b49186e79c78cbaa2d13859d8dd3abe5c5d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596346"
---
# <a name="custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs"></a>Referencias a ensamblados y código personalizado en expresiones en el Diseñador de informes (SSRS)
  Puede agregar referencias al código personalizado incrustado en un informe o a los ensamblados personalizados que haya generado y guardado en el equipo e implementado en el servidor de informes. El código incrustado se utiliza en constantes, funciones complejas o funciones personalizadas que se usan varias veces en un único informe. Use ensamblados de código personalizados para mantener el código en un único lugar y compartirlo con el fin de utilizarlos en múltiples informes. El código personalizado puede incluir nuevas constantes, variables, funciones o subrutinas personalizadas. Puede incluir referencias de solo lectura en las colecciones integradas, como la colección Parameters. Sin embargo, no puede pasar conjuntos de valores de datos de informe a las funciones personalizadas; concretamente, no se admiten agregados personalizados.  
  
> [!IMPORTANT]  
>  Para cálculos dependientes del tiempo que se evalúan una sola vez en tiempo de ejecución y cuyo valor desea conservar a lo largo del procesamiento del informe, considere la posibilidad de usar una variable de informe o una variable de grupo. Para más información, vea [Referencias a las colecciones de variables de informe y de grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md).  
  
 El Diseñador de informes es el entorno de creación preferido para agregar código personalizado a un informe. El Generador de informes admite el procesamiento de informes que tienen expresiones válidas o que incluyen referencias a ensamblados personalizados en un servidor de informes. El Generador de informes no proporciona un medio para agregar una referencia a un ensamblado personalizado.  
  
> [!NOTE]  
>  Tenga en cuenta que durante la actualización de un servidor de informes, los informes que dependen de ensamblados personalizados pueden requerir pasos adicionales para completar la actualización.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="working-with-custom-code-in-report-builder"></a><a name="RB3"></a> Trabajar con código personalizado en el Generador de informes  
 En el Generador de informes, puede abrir un informe desde un servidor de informes que incluya referencias a ensamblados personalizados. Por ejemplo, puede modificar los informes creados e implementados mediante el uso del Diseñador de informes en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Los ensamblados personalizados deben implementarse en el servidor de informes.  
  
 No puede realizar lo siguiente:  
  
1.  Agregar referencias o instancias de miembro de clase a un informe.  
  
2.  Obtener una vista previa de un informe con referencias a ensamblados personalizados en modo local.  
  
##  <a name="including-references-to-commonly-used-functions"></a><a name="Common"></a> Incluir referencias a funciones de uso frecuente  
 Use el cuadro de diálogo **Expresión** para ver una lista organizada en categorías de las funciones de uso frecuente integradas en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Si expande **Funciones comunes** y hace clic en una categoría, el panel **Elemento** muestra la lista de funciones que puede incluir en una expresión. En las funciones comunes se incluyen clases de los espacios de nombres [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.Math> y <xref:System.Convert>, así como funciones de biblioteca en tiempo de ejecución de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Para mayor comodidad, puede ver las funciones de uso más frecuente en el cuadro de diálogo **Expresión** , donde aparecen agrupadas por categorías: Texto, Fecha y hora, Matemáticas, Inspección, Flujo de programa, Agregado, Finanzas, Conversión y Varios. Las funciones de uso menos frecuente no aparecen en la lista, pero se pueden usar en una expresión.  
  
 Para usar una función integrada, haga doble clic en el nombre de la función en el panel Elemento. En el panel Descripción, aparece una descripción de la función; en el panel Ejemplo, aparece un ejemplo de la llamada a la función. En el panel de código, al escribir el nombre de la función seguido por un paréntesis izquierdo **(** , la Ayuda de IntelliSense muestra la sintaxis válida para la llamada a la función. Por ejemplo, para calcular el valor máximo de un campo denominado `Quantity` en una tabla, agregue la expresión simple `=Max(` al panel de código y, a continuación, use las etiquetas inteligentes para ver todas las posibles sintaxis válidas para la llamada a la función. Para completar este ejemplo, escriba `=Max(Fields!Quantity.Value)`.  
  
 Para más información sobre cada función, vea <xref:System.Math>, <xref:System.Convert>y [Miembros de la biblioteca en tiempo de ejecución de Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) en MSDN.  
  
##  <a name="including-references-to-less-commonly-used-functions"></a><a name="NotCommon"></a> Incluir referencias a funciones de uso menos frecuente  
 Para incluir una referencia a otros espacios de nombres de CLR de uso menos frecuente es necesario usar una referencia completa (por ejemplo, <xref:System.Text.StringBuilder>. IntelliSense no se admite en el panel de código del cuadro de diálogo **Expresión** para estas funciones menos frecuentes.  
  
 Para obtener más información, vea [Miembros de la biblioteca en tiempo de ejecución de Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) en MSDN.  
  
##  <a name="including-references-to-external-assemblies"></a><a name="External"></a> Incluir referencias a ensamblados externos  
 Para incluir una referencia a una clase de un ensamblado externo, debe identificar el ensamblado para el procesador de informes. Use la página **Referencias** del cuadro de diálogo **Propiedades del informe** para especificar el nombre completo del ensamblado que se va a agregar al informe. En la expresión, debe usar el nombre completo para la clase del ensamblado. Las clases de un ensamblado externo no aparecen en el cuadro de diálogo **Expresión** ; es necesario que proporcione el nombre correcto de la clase. Un nombre completo incluye el espacio de nombres, el nombre de la clase y el nombre del miembro.  
  
##  <a name="including-embedded-code"></a><a name="Embedded"></a> Incluir código incrustado  
 Para agregar código incrustado a un informe, use la pestaña Código del cuadro de diálogo **Propiedades del informe** . El bloque de código que cree puede contener varios métodos. Es necesario que los métodos del código incrustado estén escritos en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] y que estén basados en instancias. El procesador de informes agrega automáticamente referencias para los espacios de nombres System.Convert y System.Math. Use la página **Referencias** del cuadro de diálogo **Propiedades del informe** para agregar referencias de ensamblado adicionales. Para más información, vea [Agregar una referencia de ensamblado a un informe &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 Los métodos del código incrustado están disponibles a través del miembro **Code** definido globalmente. Para poder acceder a ellos, se hace referencia al miembro **Code** y al nombre del método. En el siguiente ejemplo, se llama al método **ToUSD**, que convierte el valor del campo `StandardCost` en un valor en dólares:  
  
```  
=Code.ToUSD(Fields!StandardCost.Value)  
```  
  
 Para hacer referencia a colecciones integradas en el código personalizado, incluya una referencia al objeto integrado **Report** :  
  
```  
=Report.Parameters!Param1.Value  
```  
  
 En los ejemplos siguientes se muestra cómo definir algunas constantes y variables personalizadas.  
  
```  
Public Const MyNote = "Authored by Bob"  
Public Const NCopies As Int32 = 2  
Public Dim  MyVersion As String = "123.456"  
Public Dim MyDoubleVersion As Double = 123.456  
```  
  
 Aunque las constantes personalizadas no aparecen en la categoría **Constantes** del cuadro de diálogo **Expresión** (que solo muestra las constantes integradas), se pueden agregar referencias a ellas desde cualquier expresión, como se muestra en los ejemplos siguientes. En una expresión, una constante personalizada se trata como **Variant**.  
  
```  
=Code.MyNote  
=Code.NCopies  
=Code.MyVersion  
=Code.MyDoubleVersion  
```  
  
 En el ejemplo siguiente se incluyen tanto la referencia a código como la implementación en el código de la función **FixSpelling**, que sustituye por el texto `"Bicycle"` todas las repeticiones del texto "Bike" en el campo `SubCategory` .  
  
 `=Code.FixSpelling(Fields!SubCategory.Value)`  
  
 Cuando el código siguiente se incrusta en un bloque de código de la definición de informe, muestra una implementación del método **FixSpelling** . En este ejemplo se muestra cómo usar una referencia completa a la clase **StringBuilder** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
```vb  
Public Function FixSpelling(ByVal s As String) As String  
   Dim strBuilder As New System.Text.StringBuilder(s)  
   If s.Contains("Bike") Then  
      strBuilder.Replace("Bike", "Bicycle")  
      Return strBuilder.ToString()  
      Else : Return s  
   End If  
End Function  
```  
  
 Para más información sobre las colecciones de objetos integradas y la inicialización, vea [Referencias a campos globales y de usuario integrados &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md) e [Inicializar objetos de ensamblados personalizados](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md).  
  
##  <a name="including-references-to-parameters-from-code"></a><a name="Parameters"></a> Incluir referencias a parámetros desde el código  
 Se puede hacer referencia a la colección global Parameters mediante código personalizado en un bloque de código de la definición de informe o en un ensamblado personalizado proporcionado por el usuario. La colección Parameters es de solo lectura y no tiene iteradores públicos. No se puede usar una construcción **For Each** de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] para recorrer la colección. Debe conocer el nombre del parámetro definido en la definición de informe para poder hacer referencia a él en el código. Pero puede recorrer en iteración todos los valores de un parámetro de varios valores.  
  
 En la tabla siguiente se incluyen ejemplos de referencias a la colección integrada `Parameters` desde código personalizado:  
  
 **Pasa una colección de parámetros globales a código personalizado.** Esta función devuelve el valor de un parámetro de informes específico *MyParameter*.  
  
 Referencia en la expresión `=Code.DisplayAParameterValue(Parameters)`  
  
 Definición del código personalizado  
  
```  
Public Function DisplayAParameterValue(ByVal parameters as Parameters) as Object  
Return parameters("MyParameter").Value  
End Function  
```  
  
 **Pasa un parámetro individual al código personalizado.**  
  
 Referencia en la expresión `=Code.ShowParametersValues(Parameters!DayOfTheWeek)`  
  
 En este ejemplo se devuelve el valor del parámetro pasado. Si el parámetro es un parámetro de varios valores, la cadena devuelta es una concatenación de todos los valores.  
  
 Definición del código personalizado  
  
```  
Public Function ShowParameterValues(ByVal parameter as Parameter)  
 as String  
   Dim s as String   
   If parameter.IsMultiValue then  
      s = "Multivalue: "   
      For i as integer = 0 to parameter.Count-1  
         s = s + CStr(parameter.Value(i)) + " "   
      Next  
   Else  
      s = "Single value: " + CStr(parameter.Value)  
   End If  
   Return s  
End Function  
```  
  
##  <a name="including-references-to-code-from-custom-assemblies"></a><a name="Custom"></a> Incluir referencias al código desde ensamblados personalizados  
 Para utilizar ensamblados personalizados en un informe, primero debe crear el ensamblado, hacer que esté disponible para el Diseñador de informes, agregar una referencia a él en el informe y, a continuación, utilizar una expresión en el informe que haga referencia a los métodos de dicho ensamblado. Cuando implemente el informe en el servidor de informes, también debe implementar el ensamblado personalizado.  
  
 Para obtener información sobre cómo crear un ensamblado personalizado y hacer que esté disponible en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Usar ensamblados personalizados con informes](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md).  
  
 Para incluir en una expresión una referencia a código personalizado, debe llamar al miembro de una clase dentro del ensamblado. La manera de hacerlo depende de si el método es estático o se basa en instancias. Los métodos estáticos de un ensamblado de código están disponibles globalmente en el informe. El acceso a estos métodos estáticos en expresiones se lleva a cabo a través de la especificación del espacio de nombres, la clase y el nombre del método. En el siguiente ejemplo, se llama al método **ToGBP**, que convierte el valor del campo **StandardCost** de dólares a libras esterlinas:  
  
```  
=CurrencyConversion.DollarCurrencyConversion.ToGBP(Fields!StandardCost.Value)  
```  
  
 Los métodos basados en instancias están disponibles a través del miembro **Code** definido globalmente. Para tener acceso a estos métodos, debe hacerse referencia al miembro **Code** , seguido del nombre de la instancia y del método. En el siguiente ejemplo, se llama al método **ToEUR**, que convierte el valor del campo **StandardCost** de dólares a euros:  
  
```  
=Code.m_myDollarCoversion.ToEUR(Fields!StandardCost.Value)  
```  
  
> [!NOTE]  
>  En el Diseñador de informes, un ensamblado personalizado se carga una vez y no se descarga hasta que se cierra [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Si se obtiene la vista previa de un informe, se realizan cambios en un ensamblado personalizado que se utiliza en el informe y, después, se vuelve a obtener la vista previa del informe, los cambios no aparecerán en la segunda vista previa. Para volver a cargar el ensamblado, cierre [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , vuelva a abrirlo y obtenga la vista previa del informe.  
  
 Para obtener más información sobre cómo tener acceso al código, vea [Accessing Custom Assemblies Through Expressions](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md).  
  
##  <a name="passing-built-in-collections-into-custom-assemblies"></a><a name="collections"></a> Pasar colecciones integradas a ensamblados personalizados  
 Si quiere pasar colecciones integradas (como las colecciones *Globals* o *Parameters* ) a un ensamblado personalizado para su procesamiento, necesita agregar una referencia de ensamblado de su proyecto de código al ensamblado que define las colecciones integradas y el acceso al espacio de nombres correcto. Dependiendo de si desarrolla el ensamblado personalizado para un informe que se ejecuta en un servidor de informes (informe de servidor) o para un informe que se ejecuta localmente en una aplicación .NET (informe local), el ensamblado al que debe hacerse referencia será diferente. Consulte a continuación para más información.  
  
-   **Espacio de nombres:** Microsoft.ReportingServices.ReportProcessing.ReportObjectModel  
  
-   **Ensamblado (informe local):** Microsoft.ReportingServices.ProcessingObjectModel.dll  
  
-   **Ensamblado (informe de servidor):** Microsoft.ReportViewer.ProcessingObjectModel.dll  
  
 Como el contenido de las colecciones *Fields* y *ReportItems* puede cambiar dinámicamente en tiempo de ejecución, no las incluya en las llamadas al ensamblado personalizado (por ejemplo, en una variable miembro). La misma recomendación se aplica normalmente a todas las colecciones integradas.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar código a un informe &#40;SSRS&#41;](../../reporting-services/report-design/add-code-to-a-report-ssrs.md)   
 [Usar ensamblados personalizados con informes](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Agregar una referencia de ensamblado a un informe &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md)   
 [Tutoriales de Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)   
 [Ejemplos de expresiones &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Ejemplos de informes (Generador de informes y SSRS)](https://go.microsoft.com/fwlink/?LinkId=198283)  
  
