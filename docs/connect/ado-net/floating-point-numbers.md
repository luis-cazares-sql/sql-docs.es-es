---
title: Números de punto flotante
description: Obtenga información sobre algunos de los problemas al trabajar con números de punto flotante en el proveedor de datos SqlClient de Microsoft para SQL Server.
ms.date: 11/13/2020
ms.assetid: 73c218c6-1c44-4402-a167-4f6262629a91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1ca251c6de5ca6157ace8ee481bae02afa9fb2fb
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771578"
---
# <a name="floating-point-numbers"></a>Números de punto flotante

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

En este tema se describen algunos de los problemas que los desarrolladores suelen encontrar al trabajar con números de punto flotante en el proveedor de datos SqlClient de Microsoft para SQL Server. Estos problemas se deben a la forma en que los equipos almacenan los números de punto flotante y no son específicos de ningún proveedor determinado, como <xref:Microsoft.Data.SqlClient>.

## <a name="floating-point-pitfall"></a>Problema de punto flotante

En general, los números de punto flotante no tienen una representación binaria exacta. En realidad, el equipo almacena una aproximación del número. En diferentes momentos se pueden utilizar diferentes números de dígitos binarios para representar el número. Cuando un número de punto flotante se convierte de una representación a otra, los dígitos menos significativos de dicho número pueden variar ligeramente. Por lo general el cambio se produce cuando el número se convierte de un tipo a otro. La variación se produce si la conversión se realiza en una base de datos, entre tipos que representan valores de base de datos o entre tipos. Debido a estos cambios, los números que lógicamente deberían ser iguales pueden presentar cambios en sus dígitos menos significativos que hagan que muestren valores diferentes. La cantidad de dígitos de precisión en el número puede ser mayor o menor de la esperada. Cuando el formato del número cambia a cadena, puede que no muestre el valor esperado.

## <a name="suggested-workarounds"></a>Soluciones alternativas sugeridas

Para reducir estos efectos al mínimo, debe usar la coincidencia más próxima entre tipos de números que haya disponible. Por ejemplo, si trabaja con SQL Server, el valor numérico exacto puede cambiar si convierte un valor de tipo real de Transact-SQL a un valor de tipo flotante. En .NET, la conversión de <xref:System.Single> a <xref:System.Double> también puede producir resultados inesperados. En ambos casos, una estrategia adecuada consiste en establecer que todos los valores de la aplicación usen el mismo tipo numérico. También puede utilizar un tipo de decimal de precisión fija o bien convertir los números de punto flotante a un tipo decimal de precisión fija antes de trabajar con ellos.

Para solucionar problemas relacionados con la comparación de igualdad, puede codificar la aplicación de forma que se pasen por alto las diferentes en los dígitos menos significativos. Por ejemplo, en lugar de comparar dos números para comprobar si son iguales, puede restar un número del otro. Si la diferencia se sitúa en un margen aceptable de redondeo, la aplicación puede considerar los números como si fuesen iguales.

## <a name="see-also"></a>Consulte también

- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
