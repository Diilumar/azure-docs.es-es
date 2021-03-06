---
title: Creación de una nueva aplicación
titleSuffix: Language Understanding - Azure Cognitive Services
description: Cree y administre las aplicaciones en la página web de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: f9cf5e723484196125548b9e6d3956e909e9c9b0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55874961"
---
# <a name="create-a-new-luis-app-in-the-luis-portal"></a>Creación de una aplicación de LUIS en el portal de LUIS
Hay un par de formas de crear aplicaciones de LUIS. Puede crear una aplicación de LUIS en el portal de [LUIS](https://www.luis.ai), o bien mediante las [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) de creación de LUIS.

## <a name="using-the-luis-portal"></a>Mediante el portal de LUIS
Puede crear una aplicación en el portal de LUIS de varias maneras:

* Comience con una aplicación vacía y cree intenciones, expresiones y entidades.
* Comience con una aplicación vacía y agregue un [dominio creado previamente](luis-how-to-use-prebuilt-domains.md).
* Importe una aplicación de LUIS desde un archivo JSON que ya contenga intenciones, expresiones y entidades.

## <a name="using-the-authoring-apis"></a>Mediante las API de creación
Puede crear una aplicación con las API de creación de dos maneras:

* [Comience](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f) con una aplicación vacía y cree intenciones, expresiones y entidades.
* [Empiece](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/59104e515aca2f0b48c76be5) con un dominio creado previamente.  


<a name="export-app"></a>
<a name="import-new-app"></a>
<a name="delete-app"></a>
 

## <a name="create-new-app-in-luis"></a>Creación de una aplicación en LUIS

1. En la página **My Apps** (Mis aplicaciones), haga clic en **Create new app** (Crear aplicación).

    ![Lista de aplicaciones de LUIS](./media/luis-create-new-app/apps-list.png)


2. En el cuadro de diálogo asigne el nombre "TravelAgent" a la aplicación.

    ![Cuadro de diálogo para crear una aplicación](./media/luis-create-new-app/create-app.png)

3. Elija la referencia cultural de la aplicación (para la aplicación TravelAgent, seleccione Inglés) y, después, haga clic en **Done** (Listo). 

    > [!NOTE]
    > La referencia cultural no se puede cambiar una vez creada la aplicación. 


## <a name="next-steps"></a>Pasos siguientes

La primera tarea en la aplicación es [agregar intenciones](luis-how-to-add-intents.md).
