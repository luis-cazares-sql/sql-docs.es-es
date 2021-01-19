---
description: Habilitación de Always Encrypted con enclaves seguros para las columnas cifradas existentes
title: Habilitación de Always Encrypted con enclaves seguros para las columnas cifradas existentes | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: 03a57949624897b2cd2e0552892803c21a408b52
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534814"
---
# <a name="enable-always-encrypted-with-secure-enclaves-for-existing-encrypted-columns"></a>Habilitación de Always Encrypted con enclaves seguros para las columnas cifradas existentes 
[!INCLUDE [sqlserver2019-windows-only-asdb](../../../includes/applies-to-version/sqlserver2019-windows-only-asdb.md)]

En este artículo se describe cómo desbloquear la funcionalidad de Always Encrypted con enclaves seguros y habilitar los cálculos de enclave para las columnas cifradas existentes.  

Si tiene columnas existentes cifradas con claves que no están habilitadas para el enclave, puede hacer que las columnas se cifren con claves habilitadas para el enclave. De este modo, podrá usar el enclave seguro en las consultas en las columnas.

Puede habilitar los cálculos de enclave para las columnas cifradas existentes de varias maneras diferentes, en función de lo siguiente:

- **Ámbito/granularidad:** ¿desea habilitar la funcionalidad de enclave para un subconjunto de columnas, o bien para todas las columnas protegidas con una clave maestra de columna especificada?
- **Tamaño de los datos:** ¿cuál es el tamaño de las tablas que incluyen las columnas que desea hacer que estén habilitadas para el enclave?
- ¿Desea también cambiar el tipo de cifrado para sus columnas? Recuerde que solo el cifrado aleatorio admite los cálculos completos (búsqueda de patrones, operadores de comparación). Si su columna se cifra mediante el cifrado determinista, también tendrá que volver a cifrarla con el cifrado aleatorio para desbloquear los cálculos completos.

En las secciones siguientes se describen los tres enfoques para habilitar los enclaves para las columnas existentes.

## <a name="method-1-rotate-the-column-master-key-to-replace-it-with-an-enclave-enabled-column-master-key"></a>Método 1: girar la clave maestra de columna para reemplazarla por una clave maestra de columna habilitada para el enclave
Al reemplazar una antigua clave maestra de columna (que no está habilitada para el enclave) por una nueva clave maestra de columna habilitada para el enclave, se consigue de forma eficaz que todas las claves de cifrado de columna (asociadas a la clave maestra de columna) también estén habilitadas para el enclave.

- Ventajas:
  - No implica que haya que volver a cifrar los datos, por lo que suele ser el enfoque más rápido. Este enfoque se recomienda para las columnas que contienen grandes cantidades de datos, que ya están habilitadas para cálculos completos y que usan cifrado determinista.
  - Puede habilitar la funcionalidad de enclave para varias columnas a escala. Al reemplazar la clave maestra de columna por la clave maestra de columna habilitada para el enclave, se consigue que todas las claves de cifrado de columna y todas las columnas cifradas asociadas a la clave maestra de columna original estén habilitadas para el enclave.
  
- Inconvenientes:
  - No permite cambiar el tipo de cifrado de determinista a aleatorio. Aunque se desbloquea el cifrado en contexto para las columnas cifradas mediante cifrado determinista, no se habilitan los cálculos completos. Deberá volver a cifrar las columnas mediante cifrado aleatorio.
  - No permite convertir de forma selectiva ninguna de las columnas asociadas a una clave maestra de columna especificada.
  - Presenta una sobrecarga de administración de claves. Deberá crear una clave maestra de columna y ponerla a disposición de las aplicaciones que consultan las columnas afectadas.

Para obtener información sobre cómo girar una clave maestra de columna, vea [Rotación de claves habilitadas para el enclave](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-2-rotate-the-column-master-key-and-re-encrypt-columns-using-randomized-encryption-in-place"></a>Método 2: girar la clave maestra de columna y volver a cifrar las columnas mediante cifrado aleatorio en contexto
Este método implica ejecutar el método 1 como primer paso y, después, volver a cifrar las columnas. Las columnas empiezan usando el cifrado determinista y, luego, se vuelven a cifrar con cifrado aleatorio para desbloquear las consultas completas.

- Ventajas:
  - Vuelve a cifrar los datos en contexto. Es un método recomendado si necesita habilitar las consultas completas para las columnas cifradas que usan actualmente el cifrado determinista y contienen grandes cantidades de datos. El paso 1 (la rotación de la clave maestra de columna) desbloquea el cifrado en contexto para las columnas que usan cifrado determinista y el paso 2 (que consiste en volver a cifrar las columnas) se puede realizar en contexto.
  - Puede habilitar la funcionalidad de enclave para varias columnas a escala.
  
- Inconvenientes:
  - No permite convertir de forma selectiva ninguna de las columnas asociadas a una clave maestra de columna especificada.
  - Presenta una sobrecarga de administración de claves. Deberá crear una clave maestra de columna y ponerla a disposición de las aplicaciones que consultan las columnas afectadas.

Para obtener información sobre cómo girar una clave maestra de columna y volver a cifrar una columna en contexto para girar una clave de cifrado de columna, vea [Rotación de claves habilitadas para el enclave](always-encrypted-enclaves-rotate-keys.md).

## <a name="method-3-re-encrypt-a-selected-column-with-an-enclave-enabled-column-encryption-key-on-the-client-side"></a>Método 3: volver a cifrar una columna seleccionada con una clave de cifrado de columna habilitada para el enclave en el lado cliente
Este método implica volver a cifrar una columna con una clave de cifrado de columna habilitada para el enclave y habilitar las consultas completas con cifrado aleatorio. Dado que la clave de cifrado de columna actual no está habilitada para el enclave, no se puede volver a cifrar la columna en contexto. Use el Asistente para Always Encrypted o el cmdlet Set-SqlColumnEncryption para volver a cifrar la columna fuera de la base de datos.

- Ventajas:
  - Permite habilitar de forma selectiva la funcionalidad de enclave (cifrado en contexto y consultas completas, si va a volver a cifrar la columna con cifrado aleatorio) para una columna o un pequeño subconjunto de columnas.
  - Puede habilitar cálculos completos para las columnas en un paso.
  - No requiere la creación de una clave maestra de columna, por lo que tiene un impacto más pequeño en las aplicaciones.
  
- Inconvenientes:
  - Para volver a cifrar los datos, la herramienta los moverá fuera de la base de datos, lo que puede tardar mucho tiempo y es propenso a errores de red.

Para obtener más información sobre cómo girar el cifrado de una columna a través de una herramienta de cliente, vea [Rotación de claves de Always Encrypted con SQL Server Management Studio](rotate-always-encrypted-keys-using-ssms.md) y [Rotación de claves de Always Encrypted con PowerShell](rotate-always-encrypted-keys-using-powershell.md).

## <a name="next-steps"></a>Pasos siguientes
- [Configuración y uso de Always Encrypted con enclaves seguros](always-encrypted-enclaves-query-columns.md)
- [Desarrollo de aplicaciones mediante Always Encrypted con enclaves seguros](always-encrypted-enclaves-client-development.md)
