---
title: Opción de configuración del servidor external scripts enabled
description: Obtenga información sobre la opción "external scripts enabled" en SQL Server. Después de activarla, puede ejecutar scripts externos en lenguajes compatibles, como R o Python.
ms.date: 06/30/2020
ms.prod: sql
ms.technology: machine-learning-services
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- external scripts enabled
- external_scripts_enabled_TSQL
helpviewer_keywords:
- external scripts enabled option
ms.assetid: 9d0ce165-8719-4007-9ae8-00f85cab3a0d
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 90a24ba11ce482e8d8608e1641d64ace067ad41b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170977"
---
# <a name="external-scripts-enabled-server-configuration-option"></a>Opción de configuración del servidor external scripts enabled
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Utilice la opción **external scripts enabled** para habilitar la ejecución de scripts con determinadas extensiones de lenguaje remoto. Esta propiedad está DESACTIVADA de forma predeterminada. Cuando se ha instalado **Machine Learning Services**, el programa de instalación puede opcionalmente establecer esta propiedad en true.

## <a name="remarks"></a>Observaciones

Debe habilitar la opción de script externo habilitado antes de poder ejecutar un script externo mediante el procedimiento [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) . Use **sp_execute_external_script** para ejecutar scripts escritos en un lenguaje compatible, como R o Python. 

+ Para [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]

    [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] es compatible con el lenguaje R en [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y un conjunto de herramientas de estación de trabajo y bibliotecas de conectividad de R.

    Instale la característica **Servicios de R** durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar la ejecución de los scripts de R.

+ Para [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] y versiones posteriores

    [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] es compatible con los lenguajes R y Python.

    Instale la característica **Machine Learning Services** durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar la ejecución de scripts externos. No olvide seleccionar al menos un lenguaje durante la instalación inicial: R, Python o ambos.
    
+ Para [!INCLUDE[ssSQL15](../../includes/sssqlv15-md.md)] y versiones posteriores, [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] admite todos los lenguajes de R, Python, Java y otros de terceros.

Instale la característica Machine Learning Services y extensiones de lenguaje durante la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para habilitar la ejecución de scripts externos para cualquier lenguaje compatible.

## <a name="additional-requirements"></a>Requisitos adicionales

Después de la instalación, ejecute el script siguiente para habilitar los scripts externos:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

Para obtener más información, vea [Instalación de SQL Server Machine Learning Services (Python y R) en Windows](../../machine-learning/install/sql-machine-learning-services-windows-install.md) o [Linux](../../linux/sql-server-linux-setup-machine-learning-docker.md?toc=/sql/machine-learning/toc.json).

## <a name="see-also"></a>Consulte también

+ [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
+ [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)
+ [sp_execute_external_script &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)
+ [Documentación del aprendizaje automático en SQL](../../machine-learning/index.yml)
