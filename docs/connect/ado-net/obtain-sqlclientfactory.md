---
title: Obtención de un SqlClientFactory
description: Obtenga información sobre cómo obtener un SqlClientFactory de la clase DbProviderFactories para trabajar con orígenes de datos específicos en .NET.
ms.date: 12/22/2020
ms.assetid: a16e4a4d-6a5b-45db-8635-19570e4572ae
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 891cabf04bc8e63537983a91d8526a5cbdf238bf
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771779"
---
# <a name="obtain-a-sqlclientfactory"></a>Obtención de un SqlClientFactory

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

El proceso de obtención de <xref:System.Data.Common.DbProviderFactory> implica pasar información sobre un proveedor de datos a la clase <xref:System.Data.Common.DbProviderFactories>. En función de esta información, el método <xref:System.Data.Common.DbProviderFactories.GetFactory%2A> crea un generador del proveedor fuertemente tipado. Por ejemplo, para crear un <xref:Microsoft.Data.SqlClient.SqlClientFactory>, puede pasar `GetFactory` una cadena con el nombre de proveedor especificado como "**Microsoft.Data.SqlClient**".

La otra sobrecarga de `GetFactory` toma <xref:System.Data.DataRow>. Una vez creado el generador del proveedor, se pueden utilizar sus métodos para crear objetos adicionales. Entre los métodos de `SqlClientFactory` se incluyen <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateConnection%2A>, <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateCommand%2A> y <xref:Microsoft.Data.SqlClient.SqlClientFactory.CreateDataAdapter%2A>.

## <a name="register-sqlclientfactory"></a>Register SqlClientFactory

Para recuperar el objeto <xref:Microsoft.Data.SqlClient.SqlClientFactory> por la clase <xref:System.Data.Common.DbProviderFactories> en .NET Framework, es necesario registrarlo en un archivo **App.config** o **web.config**. El siguiente fragmento del archivo de configuración muestra la sintaxis y formato de <xref:Microsoft.Data.SqlClient>.  

```xml  
<system.data>
  <DbProviderFactories>
    <add name="Microsoft SqlClient Data Provider"
      invariant="Microsoft.Data.SqlClient"
      description="Microsoft SqlClient Data Provider for SQL Server"
      type="Microsoft.Data.SqlClient.SqlClientFactory, Microsoft.Data.SqlClient, Version=2.0.20168.4, Culture=neutral, PublicKeyToken=23ec7fc2d6eaa4a5"/>
  </DbProviderFactories>
</system.data>  
```  

El atributo **invariant** identifica el proveedor de datos subyacente. Esta sintaxis de nomenclatura en tres partes también se utiliza al crear un nuevo generador y para identificar al proveedor en un archivo de configuración de la aplicación, de manera que el nombre de proveedor, junto con sus cadenas de conexión asociadas, se puedan recuperar en tiempo de conexión.  

> [!NOTE]  
> En .NET Core, dado que no hay compatibilidad de configuración global o GAC, el objeto <xref:Microsoft.Data.SqlClient.SqlClientFactory> se debe registrar llamando al método <xref:System.Data.Common.DbProviderFactories.RegisterFactory%2A> en el proyecto.

En el ejemplo siguiente se muestra cómo usar <xref:Microsoft.Data.SqlClient.SqlClientFactory> en una aplicación .NET Core.

[!code-csharp[SqlClientFactory_Netcoreapp#1](~/../sqlclient/doc/samples/SqlClientFactory_Netcoreapp.cs#1)]

## <a name="see-also"></a>Consulte también

- [Objetos DbProviderFactory](dbproviderfactories.md)
- [Cadenas de conexión](connection-strings.md)
- [Uso de las clases de configuración](/previous-versions/aspnet/ms228063(v=vs.100))
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)
