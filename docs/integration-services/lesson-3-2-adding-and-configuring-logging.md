---
description: 'Lección 3-2: Adición y configuración del registro'
title: 'Paso 2: Adición y configuración del registro | Microsoft Docs'
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 56105f3f-e500-4669-8c8e-acf434527727
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 32daaf012159bd42a6f73330bc8371157e459609
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125630"
---
# <a name="lesson-3-2-add-and-configure-logging"></a>Lección 3-2: Adición y configuración del registro

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]



En esta tarea, habilitará el registro del flujo de datos del paquete Lesson 3.dtsx. Después, configurará un proveedor de registro de archivos de texto para registrar los eventos PipelineExecutionPlan y PipelineExecuteTrees. El proveedor de registro de archivos de texto crea registros que se pueden ver y transportar con facilidad. La sencillez de estos archivos de registro hace que sean útiles durante la fase de prueba básica de un paquete. También puede ver las entradas del registro en la ventana **Registrar eventos** del Diseñador de [!INCLUDE[ssIS](../includes/ssis-md.md)].  
  
## <a name="add-logging-to-the-package"></a>Adición de registro al paquete  
  
1.  En el menú **SSIS**, seleccione **Registro**.  
    En Visual Studio 2019, el menú **SSIS** se encuentra en Extensiones-> SSIS. Asegúrese de que ha seleccionado la pestaña **Flujo de datos** y no **Flujo de control**.
  
2.  En el cuadro de diálogo **Configurar registros de SSIS**, asegúrese de que en el panel **Contenedores** está seleccionado el objeto en la posición superior. Este objeto representa el paquete de la lección 3.
  
3.  En la pestaña **Proveedores y registros**, en el cuadro **Tipo de proveedor**, seleccione **Proveedor de registro SSIS para archivos de texto** y haga clic en **Agregar**.  
  
    Integration Services agrega un nuevo proveedor de registro de archivos de texto al paquete con el nombre predeterminado **Proveedor de registro SSIS para archivos de texto**. Ahora puede configurar el nuevo proveedor de registro.  
  
4.  En la columna **Nombre**, escriba **Lesson 3 Log File**.  
  
5.  Si lo desea, modifique el campo **Descripción**.  
  
6.  En la columna **Configuración**, seleccione **\<New Connection>** para especificar dónde escribe [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] la información de registro.  
  
    En el cuadro de diálogo **Editor del administrador de conexiones de archivos**, en **Tipo de uso**, haga clic en **Crear archivo** y después en **Examinar**. De forma predeterminada, el cuadro de diálogo **Seleccionar archivo** abre la carpeta del proyecto, pero puede guardar la información de registro en cualquier ubicación.  
  
7.  En el cuadro de diálogo **Seleccionar archivo**, en el cuadro **Nombre de archivo**, escriba **TutorialLog.log** y haga clic en **Abrir**.
  
8.  Haga clic en **Aceptar** para cerrar el cuadro de diálogo **Editor del administrador de conexiones de archivos**.  
  
9. En el panel **Contenedores** , expanda todos los nodos de la jerarquía del contenedor de paquetes y, a continuación, desactive todas las casillas, incluida **Extract Sample Currency Data** . Ahora, active la casilla **Extract Sample Currency Data** para obtener solo los eventos de este nodo.  
  
    > [!NOTE]  
    > Si la casilla **Extract Sample Currency Data** está atenuada en lugar de activada, la tarea usa la configuración de registro del contenedor primario y no se pueden habilitar los eventos de registro específicos de la tarea. Para resolver esta instancia, desactive la casilla principal.
  
10. En la columna **Eventos** de la pestaña **Detalles** , seleccione los eventos **PipelineExecutionPlan** y **PipelineExecutionTrees** .  
  
11. Haga clic en **Avanzadas** para revisar los detalles que el proveedor de registro escribe en el registro para cada evento. De forma predeterminada, todas las categorías de información se seleccionan automáticamente para los eventos que se especifiquen.  
  
12. Haga clic en **Básicas** para ocultar las categorías de información.  
  
13. En la pestaña **Proveedores y registros** , en la columna **Nombre** , seleccione **Lesson 3 Log File**. Después de crear un proveedor de registro para el paquete, puede anular su selección para desactivar el registro, sin tener que eliminar un proveedor de registro y crearlo de nuevo.  
  
14. Seleccione **Aceptar**.  
  
## <a name="go-to-next-task"></a>Ir a la tarea siguiente  
[Paso 3: Prueba del paquete de la lección 3](../integration-services/lesson-3-3-testing-the-lesson-3-tutorial-package.md)  
  
