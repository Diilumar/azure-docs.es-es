---
title: Lista de tareas de administración y desarrollo de BizTalk Services | Microsoft Docs
description: Ayuda de planeación y de trabajo para implementar Azure BizTalk Services.
services: biztalk-services
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.openlocfilehash: e762141e089b11dd0fb129f3bf758874d4ad4da8
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227644"
---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Lista de tareas de administración y desarrollo de BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

> [!INCLUDE [Use APIs to manage MABS](../../includes/biztalk-services-retirement-azure-classic-portal.md)]

## <a name="getting-started"></a>Introducción
Cuando se trabaja con Microsoft Azure BizTalk Services, hay varios componentes locales y basados en la nube que hay que tener en cuenta. Para empezar, piense en el siguiente flujo de proceso:  

| Paso | Quién es el responsable | Task | Vínculos relacionados |
| --- | --- | --- | --- |
| 1. |Administrador |Creación de la suscripción de Microsoft Azure con una cuenta de Microsoft o una cuenta de la organización |[Azure Portal](https://portal.azure.com) |
| 2. |Administrador |Cree o aprovisione un servicio de BizTalk. |[Creación de una instancia de BizTalk Services](https://msdn.microsoft.com/library/azure/dn232347.aspx) |
| 3. |Administrador |Registrarse a usted o registrar la implementación de BizTalk Services de su empresa |[Registrar y actualizar una implementación del servicio de BizTalk en el Portal de BizTalk Services](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Administrador |Se aplica si la aplicación usa el servicio de adaptador de BizTalk para conectarse a un sistema de línea de negocio (LOB) local o usa un destino de tema o cola.  Cree el espacio de nombres de Azure Service Bus. Dé los valores de este espacio de nombres, del nombre del emisor de Service Bus y de la clave del emisor de Service Bus al desarrollador. |[Crear o modificar un espacio de nombres del servicio de Service Bus](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) y [Obtener valores del nombre de emisor y de la clave de emisor](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Instale el SDK y cree el proyecto del Servicio de BizTalk en Visual Studio. |[Instalar SDK de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689760.aspx) y [Crear puntos de conexión de mensajería enriquecida en Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Developer |Implemente su proyecto del Servicio de BizTalk para su Servicio de BizTalk hospedado en Azure. |[Implementar y actualizar el proyecto de BizTalk Services](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Administrador |Se aplica si está usando EDI.  Puede agregar partner y crear acuerdos en el portal de Microsoft Azure BizTalk Services. Al crear un contrato, puede agregar el puente o las transformaciones creadas por el desarrollador a la configuración del acuerdo. |[Configuración de EDI, AS2 y EDIFACT en el Portal de BizTalk Services](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Administrador |Con [REST](https://msdn.microsoft.com/library/azure/dn232347.aspx), supervise el estado de su instancia de BizTalk Services, incluidas las métricas de rendimiento. |[BizTalk Services: pestañas Panel, Monitor y Escala](https://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Administrador |Mediante el portal de Microsoft Azure BizTalk Services, administre los artefactos usados por BizTalk Services y realice un seguimiento de los mensajes a medida que los archivos puente los procesan. |[Uso del portal de BizTalk Services](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Administrador |Cree un plan de copia de seguridad para realizar una copia de seguridad del Servicio de BizTalk. |[Continuidad empresarial y recuperación ante desastres en BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Pasos siguientes
[Tutoriales y ejemplos](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Creación del proyecto en Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Instalación del SDK de Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Conceptos
[Creación del proyecto en Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[Mensajería EDI, AS2 y EDIFACT (Negocio a negocio)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Otros recursos
[Agregar puntos de conexión de mensajería de origen, destino y puente](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Obtener información y crear transformaciones y mapas de mensajes](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Uso del servicio del adaptador de BizTalk (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk Services](https://go.microsoft.com/fwlink/p/?LinkID=303664)

