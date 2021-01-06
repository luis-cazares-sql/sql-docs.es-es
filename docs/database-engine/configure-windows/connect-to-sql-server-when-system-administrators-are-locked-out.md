---
title: Conexión a SQL Server cuando los administradores del sistema están bloqueados | Microsoft Docs
description: Aprenda a recuperar el acceso a SQL Server como administrador del sistema si se le ha bloqueado por error.
ms.custom: contperf-fy20q4
ms.date: 05/20/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- sa account
- connecting when locked out [SQL Server]
- locked out [SQL Server]
ms.assetid: c0c0082e-b867-480f-a54b-79f2a94ceb67
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3b4cf5946fba4eb3c79150b29ef44bd232f9c155
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878956"
---
# <a name="connect-to-sql-server-when-system-administrators-are-locked-out"></a>Conexión a SQL Server cuando los administradores del sistema están bloqueados 
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
En este artículo, se explica cómo recuperar el acceso al [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] como administrador del sistema si se le ha bloqueado.  Un administrador del sistema puede perder el acceso a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por alguna de las razones siguientes:  
  
-   Por equivocación se han quitado todos los inicios de sesión que son miembros del rol fijo de servidor sysadmin.  
  
-   Por equivocación se han quitado todos los grupos de Windows que son miembros del rol fijo de servidor sysadmin.  
  
-   Los inicios de sesión que son miembros del rol fijo de servidor sysadmin son para individuos que han dejado la compañía o no están disponibles.  
  
-   La cuenta sa está deshabilitada o nadie conoce la contraseña.  
  
## <a name="resolution"></a>Resolución

Para resolver el problema de acceso, se recomienda iniciar la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único. Este modo evita que se produzcan otras conexiones mientras se intenta recuperar el acceso. Desde ahí, puede conectar a la instancia de SQL Server y agregar el inicio de sesión al rol del servidor **sysadmin**. En la sección [Instrucciones paso a paso](#step-by-step-instructions) se proporcionan los pasos detallados de esta solución.


Puede iniciar una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único con las opciones `-m` o `-f` de la línea de comandos. Luego cualquier miembro del grupo Administradores local del equipo puede conectarse a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como miembro del rol fijo de servidor **sysadmin**.  

Cuando inicie la instancia en modo de usuario único, primero detenga el servicio Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De lo contrario, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría conectarse primero y tomar la única conexión disponible al servidor, con lo que usted no podría iniciar sesión.

También es posible que una aplicación cliente desconocida tomara la única conexión disponible antes de que usted pudiera iniciar sesión. Para evitar que esto suceda, puede usar la opción `-m` seguida de un nombre de aplicación para limitar las conexiones a una única conexión de la aplicación especificada. Por ejemplo, si se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con `-mSQLCMD` se limitan las conexiones a una única conexión que se identifica a sí misma como el programa cliente **sqlcmd**. Para conectarse mediante el editor de consultas de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], use `-m"Microsoft SQL Server Management Studio - Query"`.  


> [!IMPORTANT]  
> No use `-m` con un nombre de aplicación como característica de seguridad. Las aplicaciones cliente especifican el nombre de aplicación por medio de la configuración de la cadena de conexión, así que se puede falsificar fácilmente con un nombre falso.

En la tabla siguiente se resumen las distintas formas de iniciar la instancia en modo de usuario único en la línea de comandos.

| Opción | Descripción | Cuándo se usa |
|:---|:---|:---|
|`-m` | Limita las conexiones a una única conexión | Si no hay ningún otro usuario intentando conectarse a la instancia o si no está seguro del nombre de aplicación que va a usar para conectarse a la instancia. |
|`-mSQLCMD`| Limita las conexiones a una única conexión que se debe identificar a sí misma como el programa cliente **sqlcmd**| Si planea conectarse a la instancia con **sqlcmd** y quiere evitar que otras aplicaciones tomen la única conexión disponible. |
|`-m"Microsoft SQL Server Management Studio - Query"`| Limita las conexiones a una única conexión que debe identificarse a sí misma como la aplicación **Microsoft SQL Server Management Studio - Consulta**.| Si planea conectarse a la instancia mediante el editor de consultas de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y quiere evitar que otras aplicaciones tomen la única conexión disponible. |
|`-f`| Limita las conexiones a una única conexión e inicia la instancia en configuración mínima | Si alguna otra configuración está evitando que inicie. |
| &nbsp; | &nbsp; | &nbsp; |
  
Para obtener instrucciones paso a paso sobre cómo iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único, vea [Iniciar SQL Server en modo de usuario único](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md).

## <a name="step-by-step-instructions"></a>Instrucciones paso a paso

En las siguientes instrucciones paso a paso se explica cómo conceder permisos de administrador del sistema a un inicio de sesión de SQL Server que ya no tiene acceso por equivocación.

En estas instrucciones se da por hecho que

* [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se está ejecutando en Windows 8 o superior. Se proporcionan pequeños ajustes para versiones anteriores de SQL Server o Windows si es necesario.

* [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] está instalado en el equipo.  

Siga estas instrucciones con la sesión iniciada en Windows como miembro del grupo Administradores local.

1.  En el menú Inicio de Windows, haga clic con el botón derecho en el icono del Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y seleccione **Ejecutar como administrador** para pasar las credenciales de administrador al Administrador de configuración.  
  
2.  En el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en el panel izquierdo, seleccione **Servicios de SQL Server**. En el panel derecho, busque la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. (La instancia predeterminada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye **(MSSQLSERVER)** después del nombre del equipo. Las instancias con nombre aparecen en mayúsculas con el mismo nombre que tienen en Servidores registrados). Haga clic con el botón derecho en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, después, haga clic en **Propiedades**.  
  
3.  En la pestaña **Parámetros de inicio**, en el cuadro **Especifique un parámetro de inicio**, escriba `-m` y, después, haga clic en **Agregar**. (Es un guion y a continuación una letra m minúscula).  
  
    > [!NOTE]  
    >  En algunas versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no hay ninguna pestaña **Parámetros de inicio** . En ese caso, en la pestaña **Opciones avanzadas** , haga doble clic en **Parámetros de inicio**. Los parámetros se abrirán en una ventana muy pequeña. Tenga cuidado de no cambiar ninguno de los parámetros existentes. Al final, agregue un nuevo parámetro `;-m` y, a continuación, haga clic en **Aceptar**. (Es un punto y coma, después un guion y a continuación una letra m minúscula).  
  
4.  Haga clic en **Aceptar** y, después del mensaje para reiniciar, haga clic con el botón derecho en el nombre del servidor; luego, haga clic en **Reiniciar**.  
  
5.  Después de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se haya reiniciado, el servidor se encuentra en modo de usuario único. Asegúrese de que el Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no se esté ejecutando. Si está iniciado, usará su única conexión.  
  
6.  En el menú Inicio de Windows, haga clic con el botón derecho en el icono de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y seleccione **Ejecutar como administrador**. Esto pasa las credenciales de administrador a SSMS.
  
    > [!NOTE]  
    >  En versiones anteriores de Windows, la opción **Ejecutar como administrador** aparece como un submenú.  
  
     En algunas configuraciones, SSMS intentará realizar varias conexiones. Se producirá un error en varias conexiones porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está en modo de usuario único. En función del escenario, realice una de las siguientes acciones.  
  
    1.  Conéctese al Explorador de objetos mediante autenticación de Windows, que incluye las credenciales de administrador. Expanda **Seguridad** e **Inicios de sesión** y haga doble clic en su propio inicio de sesión. En la página **Roles de servidor** , seleccione **sysadmin** y, a continuación, haga clic en **Aceptar**.  
  
    2.  En lugar de conectar con el Explorador de objetos, conectar con una ventana de consulta mediante la autenticación de Windows (que incluye sus credenciales de administrador). (Solo puede conectarse de esta forma si no se conectó con el Explorador de objetos). Ejecute código como el siguiente para agregar un nuevo inicio de sesión Autenticación de Windows que sea miembro del rol fijo de servidor **sysadmin** . El ejemplo siguiente agrega un usuario de dominio denominado `CONTOSO\PatK`.  
  
        ```  
        CREATE LOGIN [CONTOSO\PatK] FROM WINDOWS;  
        ALTER SERVER ROLE sysadmin ADD MEMBER [CONTOSO\PatK];  
        ```  
  
    3.  Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en modo de autenticación mixto, conectar con una ventana de consulta mediante la autenticación de Windows (que incluye sus credenciales de administrador). Ejecute código como el siguiente para crear un nuevo inicio de sesión de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que sea miembro del rol fijo de servidor **sysadmin**.  
  
        ```  
        CREATE LOGIN TempLogin WITH PASSWORD = '************';  
        ALTER SERVER ROLE sysadmin ADD MEMBER TempLogin;  
        ```  
  
        > [!WARNING]  
        >  Reemplace ************ con una contraseña segura.  
  
    4.  Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta en modo de autenticación mixto y quiere restablecer la contraseña de la cuenta **sa** , conecte con una ventana de consulta por medio de la autenticación de Windows (que incluye sus credenciales de administrador). Cambie la contraseña de la cuenta **sa** con la siguiente sintaxis.  
  
        ```  
        ALTER LOGIN sa WITH PASSWORD = '************';  
        ```  
  
        > [!WARNING]  
        >  Reemplace ************ con una contraseña segura.  

7. Cierre [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
8. Los pasos siguientes cambian SQL Server al modo multiusuario. En el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , en el panel izquierdo, seleccione **Servicios de SQL Server**.

9. En el panel derecho, haga clic con el botón derecho en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]y, después, haga clic en **Propiedades**.  
  
10. En la pestaña **Parámetros de inicio** , en el cuadro **Parámetros existentes** , seleccione `-m` y, después, haga clic en **Quitar**.  
  
    > [!NOTE]  
    >  En algunas versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no hay ninguna pestaña **Parámetros de inicio** . En ese caso, en la pestaña **Opciones avanzadas** , haga doble clic en **Parámetros de inicio**. Los parámetros se abrirán en una ventana muy pequeña. Quite el parámetro `;-m` que agregó anteriormente y, a continuación, haga clic en **Aceptar**.  
  
11. Haga clic con el botón derecho en el nombre del servidor y, después, haga clic en **Reiniciar**. Si ha detenido el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], asegúrese de volver a iniciarlo antes de iniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único.
  
Ahora debe poder conectarse normalmente con una de las cuentas que es miembro del rol fijo de servidor **sysadmin** .  
  
## <a name="see-also"></a>Consulte también  

* [Configurar las opciones de inicio del servidor](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
* [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md)  
