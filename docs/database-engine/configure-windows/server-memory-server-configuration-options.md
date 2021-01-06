---
title: Opciones de configuración de la memoria del servidor | Microsoft Docs
ms.custom: contperf-fy20q4
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Obtenga información sobre cómo configurar la cantidad de memoria que el administrador de memoria de SQL Server asigna a los procesos de SQL Server. Consulte diferentes enfoques y ejemplos de administración de la memoria.
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 71cab9c11aaa4f3dc569761fdfef407601162b1c
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878923"
---
# <a name="server-memory-configuration-options"></a>Opciones de configuración de la memoria del servidor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Vuelva a configurar la cantidad de memoria (en megabytes) para un proceso de SQL Server que usa una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  Existen dos opciones de memoria del servidor: **min server memory** y **max server memory**. Estas opciones cambian la cantidad de memoria que el administrador de memoria de SQL Server puede asignar a los procesos de SQL Server.
  
La configuración predeterminada y los valores mínimos permitidos para estas opciones son los siguientes:

|Opción  |  Valor predeterminado | Mínimo permitido  |
|---------|---------|---------|
|**memoria de servidor mínima**     |    0     |    0     |
|**memoria de servidor máxima**     |     2 147 483 647 megabytes (MB)     |  128 MB       |

De forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede cambiar sus requisitos de memoria de manera dinámica basándose en los recursos del sistema disponibles. Para obtener más información, consulte [Administración dinámica de memoria](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management).

> [!IMPORTANT]  
> Si establece el valor de **memoria de servidor máxima** en una cifra demasiado alta, puede producir que una única de instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compita por la memoria con otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hospedadas en el mismo host. Sin embargo, si establece este valor en una cifra demasiado baja, podría producir problemas de rendimiento y presión de memoria significativos. Si establece **Memoria de servidor máxima** en el valor mínimo, puede incluso evitar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inicie. Si no puede iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] después de cambiar esta opción, inicie esta herramienta mediante la opción de inicio **_-f_** y restablezca la opción **Memoria de servidor máxima** a su valor anterior. Para más información, consulte [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede usar memoria dinámicamente; sin embargo, es posible establecer las opciones de memoria manualmente y restringir la cantidad de memoria a la que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede acceder. Antes de establecer la cantidad de memoria para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], determine la configuración de memoria apropiada restando de la memoria física total la memoria necesaria para el sistema operativo, las asignaciones de memorias no controladas por la configuración max_server_memory y todas las demás instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (y otros usos del sistema, si el equipo no está dedicado totalmente a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Esta diferencia es la cantidad de memoria máxima que puede asignar a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] actual.  
 
## <a name="set-options-manually"></a><a name="manually"></a> Establecimiento de opciones manualmente  
Las opciones de servidor **memoria de servidor mínima** y **memoria de servidor máxima** pueden establecerse en un intervalo de valores de memoria. Este método es útil para que los administradores de bases de datos o de sistemas configuren una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] junto con los requisitos de memoria de otras aplicaciones u otras instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se ejecutan en el mismo host.

> [!NOTE]
> **Memoria de servidor mínima** y **Memoria de servidor máxima** son opciones avanzadas. Si usa el procedimiento almacenado del sistema **sp_configure** para cambiar estos valores, podrá cambiarlos solo si **Mostrar opciones avanzadas** tiene establecido el valor 1. Estos valores surten efecto inmediatamente, sin necesidad de reiniciar el servidor.  
  
<a name="min_server_memory"></a>Use **min_server_memory** para garantizar una cantidad mínima de memoria disponible para el Administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no asignará inmediatamente la cantidad de memoria especificada en **Memoria de servidor mínima** durante el inicio. No obstante, cuando el uso de memoria ha alcanzado este valor debido a una carga del cliente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede liberar memoria a menos que se reduzca el valor de **Memoria de servidor mínima** . Por ejemplo, cuando puede haber varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el mismo host simultáneamente, establezca el parámetro min_server_memory en vez de max_server_memory para reservar memoria para una instancia. Además, el establecimiento del valor min_server_memory es esencial en un entorno virtualizado para asegurarse de que la presión de memoria del host subyacente no intenta desasignar memoria del grupo de búferes en una máquina virtual (VM) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] invitada más allá de lo necesario para disfrutar de un rendimiento aceptable.

>[!NOTE]
>No es seguro que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asigne la cantidad de memoria especificada en **Memoria de servidor mínima**. Si la carga en el servidor no precisa nunca que se asigne la cantidad de memoria especificada en **Memoria de servidor mínima**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutará con menos memoria.

<a name="max_server_memory"></a>Use **max_server_memory** para garantizar que el sistema operativo no experimenta presión de memoria perjudicial. Para establecer la configuración de memoria de servidor máxima, supervise el consumo total del proceso [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para determinar los requisitos de memoria. Para ser más precisos con estos cálculos para una única instancia:
- Desde la memoria total del sistema operativo, reserve entre 1 y 4 GB para el propio sistema.
- Después reste el equivalente de las asignaciones de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potenciales al control **Memoria de servidor máxima**, que está formado por **tamaño de la pila <sup>1</sup> \* subprocesos de trabajo máximos calculados <sup>2</sup>** . El resto debería ser la configuración de max_server_memory para la instalación de una única instancia.

<sup>1</sup> Consulte la [guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md#stacksizes) para obtener información sobre los tamaños de pila de subprocesos por arquitectura.

<sup>2</sup> Consulte la página de documentación sobre cómo [Establecer la opción de configuración del servidor Máximo de subprocesos de trabajo](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) para obtener información sobre los subprocesos de trabajo predeterminados calculados para un determinado número de CPU con afinidad en el host actual.

## <a name="use-ssmanstudiofull"></a>Use [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
Use las dos opciones de memoria de servidor **memoria de servidor mínima** y **memoria de servidor máxima** para reconfigurar la cantidad de memoria (en megabytes) administrada por el Administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De forma predeterminada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede cambiar sus requisitos de memoria de manera dinámica basándose en los recursos del sistema disponibles.  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>Procedimiento para configurar una cantidad fija de memoria (no recomendado)  
Para establecer una cantidad fija de memoria:  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y seleccione **Propiedades**.  
  
2.  Haga clic en el nodo **Memoria** .  
  
3.  En **Opciones de memoria del servidor**, escriba la cantidad que desea para **Cantidad mínima de memoria del servidor** y **Cantidad máxima de memoria del servidor**.  
  
     Use la configuración predeterminada si desea que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueda cambiar dinámicamente sus requisitos de memoria según los recursos del sistema disponibles. Se recomienda establecer una **memoria de servidor máxima**, tal y como [se describe anteriormente](#max_server_memory). 

En la captura de pantalla siguiente se muestran los tres pasos: 

:::image type="content" source="media/server-memory-server-configuration-options/configure-memory-in-ssms.png" alt-text="Configuración de la memoria en SSMS":::

  
## <a name="lock-pages-in-memory-lpim"></a>Bloquear páginas en la memoria (LPIM) 
Esta directiva de Windows determina qué cuentas pueden usar un proceso para mantener los datos en la memoria física, impidiendo que el sistema realice la paginación de los datos en la memoria virtual del disco. El bloqueo de páginas en memoria puede mantener el servidor activo cuando se produce la paginación en la memoria del disco. La opción **Bloquear páginas en memoria** está establecida en ON en las instancias de la edición [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard y posterior cuando a la cuenta con privilegios para ejecutar sqlservr.exe se le ha concedido el derecho de usuario de Windows *Bloquear páginas en memoria* (LPIM).  
  
Para deshabilitar la opción **Bloquear páginas en memoria** para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], quite el derecho de usuario *Bloquear páginas en memoria* a la cuenta con privilegios que ejecuta la cuenta de inicio de sqlservr.exe (la cuenta de inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
 
La configuración de esta opción no afecta a la [administración dinámica de memoria](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], lo que permite expandir o contraer a petición de otros distribuidores de memoria. Cuando se usa el derecho de usuario *Bloquear páginas en la memoria* se recomienda establecer un límite superior para la **memoria de servidor máxima**, tal y como [se describe anteriormente](#max_server_memory).

> [!IMPORTANT]
> Esta opción solo se debería usar en caso necesario, principalmente si hay algún indicio de que el proceso sqlservr se está transfiriendo al almacenamiento auxiliar. En este caso, se notificará el error 17890 en el registro de errores, similar al ejemplo siguiente: `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.`
> A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], no se necesita la [marca de seguimiento 845](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) en Standard Edition para usar páginas bloqueadas. 
  
### <a name="to-enable-lock-pages-in-memory"></a>Para habilitar Bloquear páginas en la memoria  
Para habilitar la opción de bloqueo de páginas en memoria:  
  
1.  En el menú **Inicio** , haga clic en **Ejecutar**. En el cuadro **Abrir** , escriba **gpedit.msc**.  
  
     Se abrirá el cuadro de diálogo **Directiva de grupo** .  
  
2.  En la consola **Directiva de grupo** , expanda **Configuración del equipo** y, a continuación, expanda **Configuración de Windows**.  
  
3.  Expanda **Configuración de seguridad** y, a continuación, expanda **Directivas locales**.  
  
4.  Seleccione la carpeta **Asignación de derechos de usuario** .  
  
     Las directivas se mostrarán en el panel de detalles.  
  
5.  En el panel, haga doble clic en **Bloquear páginas en la memoria**.  
  
6.  En el cuadro de diálogo **Configuración de la directiva de seguridad local**, agregue la cuenta con privilegios para ejecutar sqlservr.exe (la cuenta de inicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
## <a name="multiple-instances-of-ssnoversion"></a>Varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Cuando esté ejecutando varias instancias de [!INCLUDE[ssDE](../../includes/ssde-md.md)], existen tres maneras con las que puede administrar la memoria:  
  
-   Usar **memoria de servidor máxima** para controlar el uso de memoria, tal y como [se detalla anteriormente](#max_server_memory). Establezca los valores máximos de cada instancia, teniendo cuidado de que la asignación total no sea mayor que la memoria física total de su equipo. Es buena idea proporcionar a cada instancia memoria proporcional a la carga de trabajo o al tamaño de la base de datos esperados. Este método tiene la ventaja de que cuando se inician nuevos procesos o instancias, habrá memoria libre para ellos de forma inmediata. El inconveniente es que si no está ejecutando todas las instancias, ninguna de las instancias que se están ejecutando podrá utilizar el resto de la memoria libre.  
  
-   Usar **memoria de servidor mínima** para controlar el uso de memoria, tal y como [se detalla anteriormente](#min_server_memory). Establezca la configuración mínima de cada instancia, de manera que la suma de estos mínimos sea 1-2 GB menos que la memoria física total de su equipo. De nuevo, puede establecer estos mínimos proporcionalmente a la carga de trabajo que se espera por cada instancia. Este método tiene la ventaja de que si no se ejecutan todas las instancias a la vez, las que se estén ejecutando pueden utilizar el resto de la memoria libre. Este método también resulta útil cuando en el equipo se está ejecutando otro proceso que consuma mucha memoria, puesto que asegura que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recibirá, al menos, una cantidad de memoria razonable. El inconveniente es que cuando se inicia una nueva instancia (o cualquier otro proceso), es posible que pase algún tiempo hasta que las instancias que se están ejecutando liberen memoria, especialmente si para ello deben escribir páginas modificadas en sus bases de datos.  
  
-   No hacer nada (no se recomienda). Las primeras instancias que se presenten con una carga de trabajo intentarán asignar toda la memoria. Puede que las instancias inactivas o las instancias que se inician más tarde terminen ejecutándose con una cantidad mínima de memoria disponible. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no intenta equilibrar el uso de memoria en todas las instancias. Sin embargo, todas las instancias responderán a las señales de notificación de memoria de Windows para ajustar el tamaño de su superficie de memoria. Windows no equilibra la memoria entre las aplicaciones con la API de notificación de memoria. Simplemente proporciona informes globales acerca de la disponibilidad de memoria del sistema.  
  
 Esta configuración se puede cambiar sin tener que reiniciar las instancias; por tanto, se puede experimentar fácilmente para encontrar la mejor configuración para el patrón de uso.  
  
## <a name="provide-the-maximum-amount-of-memory"></a>Especificación de la cantidad máxima de memoria

Se puede configurar memoria hasta el límite del espacio de direcciones virtuales de proceso en todas las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, consulte [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases#physical-memory-limits-windows-server-2016) (Límites de memoria para versiones de Windows y Windows Server).

## <a name="examples"></a>Ejemplos

### <a name="example-a-set-the-max-server-memory-option-to-4-gb"></a>Ejemplo A. Establecimiento de la opción de memoria de servidor máxima en 4 GB
 En el ejemplo siguiente se establece la opción `max server memory` en 4 GB.  Tenga en cuenta que, aunque `sp_configure` especifica el nombre de la opción como `max server memory (MB)`, el ejemplo muestra que se omite el elemento `(MB)`.

```sql
sp_configure 'show advanced options', 1;
GO
RECONFIGURE;
GO
sp_configure 'max server memory', 4096;
GO
RECONFIGURE;
GO
```
Esto dará como resultado una instrucción similar a la siguiente:

> La opción de configuración "Memoria de servidor máxima (MB)" ha cambiado de 2147483647 a 4096. Ejecute la instrucción RECONFIGURE para instalar.

### <a name="example-b-determining-current-memory-allocation"></a>Ejemplo B: Determinación de la asignación de memoria actual  
 La consulta siguiente devuelve información acerca de la memoria asignada actual.  

```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
   large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
   locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
   virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
   virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
   virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
   page_fault_count AS sql_page_fault_count,
   memory_utilization_percentage AS sql_memory_utilization_percentage, 
   process_physical_memory_low AS sql_process_physical_memory_low, 
   process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  

### <a name="example-c-determining-value-for-max-server-memory-mb"></a>Ejemplo C. Determinación del valor para "Memoria de servidor máxima (MB)"
La siguiente consulta devuelve información sobre el valor configurado actualmente y el que utiliza SQL Server.  Esta consulta devolverá resultados independientemente de si el valor "Mostrar opciones avanzadas" está establecido en "true".

```sql
SELECT c.value, c.value_in_use
FROM sys.configurations c WHERE c.[name] = 'max server memory (MB)'
```
  
## <a name="next-steps"></a>Pasos siguientes
 [Guía de arquitectura de administración de memoria](../../relational-databases/memory-management-architecture-guide.md)   
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [Ediciones y características admitidas de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [Ediciones y características admitidas de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Ediciones y características admitidas de SQL Server 2017 en Linux](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Memory Limits for Windows and Windows Server Releases](/windows/desktop/Memory/memory-limits-for-windows-releases) (Límites de memoria para versiones de Windows y Windows Server)
