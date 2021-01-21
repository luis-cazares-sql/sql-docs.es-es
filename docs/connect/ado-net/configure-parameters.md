---
title: Configuración de parámetros
description: Los objetos de comando usan parámetros para pasar valores a instrucciones SQL o procedimientos almacenados, lo que proporciona comprobación y validación de tipos en ADO.NET.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12dfcb26484b468bed9f9262403806ea88829385
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595769"
---
# <a name="configuring-parameters"></a>Configuración de parámetros

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Los objetos de comando usan parámetros para pasar valores a instrucciones SQL o procedimientos almacenados que permiten realizar operaciones de comprobación de tipos y validación. A diferencia del texto de comando, la entrada de parámetros se trata como un valor literal, y no como código ejecutable. De esta forma, se protege contra ataques por "inyección de código SQL", en los que un atacante inserta un comando que pone en peligro la seguridad del servidor en una instrucción SQL.

Los comandos parametrizados también pueden mejorar el rendimiento de ejecución de la consulta, ya que ayudan al servidor de bases de datos a que haga coincidir precisamente el comando entrante con un plan de consulta almacenado en caché adecuado. Para obtener más información, consulte [Almacenar en caché y volver a utilizar un plan de ejecución](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse) y [Parámetros y reutilización de un plan de ejecución](../../relational-databases/query-processing-architecture-guide.md#PlanReuse). Además de las ventajas en la seguridad y el rendimiento, los comandos con parámetros proporcionan un método práctico para organizar los valores que se pasan a un origen de datos.

Para crear un objeto <xref:System.Data.Common.DbParameter> , se puede usar su constructor o bien se puede agregar a <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> mediante una llamada al método `Add` de la colección <xref:System.Data.Common.DbParameterCollection> . El método `Add` acepta como entrada argumentos del constructor o cualquier objeto de parámetro ya existente, en función del proveedor de datos.

## <a name="supply-the-parameterdirection-property"></a>Provisión de la propiedad ParameterDirection

Cuando se agregan parámetros distintos de los parámetros de entrada, se debe proporcionar una propiedad <xref:System.Data.ParameterDirection> . En la tabla siguiente se muestran los valores de `ParameterDirection` que se pueden usar con la enumeración <xref:System.Data.ParameterDirection> .

|Nombre del miembro|Descripción|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|Se trata de un parámetro de entrada. Este es el valor predeterminado.|
|<xref:System.Data.ParameterDirection.InputOutput>|El parámetro se puede comportar tanto de entrada como de salida.|
|<xref:System.Data.ParameterDirection.Output>|Se trata de un parámetro de salida.|
|<xref:System.Data.ParameterDirection.ReturnValue>|El parámetro representa un valor devuelto de una operación como, por ejemplo, un procedimiento almacenado, una función integrada o una función definida por el usuario.|

## <a name="work-with-parameter-placeholders"></a>Trabajo con marcadores de posición de parámetros

La sintaxis de los marcadores de posición de parámetros depende del origen de datos. El proveedor de datos SqlClient de Microsoft para SQL Server controla la asignación de nombres y la especificación de parámetros y marcadores de posición de parámetros de forma diferente. El proveedor de datos SqlClient usa parámetros con nombre con el formato `@`*parametername*.

## <a name="specify-parameter-data-types"></a>Especificación de tipos de datos de parámetro

El tipo de datos de un parámetro es específico del proveedor de datos SqlClient de Microsoft para SQL Server. Al especificar el tipo, el valor de `Parameter` se convierte en el tipo de proveedor de datos SqlClient de Microsoft para SQL Server antes de pasar el valor al origen de datos. Si lo desea, puede especificar el tipo de un objeto `Parameter` de forma genérica estableciendo la propiedad `DbType` del objeto `Parameter` en un <xref:System.Data.DbType>determinado.

El tipo de proveedor de datos SqlClient de Microsoft para SQL Server de un objeto `Parameter` se deduce del tipo .NET Framework de `Value` del objeto `Parameter`, o de `DbType` del objeto `Parameter`. En la siguiente tabla se muestra el tipo deducido de `Parameter` en función del objeto que se ha pasado como valor `Parameter` o del `DbType`especificado.

|Tipo de .NET|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|Boolean|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|Binary|VarBinary. Esta conversión implícita generará un error en el caso de que la matriz de bytes tenga un tamaño superior al tamaño máximo de un tipo VarBinary, que es de 8000 bytes. En matrices de bytes con más de 8000 bytes, establezca de forma explícita el tipo <xref:System.Data.SqlDbType>.|
|<xref:System.Char>| |No se admite la deducción de un tipo <xref:System.Data.SqlDbType> a partir de char.|
|<xref:System.DateTime>|DateTime|DateTime|
|<xref:System.DateTimeOffset>|DateTimeOffset|DateTimeOffset en SQL Server 2008. La deducción de un elemento <xref:System.Data.SqlDbType> a partir de DateTimeOffset no se admite en versiones de SQL Server anteriores a SQL Server 2008.|
|<xref:System.Decimal>|Decimal|Decimal|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Single|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|Int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|Objeto|Variante|
|<xref:System.String>|String|NVarChar. Esta conversión implícita generará un error en el caso de que la cadena tenga un tamaño superior al tamaño máximo de un tipo NVarChar, que es de 4.000 caracteres. En cadenas con más de 4.000 caracteres, establezca de forma explícita el tipo <xref:System.Data.SqlDbType>.|
|<xref:System.TimeSpan>|Hora|Time en SQL Server 2008. La deducción de un elemento <xref:System.Data.SqlDbType> a partir de TimeSpan no se admite en versiones de SQL Server anteriores a SQL Server 2008.|
|<xref:System.UInt16>|UInt16|No se admite la deducción de un tipo <xref:System.Data.SqlDbType> a partir de UInt16.|
|<xref:System.UInt32>|UInt32|No se admite la deducción de un tipo <xref:System.Data.SqlDbType> a partir de UInt32.|
|<xref:System.UInt64>|UInt64|No se admite la deducción de un tipo <xref:System.Data.SqlDbType> a partir de UInt64.|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||Moneda|Money|
||Date|Date en SQL Server 2008. La deducción de un elemento <xref:System.Data.SqlDbType> a partir de Date no se admite en versiones de SQL Server anteriores a SQL Server 2008.|
||SByte|No se admite la deducción de un elemento <xref:System.Data.SqlDbType> a partir de SByte.|
||StringFixedLength|NChar|
||Hora|Time en SQL Server 2008. La deducción de un elemento <xref:System.Data.SqlDbType> a partir de Time no se admite en versiones de SQL Server anteriores a SQL Server 2008.|
||VarNumeric|No se admite la deducción de un elemento <xref:System.Data.SqlDbType> a partir de VarNumeric.|
|tipo definido por el usuario (un objeto con <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>|SqlClient siempre devuelve un objeto|`SqlDbType.Udt` si <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> está presente; de lo contrario, `Variant`.|

> [!NOTE]
> Las conversiones de valores de tipo decimal en otros tipos de valor son conversiones de restricción que redondean el valor decimal al valor entero más próximo a cero. Si el resultado de la conversión no puede representarse en el tipo de destino, se produce <xref:System.OverflowException> .

> [!NOTE]
> Cuando se envía un valor de parámetro nulo al servidor, se debe especificar <xref:System.DBNull>, no `null` (`Nothing` en Visual Basic). El valor nulo en el sistema es un objeto vacío que no tiene ningún valor. Para representar los valores nulos, se usa<xref:System.DBNull> .

## <a name="derive-parameter-information"></a>Derivación de información de parámetro

Los parámetros también se pueden derivar de un procedimiento almacenado mediante la clase `DbCommandBuilder` . La clase `SqlCommandBuilder` proporciona un método estático, `DeriveParameters`, que rellena automáticamente la colección de parámetros de un objeto de comando que usa información de parámetros de un procedimiento almacenado. Tenga en cuenta que `DeriveParameters` sobrescribirá toda la información de parámetros existente en el comando.

> [!NOTE]
> La derivación de información de parámetros afecta al rendimiento, ya que precisa un viaje adicional de ida y vuelta (round trip) al origen de datos para recuperar la información. Si la información de los parámetros se conoce en tiempo de diseño, se puede mejorar el rendimiento de la aplicación si se establecen los parámetros con los valores correspondientes de forma explícita.

Para obtener más información, vea [Generar comandos con objetos CommandBuilder](generate-commands-with-commandbuilders.md).

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>Uso de parámetros con SqlCommand y un procedimiento almacenado

Los procedimientos almacenados ofrecen numerosas ventajas en el caso de aplicaciones que procesan datos. Mediante el uso de procedimientos almacenados, las operaciones de bases de datos se pueden encapsular en un solo comando, optimizar para lograr el mejor rendimiento, y mejorar con seguridad adicional. Aunque es cierto que para llamar a un procedimiento almacenado basta con pasar en forma de instrucción SQL su nombre seguido de los argumentos de parámetros, el uso de la colección <xref:System.Data.Common.DbCommand.Parameters%2A> del objeto <xref:System.Data.Common.DbCommand> de ADO.NET permite definir más explícitamente los parámetros del procedimiento almacenado, y tener acceso a los parámetros de salida y a los valores devueltos.

> [!NOTE]
> Las instrucciones con parámetros se ejecutan en el servidor utilizando `sp_executesql,` ; esto permite volver a utilizar el plan de consultas. Los cursores o las variables locales del lote de `sp_executesql` no son visibles para el lote que llama a `sp_executesql`. Los cambios en el contexto de base de datos solo se mantienen hasta el final de la instrucción `sp_executesql` . Para más información, consulte [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md).

Cuando se usan parámetros con <xref:Microsoft.Data.SqlClient.SqlCommand> para ejecutar un procedimiento almacenado de SQL Server, los nombres de los parámetros agregados a la colección <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> deben coincidir con los nombres de los marcadores de parámetro del procedimiento almacenado. El proveedor de datos SqlClient de Microsoft para SQL Server no admite el marcador de posición del signo de interrogación (?) para pasar parámetros a una instrucción SQL o a un procedimiento almacenado. Este proveedor trata los parámetros del procedimiento almacenado como parámetros con nombre y busca marcadores de parámetros coincidentes. Por ejemplo, el procedimiento almacenado `CustOrderHist` se define usando un parámetro denominado `@CustomerID`. Cuando el código ejecuta el procedimiento almacenado, también debe usar un parámetro denominado `@CustomerID`.

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>Ejemplo

En este ejemplo se muestra cómo llamar a un procedimiento almacenado de SQL Server en la base de datos de ejemplo `Northwind` . El nombre del procedimiento almacenado es `dbo.SalesByCategory` e incluye un parámetro de entrada denominado `@CategoryName` con el tipo de datos `nvarchar(15)`. El código crea una nueva clase <xref:Microsoft.Data.SqlClient.SqlConnection> dentro de un bloque en uso, de forma que la conexión se cierre cuando finalice el procedimiento. Se crean los objetos <xref:Microsoft.Data.SqlClient.SqlCommand> y <xref:Microsoft.Data.SqlClient.SqlParameter> , y se establecen sus propiedades. <xref:Microsoft.Data.SqlClient.SqlDataReader> ejecuta `SqlCommand` y devuelve el conjunto de resultados del procedimiento almacenado, mostrándolos en la ventana de consola.

> [!NOTE]
> En lugar de crear objetos `SqlCommand` y `SqlParameter` y, a continuación, establecer propiedades en instrucciones independientes, puede usar uno de los constructores sobrecargados para establecer varias propiedades en una única instrucción.

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>Vea también

- [Comandos y parámetros](commands-parameters.md)
- [Objetos DataAdapter y DataReader](dataadapters-datareaders.md)
- [Asignaciones de tipos de datos en ADO.NET](data-type-mappings-ado-net.md)
- [Microsoft ADO.NET para SQL Server](microsoft-ado-net-sql-server.md)