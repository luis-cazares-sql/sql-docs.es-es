---
description: Modificar una función de partición
title: Modificación de una función de partición | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: ae5bfc09-f27a-4ea9-9518-485278b11674
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b200814dedac25809d1295146ff71d8908bce4da
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766119"
---
# <a name="modify-a-partition-function"></a>Modificar una función de partición
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Puede cambiar el modo en que se crean las particiones de una tabla o un índice en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] al sumar o restar el número de particiones especificadas, en aumentos de 1, en la función de partición de la tabla o el índice con particiones mediante [!INCLUDE[tsql](../../includes/tsql-md.md)]. Lo que sucede al agregar una partición es que se "divide" una partición existente en dos particiones y se vuelven a definir los límites de las particiones nuevas. Al quitar una partición, se "mezclan" los límites de dos particiones en una sola. Lo que hace esta última acción es volver a llenar una partición y dejar la otra sin asignar.  
  
> [!CAUTION]  
>  Varias tablas o índices pueden utilizar la misma función de partición. Cuando se modifica una función de partición, afecta a todas las funciones en una sola transacción. Compruebe las dependencias de la función de partición antes de modificarla.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para modificar una función de partición, use:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   ALTER PARTITION FUNCTION solo se puede utilizar para dividir en dos una partición o para mezclar dos particiones en una sola. Para cambiar el modo en que se crean las particiones de una tabla o un índice (de 10 a 5 particiones, por ejemplo) puede recurrir a cualquiera de las opciones que se indican a continuación:  
  
    -   Cree una nueva tabla con particiones con la función de partición deseada y, a continuación, inserte los datos de la tabla antigua en la tabla nueva mediante la instrucción INSERT INTO... SELECT FROM [!INCLUDE[tsql](../../includes/tsql-md.md)] o con el **Asistente para administrar particiones** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
    -   Cree un índice clúster con particiones en un montón.  
  
        > [!NOTE]  
        >  El resultado de quitar un índice clúster con particiones es un montón con particiones.  
  
    -   Quite y vuelva a generar un índice con particiones existente mediante la instrucción CREATE INDEX de [!INCLUDE[tsql](../../includes/tsql-md.md)] con la cláusula DROP EXISTING = ON.  
  
    -   Ejecute una secuencia de instrucciones ALTER PARTITION FUNCTION.  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no proporciona compatibilidad de replicación para modificar una función de partición. Si desea realizar cambios en una función de partición de una base de datos de publicación, deberá hacerlo manualmente en la base de datos de suscripciones.  
  
-   Todos los grupos de archivos que estén afectados por ALTER PARTITION FUNCTION deben estar en línea.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se pueden utilizar cualquiera de los siguientes permisos para ejecutar ALTER PARTITION FUNCTION:  
  
-   Permiso ALTER ANY DATASPACE. De forma predeterminada, este permiso corresponde a los miembros del rol fijo de servidor **sysadmin** y a los roles fijos de base de datos **db_owner** y **db_ddladmin** .  
  
-   Permiso CONTROL o ALTER en la base de datos en la que se ha creado la función de partición.  
  
-   Permiso CONTROL SERVER o ALTER ANY DATABASE en el servidor de la base de datos en la que se ha creado la función de partición.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para modificar una función de partición:**  
  
 Esta acción específica no se puede realizar mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para modificar una función de partición, primero debe eliminar la función y después crear una nueva con las propiedades deseadas mediante el Asistente para la creación de particiones. Para obtener más información, vea  
  
#### <a name="to-delete-a-partition-function"></a>Para eliminar una función de partición  
  
1.  Expanda la base de datos donde desea eliminar la función de partición y después expanda la carpeta **Almacenamiento**.  
  
2.  Expanda la carpeta de **Funciones de partición** .  
  
3.  Haga clic con el botón derecho en la función de partición que quiera eliminar y seleccione **Eliminar**.  
  
4.  En el cuadro de diálogo **Eliminar objeto** , asegúrese de que está seleccionada la función de partición correcta y después haga clic en **Aceptar**.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-split-a-single-partition-into-two-partitions"></a>Para dividir una única partición en dos particiones  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Split the partition between boundary_values 100 and 1000  
    --to create two partitions between boundary_values 100 and 500  
    --and between boundary_values 500 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    SPLIT RANGE (500);  
    ```  
  
#### <a name="to-merge-two-partitions-into-one-partition"></a>Para combinar dos particiones en una única partición  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- Look for a previous version of the partition function "myRangePF1" and deletes it if it is found.  
    IF EXISTS (SELECT * FROM sys.partition_functions  
        WHERE name = 'myRangePF1')  
        DROP PARTITION FUNCTION myRangePF1;  
    GO  
    -- Create a new partition function called "myRangePF1" that partitions a table into four partitions.  
    CREATE PARTITION FUNCTION myRangePF1 (int)  
    AS RANGE LEFT FOR VALUES ( 1, 100, 1000 );  
    GO  
    --Merge the partitions between boundary_values 1 and 100  
    --and between boundary_values 100 and 1000 to create one partition  
    --between boundary_values 1 and 1000.  
    ALTER PARTITION FUNCTION myRangePF1 ()  
    MERGE RANGE (100);  
    ```  
  
 Para obtener más información, vea [ALTER PARTITION FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-partition-function-transact-sql.md).  
  
  
