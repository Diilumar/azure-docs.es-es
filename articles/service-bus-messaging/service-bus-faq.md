---
title: Preguntas más frecuentes (FAQ) sobre Azure Service Bus | Microsoft Docs
description: Respuestas a algunas preguntas frecuentes acerca de Azure Service Bus.
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: fce3c2975e4b82583aa09a3862f704f05a363828
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56210601"
---
# <a name="service-bus-faq"></a>Preguntas más frecuentes sobre Service Bus

En este artículo se tratan algunas preguntas frecuentes sobre Microsoft Azure Service Bus. También puede consultar las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/) para obtener información general sobre los precios y el soporte técnico de Azure.

## <a name="general-questions-about-azure-service-bus"></a>Preguntas generales sobre Azure Service Bus
### <a name="what-is-azure-service-bus"></a>Qué es Azure Service Bus
[Azure Service Bus](service-bus-messaging-overview.md) es una plataforma en la nube de mensajería asincrónica que le permite enviar datos entre sistemas desacoplados. Microsoft ofrece esta característica como un servicio, lo que significa que el usuario no tendrá que hospedar su propio hardware para poder utilizarlo.

### <a name="what-is-a-service-bus-namespace"></a>¿Qué es un espacio de nombres de Service Bus?
Un [espacio de nombres](service-bus-create-namespace-portal.md) proporciona un contenedor con un ámbito para el desvío de recursos de Service Bus en la aplicación. Es necesario crear un espacio de nombres para usar Service Bus y es uno de los primeros pasos para comenzar.

### <a name="what-is-an-azure-service-bus-queue"></a>¿Qué es una cola de Azure Service Bus?
Una [cola de Service Bus](service-bus-queues-topics-subscriptions.md) es una entidad en la que se almacenan los mensajes. Las colas son útiles cuando tiene varias aplicaciones o varias partes de una aplicación distribuida que necesitan comunicarse entre sí. La cola es similar a un centro de distribución en el sentido de que se recibe múltiples productos (mensajes) que luego se envían desde esa ubicación.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>¿Qué son los temas y las suscripciones de Azure Service Bus?
Un tema se puede visualizar como una cola y cuando utiliza varias suscripciones, se convierte en un modelo de mensajería más enriquecido; básicamente en una herramienta de comunicación de uno a varios. Este modelo de publicación o suscripción (o *pub/sub*) permite que cuando una aplicación envía un mensaje a un tema con varias suscripciones, el mensaje lo reciban varias aplicaciones.

### <a name="what-is-a-partitioned-entity"></a>¿Qué es una entidad con particiones?
Un único agente de mensajes controla una cola o tema convencional, que se almacena en un almacén de mensajería. Las [colas o temas con particiones](service-bus-partitioning.md), que solo se admiten en los niveles de mensajería Básico y Estándar, las administran varios agentes de mensajes y se almacenan en varios almacenes de mensajería. Esta característica significa que el rendimiento general de una cola o tema particionado ya no está limitado por el rendimiento de un solo agente o almacén de mensajería. Además, una interrupción temporal de un almacén de mensajería no hace que una cola o tema con particiones deje de estar disponible.

La ordenación no está garantizada al utilizar entidades con particiones. En caso de que una partición no esté disponible, puede enviar y recibir mensajes de las otras particiones.

 Ya no se admiten las entidades con particiones en la [SKU Premium](service-bus-premium-messaging.md). 

## <a name="best-practices"></a>Procedimientos recomendados
### <a name="what-are-some-azure-service-bus-best-practices"></a>¿Cuáles son algunos de los procedimientos recomendados de Azure Service Bus?
Vea [Procedimientos recomendados para mejorar el rendimiento mediante Service Bus][Best practices for performance improvements using Service Bus]: en este artículo se describe cómo optimizar el rendimiento al intercambiar mensajes.

### <a name="what-should-i-know-before-creating-entities"></a>¿Qué debo saber antes de crear entidades?
Las siguientes propiedades de una cola y un tema son inmutables. Tenga en cuenta esta limitación al aprovisionar las entidades, ya que estas no se pueden modificar sin crear una entidad de sustitución.

* Creación de particiones
* Sesiones
* Detección de duplicados
* Entidad exprés

## <a name="pricing"></a>Precios
En esta sección se responde a algunas preguntas frecuentes sobre la estructura de precios de Service Bus.

En el artículo [Precios y facturación de Service Bus](service-bus-pricing-billing.md) se explican los medidores de facturación de Service Bus. Para obtener información específica sobre las opciones de precios de Service Bus, vea [Precios de Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

También puede consultar las [Preguntas más frecuentes de soporte técnico de Microsoft Azure](https://azure.microsoft.com/support/faq/) para obtener información general sobre los precios de Azure. 

### <a name="how-do-you-charge-for-service-bus"></a>¿Cómo se cobra Service Bus?
Para obtener más información sobre los precios de Service Bus, vea [Precios de Service Bus][Pricing overview]. Además de los precios indicados, se le cobrará por las transferencias de datos asociadas para salidas del centro de datos en el que se aprovisiona la aplicación.

### <a name="what-usage-of-service-bus-is-subject-to-data-transfer-what-is-not"></a>¿Qué uso de Service Bus está sujeto a la transferencia de datos? ¿Cuál no lo está?
Cualquier transferencia de datos dentro de una determinada región de Azure se proporciona sin cargo alguno, así como las transferencias de datos entrantes. La transferencia de datos fuera de una región está sujeta a cargos por concepto de salida; consulte [esta página](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>¿Service Bus cobra por almacenamiento?
No, Service Bus no cobra por almacenamiento. Sin embargo, hay una cuota que limita la cantidad máxima de datos que pueden persistir por cola/tema. Consulte la siguiente pregunta.

## <a name="quotas"></a>Cuotas

Para obtener una lista de las cuotas y los límites de Service Bus, consulte [Información general sobre cuotas de Service Bus][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>¿Service Bus tiene alguna cuota de uso?
De forma predeterminada, para cualquier servicio en la nube, Microsoft establece una cuota de uso mensual agregada que se calcula en todas las suscripciones del cliente. Si estos límites no son suficientes, puede ponerse en contacto con el servicio de atención al cliente en cualquier momento para que podamos conocer sus necesidades y ajustar estos límites según corresponda. En lo que respecta a Service Bus, las cuotas de uso agregado ascienden a 5000 millones de mensajes al mes.

Aunque Microsoft se reserva el derecho de deshabilitar una cuenta de cliente que supere sus cuotas de uso en un mes determinado, se proporcionará una notificación por correo electrónico y se realizarán varios intentos para ponerse en contacto con un cliente antes de llevar a cabo cualquier acción. Los clientes que superen estas cuotas siguen siendo responsables de cargos por exceso de cuotas.

Al igual que con otros servicios de Azure, Service Bus aplica un conjunto de cuotas específicas para garantizar un uso justo de los recursos. Puede encontrar más detalles acerca de estas cuotas en [Información general sobre cuotas de Service Bus][Quotas overview].

### <a name="how-to-handle-messages-of-size--1-mb"></a>Cómo administrar los mensajes con un tamaño superior a 1 MB
Los servicios de mensajería de Service Bus (colas y temas o suscripciones) permiten que la aplicación envíe mensajes de un tamaño de hasta 256 KB (nivel estándar) o 1 MB (nivel prémium). Si trabaja con mensajes de tamaño superior a 1 MB, use el patrón de comprobación de notificaciones que se describe en [esta entrada de blog](https://www.serverless360.com/blog/deal-with-large-service-bus-messages-using-claim-check-pattern).

## <a name="troubleshooting"></a>solución de problemas
### <a name="why-am-i-not-able-to-create-a-namespace-after-deleting-it-from-another-subscription"></a>¿Por qué no puedo crear un espacio de nombres después de eliminarlo de otra suscripción? 
Cuando elimine un espacio de nombres de una suscripción, espere 4 horas antes de volver a crearlo con el mismo nombre en otra suscripción. En caso contrario, es posible que reciba el mensaje de error siguiente: `Namespace already exists`. 

### <a name="what-are-some-of-the-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>¿Cuáles son algunas de las excepciones generadas por las API de Azure Service Bus y sus acciones sugeridas?
Para obtener una lista de posibles excepciones de Service Bus, consulte [Información general sobre excepciones][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>¿Qué es una firma de acceso compartido y qué lenguajes admiten la generación de una firma?
Firmas de acceso compartido son un mecanismo de autenticación basado en URI y valores hash seguros SHA-256. Para más información acerca de cómo generar sus propias firmas en Node.js, PHP, Java y C\#, consulte el artículo sobre las [firmas de acceso compartido][Shared Access Signatures].

## <a name="subscription-and-namespace-management"></a>Administración de suscripción y espacio de nombres
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>¿Cómo se migra un espacio de nombres a otra suscripción de Azure?

Puede mover un espacio de nombres de una suscripción de Azure a otra, a través de [Azure Portal](https://portal.azure.com) o con comandos de PowerShell. Para ejecutar la operación, el espacio de nombres tiene que estar ya activo. El usuario que ejecuta los comandos debe ser administrador en las suscripciones de origen y de destino.

#### <a name="portal"></a>Portal

Para usar Azure Portal para migrar espacios de nombres de Service Bus a otra suscripción, siga las instrucciones indicadas [aquí](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

La siguiente secuencia de comandos de PowerShell mueve un espacio de nombres de una suscripción de Azure a otra. Para ejecutar esta operación, el espacio de nombres ya debe estar activo y el usuario que ejecuta los comandos de PowerShell debe ser administrador en las suscripciones de origen y destino.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription to target subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre Service Bus, vea los artículos siguientes:

* [Introducción a Azure Service Bus Premium (entrada de blog)](https://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducción a Azure Service Bus Premium (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Información general de Service Bus](service-bus-messaging-overview.md)
* [Introducción a las colas de Service Bus](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
