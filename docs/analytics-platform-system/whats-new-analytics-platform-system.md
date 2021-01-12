---
title: Novedades
description: Vea las novedades de Microsoft Analytics Platform System, un dispositivo local de escalado horizontal que hospeda MPP SQL Server almacenamiento de datos paralelos.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 004550d95ca69bae45c518b8054886e635e829df
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091788"
---
# <a name="whats-new-in-analytics-platform-system-a-scale-out-mpp-data-warehouse"></a>Novedades de Analytics Platform System, un almacenamiento de datos MPP de escalabilidad horizontal
Vea las novedades de las últimas actualizaciones del dispositivo para Microsoft Analytics Platform System (AP). APS es un dispositivo local de escalado horizontal que hospeda MPP SQL Server almacenamiento de datos paralelos. 

::: moniker range=">= aps-pdw-2016-au7 "
<a name="h2-aps-cu7.7"></a>
## <a name="aps-cu77"></a>APS CU7.7
Fecha de lanzamiento: noviembre 2020

### <a name="scvmm2016"></a>SCVMM2016
APS CU 7.7 software actualiza la máquina virtual de VMM a Windows Server 2016 e instala SCVMM2016. SCVMM 2012 R2 que está actualmente en uso tiene una fecha de finalización de la actualización del 2022 de julio. Es necesario que se admita la versión más reciente de SCVMM para que CU 7.7 sea una actualización obligatoria. Se recomienda a los clientes que actualicen a CU 7.7 lo antes posible.

### <a name="ssis-destination-adapter-for-sql-server-2019-as-target"></a>Adaptador de destino de SSIS para SQL Server 2019 como destino
El nuevo adaptador de destino de SSIS de APS que admite SQL Server 2019 como destino de implementación se puede descargar desde el [sitio de descarga](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.6"></a>
## <a name="aps-cu76"></a>APS CU7.6
Fecha de lanzamiento: abril 2020

### <a name="rename-column"></a>Cambio del nombre de columna
Después de actualizar a CU 7.6, los clientes podrán cambiar el nombre de una columna de una tabla creada por el usuario. Vea [Rename (Transact-SQL)](../t-sql/statements/rename-transact-sql.md) para ver la sintaxis, ejemplos, limitaciones y más información.

### <a name="alter-view"></a>Modificar vista
Los clientes ahora podrán modificar las vistas. Vea [ALTER View (Transact-SQL)](../t-sql/statements/alter-view-transact-sql.md) para obtener más información.

<a name="h2-aps-cu7.5"></a>
## <a name="aps-cu75"></a>APS CU7.5
Fecha de lanzamiento: septiembre 2019

### <a name="alter-external-data-source"></a>Modificar origen de datos externo
Los clientes podrán modificar la definición del origen de datos externo con la actualización CU 7.5. Los clientes con la alta disponibilidad del nodo de nombre de Hadoop ahora pueden modificar el origen de datos para cambiar los argumentos cuando se produce una conmutación por error. En el caso de los APS, solo se pueden cambiar la ubicación, la RESOURCE_MANAGER_LOCATION y la credencial. Vea [ALTER external Data Source](../t-sql/statements/alter-external-data-source-transact-sql.md?view=sql-server-2017&preserve-view=true) para obtener más información.

### <a name="cdh-515-and-516-support-with-polybase"></a>Compatibilidad con CDH 5,15 y 5,16 con polybase
Polybase en APS con la actualización CU 7.5 admite ahora CDH 5,15 y 5,16 versiones de la distribución de Hadoop desde Cloudera. Use la opción 6 para las versiones de CDH 5. x. 

### <a name="try_convert-and-try_cast-support"></a>Compatibilidad con Try_Convert y Try_Cast
CU 7.5 APS ahora admite funciones de [TRY_CAST](../t-sql/functions/try-cast-transact-sql.md?view=sql-server-2017&preserve-view=true) y [TRY_CONVERT](../t-sql/functions/try-convert-transact-sql.md?view=sql-server-2017&preserve-view=true) TSQL. Ambas funciones devuelven un valor convertido al tipo de datos especificado si la conversión se realiza correctamente; de lo contrario, devuelve NULL.

<a name="h2-aps-cu7.4"></a>
## <a name="aps-cu74"></a>APS CU7.4
Fecha de lanzamiento: 2019 de mayo

### <a name="loading-large-rows-with-dwloader"></a>Cargar filas grandes con dwloader
A partir de APS CU 7.4, los clientes podrán usar un nuevo dwloader para cargar filas en tablas de más de 32 KB (32.768 bytes). El nuevo dwloader admite el modificador-l que toma un valor entero entre 32768 y 33554432 (en bytes) para cargar filas de más de 32 KB. Use esta opción solo cuando se carguen grandes filas (más de 32 KB), ya que este modificador asignará más memoria en el cliente y en el servidor y puede ralentizar las cargas. Puede descargar el nuevo dwloader desde el [sitio de descarga](https://www.microsoft.com/download/details.aspx?id=57472).  

### <a name="hdp-30-and-31-support-with-polybase"></a>Compatibilidad con HDP 3,0 y 3,1 con polybase
Polybase en APS ahora admite HDP 3,0 y 3,1 con esta actualización. Use la opción 7 para las versiones de HDP 3. x. Para obtener más información, consulte la página de [conectividad de polybase](../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) .

### <a name="utf16-file-support-with-polybase"></a>Compatibilidad con archivos UTF16 con polybase
Polybase admite ahora la lectura de archivos de texto delimitados que están en la codificación UTF16 (LE). Vea [Create external File Format](../t-sql/statements/create-external-file-format-transact-sql.md) para obtener información detallada sobre la instalación. 

<a name="h2-aps-cu7.3"></a>
## <a name="aps-cu73"></a>APS CU7.3
Fecha de lanzamiento: diciembre 2018

### <a name="common-subexpression-elimination"></a>Eliminación de subexpresiones comunes
APS CU 7.3 mejora el rendimiento de las consultas con una eliminación común de subexpresiones en el optimizador de consultas de SQL. La mejora mejora las consultas de dos maneras. La primera ventaja es que la capacidad de identificar y eliminar estas expresiones ayuda a reducir el tiempo de compilación de SQL. La segunda ventaja y más importante son las operaciones de movimiento de datos para estas subexpresiones redundantes que se eliminan, por lo que el tiempo de ejecución de las consultas es más rápido. [Aquí](common-sub-expression-elimination.md)puede encontrar una explicación detallada de esta característica.

### <a name="aps-informatica-connector-for-informatica-1020-published"></a>AP Informatica Connector for Informatica 10.2.0 Published
Hemos publicado una nueva versión de los conectores de Informatica para APS que funciona con la versión de Informatica 10.2.0 y 10.2.0 revisión 1. Los nuevos conectores se pueden descargar desde el [sitio de descarga](https://www.microsoft.com/download/details.aspx?id=57472).
> [!NOTE]
> APS Informatica Connector for Informatica 10.2.0 o 10.2.0 Hotfix 1 no funciona en TLS 1.2 estricto y requiere que TLS 1.0 y 1,1 sean totalmente funcionales.

#### <a name="supported-versions"></a>Versiones admitidas

| Versión de APS | Informatica PowerCenter | Controlador |
|:---|:---|:---|
| APS 2016 | 9.6.1 | SQL Server Native Client 11. x |
| APS 2016 y versiones posteriores | 10.2.0, 10.2.0 revisión 1 | SQL Server Native Client 11. x |

<a name="h2-aps-cu7.2"></a>
## <a name="aps-cu72"></a>APS CU7.2
Fecha de lanzamiento: 2018 de octubre

### <a name="support-for-tls-12"></a>Compatibilidad con TLS 1.2
APS CU 7.2 admite TLS 1,2. El equipo cliente para la comunicación entre nodos APS y APS ahora se puede establecer para comunicarse solo a través de TLS 1.2. Las herramientas como SSDT, SSIS y Dwloader instaladas en equipos cliente que están configurados para comunicarse solo a través de TLS 1,2 pueden conectarse ahora a APS mediante TLS 1,2. De forma predeterminada, APS es compatible con todas las versiones de TLS (1,0, 1,1 y 1,2) por compatibilidad con versiones anteriores. Si desea establecer el dispositivo APS para usar estrictamente TLS 1,2, puede hacerlo cambiando la configuración del registro. 

Para obtener más información, consulte [configuración de TLS 1.2 en APS](configure-tls12-aps.md).

### <a name="hadoop-encryption-zone-support-for-polybase"></a>Compatibilidad de la zona de cifrado de Hadoop con polybase
Polybase ahora puede comunicarse con zonas de cifrado de Hadoop. Consulte cambios de configuración de APS necesarios en [configuración](polybase-configure-hadoop-security.md#encryptionzone)de la seguridad de Hadoop.

### <a name="insert-select-maxdop-options"></a>Insert-Select opciones maxdop
Hemos agregado un [modificador de características](appliance-feature-switch.md) que le permite elegir una configuración de maxdop mayor que 1 para las operaciones de inserción y selección. Ahora puede establecer el valor de maxdop en 0, 1, 2 o 4. El valor predeterminado es 1.

> [!IMPORTANT]  
> Al aumentar maxdop, a veces pueden producirse operaciones más lentas o errores de interbloqueo. Si esto ocurre, vuelva a cambiar la configuración a maxdop 1 y reintente la operación.

### <a name="columnstore-index-health-dmv"></a>DMV de mantenimiento de índice de almacén de columnas
Puede ver la información de mantenimiento del índice de almacén de columnas mediante **dm_pdw_nodes_db_column_store_row_group_physical_stats** DMV. Utilice la siguiente vista para determinar la fragmentación y decidir cuándo volver a generar o reorganizar un índice de almacén de columnas.

```sql
create view dbo.vCS_rg_physical_stats
as 
with cte
as
(
select   tb.[name]                    AS [logical_table_name]
,        rg.[row_group_id]            AS [row_group_id]
,        rg.[state]                   AS [state]
,        rg.[state_desc]              AS [state_desc]
,        rg.[total_rows]              AS [total_rows]
,        rg.[trim_reason_desc]        AS trim_reason_desc
,        mp.[physical_name]           AS physical_name
FROM    sys.[schemas] sm
JOIN    sys.[tables] tb               ON  sm.[schema_id]          = tb.[schema_id]                             
JOIN    sys.[pdw_table_mappings] mp   ON  tb.[object_id]          = mp.[object_id]
JOIN    sys.[pdw_nodes_tables] nt     ON  nt.[name]               = mp.[physical_name]
JOIN    sys.[dm_pdw_nodes_db_column_store_row_group_physical_stats] rg      ON  rg.[object_id]     = nt.[object_id]
                                                                            AND rg.[pdw_node_id]   = nt.[pdw_node_id]
                                        AND rg.[pdw_node_id]    = nt.[pdw_node_id]                                          
)
select *
from cte;
```

### <a name="polybase-date-range-increase-for-orc-and-parquet-files"></a>Aumento del intervalo de fechas de polybase para los archivos ORC y parquet
La lectura, importación y exportación de tipos de datos de fecha con polybase admite ahora fechas anteriores a 1970-01-01 y posteriores a 2038-01-20 para los tipos de archivo ORC y parquet.

### <a name="ssis-destination-adapter-for-sql-server-2017-as-target"></a>Adaptador de destino de SSIS para SQL Server 2017 como destino
El nuevo adaptador de destino de SSIS de APS que admite SQL Server 2017 como destino de implementación se puede descargar desde el [sitio de descarga](https://www.microsoft.com/download/details.aspx?id=57472).

<a name="h2-aps-cu7.1"></a>
## <a name="aps-cu71"></a>APS CU7.1
Fecha de lanzamiento: 2018 de julio

### <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>Los comandos DBCC no consumen espacios de simultaneidad (cambio de comportamiento)
APS admite un subconjunto de [comandos DBCC](../t-sql/database-console-commands/dbcc-transact-sql.md) de T-SQL, como [DBCC DROPCLEANBUFFERS](../t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql.md). Anteriormente, estos comandos consumían un [espacio de simultaneidad](./workload-management.md?view=aps-pdw-2016-au7&preserve-view=true&#concurrency-slots) reduciendo el número de cargas y consultas de usuario que podían ejecutarse. Los `DBCC` comandos se ejecutan ahora en una cola local que no consume un espacio de simultaneidad de usuarios que mejora el rendimiento general de la ejecución de consultas.

### <a name="replaces-some-metadata-calls-with-catalog-objects"></a>Reemplaza algunas llamadas de metadatos por objetos de catálogo
El uso de objetos de catálogo para llamadas de metadatos en lugar de usar SMO ha mostrado la mejora del rendimiento en APS. A partir de CU 7.1, algunas de estas llamadas de metadatos ahora usan objetos de catálogo de forma predeterminada. Este comportamiento se puede desactivar por el [modificador de características](appliance-feature-switch.md) si los clientes que usan consultas de metadatos se ejecutan en cualquier problema.

### <a name="bug-fixes"></a>Corrección de errores
Hemos actualizado a SQL Server 2016 SP2 CU2 con APS CU 7.1. La actualización corrige algunos problemas que se describen a continuación.

| Título | Descripción |
|:---|:---|
| **Posible interbloqueo del Movedor de tupla** |La actualización corrige una posibilidad prolongada de un interbloqueo en una transacción distribuida y un subproceso en segundo plano de tupla. Después de instalar CU 7.1, los clientes que usaban TF634 para detener el motor de tupla como SQL Server parámetro de inicio o marca de seguimiento global pueden quitarlo de forma segura. | 
| **Se produce un error en una consulta de intervalo o de adelanto** |Algunas consultas en tablas de CCI con funciones de retardo o de inicialización anidadas que generarían un error ahora se corrigen con esta actualización. | 


<a name="h2-aps-au7"></a>
## <a name="aps-au7"></a>APS AU7
Fecha de lanzamiento: 2018 de mayo

APS 2016 es un requisito previo para actualizar a AU7. A continuación se muestran nuevas características de APS AU7:

### <a name="auto-create-and-auto-update-statistics"></a>Creación automática y actualización automática de estadísticas
APS AU7 crea y actualiza automáticamente las estadísticas de forma predeterminada. Para actualizar la configuración de las estadísticas, los administradores pueden usar un nuevo elemento de menú de modificador de características en el [Configuration Manager](appliance-configuration.md#CMTasks). El [modificador de características](appliance-feature-switch.md) controla el comportamiento de la creación automática, la actualización automática y la actualización asincrónica de las estadísticas. También puede actualizar la configuración de las estadísticas con la instrucción [ALTER DATABASE (almacenamiento de datos paralelos)](../t-sql/statements/alter-database-transact-sql.md?tabs=sqlpdw) .

### <a name="t-sql"></a>T-SQL
@varAhora se admite Select. Para obtener más información, vea [seleccionar variable local](../t-sql/language-elements/select-local-variable-transact-sql.md) . 

Ahora se admiten las sugerencias de consulta HASH y el grupo de pedidos. Para obtener más información, vea [sugerencias (Transact-SQL)-Query](../t-sql/queries/hints-transact-sql-query.md)

### <a name="feature-switch"></a>Modificador de características
APS AU7 introduce el modificador de características en [Configuration Manager](launch-the-configuration-manager.md). AutoStatsEnabled y DmsProcessStopMessageTimeoutInSeconds son ahora opciones configurables que los administradores pueden cambiar.

### <a name="known-issues"></a>Problemas conocidos
Con el software APS AU7, se proporciona una actualización del BIOS de Intel que corrige un problema descrito como *ataques de canal lateral de ejecución especulativa*. Los ataques tienen como objetivo aprovechar lo que se denomina *Spectre y Meltdown vulnerabilidades*. Aunque se empaqueta junto con APS, la actualización del BIOS se instala manualmente y no como parte de la instalación del software APS AU7.

Microsoft recomienda que todos los clientes instalen el BIOS actualizado. Microsoft ha medido el efecto del sombreado de la dirección virtual del kernel (KVAS), el direccionamiento indirecto de la tabla de páginas del kernel (KPTI) y la mitigación de predicción de rama indirecta (IBP) en varias cargas de trabajo de SQL en varios entornos. Las medidas han detectado una degradación significativa en algunas cargas de trabajo. En función de los resultados, se recomienda probar el efecto de rendimiento de habilitar la actualización del BIOS antes de implementarla en un entorno de producción. Consulte la guía de SQL Server [aquí](https://support.microsoft.com/help/4073225/guidance-protect-sql-server-against-spectre-meltdown).

::: moniker-end
::: moniker range=">= aps-pdw-2016 "
<a name="h2-aps-au6"></a>
## <a name="aps-2016"></a>APS 2016
En esta sección se describen las nuevas características de APS 2016-AU6.

### <a name="sql-server-2016"></a>SQL Server 2016

APS AU6 se ejecuta en la versión más reciente SQL Server 2016 y usa el nivel de compatibilidad de base de datos predeterminado 130. SQL Server 2016 habilita la compatibilidad con nuevas características, como:

- Índices secundarios para los índices de almacén de columnas en clúster.
- Kerberos para polybase.

### <a name="t-sql"></a>T-SQL
APS AU6 admite estas mejoras de compatibilidad con T-SQL.  Estos elementos adicionales del lenguaje facilitan la migración desde SQL Server y otros orígenes de datos. 

- Ahora se admiten las [intercalaciones de SQL de nivel de columna][] , además de las intercalaciones de Windows.
- Los [índices no clúster en los índices de almacén de columnas agrupados][] mejoran el rendimiento de las consultas que buscan valores específicos en el índice de almacén de columnas agrupado. 
- [Seleccione... DENTRO][] 
- [sp_spaceused ()][] muestra el espacio en disco utilizado o reservado en una tabla o base de datos.
- La compatibilidad con [tablas anchas][] es igual que SQL Server 2016. Ya no existe el límite anterior de 32 K para el tamaño de la fila. 

**Tipos de datos**

- [VARCHAR (Max)][], [nvarchar (Max)][] y [varbinary (Max)][]. Estos tipos de datos LOB tienen un tamaño máximo de 2 GB. Para cargar estos objetos, utilice la [utilidad BCP][]. Polybase y dwloader no admiten estos tipos de datos actualmente. 
- [PREDETERMINADO][]
- [UNIQUEIDENTIFIER][]
- Tipos de datos [numéricos][] y decimales.

**Funciones de ventana**

- [Filas o intervalo][] en la cláusula over de la instrucción SELECT.
- [FIRST_VALUE][]
- [LAST_VALUE][]
- [CUME_DIST][]
- [PERCENT_RANK][]

**Funciones de seguridad**

- [Checksum ()][] y [BINARY_CHECKSUM ()][]
- [HAS_PERMS_BY_NAME ()][]

**Funciones adicionales**

- [NEWID ()][]
- [RAND ()][]

### <a name="polybasehadoop-enhancements"></a>Mejoras de polybase y Hadoop

- Compatibilidad con Hortonworks HDP 2,4 y HDP 2,5
- Compatibilidad con Kerberos a través de credenciales con ámbito de base de datos
- Compatibilidad con credenciales con blobs Azure Storage

### <a name="install-and-upgrade-enhancements"></a>Mejoras en la instalación y la actualización

**Actualizaciones de arquitectura de empresa** La actualización del dispositivo existente a APS AU6 instala las últimas actualizaciones de firmware y controlador, que incluyen revisiones de seguridad. 

Un nuevo dispositivo de HPE o DELL incluye todas las actualizaciones más recientes más:

- Compatibilidad con el procesador de última generación (Broadwell)
- Actualización de DDR4 DIMMs
- Rendimiento de DIMM mejorado

**Integración**

- La compatibilidad con el nombre de dominio completo (FQDN) permite configurar una confianza de dominio en el dispositivo. 
- Para usar el FQDN, debe realizar una actualización completa y participar durante la actualización. 

**Tiempo de inactividad reducido** La instalación o actualización a APS AU6 es más rápida y requiere menos tiempo de inactividad que las versiones anteriores. Para reducir el tiempo de inactividad, instale o actualice: 

 - Simplifica la aplicación de actualizaciones de WSUS mediante el uso de una imagen que contiene todas las actualizaciones hasta el 2016 de junio
 - Aplica las actualizaciones de seguridad con las actualizaciones del firmware y del controlador
 - Coloca las revisiones más recientes y la utilidad de comprobación de dispositivos (PAV) en el dispositivo para que estén listas para instalarse sin necesidad de descargarlas.

::: moniker-end

<!--
Link references to other articles in this same GitHub repo.

The link format that starts with '/sql/what-ever/my-artlcle' is not appropriate for common links within the same repo (as most of these link are).  The first couple links have been edited to show the proper syntax, but all other links in this article need to be similarly edited.
The proper formats have at least two big advantages.  One big advantage is that the proper formats enable the OPS Build system to detect broken links at Pull Request build time, instead of only later during run time.
-->
[database compatibility level 130]: ../t-sql/statements/alter-database-transact-sql-compatibility-level.md
[Intercalaciones de SQL de nivel de columna]: ~/relational-databases/collations/collation-and-unicode-support.md

[Índices no clúster en índices de almacén de columnas agrupados]:/sql/t-sql/statements/create-index-transact-sql
[VARCHAR (MAX)]:/sql/t-sql/data-types/char-and-varchar-transact-sql
[NVARCHAR (MAX)]:/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[VARBINARY (MAX)]:/sql/t-sql/data-types/binary-and-varbinary-transact-sql
[PREDETERMINADO]:/sql/relational-databases/system-catalog-views/sys-types-transact-sql
[Seleccione... DENTRO]:/sql/t-sql/queries/select-into-clause-transact-sql
[sp_spaceused ()]:/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql
[Tablas anchas]:/sql/sql-server/maximum-capacity-specifications-for-sql-server
[BULK INSERT]:/sql/t-sql/statements/bulk-insert-transact-sql
[bcp (utilidad)]:/sql/tools/bcp-utility
[UNIQUEIDENTIFIER]:/sql/t-sql/data-types/uniqueidentifier-transact-sql
[NUMERIC]:/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[ROWS o RANGE]:/sql/t-sql/queries/select-over-clause-transact-sql
[FIRST_VALUE]:/sql/t-sql/functions/first-value-transact-sql
[LAST_VALUE]:/sql/t-sql/functions/last-value-transact-sql
[CUME_DIST]:/sql/t-sql/functions/cume-dist-transact-sql
[PERCENT_RANK]:/sql/t-sql/functions/percent-rank-transact-sql
[SUMA de comprobación ()]:/sql/t-sql/functions/checksum-transact-sql
[BINARY_CHECKSUM ()]:/sql/t-sql/functions/binary-checksum-transact-sql
[HAS_PERMS_BY_NAME ()]:/sql/t-sql/functions/has-perms-by-name-transact-sql
[NEWID ()]:/sql/t-sql/functions/newid-transact-sql
[RAND ()]:/sql/t-sql/functions/rand-transact-sql


  

