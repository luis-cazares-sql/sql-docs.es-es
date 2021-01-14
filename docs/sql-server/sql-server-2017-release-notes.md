---
title: Notas de la versión de SQL Server 2017 | Microsoft Docs
description: En este artículo se describen las limitaciones y los problemas de SQL Server 2017 y se proporcionan vínculos a información relacionada.
ms.custom: ''
ms.date: 11/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: conceptual
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-2017
ms.openlocfilehash: cd13f9fea3ade8f0bcb92e77470ab74f40d69ae2
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98167717"
---
# <a name="sql-server-2017-release-notes"></a>Notas de la versión de SQL Server 2017
[!INCLUDE[SQL Server 2017](../includes/applies-to-version/sqlserver2017.md)]
En este artículo se describen las limitaciones y los problemas de SQL Server 2017. Para obtener información relacionada, consulte:
- [Novedades de SQL Server 2017](../sql-server/what-s-new-in-sql-server-2017.md)
- [Notas de la versión de SQL Server en Linux](../linux/sql-server-linux-release-notes.md)
- [Actualizaciones acumulativas de SQL Server 2017](https://aka.ms/sql2017cu) para más información sobre la versión de la última actualización acumulativa (CU)

**Probar SQL Server**
- [![Descargar desde el Centro de evaluación](../includes/media/download2.png)](https://go.microsoft.com/fwlink/?LinkID=829477) [Descargar SQL Server 2017](https://go.microsoft.com/fwlink/?LinkID=829477)
- [![Creación de una máquina virtual](../includes/media/azure-vm.png)](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm) [Poner en marcha una máquina virtual con SQL Server 2017](https://azure.microsoft.com/services/virtual-machines/sql-server/?wt.mc_id=sqL16_vm)

> [!NOTE]
> Ya está disponible la versión preliminar de SQL Server 2019. Para más información, vea [Novedades de SQL Server 2019](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15&preserve-view=true).

## <a name="sql-server-2017---general-availability-release-october-2017"></a>SQL Server de 2017: versión de disponibilidad general (octubre de 2017)
### <a name="database-engine"></a>Motor de base de datos

- **Problema e impacto en el cliente:** después de la actualización, puede que el recurso compartido de red FILESTREAM ya no esté disponible.

- **Solución alternativa:** en primer lugar, reinicie el equipo y compruebe si el recurso compartido de red FILESTREAM está disponible. Si el recurso compartido aún no está disponible, realice los siguientes pasos:

    1. En el Administrador de configuración de SQL Server, haga clic con el botón derecho en la instancia de SQL Server y haga clic en **Propiedades**. 
    2. En la pestaña **FILESTREAM**, desactive **Habilitar FILESTREAM para el acceso de transmisión por secuencias de E/S de archivos** y después haga clic en **Aplicar**.
    3. Seleccione **Habilitar FILESTREAM para el acceso de transmisión por secuencias de E/S de archivos** de nuevo con el nombre del recurso compartido original y haga clic en **Aplicar**.

### <a name="master-data-services-mds"></a>Master Data Services (MDS)
- **Problema e impacto en el cliente:**  en la página de permisos de usuario, al conceder permisos al nivel raíz de la vista de árbol de entidades, verá el siguiente error: `"The model permission cannot be saved. The object guid is not valid"`

- **Solución alternativa:** 
  - Conceda permisos a los subnodos en la vista de árbol en lugar de a nivel de raíz.

### <a name="analysis-services"></a>Analysis Services
- **Problema e impacto en el cliente:** los conectores de datos de los siguientes orígenes aún no están disponibles para los modelos tabulares en el nivel de compatibilidad 1400.
  - Amazon Redshift
  - IBM Netezza
  - Impala
- **Solución alternativa:** Ninguno.   

- **Problema e impacto en el cliente:** los modelos de DirectQuery con el nivel de compatibilidad 1400 con perspectivas pueden generar errores al consultar o detectar metadatos.
- **Solución alternativa:** elimine las perspectivas y vuelva a realizar la implementación.

### <a name="tools"></a>Herramientas
- **Problema e impacto en el cliente:** cuando se ejecuta *DReplay* se produce un error con el siguiente mensaje: "Se ha producido un error inesperado de DReplay".
- **Solución alternativa:** Ninguno.

![barra_horizontal](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc2---august-2017"></a>SQL Server 2017 Release Candidate (RC2: agosto de 2017)
No hay notas de la versión de SQL Server en Windows relacionadas con esta versión. Consulte [Notas de la versión de SQL Server 2017 en Linux](../linux/sql-server-linux-release-notes.md).


![barra_horizontal](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-release-candidate-rc1---july-2017"></a>SQL Server 2017 Release Candidate (RC1, julio de 2017)
### <a name="sql-server-integration-services-ssis-rc1---july-2017"></a>SQL Server Integration Services (SSIS) (RC1, julio de 2017)
- **Problema e impacto en el cliente:** Se cambió el nombre del parámetro *runincluster* del procedimiento almacenado **[catálogo].[create_execution]** a *runinscaleout* para mejorar la coherencia y la legibilidad.
- **Solución alternativa:** Si tiene scripts existentes para ejecutar paquetes en Escalabilidad horizontal, debe cambiar el nombre del parámetro de *runincluster* a *runinscaleout* para que los scripts funcionen en RC1.

- **Problema e impacto en el cliente:** SQL Server Management Studio (SSMS) 17.1 y versiones anteriores no pueden desencadenar la ejecución de paquetes en Escalabilidad horizontal en RC1. El mensaje de error es: " *\@runincluster* no es un parámetro para el procedimiento **create_execution**". Este problema se corrige en la versión siguiente de SSMS, la versión 17.2. La versión 17.2 y versiones posteriores de SSMS admiten el nuevo nombre de parámetro y la ejecución de paquetes en Escalabilidad horizontal. 
- **Solución alternativa:** hasta que esté disponible la versión 17.2 de SSMS, siga estos pasos:
  1. Utilice la versión existente de SSMS para generar el script de ejecución de paquetes.
  2. En el script, cambie el nombre del parámetro *runincluster* a *runinscaleout*.
  3. Ejecute el script.

![barra_horizontal](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-21-may--2017"></a>SQL Server 2017 CTP 2.1 (mayo de 2017)
### <a name="documentation-ctp-21"></a>Documentación (CTP 2.1)
- **Problema e impacto en el cliente:** La documentación de [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] es limitada, y el contenido está incluido en el conjunto de documentación de [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)].  El contenido de los artículos específico de [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] se distinguirá con **Se aplica a**. 
- **Problema e impacto en el cliente:** No hay ningún contenido sin conexión disponible para [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="sql-server-reporting-services-ctp-21"></a>SQL Server Reporting Services (CTP 2.1)

- **Problema e impacto en el cliente:** si tiene SQL Server Reporting Services y Power BI Report Server en la misma máquina y desinstala uno de ellos, no podrá conectarse al servidor de informes que quede con el Administrador de configuración de Report Server.
- **Solución alternativa:** para solucionar este problema, debe realizar las siguientes operaciones después de desinstalar uno de los servidores.

    1. Inicie un símbolo del sistema en modo de administrador.
    2. Vaya al directorio donde está instalado el servidor de informes restante.

        *Ubicación predeterminada de Power BI Report Server: C:\Program Files\Microsoft Power BI Report Server*

        *Ubicación predeterminada de SQL Server Reporting Services: C:\Program Files\Microsoft SQL Server Reporting Services*

    3. Después, vaya a la siguiente carpeta, que puede ser *SSRS* o *PBIRS*, en función del servidor que quede.
    4. Vaya a la carpeta WMI.
    5. Ejecute el siguiente comando:

        ```console
        regsvr32 /i ReportingServicesWMIProvider.dll
        ```

        Si ve el error siguiente, ignórelo.

        ```
        The module "ReportingServicesWMIProvider.dll" was loaded but the entry-point DLLInstall was not found. Make sure that "ReportingServicesWMIProvider.dll" is a valid DLL or OCX file and then try again.
        ```

### <a name="tsqllanguageservicemsi-ctp-21"></a>TSqlLanguageService.msi (CTP 2.1)

- **Problema e impacto en el cliente:** después de realizar la instalación en un equipo que tenga una versión 2016 de *TSqlLanguageService.msi* (mediante el programa de instalación de SQL o como un paquete redistribuible independiente), se quitan las versiones v13.* (SQL 2016) de *Microsoft.SqlServer.Management.SqlParser.dll* y *Microsoft.SqlServer.Management.SystemMetadataProvider.dll*. Cualquier aplicación que tenga una dependencia de las versiones de 2016 de esos ensamblados deja de funcionar y genera un error similar a: *error: No se pudo cargar el archivo o ensamblado "Microsoft.SqlServer.Management.SqlParser, Version=13.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91" o una de sus dependencias. El sistema no encuentra el archivo especificado.*

   Además, los intentos de volver a instalar una versión de 2016 de TSqlLanguageService.msi generan el mensaje de error: *Error en la instalación del servicio de lenguaje T-SQL de Microsoft SQL Server 2016 porque ya existe una versión superior en la máquina*.

- **Solución alternativa:** para solucionar este problema y corregir una aplicación que depende de la versión v13 de los ensamblados, siga estos pasos:

   1. Vaya a **Agregar o quitar programas**.
   2. Busque *Microsoft SQL Server 2019 T-SQL Language Service CTP2.1*, haga clic en él con el botón derecho y seleccione **Desinstalar**.
   3. Después de quitar el componente, repare la aplicación que se interrumpe o vuelva a instalar la versión adecuada de *TSqlLanguageService.MSI*.

   Esta solución quitará la versión v14 de esos ensamblados, por lo que dejarán de funcionar todas las aplicaciones que dependan de las versiones v14. Si se necesitan esos ensamblados, es necesario realizar una instalación independiente sin ninguna instalación en paralelo de 2016.

![barra_horizontal](../sql-server/media/horizontal-bar.png)
## <a name="sql-server-2017-ctp-20-april--2017"></a>SQL Server 2017 CTP 2.0 (abril 2017)
### <a name="documentation-ctp-20"></a>Documentación (CTP 2.0)
- **Problema e impacto en el cliente:** La documentación de [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] es limitada, y el contenido está incluido en el conjunto de documentación de [!INCLUDE[ssSQL15_md](../includes/sssql16-md.md)].  El contenido de los artículos específico de [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] se distinguirá con **Se aplica a**. 
- **Problema e impacto en el cliente:** No hay ningún contenido sin conexión disponible para [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)].

### <a name="always-on-availability-groups"></a>Grupos de disponibilidad AlwaysOn

- **Problema e impacto en el cliente:** una instancia de SQL Server que hospeda una réplica secundaria del grupo de disponibilidad se bloquea si la versión principal de SQL Server es inferior a la instancia que hospeda la réplica principal. Afecta a las actualizaciones de todas las versiones compatibles de SQL Server que hospedan grupos de disponibilidad para SQL Server [!INCLUDE[ssSQLv14_md](../includes/sssqlv14-md.md)] CTP 2.0. Este problema se produce en las siguientes circunstancias. 

> 1. El usuario actualiza la réplica secundaria que hospeda la instancia de SQL Server de acuerdo con los [procedimientos recomendados](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).
> 2. Después de actualizar, se produce una conmutación por error y la réplica secundaria que se acaba de actualizar se convierte en la principal antes de completar la actualización de todas las réplicas secundarias del grupo de disponibilidad. La réplica principal anterior es ahora una réplica secundaria que tiene una versión anterior a la principal.
> 3. El grupo de disponibilidad está en una configuración no admitida y las réplicas secundarias restantes pueden ser vulnerables a los bloqueos. 

- **Solución alternativa:** conéctese a la instancia de SQL Server que hospeda la nueva réplica principal y quite la réplica secundaria errónea de la configuración.

   ```sql
   ALTER AVAILABILITY GROUP agName REMOVE REPLICA ON NODE instanceName;
   ```

   La instancia de SQL Server que hospedaba la réplica secundaria se recupera.

## <a name="more-information"></a>Más información
- [notas de la versión de SQL Server Reporting Services](../reporting-services/release-notes-reporting-services.md).
- [Problemas conocidos de Machine Learning Services](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)
- [Vínculos e información del Centro de actualizaciones de SQL Server sobre todas las versiones compatibles](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png)