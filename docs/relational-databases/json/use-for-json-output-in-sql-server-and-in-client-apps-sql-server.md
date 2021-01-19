---
description: Uso de salidas FOR JSON en SQL Server y en aplicaciones cliente (SQL Server)
title: Uso de salidas FOR JSON en SQL Server y en aplicaciones cliente
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, using in client apps
- FOR JSON, using in SQL Server
ms.assetid: 302e5397-b499-4ea3-9a7f-c24ccad698eb
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4bd5911552144084550abdadb3d432ab08f3bf9a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172507"
---
# <a name="use-for-json-output-in-sql-server-and-in-client-apps-sql-server"></a>Uso de salidas FOR JSON en SQL Server y en aplicaciones cliente (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

En los siguientes ejemplos, se muestran algunas de las formas de usar la cláusula **FOR JSON** o su salida JSON en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en aplicaciones cliente.  
  
## <a name="use-for-json-output-in-sql-server-variables"></a>Uso de salidas FOR JSON en variables de SQL Server  
La salida de la cláusula FOR JSON es de tipo NVARCHAR(MAX), así que se puede asignar a cualquier variable, como se muestra en el ejemplo siguiente.  
  
```sql  
DECLARE @x NVARCHAR(MAX) =
  (SELECT TOP 10 *
     FROM Sales.SalesOrderHeader
     FOR JSON AUTO)  
```  
  
## <a name="use-for-json-output-in-sql-server-user-defined-functions"></a>Uso de salidas FOR JSON en funciones definidas por el usuario de SQL Server  
 Puede crear funciones definidas por el usuario que formatean conjuntos de resultados como JSON y devuelven esta salida JSON. En el ejemplo siguiente se crea una función definida por el usuario que recupera algunas filas de detalle de pedido de ventas y las formatea como una matriz JSON.  
  
```sql  
CREATE FUNCTION GetSalesOrderDetails(@salesOrderId int)  
 RETURNS NVARCHAR(MAX)  
AS  
BEGIN  
   RETURN (SELECT UnitPrice, OrderQty  
           FROM Sales.SalesOrderDetail  
           WHERE SalesOrderID = @salesOrderId  
           FOR JSON AUTO)  
END
```  
  
 Puede utilizar esta función en un lote o una consulta, como se muestra en el ejemplo siguiente.  
  
```sql  
DECLARE @x NVARCHAR(MAX) = dbo.GetSalesOrderDetails(43659)

PRINT dbo.GetSalesOrderDetails(43659)

SELECT TOP 10
  H.*, dbo.GetSalesOrderDetails(H.SalesOrderId) AS Details
FROM Sales.SalesOrderHeader H
```  
  
## <a name="merge-parent-and-child-data-into-a-single-table"></a>Combinación de datos primarios y secundarios en una sola tabla  
En el ejemplo siguiente, a cada conjunto de filas secundarias se le aplica el formato de una matriz JSON. La matriz JSON se convierte en el valor de la columna Detalles de la tabla principal.  
  
```sql  
SELECT TOP 10 SalesOrderId, OrderDate,  
      (SELECT TOP 3 UnitPrice, OrderQty  
         FROM Sales.SalesOrderDetail D  
         WHERE H.SalesOrderId = D.SalesOrderID  
         FOR JSON AUTO) AS Details  
INTO SalesOrder  
FROM Sales.SalesOrderHeader H  
```  
  
## <a name="update-the-data-in-json-columns"></a>Actualización de los datos en columnas JSON  
 En el ejemplo siguiente, se muestra que se puede actualizar el valor de una columna que contiene texto JSON.  
  
```sql  
UPDATE SalesOrder  
SET Details =  
     (SELECT TOP 1 UnitPrice, OrderQty  
       FROM Sales.SalesOrderDetail D  
       WHERE D.SalesOrderId = SalesOrder.SalesOrderId  
      FOR JSON AUTO) 
```  
  
## <a name="use-for-json-output-in-a-c-client-app"></a>Uso de salidas FOR JSON en una aplicación cliente de C#  
 En el ejemplo siguiente se muestra cómo recuperar la salida JSON de una consulta en un objeto StringBuilder en una aplicación cliente de C#. Supongamos que la variable `queryWithForJson` contiene el texto de una instrucción SELECT con una cláusula FOR JSON.  
  
```csharp  
var queryWithForJson = "SELECT ... FOR JSON";
using(var conn = new SqlConnection("<connection string>"))
{
    using(var cmd = new SqlCommand(queryWithForJson, conn))
    {
        conn.Open();
        var jsonResult = new StringBuilder();
        var reader = cmd.ExecuteReader();
        if (!reader.HasRows)
        {
            jsonResult.Append("[]");
        }
        else
        {
            while (reader.Read())
            {
                jsonResult.Append(reader.GetValue(0).ToString());
            }
        }
    }
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
 
## <a name="see-also"></a>Consulte también  
 [Format Query Results as JSON with FOR JSON &#40;SQL Server&#41; (Dar formato JSON a los resultados de consulta con FOR JSON &#40;SQL Server&#41;)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
