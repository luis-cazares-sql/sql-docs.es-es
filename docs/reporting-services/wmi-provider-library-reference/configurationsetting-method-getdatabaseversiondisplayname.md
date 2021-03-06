---
description: 'Método de ConfigurationSetting: GetDatabaseVersionDisplayName'
title: Método GetDatabaseVersionDisplayName (WMI) | Microsoft Docs
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: wmi-provider-library-reference
ms.topic: conceptual
helpviewer_keywords:
- GetDatabaseVersionDisplayName method
ms.assetid: e1286424-7043-4f12-a7ad-1cf69e81baa4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 598bc82d266bfff12f085275a03598f2caf6fc48
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88423229"
---
# <a name="configurationsetting-method---getdatabaseversiondisplayname"></a>Método de ConfigurationSetting: GetDatabaseVersionDisplayName
  Obtiene el nombre para mostrar para una cadena de versión de base de datos del servidor de informes determinada.  
  
## <a name="syntax"></a>Sintaxis  
  
```vb  
Public Sub GetDatabaseVersionDisplayName(Version As String, DisplayName As String, ByRef HRESULT As Int32)  
```  
  
```csharp  
public void GetDatabaseVersionDisplayName(string Version, string DisplayName, out Int32 HRESULT);  
```  
  
## <a name="parameters"></a>Parámetros  
 *Versión*  
 Una cadena que contiene la cadena de la versión de una base de datos del servidor de informes.  
  
 *DisplayName*  
 [out] Una cadena que contiene el nombre para mostrar que corresponde a la versión proporcionada.  
  
 *HRESULT*  
 [out] Valor que indica si la llamada se realizó correctamente o no.  
  
## <a name="remarks"></a>Observaciones  
 En la tabla siguiente se muestra la asignación de la versión de la base de datos para la cadena de nombre para mostrar.  
  
|**Versión**|**Versión**|**Nombre para mostrar**|  
|-----------------|-----------------|----------------------|  
|RS 2005 SP2|@DBVersion = "C.0.8.54"|SQL Server 2005 SP2|  
|RS 2005 SP1|@DBVersion = "C.0.8.43"|SQL Server 2005 SP1|  
|RS 2005 RTM|@DBVersion = "C.0.8.40"|SQL Server 2005|  
|RS 2000 SP2|@DBVersion = "C.0.6.54"|SQL Server 2000 SP2|  
|RS 2000 SP1|@DBVersion = "C.0.6.51"|SQL Server 2000 SP1|  
|RS 2000 RTM|@DBVersion = "C.0.6.43"|SQL Server 2000|  
|Revisión||Versión aplicable más cercana|  
  
 Para una *versión* anterior a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2000 se devuelve un valor de HRESULT de ACT_E_BAD_VERSION.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve *HRESULT* que indica si la llamada al método se realizó correctamente o no. Un valor de 0 indica que la llamada al método se realizó correctamente. Un valor distinto de cero indica que se ha producido un error.  
  
## <a name="requirements"></a>Requisitos  
 **Espacio de nombres:** [!INCLUDE[ssRSWMInmspcA](../../includes/ssrswminmspca-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Miembros MSReportServer_ConfigurationSetting](../../reporting-services/wmi-provider-library-reference/msreportserver-configurationsetting-members.md)  
  
  
