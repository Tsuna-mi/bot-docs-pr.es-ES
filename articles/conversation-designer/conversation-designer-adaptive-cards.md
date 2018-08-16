---
title: Configuración de tarjetas adaptables | Microsoft Docs
description: Aprenda a configurar tarjetas adaptables.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 01f52a6aa6e2c9fc3d2613ff03386a7d87e64a3a
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306068"
---
# <a name="configure-adaptive-cards"></a><span data-ttu-id="80154-103">Configuración de tarjetas adaptables</span><span class="sxs-lookup"><span data-stu-id="80154-103">Configure adaptive cards</span></span>
> [!IMPORTANT]
> <span data-ttu-id="80154-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="80154-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="80154-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="80154-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="80154-106">Las <a href="http://adaptivecards.io" target="_blank">tarjetas adaptables</a> son un nuevo esquema que define tarjetas de interfaz de usuario enriquecidas para su uso en varios puntos de conexión diferentes, incluidos los canales de Microsoft Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="80154-106"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> is a new schema that defines rich UI cards for use in several different endpoints including Microsoft Bot Framework channels.</span></span> 

<span data-ttu-id="80154-107">Conversation Designer proporciona un entorno de creación profundamente integrado para crear, usar y obtener una vista previa de tarjetas adaptables en los bots.</span><span class="sxs-lookup"><span data-stu-id="80154-107">Conversation Designer provides a deeply integrated authoring environment to author, preview, and use adaptive cards in your bots.</span></span> 

<span data-ttu-id="80154-108">Las tarjetas adaptables se pueden definir en distintos lugares clave.</span><span class="sxs-lookup"><span data-stu-id="80154-108">Adaptive cards can be defined in several different key places.</span></span>

- <span data-ttu-id="80154-109">Una respuesta sencilla a una [acción](conversation-designer-tasks.md) para una tarea.</span><span class="sxs-lookup"><span data-stu-id="80154-109">A simple response to [action](conversation-designer-tasks.md) for a task.</span></span>
- <span data-ttu-id="80154-110">En estado de comentario en un diálogo.</span><span class="sxs-lookup"><span data-stu-id="80154-110">In feedback state in a dialogue.</span></span>
- <span data-ttu-id="80154-111">En los estados de aviso, en un diálogo.</span><span class="sxs-lookup"><span data-stu-id="80154-111">In prompt states in a dialogue.</span></span> <span data-ttu-id="80154-112">Tenga en cuenta que los avisos pueden tener tarjetas distintas: una para la respuesta y otra para volver a pedir información.</span><span class="sxs-lookup"><span data-stu-id="80154-112">Note that prompts can have separate cards: one for the response and another for re-prompting.</span></span>

<span data-ttu-id="80154-113">Para definir una tarjeta adaptable, vaya al editor correspondiente.</span><span class="sxs-lookup"><span data-stu-id="80154-113">To define an adaptive card, navigate to the relevant editor.</span></span> <span data-ttu-id="80154-114">Examine y elija una de las plantillas de tarjeta adaptable existentes o cree la suya propia en el editor de código JSON.</span><span class="sxs-lookup"><span data-stu-id="80154-114">Browse and choose from one of the existing Adaptive Card Templates or build your own in the JSON code editor.</span></span> 

<!--TODO: Insert screenshot -->

<span data-ttu-id="80154-115">A medida que se crea una tarjeta, se representa una vista previa completa de ella en el portal de creación.</span><span class="sxs-lookup"><span data-stu-id="80154-115">As you are building a card, a rich preview of the card is rendered in the authoring portal.</span></span>

> [!NOTE]
> <span data-ttu-id="80154-116">Las características de las tarjetas adaptables permanecen en desarrollo continuado.</span><span class="sxs-lookup"><span data-stu-id="80154-116">Features of adaptive cards remain under ongoing development.</span></span> <span data-ttu-id="80154-117">En este momento, no todos los canales admiten todas las características de las tarjetas adaptables.</span><span class="sxs-lookup"><span data-stu-id="80154-117">All channels do not support all adaptive card features at this time.</span></span> <span data-ttu-id="80154-118">Para ver qué características admite cada canal, consulte la sección [Estado del canal](/adaptive-cards/get-started/bots#channel-status).</span><span class="sxs-lookup"><span data-stu-id="80154-118">To see which features each channel supports, see the [Channel status](/adaptive-cards/get-started/bots#channel-status) section.</span></span>

## <a name="input-form"></a><span data-ttu-id="80154-119">Formulario de entrada</span><span class="sxs-lookup"><span data-stu-id="80154-119">Input form</span></span>

<span data-ttu-id="80154-120">Las tarjetas adaptables pueden contener formularios de entrada.</span><span class="sxs-lookup"><span data-stu-id="80154-120">Adaptive cards can contain input forms.</span></span> <span data-ttu-id="80154-121">En Conversation Designer, los formularios se integran con entidades de tareas.</span><span class="sxs-lookup"><span data-stu-id="80154-121">In Conversation Designer, forms are integrated with task entities.</span></span> <span data-ttu-id="80154-122">Por ejemplo, si un campo tiene un `id` de **myName** y se realiza la acción `Submit` del formulario, se crea un elemento `taskEntity` con el nombre **myName** que contendrá el valor del campo.</span><span class="sxs-lookup"><span data-stu-id="80154-122">For example, if a field has an `id` of **myName** and the form `Submit` action is performed, a `taskEntity` with the name **myName** will be created and will contain the value of the field.</span></span> 

<span data-ttu-id="80154-123">El fragmento de código siguiente muestra cómo la entidad **myName** se define en el código:</span><span class="sxs-lookup"><span data-stu-id="80154-123">The code snippet below shows how the **myName** entity is defined in code:</span></span>

``javascript
{
   "type": "Input.Text",
   "id": "myName",
   "placeholder": "Last, First"
}
``

<span data-ttu-id="80154-124">Además, si un campo tiene un identificador de `@task`, entonces el valor del campo se usa como un nombre de tarea.</span><span class="sxs-lookup"><span data-stu-id="80154-124">Additionally, if a field has an id of `@task` then the value of the field will be used as a task name.</span></span> <span data-ttu-id="80154-125">Cuando se desencadena este campo (p. ej.: con un clic de botón), se ejecuta entonces la tarea designada.</span><span class="sxs-lookup"><span data-stu-id="80154-125">When this field is triggered (e.g.: a button click), then the named task will be executed.</span></span> 

<span data-ttu-id="80154-126">Vamos a tomar este fragmento de código como ejemplo:</span><span class="sxs-lookup"><span data-stu-id="80154-126">Take this snippet code for example:</span></span>

``javascript
{
  'type': 'Action.Submit',
  'title': 'Search',
  'speak': '<s>Search</s>',
  'data': {
    '@task': 'Hotel Search'
  }
}
``

<span data-ttu-id="80154-127">Cuando se hace clic en este botón, se desencadena una acción de envío y `context.sticky` se establece en `Hotel Search`.</span><span class="sxs-lookup"><span data-stu-id="80154-127">When this button is clicked, a submit action is triggered and the `context.sticky` will be set to `Hotel Search`.</span></span> <span data-ttu-id="80154-128">Como consecuencia, se ejecuta la tarea **Hotel Search**.</span><span class="sxs-lookup"><span data-stu-id="80154-128">This will result in the **Hotel Search** task to execute.</span></span> <span data-ttu-id="80154-129">Para usar esta característica, asegúrese de que `@task` coincida con un nombre de tarea que haya definido en Conversation Designer.</span><span class="sxs-lookup"><span data-stu-id="80154-129">To use this feature, make sure the `@task` matches a task name that you have defined in Conversation Designer.</span></span>

## <a name="use-entities-and-language-generation-templates"></a><span data-ttu-id="80154-130">Uso de entidades y plantillas de generación de idioma</span><span class="sxs-lookup"><span data-stu-id="80154-130">Use entities and language generation templates</span></span>
<span data-ttu-id="80154-131">Las tarjetas adaptables admiten la resolución de generación de idioma completo.</span><span class="sxs-lookup"><span data-stu-id="80154-131">Adaptive cards support full language generation resolution.</span></span>

* <span data-ttu-id="80154-132">`entityName` usa entidades dentro de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="80154-132">`entityName` uses entities inside the card.</span></span>
* <span data-ttu-id="80154-133">`responseTemplateName` usa plantillas de respuesta simple o condicional dentro de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="80154-133">`responseTemplateName` uses simple or conditional response templates inside the card.</span></span>

<!--
# Binding form flow input fields to bot entities
TODO: fill this out based on design/ implementation -->

<!-- ## Adaptive Card schema

You can learn more about adaptive cards here  TODO: Insert link to adaptive cards schema documentation -->

## <a name="sample-adaptive-card-payload"></a><span data-ttu-id="80154-134">Ejemplo de carga de tarjeta adaptable</span><span class="sxs-lookup"><span data-stu-id="80154-134">Sample adaptive card payload</span></span>

<span data-ttu-id="80154-135">El siguiente código JSON muestra la carga de una tarjeta adaptable.</span><span class="sxs-lookup"><span data-stu-id="80154-135">The following JSON shows the payload of an adaptive card.</span></span>

```json
{
    "$schema": "https://microsoft.github.io/AdaptiveCards/schemas/adaptive-card.json",
    "type": "AdaptiveCard",
    "version": "1.0",
    "body": [
        {
            "speak": "<s>Serious Pie is a Pizza restaurant which is rated 9.3 by customers.</s>",
            "type": "ColumnSet",
            "columns": [
                {
                    "type": "Column",
                    "size": "2",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "[Greeting], [TimeOfDayTemplate], You can eat in {location}",
                            "weight": "bolder",
                            "size": "extraLarge"
                        },
                        {
                            "type": "TextBlock",
                            "text": "9.3 · $$ · Pizza",
                            "isSubtle": true
                        },
                        {
                            "type": "TextBlock",
                            "text": "[builtin.feedback.display]",
                            "wrap": true
                        }
                    ]
                },
                {
                    "type": "Column",
                    "size": "1",
                    "items": [
                        {
                            "type": "Image",
                            "url": "http://res.cloudinary.com/sagacity/image/upload/c_crop,h_670,w_635,x_0,y_0/c_scale,w_640/v1397425743/Untitled-4_lviznp.jpg",
                            "size":"auto"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.Http",
            "method": "POST",
            "title": "More Info",
            "url": "http://foo.com"
        },
        {
            "type": "Action.Http",
            "method": "POST",
            "title": "View on Foursquare",
            "url": "http://foo.com"
        }
    ]
}
```

## <a name="next-step"></a><span data-ttu-id="80154-136">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="80154-136">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="80154-137">Conexión a canales</span><span class="sxs-lookup"><span data-stu-id="80154-137">Connect to channels</span></span>](conversation-designer-deploy.md)
