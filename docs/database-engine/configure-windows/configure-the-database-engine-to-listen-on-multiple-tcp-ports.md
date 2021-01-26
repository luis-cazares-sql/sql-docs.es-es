---
title: Configurar el motor de base de datos para escuchar en varios puertos TCP | Microsoft Docs
description: Familiarícese con los puntos de conexión del flujo TDS. Vea cómo usarlos para configurar el Motor de base de datos de SQL Server para escuchar en varios puertos TCP.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multiple
- TDS
- listen on multiple ports
- endpoints [SQL Server], TDS
- adding ports
- tabular data stream
- multiple ports
ms.assetid: 8e955033-06ef-403f-b813-3d8241b62f1f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28c73233d8a7b38ec2d14fa92c40f69d9ae0af05
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783290"
---
# <a name="configure-the-database-engine-to-listen-on-multiple-tcp-ports"></a>Configurar el motor de base de datos para escuchar en varios puertos TCP
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar el [!INCLUDE[ssDE](../../includes/ssde-md.md)] para que escuche en varios puertos TCP en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de SQL Server. Si se ha habilitado TCP/IP para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el [!INCLUDE[ssDE](../../includes/ssde-md.md)] escuchará las conexiones entrantes en un punto de conexión compuesto por una dirección IP y un número de puerto TCP. Los procedimientos siguientes crean un extremo de flujo TDS para que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escuche en otro puerto TCP.  
  
 A continuación se detallan algunos posibles motivos que pueden llevar a crear un segundo extremo de TDS:  
  
-   Incrementar la seguridad configurando el firewall de modo que se restrinja el acceso al extremo predeterminado a equipos cliente locales de una subred específica. Mantener el acceso a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a través de Internet para el equipo de soporte creando un nuevo extremo que quede expuesto a Internet a través del firewall y restringiendo los derechos de conexión a este extremo al equipo de soporte del servidor.  
  
-   Establecer afinidades entre conexiones y procesadores específicos al utilizar acceso no uniforme a memoria (NUMA).  
  
 Para configurar un extremo de TDS hay que realizar los siguientes pasos, que se pueden efectuar en cualquier orden:  
  
-   Cree el extremo de TDS para el puerto TCP y restaure el acceso al extremo predeterminado si procede.  
  
-   Conceda acceso al extremo a las entidades de seguridad de servidor que desee.  
  
-   Especifique el número de puerto TCP para la dirección IP seleccionada.  
  
 Para obtener más información sobre la configuración predeterminada de Firewall de Windows y una descripción de los puertos TCP que afectan al motor de base de datos, Analysis Services, Reporting Services e Integration Services, vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-create-a-tds-endpoint"></a>Para crear un extremo de TDS  
  
-   Ejecute la siguiente instrucción para crear un extremo denominado **CustomConnection** para el puerto 1500, para todas las direcciones TCP disponibles en el servidor.  
  
    ```  
    USE master;  
    GO  
    CREATE ENDPOINT [CustomConnection]  
    STATE = STARTED  
    AS TCP  
       (LISTENER_PORT = 1500, LISTENER_IP =ALL)  
    FOR TSQL() ;  
    GO  
    ```  
  
 Cuando se crea un nuevo extremo de [!INCLUDE[tsql](../../includes/tsql-md.md)] , los permisos de conexión correspondientes a **public** se revocan para el extremo de TDS predeterminado. Si el acceso al grupo **public** es necesario para el extremo predeterminado, aplique de nuevo este permiso mediante la instrucción `GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] to [public];` .  
  
#### <a name="to-grant-access-to-the-endpoint"></a>Para conceder acceso al extremo  
  
-   Ejecute la siguiente instrucción para conceder acceso al extremo **CustomConnection** al grupo de soporte de SQL en el dominio corporativo.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[CustomConnection] to [corp\SQLSupport] ;  
    GO  
    ```  
  
#### <a name="to-configure-the-sql-server-database-engine-to-listen-on-an-additional-tcp-port"></a>Para configurar el motor de la base de datos de SQL Server para escuchar en otro puerto TCP  
  
1.  En Administrador de configuración de SQL Server, expanda **Configuración de red de SQL Server** y, después, haga clic en **Protocolos de** _&lt;nombre_instancia&gt;_ .  
  
2.  Expanda **Protocolos de** _&lt;nombre_instancia&gt;_ y, después, haga clic en **TCP/IP**.  
  
3.  En el panel derecho, haga clic con el botón derecho en cada dirección IP deshabilitada que quiera habilitar y, después, haga clic en **Habilitar**.  
  
4.  Haga clic con el botón derecho en **IPAll** y, después, haga clic en **Propiedades**.  
  
5.  En el cuadro **Puerto TCP** , escriba los puertos en los que desee que escuche el [!INCLUDE[ssDE](../../includes/ssde-md.md)] , separados por comas. En nuestro ejemplo, si aparece el puerto predeterminado 1433, escriba **,1500** para que en el cuadro se lea **1433,1500** y, después, haga clic en **Aceptar**.  
  
    > [!NOTE]  
    >  Si no va a habilitar el puerto en todas las direcciones IP, configure el puerto adicional en el cuadro de propiedades solo para la dirección deseada. Después, en el panel de la consola, haga clic con el botón derecho en **TCP/IP**, haga clic en **Propiedades** y, en el cuadro **Escuchar todo** , seleccione **No**.  
  
6.  En el panel izquierdo, haga clic en **Servicios de SQL Server**.  
  
7.  En el panel derecho, haga clic con el botón derecho en **SQL Server** _&lt;nombre_instancia&gt;_ y, después, haga clic en **Reiniciar**.  
  
     Cuando se reinicie el [!INCLUDE[ssDE](../../includes/ssde-md.md)], el registro de errores mostrará los puertos en los que escucha [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-connect-to-the-new-endpoint"></a>Para conectarse al nuevo extremo  
  
-   Ejecute la siguiente instrucción para conectarse al punto de conexión **CustomConnection** de la instancia predeterminada de SQL Server en el servidor llamado ACCT, usando una conexión de confianza y suponiendo que el usuario es miembro del grupo [corp\SQLSupport].  
  
    ```  
    sqlcmd -SACCT,1500  
    ```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Asignación de puertos TCP/IP a nodos NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)  
  
  
