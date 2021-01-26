---
title: Usar el Asistente para planes de mantenimiento | Microsoft Docs
description: Obtenga información sobre cómo crear un plan de mantenimiento de un solo servidor o multiservidor mediante el Asistente para planes de mantenimiento de SQL Server.
ms.date: 06/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.ag.maintwiz.integrity.f1
- sql13.ag.maintwiz.order.f1
- sql13.ag.maintwiz.report.f1
- sql13.ag.maintwiz.updatestats.f1
- sql13.ag.maintwiz.indexdefrag.f1
- sql13.ag.maintwiz.progress.f1
- sql13.ag.maintwiz.maintcleanup.f1
- sql13.ag.maintwiz.backupfull.f1
- sql13.ag.maintwiz.task.f1
- sql13.ag.maintwiz.server.f1
- sql13.ag.maintwiz.shrinkdb.f1
- sql13.ag.maintwiz.execagentjob.f1
- sql13.ag.maintwiz.summary.f1
- sql13.ag.maintwiz.welcome.f1
- sql13.ag.maintwiz.planprop.f1
- sql13.ag.maintwiz.reindex.f1
- sql13.ag.maintwiz.histcleanup.f1
- sql13.ag.maintwiz.backuplog.f1
- sql13.ag.maintwiz.backupdiff.f1
helpviewer_keywords:
- Maintenance Plan Wizard
- Database Maintenance Plan Wizard
- Database Maintenance Plan Wizard, starting
ms.assetid: db65c726-9892-480c-873b-3af29afcee44
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 68c3ff265fbf1bf56fb2d7f0ba42aa6f76dd72b2
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766243"
---
# <a name="use-the-maintenance-plan-wizard"></a>Usar el Asistente para planes de mantenimiento
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear un plan de mantenimiento de un solo servidor o multiservidor mediante el Asistente para planes de mantenimiento de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. El Asistente para planes de mantenimiento crea un plan de mantenimiento que el Agente [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede ejecutar periódicamente. Esto permite realizar diversas tareas de administración de bases de datos, incluidas copias de seguridad, comprobaciones de integridad de la base de datos o actualizaciones de las estadísticas de la base de datos a intervalos especificados.  
    
 
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Para crear un plan de mantenimiento multiservidor, debe configurar un entorno multiservidor con un servidor maestro y uno o varios servidores de destino. Debe crear y mantener planes de mantenimiento multiservidor en el servidor maestro. Puede ver planes en servidores de destino.   

-   Los miembros de los roles **db_ssisadmin** y **dc_admin** quizá puedan elevar sus privilegios a **sysadmin**. Esta elevación de privilegio se puede producir porque estos roles pueden modificar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ; estos paquetes los puede ejecutar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizando el contexto de seguridad de **sysadmin** del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . 

Para protegerse contra esta elevación de privilegio al ejecutar planes de mantenimiento, conjuntos de recopilación de datos y otros paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , configure los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecutan paquetes para usar una cuenta de proxy con privilegios limitados o agregar solo los miembros de **sysadmin** a los roles **db_ssisadmin** y **dc_admin** .  

##  <a name="prerequisites"></a><a name="Prerequisite"></a> Requisitos previos 
Debe habilitar la [opción de configuración del servidor Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md).
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para crear o administrar planes de mantenimiento, debe ser miembro del rol fijo de servidor **sysadmin** . El Explorador de objetos solo muestra el nodo **Planes de mantenimiento** para los usuarios que son miembros del rol fijo de servidor **sysadmin** .  
  
##  <a name="use-the-maintenance-plan-wizard"></a><a name="SSMSProcedure"></a> Usar el Asistente para planes de mantenimiento  
  
**Iniciar el asistente** 

  
1.  Expanda el servidor donde desea crear un plan de administración.  
  
2.  Expanda la carpeta **Administración** .  
  
3.  Haga clic con el botón derecho en la carpeta **Planes de mantenimiento** y seleccione **Asistente para planes de mantenimiento**.  
  
4.  En la página **Asistente para planes de mantenimiento de SQL Server** , haga clic en **Siguiente**.  
  
5.  En la página **Seleccionar propiedades del plan** :  
  
    1.  En el cuadro **Nombre** , escriba el plan de mantenimiento que va a crear.  
  
    2.  En el cuadro **Descripción** , describa brevemente el plan de mantenimiento.  
  
    3.  En la lista **Ejecutar como** , especifique la credencial que el Agente Microsoft SQL Server usa al ejecutar el plan de mantenimiento.  
  
    4.  Seleccione **Programaciones independientes para cada tarea** o **Una sola programación para todo el plan o ninguna programación** para especificar la programación periódica del plan de mantenimiento.  
  
        > **NOTA:** Si selecciona **Programaciones independientes para cada tarea**, deberá seguir los pasos del apartado **e.** siguiente para cada tarea del plan de mantenimiento.  
  
    5.  Si seleccionó **Una sola programación para todo el plan o ninguna programación**, en **Programación**, haga clic en **Cambiar**.  
  
        1.  En el cuadro de diálogo **Nueva programación de trabajo**, en el cuadro **Nombre**, escriba el nombre de la programación de trabajo.  
  
        2.  En la lista **Tipo de programación** , seleccione el tipo de la programación:  
  
            -   **Iniciar automáticamente al iniciar el Agente SQL Server.**  
  
            -   **Iniciar al quedar inactivas las CPU**  
  
            -   **Periódica**. Esta es la selección predeterminada.  
  
            -   **Una vez**  
  
        3.  Active o desactive la casilla **Habilitado** para habilitar o deshabilitar la programación.  
  
        4.  Si selecciona **Periódica**:  
  
            1.  En **Frecuencia**, en la lista **Sucede** , especifique la frecuencia de repetición:  
  
                -   Si selecciona **Diario**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en días.  
  
                -   Si selecciona **Semanal**, en el cuadro **Se repite cada** , escriba la frecuencia con que se repite la programación de trabajo en semanas. Seleccione el día de la semana en que se ejecuta la programación de trabajo.  
  
                -   Si selecciona **Mensual**, seleccione **Día** o **El**.  
  
                    -   Si selecciona **Día**, especifique la fecha del mes que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el decimoquinto día de cada mes, seleccione **Día** y escriba "15" en el primer cuadro y "2" en el segundo. Tenga en cuenta que el mayor número permitido en el segundo cuadro es "99".  
  
                    -   Si selecciona **El**, seleccione el día concreto de la semana del mes en que desea que se ejecute la programación de trabajo y con qué frecuencia debe repetirse la programación de trabajo en meses. Por ejemplo, si quiere que la programación de trabajo se ejecute el último día de la semana de cada mes, seleccione **Día**, seleccione **último** en la primera lista y **día de la semana** en la segunda y, después, escriba "2" en el último cuadro. En las primeras dos listas, también puede seleccionar **primero**, **segundo**, **tercero** o **cuarto**, así como días de la semana concretos (por ejemplo: domingo o miércoles). Tenga en cuenta que el mayor número permitido en el último cuadro es "99".  
  
            2.  Debajo de **Frecuencia diaria**, especifique la frecuencia con que se repite la programación de trabajo en el día en que se ejecuta:  
  
                -   Si selecciona **Sucede una vez a las**, escriba la hora concreta en que debe ejecutarse la programación de trabajo en el cuadro **Sucede una vez a las** . Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
                -   Si selecciona **Sucede cada**, especifique la frecuencia con que se ejecuta la programación de trabajo durante el día seleccionado en **Frecuencia**. Por ejemplo, si quiere que la programación de trabajo se repita cada dos horas al día cuando se ejecuta, seleccione **Sucede cada**, escriba "2" en el primer cuadro y, después, seleccione **horas** en la lista. En esta lista también puede seleccionar **minutos** y **segundos**. Tenga en cuenta que el mayor número permitido en el primer cuadro es "100".  
  
                     En el cuadro **A partir de** , especifique la hora en que la programación de trabajo debe iniciar su ejecución. En el cuadro **Finaliza** , especifique la hora en que la programación de trabajo debe dejar de repetirse. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
            3.  Debajo de **Duración**, en **Fecha de inicio**, escriba la fecha en que desea que la programación de trabajo inicie su ejecución. Seleccione **Fecha de finalización** o **Sin fecha de finalización** para indicar cuándo se debe detener la ejecución de la programación de trabajo. Si selecciona **Fecha de finalización**, escriba la fecha en que desea que deje de ejecutarse la programación de trabajo.  
  
        5.  Si selecciona **Una vez**, debajo de **Única repetición**, en el cuadro **Fecha** , escriba la fecha en que se ejecutará la programación de trabajo. En el cuadro **Hora** , especifique la hora a la que se ejecutará la programación de trabajo. Especifique la hora, minuto y segundo del día, así como a.m. o p.m.  
  
        6.  Debajo de **Resumen**, en **Descripción**, compruebe que todos los valores de la programación de trabajo son correctos.  
  
        7.  Haga clic en **OK**.  
  
    6.  Haga clic en **Next**.  
  
6.  En la página **Seleccionar servidores de destino** , seleccione los servidores en los que desea ejecutar el plan de mantenimiento. Esta página solo está visible en instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configuradas como servidores maestros.  
  
    > **NOTA:** Para crear un plan de mantenimiento multiservidor, debe configurarse un entorno multiservidor que contenga un servidor maestro y uno o varios servidores de destino; y el servidor local debe configurarse como servidor maestro. En entornos multiservidor, esta página muestra el servidor maestro **(local)** y todos los servidores de destino correspondientes.  
  
7.  En la página **Seleccionar tareas de mantenimiento** , seleccione una o más tareas de mantenimiento para agregar al plan. Cuando haya seleccionado todas las tareas necesarias, haga clic en **Siguiente**.  
  
    > **NOTA:** Las tareas que seleccione aquí determinarán las páginas que necesitará completar después de la página **Seleccionar el orden de las tareas de mantenimiento** .  
  
8.  En la página **Seleccionar el orden de las tareas de mantenimiento**, seleccione una tarea y haga clic en **Subir...** o en **Bajar...** para cambiar su orden de ejecución. Cuando termine, o si está satisfecho con el orden actual de las tareas, haga clic en **Siguiente**.  
  
    > **NOTA:** Si seleccionó **Programaciones independientes para cada tarea** en la página **Seleccionar propiedades del plan** anterior, no puede cambiar el orden de las tareas de mantenimiento en esta página.  
  
## <a name="define-database-check-integrity-checkdb"></a>Definir Comprobar la integridad de la base de datos (CHECKDB)  
  
 En la página **Definir la tarea Comprobar la integridad de la base de datos** , elija una o varias bases de datos donde se comprobará la asignación y la integridad estructural de las tablas y los índices de usuario y del sistema. La ejecución de la instrucción `DBCC CHECKDB`[!INCLUDE[tsql](../../includes/tsql-md.md)] garantiza que se notifiquen todos los problemas de integridad que puedan existir en la base de datos, lo que permitirá su tratamiento posterior por parte de un administrador del sistema o del propietario de la base de datos. Para obtener más información, vea [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)Cuando termine, haga clic en **Siguiente**.  
  
En esta página están disponibles las opciones siguientes.  
  
 Lista **Bases de datos**  
 Especifique las bases de datos a las que afecta esta tarea.  
  
 -  **Todas las bases de datos**  
  
Genere un plan de mantenimiento que ejecute esta tarea en todas las bases de datos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], a excepción de **tempdb**.  
  
**Bases de datos del sistema**  
  
  - Genera un plan de mantenimiento que ejecuta esta tarea en todas las bases de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , a excepción de **tempdb** y las bases de datos creadas por el usuario.  
  
 **Todas las bases de datos de usuario (master, model y msdb excluidas)**  
  
 - Genera un plan de mantenimiento que ejecuta esta tarea en todas las bases de datos creadas por los usuarios. No se ejecutarán tareas de mantenimiento en las bases de datos del sistema de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Las bases de datos**  
  
  - Genera un plan de mantenimiento que ejecuta esta tarea solo en las bases de datos seleccionadas. Si elige esta opción, deberá seleccionar al menos una base de datos de la lista.  
  
Casilla **Incluir índices**  
 - Comprueba la integridad de todas las páginas de índice y de todas las páginas de datos de tabla.  
  
**Solo físico**  
 - Limita la comprobación a la integridad de la estructura física de la página, los encabezados de registros y la coherencia de la asignación de la base de datos. Con esta opción puede reducir el tiempo de ejecución de DBCC CHECKDB en bases de datos grandes, y se recomienda para el uso frecuente en sistemas de producción.  
  
**Tablock**  
 - Hace que DBCC CHECKDB obtenga bloqueos en lugar de utilizar una instantánea de base de datos interna. Se incluye un bloqueo exclusivo (X) a corto plazo en la base de datos. El uso de esta opción puede ayudar a que DBCC CHECKDB se ejecute más rápido en una base de datos con mucha carga, pero mientras está en ejecución disminuye la simultaneidad disponible en la base de datos.  
  
## <a name="define-database-shrink-tasks"></a>Definir las tareas Reducir base de datos  
  
1.  En la página **Definir la tarea Reducir base de datos** , cree una tarea que intente reducir el tamaño de las bases de datos seleccionadas mediante la instrucción `DBCC SHRINKDATABASE` , con la opción `NOTRUNCATE` o `TRUNCATEONLY` . Para obtener más información, vea [DBCC SHRINKDATABASE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-shrinkdatabase-transact-sql.md). Cuando haya terminado, haga clic en **Siguiente**.  
  
    > **¡ADVERTENCIA!** Los datos movidos para reducir un archivo se pueden dispersar en cualquier ubicación disponible en el archivo. Esto produce la fragmentación de índices y puede reducir el rendimiento de las consultas que buscan un intervalo del índice. Para eliminar la fragmentación, considere la posibilidad de volver a generar los índices en el archivo después de la reducción.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Lista **Bases de datos**  
     Especifique las bases de datos a las que afecta esta tarea. Vea el paso 9 anterior para obtener más información sobre las opciones disponibles en esta lista.  
  
     Cuadro **Reducir la base de datos cuando se incremente por encima de**  
     Especifique el tamaño en megabytes que provoca la ejecución de esta tarea.  
  
     Cuadro **Espacio disponible tras la reducción**  
     Detiene la reducción cuando el espacio disponible en los archivos de base de datos alcanza este tamaño (como un porcentaje).  
  
     **Mantener espacio liberado en los archivos de base de datos**  
     La base de datos se comprime en páginas contiguas, pero no se cancela la asignación de las páginas y los archivos de la base de datos no se comprimen. Utilice esta opción si espera que la base de datos se expanda de nuevo y no desea reasignar el espacio. Con esta opción, los archivos de la base de datos no se comprimen lo máximo posible. Utiliza la opción NOTRUNCATE.  
  
     **Devolver espacio liberado al sistema operativo**  
     La base de datos se comprime en páginas contiguas y las páginas se devuelven al sistema operativo para que otros programas puedan utilizarlas. Utiliza la opción TRUNCATEONLY. Ésta es la opción predeterminada.  
  
## <a name="define-the-index-tasks"></a>Definir las tareas de índice  
  
1.  En la página **Definir la tarea Reorganizar índice** , seleccione el servidor o los servidores en los que va a mover páginas de índice a un orden de búsqueda más eficiente. Esta tarea usa la instrucción `ALTER INDEX ... REORGANIZE`. Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md). Cuando haya terminado, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Lista **Bases de datos**  
     Especifique las bases de datos a las que afecta esta tarea. Vea el paso 9 anterior para obtener más información sobre las opciones disponibles en esta lista.  
  
     Lista **Objeto**  
     Limita la lista **Selección** para mostrar tablas, vistas o ambas cosas. Esta lista solo está disponible si se elige una sola base de datos en la lista **Bases de datos** anterior.  
  
     Lista **Selección**  
     Especifique las tablas o índices que se ven afectados por esta tarea. No estará disponible cuando se seleccione **Tablas y vistas** en el cuadro Objeto.  
  
     Casilla **Compactar objetos grandes**  
     Cancela la asignación de espacio para tablas y vistas cuando es posible. Esta opción utiliza `ALTER INDEX ... LOB_COMPACTION = ON`.  
  
2.  En la página **Definir la tarea Volver a generar índice** , seleccione una o varias bases de datos donde va a volver a crear varios índices. Esta tarea usa la instrucción `ALTER INDEX ... REBUILD PARTITION`. Para obtener más información, vea [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md). Cuando termine, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Lista **Bases de datos**  
     Especifique las bases de datos a las que afecta esta tarea. Vea el paso 9 anterior para obtener más información sobre las opciones disponibles en esta lista.  
  
     Lista **Objeto**  
     Limita la lista **Selección** para mostrar tablas, vistas o ambas cosas. Esta lista solo está disponible si se elige una sola base de datos en la lista **Bases de datos** anterior.  
  
     Lista **Selección**  
     Especifique las tablas o índices que se ven afectados por esta tarea. No estará disponible cuando se seleccione **Tablas y vistas** en el cuadro Objeto.  
  
     Área **Opciones de espacio disponible**  
     Muestra opciones para aplicar un factor de relleno a índices y tablas.  
  
     **Espacio disponible predeterminado por página**  
     Reorganiza las páginas con la cantidad predeterminada de espacio disponible. Se quitarán los índices de las tablas de la base de datos y se volverán a crear con el factor de relleno que se especificó al crear los índices. Ésta es la opción predeterminada.  
  
     Cuadro **Cambiar el espacio disponible por página a**  
     Quita los índices de las tablas de la base de datos y vuelve a crearlos con un nuevo factor de relleno calculado automáticamente, de forma que reserva la cantidad de espacio disponible especificada en las páginas de índice. Cuanto mayor sea el porcentaje, más espacio disponible se reservará en las páginas de índice y mayor tamaño tendrá el índice. Los valores válidos son de 0 a 100. Usa la opción `FILLFACTOR` .  
  
     Área **Opciones avanzadas**  
     Muestra opciones adicionales para ordenar índices y volver a indizar.  
  
     Casilla **Ordenar resultados de tempdb**  
     Usa la opción `SORT_IN_TEMPDB` que determina dónde se almacenan temporalmente los resultados de ordenación intermedios, generados durante la creación de índices. En caso de que sea necesario realizar una operación de ordenación o de que esta pueda realizarse en la memoria, se omitirá la opción `SORT_IN_TEMPDB` .  
  
     Casilla **Rellenar índice**  
     Usa la opción `PAD_INDEX` .  
  
     Casilla **Mantener el índice en línea al volver a indexar**  
     Usa la opción `ONLINE` para permitir a los usuarios obtener acceso a los datos de la tabla subyacente o del índice clúster y a todos los índices no clúster asociados durante las operaciones de índice. La selección de esta opción activa opciones adicionales para volver a crear índices que no permiten volver a generarlos en línea: **No volver a generar índices** y **Volver a generar índices sin conexión**.  
  
     Seleccionar esta opción también activa Low Priority Used (Prioridad baja usada), que usa la opción `WAIT_AT_LOW_PRIORITY` . Las operaciones de recompilación de índices en línea esperarán a los bloqueos de prioridad baja durante `MAX_DURATION` minutos, de forma que otras operaciones podrán continuar mientras se espera la operación de compilación de índices en línea.  
  
    > **NOTA:** Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Características compatibles con las ediciones de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Casilla **MAXDOP**  
     Invalida la opción de configuración de grado máximo de paralelismo de sp_configure para DBCC CHECKDB. Para obtener más información, vea [DBCC CHECKDB &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)  
  
#### <a name="define-the-update-statistics-task"></a>Definir la tarea Actualizar estadísticas  
  
1.  En la página **Definir la tarea Actualizar estadísticas** , defina una o varias bases de datos en las que se actualizarán las estadísticas de tabla e índice. Esta tarea usa la instrucción `UPDATE STATISTICS`. Para obtener más información, vea [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md) Cuando termine, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Lista **Bases de datos**  
     Especifique las bases de datos a las que afecta esta tarea. Vea el paso 9 anterior para obtener más información sobre las opciones disponibles en esta lista.  
  
     Lista **Objeto**  
     Limita la lista **Selección** para mostrar tablas, vistas o ambas cosas. Esta lista solo está disponible si se elige una sola base de datos en la lista **Bases de datos** anterior.  
  
     Lista **Selección**  
     Especifique las tablas o índices que se ven afectados por esta tarea. No estará disponible cuando se seleccione **Tablas y vistas** en el cuadro Objeto.  
  
     **Todas las estadísticas existentes**  
     Actualiza las estadísticas tanto de las columnas como de los índices.  
  
     **Solo estadísticas de columna**  
     Solo actualiza las estadísticas de las columnas. Usa la opción `WITH COLUMNS` .  
  
     **Solo estadísticas de índice**  
     Solo actualiza las estadísticas de los índices. Usa la opción `WITH INDEX` .  
  
     **Tipo de recorrido**  
     Tipo de recorrido usado para obtener estadísticas actualizadas.  
  
     **Recorrido completo**  
     Lee todas las filas de la tabla o vista para obtener las estadísticas.  
  
     **Muestrear por**  
     Especifica el porcentaje de la tabla o vista indizada, o el número de filas del que se tomarán muestras cuando se recopilen las estadísticas de tablas o vistas de gran tamaño.  
  
#### <a name="define-the-history-cleanup-task"></a>Definir la tarea Limpieza de historial  
  
1.  En la página **Definir la tarea Limpieza de historial** , defina una o varias bases de datos donde desee descartar el historial de tareas. Esta tarea usa las instrucciones `EXEC sp_purge_jobhistory`, `EXEC sp_maintplan_delete_log`y `EXEC sp_delete_backuphistory` para quitar información de historial de las tablas **msdb** . Cuando termine, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     **Seleccionar los datos históricos para eliminar**  
     Elija el tipo de datos de tarea que desea quitar.  
  
     **Historial de copias de seguridad y restauración**  
     Conservar los registros de creación de las copias de seguridad recientes puede ser útil para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cree un plan de recuperación cuando desee restaurar una base de datos. El periodo de retención debe tener, al menos, la frecuencia de las copias de seguridad completas de la base de datos.  
  
     **Historial de trabajos del Agente SQL Server**  
     Este historial puede ser útil para solucionar errores de los trabajos o para determinar las causas de las acciones de la base de datos.  
  
     **Historial del plan de mantenimiento**  
     Este historial puede ser útil para solucionar errores de los trabajos del plan de mantenimiento o para determinar las causas de las acciones de la base de datos.  
  
     **Quitar datos históricos anteriores a**  
     Especifique la antigüedad de los elementos que desea eliminar. Puede especificar **Horas**, **Días**, **Semanas** (valor predeterminado), **Meses** o **Años**.  
  
#### <a name="define-the-execute-agent-job-task"></a>Definir la tarea Ejecutar trabajo del Agente SQL Server  
  
1.  En la página **Definir tarea Ejec. trabajo Agente** , en **Trabajos del Agente SQL Server disponibles**, elija el trabajo o los trabajos que desea ejecutar. Esta opción no estará disponible si no tiene trabajos del Agente SQL. Esta tarea usa la instrucción `EXEC sp_start_job`. Para obtener más información, vea [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md) Cuando termine, haga clic en **Siguiente**.  
  
#### <a name="define-backup-tasks"></a>Definir las tareas Copia de seguridad  
  
1.  En la página **Definir la tarea Copia de seguridad de BD (completa)** , seleccione una o varias bases de datos en la que quiere ejecutar una copia de seguridad completa. Esta tarea usa la instrucción `BACKUP DATABASE`. Para obtener más información, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Cuando termine, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Lista **Tipo de copia de seguridad**  
     Muestra el tipo de copia de seguridad que se va a realizar. Es de solo lectura.  
  
     Lista **Bases de datos**  
     Especifique las bases de datos a las que afecta esta tarea. Vea el paso 9 anterior para obtener más información sobre las opciones disponibles en esta lista.  
  
     **Componente de copia de seguridad**  
     Seleccione **Base de datos** para realizar una copia de seguridad de toda la base de datos. Seleccione **Archivo y grupos de archivos** para realizar una copia de seguridad únicamente de una parte de la base de datos. Si selecciona esta opción, debe especificar el nombre del archivo o del grupo de archivos. Cuando se seleccionan varias bases de datos en el cuadro **Bases de datos** , solo hay que especificar **Bases de datos** para **Componente de copia de seguridad**. Para realizar copias de seguridad de un archivo o grupo de archivos, cree una tarea para cada base de datos. Estas opciones solo están disponibles si se elige una sola base de datos en la lista **Bases de datos** anterior.  
  
     Casilla **El conjunto de copia de seguridad expira**  
     Especifica cuándo se puede sobrescribir el conjunto de copia de seguridad para esta copia de seguridad. Seleccione **Después de** y escriba un número de días para la expiración o seleccione **El** y especifique una fecha de expiración. Esta opción se deshabilita si la opción **Dirección URL** se selecciona como destino de la copia de seguridad.  
  
     **Copia de seguridad en**  
     Especifica el medio en el que se va a realizar la copia de seguridad de la base de datos. Seleccione **Disco**, **Cinta** o **Dirección URL**. Solo están disponibles los dispositivos de cinta conectados al equipo que contiene la base de datos.  
  
     **Realizar copia de seguridad de las bases de datos en uno o varios archivos**  
     Haga clic en **Agregar** para abrir el cuadro de diálogo **Seleccionar destino de la copia de seguridad** . Esta opción se deshabilita si seleccionó Dirección URL como destino de la copia de seguridad.  
  
     Haga clic en **Quitar** para quitar un archivo del cuadro.  
  
     Haga clic en **Contenido** para leer el encabezado de archivo y mostrar el contenido de copia de seguridad actual del archivo.  
  
     Cuadro de diálogo **Seleccionar destino de la copia de seguridad**  
     Seleccione el archivo, la unidad de cinta o el dispositivo de copia de seguridad que será el destino de la copia de seguridad. Esta opción se deshabilita si seleccionó Dirección URL como destino de la copia de seguridad.  
  
     Lista **Si existen copias de seguridad**  
     Especifique la forma de controlar las copias de seguridad existentes. Seleccione **Anexar** para agregar las nuevas copias de seguridad después de otras copias de seguridad existentes en el archivo o en la cinta. Seleccione **Sobrescribir** para quitar el contenido antiguo de un archivo o una cinta y reemplazarlo por esta nueva copia de seguridad.  
  
     **Crear un archivo de copia de seguridad para cada base de datos**  
     Crea un archivo de copia de seguridad en la ubicación especificada en el cuadro de la carpeta. Se crea un archivo para cada base de datos seleccionada. Esta opción se deshabilita si seleccionó Dirección URL como destino de la copia de seguridad.  
  
     Casilla **Crear un subdirectorio para cada base de datos**  
     Crea un subdirectorio en el directorio de disco especificado que contiene la copia de seguridad de cada base de datos de la que se hace la copia de seguridad como parte del plan de mantenimiento.  
  
    > **IMPORTANTE:** El subdirectorio heredará permisos del directorio principal. Restrinja los permisos para evitar el acceso no autorizado.  
  
     Cuadro **Carpeta**  
     Especifica la carpeta que va a contener los archivos de base de datos creados de forma automática. Esta opción se deshabilita si seleccionó Dirección URL como destino de la copia de seguridad.  
  
     **Credencial SQL**  
     Seleccione una credencial SQL utilizada para autenticarse en Azure Storage. Si no tiene una credencial existente de SQL que se pueda usar, haga clic en el botón **Crear** para crear una nueva credencial SQL.  
  
    > **IMPORTANTE:** El cuadro de diálogo que se abre al hacer clic en **Crear** requiere un certificado de administración o el perfil de publicación para la suscripción. Si no tiene acceso al certificado de administración o al perfil de publicación, puede crear una credencial de SQL; para ello, especifique la información del nombre de cuenta de almacenamiento y de clave de acceso mediante Transact-SQL o SQL Server Management Studio. Vea el código de ejemplo del tema [Crear una credencial](../../relational-databases/backup-restore/sql-server-backup-to-url.md#credential) para crear una credencial mediante Transact-SQL. O bien, con SQL Server Management Studio, desde el motor de base de datos, haga clic con el botón secundario en **Seguridad**, seleccione **Nuevo** y **Credencial**. Especifique el nombre de cuenta de almacenamiento para **Identidad** y la clave de acceso en el campo **Contraseña** .  
  
     **Contenedor de almacenamiento de Windows Azure**  
     Especifique el nombre del contenedor de Azure Storage.  
  
     **Prefijo URL:**  
     Esto se genera automáticamente en función de la información de la cuenta de almacenamiento almacenada en una credencial de SQL, y el nombre del contenedor de almacenamiento de Windows Azure que especificó. Se recomienda no editar la información de este campo a menos que esté usando un dominio que emplee un formato distinto de **\<storage account>.blob.core.windows.net**.  
  
     Cuadro **Extensión del archivo de copia de seguridad**  
     Especifique la extensión que se va a utilizar para los archivos de copia de seguridad. El valor predeterminado es .bak.  
  
     Casilla **Comprobar integridad de copia de seguridad**  
     Comprueba que el conjunto de copias de seguridad está completo y que todos los volúmenes son legibles.  
  
     Casilla **Realizar suma de comprobación**  
     Compruebe en cada página si hay suma de comprobación y página rasgada, si está habilitada y disponible, y genere una suma de comprobación para toda la copia de seguridad.  
  
     Casilla **Continuar después de un error**  
     Indica a BACKUP que continúe a pesar de la detección de errores como sumas de comprobación no válidas o páginas rasgadas.  
  
     **Cifrado de copia de seguridad**  
     Para crear un archivo de copia de seguridad, active la casilla **Cifrar copia de seguridad** . Seleccione un algoritmo de cifrado para usar para el paso de cifrado y proporcione un certificado o clave asimétrica de una lista de los certificados existentes o de claves asimétricas. Los algoritmos disponibles para el cifrado son:  
  
    -   AES 128  
  
    -   AES 192  
  
    -   AES 256  
  
    -   Triple DES  
  
     La opción de cifrado se deshabilita si seleccionó anexar en un conjunto de copia de seguridad existente.  
  
     Es una práctica recomendada hacer copias de seguridad del certificado o las claves y almacenarlas en una ubicación diferente que la copia de seguridad que cifró.  
  
     Solo se admiten las claves que residen en Administración extensible de claves (EKM).  
  
     Casilla **Tamaño de bloque** , lista  
  
     Especifica el tamaño de bloque físico, en bytes. Normalmente, esta opción afecta al rendimiento al escribir en dispositivos de cinta, matrices RAID o SAN.  
  
     Casilla **Max transfer size** (Tamaño máximo de transferencia), lista  
  
     Especifica la unidad de transferencia de mayor tamaño (en bytes) que se debe usar entre SQL Server y el medio de copia de seguridad.  
  
     Lista **Establecer compresión de copia de seguridad**  
     En [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] (o en versiones posteriores), seleccione uno los siguientes valores de [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md) :  
  
    |||  
    |-|-|  
    |**Usar la configuración de servidor predeterminada**|Haga clic para utilizar el valor predeterminado de nivel de servidor. La opción de la configuración del servidor **Compresión de copia de seguridad predeterminada** establece este valor predeterminado. Para obtener más información sobre cómo ver la configuración actual de esta opción, vea [Ver o establecer la opción de configuración del servidor de compresión de copia de seguridad predeterminada](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md).|  
    |**Comprimir copia de seguridad**|Haga clic para comprimir la copia de seguridad, sin tener en cuenta el valor predeterminado de nivel de servidor.<br /><br /> **\*\* Importante \*\*** De forma predeterminada, la compresión aumenta significativamente el uso de CPU y la CPU adicional que consume el proceso de compresión puede afectar negativamente a las operaciones simultáneas. Por tanto, podría ser conveniente crear copias de seguridad comprimidas de prioridad baja en una sesión en la que el regulador de recursos limite el uso de CPU. Para obtener más información, vea [Usar el regulador de recursos para limitar el uso de CPU mediante compresión de copia de seguridad &#40;Transact-SQL&#41;](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)limite el uso de CPU.|  
    |**No comprimir copia de seguridad**|Haga clic para crear una copia de seguridad sin comprimir, independientemente del valor predeterminado de nivel de servidor.|  
  
2.  En la página **Definir la tarea Copia de seguridad de BD (diferencial)** , seleccione una o varias bases de datos en la que quiera ejecutar una copia de seguridad parcial. Vea la lista de definiciones en el paso 16 anterior para obtener más información acerca de las opciones disponibles en esta página. Esta tarea usa la instrucción `BACKUP DATABASE ... WITH DIFFERENTIAL`. Para obtener más información, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md).  Cuando termine, haga clic en **Siguiente**.  
  
3.  En la página **Definir la tarea Copia de seguridad de BD (reg. trans.)** , seleccione una o varias bases de datos en la que quiere ejecutar una copia de seguridad del registro de transacciones. Vea la lista de definiciones en el paso 16 anterior para obtener más información acerca de las opciones disponibles en esta página. Esta tarea usa la instrucción `BACKUP LOG`. Para obtener más información, vea [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md). Cuando termine, haga clic en **Siguiente**.  
  
#### <a name="define-maintenance-cleanup-tasks"></a>Definir las tareas Limpieza de mantenimiento  
  
1.  En la página **Definir la tarea Limpieza de mantenimiento** , especifique los tipos de archivos que desea eliminar como parte del plan de mantenimiento, incluidos los informes de texto creados por planes de mantenimiento y los archivos de copia de seguridad de base de datos. Esta tarea usa la instrucción `EXEC xp_delete_file`. Cuando termine, haga clic en **Siguiente**.  
  
    > **IMPORTANTE:** Esta tarea no elimina automáticamente archivos incluidos en las subcarpetas del directorio especificado. Esta precaución reduce la posibilidad de un ataque malintencionado que utilice la tarea Limpieza de mantenimiento para eliminar archivos. Si quiere eliminar archivos en las subcarpetas de primer nivel, hay que seleccionar **Incluir subcarpetas de primer nivel**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     **Eliminar archivos del siguiente tipo**  
     Especifique el tipo de archivos que se van a eliminar.  
  
     **Archivos de copia de seguridad**  
     Elimina archivos de copia de seguridad de bases de datos.  
  
     **Informes de texto de plan de mantenimiento**  
     Elimina informes de texto de planes de mantenimientos ejecutados con anterioridad.  
  
     **Ubicación del archivo**  
     Especifique la ruta de acceso a los archivos que se van a eliminar.  
  
     **Eliminar archivo específico**  
     Elimina el archivo específico que se indica en el cuadro de texto **Nombre de archivo** .  
  
     **Buscar en carpeta y eliminar archivos según su extensión**  
     Elimina todos los archivos con la extensión especificada de la carpeta indicada. Utilice esta opción para eliminar varios archivos a la vez, por ejemplo, todos los archivos de copia de seguridad de la carpeta Martes con la extensión .bak.  
  
     Cuadro **Carpeta**  
     Ruta de acceso y nombre de la carpeta que contiene los archivos que se van a eliminar.  
  
     Cuadro **Extensión del archivo**  
     Indique la extensión de archivo de los archivos que se van a eliminar. Para eliminar varios archivos a la vez, como todos los archivos de copia de seguridad que tienen la extensión .bak en la carpeta Martes, especifique .bak.  
  
     Casilla **Incluir subcarpetas de primer nivel**  
     Elimina archivos que tienen la extensión especificada en **Extensión del archivo** de las subcarpetas de primer nivel bajo la carpeta especificada en **Carpeta**.  
  
     Casilla **Eliminar archivos en función de la antigüedad del archivo en el tiempo de ejecución de la tarea**  
     Especifique la antigüedad mínima que deben tener los archivos que quiere eliminar; para ello, especifique un número y una unidad de tiempo en el cuadro **Eliminar archivos anteriores a** .  
  
     **Eliminar archivos anteriores a**  
     Especifique la antigüedad mínima que deben tener los archivos que quiere eliminar; para ello, especifique un número y una unidad de tiempo (**Hora**, **Día**, **Semana**, **Mes** o **Año**). Se eliminarán los archivos con una antigüedad mayor que el intervalo de tiempo especificado.  
  
#### <a name="select-report-options"></a>Seleccionar opciones de informe  
  
1.  En la página **Seleccionar opciones de informe** , seleccione opciones para guardar o distribuir un informe de las acciones del plan de mantenimiento. Esta tarea usa la instrucción `EXEC sp_notify_operator`. Para obtener más información, vea [sp_notify_operator &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-notify-operator-transact-sql.md). Cuando termine, haga clic en **Siguiente**.  
  
     En esta página están disponibles las opciones siguientes.  
  
     Casilla **Escribir un informe en un archivo de texto**  
     Guarda el informe en un archivo.  
  
     Cuadro **Ubicación de la carpeta**  
     Especifica la ubicación del archivo donde se incluirá el informe.  
  
     Casilla **Enviar informe por correo electrónico**  
     Envía un mensaje de correo electrónico cuando se produce un error en una tarea. Para usar esta tarea, Correo electrónico de base de datos debe haberse habilitado y configurado correctamente con MSDB como una base de datos host de correo y debe tener un operador del Agente [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con una dirección de correo electrónico válida.  
  
     **Operador del agente**  
     Especifica el destinatario del correo electrónico.  
  
     **Perfil de correo**  
     Especifique el perfil que define al remitente del correo electrónico.  
  
#### <a name="complete-the-wizard"></a>Finalización del asistente  
  
1.  En la página **Finalización del asistente** , compruebe las selecciones realizadas en las páginas anteriores y haga clic en **Finalizar**.  
  
2.  En la página **Progreso del Asistente para planes de mantenimiento** , supervise la información de estado sobre las acciones del Asistente para planes de mantenimiento. Según las opciones que se seleccionen en el asistente, la página de progreso puede contener una o varias acciones. El cuadro superior muestra el estado general del asistente y el número de mensajes de estado, error y advertencia que ha recibido.  
  
     Las siguientes opciones están disponibles en la página **Progreso del Asistente para planes de mantenimiento** :  
  
     **Detalles**  
     Proporciona la acción, el estado y los mensajes devueltos por la acción llevada a cabo por el asistente.  
  
     **Acción**  
     Especifica el tipo y el nombre de cada acción.  
  
     **Estado**  
     Indica si la acción del asistente como conjunto ha devuelto el valor **Correcto** o **Error**.  
  
     **Mensaje**  
     Proporciona los mensajes de error o de advertencia devueltos por el proceso.  
  
     **Report**  
     Crea un informe que contiene los resultados del Asistente para la creación de particiones. Las opciones son **Ver informe**, **Guardar informe en archivo**, **Copiar informe al Portapapeles** y **Enviar informe como correo electrónico**.  
  
     **Ver informe**  
     Abre el cuadro de diálogo **Ver informe** , que contiene un informe de texto del progreso del Asistente para la creación de particiones.  
  
     **Guardar informe en archivo**  
     Abre el cuadro de diálogo **Guardar informe como** .  
  
     **Copiar informe al Portapapeles**  
     Copia los resultados del informe de progreso del asistente al Portapapeles.  
  
     **Enviar informe como correo electrónico**  
     Copia los resultados del informe de progreso del asistente en un mensaje de correo electrónico.  
  
  

