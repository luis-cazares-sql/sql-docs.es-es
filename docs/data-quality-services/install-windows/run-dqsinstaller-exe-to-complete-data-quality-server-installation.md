---
title: Ejecutar DQSInstaller.exe para completar la instalación del servidor de calidad de datos
description: Obtenga información acerca de cómo ejecutar el DQSInstaller.exe para completar una instalación de SQL Server Data Quality Services (DQS).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 7a8c96e0-1328-4f35-97fc-b6d9cb808bae
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 2933803ce4a8bce84468f4ce2e360a18d2444635
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765794"
---
# <a name="run-dqsinstallerexe-to-complete-data-quality-server-installation"></a>Ejecutar DQSInstaller.exe para completar la instalación del servidor de calidad de datos

[!INCLUDE [SQL Server - Windows only ](../../includes/applies-to-version/sql-windows-only.md)]

  Para completar la instalación del [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] , debe ejecutar el archivo DQSInstaller.exe después de instalar [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. En este tema se describe cómo ejecutar DQSInstaller.exe desde la pantalla **Inicio** , el menú **Inicio** , el Explorador de Windows o el símbolo del sistema; puede elegir cualquiera de estos métodos para ejecutar el archivo DQSInstaller.exe.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Debe tener seleccionado **Data Quality Services** debajo de **Servicios de Motor de base de datos** en la página de selección de características en el programa de instalación de SQL Server al instalar SQL Server y haber completado la instalación de SQL Server. Para más información, consulte [Install Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md).  
  
-   La cuenta de usuario de Windows debe ser miembro del rol fijo de servidor sysadmin en la instancia del motor de base de datos de SQL Server.  
  
-   Debe haber iniciado sesión como miembro del grupo Administradores en el equipo donde se ejecuta DQSInstaller.exe.  
  
##  <a name="run-dqsinstallerexe-from-start-screen-start-menu-or-windows-explorer"></a><a name="WindowsExplorer"></a> Ejecutar DQSInstaller.exe desde la pantalla Inicio, el menú Inicio o el explorador de Windows  
  
1.  En el equipo en el que decidió instalar [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], ejecute el archivo DQSInstaller.exe mediante cualquiera de los procedimientos siguientes:  
  
    -   **Pantalla Inicio**: en la pantalla **Inicio** , haga clic en **Instalador de Data Quality Server**  
  
    -   **Menú Inicio**: en la barra de tareas, haga clic en **Inicio**, seleccione **Todos los programas** y haga clic en [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)]. En [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], haga clic en **Data Quality Services** y luego en **Instalador de Data Quality Server**.  
  
    -   **Explorador de Windows**: busque el archivo DQSInstaller.exe. Si instaló la instancia predeterminada de SQL Server, el archivo DQSInstaller.exe estará disponible en C:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn. Haga doble clic en el archivo DQSInstaller.exe.  
  
2.  Aparecerá una ventana de símbolo del sistema que mostrará el estado de la instalación. Observará estas tres cosas:  
  
    1.  El instalador crea un archivo de registro de instalación, DQS_install.log, que contiene información sobre las acciones realizadas en la ejecución del archivo DQSInstaller.exe. Si instaló la instancia predeterminada de SQL Server, el archivo DQS_install.log estará disponible en C:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Log.  
  
    2.  El instalador usa la intercalación predeterminada del servidor, SQL_Latin1_General_CP1_CI_AS, para instalar el [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)].  
  
        > [!IMPORTANT]  
        >  Solo podrá proporcionar un valor de intercalación de servidor diferente para instalar el [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] si va a ejecutar DQSInstaller.exe desde el símbolo del sistema. Para obtener más información, vea [Ejecutar DQSInstaller.exe desde el símbolo del sistema](#CommandPrompt) , más adelante en este tema.  
  
    3.  El instalador comprueba si hay reinicios pendientes en su equipo debido a cualquier actualización que se haya instalado recientemente en el equipo. Si se encuentra algún reinicio pendiente, aparecerá un mensaje de notificación y podrá elegir continuar o anular la instalación presionando Y o N respectivamente. Se recomienda que si hay reinicios pendientes, anule la instalación, reinicie el equipo y, a continuación, ejecute DQSInstaller.exe de nuevo.  
  
3.  Se le pedirá que escriba una contraseña para la clave maestra de base de datos. La clave maestra de base de datos es necesaria para cifrar las claves del proveedor de servicios de datos de referencia que se almacenarán en la base de datos DQS_MAIN cuando se configuran los proveedores de datos de referencia en el [!INCLUDE[ssDQSnoversion](../../includes/ssdqsnoversion-md.md)] (DQS) más adelante.  
  
    > [!IMPORTANT]  
    >  La contraseña debe tener al menos 8 caracteres y debe contener caracteres de tres de las cuatro categorías siguientes: letra mayúscula de inglés (A, B, C,... Z), letra minúscula de inglés (a, b, c,... z), numeral (0, 1,2,... 9) y caracteres no alfanuméricos o especiales (~! @ # $% ^& * () _-+ = | \\ {} []:;"' <>,.? /). Por ejemplo: P@ssword. El instalador le solicitará que escriba otra contraseña si la contraseña actual no satisface el requisito.  
  
4.  Proporcione una contraseña, confírmela y, a continuación, presione ENTRAR para continuar con la instalación.  
  
    > [!IMPORTANT]  
    >  Debe conservar la contraseña especificada para la clave maestra de base de datos porque la necesitará cuando restaure las bases de datos de DQS a partir de una copia de seguridad en el futuro, si elige hacerlo. Para obtener más información acerca de cómo restaurar bases de datos de DQS, vea [Backing Up and Restoring DQS Databases](../../data-quality-services/backing-up-and-restoring-dqs-databases.md).  
  
5.  Si una base de datos de Master Data Services se encuentra en la misma instancia de SQL Server que el [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)], el instalador crea un usuario asignado al inicio de sesión de Master Data Services y le concede el rol dqs_administrator en la base de datos DQS_MAIN. Para obtener información acerca de cómo instalar Master Data Services y crear una base de datos de Master Data Services, vea [Install Master Data Services](../../master-data-services/install-windows/install-master-data-services.md).  
  
6.  Se mostrará un mensaje de finalización cuando la instalación se haya completado correctamente. Presione cualquier tecla para cerrar la ventana de símbolo del sistema.  
  
##  <a name="run-dqsinstallerexe-from-command-prompt"></a><a name="CommandPrompt"></a> Ejecutar DQSInstaller.exe desde el símbolo del sistema  
 Puede ejecutar DQSInstaller.exe desde el símbolo del sistema mediante los siguientes parámetros de línea de comandos:  
  
|Parámetro DQSInstaller.exe|Descripción|Sintaxis de ejemplo|  
|--------------------------------|-----------------|-------------------|  
|-collation|La intercalación de servidor que se usará para instalar el [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)].<br /><br /> DQS solo admite la intercalación sin distinción entre mayúsculas y minúsculas. Si especifica una intercalación con distinción entre mayúsculas y minúsculas, el instalador intenta utilizar la versión de la intercalación especificada sin distinción entre mayúsculas y minúsculas. Si no hay ninguna versión sin distinción entre mayúsculas y minúsculas, o si la intercalación no es compatible con SQL, la instalación del [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] producirá un error.<br /><br /> Si no se especifica una intercalación de servidor, se utiliza la intercalación predeterminada, SQL_Latin1_General_CP1_CI_AS.|`dqsinstaller.exe -collation <collation_name>`|  
|-upgradedlls|Omite la reconstrucción de las bases de datos DQS (DQS_MAIN, DQS_PROJECTS y DQS_STAGING_DATA) y solo actualiza los ensamblados de Common Language Runtime de SQL (SQLCLR) utilizados por DQS en la base de datos de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .<br /><br /> Para obtener más información, vea [Actualizar ensamblados de SQLCLR después de actualizar .NET Framework](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md).|`dqsinstaller.exe -upgradedlls`|  
|-exportkbs|Exporta todas las bases de conocimiento a un archivo de copia de seguridad de DQS (.dqsb). También debe especificar la ruta de acceso completa y el nombre de archivo donde desea exportar todas las bases de conocimiento.<br /><br /> Para obtener más información, vea [Exportar e importar bases de conocimiento de DQS mediante DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).|`dqsinstaller.exe -exportkbs <path><filename>`<br /><br /> Por ejemplo: `dqsinstaller.exe -exportkbs c:\DQSBackup.dqsb`|  
|-importkbs|Importe todas las bases de conocimiento desde un archivo de copia de seguridad de DQS (.dqsb) después de completar la instalación de [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] . También debe especificar la ruta de acceso completa y el nombre de archivo desde el que desea importar todas las bases de conocimiento.<br /><br /> Para obtener más información, vea [Exportar e importar bases de conocimiento de DQS mediante DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).|`dqsinstaller.exe -importkbs <path><filename>`<br /><br /> Por ejemplo: `dqsinstaller.exe -importkbs c:\DQSBackup.dqsb`|  
|-upgrade|Actualiza el esquema de bases de datos DQS. Debe usar este parámetro después de instalar una actualización de SQL Server en una instancia configurada previamente de DQS. Para obtener más información, vea [Upgrade DQS Databases Schema After Installing SQL Server Update](../../data-quality-services/install-windows/upgrade-dqs-databases-schema-after-installing-sql-server-update.md).|`dqsinstaller.exe -upgrade`|  
|-uninstall|Desinstala [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] de la instancia de SQL Server actual.<br /><br /> También puede exportar todas las bases de conocimiento en la instalación del servidor de calidad de datos existente a un archivo de copia de seguridad de DQS (.dqsb) y, a continuación, desinstalar el servidor de calidad de datos. Para obtener más información, vea [Exportar e importar bases de conocimiento de DQS mediante DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md).<br /><br /> Importante: Si desinstala desde una instancia de SQL Server mediante el parámetro de línea **\* de comandos, todos \* los objetos de DQS se eliminarán como parte del proceso de desinstalación. \* \*** [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] `-uninstall` No es necesario eliminarlos manualmente después de desinstalar [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] como se ha mencionado en [Quitar objetos del servidor de calidad de datos](../../sql-server/install/remove-data-quality-server-objects.md).|**Para desinstalar solo el servidor de calidad de datos:**<br /><br /> `dqsinstaller.exe -uninstall`<br /><br /> **Para exportar todas las bases de conocimiento a un archivo y, a continuación, desinstalar el servidor de calidad de datos:**<br /><br /> `dqsinstaller.exe -uninstall <path><filename>`<br /><br /> Por ejemplo: `dqsinstaller.exe -uninstall c:\DQSBackup.dqsb`|  
  
 **Para ejecutar DQSInstaller.exe desde el símbolo del sistema:**  
  
1.  Inicie el símbolo del sistema.  
  
2.  En el símbolo del sistema, cambie el directorio a la ubicación donde DQSInstaller.exe esté disponible. Si instaló la instancia predeterminada de SQL Server, el archivo DQSInstaller.exe estará disponible en C:\Archivos de programa\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn:  
  
    ```  
    cd C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn  
    ```  
  
3.  En el símbolo del sistema, ejecute DQSInstaller.exe con o sin parámetros de línea de comandos:  
  
    -   **Sin el parámetro de línea de comandos**: escriba `dqsinstaller.exe`y pulse ENTRAR.  
  
    -   **Con el parámetro de línea de comandos**: escriba el comando necesario según se indica en la tabla anterior y pulse ENTRAR.  
  
4.  Se realizarán las acciones necesarias en función del comando especificado. Si ha optado por instalar el [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] sin ningún parámetro de línea de comandos, todos los demás pasos son los mismos que los descritos en los pasos 2 a 6 de la sección anterior, [Ejecutar DQSInstaller.exe desde la pantalla Inicio, el menú Inicio o el Explorador de Windows](../../data-quality-services/install-windows/run-dqsinstaller-exe-to-complete-data-quality-server-installation.md#WindowsExplorer).  
  
## <a name="next-steps"></a>Pasos siguientes  
  
-   Conceda a los usuarios los roles adecuados para DQS en función de su perfil del trabajo. Vea [Conceder roles de DQS a los usuarios](../../data-quality-services/install-windows/grant-dqs-roles-to-users.md).  
  
-   Si se va obtener acceso al [!INCLUDE[ssDQSServer](../../includes/ssdqsserver-md.md)] de forma remota desde un [!INCLUDE[ssDQSClient](../../includes/ssdqsclient-md.md)], habilite el protocolo TCP/IP mediante el Administrador de configuración de SQL Server en este equipo.  
  
-   Asegúrese de que puede tener acceso a los datos de origen para las operaciones de DQS y que puede exportar los datos procesados a una tabla de una base de datos. Vea [Acceso a datos para las operaciones de DQS](../../data-quality-services/install-windows/access-data-for-the-dqs-operations.md).  
  
## <a name="see-also"></a>Consulte también  
 [Instalar Data Quality Services](../../data-quality-services/install-windows/install-data-quality-services.md)   
 [Actualizar ensamblados de SQLCLR después de .NET Framework Update](../../data-quality-services/install-windows/upgrade-sqlclr-assemblies-after-net-framework-update.md)   
 [Exportar e importar bases de conocimiento de DQS mediante DQSInstaller.exe](../../data-quality-services/install-windows/export-and-import-dqs-knowledge-bases-using-dqsinstaller-exe.md)  
  
  
