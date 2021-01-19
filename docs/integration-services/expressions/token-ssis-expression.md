---
description: TOKEN (expresión de SSIS)
title: TOKEN (expresión de SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 9fdd06bf-5bc9-445c-95bf-709e0ca5989b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cc99e599596658019c073b67dbf39b6dd7e573ce
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125614"
---
# <a name="token--ssis-expression"></a>TOKEN (expresión de SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Devuelve un token (subcadena) de una cadena basándose en los delimitadores especificados que separan los tokens en la cadena y el número del token que indica qué token se va a devolver.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
TOKEN(character_expression, delimiter_string, occurrence)  
```  
  
## <a name="arguments"></a>Argumentos  
 *character_expression*  
 Cadena que contiene los tokens separados por delimitadores.  
  
 *delimiter_string*  
 Cadena que contiene caracteres delimitadores. Por ejemplo, “; ,” contiene tres caracteres delimitadores: punto y coma, un espacio en blanco y una coma.  
  
 *occurrence*  
 Entero con o sin signo que especifica el token que se va a devolver. Por ejemplo, si especifica 3 como valor para este parámetro, se devuelve el tercer token de la cadena.  
  
## <a name="result-types"></a>Tipos de resultado  
 DT_WSTR  
  
## <a name="remarks"></a>Observaciones  
 Esta función divide la cadena <character_expression> en un conjunto de tokens separados por los delimitadores especificados en <delimiter_string> y devuelve el token número N, donde N es el número de aparición del token especificado por el parámetro \<occurrence>. Vea la sección Ejemplos para los usos de ejemplo de esta función.  
  
 Las observaciones siguientes se aplican a la función TOKEN:  
  
-   La cadena delimitadora puede contener uno o más caracteres delimitadores.  
  
-   Si el valor del parámetro \<occurrence> es mayor que el número total de tokens de la cadena, la función devuelve NULL.  
  
-   Los delimitadores iniciales se omiten.  
  
-   TOKEN solo funciona con el tipo de datos DT_WSTR. Un argumento *character_expression* que sea un literal de cadena o una columna de datos con el tipo de datos DT_STR se convertirá implícitamente al tipo de datos DT_WSTR antes de que TOKEN realice su operación. Los otros tipos de datos deben convertirse explícitamente al tipo de datos DT_WSTR.  
  
-   TOKEN devuelve un resultado NULL si el valor de character_expression es NULL.  
  
-   Puede usar variables y columnas como valores de todos los argumentos de la expresión.  
  
## <a name="expression-examples"></a>Ejemplos de expresiones  
 En el ejemplo siguiente, la función TOKEN devuelve "a". La cadena “a little white dog” tiene 4 tokens (“a”, “little”, “white” y “dog”) separados por el delimitador “ ” (carácter de espacio). El segundo argumento, una cadena delimitadora, especifica que solo un delimitador, el carácter de espacio, se utiliza en la división de la cadena de entrada en tokens. El último argumento, 1, especifica que se va a devolver el primer token. El primer token es “a” en esta cadena de ejemplo.  
  
```  
TOKEN("a little white dog"," ",1)  
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve "dog". La cadena delimitadora en este ejemplo contiene 5 delimitadores. La cadena de entrada contiene 4 tokens: “a”, “little”, “white”, “dog”.  
  
```  
TOKEN("a:little|white dog","| ,.:",4)  
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve "" (una cadena vacía) porque no hay 99 tokens en la cadena.  
  
```  
TOKEN("a little white dog"," ",99)  
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve la cadena completa. La función analiza la cadena de entrada para los delimitadores y, como hay ninguno en la cadena, simplemente agrega la cadena completa como primer token.  
  
```  
TOKEN("a little white dog","|",1)  
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve "a". Omite todos los caracteres de espacio iniciales.  
  
```  
TOKEN("        a little white dog", " ", 1)  
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve el año de una cadena de fecha.  
  
```  
TOKEN("2009/01/01", "/", 1)
```  
  
 En el ejemplo siguiente, la función TOKEN devuelve el nombre de archivo a partir de la ruta de acceso especificada. Por ejemplo, si el valor de User::Path es “C:\Archivos de programa\data\myfile.txt”, la función TOKEN devuelve “myfile.txt”. La función TOKENCOUNT devuelve 4 y la función TOKEN devuelve el cuarto token, “myfile.txt”.  
  
```  
TOKEN(@[User::Path], "\\", TOKENCOUNT(@[User::Path], "\\"))  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones &#40;expresión de SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
