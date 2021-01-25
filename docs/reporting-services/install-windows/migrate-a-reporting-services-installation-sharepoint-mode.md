---
description: Migrar una instalación de Reporting Services (modo de SharePoint)
title: Migrar una instalación de Reporting Services (modo de SharePoint) | Microsoft Docs
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 61290949-690a-4e19-b078-57c99b6b30fa
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 9281c38d56b85cda96b9a5f9888ff707bc4e139a
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596216"
---
# <a name="migrate-a-reporting-services-installation-sharepoint-mode"></a>Migrar una instalación de Reporting Services (modo de SharePoint)

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE [ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  Este tema contiene información general de los pasos necesarios para migrar una implementación del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] desde un entorno de SharePoint a otro. Los pasos concretos pueden ser distintos dependiendo de la versión desde la que esté migrando. Para obtener más información sobre los escenarios de actualización y migración para el modo de SharePoint, vea [Upgrade and Migrate Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md). Si solo quiere copiar los elementos de informe de un servidor a otro, vea [Sample Reporting Services rs.exe Script to Copy Content between Report Servers](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md) (Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes).  
  
 Para obtener información sobre cómo migrar una implementación en modo nativo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vea [Migrar una instalación de Reporting Services &#40;modo nativo&#41;](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
 Un motivo frecuente para completar una migración es cuando se desea actualizar la implementación de SharePoint 2010 a SharePoint 2013/2016. SharePoint 2013/2016 no admite la actualización en contexto desde SharePoint 2010 y se debe completar el procedimiento de **actualizar y adjuntar la base de datos** o realizar una migración solo de contenido.  
  
 Para más información acerca de la actualización de SharePoint 2013/2016, consulte lo siguiente:  

-   [Información general del proceso de actualización a SharePoint 2016](https://technet.microsoft.com/library/cc262483\(v=office.16\)).

-   [Información general del proceso de actualización a SharePoint 2013](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Limpiar un entorno antes de realizar una actualización a SharePoint 2013](/SharePoint/upgrade-and-update/clean-up-an-environment-before-an-upgrade-to-sharepoint-2013).  
  
-   [Actualizar bases de datos de SharePoint 2013 a SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\)).

-   [Actualizar bases de datos de SharePoint 2010 a SharePoint 2013](/SharePoint/upgrade-and-update/upgrade-content-databases-from-sharepoint-2010-to-sharepoint-2013).
  
-   [Mover bases de datos de contenido en SharePoint 2016](https://technet.microsoft.com/library/cc262792\(v=office.16\).aspx).

-   [Mover bases de datos de contenido en SharePoint 2013](/SharePoint/administration/move-content-databases).
  
##  <a name="migrate-from-reporting-services-sharepoint-mode-versions-prior-to-sql-server-2012"></a><a name="bkmk_prior_versions"></a> Migrar desde versiones anteriores a SQL Server 2012 del modo de SharePoint de Reporting Services  
 La arquitectura del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] cambió en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], incluido el esquema de la base de datos de aplicación de servicio. Si quiere migrar al modo de SharePoint de [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] desde una versión anterior a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], cree primero el nuevo entorno de SharePoint instalando SharePoint y el modo de SharePoint de SQL Server 2016 Reporting Services. Para obtener más información, vea [Instalar el modo de SharePoint de Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
 Una vez que el nuevo entorno de SharePoint está en ejecución, puede elegir entre realizar una migración solo de contenido o una migración completa en el nivel de base de datos que incluya las bases de datos de contenido.  
  
###  <a name="content-only-migration"></a><a name="bkmk_content_only_migration"></a> Migrar solo contenido  
 **Migración solo de contenido de Reporting Services** : si desea copiar el contenido de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a una nueva granja, necesita usar herramientas como **rs.exe** para copiar el contenido a la nueva instalación de SharePoint. Para obtener más información acerca de las migraciones solo de contenido, vea lo siguiente:  
  
-   **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Scripts de RSS:** Los scripts pueden migrar contenido y recursos entre servidores de informes en modo nativo y en modo de SharePoint. Para obtener más información, vea [Script rs.exe de ejemplo de Reporting Services para copiar contenido entre servidores de informes](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md) y [Reporting Services RS.exe script migrates content](https://azuresql.codeplex.com/releases/view/115207)(Script RS.exe de Reporting Services que migra contenido).  
  
-   **Herramienta de migración de Reporting Services:** esta heramienta puede copiar elementos de informe de un servidor en modo nativo a un servidor en modo de SharePoint. Para más información, consulte la página de la [herramienta de migración de Reporting Services](https://www.microsoft.com/download/details.aspx?id=29560) (https://www.microsoft.com/download/details.aspx?id=29560).  
  
###  <a name="full-migration"></a><a name="bkmk_full_migration"></a> Migración completa  
 **Migración completa** : si va a migrar bases de datos de contenido de SharePoint junto con las bases de datos del catálogo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a una nueva granja, puede seguir una serie de opciones de copia de seguridad y restauración que se resumen en este tema. En algunos casos necesitará emplear otra herramienta diferente para la fase de restauración que la que usó para la fase de copia de seguridad. Por ejemplo, puede usar el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para hacer una copia de seguridad de las claves de cifrado de una versión anterior de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], pero necesita usar Administración central de SharePoint o PowerShell para restaurar las claves de cifrado a una instalación en modo de SharePoint de SQL Server 2016 Reporting Services.  
  
####  <a name="databases-you-will-see-in-the-completed-migration"></a><a name="bkmk_databases"></a> Bases de datos que verá cuando se complete la migración  
 En la tabla siguiente se describen las bases de datos de SQL Server relacionadas con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que tendrá una vez que haya migrado correctamente la instalación en modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
|Base de datos|Nombre del ejemplo|Notas|  
|--------------|------------------|-|  
|Base de datos del catálogo|ReportingService_[GUID de la aplicación de servicio] **(&#42;)**|Migraciones de usuario.|  
|Base de datos temp|ReportingService_[GUID de la aplicación de servicio]TempDB **(&#42;)**|Migraciones de usuario.|  
|Base de datos de alertas|ReportingService_[GUID de la aplicación de servicio]_Alerting|Se crea cuando se crea una aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|  
  
 **(&#42;)** Los nombres de ejemplo que se muestran en la tabla siguen la convención de nomenclatura que SSRS usa al crear una aplicación de servicio de SSRS. Si está migrando desde un servidor diferente, el catálogo y las bases de datos tempDB tendrán los nombres de la instalación original.  
  
####  <a name="backup-operations"></a><a name="bkmk_backup_operations"></a> Operaciones de copia de seguridad  
 En esta sección se describen los tipos de información que necesita migrar y las herramientas o los procesos que se usan para completar la copia de seguridad.  
  
 ![Diagrama básico de SSRS SharePoint Migration](../../reporting-services/install-windows/media/rs-sharepoint-migration.gif "Diagrama básico de SSRS SharePoint Migration")  
  
|Elemento|Objetos|Método|Notas|  
|-|-------------|------------|-----------|  
|**1**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|**Rskeymgmt.exe** o Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vea [Hacer copia de seguridad y restaurar claves de cifrado de Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).|Las herramientas indicadas se pueden usar para la copia de seguridad, pero para la operación de restauración usará las páginas de administración de la aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o PowerShell.|  
|**2**|Bases de datos de contenido de SharePoint.||Haga una copia de seguridad de la base de datos y sepárela.<br /><br /> Vea la sección "Actualizar y adjuntar una base de datos" en [Determinación del enfoque de actualización (SharePoint Server 2010) (/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013).|  
|**3**|La base de datos de SQL Server que es la base de datos del catálogo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Hacer copia de seguridad y restaurar bases de datos de SQL Server<br /><br /> o<br /><br /> Adjuntar y separar bases de datos de SQL Server.||  
|**4**|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Copia de archivos simple.|Solo necesita copiar rsreportserver.config si ha realizado personalizaciones en el archivo. Ejemplo de la ubicación predeterminada de los archivos: C:\Archivos de programa\Archivos comunes\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting\\*:<br /><br /> <br /><br /> RSReportServer.config<br /><br /> Rssvrpolicy.config<br /><br /> Web.config para la aplicación ASP.NET del servidor de informes.<br /><br /> Machine.config para ASP.NET.|  
  
####  <a name="restore-operations"></a><a name="bkmk_restore_operations"></a> Operaciones de restauración  
 En esta sección se describen los tipos de información que necesita migrar y las herramientas o los procesos que se usan para completar la restauración. Las herramientas que use para la restauración pueden ser diferentes de las empleadas para la copia de seguridad.  
  
 Antes de completar los pasos de restauración, debe instalar y configurar la nueva granja de SharePoint y el modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para obtener más información sobre la instalación básica del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vea [Instalar el modo de SharePoint de Reporting Services](../../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md).  
  
|Elemento|Objetos|Método|Notas|  
|-|-------------|------------|-----------|  
|**1**|Restaurar bases de datos de contenido de SharePoint en la nueva granja.|Método "Actualización de base de datos adjunta" de SharePoint.|Pasos básicos:<br /><br /> 1) Restaure la base de datos en el nuevo servidor.<br /><br /> 2) Adjunte la base de datos de contenido a una aplicación Web indicando la dirección URL.<br /><br /> 3) Get-SPWebapplication muestra todas las aplicaciones web y las direcciones URL.<br /><br /> <br /><br /> Vea la sección "Actualizar y adjuntar una base de datos" en [Determinación del enfoque de actualización (SharePoint Server 2010) (/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013)](/SharePoint/upgrade-and-update/determine-strategy-for-upgrade-to-sharepoint-2013) y [Adjuntar bases de datos y actualización a SharePoint Server 2010 (/SharePoint/upgrade-and-update/upgrade-content-databases)](/SharePoint/upgrade-and-update/upgrade-content-databases).|  
|**2**|Restaure la base de datos de SQL Server que sea la base de datos del catálogo de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (servidor de informes).|Copia de seguridad y restauración de bases de datos de SQL.<br /><br /> **or**<br /><br /> Adjuntar y separar bases de datos de SQL Server.|La primera vez que se use la base de datos, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] actualizará el esquema de la base de datos según sea necesario para que funcione con el entorno de SQL Server 2016.|  
|**3**|Cree una nueva aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Cree una nueva aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Al crear la nueva aplicación de servicio, configúrela para usar la base de datos del servidor de informes que copió.<br /><br /> Para obtener más información sobre el uso de Administración central de SharePoint, vea la sección "Paso 3: Crear una aplicación de servicio de Reporting Services" en [Instalación del primer servidor de informes en modo de SharePoint](../../reporting-services/install-windows/install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Para obtener ejemplos de uso de PowerShell, vea la sección "Para crear una aplicación de servicio de Reporting Services mediante PowerShell" en [Aplicaciones de servicio y servicio de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/reporting-services-sharepoint-service-and-service-applications.md).|  
|**4**|Restaure los archivos de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .|Copia de archivos simple.|Ejemplo de la ubicación predeterminada de los archivos: C:\Archivos de programa\Archivos comunes\Microsoft Shared\Web Server Extensions\15\WebServices\Reporting.|  
|**5**|Restaure las claves de cifrado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].|Restaure el archivo de copia de seguridad de claves mediante la página "Configuración del sistema" de la aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].<br /><br /> **or**<br /><br /> PowerShell.|Vea la sección "Administración de claves" del tema [Administrar una aplicación de servicio de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).|   
  
##  <a name="migrate-from-a-sql-server-2012-or-sql-server-2014-deployment"></a><a name="bkmk_migrate_from_ctp"></a> Migrar desde una implementación de SQL Server 2012 o SQL Server 2014  
 En una granja con varios servidores, los usuarios probablemente tendrán que las bases de datos de contenido y del catálogo en equipos diferentes, en cuyo caso solo se necesita agregar a la granja de SharePoint un nuevo servidor con el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] instalado y después quitar el servidor antiguo. No debe ser necesario copiar ninguna base de datos.  
  
### <a name="backup-operations"></a>Operaciones de copia de seguridad  
  
1.  Realice una copia de seguridad de las claves de cifrado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
2.  Haga una copia de seguridad de la aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en Administración central de SharePoint (o use PowerShell). Esto también hará una copia de seguridad de las bases de datos de la aplicación de servicio de SharePoint. Vea el tema  [Backup and Restore Reporting Services SharePoint Service Applications](../../reporting-services/report-server-sharepoint/backup-and-restore-reporting-services-sharepoint-service-applications.md)(Aplicaciones de servicio de SharePoint de Reporting Services de copia de seguridad y restauración).  
  
3.  Si tiene una cuenta de ejecución desatendida (UEA) y autenticación de Windows, anote las credenciales para poder usarlas en el proceso de restauración.  
  
4.  Para obtener más información, vea [Realizar copias de seguridad de aplicaciones de servicio en SharePoint 2013](/SharePoint/administration/back-up-a-service-application).  
  
### <a name="restore-operations"></a>Operaciones de restauración  
  
1.  Restaure la aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] mediante Administración central de SharePoint. También podría emplear PowerShell.  
  
2.  Restaure las claves de cifrado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
     Vea la sección "Administración de claves" del tema [Administrar una aplicación de servicio de SharePoint de Reporting Services](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md).  
  
3.  Configure la UEA y las credenciales de Windows en la aplicación de servicio.  
  
4.  Para obtener más información, vea [Restaurar las aplicaciones de servicio en SharePoint 2013](/SharePoint/administration/restore-a-service-application).  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> Recursos adicionales  
  
-   [Introducción a las actualizaciones a SharePoint 2013 (/SharePoint/upgrade-and-update/get-started-with-upgrade)](/SharePoint/upgrade-and-update/get-started-with-upgrade).  
  
-   [Información general del proceso de actualización a SharePoint 2013 (/SharePoint/upgrade-and-update/overview-of-the-upgrade-process)](/SharePoint/upgrade-and-update/overview-of-the-upgrade-process).  

## <a name="next-steps"></a>Pasos siguientes

[Actualizar y migrar Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
[Migrar una instalación de Reporting Services](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).