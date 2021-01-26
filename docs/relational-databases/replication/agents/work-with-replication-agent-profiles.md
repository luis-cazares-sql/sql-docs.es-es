---
description: Trabajar con perfiles del Agente de replicación
title: Trabajar con perfiles del Agente de replicación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- replication [SQL Server], agents and profiles
- replication agent profiles [SQL Server]
- agents [SQL Server replication], profiles
- profiles [SQL Server], replication agents
ms.assetid: 9c290a88-4e9f-4a7e-aab5-4442137a9918
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 613d3be8703563967241dda54725eda4c777a712
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765590"
---
# <a name="work-with-replication-agent-profiles"></a>Trabajar con perfiles del Agente de replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema, se describe cómo trabajar con perfiles de agente de replicación en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO). El comportamiento de cada agente de replicación está controlado por un conjunto de parámetros que se pueden establecer a través de perfiles de agente. Cada agente tiene un perfil predeterminado y algunos tienen perfiles adicionales predefinidos; solo hay un perfil activo para un agente en cada momento.  
  
 **En este tema**  
  
-   **Para trabajar con perfiles de agente de replicación con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
    -   Obtener acceso al cuadro de diálogo Perfiles de agente  
  
    -   Especificar un perfil para un agente  
  
    -   Crear un perfil  
  
    -   Modificar un perfil  
  
    -   Eliminar un perfil  
  
     [Transact-SQL](#TsqlProcedure)  
  
    -   Crear un perfil  
  
    -   Modificar un perfil  
  
    -   Eliminar un perfil  
  
    -   Usar perfiles de agente durante la sincronización  
  
    -   Ejemplo de Transact-SQL  
  
     [Replication Management Objects](#RMOProcedure)  
  
    -   Crear un perfil  
  
    -   Modificar un perfil  
  
    -   Eliminar un perfil  
  
-   **Seguimiento:**  [después de cambiar los parámetros de agente](#FollowUp)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
###  <a name="to-access-the-agent-profiles-dialog-box-from-sql-server-management-studio"></a><a name="Access_SSMS"></a> Para obtener acceso al cuadro de diálogo Perfiles de agente en SQL Server Management Studio  
  
1.  En la página **General** del cuadro de diálogo **Propiedades del distribuidor: \<Distributor>** , haga clic en **Valores predeterminados de perfil**.  

#### <a name="to-access-the-agent-profiles-dialog-box-from-replication-monitor"></a>Para obtener acceso al cuadro de diálogo Perfiles de agente en el Monitor de replicación  
  
-   Para abrir el cuadro de diálogo para todos los agentes, haga clic con el botón secundario en un publicador y, a continuación, en **Perfiles de agente**.  
  
-   Para abrir el cuadro de diálogo para un solo agente:  
  
    1.  Expanda un grupo de publicador en el panel izquierdo del Monitor de replicación, expanda un publicador y, a continuación, haga clic en una publicación.  
  
    2.  Para los perfiles del Agente de distribución y el Agente de mezcla, haga clic con el botón secundario en una suscripción de la pestaña **Todas las suscripciones** y, a continuación, en **Perfil del agente**. Para otros agentes, haga clic con el botón secundario en la pestaña **Agentes** y, a continuación, haga click en **Perfil del agente**.  
  
###  <a name="to-specify-a-profile-for-an-agent"></a><a name="Specify_SSMS"></a> Para especificar un perfil para un agente  
  
1.  Si el cuadro de diálogo **Perfiles de agente** muestra perfiles para varios agentes, seleccione un agente.  
  
2.  Seleccione un perfil en la columna **Predeterminado para nuevos** de la cuadrícula **Perfiles de agente** . De forma predeterminada, el perfil solamente se aplica a los agentes para las publicaciones y suscripciones nuevas.  
  
3.  Para especificar que utilicen el perfil todos los agentes del tipo seleccionado para las publicaciones o suscripciones existentes, haga clic en **Cambiar agentes existentes**.  
  
###  <a name="to-view-and-edit-the-parameters-associated-with-a-profile"></a><a name="Modify_SSMS"></a> Para ver y modificar los parámetros asociados con un perfil  
  
1.  Si el cuadro de diálogo **Perfiles de agente** muestra perfiles para varios agentes, seleccione un agente.  
  
2.  Haga clic en el botón Propiedades ( **...** ) que aparece junto a un perfil.  
  
3.  Vea los parámetros y los valores en el cuadro de diálogo **Propiedades del perfil \<ProfileName>** .  
  
    -   Los parámetros de los perfiles definidos por el usuario se pueden modificar, a diferencia de los parámetros de los perfiles predefinidos del sistema, que no se pueden modificar.  
  
    -   Para ver todos los parámetros de un agente, desactive la casilla **Mostrar solamente los parámetros utilizados en este perfil** . Para obtener información acerca de los parámetros de agentes, vea los vínculos que aparecen al final de este tema.  
  
4.  Haga clic en **Cerrar**.  
  
###  <a name="to-create-a-user-defined-profile"></a><a name="Create_SSMS"></a> Para crear un perfil definido por el usuario  
  
1.  Si el cuadro de diálogo **Perfiles de agente** muestra perfiles para varios agentes, seleccione un agente.  
  
2.  Haga clic en **Nueva**.  
  
3.  En el cuadro de diálogo de inicialización **Nuevo perfil de agente** , seleccione el perfil existente en el que desea basar el nuevo perfil.  
  
4.  En el cuadro de diálogo **Nuevo perfil de agente** , escriba valores en los cuadros de texto **Nombre** y **Descripción** .  
  
5.  Modifique los parámetros para personalizar el perfil. Para ver todos los parámetros de un agente, desactive la casilla **Mostrar solamente los parámetros utilizados en este perfil** . Para obtener información acerca de los parámetros de agentes, vea los vínculos que aparecen al final de este tema.  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="to-delete-a-user-defined-profile"></a><a name="Delete_SSMS"></a> Para eliminar un perfil definido por el usuario  
  
1.  Si el cuadro de diálogo **Perfiles de agente** muestra perfiles para varios agentes, seleccione un agente.  
  
2.  Si un perfil está asociado con uno o más agentes, cambie el perfil para esos agentes:  
  
    1.  Seleccione un perfil distinto en la cuadrícula **Perfiles de agente** .  
  
    2.  Haga clic en **Cambiar agentes existentes**.  
  
        > [!NOTE]  
        >  Al hacer esto, cambiará el perfil de todos los agentes del tipo seleccionado para las publicaciones y suscripciones existentes, y no solo de los que usen el perfil que desea eliminar.  
  
3.  Seleccione el perfil que desea eliminar y haga clic en **Eliminar**.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_tsql"></a> Para crear un nuevo perfil de agente  
  
1.  En el distribuidor, ejecute [sp_add_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-profile-transact-sql.md). Especifique **\@name**, un valor de **1** para **\@profile_type** y uno de los valores siguientes para **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     Si este perfil se va a convertir en el nuevo perfil predeterminado para su tipo de agente de replicación, especifique el valor **1** para **\@default**. El identificador del nuevo perfil se devuelve con el parámetro de salida **\@profile_id**. De esta forma crea un nuevo perfil con un conjunto de parámetros de perfil basados en el perfil predeterminado del tipo de agente indicado.  
  
2.  Una vez creado el nuevo perfil, agregue, quite o modifique los parámetros predeterminados para personalizarlo.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_tsql"></a> Para modificar un perfil de agente existente  
  
1.  En el distribuidor, ejecute [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Especifique uno de los valores siguientes para **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     De esta forma se devuelven todos los perfiles del tipo de agente especificado. Anote el valor de **profile_id** en el conjunto de resultados correspondiente al perfil que va a cambiar.  
  
2.  En el distribuidor, ejecute [sp_help_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-parameter-transact-sql.md). Especifique el identificador de perfil del paso 1 para **\@profile_id**. De esta forma se devuelven todos los parámetros del perfil. Anote el nombre de cualquier parámetro que desee modificar o quitar del perfil.  
  
3.  Para cambiar el valor de un parámetro de un perfil, ejecute [sp_change_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-change-agent-parameter-transact-sql.md). Especifique el identificador de perfil del paso 1 para **\@profile_id**, el nombre del parámetro que se va a cambiar en **\@parameter_name** y un nuevo valor para el parámetro en **\@parameter_value**.  
  
    > [!NOTE]  
    >  No puede cambiar un perfil de agente existente para convertirlo en el perfil predeterminado de un agente. Debe crear un nuevo perfil como perfil predeterminado, como se muestra en el procedimiento anterior.  
  
4.  Para quitar un parámetro de un perfil, ejecute [sp_drop_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-parameter-transact-sql.md). Especifique el identificador de perfil del paso 1 para **\@profile_id** y el nombre del parámetro que va a quitar para **\@parameter_name**.  
  
5.  Para agregar un nuevo parámetro a un perfil, debe hacer lo siguiente:  
  
    -   Consulte la tabla [MSagentparameterlist &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/msagentparameterlist-transact-sql.md) en el distribuidor para determinar qué parámetros de perfil se pueden establecer en cada tipo de agente.  
  
    -   En el distribuidor, ejecute [sp_add_agent_parameter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-add-agent-parameter-transact-sql.md). Especifique el identificador de perfil del paso 1 para **\@profile_id**, el nombre de un parámetro válido para agregar en **\@parameter_name** y el valor del parámetro para **\@parameter_value**.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_tsql"></a> Para eliminar un perfil de agente  
  
1.  En el distribuidor, ejecute [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Especifique uno de los valores siguientes para **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     De esta forma se devuelven todos los perfiles del tipo de agente especificado. Anote el valor de **profile_id** en el conjunto de resultados correspondiente al perfil que va a quitar.  
  
2.  En el distribuidor, ejecute [sp_drop_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-drop-agent-profile-transact-sql.md). Especifique el identificador de perfil del paso 1 para **\@profile_id**.  
  
###  <a name="to-use-agent-profiles-during-synchronization"></a><a name="Synch_tsql"></a> Para usar perfiles de agente durante la sincronización  
  
1.  En el distribuidor, ejecute [sp_help_agent_profile &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-help-agent-profile-transact-sql.md). Especifique uno de los valores siguientes para **\@agent_type**:  
  
    -   **1** - [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)  
  
    -   **2** - [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)  
  
    -   **3** - [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)  
  
    -   **4** - [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)  
  
    -   **9** - [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
     De esta forma se devuelven todos los perfiles del tipo de agente especificado. Anote el valor de **profile_name** en el conjunto de resultados correspondiente al perfil que va a usar.  
  
2.  Si el agente se inicia a partir de un trabajo de agente, modifique el paso de trabajo que inicia el agente para especificar el valor de **profile_name** obtenido en el paso 1 después del parámetro de línea de comandos **-ProfileName** . Para obtener más información, consulte [Ver y modificar parámetros del símbolo del sistema de los agentes de replicación &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md).  
  
3.  Al iniciar el agente desde el símbolo del sistema, especifique el valor de **profile_name** obtenido en el paso 1 después del parámetro de línea de comandos **-ProfileName** .  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 Este ejemplo crea un perfil personalizado para el Agente de mezcla denominado **custom_merge**, cambia el valor del parámetro **-UploadReadChangesPerBatch** , agrega un nuevo parámetro **-ExchangeType** y devuelve información sobre el perfil que se crea.  
  
 [!code-sql[HowTo#sp_addagentprofileparam](../../../relational-databases/replication/codesnippet/tsql/work-with-replication-ag_1.sql)]  
  
##  <a name="using-rmo"></a><a name="RMOProcedure"></a> Usar RMO  
  
###  <a name="to-create-a-new-agent-profile"></a><a name="Create_RMO"></a> Para crear un nuevo perfil de agente  
  
1.  Cree una conexión al distribuidor utilizando una instancia de la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.AgentProfile>.  
  
3.  Establezca las siguientes propiedades en el objeto:  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> : nombre del perfil.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AgentType%2A> : un valor <xref:Microsoft.SqlServer.Replication.AgentType> que especifica el tipo de Agente de replicación para el que se crea el perfil.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> : la <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creada en el paso 1.  
  
    -   (Opcional) <xref:Microsoft.SqlServer.Replication.AgentProfile.Description%2A> : una descripción del perfil.  
  
    -   (Opcional) <xref:Microsoft.SqlServer.Replication.AgentProfile.Default%2A> : establezca esta propiedad en **true** si todos los nuevos trabajos de agente para <xref:Microsoft.SqlServer.Replication.AgentType> van a utilizar de forma predeterminada este perfil.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.Create%2A> para crear el perfil en el servidor.  
  
5.  Una vez que el perfil existe en el servidor, puede personalizarlo agregando, quitando o cambiando los valores de parámetros de agente de replicación.  
  
6.  Para asignar el perfil a un trabajo del agente de replicación existente, llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.AssignToAgent%2A> . Pase el nombre de la base de datos de distribución para *distributionDBName* y el Id. del trabajo para *agentID*.  
  
###  <a name="to-modify-an-existing-agent-profile"></a><a name="Modify_RMO"></a> Para modificar un perfil de agente existente  
  
1.  Cree una conexión al distribuidor utilizando una instancia de la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationServer>. Pase el objeto <xref:Microsoft.SqlServer.Management.Common.ServerConnection> creado en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, compruebe que el distribuidor existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationServer.EnumAgentProfiles%2A> . Pase un valor <xref:Microsoft.SqlServer.Replication.AgentType> para reducir los perfiles devueltos a un tipo específico de agente de replicación.  
  
5.  Obtenga el objeto <xref:Microsoft.SqlServer.Replication.AgentProfile> que desea del <xref:System.Collections.ArrayList>devuelto, donde la propiedad <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> del objeto coincide con el nombre del perfil.  
  
6.  Llame a uno de los métodos siguientes de <xref:Microsoft.SqlServer.Replication.AgentProfile> para cambiar el perfil:  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.AddParameter%2A> : agrega un parámetro compatible al perfil, donde *name* es el nombre del parámetro del Agente de replicación y *value* es el valor especificado. Para enumerar todos los parámetros del agente compatibles para un tipo de agente determinado, llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Este método devuelve <xref:System.Collections.ArrayList> de objetos <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> que representa todos los parámetros compatibles.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.RemoveParameter%2A> : quita un parámetro existente del perfil, donde *name* es el nombre del parámetro de Agente de replicación. Para enumerar todos los parámetros de agente actuales definidos para el perfil, llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Este método devuelve <xref:System.Collections.ArrayList> de objetos <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> que representan el parámetro existente para este perfil.  
  
    -   <xref:Microsoft.SqlServer.Replication.AgentProfile.ChangeParameter%2A> : cambia la configuración de un parámetro existente en el perfil, donde *name* es el nombre del parámetro del agente y *newValue* es el valor al que se cambia el parámetro. Para enumerar todos los parámetros de agente actuales definidos para el perfil, llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameters%2A> . Este método devuelve <xref:System.Collections.ArrayList> de objetos <xref:Microsoft.SqlServer.Replication.AgentProfileParameter> que representan el parámetro existente para este perfil. Para enumerar todos los valores de parámetro del agente compatibles, llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.EnumParameterInfo%2A> . Este método devuelve <xref:System.Collections.ArrayList> de objetos <xref:Microsoft.SqlServer.Replication.AgentProfileParameterInfo> que representa los valores compatibles para todos los parámetros.  
  
###  <a name="to-delete-an-agent-profile"></a><a name="Delete_RMO"></a> Para eliminar un perfil de agente  
  
1.  Cree una conexión al distribuidor utilizando una instancia de la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.AgentProfile>. Establezca el nombre del perfil para <xref:Microsoft.SqlServer.Replication.AgentProfile.Name%2A> y la <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, significa que el nombre especificado era incorrecto o el perfil no existe en el servidor.  
  
4.  Compruebe que la propiedad <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A> está establecida en <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.User>, que indica un perfil del cliente. No debería quitar ningún perfil que tenga un valor de <xref:Microsoft.SqlServer.Replication.AgentProfileTypeOption.System> para <xref:Microsoft.SqlServer.Replication.AgentProfile.Type%2A>.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.AgentProfile.Remove%2A> para quitar el perfil definido por el usuario representado por este objeto del servidor.  
  
##  <a name="follow-up-after-changing-agent-parameters"></a><a name="FollowUp"></a> Seguimiento: después de cambiar los parámetros de agente  
Los cambios en los parámetros del agente tendrán efecto la próxima vez que se inicie el agente. Si el agente se ejecuta sin interrupción, debe detenerlo y reiniciarlo. A partir de SQL Server 2017 CU3, algunos cambios de parámetros de agente surten efecto sin tener que reiniciar los agentes. 
  
## <a name="see-also"></a>Consulte también  
 [Perfiles del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-profiles.md)   
 [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md)   
 [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md)   
 [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md)   
 [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md)   
 [Agente de lectura de cola de replicación](../../../relational-databases/replication/agents/replication-queue-reader-agent.md)  
  
  
