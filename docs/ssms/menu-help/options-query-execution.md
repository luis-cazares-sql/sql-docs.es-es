---
title: 'Página Opciones de SQL Server: ejecución de consultas'
description: Opciones (ejecución de consultas)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Query_Execution.Sql_Server.General
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/13/2021
ms.openlocfilehash: 25ac37cdb9095151c90fdf81314d4c32044e6159
ms.sourcegitcommit: af64e2b8d498af26b973e86db5c00f8d72991295
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2021
ms.locfileid: "98193139"
---
# <a name="query-options-execution-general-page"></a>Ejecución de Opciones de consulta (página General)

Use esta página para especificar las opciones para ejecutar consultas de Microsoft SQL Server. Para tener acceso a este cuadro de diálogo, haga clic con el botón derecho en el cuerpo de una ventana del Editor de consultas y haga clic en Opciones de consulta.

- **SET ROWCOUNT** El valor predeterminado 0 indica que SQL Server esperará a que se reciban todos los resultados. Especifique un valor mayor que 0 si desea que SQL Server detenga la consulta después de obtener el número de filas especificado. Para desactivar esta opción (de modo que se devuelvan todas las filas), especifique SET ROWCOUNT 0.

- **SET TEXTSIZE** El valor predeterminado de 2 147 483 647 bytes indica que SQL Server proporcionará un campo de datos completo hasta el límite de los campos de datos text, ntext, nvarchar(max) y varchar(max). No afecta al tipo de datos XML. Especifique un número menor para limitar los resultados en caso de que los valores sean elevados. Las columnas que superen el número especificado se truncarán.

- **Execution time-out** (Tiempo de espera de ejecución) Indica el número de segundos de espera antes de cancelar la consulta. El valor 0 indica una espera infinita o que no hay tiempo de espera.

- **Batch separator** (Separador de lotes) Escriba la palabra que usará para separar instrucciones de Transact-SQL en lotes. El separador predeterminado es GO.

- **De forma predeterminada, abrir nuevas consultas en modo SQLCMD** Active esta casilla para abrir nuevas consultas en modo SQLCMD. Esta casilla solo se muestra cuando el cuadro de diálogo se abre desde el menú Herramientas.

    Cuando seleccione esta opción, tenga en cuenta las siguientes limitaciones:

    - IntelliSense se desactiva en el Editor de consultas del Motor de base de datos.

    - Debido a que el Editor de consultas no se ejecuta desde la línea de comandos, no podrá pasar parámetros de línea de comandos, tales como variables.

    - Dado que el Editor de consultas no puede responder a comandos del sistema operativo, debe tener cuidado de no ejecutar instrucciones interactivas.

- **Valores predeterminados** Restablece todos los valores de esta página a los valores predeterminados originales.