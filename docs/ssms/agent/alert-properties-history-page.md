---
title: Propiedades de alerta (página Historial)
description: Propiedades de alerta (página Historial)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.history.f1
ms.assetid: f5359f5c-93a3-4a4a-8286-e9fe6f0196c7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4dd7bce8b5894ab272250ae68b74ff7a0eac2b1a
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878976"
---
# <a name="alert-properties-history-page"></a>Propiedades de alerta (página Historial)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.


Utilice esta página para ver y modificar el historial de las alertas del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="options"></a>Opciones  
**Fecha de la última alerta**  
Muestra la fecha en que se produjo el último evento especificado, o **(Nunca ocurrido)** si el evento no se ha producido desde que se creó la alerta.  
  
**Fecha de la última respuesta**  
Muestra la fecha en que la alerta respondió por última vez al evento, o **(Nunca respondió)** si el evento no se ha producido desde que se creó la alerta.  
  
**Número de repeticiones**  
Número total de repeticiones del evento desde que se creó la alerta, o desde la última vez que se restableció el recuento.  
  
**Restablecer cuenta**  
Restablece la información de esta página.  
  
## <a name="see-also"></a>Consulte también  
[Alertas](../../ssms/agent/alerts.md)  
