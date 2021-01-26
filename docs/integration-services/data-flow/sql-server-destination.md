---
description: SQL Server, destino
title: Destino de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sqlserverdest.f1
- sql13.dts.designer.sqlserverdestadapter.connection.f1
- sql13.dts.designer.sqlserverdestadapter.mappings.f1
- sql13.dts.designer.sqlserverdestadapter.advanced.f1
helpviewer_keywords:
- SQL Server destination
- loading data
- destinations [Integration Services], SQL Server
- inserting data
- bulk load [Integration Services]
ms.assetid: a0227cd8-6944-4547-87e8-7b2507e26442
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6e980e075f23d3a943870fc6dd02aa420797f8a4
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596482"
---
# <a name="sql-server-destination"></a>SQL Server, destino

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El destino de SQL Server se conecta a una base de datos local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y realiza una carga masiva de datos en tablas y vistas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . No se puede usar el destino de SQL Server en paquetes con acceso a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un servidor remoto. En su lugar, los paquetes deben utilizar el destino de OLE DB. Para más información, consulte [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md).  
  
## <a name="permissions"></a>Permisos  
 Los usuarios que ejecutan paquetes que incluyen el destino de SQL Server deben tener el permiso Crear objetos globales. Para otorgar este permiso a los usuarios, utilice la herramienta Directiva de seguridad local que se abre desde el menú **Herramientas administrativas** . Si recibe un mensaje de error al ejecutar un paquete que utiliza el destino de SQL Server, asegúrese de que la cuenta que ejecuta el paquete tiene el permiso Crear objetos globales.  
  
## <a name="bulk-inserts"></a>Inserciones masivas  
 Si se intenta utilizar el destino de SQL Server para realizar una carga masiva de datos en una base de datos remota de SQL Server, puede recibir un mensaje de error similar al siguiente: "Hay un registro OLE DB disponible. Origen: "Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client" Resultado: 0x80040E14 Descripción: "No se pudo realizar la carga masiva porque el objeto de asignación de archivos SSIS "Global\DTSQLIMPORT" no se pudo abrir. Código de error del sistema operativo 2 (El sistema no encuentra el archivo especificado). Asegúrese de que tiene acceso a un servidor local mediante Seguridad de Windows."".  
  
 El destino de SQL Server ofrece la misma inserción de datos de alta velocidad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que proporciona la tarea Inserción masiva. En cambio, al usar el destino de SQL Server, un paquete puede aplicar transformaciones a los datos de las columnas antes de que estos se carguen en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Para cargar los datos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], es recomendable usar el destino de SQL Server en lugar del destino de OLE DB.  
  
### <a name="bulk-insert-options"></a>Opciones de inserción masiva  
 Si el destino de SQL Server usa el modo de acceso de datos de carga rápida, puede especificar las siguientes opciones de carga rápida:  
  
-   Mantener los valores de identidad del archivo de datos importado o usar valores exclusivos asignados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Conservar los valores NULL durante la operación de carga masiva.  
  
-   Comprobar las restricciones en la tabla o vista de destino durante la operación de importación masiva.  
  
-   Adquirir un bloqueo de nivel de tabla durante la operación de carga masiva.  
  
-   Ejecutar desencadenadores de inserción definidos en la tabla de destino durante la operación de carga masiva.  
  
-   Especificar el número de la primera fila en la entrada que se debe cargar durante la operación de inserción masiva.  
  
-   Especificar el número de la última fila en la entrada que se debe cargar durante la operación de inserción masiva.  
  
-   Especificar el número máximo de errores que pueden producirse antes de que se cancele la operación de carga masiva. Cada fila que no puede importarse se cuenta como un error.  
  
-   Especificar las columnas de la entrada que contienen datos ordenados.  
  
 Para obtener más información sobre las opciones de carga masiva, vea [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md).  
  
#### <a name="performance-improvements"></a>Mejoras en el rendimiento  
 Para mejorar el rendimiento de una inserción masiva y el acceso a los datos de tablas durante la operación de inserción masiva, se deben cambiar las opciones predeterminadas de la manera siguiente:  
  
-   No comprobar las restricciones en la tabla o vista de destino durante la operación de importación masiva.  
  
-   No ejecutar desencadenadores de inserción definidos en la tabla de destino durante la operación de carga masiva.  
  
-   No aplicar un bloqueo a la tabla. De esta manera, la tabla sigue disponible para otros usuarios y aplicaciones durante la operación de inserción masiva.  
  
## <a name="configuration-of-the-sql-server-destination"></a>Configuración del destino de SQL Server  
 Puede configurar el destino de SQL Server de las maneras siguientes:  
  
-   Especificar la tabla o vista en la que se debe realizar la carga masiva de los datos.  
  
-   Personalizar la operación de carga masiva especificando opciones tales como si se deben comprobar restricciones.  
  
-   Especificar si todas las filas se confirman en un lote o establecer el número máximo de filas que se confirman como un lote.  
  
-   Especificar un tiempo de espera para la operación de carga masiva.  
  
 Este destino usa un administrador de conexiones de OLE DB para conectarse a un origen de datos y el administrador de conexiones especifica el proveedor OLE DB que se debe usar. Para más información, consulte [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Un proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] también proporciona el objeto de origen de datos desde el que se puede crear un administrador de conexiones de OLE DB. Esto hace que los orígenes de datos y vistas del origen de datos queden a disposición del destino de SQL Server.  
  
 El destino de SQL Server tiene una entrada. No admite una salida de error.  
  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 El cuadro de diálogo **Editor avanzado** indica las propiedades que se pueden establecer mediante programación. Para obtener más información acerca de las propiedades que puede establecer a través del cuadro de diálogo **Editor avanzado** o mediante programación, haga clic en uno de los temas siguientes:  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [Propiedades personalizadas del destino SQL Server](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
 Para obtener más información sobre cómo establecer valores de propiedades, haga clic en uno de los temas siguientes:  
  
-   [Realizar una carga masiva de datos mediante el destino de SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Establecer las propiedades de un componente de flujo de datos](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Realizar una carga masiva de datos mediante el destino de SQL Server](../../integration-services/data-flow/bulk-load-data-by-using-the-sql-server-destination.md)  
  
-   [Establecer las propiedades de un componente de flujo de datos](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Artículo técnico, sobre la [posible aparición del error "No se puede preparar la inserción masiva de SSIS para la inserción de datos" en sistemas habilitados para UAC](/troubleshoot/sql/integration-services/error-you-run-ssis-package), en support.microsoft.com.  
  
-   Artículo técnico, [The Data Loading Performance Guide](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), en msdn.microsoft.com.  
  
-   Artículo técnico sobre cómo [usar SQL Server Integration Services para la carga masiva de datos](https://go.microsoft.com/fwlink/?LinkId=233701), en simple-talk.com.  
  
## <a name="sql-destination-editor-connection-manager-page"></a>Editor de destino de SQL (página Administrador de conexiones)
  Utilice la página **Administrador de conexiones** del cuadro de diálogo **Editor de destino de SQL** para especificar la información de orígenes de datos y para obtener una vista previa de los resultados. El destino [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] carga los datos en tablas o vistas en una base de datos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="options"></a>Opciones  
 **Administrador de conexiones OLE DB**  
 Seleccione una conexión existente de la lista o cree una nueva conexión haciendo clic en **Nueva**.  
  
 **Nuevo**  
 Cree una conexión mediante el cuadro de diálogo **Configurar el administrador de conexiones OLE DB** .  
  
 **Usar una tabla o una vista**  
 Seleccione una tabla o vista existente de la lista, o cree una nueva conexión haciendo clic en **Nueva**.  
  
 **Nuevo**  
 Permite crear una tabla con el cuadro de diálogo **Crear tabla** .  
  
> [!NOTE]  
>  Al hacer clic en **Nueva**, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] genera una instrucción predeterminada CREATE TABLE basada en el origen de datos conectado. La instrucción predeterminada CREATE TABLE no incluirá el atributo FILESTREAM, aunque la tabla de origen tenga una columna con el atributo FILESTREAM declarado. Para ejecutar un componente [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] con el atributo FILESTREAM, implemente en primer lugar el almacenamiento de FILESTREAM en la base de datos de destino. A continuación, agregue el atributo FILESTREAM a la instrucción CREATE TABLE en el cuadro de diálogo **Crear tabla** . Para más información, vea [Datos de objeto binario grande &#40;Blob&#41; &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md).  
  
 **Versión preliminar**  
 Obtenga una vista previa de los resultados mediante el cuadro de diálogo **Vista previa de los resultados de la consulta** . La vista previa puede mostrar hasta 200 filas.  
  
## <a name="sql-destination-editor-mappings-page"></a>Editor de destino de SQL (página Asignaciones)
  Utilice la página **Asignaciones** del cuadro de diálogo **Editor de destino de SQL** para asignar columnas de entrada a columnas de destino.  
  
### <a name="options"></a>Opciones  
 **Columnas de entrada disponibles**  
 Muestra la lista de columnas de entrada disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de entrada disponibles de la tabla a columnas de destino.  
  
 **Columnas de destino disponibles**  
 Muestra la lista de columnas de destino disponibles. Utilice una operación de arrastrar y colocar para asignar columnas de destino disponibles de la tabla a columnas de entrada.  
  
 **Columna de entrada**  
 Muestra las columnas de entrada seleccionadas de la tabla anterior. Puede cambiar las asignaciones utilizando la lista de **Columnas de entrada disponibles**.  
  
 **Columna de destino**  
 Muestra las columnas de destino disponibles, independientemente de si están asignadas o no.  
  
## <a name="sql-destination-editor-advanced-page"></a>Editor de destino de SQL (página Avanzadas)
  Utilice la página **Avanzadas** del cuadro de diálogo **Editor de destino de SQL** para especificar opciones avanzadas de inserción masiva.  
  
### <a name="options"></a>Opciones  
 **Mantener valores de identidad**  
 Especifique si la tarea debe insertar valores en columnas de identidad. El valor predeterminado de esta propiedad es **False**.  
  
 **Mantener valores NULL**  
 Especifique si la tarea debe mantener valores NULL. El valor predeterminado de esta propiedad es **False**.  
  
 **Bloqueo de tabla**  
 Especifique si la tabla está bloqueada cuando se cargan los datos. El valor predeterminado de esta propiedad es **True**.  
  
 **Restricciones CHECK**  
 Especifique si la tarea debe comprobar restricciones. El valor predeterminado de esta propiedad es **True**.  
  
 **Activar desencadenadores**  
 Especifique si la inserción masiva debe activar desencadenadores en tablas. El valor predeterminado de esta propiedad es **False**.  
  
 **Primera fila**  
 Especifique la primera fila donde se va a insertar. El valor predeterminado de esta propiedad es **-1**, que indica que no se ha asignado ningún valor.  
  
> [!NOTE]  
>  Borre el cuadro de texto del **Editor de destino de SQL** para indicar que no desea asignar un valor a esta propiedad. Use -1 en la ventana **Propiedades** , el **Editor avanzado** y el modelo de objetos.  
  
 **Última fila**  
 Especifique la última fila donde se va a insertar. El valor predeterminado de esta propiedad es **-1**, que indica que no se ha asignado ningún valor.  
  
> [!NOTE]  
>  Borre el cuadro de texto del **Editor de destino de SQL** para indicar que no desea asignar un valor a esta propiedad. Use -1 en la ventana **Propiedades** , el **Editor avanzado** y el modelo de objetos.  
  
 **Número máximo de errores**  
 Especifique el número de errores que se pueden producir antes de que se detenga la inserción masiva. El valor predeterminado de esta propiedad es **-1**, que indica que no se ha asignado ningún valor.  
  
> [!NOTE]  
>  Borre el cuadro de texto del **Editor de destino de SQL** para indicar que no desea asignar un valor a esta propiedad. Use -1 en la ventana **Propiedades** , el **Editor avanzado** y el modelo de objetos.  
  
 **Tiempo de espera**  
 Especifique el número de segundos que se debe esperar antes de que la inserción masiva se detenga debido a un tiempo de espera.  
  
 **Columnas de orden**  
 Escriba los nombres de las columnas de orden. Las columnas se pueden ordenar en sentido ascendente o descendente. Si utiliza varias columnas de orden, delimite la lista con comas.  
  
## <a name="see-also"></a>Consulte también  
 [Flujo de datos](../../integration-services/data-flow/data-flow.md)  
