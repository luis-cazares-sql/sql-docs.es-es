---
description: CREATE ASSEMBLY (Transact-SQL)
title: CREATE ASSEMBLY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ASSEMBLY
- CREATE ASSEMBLY
- CREATE_ASSEMBLY_TSQL
- ASSEMBLY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], validating
- validating assemblies
- CREATE ASSEMBLY statement
- assemblies [CLR integration], creating
ms.assetid: d8d1d245-c2c3-4325-be52-4fc1122c2079
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 13b639ace60f98ce9ab55cc0280130131f6851ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098413"
---
# <a name="create-assembly-transact-sql"></a>CREATE ASSEMBLY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md )]

  Crea un módulo de aplicación administrada que contiene metadatos de clase y código administrado como un objeto en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mediante este módulo, puede crear en la base de datos funciones CLR (Common Language Runtime), procedimientos almacenados, desencadenadores, funciones de agregado definidas por el usuario y tipos definidos por el usuario.  
  
> [!WARNING]
>  CLR usa la seguridad de acceso del código (CAS) de .NET Framework, que ya no se admite como un límite de seguridad. Un ensamblado CLR creado con la opción `PERMISSION_SET = SAFE` puede tener acceso a los recursos externos del sistema, llamar a código no administrado y adquirir privilegios sysadmin. A partir de [!INCLUDE[sssqlv14-md](../../includes/sssqlv14-md.md)], se incluye una opción de `sp_configure` denominada `clr strict security` para mejorar la seguridad de los ensamblados CLR. La opción `clr strict security` está habilitada de forma predeterminada y trata los ensamblados `SAFE` y `EXTERNAL_ACCESS` como si estuvieran marcados con `UNSAFE`. La opción `clr strict security` se puede deshabilitar para permitir la compatibilidad con versiones anteriores, pero no se recomienda hacerlo. Microsoft recomienda que todos los ensamblados estén firmados con un certificado o clave asimétrica con el correspondiente inicio de sesión que tenga concedido el permiso `UNSAFE ASSEMBLY` en la base de datos maestra. Para obtener más información, vea [CLR strict security](../../database-engine/configure-windows/clr-strict-security.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE ASSEMBLY assembly_name  
[ AUTHORIZATION owner_name ]  
FROM { <client_assembly_specifier> | <assembly_bits> [ ,...n ] }  
[ WITH PERMISSION_SET = { SAFE | EXTERNAL_ACCESS | UNSAFE } ]  
[ ; ]  
<client_assembly_specifier> :: =  
        '[\\computer_name\]share_name\[path\]manifest_file_name'  
  | '[local_path\]manifest_file_name'  
  
<assembly_bits> :: =  
{ varbinary_literal | varbinary_expression }  
```  
  
## <a name="arguments"></a>Argumentos  
 *assembly_name*  
 Es el nombre del ensamblado. El nombre debe ser único en la base de datos y un [identificador](../../relational-databases/databases/database-identifiers.md) válido.  
  
 AUTHORIZATION *owner_name*  
 Especifica el nombre de un usuario o rol como propietario del ensamblado. *owner_name* debe ser el nombre de un rol del que el usuario actual sea miembro, o bien el usuario actual debe tener el permiso IMPERSONATE sobre *owner_name*. Si no se especifica, la propiedad se otorga al usuario actual.  
  
 \<client_assembly_specifier>  
Especifica la ruta de acceso local o ubicación de red en la que se encuentra el ensamblado que se carga y, además, el nombre de archivo de manifiesto que se corresponde con el ensamblado.  \<client_assembly_specifier> se puede expresar como una cadena fija o una expresión que se evalúe como una cadena fija, con variables. CREATE ASSEMBLY no admite la carga de ensamblados de varios módulos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también busca cualquier ensamblado dependiente de este ensamblado en la misma ubicación y lo carga con el mismo propietario que el ensamblado de nivel raíz. Si estos ensamblados dependientes no se encuentran o aún no están cargados en la base de datos actual, CREATE ASSEMBLY produce un error. Si los ensamblados dependientes ya están cargados en la base de datos actual, el propietario de los mismos deberá ser el mismo que el del ensamblado nuevo que se crea.

> [!IMPORTANT]
> Azure SQL Database no admite la creación de un ensamblado desde un archivo.
  
 No se puede especificar \<client_assembly_specifier> si el usuario que ha iniciado sesión es suplantado.  
  
 \<assembly_bits>  
 Es la lista de valores binarios que forman el ensamblado y sus ensamblados dependientes. El primer valor de esta lista se considera el ensamblado raíz. Los valores correspondientes a los ensamblados dependientes pueden suministrarse en cualquier orden. Se ignora cualquier valor que no se corresponda con los ensamblados dependientes del ensamblado raíz.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 *varbinary_literal*  
 Es un literal **varbinary**.  
  
 *expresión_varbinary*  
 Es una expresión de tipo **varbinary**.  
  
 PERMISSION_SET { **SAFE** | EXTERNAL_ACCESS | UNSAFE }  
> [!IMPORTANT]
>  La opción `PERMISSION_SET` se ve afectada por la opción `clr strict security`, descrita en la advertencia inicial. Cuando `clr strict security` está habilitada, todos los ensamblados se tratan como `UNSAFE`.
 
 Especifica un conjunto de permisos de acceso a código que se conceden al ensamblado cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tiene acceso al mismo. Si no se especifica, el valor SAFE se aplica de forma predeterminada.  
  
 Es recomendable el uso de SAFE. SAFE es el conjunto de permisos más restrictivo. El código que ejecuta un ensamblado con permisos SAFE no puede tener acceso a recursos externos del sistema, como archivos, la red, variables de entorno o el registro.  
  
 EXTERNAL_ACCESS habilita que los ensamblados tengan acceso a determinados recursos externos del sistema, como archivos, redes, variables de entorno y el Registro.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
 UNSAFE habilita que los ensamblados tengan acceso ilimitado a los recursos, tanto dentro como fuera de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El código que se ejecuta desde dentro de un ensamblado UNSAFE puede llamarse código no administrado.  
  
> [!NOTE]  
>  Esta opción no está disponible en las bases de datos independientes.  
  
> [!IMPORTANT]  
>  SAFE es el ajuste de permiso recomendado para ensamblados que realizan tareas de administración de datos y cálculos sin tener acceso a los recursos fuera de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
>   
>  Es recomendable el uso de EXTERNAL_ACCESS para ensamblados que tengan acceso a recursos fuera de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los ensamblados EXTERNAL_ACCESS incluyen la protección de escalabilidad y confiabilidad ofrecida por los ensamblados SAFE; sin embargo, desde el punto de vista de la seguridad, son similares a los ensamblados UNSAFE. Esto se debe a que el código en los ensamblados EXTERNAL_ACCESS se ejecuta de forma predeterminada con la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y tiene acceso a los recursos externos con esa misma cuenta, a no ser que el código suplante al autor de la llamada de forma explícita. Por esta razón, el permiso para crear ensamblados EXTERNAL_ACCESS solo debe concederse a los inicios de sesión en los que se confíe para ejecutar código con la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información sobre la suplantación, vea [Seguridad de la integración CLR](../../relational-databases/clr-integration/security/clr-integration-security.md).  
>   
>  Si especifica UNSAFE, otorga libertad absoluta al código en el ensamblado para realizar operaciones en el espacio de procesos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que pueden comprometer la solidez de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Los ensamblados UNSAFE también pueden trastornar potencialmente el sistema de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o de Common Language Runtime. Los permisos UNSAFE solo deben otorgarse a los ensamblados de gran confianza. Solo los miembros del rol fijo de servidor **sysadmin** pueden crear y modificar los ensamblados UNSAFE.  
  
 Para obtener más información sobre los conjuntos de permisos de ensamblado, vea [Diseño de ensamblados](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="remarks"></a>Observaciones  
 CREATE ASSEMBLY carga un ensamblado que ha sido compilado anteriormente como un archivo .dll desde código administrado para su uso dentro de una instancia de SQL Server.  
 
Cuando se habilita, la opción `PERMISSION_SET` en las instrucciones `CREATE ASSEMBLY` y `ALTER ASSEMBLY` se omite en tiempo de ejecución, pero las opciones `PERMISSION_SET` se conservan en los metadatos. Si la opción se omite, se reduce el riesgo de romper las instrucciones de código existentes.
 
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no permite registrar distintas versiones de un ensamblado con el mismo nombre, referencia cultural y clave pública.  
  
Cuando se intenta obtener acceso al ensamblado especificado en \<client_assembly_specifier>, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suplanta el contexto de seguridad del inicio de sesión de Windows actual. Si \<client_assembly_specifier> especifica una ubicación de red (ruta de acceso UNC), la suplantación del inicio de sesión actual no se mantiene en la ubicación de red debido a las limitaciones de delegación. En este caso, el acceso se realiza mediante el contexto de seguridad de la cuenta del servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, vea [Credenciales &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md).
  
 Además del ensamblado raíz especificado por *assembly_name*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta cargar cualquier ensamblado al que haga referencia el ensamblado raíz que se carga. Si ya se ha cargado en la base de datos un ensamblado referenciado por la existencia de una instrucción CREATE ASSEMBLY anterior, este ensamblado no se carga, sino que está disponible para el ensamblado raíz. Si no se ha cargado antes un ensamblado dependiente, pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede localizar el archivo de manifiesto en el directorio de origen, CREATE ASSEMBLY produce un error.  
  
 Si un ensamblado dependiente referenciado por el ensamblado raíz no está aún en la base de datos y se ha cargado implícitamente junto con el ensamblado raíz, este ensamblado tiene establecidos los mismos permisos que el ensamblado raíz. Si se deben crear ensamblados dependientes mediante el uso de permisos diferentes de los del ensamblado raíz, estos deben cargarse explícitamente antes del ensamblado raíz con los permisos apropiados establecidos.  
  
## <a name="assembly-validation"></a>Validación del ensamblado  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza comprobaciones en los binarios del ensamblado cargados mediante la instrucción CREATE ASSEMBLY para garantizar que lo siguiente se cumple:  
  
-   El binario del ensamblado está formado por metadatos y segmentos de código válidos; los segmentos de código tienen instrucciones MSIL (Lenguaje intermedio de Microsoft) válidas.  
  
-   El conjunto de ensamblados del sistema al que se hace referencia es uno de los siguientes ensamblados admitidos en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: Microsoft.Visualbasic.dll, Mscorlib.dll, System.Data.dll, System.dll, System.Xml.dll, Microsoft.Visualc.dll, Custommarshallers.dll, System.Security.dll, System.Web.Services.dll, System.Data.SqlXml.dll, System.Core.dll y System.Xml.Linq.dll. Se puede hacer referencia a otros ensamblados del sistema, pero deben estar registrados de forma explícita en la base de datos.  
  
-   Para ensamblados creados mediante los conjuntos de permisos SAFE o EXTERNAL ACCESS:  
  
    -   El código del ensamblado debe tener seguridad de tipos. La seguridad de tipos se establece ejecutando el comprobador de Common Language Runtime en el ensamblado.  
  
    -   El ensamblado no debe contener ningún miembro de datos estáticos en sus clases, a menos que esté marcado como de solo lectura.  
  
    -   Las clases en el ensamblado no pueden contener métodos de finalizador.  
  
    -   Las clases o métodos del ensamblado deben anotarse solamente con atributos de código permitidos. Para obtener más información, vea [Atributos personalizados para las rutinas CLR](../../relational-databases/clr-integration/database-objects/clr-integration-custom-attributes-for-clr-routines.md).  
  
 Además de las comprobaciones previas realizadas al ejecutar CREATE ASSEMBLY, existen comprobaciones adicionales que se realizan en el tiempo de ejecución del código en el ensamblado:  
  
-   Se puede producir un error en la llamada a determinadas API de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] que requieren un permiso de acceso a código específico si el conjunto de permisos del ensamblado no incluye ese permiso.  
  
-   En el caso de ensamblados SAFE y EXTERNAL_ACCESS, cualquier intento de llamada a las API [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] anotadas con HostProtectionAttributes dará error.  
  
 Para obtener más información, vea [Diseño de ensamblados](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE ASSEMBLY.  
  
 Si se especifica PERMISSION_SET = EXTERNAL_ACCESS, se requiere el permiso **EXTERNAL ACCESS ASSEMBLY** en el servidor. Si se especifica PERMISSION_SET = UNSAFE, se requiere el permiso **UNSAFE ASSEMBLY** en el servidor.  
  
 El usuario debe ser el propietario de cualquier ensamblado al que el ensamblado que se va a cargar haga referencia (si los ensamblados ya existen en la base de datos). Para cargar un ensamblado mediante una ruta de acceso de archivo, el usuario actual debe ser miembro del inicio de sesión autenticado de Windows o del rol fijo de servidor **sysadmin**. El inicio de sesión de Windows del usuario que ejecuta CREATE ASSEMBLY debe tener permisos de lectura en el recurso compartido y en los archivos que se cargan por medio de la instrucción.  

### <a name="permissions-with-clr-strict-security"></a>Permisos con seguridad estricta de CLR    
Los siguientes permisos son necesarios para crear un ensamblado CLR cuando la opción `CLR strict security` está habilitada:

- El usuario debe tener el permiso `CREATE ASSEMBLY`.  
- Además, se debe dar una de las siguientes condiciones:  
  - El ensamblado debe estar firmado con un certificado o clave asimétrica que tenga el inicio de sesión correspondiente con el permiso `UNSAFE ASSEMBLY` en el servidor. Se recomienda firmar el ensamblado.  
  - La base de datos tiene la propiedad `TRUSTWORTHY` establecida en `ON` y pertenece a un inicio de sesión que tiene el permiso `UNSAFE ASSEMBLY` en el servidor. Esta opción no se recomienda.  
  
 Para obtener más información sobre los conjuntos de permisos de ensamblado, vea [Diseño de ensamblados](../../relational-databases/clr-integration/assemblies-designing.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="example-a-creating-an-assembly-from-a-dll"></a>Ejemplo A: creación de un ensamblado desde un archivo dll  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 En el ejemplo siguiente se asume que los ejemplos del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] se han instalado en la ubicación predeterminada del equipo local y que la aplicación de ejemplo HelloWorld.csproj se ha compilado. Para más información, vea [Ejemplo de Hola a todos](/previous-versions/sql/sql-server-2016/ff878250(v=sql.130)).  
  
```sql  
CREATE ASSEMBLY HelloWorld   
FROM '<system_drive>:\Program Files\Microsoft SQL Server\100\Samples\HelloWorld\CS\HelloWorld\bin\debug\HelloWorld.dll'  
WITH PERMISSION_SET = SAFE;  
```  

> [!IMPORTANT]
> Azure SQL Database no admite la creación de un ensamblado desde un archivo.
  
### <a name="example-b-creating-an-assembly-from-assembly-bits"></a>Ejemplo B: creación de un ensamblado a partir de bits de ensamblado  
  
**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.  
  
 Reemplace los bits de ejemplo (que no son completos o válidos) con sus bits de ensamblado.  
  
```sql  
CREATE ASSEMBLY HelloWorld  
    FROM 0x4D5A900000000000  
WITH PERMISSION_SET = SAFE;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-assembly-transact-sql.md)   
 [DROP ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-assembly-transact-sql.md)   
 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)   
 [CREATE AGGREGATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-aggregate-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [Escenarios de uso y ejemplos para la integración de Common Language Runtime &#40;CLR&#41;](/previous-versions/sql/sql-server-2016/ms131078(v=sql.130))  
  
