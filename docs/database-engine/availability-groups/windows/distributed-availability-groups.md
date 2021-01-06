---
title: Concepto de grupo de disponibilidad distribuido
description: Un grupo de disponibilidad distribuido es un tipo especial de grupo de disponibilidad que abarca dos tipos distintos de grupo de disponibilidad. No es necesario que los grupos de disponibilidad que participan en un grupo de disponibilidad distribuido estén en la misma ubicación.
ms.custom: seodec18
ms.date: 10/15/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], distributed
ms.assetid: ''
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0b6a1c97a1a5958811111915117793da8c416a0
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643645"
---
# <a name="distributed-availability-groups"></a>Grupos de disponibilidad distribuidos
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
Los grupos de disponibilidad distribuidos son una característica nueva que se incluyó en SQL Server 2016 como variante de la característica Grupos de disponibilidad AlwaysOn existente. En este artículo se aclaran algunos aspectos de los grupos de disponibilidad distribuidos y se complementa la [documentación de SQL Server](../../../sql-server/index.yml) existente.

> [!NOTE]
> "DAG" no es la abreviatura oficial de *grupo de disponibilidad distribuido*, porque esa abreviatura ya se usa para hacer referencia a la característica de grupo de disponibilidad de base de datos de Exchange. Esta característica de Exchange no tiene relación alguna con los grupos de disponibilidad de SQL Server ni con los grupos de disponibilidad distribuidos.

Para configurar un grupo de disponibilidad distribuido, vea [Configure distributed availability groups](configure-distributed-availability-groups.md) (Configurar grupos de disponibilidad distribuidos).

## <a name="understand-distributed-availability-groups"></a>Conceptos de los grupos de disponibilidad distribuidos

Un grupo de disponibilidad distribuido es un tipo especial de grupo de disponibilidad que abarca dos tipos distintos de grupo de disponibilidad. No es necesario que los grupos de disponibilidad que participan en un grupo de disponibilidad distribuido estén en la misma ubicación. Pueden ser físicos, virtuales, hospedarse localmente, en la nube pública o en cualquier lugar donde se puedan implementar grupos de disponibilidad. Esto incluye grupos entre dominios y entre plataformas, como aquellos entre un grupo de disponibilidad hospedado en Linux y otro hospedado en Windows. Siempre que dos grupos de disponibilidad puedan comunicarse, se podrá configurar un grupo de disponibilidad distribuido con ellos.

Los grupos de disponibilidad tradicionales tienen recursos configurados en un clúster WSFC, mientras que los grupos de disponibilidad distribuidos no tienen nada configurado en este tipo de clúster: todo se mantiene en SQL Server. Para más información sobre cómo consultar información relativa a un grupo de disponibilidad distribuido, vaya a [Ver la información de grupo de disponibilidad distribuido](#monitor-distributed-availability-group-health). 

Un grupo de disponibilidad distribuido requiere que los grupos de disponibilidad subyacentes tengan un agente de escucha. Al contrario de lo que haría en un grupo de disponibilidad tradicional, en vez de proporcionar el nombre de servidor subyacente de una instancia independiente (o, si se trata de una instancia de clúster de conmutación por error de SQL Server, el valor asociado al recurso de nombre de red), cuando se cree el grupo de disponibilidad distribuido hay que especificar el agente de escucha configurado por medio del parámetro ENDPOINT_URL. Aunque cada grupo de disponibilidad subyacente del grupo de disponibilidad distribuido tiene un agente de escucha, el grupo de disponibilidad distribuido carece de dicho agente.

En la siguiente imagen se muestra una vista de alto nivel de un grupo de disponibilidad distribuido que abarca dos grupos de disponibilidad (AG 1 y AG 2), cada uno de ellos configurado en su propio clúster WSFC. El grupo de disponibilidad distribuido tiene un total de cuatro réplicas, dos en cada grupo de disponibilidad. Cada grupo de disponibilidad puede admitir hasta el número máximo de réplicas, para que una disponibilidad distribuida pueda tener hasta 18 réplicas en total.


![Vista general de un grupo de disponibilidad distribuido](./media/distributed-availability-group/dag-01-high-level-view-distributed-ag.png)

El movimiento de datos en los grupos de disponibilidad distribuidos se puede configurar como sincrónico o como asincrónico. Con todo, el movimiento de datos en el caso de los grupos de disponibilidad distribuidos es ligeramente distinto al de los grupos de disponibilidad tradicionales. A pesar de que cada grupo de disponibilidad tiene una réplica principal, solo hay una copia de las bases de datos que participan en un grupo de disponibilidad distribuido que puede aceptar inserciones, actualizaciones y eliminaciones. Como se aprecia en la siguiente imagen, AG 1 es el grupo de disponibilidad principal. Su réplica principal envía transacciones tanto a las réplicas secundarias de AG 1 como a la réplica principal de AG 2. La réplica principal de AG 2 también se denomina *reenviador*. Un reenviador es una réplica principal de un grupo de disponibilidad secundario en un grupo de disponibilidad distribuido. El reenviador recibe las transacciones de la réplica principal del grupo de disponibilidad principal y las reenvía a las réplicas secundarias de su propio grupo de disponibilidad.  De esta forma, el reenviador mantiene actualizadas las réplicas secundarias de AG 2. 

![Grupo de disponibilidad distribuido y movimiento de datos](./media/distributed-availability-group/dag-02-distributed-ag-data-movement.png)

La única manera de que la réplica principal de AG 2 acepte inserciones, actualizaciones y eliminaciones es conmutar por error el grupo de disponibilidad distribuido de AG 1 manualmente. En la imagen anterior, como AG 1 contiene la copia de la base de datos de escritura, cuando se realiza una conmutación por error, AG 2 se convierte en el grupo de disponibilidad que puede controlar las inserciones, actualizaciones y eliminaciones. Para más información sobre cómo realizar una conmutación por error de un grupo de disponibilidad distribuido a otro, vea [Conmutación por error a un grupo de disponibilidad secundario](configure-distributed-availability-groups.md#failover).

> [!NOTE]
> En los grupos de disponibilidad distribuidos de SQL Server 2016, la conmutación por error de un grupo de disponibilidad a otro solo puede realizarse a través de la opción FORCE_FAILOVER_ALLOW_DATA_LOSS.

> [!NOTE]
> Al usar la replicación transaccional con grupos de disponibilidad distribuidos, la réplica del reenviador no puede configurarse como publicador.

## <a name="sql-server-version-and-edition-requirements-for-distributed-availability-groups"></a>Requisitos de versión y edición de SQL Server de los grupos de disponibilidad distribuidos

Los grupos de disponibilidad distribuidos de SQL Server 2017 o una versión posterior pueden mezclar versiones principales de SQL Server en el mismo grupo de disponibilidad distribuido. El grupo de disponibilidad que contiene el principal de lectura y escritura puede tener la misma versión (o una inferior) que los demás grupos de disponibilidad que participan en el grupo de disponibilidad distribuido. Los otros grupos de disponibilidad pueden tener la misma versión o versiones posteriores. Este escenario está dirigido a escenarios de actualización y migración. Por ejemplo, si el grupo de disponibilidad que contiene la réplica principal de lectura y escritura es SQL Server 2016, pero quiere actualizar o migrar a SQL Server 2017 o versiones posteriores, el otro grupo de disponibilidad que participa en el grupo de disponibilidad distribuido se puede configurar con SQL Server 2017.

Dado que la característica de grupos de disponibilidad distribuidos no existía en SQL Server 2012 ni en 2014, los grupos de disponibilidad que se crearon con esas versiones no pueden participar en grupos de disponibilidad distribuidos. 

> [!NOTE]
> No se pueden configurar grupos de disponibilidad distribuidos con la edición Standard o una combinación de las ediciones Standard y Enterprise.

Dado que hay dos grupos de disponibilidad diferentes, el proceso para instalar un Service Pack o una actualización acumulativa en una réplica que participa en un grupo de disponibilidad distribuido es ligeramente diferente del de un grupo de disponibilidad tradicional:

1. Empiece actualizando las réplicas del segundo grupo de disponibilidad del grupo de disponibilidad distribuido.

2. Aplique revisiones a las réplicas del grupo de disponibilidad principal del grupo de disponibilidad distribuido.

3. Como haría con un grupo de disponibilidad estándar, conmute por error el grupo de disponibilidad principal a una de sus propias réplicas (y no a la réplica principal del segundo grupo de disponibilidad) y aplíquele las revisiones pertinentes. Si no hay ninguna réplica aparte de la principal, será necesario realizar una conmutación por error manual al segundo grupo de disponibilidad.

> [!NOTE]
> No hay ningún anuncio relativo a si en futuras versiones de SQL Server se va a permitir que las versiones anteriores puedan participar en el mismo grupo de disponibilidad distribuido. Si este escenario fuera factible, permitiría que los grupos de disponibilidad distribuidos formaran parte de un plan de actualización de versión de SQL Server.

### <a name="windows-server-versions-and-distributed-availability-groups"></a>Grupos de disponibilidad distribuidos y versiones de Windows Server

Un grupo de disponibilidad distribuido abarca varios grupos de disponibilidad (cada uno en su propio clúster WSFC subyacente) y es además una construcción solo de SQL Server.  Esto significa que los clústeres WSFC que hospedan grupos de disponibilidad individuales pueden tener versiones principales de Windows Server distintas. Las versiones principales de SQL Server deben ser la misma, como se ha indicado en la sección anterior. De manera bastante parecida a la imagen inicial, la siguiente imagen muestra los grupos AG 1 y AG 2 que participan en un grupo de disponibilidad distribuido, pero cada clúster WSFC pertenece a una versión de Windows Server distinta.


![Grupos de disponibilidad distribuidos con clústeres WSFC con distintas versiones de Windows Server](./media/distributed-availability-group/dag-03-distributed-ags-wsfcs-different-versions-windows-server.png)

Los clústeres WSFC individuales y sus correspondientes grupos de disponibilidad siguen las reglas tradicionales; es decir, pueden estar o no unidos a un dominio (Windows Server 2016 o posterior). Cuando dos grupos de disponibilidad diferentes se combinan en un único grupo de disponibilidad distribuido, hay cuatro escenarios posibles:

* Ambos clústeres WSFC están unidos al mismo dominio.
* Cada clúster WSFC está unido a un dominio distinto.
* Un clúster WSFC está unido a un dominio y el otro no.
* Ninguno de los dos clústeres WSFC está unido a un dominio.

Cuando ambos clústeres WSFC están unidos al mismo dominio (dominios que no son de confianza), no es necesario hacer nada especial al crear el grupo de disponibilidad distribuido. En el caso de los grupos de disponibilidad y clústeres WSFC que no están unidos al mismo dominio, use certificados para que el grupo de disponibilidad distribuido funcione, como haría en gran medida al crear un grupo de disponibilidad para un grupo de disponibilidad independiente del dominio. Para saber cómo configurar certificados para un grupo de disponibilidad distribuido, siga los pasos 3-13 de [Create a domain-independent availability group](domain-independent-availability-groups.md) (Crear un grupo de disponibilidad independiente del dominio).

En un grupo de disponibilidad distribuido, las réplicas principales de cada grupo de disponibilidad subyacente deben tener los certificados de todas las demás. Si ya tiene puntos de conexión que no usan certificados, reconfigure esos puntos de conexión con [ALTER ENDPOINT](../../../t-sql/statements/alter-endpoint-transact-sql.md) para reflejar el uso de certificados.

## <a name="distributed-availability-group-usage-scenarios"></a>Escenarios de uso de grupos de disponibilidad distribuidos

Estos son los tres escenarios más importantes de uso de un grupo de disponibilidad distribuido: 

* [Recuperación ante desastres y configuraciones de varios sitios más sencillas](#disaster-recovery-and-multi-site-scenarios)
* [Migración a un nuevo hardware o nuevas configuraciones, lo que puede conllevar el uso de hardware nuevo o el cambio de los sistemas operativos subyacentes](#migrate-by-using-a-distributed-availability-group)
* [Expansión de varios grupos de disponibilidad para aumentar el número de réplicas legibles a más de ocho en un mismo grupo de disponibilidad](#scale-out-readable-replicas-with-distributed-availability-groups)

### <a name="disaster-recovery-and-multi-site-scenarios"></a>Escenarios de recuperación ante desastres y configuraciones de varios sitios

Un grupo de disponibilidad tradicional requiere que todos los servidores formen parte del mismo clúster WSFC, de modo que la expansión de varios centros de datos puede suponer un desafío. En la siguiente imagen se muestra cómo es una arquitectura de grupo de disponibilidad tradicional de varios sitios, incluido el flujo de datos. No hay una réplica principal que envíe transacciones a todas las réplicas secundarias. Esta configuración es menos flexible en cierto modo que la de un grupo de disponibilidad distribuido. Por ejemplo, deberá implementar elementos como Active Directory (si procede) y el testigo de un cuórum en el clúster WSFC. También tendrá que tener en cuenta otros aspectos de un clúster WSFC, como modificar los votos de nodos.

![Grupo de disponibilidad tradicional de varios sitios](./media/distributed-availability-group/dag-04-traditional-multi-site-ag.png)

Los grupos de disponibilidad distribuidos ofrecen un escenario de implementación más flexible para los grupos de disponibilidad que abarcan varios centros de datos. Se pueden usar grupos de disponibilidad distribuidos donde antes se usaban características como el [trasvase de registros]( https://docs.microsoft.com/sql/database-engine/log-shipping/about-log-shipping-sql-server) para escenarios como la recuperación ante desastres. Pero, a diferencia del trasvase de registros, los grupos de disponibilidad distribuidos no pueden retrasar la aplicación de transacciones. Esto significa que estos grupos no son de ayuda en caso de que se cometa un error humano por el cual se actualizan o eliminan datos incorrectamente.

El emparejamiento de los grupos de disponibilidad distribuidos no es muy fuerte, lo que se traduce en que no necesitan un único clúster WSFC. Se mantienen en SQL Server. Dado que los clústeres WSFC se mantienen individualmente y la sincronización es básicamente asincrónica entre los dos grupos de disponibilidad, es más fácil configurar la recuperación ante desastres en otro sitio. Las réplicas principales de cada grupo de disponibilidad sincronizan sus propias réplicas secundarias.

* Los grupos de disponibilidad distribuidos solo admiten la conmutación por error manual. En una situación de recuperación ante desastres donde se intercambian centros de datos, conviene no configurar la conmutación automática por error (salvo raras excepciones). 
* Probablemente no necesitará establecer algunos de los elementos o parámetros tradicionales de los clústeres WSFC de varios sitios o subred (como CrossSubnetThreshold), pero sí sigue siendo necesario tener presente la latencia de red en una capa diferente para el transporte de datos. La diferencia es que cada clúster WSFC mantiene su propia disponibilidad; el clúster no es una gran entidad de cuatro nodos. Existen dos clústeres WSFC de dos nodos independientes, como muestra la imagen anterior.  
* Recomendamos el movimiento de datos asincrónico, porque con este método la recuperación ante desastres sería viable.
* Si configura un movimiento de datos sincrónico entre la réplica principal y, al menos, una réplica secundaria del segundo grupo de disponibilidad y un movimiento sincrónico en el grupo de disponibilidad distribuido, el grupo de disponibilidad distribuido esperará hasta que todas las copias sincrónicas confirmen que tienen los datos.

### <a name="migrate-by-using-a-distributed-availability-group"></a>Migrar por medio de un grupo de disponibilidad distribuido

Los grupos de disponibilidad distribuidos admiten dos configuraciones de grupo de disponibilidad completamente diferentes, lo que permite no solo escenarios de varios sitios y de recuperación ante desastres más sencillos, sino también escenarios de migración. Tanto si va a migrar a un nuevo hardware o máquinas virtuales (locales o IaaS en la nube pública), la configuración de un grupo de disponibilidad distribuido facilita una migración cuando, anteriormente, había que recurrir a trabajos de copia de seguridad, copia y restauración o al trasvase de registros. 

La capacidad de migrar es especialmente útil en escenarios donde el sistema operativo subyacente se va a cambiar o a actualizar mientras se mantiene la misma versión de SQL Server. Si bien Windows Server 2016 permite una actualización gradual desde Windows Server 2012 R2 en el mismo hardware, la mayoría de los usuarios opta por implementar nuevo hardware o máquinas virtuales. 

Para completar la migración a la nueva configuración, cuando el proceso llegue a su fin, detenga todo el tráfico de datos al grupo de disponibilidad original y cambie el grupo de disponibilidad distribuido al movimiento de datos sincrónico. Con ello, se garantiza que la réplica principal del segundo grupo de disponibilidad va a estar completamente sincronizada, sin perder ningún dato. Tras comprobar la sincronización, conmute por error el grupo de disponibilidad distribuido al grupo de disponibilidad secundario. Para obtener más información, consulte [Fail over to a secondary availability group (Conmutación por error de un grupo de disponibilidad secundario)](configure-distributed-availability-groups.md#failover).

Después de la migración, cuando el segundo grupo de disponibilidad es ya el nuevo grupo de disponibilidad principal, es posible que tenga que realizar una de las siguientes acciones:

* Cambiar el nombre del agente de escucha del grupo de disponibilidad secundario (y, posiblemente, eliminar el agente anterior del grupo de disponibilidad principal original o cambiarlo de nombre) o volver a crearlo con el agente de escucha del grupo de disponibilidad principal original, de forma que las aplicaciones y los usuarios puedan acceder a la nueva configuración.
* Si no puede volver a crearlo ni cambiarlo de nombre, dirija las aplicaciones y los usuarios al agente de escucha del segundo grupo de disponibilidad.

### <a name="scale-out-readable-replicas-with-distributed-availability-groups"></a>Escalar horizontalmente réplicas legibles con grupos de disponibilidad distribuidos

Un solo grupo de disponibilidad distribuido puede tener hasta 16 réplicas secundarias legibles. Esto significa que puede tener hasta 18 copias de lectura, si incluimos las dos réplicas principales de los diferentes grupos de disponibilidad. Este método permite que más de un sitio pueda tener acceso casi en tiempo real para notificar a diversas aplicaciones.

Con los grupos de disponibilidad distribuidos, el escalado horizontal de una granja de servidores de solo lectura es más fácil que con simplemente un único grupo de disponibilidad. Un grupo de disponibilidad distribuido puede escalar réplicas legibles horizontalmente de dos maneras:

* Se puede usar la réplica principal del segundo grupo de disponibilidad de un grupo de disponibilidad distribuido para crear otro grupo de disponibilidad distribuido, aun cuando la base de datos no esté en estado RECOVERY.
* También puede usar la réplica principal del primer grupo de disponibilidad para crear otro grupo de disponibilidad distribuido.

Dicho de otro modo, una réplica principal puede participar en dos grupos de disponibilidad distribuidos diferentes. En la siguiente imagen se muestran dos grupos, AG 1 y AG 2, y ambos participan en el grupo distribuido AG 1, mientras que AG 2 y AG 3 participan en el grupo distribuido AG 2. La réplica principal (o reenviador) de AG 2 es al mismo tiempo una réplica secundaria del grupo Distributed AG 1 y una réplica principal del grupo Distributed AG 2.

![Escalar lecturas horizontalmente con grupos de disponibilidad distribuidos](./media/distributed-availability-group/dag-05-scaling-out-reads-with-distributed-ags.png)

En la siguiente imagen, AG 1 es la réplica principal de dos grupos de disponibilidad distribuidos diferentes: Distributed AG 1 (compuesto por AG 1 y AG 2) y Distributed AG 2 (compuesto por AG 1 y AG 3).


![Otro ejemplo de escalado horizontal de lecturas con grupos de disponibilidad distribuidos]( ./media/distributed-availability-group/dag-06-another-scaling-out-reads-using-distributed-ags-example.png)


En los dos ejemplos anteriores, puede haber hasta 27 réplicas en total entre los tres grupos de disponibilidad, y todas ellas se pueden usar para consultas de solo lectura. 

El [enrutamiento de solo lectura]( https://docs.microsoft.com/sql/database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server) no funciona completamente con los grupos de disponibilidad distribuidos. Más concretamente:

1. El enrutamiento de solo lectura se puede configurar, y funcionará con el grupo de disponibilidad principal del grupo de disponibilidad distribuido. 
2. El enrutamiento de solo lectura se puede configurar, pero no funcionará con el grupo de disponibilidad secundario del grupo de disponibilidad distribuido. Todas las consultas, si usan el agente de escucha para conectarse al grupo de disponibilidad secundario, irán a la réplica principal del grupo de disponibilidad secundario. Si no, tendrá que configurar cada réplica para permitir todas las conexiones como una réplica secundaria y tener acceso a ellas directamente. A pesar de ello, el enrutamiento de solo lectura funcionará si el grupo de disponibilidad secundario se convierte en principal tras una conmutación por error. Este comportamiento podría cambiar en una actualización de SQL Server 2016 o en una versión futura de SQL Server.


## <a name="initialize-secondary-availability-groups-in-a-distributed-availability-group"></a>Inicializar grupos de disponibilidad secundarios en un grupo de disponibilidad distribuido

Los grupos de disponibilidad distribuidos están diseñados con la [propagación automática](./automatically-initialize-always-on-availability-group.md) como método principal para inicializar la réplica principal en el segundo grupo de disponibilidad. Si hace lo siguiente, realizará una restauración de base de datos completa en la réplica principal del segundo grupo de disponibilidad:

1. Restaure las copias de seguridad de las bases de datos con WITH NORECOVERY.
2. Si procede, restaure las copias de seguridad de registro de transacciones apropiadas con WITH NORECOVERY.
3. Cree el segundo grupo de disponibilidad sin especificar un nombre de base de datos y con SEEDING_MODE establecido en AUTOMATIC.
4. Cree el grupo de disponibilidad distribuido por medio de la propagación automática.

Cuando la réplica principal del segundo grupo de disponibilidad se agrega al grupo de disponibilidad distribuido, dicha réplica se compara con las bases de datos principales del primer grupo de disponibilidad y la propagación detecta la base de datos hasta el origen. Existen varios inconvenientes:

* El resultado que consta en `sys.dm_hadr_automatic_seeding` en la réplica principal del segundo grupo de disponibilidad mostrará `current_state` como FAILED con el motivo "Seeding Check Message Timeout" (se agotó el tiempo de espera del mensaje de comprobación de propagación).

* El registro de SQL Server actual de la réplica principal del segundo grupo de disponibilidad reflejará que la propagación ha funcionado y que los [LSN](../../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md) se sincronizaron.

* El resultado que consta en `sys.dm_hadr_automatic_seeding` en la réplica principal del primer grupo de disponibilidad mostrará current_state como COMPLETED. 

* La propagación tiene un comportamiento distinto también con los grupos de disponibilidad distribuidos. Para que la propagación comience en la segunda réplica, hay que emitir el comando `ALTER AVAILABILITY GROUP [AGName] GRANT CREATE ANY DATABASE` en la réplica. Aunque esto sigue siendo así en cualquier réplica secundaria que participa en el grupo de disponibilidad subyacente, la réplica principal del segundo grupo de disponibilidad ya tiene los permisos adecuados para permitir que la propagación se inicie después de que se haya agregado al grupo de disponibilidad distribuido. 

## <a name="monitor-distributed-availability-group-health"></a>Supervisar el estado de grupos de disponibilidad distribuidos

Un grupo de disponibilidad distribuido es una construcción solo de SQL Server y no se ve en el clúster WSFC subyacente. En la siguiente imagen aparecen dos clústeres WSFC distintos (CLUSTER_A y CLUSTER_B), cada uno con sus propios grupos de disponibilidad. Aquí solo nos centraremos en AG1 en CLUSTER_A y en AG2 en CLUSTER_B. 

[Dos clústeres WSFC con varios grupos de disponibilidad a través del comando de PowerShell Get-ClusterGroup](./media/distributed-availability-group/dag-07-two-wsfcs-multiple-ags-through-get-clustergroup-command.png)


```
PS C:\> Get-ClusterGroup -Cluster CLUSTER_A

Name                            OwnerNode             State
----                            ---------             -----
AG1                             DENNIS                Online
Available Storage               GLEN                  Offline
Cluster Group                   JY                    Online
New_RoR                         DENNIS                Online
Old_RoR                         DENNIS                Online
SeedingAG                       DENNIS                Online


PS C:\> Get-ClusterGroup -Cluster CLUSTER_B

Name                            OwnerNode             State
----                            ---------             -----
AG2                             TOMMY                 Online
Available Storage               JC                    Offline
Cluster Group                   JC                    Online
```

Toda la información detallada sobre un grupo de disponibilidad distribuido está en SQL Server, concretamente en las vistas de administración dinámica de grupos de disponibilidad. Actualmente, la única información que se muestra en SQL Server Management Studio sobre un grupo de disponibilidad distribuido está disponible en la réplica principal de los grupos de disponibilidad. Tal como se muestra en la figura siguiente, en la carpeta Grupos de disponibilidad, SQL Server Management Studio muestra que hay un grupo de disponibilidad distribuido. La figura muestra GD1 como una réplica principal de un grupo de disponibilidad específico que se encuentra en la instancia en lugar de un grupo de disponibilidad distribuido.

![Vista de la réplica principal del primer clúster WSFC de un grupo de disponibilidad distribuido en SQL Server Management Studio](./media/distributed-availability-group/dag-08-view-smss-primary-replica-first-wsfc-distributed-ag.png)


En cambio, si hace clic con el botón derecho en el grupo de disponibilidad distribuido, no se mostrará ninguna opción (consulte la figura siguiente), mientras que las carpetas expandidas Bases de datos de disponibilidad, Agentes de escucha de grupos de disponibilidad y Réplicas de disponibilidad estarán vacías. SQL Server Management Studio 16 muestra este resultado, pero puede cambiar en una versión futura de la herramienta.

![No hay opciones disponibles para la acción](./media/distributed-availability-group/dag-09-no-options-available-action.png)

Tal como se muestra en la figura siguiente, las réplicas secundarias no muestran ningún elemento relacionado con el grupo de disponibilidad distribuido en SQL Server Management Studio. Estos nombres de grupo de disponibilidad se ajustan a los roles que se muestran en la imagen anterior, Clúster WSFC CLUSTER_A.

![Vista de una réplica secundaria en SQL Server Management Studio](./media/distributed-availability-group/dag-10-view-ssms-secondary-replica.png)

### <a name="dmv-to-list-all-availability-replica-names"></a>DMV para enumerar todos los nombres de réplica de disponibilidad

Al usar las vistas de administración dinámica, se aplican los mismos conceptos. Mediante el uso de la consulta siguiente, puede ver todos los grupos de disponibilidad (normales y distribuidos), así como los nodos implicados. Este resultado se muestra solo si realiza una consulta a la réplica principal en uno de los clústeres WSFC que forman parte del grupo de disponibilidad distribuido. Hay una nueva columna en la vista de administración dinámica `sys.availability_groups` denominada `is_distributed`, que es 1 cuando el grupo de disponibilidad es del tipo distribuido. Para ver esta columna:

```sql
-- shows replicas associated with availability groups
SELECT 
   ag.[name] AS [AG Name], 
   ag.Is_Distributed, 
   ar.replica_server_name AS [Replica Name]
FROM sys.availability_groups AS ag 
INNER JOIN sys.availability_replicas AS ar       
   ON ag.group_id = ar.group_id
GO
```

En la figura siguiente se muestra un ejemplo de salida desde el segundo clúster WSFC que participa en un grupo de disponibilidad distribuido. SPAG1 se compone de dos réplicas: DENNIS y JY. En cambio, el grupo de disponibilidad distribuido denominado SPDistAG contiene los nombres de los dos grupos de disponibilidad implicados (SPAG1 y SPAG2) en lugar de los nombres de las instancias, al igual que sucede con un grupo de disponibilidad tradicional. 

![Ejemplo de salida de la consulta anterior](./media/distributed-availability-group/dag-11-example-output-of-query-above.png)

### <a name="dmv-to-list-distributed-ag-health"></a>DMV para enumerar el estado del grupo de disponibilidad distribuido

En SQL Server Management Studio, cualquier tipo de estado que se muestra en el panel y en otras áreas está diseñado para la sincronización local solo dentro del grupo de disponibilidad. Para mostrar el estado de un grupo de disponibilidad distribuido, consulte las vistas de administración dinámica. La consulta de ejemplo siguiente amplía y mejora la consulta anterior:

```sql
-- shows sync status of distributed AG
SELECT 
   ag.[name] AS [AG Name], 
   ag.is_distributed, 
   ar.replica_server_name AS [Underlying AG], 
   ars.role_desc AS [Role], 
   ars.synchronization_health_desc AS [Sync Status]
FROM  sys.availability_groups AS ag
INNER JOIN sys.availability_replicas AS ar 
   ON  ag.group_id = ar.group_id        
INNER JOIN sys.dm_hadr_availability_replica_states AS ars       
   ON  ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```
       
       
![Estado del grupo de disponibilidad distribuido](./media/distributed-availability-group/dag-12-distributed-ag-status.png)

### <a name="dmv-to-view-underlying-performance"></a>DMV para ver el rendimiento subyacente

Para ampliar aún más la consulta anterior, también puede ver el rendimiento subyacente mediante las vistas de administración dinámica agregando `sys.dm_hadr_database_replicas_states`. La vista de administración dinámica actualmente solo almacena información sobre el segundo grupo de disponibilidad. La consulta de ejemplo siguiente, al ejecutarse en el grupo de disponibilidad principal, genera el resultado de ejemplo que se muestra a continuación:

```sql
-- shows underlying performance of distributed AG
SELECT 
   ag.[name] AS [Distributed AG Name], 
   ar.replica_server_name AS [Underlying AG], 
   dbs.[name] AS [Database],
   ars.role_desc AS [Role],
   drs.synchronization_health_desc AS [Sync Status],
   drs.log_send_queue_size,
   drs.log_send_rate, 
   drs.redo_queue_size, 
   drs.redo_rate
FROM sys.databases AS dbs
INNER JOIN sys.dm_hadr_database_replica_states AS drs
   ON dbs.database_id = drs.database_id
INNER JOIN sys.availability_groups AS ag
   ON drs.group_id = ag.group_id
INNER JOIN sys.dm_hadr_availability_replica_states AS ars
   ON ars.replica_id = drs.replica_id
INNER JOIN sys.availability_replicas AS ar
   ON ar.replica_id = ars.replica_id
WHERE ag.is_distributed = 1
GO
```


![Información sobre el rendimiento de un grupo de disponibilidad distribuido](./media/distributed-availability-group/dag-13-performance-information-distributed-ag.png)

### <a name="dmv-to-view-performance-counters-for-distributed-ag"></a>DMV para ver los contadores de rendimiento para el grupo de disponibilidad distribuido
En la siguiente consulta se muestran contadores de rendimiento asociados específicamente con el grupo de disponibilidad distribuido. 


 ```sql
 -- displays OS performance counters related to the distributed ag named 'distributedag'
 SELECT * FROM sys.dm_os_performance_counters WHERE instance_name LIKE '%distributed%'
 ```

 ![DMV que muestra los contadores de rendimiento del sistema operativo para el grupo de disponibilidad distribuido](./media/distributed-availability-group/dmv-os-performance-counters.png)


 >[!NOTE]
 >El filtro `LIKE` debería tener el nombre del grupo de disponibilidad distribuido. En este ejemplo, el nombre del grupo de disponibilidad distribuido es "distributedag". Cambie el modificador `LIKE` para que refleje el nombre del grupo de disponibilidad distribuido.  

### <a name="dmv-to-display-health-of-both-ag-and-distributed-ag"></a>DMV para mostrar el estado tanto del grupo de disponibilidad como del grupo de disponibilidad distribuido
En la siguiente consulta se muestra una gran cantidad de información sobre el estado tanto del grupo de disponibilidad como del grupo de disponibilidad distribuido. [¡Gracias, Tracy Boggiano!](https://tracyboggiano.com/archive/2017/11/distributed-availability-groups-setup-and-monitoring/)

 ```sql
 -- displays sync status, send rate, and redo rate of availability groups, including distributed AG
 SELECT 
        ag.name AS 'AG Name', 
        ag.is_distributed, 
        ar.replica_server_name AS 'AG', 
        dbs.name AS 'Database',
    ars.role_desc, 
    drs.synchronization_health_desc, 
    drs.log_send_queue_size, 
    drs.log_send_rate, 
    drs.redo_queue_size, 
    drs.redo_rate,
    drs.suspend_reason_desc,
    drs.last_sent_time,
    drs.last_received_time,
    drs.last_hardened_time,
    drs.last_redone_time,
    drs.last_commit_time,
    drs.secondary_lag_seconds
 FROM sys.databases dbs 
 INNER JOIN sys.dm_hadr_database_replica_states drs 
    ON dbs.database_id = drs.database_id
 INNER JOIN sys.availability_groups ag 
    ON drs.group_id = ag.group_id
 INNER JOIN sys.dm_hadr_availability_replica_states ars 
    ON ars.replica_id = drs.replica_id
 INNER JOIN sys.availability_replicas ar 
    ON ar.replica_id =  ars.replica_id
 --WHERE ag.is_distributed = 1
 GO
 ```

![Estado del grupo de disponibilidad y el grupo de disponibilidad distribuido](./media/distributed-availability-group/dmv-sync-status-send-rate.png)

### <a name="dmvs-to-view-metadata-of-distributed-ag"></a>DMV para ver los metadatos del grupo de disponibilidad distribuido
En las consultas siguientes se mostrará información sobre las direcciones URL de punto de conexión utilizadas por los grupos de disponibilidad, incluido el grupo de disponibilidad distribuido.  [¡Gracias, David Barbarin!](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)



 ```sql
 -- shows endpoint url and sync state for ag, and dag
 SELECT
    ag.name AS group_name,
    ag.is_distributed,
    ar.replica_server_name AS replica_name,
    ar.endpoint_url,
    ar.availability_mode_desc,
    ar.failover_mode_desc,
    ar.primary_role_allow_connections_desc AS allow_connections_primary,
    ar.secondary_role_allow_connections_desc AS allow_connections_secondary,
    ar.seeding_mode_desc AS seeding_mode
 FROM sys.availability_replicas AS ar
 JOIN sys.availability_groups AS ag
    ON ar.group_id = ag.group_id
 GO
 ```


![DMV de metadatos para el grupo de disponibilidad distribuido](./media/distributed-availability-group/dmv-metadata-dag2.png)



### <a name="dmv-to-show-current-state-of-seeding"></a>DMV para mostrar el estado actual de la propagación
En la siguiente consulta se muestra información sobre el estado actual de la propagación. Esto es útil para solucionar errores de sincronización entre réplicas. [¡Gracias de nuevo, David Barbarin!](https://blog.dbi-services.com/sql-server-2016-alwayson-distributed-availability-groups/)

 ```sql
 -- shows current_state of seeding 
 SELECT
    ag.name AS aag_name,
    ar.replica_server_name,
    d.name AS database_name,
    has.current_state,
    has.failure_state_desc AS failure_state,
    has.error_code,
    has.performed_seeding,
    has.start_time,
    has.completion_time,
    has.number_of_attempts
 FROM sys.dm_hadr_automatic_seeding AS has
 JOIN sys.availability_groups AS ag
    ON ag.group_id = has.ag_id
 JOIN sys.availability_replicas AS ar
    ON ar.replica_id = has.ag_remote_replica_id
 JOIN sys.databases AS d
    ON d.group_database_id = has.ag_db_id
 GO
 ```


![Estado actual de la propagación](./media/distributed-availability-group/dmv-seeding.png)




### <a name="next-steps"></a>Pasos siguientes 

* [Usar el Asistente para grupo de disponibilidad (SQL Server Management Studio)](use-the-availability-group-wizard-sql-server-management-studio.md)

* [Usar el cuadro de diálogo Nuevo grupo de disponibilidad (SQL Server Management Studio)](use-the-new-availability-group-dialog-box-sql-server-management-studio.md)
 
* [Crear un grupo de disponibilidad (Transact-SQL)](create-an-availability-group-transact-sql.md)