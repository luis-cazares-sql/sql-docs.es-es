---
title: Objetos DbProviderFactory
description: Describe el modelo de generador de proveedor y demuestra cómo usar las clases base del espacio de nombres `System.Data.Common`.
ms.date: 12/22/2020
ms.assetid: 2a8e2640-3a49-42a1-a3a9-b43026907ae1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1a373e7bde1e93d8dc92294f983a96de64e28ae1
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771783"
---
# <a name="dbproviderfactories"></a>Objetos DbProviderFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El espacio de nombres <xref:System.Data.Common> proporciona clases para la creación de instancias <xref:System.Data.Common.DbProviderFactory> que permiten trabajar con orígenes de datos específicos. Cuando crea una instancia <xref:System.Data.Common.DbProviderFactory> y le pasa información acerca del proveedor de datos, la instancia `DbProviderFactory` puede determinar el objeto fuertemente tipado correcto que debe devolver en función de la información que se ha proporcionado.  

El proveedor de datos <xref:Microsoft.Data.SqlClient> ya no aparece en el archivo machine.config, pero los proveedores personalizados seguirán apareciendo allí.  

## <a name="in-this-section"></a>En esta sección  

[Obtención de un SqlClientFactory](obtain-sqlclientfactory.md)  
Muestra cómo obtener `SqlClientFactory` de la clase `DbProviderFactories` para trabajar con orígenes de datos específicos en .NET.  

## <a name="see-also"></a>Vea también

- [Recuperación y modificación de datos en ADO.NET](retrieving-modifying-data.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
