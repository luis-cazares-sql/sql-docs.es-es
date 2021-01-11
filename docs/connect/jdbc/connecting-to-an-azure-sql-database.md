---
title: Conectarse a una base de datos de SQL Azure
description: En este artículo se describen los problemas que se producen al usar Microsoft JDBC Driver para SQL Server para conectarse a una instancia de Azure SQL Database.
ms.custom: ''
ms.date: 12/18/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 49645b1f-39b1-4757-bda1-c51ebc375c34
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 03768a309ac10fc16fd1a743660df6fe74b088e7
ms.sourcegitcommit: bc8474fa200ef0de7498dbb103bc76e3e3a4def4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2020
ms.locfileid: "97709674"
---
# <a name="connecting-to-an-azure-sql-database"></a>Conectarse a una base de datos de SQL Azure

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

En este artículo se tratan los problemas de uso de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] para conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)]. Para más información sobre la conexión a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], vea:  
  
- [Azure SQL Database](/azure/sql-database/sql-database-technical-overview)  
  
- [Cómo: Conexión a Azure SQL mediante JDBC](/azure/sql-database/sql-database-connect-query-java)  

- [Conectarse usando la autenticación de Azure Active Directory](connecting-using-azure-active-directory-authentication.md)  
  
## <a name="details"></a>Detalles

Al conectarse a [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], debe conectarse a la base de datos maestra para llamar a **SQLServerDatabaseMetaData.getCatalogs**.  
[!INCLUDE[ssAzure](../../includes/ssazure_md.md)] no es compatible con la devolución de todo el conjunto de catálogos de una base de datos de usuario. **SQLServerDatabaseMetaData.getCatalogs** usa la vista sys.databases para obtener los catálogos. Consulte la explicación de los permisos en [sys.databases (Transact-SQL)](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) para entender el comportamiento de **SQLServerDatabaseMetaData.getCatalogs** en una instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)].  
  
## <a name="connections-dropped"></a>Conexiones eliminadas

Al conectar con una instancia de [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], un componente de red (como un firewall) puede terminar las conexiones inactivas tras un período de inactividad. Existen dos tipos de conexiones inactivas en este contexto:  

- Inactiva en la capa de TCP, donde cualquier número de dispositivos de red puede quitar las conexiones.  

- Inactiva por la puerta de enlace de SQL Azure, donde pueden darse mensajes **keepalive** de TCP (pasando a no ser una conexión inactiva desde una perspectiva de TCP) y no tener una consulta activa en 30 minutos. En este escenario, la puerta de enlace determinará si la conexión TDS está inactiva después de 30 minutos y la finalizará.  
  
Para solucionar el segundo punto y evitar que la puerta de enlace termine las conexiones inactivas, puede:

* Usar la [directiva de conexión](/azure/azure-sql/database/connectivity-architecture#connection-policy) **Redirect** al configurar el origen de datos de Azure SQL.

* Mantenga las conexiones activas a través de la actividad ligera. Este método no se recomienda y solo debe usarse si no hay otras opciones posibles.

Para abordar el primer punto y evitar que un componente de red elimine las conexiones inactivas, se debe establecer la siguiente configuración del Registro (o su equivalente si no es Windows) en el sistema operativo donde esté cargado el controlador:  
  
|Configuración del registro|Valor recomendado|  
|----------------------|-----------------------|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveTime|30000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ KeepAliveInterval|1000|  
|HKEY_LOCAL_MACHINE \ SYSTEM \ CurrentControlSet \ Services \ Tcpip \ Parameters \ TcpMaxDataRetransmissions|10|  
  
Reinicie el equipo para que surta efecto la configuración del Registro.  

Los valores KeepAliveTime y KeepAliveInterval se indican en milisegundos. Esta configuración tendrá el efecto de desconectar una conexión que no responde en un plazo de 10 a 40 segundos. Después de enviar un paquete de mantenimiento de conexión, si no se recibe ninguna respuesta, se volverá a intentar cada segundo hasta diez veces. Si no se recibe ninguna respuesta durante ese tiempo, el socket del lado cliente se desconecta. Dependiendo del entorno, puede que desee aumentar el valor de KeepAliveInterval para adaptarse a interrupciones conocidas (por ejemplo, migraciones de máquinas virtuales) que podrían hacer que un servidor deje de responder durante más de diez segundos.

> [!NOTE]
> TcpMaxDataRetransmissions no es controlable en Windows Vista o Windows 2008 y versiones posteriores.

Para llevar a cabo esta configuración cuando se ejecuta en Azure, cree una tarea de inicio para agregar las claves del Registro.  Por ejemplo, agregue la siguiente tarea de inicio al archivo de definición de servicios:  


```xml
<Startup>  
    <Task commandLine="AddKeepAlive.cmd" executionContext="elevated" taskType="simple">  
    </Task>  
</Startup>  
```

A continuación, agregue un archivo AddKeepAlive.cmd al proyecto. Establezca la configuración "Copy to Output Directory" en Copy always. El siguiente script es un archivo AddKeepAlive.cmd de ejemplo:  

```bat
if exist keepalive.txt goto done  
time /t > keepalive.txt  
REM Workaround for JDBC keep alive on Azure SQL  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveTime /t REG_DWORD /d 30000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v KeepAliveInterval /t REG_DWORD /d 1000 >> keepalive.txt  
REG ADD HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters /v TcpMaxDataRetransmissions /t REG_DWORD /d 10 >> keepalive.txt  
shutdown /r /t 1  
:done  
```

## <a name="appending-the-server-name-to-the-userid-in-the-connection-string"></a>Anexar el nombre de servidor al identificador de usuario en la cadena de conexión  

Antes de la versión 4.0 de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], al conectarse a una [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], se tenía que anexar el nombre de servidor al identificador de usuario en la cadena de conexión. Por ejemplo, user@servername. A partir de la versión 4.0 de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], ya no es necesario anexar @servername al identificador de usuario en la cadena de conexión.  

## <a name="using-encryption-requires-setting-hostnameincertificate"></a>El empleo de cifrado exige establecer hostNameInCertificate

Antes de la versión 7.2 de [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], al conectarse a una [!INCLUDE[ssAzure](../../includes/ssazure_md.md)], debe especificar **hostNameInCertificate** si especifica **encrypt=true** (si el nombre del servidor de la cadena de conexión es *shortName*.*domainName*, establezca la propiedad **hostNameInCertificate** en \*.*domainName*.). Esta propiedad es opcional a partir de la versión 7.2 del controlador.

Por ejemplo:

```java
jdbc:sqlserver://abcd.int.mscds.com;databaseName=myDatabase;user=myName;password=myPassword;encrypt=true;hostNameInCertificate=*.int.mscds.com;
```

## <a name="see-also"></a>Consulte también

[Conexión a SQL Server con el controlador JDBC](connecting-to-sql-server-with-the-jdbc-driver.md)
