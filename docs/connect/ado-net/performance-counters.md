---
title: Contadores de rendimiento en SqlClient
description: Use los contadores de rendimiento del proveedor de datos SqlClient de Microsoft para SQL Server para supervisar el estado de la aplicación y sus recursos de conexión mediante el uso del Monitor de rendimiento de Windows o mediante programación.
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 0b121b71-78f8-4ae2-9aa1-0b2e15778e57
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e9d8c2edb88a9ed50b47c761d3af8aec8016065a
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804293"
---
# <a name="performance-counters-in-sqlclient"></a>Contadores de rendimiento en SqlClient

[!INCLUDE[appliesto-netfx-xxxx-xxxx-md](../../includes/appliesto-netfx-xxxx-xxxx-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Puede usar contadores de rendimiento de <xref:Microsoft.Data.SqlClient> para supervisar el estado de la aplicación y los recursos de conexión que usa. Los contadores de rendimiento se pueden controlar con el Monitor de rendimiento de Windows pero también se puede tener acceso a ellos mediante programación usando la clase <xref:System.Diagnostics.PerformanceCounter> del espacio de nombres <xref:System.Diagnostics>.

## <a name="available-performance-counters"></a>Contadores de rendimiento disponibles

Actualmente, hay 14 contadores de rendimiento distintos disponibles para <xref:Microsoft.Data.SqlClient>, tal como se describe en la tabla siguiente.

|Contador de rendimiento|Descripción|  
|-------------------------|-----------------|  
|`HardConnectsPerSecond`|El número de conexiones por segundo que se establecen con un servidor de bases de datos.|  
|`HardDisconnectsPerSecond`|El número de desconexiones por segundo que se producen con un servidor de bases de datos.|  
|`NumberOfActiveConnectionPoolGroups`|El número de conjuntos de grupos de conexiones únicas que están activos. Este contador depende del número de cadenas de conexión única que haya en el AppDomain.|  
|`NumberOfActiveConnectionPools`|El número total de grupos de conexiones.|  
|`NumberOfActiveConnections`|El número de conexiones activas que se están utilizando actualmente. **Nota:**  Este contador de rendimiento no está habilitado de manera predeterminada. Para habilitar este contador de rendimiento, consulte [Activación de contadores desactivados de manera predeterminada](#ActivatingOffByDefault).|  
|`NumberOfFreeConnections`|El número de conexiones que se pueden utilizar en los grupos de conexiones. **Nota:**  Este contador de rendimiento no está habilitado de manera predeterminada. Para habilitar este contador de rendimiento, consulte [Activación de contadores desactivados de manera predeterminada](#ActivatingOffByDefault).|  
|`NumberOfInactiveConnectionPoolGroups`|El número de conjuntos de grupos de conexiones únicas que están marcados para ser eliminados. Este contador depende del número de cadenas de conexión única que haya en el AppDomain.|  
|`NumberOfInactiveConnectionPools`|El número de grupos de conexiones inactivas que no han tenido ninguna actividad recientemente y que están a la espera de ser eliminadas.|  
|`NumberOfNonPooledConnections`|El número de conexiones activas que no están agrupadas.|  
|`NumberOfPooledConnections`|El número de conexiones activas que administra la infraestructura de agrupación de conexiones.|  
|`NumberOfReclaimedConnections`|El número de conexiones que se han reclamado a través de la recolección de elementos no utilizados si la aplicación no llamó a `Close` o `Dispose`. **Nota:** No cerrar o eliminar explícitamente las conexiones afecta el rendimiento.|  
|`NumberOfStasisConnections`|El número de conexiones que están actualmente en espera de que se finalice una acción y que por lo tanto no pueden ser utilizadas por la aplicación.|  
|`SoftConnectsPerSecond`|El número de conexiones activas que se están extrayendo del grupo de conexiones. **Nota:**  Este contador de rendimiento no está habilitado de manera predeterminada. Para habilitar este contador de rendimiento, consulte [Activación de contadores desactivados de manera predeterminada](#ActivatingOffByDefault).|  
|`SoftDisconnectsPerSecond`|El número de conexiones activas que se devuelven al grupo de conexiones. **Nota:**  Este contador de rendimiento no está habilitado de manera predeterminada. Para habilitar este contador de rendimiento, consulte [Activación de contadores desactivados de manera predeterminada](#ActivatingOffByDefault).|  

### <a name="connection-pool-groups-and-connection-pools"></a>Grupos de conjuntos de conexiones y grupos de conexiones

Si utiliza la autenticación de Windows (seguridad integrada) debe supervisar los contadores de rendimiento `NumberOfActiveConnectionPoolGroups` y `NumberOfActiveConnectionPools`. El motivo es que los conjuntos de grupos de conexiones se corresponden con cadenas de conexión única. Si se usa seguridad integrada, los grupos de conexiones se asignan a cadenas de conexión y además crean grupos diferentes para cada identidad de Windows. Por ejemplo, si Alfredo y Julia, los dos dentro del mismo AppDomain, utilizan la cadena de conexión `"Data Source=MySqlServer;Integrated Security=true"`, se crea un conjunto de grupos de conexiones para la cadena de conexión y dos grupos adicionales, uno para Alfredo y otro para Julia. Si Francisco y Marta usan una cadena de conexión con un inicio de sesión de SQL Server idéntico, `"Data Source=MySqlServer;User Id=<myUserID>;Password=<myPassword>"`, solo se creará un grupo para la identidad **<myUserID>** .

<a name="ActivatingOffByDefault"></a>

### <a name="activate-off-by-default-counters"></a>Activación de contadores desactivados de manera predeterminada

Los contadores de rendimiento `NumberOfFreeConnections`, `NumberOfActiveConnections`, `SoftDisconnectsPerSecond` y `SoftConnectsPerSecond` están desactivados de forma predeterminada. Agregue la siguiente información al archivo de configuración de la aplicación para habilitarlos:

```xml  
<system.diagnostics>  
  <switches>  
    <add name="ConnectionPoolPerformanceCounterDetail"  
         value="4"/>  
  </switches>  
</system.diagnostics>  
```  

## <a name="retrieve-performance-counter-values"></a>Recuperación de los valores de los contadores de rendimiento

La siguiente aplicación de consola muestra cómo recuperar valores de los contadores de rendimiento en su aplicación. Las conexiones deben estar abiertas y activas para que se devuelva la información de todos los contadores de rendimiento del proveedor de datos SqlClient de Microsoft para SQL Server.

> [!NOTE]
> En este ejemplo se usa la base de datos [**AdventureWorks** de ejemplo](../../samples/adventureworks-install-configure.md). Las cadenas de conexión proporcionadas en el código de ejemplo suponen que la base de datos está instalada y disponible en el equipo local, y que ha creado inicios de sesión que coinciden con los proporcionados en las cadenas de conexión. Quizá deba habilitar inicios de sesión de SQL Server si su servidor se ha configurado usando la configuración de seguridad predeterminada, que solo admite la autenticación de Windows. Modifique las cadenas de conexión según sea necesario para su entorno.

### <a name="example"></a>Ejemplo

[!code-csharp[SqlClient_PerformanceCounter#1](~/../sqlclient/doc/samples/SqlClient_PerformanceCounter.cs#1)]

## <a name="see-also"></a>Vea también

- [Conexión a un origen de datos](connecting-to-data-source.md)
- [Generación de perfiles en tiempo de ejecución](/dotnet/framework/debug-trace-profile/runtime-profiling)
- [Introducción a la supervisión de los umbrales de rendimiento](/previous-versions/visualstudio/visual-studio-2008/bd20x32d(v=vs.90))
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
