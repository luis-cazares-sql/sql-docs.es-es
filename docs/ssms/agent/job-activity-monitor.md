---
description: Monitor de actividad de trabajo
title: Monitor de actividad de trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.jobactivitymonitor.alljobs.f1
- SQL13.SWB.ACTIVITYMON.F1
ms.assetid: 11f2182c-5f71-46f8-8d2b-74f0fc48f2d6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 96a19ea760409d354390c171965f494f59cdff42
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477036"
---
# <a name="job-activity-monitor"></a>Monitor de actividad de trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Utilice esta página para ver la actividad actual de los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Haga clic en **Filtro** para limitar el número de trabajos mostrados. La cuadrícula **Actividad de trabajo del agente** es de solo lectura. Haga clic en los encabezados de columna para ordenar la cuadrícula. Si desea modificar un trabajo, haga doble clic en el trabajo para abrir el cuadro de diálogo **Propiedades del trabajo** . Haga clic con el botón secundario en un trabajo de la cuadrícula para que comiencen a ejecutarse todos los pasos del trabajo, para iniciar un paso del trabajo determinado, deshabilitar o habilitar el trabajo, actualizarlo, eliminarlo, ver su historial o ver sus propiedades. Haga clic en **Actualizar** para actualizar la cuadrícula con información actual.  
  
## <a name="options"></a>Opciones  
**Nombre**  
Nombre del trabajo.  
  
**Habilitado**  
Indica si el trabajo está habilitado (**sí**) o no (**no**).  
  
**Estado** _  
Estado actual del trabajo.  
  
_ *Resultado de la última ejecución**  
Estado del trabajo cuando se ejecutó por última vez.  
  
**Última ejecución**  
Fecha y hora en que se ejecutó el trabajo por última vez con la fecha y hora locales del servidor.  
  
**Próxima ejecución** _  
Fecha y hora en que se ha programado la próxima ejecución del trabajo con la fecha y hora locales del servidor.  
  
_ *Categoría**  
Categoría asignada al trabajo.  
  
**Ejecutable**  
**Sí** si el trabajo puede ejecutarse; **No** si el trabajo no puede ejecutarse. No puede ejecutarse un trabajo que no disponga de pasos o de un servidor de destino.  
  
**Programado**  
**Sí** si se ha asignado el trabajo a una programación de trabajo; **No** si el trabajo no tiene ninguna programación.  
  
*Solo los miembros con el rol fijo de servidor sysadmin de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el grupo de administradores del servidor puede ser los valores de esta columna. Los miembros del rol SQLAgentOperatorRole no pueden ver los valores en esta columna.  
  
#### <a name="to-open-the-job-activity-monitor"></a>Para abrir el Monitor de actividad de trabajo  
  
-   En el **Explorador de objetos**, expanda su servidor, expanda **Agente SQL Server**, haga clic con el botón derecho en **Monitor de actividad de trabajo** y, luego, haga clic en **Ver actividad de trabajo**.  
  
## <a name="see-also"></a>Consulte también  
[Actividad de trabajos de monitor](../../ssms/agent/monitor-job-activity.md)  
