---
description: Definir un artículo
title: Definición de un artículo | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], defining
- sp_addmergearticle
- adding articles
- sp_addarticle
- articles [SQL Server replication], adding
ms.assetid: 220584d8-b291-43ae-b036-fbba3cc07a2e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: b93fbfea46931eb460b842a3a34ae13853726792
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766367"
---
# <a name="define-an-article"></a>Definir un artículo
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo definir un artículo en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para definir un artículo, mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Los nombres del artículo no pueden incluir ninguno de los caracteres siguientes: % , * , [ , ] , | , : , " , ? , ' , \ , / , < , >. Si los objetos de la base de datos incluyen cualquiera de estos caracteres y desea replicarlos, debe especificar un nombre de artículo diferente del nombre de objeto.  
  
##  <a name="security"></a><a name="Security"></a> Seguridad  
 Cuando sea posible, pida a los usuarios que proporcionen credenciales de seguridad en tiempo de ejecución. Si debe almacenar credenciales, use los [servicios de cifrado](/previous-versions/aa719848(v=vs.71)) (en inglés) proporcionados por [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows .NET Framework.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Cree publicaciones y defina artículos con el Asistente para nueva publicación. Una vez que se ha creado la publicación, se pueden ver y modificar sus propiedades en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener información sobre cómo crear una publicación de una base de datos de Oracle, vea [Crear una publicación a partir de una base de datos de Oracle](../../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md).  
  
#### <a name="to-create-a-publication-and-define-articles"></a>Para crear publicaciones y definir artículos  
  
1.  Conéctese al publicador de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, haga clic con el botón secundario en la carpeta **Publicaciones locales** .  
  
3.  Haga clic en **Nueva publicación**.  
  
4.  Siga las indicaciones de las páginas del Asistente para nueva publicación para:  

    -   Especificar un distribuidor si la distribución no se ha configurado en el servidor. Para obtener más información sobre cómo configurar la distribución, vea [Configurar la publicación y la distribución](../../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
         Si en la página **Distribuidor** especifica que el servidor del publicador actúe como su propio distribuidor (un distribuidor local) y el servidor no está configurado como tal, el Asistente para nueva publicación configurará el servidor. Especifique una carpeta de instantáneas predeterminada para el distribuidor en la página **Carpeta de instantáneas** . La carpeta de instantáneas es simplemente un directorio designado como recurso compartido; los agentes que leen y escriben en esta carpeta deben tener permisos de acceso suficientes a ella. Para obtener más información sobre cómo proteger la carpeta adecuadamente, vea [Proteger la carpeta de instantáneas](../../../relational-databases/replication/security/secure-the-snapshot-folder.md).  
  
         Si especifica que otro servidor actúe como distribuidor, deberá escribir una contraseña en la página **Contraseña administrativa** para las conexiones que se realicen desde el publicador al distribuidor. Esta contraseña debe coincidir con la especificada cuando se habilitó el publicador en el distribuidor remoto.  
  
         Para más información, consulte [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
    -   Elegir una base de datos de publicación.  
  
    -   Seleccionar un tipo de publicación. Para obtener más información, vea [Tipos de replicación](../../../relational-databases/replication/types-of-replication.md).  
  
    -   Especificar los datos y los objetos de base de datos que se van a publicar; de forma opcional, filtrar columnas de artículos de tablas y establecer las propiedades de los artículos.  
  
    -   De forma opcional, filtrar filas de artículos de tablas. Para obtener más información, vea [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md).  
  
    -   Establecer la programación del Agente de instantáneas.  
  
    -   Especificar las credenciales con las que los siguientes agentes de replicación se ejecutan y efectúan conexiones:  
  
         \- Agente de instantáneas (para todas las publicaciones)  
  
         \- Agente de registro del LOG (para todas las publicaciones transaccionales)  
  
         \- Agente de lectura de cola para publicaciones transaccionales que permiten suscripciones de actualización.  
  
         Para obtener más información, consulte [Replication Agent Security Model](../../../relational-databases/replication/security/replication-agent-security-model.md) y [Replication Security Best Practices](../../../relational-databases/replication/security/replication-security-best-practices.md).  
  
    -   De forma opcional, incluir la publicación. Para más información, consulte [Scripting Replication](../../../relational-databases/replication/scripting-replication.md).  
  
    -   Especificar un nombre para la publicación.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Una vez creada una publicación, los artículos se pueden crear mediante programación usando los procedimientos almacenados de replicación. Los procedimientos almacenados usados para crear un artículo dependerán del tipo de publicación para la que se está definiendo el artículo. Para obtener más información, vea [Crear una suscripción](../../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-define-an-article-for-a-snapshot-or-transactional-publication"></a>Para definir un artículo para una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, un nombre de artículo para `@article`, el objeto de base de datos que se va a publicar para `@source_object` y cualquier otro parámetro opcional. Use `@source_owner` para especificar la propiedad del esquema del objeto, si no es **dbo**. Si el artículo no es un artículo de tabla basado en registros, especifique el tipo de artículo para `@type`; para más información, vea [Especificación de tipos de artículo &#40;Programación de la replicación con Transact-SQL&#41;](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md).  
  
2.  Para filtrar filas horizontalmente en una tabla o ver un artículo, use [sp_articlefilter](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) para definir la cláusula de filtro. Para más información, consulte [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md).  
  
3.  Para filtrar columnas verticalmente en una tabla o ver un artículo, use [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Para más información, consulte [definir y modificar un filtro de columna](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
4.  Ejecute [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) si se filtra el artículo.  
  
5.  Si la publicación tiene suscripciones existentes y [sp_helppublication](../../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) devuelve un valor de **0** en la columna **de immediate_sync** , debe llamar a [sp_addsubscription](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md) para agregar el artículo a cada suscripción existente.  
  
6.  Si la publicación tiene suscripciones de extracción existentes, ejecute [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) en el Publicador para crear una nueva instantánea para las suscripciones de extracción existentes que contienen únicamente el nuevo artículo.  
  
    > [!NOTE]  
    >  Para las suscripciones que no se inicializan usando una instantánea, no tiene que ejecutar [sp_refreshsubscriptions](../../../relational-databases/system-stored-procedures/sp-refreshsubscriptions-transact-sql.md) ya que este procedimiento lo ejecuta [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).  
  
#### <a name="to-define-an-article-for-a-merge-publication"></a>Para definir un artículo para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique el nombre de la publicación para `@publication`, un nombre para el nombre de artículo para `@article` y el objeto que se va a publicar para `@source_object`. Para filtrar horizontalmente filas de tabla, especifique un valor para `@subset_filterclause`. Para obtener más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md) y [Definir y modificar un filtro de fila estático](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md). Si el artículo no es un artículo de tabla, especifique el tipo de artículo para `@type`. Para obtener más información, vea [Especificar tipos de artículo &#40;programación de la replicación con Transact-SQL&#41;](../../../relational-databases/replication/publish/specify-article-types-replication-transact-sql-programming.md).  
  
2.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) para definir un filtro de combinación entre dos artículos. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
3.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) para filtrar columnas de tabla. Para más información, consulte [definir y modificar un filtro de columna](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En este ejemplo se define un artículo basado en la tabla `Product` para una publicación transaccional, donde el artículo se filtra tanto horizontal como verticalmente.  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_1.sql)]  
  
 En este ejemplo se definen artículos para una publicación de mezcla, donde el artículo `SalesOrderHeader` se filtra estáticamente según **SalesPersonID** y el artículo `SalesOrderDetail` se filtra por combinación según `SalesOrderHeader`.  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-an-article_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede definir artículos mediante programación utilizando Replication Management Objects (RMO). Las clases RMO que usa para definir un artículo dependen del tipo de publicación para la que se define el artículo.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 El ejemplo siguiente agrega un artículo con filtros de filas y columnas a una publicación transaccional.  
  
 [!code-cs[HowTo#rmo_CreateTranArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranarticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranarticles)]  
  
 El ejemplo siguiente agrega tres artículos a una publicación de combinación. Los artículos tienen filtros de columna y se usan dos filtros de combinación para propagar un filtro de fila con parámetros a los demás artículos.  
  
 [!code-cs[HowTo#rmo_CreateMergeArticles](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergearticles)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergeArticles](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergearticles)]  
  
## <a name="see-also"></a>Consulte también  
 [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Agregar y quitar artículos de publicaciones existentes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Conceptos de procedimientos almacenados del sistema de replicación](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
