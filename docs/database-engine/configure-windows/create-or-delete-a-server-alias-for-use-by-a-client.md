---
title: Crear o eliminar un alias de servidor para que lo use un cliente | Microsoft Docs
description: Descubra cómo crear y eliminar un alias, un nombre alternativo que puede usar al conectarse a una instancia de SQL Server. Obtenga información sobre las ventajas de los alias.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- server alias
helpviewer_keywords:
- aliases [SQL Server], deleting
- aliases [SQL Server], creating
ms.assetid: b687e376-ee33-470d-b65a-87246bfefe6f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bb1c9179151dd41f166c81791078ed8c82dcd5c3
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98782996"
---
# <a name="create-or-delete-a-server-alias-for-use-by-a-client"></a>Crear o eliminar un alias de servidor para que lo use un cliente
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear o eliminar un alias de servidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de SQL Server. Un alias es un nombre alternativo que se puede utilizar para establecer una conexión. El alias encapsula los elementos necesarios de una cadena de conexión y los expone con un nombre elegido por el usuario. Los alias se pueden utilizar con cualquier aplicación cliente. Mediante la creación de alias de servidor, el equipo cliente puede conectarse a varios servidores que utilizan distintos protocolos de red sin necesidad de especificar el protocolo y los detalles de conexión de cada uno de ellos. Además, también puede habilitar varios protocolos de red al mismo tiempo, aunque solo los utilice de vez en cuando. Si ha configurado el servidor para que escuche en un número de puerto o canalización con nombre que no es el predeterminado, y ha deshabilitado el servicio SQL Server Browser, cree un alias que especifique el nuevo número de puerto o canalización con nombre.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-create-an-alias"></a>Para crear un alias  
  
1.  En el Administrador de configuración de SQL Server, expanda **Configuración de SQL Server Native Client**, haga clic con el botón derecho en **Alias** y, luego, haga clic en **Nuevo alias**.  
  
2.  En el cuadro **Nombre de alias** , escriba el nombre del alias. Las aplicaciones cliente utilizan este nombre cuando se conectan.  
  
3.  En el cuadro **Servidor** , escriba el nombre o la dirección IP del servidor. Para una instancia con nombre, incluya el nombre de la instancia.  
  
4.  En el cuadro **Protocolo** , seleccione el protocolo utilizado para este alias. Al seleccionar un protocolo, el título del cuadro de diálogo de propiedades opcional cambia a Nº de puerto, Nombre de canalización o Cadena de conexión.  
  
 Las cadenas de conexión descritas en la Ayuda del Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden resultar útiles para que los programadores creen sus propias cadenas de conexión. Para obtener acceso a esta información, en el cuadro de diálogo **Nuevo alias** , presione F1 o haga clic en **Ayuda**.  
  
> [!NOTE]  
>  Si un alias configurado intenta conectarse a una instancia o un servidor incorrectos, deshabilite y vuelva a habilitar el protocolo de red asociado. De este modo, se borrará la información de conexión almacenada en la memoria caché y se podrá establecer una conexión correctamente.  
  
#### <a name="to-delete-an-alias"></a>Para eliminar un alias  
  
1.  En el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , expanda **Configuración de SQL Server Native Client** y haga clic en **Alias**.  
  
2.  En el panel de detalles, haga clic con el botón derecho en el alias que quiera eliminar y, después, haga clic en **Eliminar**.  
  
  
