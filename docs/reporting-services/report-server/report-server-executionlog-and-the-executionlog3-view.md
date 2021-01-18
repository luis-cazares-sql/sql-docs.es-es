---
title: Registro de ejecución del servidor de informes y la vista ExecutionLog3 | Microsoft Docs
description: Obtenga información sobre el registro de ejecución del servidor de informes en Reporting Services, que contiene información sobre los informes de los servidores en modo nativo o en una granja de servidores de SharePoint.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], execution
- execution logs [Reporting Services]
ms.assetid: a7ead67d-1404-4e67-97e7-4c7b0d942070
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 28d083e053e31c1fcce26e233ba22211e211a993
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125596"
---
# <a name="report-server-executionlog-and-the-executionlog3-view"></a>Registro de ejecución del servidor de informes y la vista ExecutionLog3
  El registro de la ejecución del servidor de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]contiene información sobre los informes que se ejecutan en el servidor o en varios servidores de una implementación escalada en modo nativo o de una granja de servidores de SharePoint. Puede usar el registro de la ejecución de informes para averiguar con qué frecuencia se solicita el informe, qué formatos de salida se usan más y cuántos milisegundos del tiempo de procesamiento se dedica a cada fase del procesamiento. El registro contiene información sobre el tiempo de ejecución de la consulta de conjunto de datos de un informe y el tiempo empleado en el procesamiento de los datos. Si es administrador del servidor de informes, puede revisar la información del registro e identificar las tareas de ejecución prolongada, y realizar sugerencias a los autores de informes en las áreas del informe (conjunto de datos o procesamiento) que se puedan mejorar.  
  
 Los servidores de informes configurados para el modo de SharePoint, también pueden usar los registros de ULS de SharePoint. Para obtener más información, vea [Activar eventos de Reporting Services para el registro de seguimiento de SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
##  <a name="viewing-log-information"></a><a name="bkmk_top"></a> Ver la información del registro  
 La ejecución del servidor de informes registra datos sobre la ejecución de informes en una tabla interna de la base de datos. La información de la tabla está disponible en las vistas de SQL Server.  
  
 El registro de la ejecución de informes se almacena en la base de datos del servidor de informes que se denomina **ReportServer** de forma predeterminada. Las vistas de SQL proporcionan la información del registro de la ejecución. Las vistas "2" y "3" se agregaron en versiones más recientes y contienen campos nuevos o campos con nombres más descriptivos que las versiones anteriores. Permanece en el producto las vistas antiguas para que no se vean afectadas las aplicaciones personalizadas que dependen de ellas. Si no tiene una dependencia en una vista anterior, por ejemplo ExecutionLog, se recomienda utilizar la vista más reciente, ExecutionLog **3**.  
  
 En este tema:  
  
-   [Configuración para un servidor de informes en modo de SharePoint](#bkmk_sharepoint)  
  
-   [Configuración de un servidor de informes en modo nativo](#bkmk_native)  
  
-   [Campos de registro (ExecutionLog3)](#bkmk_executionlog3)  
  
-   [El campo AdditionalInfo](#bkmk_additionalinfo)  
  
-   [Campos de registro (ExecutionLog2)](#bkmk_executionlog2)  
  
-   [Campos de registro (ExecutionLog)](#bkmk_executionlog)  
  
##  <a name="configuration-settings-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepoint"></a> Configuración para un servidor de informes en modo de SharePoint  
 Puede activar o desactivar la ejecución de informes en la configuración del sistema de una aplicación de servicios de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 De forma predeterminada, las entradas de registro se mantienen 60 días. Las entradas que superan esta fecha se quitan a las 2:00 a. m. todos los días. En instalaciones antiguas, solo habrá 60 días de información disponibles en cualquier momento.  
  
 No puede establecer límites en el número de filas o en el tipo de entradas que se registran.  
  
 **Para habilitar el registro de la ejecución:**  
  
1.  En Administración central de SharePoint, haga clic en **Administrar aplicaciones de servicio** en el grupo **Administración de aplicaciones** .  
  
2.  Haga clic en el nombre de la aplicación de servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] que desee configurar.  
  
3.  Haga clic en **Configuración del sistema**.  
  
4.  Seleccione **Habilitar el registro de la ejecución** en la sección **Registro** .  
  
5.  Haga clic en **OK**.  
  
 **Para habilitar el registro detallado:**  
  
 Debe habilitar el registro como se describe en los pasos anteriores y, a continuación, realizar las acciones siguientes:  
  
1.  En la página **Configuración del sistema** de la aplicación de servicios de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , busque la sección **Definido por el usuario** .  
  
2.  Cambie **ExecutionLogLevel** a **detallado**. Este campo es un campo de entrada de texto y los dos valores posibles son **detallado** y **normal**.  
  
##  <a name="configuration-settings-for-a-native-mode-report-server"></a><a name="bkmk_native"></a> Configuración de un servidor de informes en modo nativo  
 Puede activar o desactivar la ejecución de informes en la página propiedades del servidor en SQL Server Management Studio. **EnableExecutionLogging** es una propiedad avanzada.  
  
 De forma predeterminada, las entradas de registro se mantienen 60 días. Las entradas que superan esta fecha se quitan a las 2:00 a. m. todos los días. En instalaciones antiguas, solo habrá 60 días de información disponibles en cualquier momento.  
  
 No puede establecer límites en el número de filas o en el tipo de entradas que se registran.  
  
 **Para habilitar el registro de la ejecución:**  
  
1.  Inicie SQL Server Management Studio con privilegios de administrador. Por ejemplo, haga clic con el botón derecho en el icono de Management Studio y seleccione "Ejecutar como administrador".  
  
2.  Conéctese al servidor de informes que desee.  
  
3.  Haga clic con el botón derecho en el nombre del servidor y haga clic en **Propiedades**. Si la opción Propiedades está deshabilitada, compruebe que ha ejecutado SQL Server Management Studio con privilegios de administrador.  
  
4.  Haga clic en la página **Registro** .  
  
5.  Seleccione **Habilitar el registro de la ejecución de informes**.  
  
 **Para habilitar el registro detallado:**  
  
 Debe habilitar el registro como se describe en los pasos anteriores y, a continuación, realizar las acciones siguientes:  
  
1.  En el cuadro de diálogo **Propiedades de servidor** , haga clic en la página **Avanzadas** .  
  
2.  En la sección **Definido por el usuario** , cambie **ExecutionLogLevel** a **detallado**. Este campo es un campo de entrada de texto y los dos valores posibles son **detallado** y **normal**.  
  
##  <a name="log-fields-executionlog3"></a><a name="bkmk_executionlog3"></a> Campos de registro (ExecutionLog3)  
 En esta vista, se agregaron nodos de diagnóstico adicionales en la columna **AdditionalInfo** basada en XML. La columna AdditionalInfo contiene una estructura XML de 1 a muchos campos adicionales de información. La siguiente es una instrucción Transact-SQL de ejemplo para recuperar filas de la vista ExecutionLog3. En el ejemplo se supone que la base de datos del servidor de informes se denomina **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog3 order by TimeStart DESC  
```  
  
 La siguiente tabla describe los datos que se capturan en el registro de ejecución de informes  
  
|Columna|Descripción|  
|------------|-----------------|  
|InstanceName|Nombre de la instancia de servidor de informes que procesó la solicitud. Si el entorno tiene más de un servidor de informes, puede analizar la distribución de InstanceName para supervisar y determinar si el equilibrador de carga de red distribuye las solicitudes entre los servidores de informes como se esperaba.|  
|ItemPath|Ruta de acceso donde se almacena un informe o un elemento de informe.|  
|UserName|Identificador del usuario.|  
|ExecutionID|Identificador interno asociado a una solicitud. Las solicitudes en las mismas sesiones de usuario comparten el mismo identificador de ejecución.|  
|RequestType|Valores posibles:<br /><br /> Interactive<br /><br /> Suscripción<br /><br /> <br /><br /> El análisis de los datos de registro filtrados por RequestType=Subscription y ordenados por TimeStart pueden revelar los periodos de mucho uso de suscripciones y puede indicar la conveniencia de modificar algunas de las suscripciones de informe a un momento diferente.|  
|Formato|Formato de representación.|  
|Parámetros|Valores de parámetros utilizados para la ejecución de un informe.|  
|ItemAction|Valores posibles:<br /><br /> Representación<br /><br /> Sort<br /><br /> BookMarkNavigation<br /><br /> DocumentNavigation<br /><br /> GetDocumentMap<br /><br /> Findstring<br /><br /> Execute<br /><br /> RenderEdit|  
|TimeStart|Horas de inicio y detención que indican la duración del procesamiento de un informe.|  
|TimeEnd||  
|TimeDataRetrieval|Número de milisegundos empleados en recuperar los datos.|  
|TimeProcessing|Número de milisegundos empleados en procesar el informe.|  
|TimeRendering|Número de milisegundos empleados en representar el informe.|  
|Source|Origen de la ejecución del informe. Valores posibles:<br /><br /> En vivo<br /><br /> Caché: hace referencia a una ejecución almacenada en caché, por ejemplo, las consultas de conjunto de datos no se ejecutan de forma dinámica.<br /><br /> Instantánea<br /><br /> Historial<br /><br /> AdHoc: hace referencia a un informe detallado basado en un modelo de informe generado de forma dinámica, o bien a un informe del Generador de informes del cual se puede obtener una vista previa en un cliente mediante el uso de un servidor de informes para el procesamiento y la representación.<br /><br /> Sesión: hace referencia a una solicitud de seguimiento en una sesión que ya se ha establecido.  Por ejemplo, la solicitud inicial es ver la página 1 y la solicitud de seguimiento es exportar a Excel con el estado actual de la sesión.<br /><br /> Rdce:  hace referencia a una extensión de personalización de definición de informe. Las extensiones personalizadas de RDCE pueden personalizar dinámicamente una definición de informe antes de que se pase al motor de procesamiento en el momento de la ejecución de informes.|  
|Estado|Estado (rsSuccess o un código de error; si se producen varios errores, solo se registra el primero).|  
|ByteCount|Tamaño de los informes representados en bytes|  
|RowCount|Número de filas devueltas de consultas.|  
|AdditionalInfo|Un contenedor de propiedades XML que incluye información adicional sobre la ejecución. El contenido puede ser diferente para cada fila.|  
  
##  <a name="the-additionalinfo-field"></a><a name="bkmk_additionalinfo"></a> El campo AdditionalInfo  
 El campo AdditionalInfo es un contenedor de propiedades o estructura XML que incluye información adicional sobre la ejecución. El contenido puede ser diferente para cada fila del registro.  
  
 Estos son algunos ejemplos de los contenidos del campo AddtionalInfo para el registro estándar y el registro detallado:  
  
 **Ejemplo de registro estándar de AddtionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>147</ConnectionOpenTime>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>642</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>63</ExecuteReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>157</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>60</ExecuteReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 **Ejemplo de registro detallado de AdditionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>127</ConnectionOpenTime>  
      <DataSource>  
        <Name>DataSource1</Name>  
        <DataExtension>SQL</DataExtension>  
      </DataSource>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>33</ExecuteReaderTime>  
          <DataReaderMappingTime>30</DataReaderMappingTime>  
          <DisposeDataReaderTime>1</DisposeDataReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>1</ExecuteReaderTime>  
          <DataReaderMappingTime>0</DataReaderMappingTime>  
          <DisposeDataReaderTime>0</DisposeDataReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 A continuación se describen algunos de los valores que aparecerán en el campo AdditionalInfo:  
  
-   **ProcessingEngine**  
  
     1=SQL Server 2005, 2=Nuevo motor de procesamiento a petición. Si una mayoría de los informes sigue mostrando el valor 1, puede investigar cómo reajustarlos para que usen el motor de procesamiento a petición más reciente y más eficaz.  
  
     `<ProcessingEngine>2</ProcessingEngine>`  
  
-   **ScalabilityTime**  
  
     Número de milisegundos empleados en realizar operaciones relacionadas con la escala en el motor de procesamiento. Un valor de 0 indica que no se empleó ningún tiempo adicional en las operaciones de escala y 0 también indica que la solicitud no estaba bajo presión de memoria.  
  
    ```  
    <ScalabilityTime>  
        <Processing>0</Processing>  
    </ScalabilityTime>  
    ```  
  
-   **EstimatedMemoryUsageKB**  
  
     Una estimación de la cantidad máxima de memoria, en kilobytes, que utiliza cada componente durante una solicitud determinada.  
  
    ```  
    <EstimatedMemoryUsageKB>  
        <Processing>38</Processing>  
    </EstimatedMemoryUsageKB>  
    ```  
  
-   **DataExtension**  
  
     Los tipos de extensiones de datos u orígenes de datos usados en el informe. El número es un recuento del número de repeticiones del origen de datos concreto.  
  
    ```  
    <DataExtension>  
       <DAX>2</DAX>  
    </DataExtension>  
    ```  
  
-   **ExternalImages**  
  
     El valor se expresa en milisegundos. Estos datos se pueden utilizar para diagnosticar problemas de rendimiento. El tiempo necesario para recuperar imágenes desde un servidor web externo puede ralentizar la ejecución de informes global.  
  
    ```  
    <ExternalImages>  
        <Count>3</Count>  
        <ByteCount>9268</ByteCount>  
        <ResourceFetchTime>9</ResourceFetchTime>  
    </ExternalImages>  
    ```  
  
-   **Conexiones**  
  
     Estructura de varios niveles  
  
    ```  
    <Connections>  
        <Connection>  
          <ConnectionOpenTime>127</ConnectionOpenTime>  
          <DataSource>  
            <Name>DataSource1</Name>  
            <DataExtension>SQL</DataExtension>  
          </DataSource>  
          <DataSets>  
            <DataSet>  
              <Name>DataSet1</Name>  
              <RowsRead>16</RowsRead>  
              <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>33</ExecuteReaderTime>  
              <DataReaderMappingTime>30</DataReaderMappingTime>  
              <DisposeDataReaderTime>1</DisposeDataReaderTime>  
            </DataSet>  
            <DataSet>  
              <Name>DataSet2</Name>  
              <RowsRead>3</RowsRead>  
              <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>1</ExecuteReaderTime>  
              <DataReaderMappingTime>0</DataReaderMappingTime>  
              <DisposeDataReaderTime>0</DisposeDataReaderTime>  
            </DataSet>  
          </DataSets>  
        </Connection>  
    </Connections>  
  
    ```  
  
##  <a name="log-fields-executionlog2"></a><a name="bkmk_executionlog2"></a> Campos de registro (ExecutionLog2)  
 Esta vista ha agregado algunos campos nuevos y ha cambiado el nombre de otros. La siguiente es una instrucción Transact-SQL de ejemplo para recuperar filas de la vista ExecutionLog2. En el ejemplo se supone que la base de datos del servidor de informes se denomina **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog2 order by TimeStart DESC  
```  
  
 La siguiente tabla describe los datos que se capturan en el registro de ejecución de informes  
  
|Columna|Descripción|  
|------------|------------------------------------------------------------|  
|InstanceName|Nombre de la instancia de servidor de informes que procesó la solicitud.|  
|ReportPath|La estructura de ruta de acceso al informe. Un informe guardado en la carpeta raíz como "test" tiene un ReportPath de '"/test".<br /><br /> Un informe denominado "test" guardado en la carpeta "samples" tendrá un ReportPath de "/Samples/test/".|  
|UserName|Identificador del usuario.|  
|ExecutionID||  
|RequestType|Tipo de solicitud (de usuario o del sistema).|  
|Formato|Formato de representación.|  
|Parámetros|Valores de parámetros utilizados para la ejecución de un informe.|  
|ReportAction|Valores posibles: Render, Sort, BookMarkNavigation, DocumentNavigation, GetDocumentMap, Findstring|  
|TimeStart|Hora de inicio que indica la duración de un proceso de informe.|
|TimeEnd|Hora de finalización que indica la duración de un proceso de informe.|
|TimeDataRetrieval|Número de milisegundos empleados en recuperar los datos.|
|TimeProcessing|Número de milisegundos empleados en procesar el informe.|
|TimeRendering|Número de milisegundos empleados en representar el informe.|
|Source|Origen de la ejecución del informe (1=Activo, 2=Caché, 3=Instantánea, 4=Historial).|  
|Estado|Estado (rsSuccess o un código de error; si se producen varios errores, solo se registra el primero).|  
|ByteCount|Tamaño de los informes representados en bytes|  
|RowCount|Número de filas devueltas de consultas.|  
|AdditionalInfo|Un contenedor de propiedades XML que incluye información adicional sobre la ejecución.|  
  
##  <a name="log-fields-executionlog"></a><a name="bkmk_executionlog"></a> Campos de registro (ExecutionLog)  
 La siguiente es una instrucción Transact-SQL de ejemplo para recuperar filas de la vista ExecutionLog. En el ejemplo se supone que la base de datos del servidor de informes se denomina **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog order by TimeStart DESC  
  
```  
  
 La siguiente tabla describe los datos que se capturan en el registro de ejecución de informes  
  
|Columna|Descripción|  
|------------|-----------------|  
|InstanceName|Nombre de la instancia de servidor de informes que procesó la solicitud.|  
|ReportID|Identificador del informe.|  
|UserName|Identificador del usuario.|  
|RequestType|Valores posibles:<br /><br /> True = una solicitud de suscripción<br /><br /> False= una solicitud interactiva|  
|Formato|Formato de representación.|  
|Parámetros|Valores de parámetros utilizados para la ejecución de un informe.|  
|TimeStart|Horas de inicio y detención que indican la duración del procesamiento de un informe.|  
|TimeEnd||  
|TimeDataRetrieval|Número de milisegundos dedicados a recuperar los datos, procesar el informe y representarlo.|  
|TimeProcessing||  
|TimeRendering||  
|Source|Origen de la ejecución del informe. Valores posibles: (1=Activo, 2=Caché, 3=Instantánea, 4=Historial, 5=Adhoc, 6=Sesión, 7=RDCE).|  
|Estado|Valores posibles: rsSuccess, rsProcessingAborted o un código de error. Si aparecen varios errores, solo se registra el primero.|  
|ByteCount|Tamaño de los informes representados en bytes|  
|RowCount|Número de filas devueltas de consultas.|  
  
## <a name="see-also"></a>Consulte también  
 [Activar eventos de Reporting Services para el registro de seguimiento de SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)   
 [Archivos de registro y orígenes de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Referencia de errores y eventos &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
