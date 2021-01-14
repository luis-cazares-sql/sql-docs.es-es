---
title: 'Actualice instancias de SQL Server: Windows Server 2012 y clústeres anteriores'
description: Describe cómo actualizar las instancias de clúster de conmutación por error de SQL Server que se ejecutan en Windows Server 2008, Windows Server 2008 R2 y Windows Server 2012.
ms.date: 06/15/2020
ms.custom: seo-lt-2019
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- upgrading failover clusters
- clusters [SQL Server], upgrading
- failover clustering [SQL Server], upgrading
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4d9e3116902ae96eaa97e9624ea33ba3fa8952d3
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171587"
---
# <a name="upgrade-sql-server-instances-running-on-windows-server-20082008-r22012-clusters"></a>Actualizar instancias de SQL Server que se ejecutan en clústeres de Windows Server 2008/2008 R2/2012

[!INCLUDE[nextref-longhorn-md](../../../includes/nextref-longhorn-md.md)], [!INCLUDE[winserver2008r2-md](../../../includes/winserver2008r2-md.md)] y [!INCLUDE[win8srv-md](../../../includes/win8srv-md.md)] impiden que los clústeres de conmutación por error de Windows Server realicen actualizaciones del sistema de operativo locales, por lo que limitan la versión permitida de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para un clúster. Una vez que el clúster se actualice al menos a [!INCLUDE[winblue-server-2-md](../../../includes/winblue-server-2-md.md)], podrá permanecer actualizado.

## <a name="prerequisites"></a>Prerrequisitos

-   Antes de realizar cualquiera de las estrategias de migración, se debe preparar un clúster de conmutación por error de Windows Server paralelo con Windows Server 2016/2012 R2. Todos los nodos que contengan instancias de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] deben estar unidas al clúster de Windows con los FCI instalados. Cualquier equipo independiente **no debe** combinarse con el clúster de conmutación por error de Windows Server antes de la migración. Las bases de datos de usuario deberán estar sincronizadas con el nuevo entorno antes de la migración.
-   Todas las instancias de destino deberán estar ejecutando la misma versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] como su instancia paralela en el entorno original, con los mismos nombres de instancia e identificadores. Además, deberán estar instaladas con las mismas características. Las rutas de acceso de instalación y la estructura del directorio deberán ser idénticas en los equipos de destino. Esto no incluye los nombres de red virtual de FCI, que deben ser diferentes antes de la migración. Las funciones habilitadas por la instancia original (como Always On y FILESTREAM) deberán estar habilitadas en la instancia de destino.

-   El clúster paralelo no deberá tener [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)] instalado antes de la migración.

-   El tiempo de actividad al realizar la migración de un clúster que usa Grupos de disponibilidad de forma estricta (con o sin FCI de SQL) se puede limitar ampliamente usando grupos de disponibilidad distribuidos, pero no requiere que todas las instancias ejecuten [!INCLUDE[sssql15-md](../../../includes/sssql16-md.md)] RTM o versiones posteriores.

-   Todas las estrategias de migración requieren el rol sysadmin de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Todos los usuarios de Windows usados por los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (es decir, la cuenta Windows que ejecuta los agentes de replicación) deberán tener los permisos de nivel de sistema operativo en cada equipo del nuevo entorno.

-   Todos los recursos compartidos de archivos y las unidades asignadas de la red que se usen en el entorno del clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] original deberán existir y permanecer disponibles para el clúster de destino con los mismos permisos que las instancias originales.

-   Ninguno de los puertos TCP/IP que escucha la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] original deberán estar sin uso y permitir el tráfico de entrada en el equipo de destino.
-   Todos los servicios relacionados con SQL deben instalarse y ejecutarse con el mismo usuario de Windows.

-   Las instancias de destino deben instalarse con la misma configuración regional que las instancias originales.

## <a name="migration-scenarios"></a>Escenarios de migración

La estrategia de migración adecuada dependerá de ciertos parámetros de la topología del clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] original, es decir, el uso de instancias de clúster de conmutación por error de [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)] y SQL. La estrategia elegida dependerá también de los requisitos del entorno de destino. Si el nuevo entorno requiere que cada equipo o FCI de SQL mantenga el nombre original de la red virtual (VNN), o si la topología de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] depende de las nuevas instancias que heredarán todos los objetos de sistema de las instancias originales, deberá elegir una estrategia que incluya su migración.

|                                   | Requiere todos los objetos de servidor y VNNS | Requiere todos los objetos de servidor y VNNS | No requiere ningún objeto de servidor ni VNN\* | No requiere ningún objeto de servidor ni VNN\* |
|-----------------------------------|--------------------------------------|--------------------------------------------------------------------|------------|------------|
| **_¿Grupos de disponibilidad? (S/N)_**                  | **_S_**                              | **_N_**                                                            | **_S_**    | **_N_**    |
| **El clúster solo usa FCI de SQL**         | [Escenario 3](#scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups)                           | [Escenario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis)                                                        | [Escenario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Escenario 2](#scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis) |
| **El clúster usa instancias independientes** | [Escenario 5](#scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups)                           | [Escenario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups)                                                         | [Escenario 1](#scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis) | [Escenario 4](#scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups) |

\* Excluir nombres de agentes de escucha del grupo de disponibilidad

## <a name="scenario-1-windows-cluster-with-sql-server-availability-groups-and-no-failover-cluster-instances-fcis"></a>Escenario 1: Windows Cluster con grupos de disponibilidad de SQL Server y sin instancias de clúster de conmutación por error (FCI)
Si tiene una configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que usa grupos de disponibilidad sin instancias de clúster de conmutación por error, puede llevar a cabo la migración a un nuevo clúster creando una implementación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] paralela en otra instancia de Windows Cluster con Windows Server 2016/2012 R2. Después de esto, puede crear un grupo de disponibilidad distribuido en el que el clúster de destino sea la base de datos secundaria para el clúster de producción actual. Esto requiere que el usuario actualice a [!INCLUDE[sssql15-md](../../../includes/sssql16-md.md)] o versiones posteriores.

###  <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1.  Si es necesario, actualice todas las instancias a [!INCLUDE[sssql15-md](../../../includes/sssql16-md.md)] o versiones posteriores. Las instancias paralelas deben estar ejecutando la misma versión de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Cree un grupo de disponibilidad para el clúster de destino. Si el nodo principal del clúster de destino no es una FCI, cree un agente de escucha.

3.  Cree un grupo de disponibilidad distribuido en el que el clúster de destino sea el grupo de disponibilidad secundario.

    >[!NOTE]
    >El parámetro de URL LISTENER\_ de la consulta T-SQL para el grupo de disponibilidad distribuido tiene un comportamiento distinto al de los grupos de disponibilidad que tienen una FCI de SQL como instancia principal. Si este es el caso del servidor principal o el grupo de disponibilidad secundario, use el VNN de la FCI de SQL principal como dirección URL del agente de escucha en lugar de su nombre de red, junto con el puerto de punto de conexión de creación de reflejo de la base de datos.

4.  Combine el grupo de disponibilidad secundario con el grupo de disponibilidad distribuido.

5.  Combine las bases de datos secundarias con el grupo de disponibilidad secundario.

    >[!NOTE]
    >Esto se realizará automáticamente si el grupo de disponibilidad de destino usa la propagación automática. Esto será posible si las rutas de acceso de datos y de registro son iguales en todas las réplicas.

6.  Corte todo el tráfico hacia el grupo de disponibilidad principal y permita la sincronización del segundo grupo.

7.  Cambie la directiva de confirmación en ambos grupos de disponibilidad para que se muestre el valor SYNCHRONOUS_COMMIT y realice la conmutación por error del clúster de destino cuando el estado sea SYNCHRONIZED.

8.  Elimine el grupo de disponibilidad distribuido.

9.  Elimine o cambie el nombre de agente de escucha en el grupo de disponibilidad original.

10. Cambie el nombre o cree el agente de escucha de los nuevos grupos de disponibilidad con el nombre del agente de escucha del grupo de disponibilidad original.

    >[!NOTE]
    >Mientras exista el registro DNS del agente de escucha del grupo de disponibilidad original, se producirá un error al intentar crear un agente de escucha con este nombre.

11. Reanude el tráfico hacia el agente de escucha.

## <a name="scenario-2-windows-clusters-with-sql-server-failover-cluster-instances-fcis"></a>Escenario 2: Windows Cluster con instancias de clúster de conmutación por error (FCI) de SQL Server

Si tiene un entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que solo usa instancias de FCI de SQL, puede realizar la migración a un nuevo clúster creando un entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] paralelo en otra instancia de Windows Cluster con Windows Server 2016/2012 R2. Realizará la migración del clúster de destino "adquiriendo" los VNN de las FCI de SQL antiguas y incorporándolos a los nuevos clústeres. Esto creará tiempo de inactividad adicional en función de los tiempos de propagación de DNS.

###  <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1.  Realice una copia de seguridad completa y detenga el tráfico hacia el clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] original.

2.  Realice una copia de seguridad del final del registro que incluya las bases de datos de usuario y restaure mediante recuperación en el nuevo entorno.

3.  En el clúster de destino del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

4.  Sin salir del clúster de destino del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

5.  En el clúster original del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

6.  Sin salir del clúster original del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

7.  Copie las bases de datos del sistema de los equipos originales en su equipo de destino correspondiente.

8.  En el entorno original del Administrador de clústeres de conmutación por error, cambie el nombre del recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

9.  Ahora vuelva a conectar el recurso de nombre de servidor con el nuevo nombre para cada uno de los roles de FCI de SQL.

10. Ahora, en el clúster de destino del Administrador de clústeres de conmutación por error, cambie el nombre el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al nombre que tenía el clúster original.

    >[!NOTE]
    >Los errores derivados de nombres que ya se hayan usado en otros equipos desparecerán cuando se eliminen los registros DNS del nombre en cuestión.

11. Una vez que se haya cambiado el nombre de todas las FCI, reinicie cada uno de los equipos en el nuevo clúster.

12. Cuando los equipos se reinicien y vuelvan a conectarse, inicie cada uno de los roles de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el Administrador de clústeres de conmutación por error.

## <a name="scenario-3-windows-cluster-has-both-sql-fcis-and-sql-server-availability-groups"></a>Escenario 3: Windows Cluster tiene tanto FCI de SQL como grupos de disponibilidad de SQL Server

Si tiene una configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que no usa ninguna instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] independiente, sino que solo usa FCI de SQL contenidas en al menos un grupo de disponibilidad, podrá realizar la migración de esta configuración a un nuevo clúster usando métodos parecidos al escenario en el que no hay ningún grupo de disponibilidad ni ninguna instancia independiente. Antes de copiar las tablas del sistema en los discos compartidos de FCI de destino, deberá quitar todos los grupos de disponibilidad del entorno original. Después de que se haya realizado la migración de todas las bases de datos a los equipos de destino, volverá a crear los grupos de disponibilidad con los mismos nombres de esquema y agente de escucha. Al hacerlo, los recursos de clúster de conmutación por error de Windows Server se formarán y administrarán correctamente en el clúster de destino. **Always On debe estar habilitado en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager en cada equipo del entorno de destino antes de la migración.**

### <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1.  Detenga el tráfico hacia [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Realice una copia de seguridad del final del registro que incluya bases de datos de usuario mediante la recuperación en el nuevo elemento principal del entorno y con el valor NORECOVERY en todos los elementos secundarios.

3.  En el clúster de destino del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

4.  Sin salir del clúster de destino del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

5.  En el clúster original, elimine el grupo de disponibilidad.

6.  En el clúster original del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

7.  Sin salir del clúster original del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

8.  Copie las bases de datos del sistema de los equipos originales en su equipo de destino correspondiente.

9.  En el entorno original del Administrador de clústeres de conmutación por error, cambie el nombre del recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

10. Ahora vuelva a conectar el recurso de nombre de servidor con el nuevo nombre para cada uno de los roles de FCI de SQL.

11. Ahora, en el clúster de destino del Administrador de clústeres de conmutación por error, cambie el nombre el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al nombre que tenía el clúster original.

12. Una vez que se haya cambiado el nombre de todas las FCI, reinicie cada uno de los equipos en el nuevo clúster.

13. Cuando los equipos se reinicien y vuelvan a conectarse, inicie cada uno de los roles de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el Administrador de clústeres de conmutación por error.

14. Una vez que todas las instancias estén conectadas, forme el grupo de disponibilidad en la réplica en la que se restauraron las bases de datos mediante recuperación.

15. Combine todas las réplicas y bases de datos secundarias con el grupo de disponibilidad.

16. Cree un agente de escucha en el nuevo grupo de disponibilidad con el nombre del agente de escucha del grupo de disponibilidad original.

## <a name="scenario-4-windows-cluster-with-standalone-sql-server-instances-and-no-availability-groups"></a>Escenario 4: Windows Cluster con instancias independientes de SQL Server y sin grupos de disponibilidad

Realizar la migración de un clúster con instancias independientes es un proceso similar a realizar la migración de un clúster de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que solo tiene FCI; en lugar de cambiar el VNN del recurso de clúster de nombre de red de las FCI, deberá cambiar el nombre de equipo del equipo independiente original y "adquirir" el nombre del equipo antiguo en el equipo de destino. Esto presenta un tiempo de inactividad adicional con respecto a los escenarios no independientes, ya que no podrá combinar el equipo independiente de destino con el WSFC hasta que haya adquirido el nombre de red del equipo antiguo.

###  <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1.  Detenga el tráfico hacia [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Realice una copia de seguridad del final del registro que incluya las bases de datos de usuario y restaure mediante recuperación en el nuevo entorno de cada equipo.

3.  En el clúster de destino del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

4.  Sin salir del clúster de destino del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

5.  En el clúster original del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL y detenga las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] independientes en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.

6.  Para cada equipo independiente del clúster original, cambie el nombre de cada equipo a un nuevo nombre de equipo único. Reinicie cada uno de los equipos tal como se indica.

7.  Sin salir del clúster original del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

8.  Copie las bases de datos del sistema en los equipos de destino.

9.  En el entorno original del Administrador de clústeres de conmutación por error, cambie el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a un nuevo nombre único.

10. Ahora vuelva a conectar el recurso de nombre de servidor con el nuevo nombre para cada uno de los roles de FCI de SQL.

11. Ahora, en las instancias independientes paralelas, cambie el nombre de los equipos por el nombre de los equipos independientes originales. Quite el antiguo nombre de servidor y agregue el nuevo con el parámetro local. Reinicie los equipos tal como se indica.

12. Después del reinicio, combine cada uno de los equipos independientes con el clúster de conmutación por error de Windows Server de destino.

13. Ahora, en el clúster de destino del Administrador de clústeres de conmutación por error, cambie el nombre el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al nombre que tenía el clúster original.

14. Una vez que se haya cambiado el nombre de todas las FCI, reinicie cada uno de los equipos en el nuevo clúster.

15. Cuando los equipos se reinicien y vuelvan a conectarse, inicie cada uno de los roles de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el Administrador de clústeres de conmutación por error.

## <a name="scenario-5-windows-cluster-with-standalone-sql-server-instances-and-availability-groups"></a>Escenario 5: Windows Cluster con instancias independientes de SQL Server y grupos de disponibilidad

Realizar la migración de un clúster que usa grupos de disponibilidad con réplicas independientes es un proceso similar a la migración de un clúster con FCI que usan grupos de disponibilidad. También deberá eliminar los grupos de disponibilidad originales y reconstruirlos en el clúster de destino. Pero el tiempo de inactividad adicional se produce debido a la dificultad añadida de la migración de instancias independientes. **Always On debe estar habilitado en cada FCI del entorno de destino antes de la migración.**

###  <a name="to-perform-the-upgrade"></a>Para realizar la actualización

1.  Detenga el tráfico hacia [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].

2.  Realice una copia de seguridad del final del registro que incluya bases de datos de usuario mediante la recuperación en el nuevo elemento principal del entorno y con el valor NORECOVERY en cada elemento secundario.

3.  Elimine el grupo de disponibilidad del clúster original.

4.  En el clúster de destino del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL.

5.  Sin salir del clúster de destino del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

6.  En el clúster original del Administrador de clústeres de conmutación por error, reduzca la prioridad de los roles en clúster de cada FCI de SQL y detenga las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] independientes en [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Configuration Manager.

7.  Para cada equipo independiente del clúster original, cambie el nombre de cada equipo a un nuevo nombre de equipo único. Reinicie cada uno de los equipos tal como se indica.

8.  Sin salir del clúster original del Administrador de clústeres de conmutación por error, aumente la prioridad de los discos en clúster para cada FCI de SQL.

9.  Copie las bases de datos del sistema en los equipos de destino.

10. En el entorno original del Administrador de clústeres de conmutación por error, cambie el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a un nuevo nombre único.

11. Ahora vuelva a conectar el recurso de nombre de servidor con el nuevo nombre para cada uno de los roles de FCI de SQL.

12. Ahora, en las instancias independientes paralelas, cambie el nombre de los equipos por el nombre de los equipos independientes originales. Quite y agregue el nombre del servidor en SQL. Reinicie los equipos tal como se indica.

13. Después del reinicio, combine cada uno de los equipos independientes con el clúster de conmutación por error de Windows Server de destino.

14. Ahora, en el clúster de destino del Administrador de clústeres de conmutación por error, cambie el nombre el recurso "Nombre de servidor" de cada rol de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] al nombre que tenía el clúster original.

15. Una vez que se haya cambiado el nombre de todas las FCI, reinicie cada uno de los equipos en el nuevo clúster.

16. Cuando los equipos se reinicien y vuelvan a conectarse, inicie cada uno de los roles de FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en el Administrador de clústeres de conmutación por error.

17. Una vez que todas las instancias estén conectadas, vuelva a crear el grupo de disponibilidad en el elemento principal adecuado.

18. Combine cada réplica secundaria con una base de datos secundaria.

19. Vuelva a crear el agente de escucha de grupo de disponibilidad con el mismo nombre que el original.

## <a name="specific-concerns-for-individual-features"></a>Consideraciones específicas de cada característica

### [!INCLUDE[sshadrc-md](../../../includes/sshadrc-md.md)]

-   **Punto de conexión de creación de reflejo de la base de datos**

    Desde el punto de vista de SQL, se realzará la migración del punto de conexión de creación de reflejo de la base de datos a la nueva instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] junto con las tablas del sistema. Antes de la migración, asegúrese de que se apliquen las reglas correspondientes en los firewall y que ningún otro proceso esté escuchando en el mismo puerto.

-   **Grupos de disponibilidad**

    No se puede realizar la migración de los grupos de disponibilidad y sus agentes de escucha de una instancia a otra. Los recursos de clúster de conmutación por error de Windows Server creados por el grupo de disponibilidad no se pueden reconstruir fácilmente en el entorno de destino. En lugar de intentar realizar la migración de grupos de disponibilidad, el objetivo es quitar y volver a crear los grupos de disponibilidad en el clúster de destino.

-   **Agentes de escucha del grupo de disponibilidad**

    Al igual que los grupos de disponibilidad, deberá quitar y volver a crear los agentes de escucha en lugar de realizar su migración directamente.

### <a name="replication"></a>Replicación

-   **Distribuidores remotos, publicadores, suscriptores**

    La relación entre un distribuidor y un publicador se basa solo en el VNN de los equipos que los hospeden, que se resolverán adecuadamente en el nuevo equipo. También se realizará la migración de los trabajos del agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] junto con las tablas del sistema, por lo que varios agentes de replicación podrán continuar ejecutándose normalmente. Es necesario que, antes de la migración, las cuentas de Windows que ejecuten el agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o cualquier trabajo del agente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] tengan los mismos permisos en el entorno de destino. La comunicación con el publicador y los suscriptores se ejecutará con normalidad.

-   **Carpeta de instantáneas**

    Antes de la migración, es necesario que todos los recursos compartidos de red usados por cualquier característica de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] estén disponibles para los equipos del entorno de destino con los mismos permisos que en el entorno original. Tendrá que asegurarse de que sea cierto antes de la migración.

### <a name="service-broker"></a>Service Broker

-   **Punto de conexión de Service Broker**

    Desde el punto de vista de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], no hay ninguna consideración relacionada con el punto de conexión. Antes de la migración, tendrá que asegurarse de que no haya ningún proceso que ya esté escuchando en el mismo puerto, así como de que no haya ninguna regla de firewall que esté bloqueando o permitiendo específicamente el puerto.

-   **Certificados**

    También se debería realizar una copia de seguridad de los certificados y restaurarlos en los equipos de destino en caso de que sea necesario.

-   **Rutas**

    Las rutas dependen del nombre de red virtual del destino, que se resolverá correctamente para los nombres de equipo y de red de FCI de SQL en los equipos adecuados del nuevo entorno. También se deberá redirigir al equipo nuevo cualquier otro VNN al que se haga referencia.

-   **Enlaces de servicio remoto**

    Los enlaces de servicio remoto funcionarán adecuadamente después de la migración, ya que se realizará correctamente la migración de cualquier usuario que use esta función.

### <a name="ssnoversion-agent"></a>e[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

-   **Trabajos**

    Se realizará correctamente la migración de los trabajos junto con las bases de datos del sistema. Cualquier usuario que ejecute un trabajo del agente de SQL o el propio agente tendrá los mismos permisos en el equipo de destino, tal como se especifica en los requisitos previos.

-   **Alertas y operadores**

    Se realizará correctamente la migración de las alertas y los operadores junto a las bases de datos del sistema.

### <a name="filestream"></a>FILESTREAM

-   **Puertos de uso compartido de archivos de Windows**

    Los puertos de uso compartido de archivos 139 y 445 de Windows deben tener reglas que permitan el tráfico de entrada para poder usar FILESTREAM.

-   **Recurso compartido de Windows**

    La ruta de acceso del recurso compartido de Windows depende del recurso de nombre de la FCI de SQL, ya que `\\ServerName\ShareName` obtiene acceso a este. La migración de FILESTREAM requiere que todos los nodos de la FCI de destino habiliten FILESTREAM. Además, si se usa un recurso compartido de Windows, estarán configurados para usar el mismo nombre tanto para el recurso compartido como para el equipo original. Una vez que la FCI de destino obtenga el nombre de servidor correcto, hospedará el recurso compartido de Windows usando la ruta de acceso elegida.

-   **Datos de FILESTREAM**

    Los datos de FILESTREAM están incluidos en la copia de seguridad.

### <a name="integration-services"></a>Integration Services

-   **Proyectos de SSIS**

    Los proyectos de SSIS se incluyen en migraciones junto con la base de datos de SSIS. Una vez que se mueva la base de datos de SSIS, los paquetes serán ejecutables inmediatamente antes de mover las tablas del sistema.

-   **Orígenes de datos basados en archivos**

    Los archivos planos, los archivos de Excel, los orígenes de XML y otros elementos deben estar disponibles en la misma ubicación especificada por el paquete de SSIS.

## <a name="next-steps"></a>Pasos siguientes
- [Completar la actualización motor de base de datos](../../../database-engine/install-windows/complete-the-database-engine-upgrade.md)
- [Cambiar el modo de compatibilidad de la base de datos y usar el almacén de consultas](../../../database-engine/install-windows/change-the-database-compatibility-mode-and-use-the-query-store.md)
- [Aprovechamiento de las nuevas características de SQL Server 2016](../../what-s-new-in-sql-server-2017.md)
- [Actualización de una instancia del clúster de conmutación por error de SQL Server](upgrade-a-sql-server-failover-cluster-instance.md).
- [Ver y leer los archivos de registro de instalación de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)
- [Agregar características a una instancia de SQL Server 2016 (programa de instalación)](../../../database-engine/install-windows/add-features-to-an-instance-of-sql-server-setup.md)