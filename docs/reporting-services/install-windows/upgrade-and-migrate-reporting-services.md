---
description: Upgrade and Migrate Reporting Services
title: Actualizar y migrar Reporting Services | Microsoft Docs
ms.prod: reporting-services
ms.prod_service: reporting-services-native
helpviewer_keywords:
- SSRS, upgrading
- Reporting Services, upgrades
- SQL Server Reporting Services, upgrading
- upgrading Reporting Services
author: maggiesMSFT
ms.author: maggies
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: 2515a93b025392735135cd13cbd734975f8ed221
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596832"
---
# <a name="upgrade-and-migrate-reporting-services"></a>Upgrade and Migrate Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  En este tema se proporciona información general sobre las opciones de actualización y migración de SQL Server Reporting Services. Hay dos enfoques generales para actualizar una implementación de SQL Server Reporting Services:  
 
- **Actualizar *a* Reporting Services 2016 y versiones anteriores *desde* Reporting Services 2016 y versiones anteriores:** se actualizan los componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en los servidores y las instancias donde están instalados actualmente. Habitualmente se denomina actualización "en contexto". No se admite la actualización en contexto de un modo de servidor de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a otro. Por ejemplo, no puede actualizar un servidor de informes en modo nativo a un servidor de informes en modo de SharePoint. Puede migrar los elementos de informe de un modo a otro. Para más información, vea la sección [Escenarios de actualización y migración en modo de SharePoint](#bkmk_sharePoint_scenarios) más adelante en este documento.  

- **Actualizar *a* Reporting Services 2017 y versiones posteriores *desde* Reporting Services 2016 y versiones anteriores** no es el mismo escenario de actualización que en las versiones anteriores. Al actualizar *a* Reporting Services 2016 y versiones anteriores, se podía seguir un proceso de actualización local con el soporte de instalación de SQL Server. Al actualizar *a* Reporting Services 2017 y versiones posteriores *desde* Reporting Services 2016 y versiones anteriores, no se pueden seguir los mismos pasos porque la nueva instalación de Reporting Services es un producto independiente. Ya no forma parte del soporte de instalación de SQL Server. 

    Para actualizar desde Reporting Services 2016 y versiones anteriores a Reporting Services 2017 y versiones posteriores, siga el artículo [Migración de una instalación de Reporting Services (modo nativo)](migrate-a-reporting-services-installation-native-mode.md) con Reporting Services 2017 o versiones posteriores como instancia de destino. 

- **Actualizar *desde* Reporting Services 2017 a versiones futuras** es de nuevo un escenario de actualización local, ya que los GUID de instalación del producto son los mismos. Ejecute el archivo de instalación SQLServerReportingServices.exe para iniciar la actualización local en el servidor donde Reporting Services está instalado actualmente.
  
- **Migración**: se instala y configura un nuevo entorno de SharePoint, se copian los elementos de informe y los recursos al nuevo entorno, y se configura el nuevo entorno para usar contenido existente. Una forma de nivel inferior de migración es copiar bases de datos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , los archivos de configuración y, si se usa el modo de SharePoint, las bases de datos de contenido de SharePoint.  


> [!NOTE]
> La integración de Reporting Services con SharePoint no está disponible a partir de SQL Server 2016.
   
##  <a name="known-upgrade-issues-and-best-practices"></a><a name="bkmk_known_issues"></a> Problemas conocidos y prácticas recomendadas de actualización  
 Para obtener una lista detallada de las ediciones y las versiones admitidas que puede actualizar, vea [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md).  
  
> [!TIP]  
>  Para conocer la información más reciente sobre problemas con SQL Server, vea [Notas de la versión de SQL Server 2016](../../sql-server/sql-server-2016-release-notes.md).  
  
  
##  <a name="side-by-side-installations"></a><a name="bkmk_side_by_side"></a> Instalaciones en paralelo  
 El modo nativo de SQL Server Reporting Services se puede instalar en paralelo con una implementación de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] en modo nativo.  
  
 No se admiten las implementaciones en paralelo de SQL Server Reporting Services en el modo de SharePoint ni ninguna de las versiones anteriores de los componentes del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
  
##  <a name="in-place-upgrade"></a><a name="bkmk_inplace_upgrade"></a> Actualización en contexto  
 El programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ocupa de realizar la actualización. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se puede usar para actualizar algunos o todos los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , incluido [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. El programa de instalación detecta las instancias existentes y solicita que se actualice. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El programa de instalación proporciona opciones de actualización que puede especificar como argumentos de la línea de comandos o en el Asistente para la instalación.  
  
 Al ejecutar el programa de instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede seleccionar la opción para actualizar desde una de las versiones siguientes o puede instalar una nueva instancia de SQL Server Reporting Services que se ejecute en paralelo con las instalaciones existentes:  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 Para obtener más información sobre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea lo siguiente  

* [Actualizar a SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)
* [Actualización a SQL Server 2016 mediante el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [Instalar SQL Server 2016 desde el símbolo del sistema](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)
  
  
##  <a name="pre-upgrade-checklist"></a><a name="bkmk_upgrade_checklist"></a> Lista de comprobación previa a la actualización  
 Antes de actualizar a SQL Server Reporting Services, revise lo siguiente:  
  
-   Revise los requisitos para determinar si el hardware y el software pueden admitir [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]. Para más información, vea [Requisitos de hardware y software para instalar SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  
  
-   Use el Comprobador de configuración del sistema (SCC) para examinar el equipo del servidor de informes en busca de cualquier condición que pudiera evitar la instalación correcta de SQL Server Reporting Services. Para obtener más información, vea [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md).  
  
-   Revise las prácticas recomendadas de seguridad y orientación para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
-   Haga una copia de seguridad de la clave simétrica. Para obtener más información, vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
-   Haga copia de seguridad de las bases de datos del servidor de informes y de los archivos de configuración. Para obtener más información, vea [Backup and Restore Operations for Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md).  
  
-   Haga copia de seguridad de las personalizaciones de los directorios virtuales de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] existentes en IIS.  
  
-   Quite los certificados TLS/SSL no válidos.  Se trata de los certificados expirados y que no piensa actualizar antes de actualizar Reporting Services.  Los certificados no válidos provocarán un error de actualización y la aparición de un mensaje de error similar al siguiente en el archivo de registro de Reporting Services: **Microsoft.ReportingServices.WmiProvider.WMIProviderException: No se configuró ningún certificado SSL (Capa de sockets seguros) en el sitio web.** .  
  
 Antes de actualizar un entorno de producción, ejecute siempre una actualización de prueba en un entorno de preproducción que tenga la misma configuración que el entorno de producción.  
  
  
## <a name="overview-of-migration-scenarios"></a>Información general de los escenarios de migración  
 Si va a realizar una actualización desde una versión admitida de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], normalmente puede ejecutar el Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el fin de actualizar los archivos de programa, la base de datos y todos los datos de aplicación del servidor de informes.  
  
 Sin embargo, es necesario **migrar** manualmente la instalación de un servidor de informes si se da alguna de las condiciones siguientes:  
  
-   Desea cambiar el tipo de servidor de informes usado en la implementación. Por ejemplo, no puede actualizar o convertir un servidor de informes en modo nativo al modo de SharePoint. Para más información, vea [Native to SharePoint Migration &#40;SSRS&#41;](../../reporting-services/install-windows/native-to-sharepoint-migration-ssrs.md) (Migración nativa a SharePoint &#40;SSRS&#41;).  
  
-   Desea minimizar la cantidad de tiempo que el servidor de informes está sin conexión durante el proceso de actualización. La instalación actual permanece en línea mientras copia datos de contenido en una instancia del servidor de informes y prueba la instalación sin cambiar el estado de la instalación del servidor de informes existente.  
  
-   Es recomendable migrar una implementación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SharePoint 2010 a SharePoint 2013/2016. SharePoint 2013/2016 no admite la actualización en contexto desde SharePoint 2010. Para obtener más información, vea [Migrar una instalación de Reporting Services &#40;modo de SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
  
##  <a name="native-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_native_scenarios"></a> Escenarios de actualización y migración en modo nativo  
 **Actualización:** la actualización en contexto para el modo nativo es el mismo proceso para cada una de las versiones admitidas mencionadas anteriormente en este tema. Ejecute el Asistente para la instalación de SQL Server o una instalación desde la línea de comandos. Después de la instalación, la base de datos del servidor de informes se actualizará automáticamente al nuevo esquema de la base de datos del servidor de informes. Para obtener más información, vea la sección [Actualización en contexto](#bkmk_inplace_upgrade) de este tema.  
  
 El proceso de actualización comienza al seleccionar una instancia del servidor de informes existente para actualizar.  
  
1.  Si la base de datos del servidor de informes está en un equipo remoto y no dispone del permiso para actualizarla, el programa de instalación le solicita que proporcione las credenciales para actualizar a una base de datos del servidor de informes remota. Asegúrese de proporcionar las credenciales que tiene **sysadmin** o los permisos de actualización de base de datos.  
  
2.  El programa de instalación comprueba la configuración o las condiciones que impiden la actualización y lee los valores de configuración. Algunos ejemplos pueden ser las extensiones personalizadas que se implementan en el servidor de informes. Si la actualización permanece bloqueada, debe modificar la instalación para que deje de estarlo o migrar a una nueva instancia de SQL Server Reporting Services. Para obtener más información, vea la documentación del Asesor de actualizaciones.  
  
3.  Si la actualización puede continuar, el programa de instalación le solicita que continúe con el proceso de actualización.  
  
4.  El programa de instalación crea carpetas para los archivos del programa SQL Server Reporting Services. Las carpetas de programas para una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] incluyen MSRS13.\<*instance name*>.  
  
5.  El programa de instalación agrega los archivos de programa del servidor de informes de SQL Server Reporting Services, las herramientas de configuración y las utilidades de línea de comandos que forman parte de la característica del servidor de informes.  
  
    1.  Los archivos de programa de la versión anterior se quitan.  
  
    2.  Entre las has herramientas de configuración del servidor de informes y las utilidades que se actualizan a la nueva versión se incluyen la herramienta Configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo nativo, utilidades de la línea de comandos como RS.exe y el Generador de informes.  
  
    3.  Otras herramientas cliente como [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] son una descarga independiente y se deben actualizar por separado. Para más información, consulte [Descargar SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md).
  
    4.  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] es una descarga independiente. Para más información, consulte [SQL Server Data Tools en Visual Studio 2015](/previous-versions/mt186501(v=msdn.10)).  
  
6.  El programa de instalación reutiliza la entrada de servicio del Administrador de control de servicios para el servicio Servidor de informes de SQL Server Reporting Services. Esta entrada de servicio incluye la cuenta de servicio de Windows Servidor de informes.  
  
7.  El programa de instalación reserva direcciones URL nuevas de acuerdo con la configuración actual del directorio de IIS. Es posible que el programa de instalación no quite los directorios virtuales en IIS, de modo que tiene que asegurarse de quitarlos manualmente una vez finalizada la actualización.  
  
8.  El programa de instalación combina los valores de los archivos de configuración. Utilizando como base los archivos de configuración de instalación de la instalación actual, se agregan las nuevas entradas. No se quitan las entradas desusadas, pero el servidor de informes dejará de leerlas una vez finalizada la actualización. La actualización no eliminará los archivos de registro anteriores, el archivo RSWebApplication.config obsoleto o la configuración de directorios virtuales en IIS. La actualización no quitará el Diseñador de informes anterior, Management Studio u otras herramientas de cliente. Si ya no los necesita, asegúrese de quitar estos archivos y herramientas una vez finalizada la actualización.  
  
 **Migración:** la migración de una versión anterior de una instalación en modo nativo a SQL Server Reporting Services consta de los mismos pasos para todas las versiones admitidas enumeradas anteriormente en este tema. Para obtener más información, vea [Migrar una instalación de Reporting Services &#40;modo nativo&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
  
##  <a name="upgrade-a-reporting-services-native-mode-scale-out-deployment"></a><a name="bkmk_native_scaleout"></a> Actualizar una implementación escalada horizontalmente de Reporting Services en modo nativo  
 A continuación se muestra un resumen de cómo actualizar una implementación en modo nativo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] escalada horizontalmente a más de un servidor de informes. Este proceso requiere un tiempo de inactividad de la implementación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
1.  Haga copia de seguridad de las claves de cifrado y las bases de datos del servidor de informes. Para más información, vea [Operaciones de copia de seguridad y restauración de Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md) y [Agregar y quitar claves de cifrado para implementaciones escaladas &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md).  
  
2.  Use el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y quite todos los servidores de informes de la implementación escalada. Para más información, vea [Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
3.  Actualice uno de los servidores de informes a SQL Server Reporting Services.  
  
4.  Use el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para volver a agregar los servidores de informes a la implementación escalada. Para más información, vea [Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
     Para cada servidor, repita los pasos de actualización y escalado horizontal.  
  
##  <a name="sharepoint-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_sharePoint_scenarios"></a> Escenarios de actualización y migración en modo de SharePoint  
 En las próximas secciones se describen los problemas y los pasos básicos necesarios para actualizar o migrar desde versiones especificadas del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] al modo de SharePoint de SQL Server Reporting Services [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Hay dos componentes de instalación para actualizar una implementación del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
    > [!TIP]  
    >  Use el cmdlet [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de SharePoint de `Get-SPRSServiceApplicationServers` para determinar los servidores de la granja de servidores de SharePoint que están ejecutando actualmente el servicio compartido de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y, por tanto, requieren una actualización.  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Complemento para productos de SharePoint. Para obtener más información, vea [Instalar o desinstalar el complemento Reporting Services para SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md).  
  
 Para obtener los pasos detallados sobre cómo migrar una instalación en modo de SharePoint, vea [Migrar una instalación de Reporting Services &#40;modo de SharePoint&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md).  
  
> [!IMPORTANT]  
>  Algunos de los escenarios siguientes necesitan un tiempo de inactividad del entorno de SharePoint debido a las diversas tecnologías que hay que actualizar. Si su situación no permite que haya tiempo de inactividad, necesitará completar una migración en lugar de una actualización en contexto.  
  
### <a name="sssql14-to-sql-server-reporting-services"></a>De [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a SQL Server Reporting Services  
 **Entorno inicial:** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP1, SharePoint 2010 o SharePoint 2013.  
  
 **Entorno final:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.   
  
-   **SharePoint 2013/2016:** SharePoint 2013/2016 no admite la actualización en contexto desde SharePoint 2010. Pero se admite el procedimiento de **actualizar y adjuntar la base de datos**  .
  
     Si tiene una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] integrada con SharePoint 2010, no puede realizar una actualización en contexto del servidor de SharePoint. Sin embargo, puede migrar las bases de datos de contenido y las bases de datos de aplicación de servicio de la granja de SharePoint 2010 a una granja de SharePoint 2013/2016.  
  
### <a name="sssql11-to-sql-server-reporting-services"></a>De [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a SQL Server Reporting Services  
 **Entorno inicial:** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)] y SharePoint 2010.  
  
 **Entorno final:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.   
  
-   **SharePoint 2013/2016:** SharePoint 2013/2016 no admite la actualización en contexto desde SharePoint 2010. Pero se admite el procedimiento de **actualizar y adjuntar la base de datos**  .
  
     Si tiene una instalación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] integrada con SharePoint 2010, no puede realizar una actualización en contexto del servidor de SharePoint. Sin embargo, puede migrar las bases de datos de contenido y las bases de datos de aplicación de servicio de la granja de SharePoint 2010 a una granja de SharePoint 2013/2016.  
  
### <a name="sskilimanjaro-to-sql-server-reporting-services"></a>De [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] a SQL Server Reporting Services  
 **Entorno inicial:** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y SharePoint 2010.  
  
 **Entorno final:** SQL Server Reporting Services, SharePoint 2013 o SharePoint 2016.  
 
-   **SharePoint 2013/2016:** SharePoint 2013/2016 no admite la actualización en contexto desde SharePoint 2010. Pero se admite el procedimiento de **actualizar y adjuntar la base de datos**  .

    SharePoint se debe migrar antes de poder actualizar Reporting Services.
  
-   Instale la versión de SQL Server Reporting Services del complemento de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para SharePoint en cada servidor web front-end de la granja. Puede instalar el complemento mediante el Asistente para la instalación de SQL Server Reporting Services o descargándolo.  
  
-   Ejecute la instalación de SQL Server Reporting Services para actualizar el modo de SharePoint para cada "servidor de informes". El Asistente para la instalación de SQL Server instalará el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y creará una nueva aplicación de servicio. 
  
  
##  <a name="considerations-for-a-migration"></a><a name="bkmk_migration_considerations"></a> Consideraciones sobre una migración  
 Al mover los datos de la aplicación, debe ser consciente de los aspectos y restricciones siguientes:  
  
-   La protección de la clave de cifrado incluye un hash que incorpora la identidad del equipo.  
  
-   Los nombres de base de datos del servidor de informes son fijos y no se pueden cambiar en el equipo nuevo.  
  
### <a name="encryption-key-considerations"></a>Consideraciones sobre las claves de cifrado  
 Realice siempre una copia de seguridad de las claves de cifrado antes de mover una base de datos del servidor de informes a otro equipo.  
  
 Al mover una instalación del servidor de informes a otro equipo, invalidará el hash que protege las claves de cifrado que se usan para proteger los datos confidenciales almacenados en la base de datos del servidor de informes. Cada instancia del servidor de informes que utiliza la base de datos tiene su copia de la clave de cifrado, que se cifra con la identidad de la cuenta de servicio que está definida en el equipo actual. Si cambia los equipos, el servicio dejará de tener acceso a la clave, aunque utilice el mismo nombre de cuenta en el equipo nuevo.  
  
 Para volver a establecer el cifrado reversible en el nuevo equipo del servidor de informes, debe restaurar la clave de la que realizó anteriormente una copia de seguridad. El conjunto de claves completo que se almacena en la base de datos del servidor de informes está compuesto de un valor de clave simétrica más la información de identidad del servicio que se usa para restringir el acceso a la clave, para que solo pueda usarla la instancia del servidor de informes donde se almacenó. Durante la restauración de la clave, el servidor de informes reemplaza las copias existentes de la clave por las versiones nuevas. La versión nueva incluye los valores de identidad de servicio y de equipo definidos en el equipo actual. Para obtener más información, vea los temas siguientes:  
  
-   Modo SharePoint: consulte la sección "Administración de claves" de [Administrar una aplicación de servicio de SharePoint para Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  
  
-   Modo nativo: Consulte [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
  
### <a name="fixed-database-name"></a>Nombre fijo de la base de datos  
 No se puede cambiar el nombre de la base de datos del servidor de informes. La identidad de la base de datos se registra en los procedimientos almacenados del servidor de informes cuando se crea la base de datos. El cambio del nombre de las bases de datos temporales o principales del servidor de informes hará que se produzcan errores cuando se ejecuten los procedimientos, lo que invalida la instalación del servidor de informes.  
  
 Si el nombre de la base de datos de la instalación existente no es adecuado para la instalación nueva, plantéese la posibilidad de crear una base de datos nueva con el nombre que prefiera y, a continuación, cargar los datos de la aplicación existente mediante las técnicas siguientes:  
  
-   Escriba un script de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] que llame a los métodos SOAP del servicio web del servidor de informes para copiar datos entre las bases de datos. Para ejecutar el script, puede usar la utilidad RS.exe. Para obtener más información sobre este método, vea [Scripting and PowerShell with Reporting Services (Scripting y PowerShell con Reporting Services)](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md).  
  
-   Escriba código que llame al proveedor de WMI para copiar datos entre las bases de datos. Para obtener más información sobre este método, vea [Access the Reporting Services WMI Provider (Obtener acceso al proveedor de WMI de Reporting Services)](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md).  
  
-   Si solo tiene unos pocos elementos, puede volver a publicar los informes y los orígenes de datos compartidos del Diseñador de informes, del Diseñador de modelos y del Generador de informes en el nuevo servidor de informes. Debe volver a crear asignaciones de roles, suscripciones, programaciones compartidas, calendarios de instantáneas de informes, propiedades personalizadas que establezca en informes u otros elementos, seguridad de elementos de modelo y propiedades que establezca en el servidor de informes. Perderá el historial de informes y los datos del registro de ejecución de informes.  
  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Recursos adicionales  
  
> [!NOTE]  
>  Para obtener más información acerca de cómo actualizar y adjuntar la base de datos de SharePoint 2013, vea lo siguiente:  
  
-   [Información general del proceso de actualización a SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Información general del proceso de actualización a SharePoint 2013](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Limpiar un entorno antes de realizar una actualización a SharePoint 2013](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013).  
  
-   [Actualizar bases de datos de SharePoint 2013 a SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Actualizar bases de datos de SharePoint 2010 a SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013).  

## <a name="next-steps"></a>Pasos siguientes

[Actualizar informes](../../reporting-services/install-windows/upgrade-reports.md)   
[Actualización a SQL Server 2016 mediante el Asistente para instalación &#40;programa de instalación&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).