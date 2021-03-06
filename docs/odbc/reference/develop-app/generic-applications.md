---
description: Aplicaciones genéricas
title: Aplicaciones genéricas | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- interoperability [ODBC], generic applications
- interoperability [ODBC], levels
- generic applications [ODBC]
ms.assetid: dda2a3c4-76ef-40a6-b3a1-9e95bed61618
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9980edcaf34182b4c7f43aa8e935d095f2345138
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2020
ms.locfileid: "88476687"
---
# <a name="generic-applications"></a>Aplicaciones genéricas
A veces, las aplicaciones genéricas realizan una tarea codificada de forma rígida, como una hoja de cálculo que recupera datos de una base de datos. También pueden realizar una serie de tareas definidas por el usuario, como una aplicación de consulta genérica que permite al usuario escribir y ejecutar una instrucción SQL. Lo habitual de las aplicaciones genéricas es que deben funcionar con una variedad de DBMS diferentes y que el desarrollador no sabe de antemano qué serán estos DBMS.  
  
 Por lo tanto, las aplicaciones genéricas deben ser altamente interoperables. El desarrollador debe tomar muchas decisiones, desmarcando la interoperabilidad de las características y debe escribir código que espera que los controladores admitan una amplia gama de funcionalidades. Aunque es posible que las aplicaciones genéricas estén optimizadas para trabajar con DBMS populares, rara vez contienen código específico del controlador o específico del DBMS.
