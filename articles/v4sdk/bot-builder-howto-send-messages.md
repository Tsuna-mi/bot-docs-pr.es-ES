---
title: Envío de mensajes | Microsoft Docs
description: Obtenga información sobre cómo enviar mensajes con Bot Builder SDK.
keywords: envío de mensajes, actividades de mensajes, mensaje de texto simple, voz, mensaje hablado
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/23/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 075e9b3a41462bfbb1398bd72840cc7e50d3cfca
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707521"
---
# <a name="send-text-and-spoken-messages"></a><span data-ttu-id="6e553-104">Envío de mensajes de texto y de voz</span><span class="sxs-lookup"><span data-stu-id="6e553-104">Send text and spoken messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="6e553-105">La forma principal comunicación del bot con los usuarios y de recepción de comunicación es mediante actividades de **mensaje**.</span><span class="sxs-lookup"><span data-stu-id="6e553-105">The primary way your bot will communicate with users, and likewise receive communication, is through **message** activities.</span></span> <span data-ttu-id="6e553-106">Algunos mensajes pueden constar simplemente de texto sin formato, mientras que otros pueden tener un contenido más rico, como tarjetas o datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="6e553-106">Some messages may simply consist of plain text, while others may contain richer content such as cards or attachments.</span></span> <span data-ttu-id="6e553-107">El controlador de turnos del bot recibe mensajes del usuario y puede enviar respuestas al usuario desde ahí.</span><span class="sxs-lookup"><span data-stu-id="6e553-107">Your bot's turn handler receives messages from the user, and you can send responses to the user from there.</span></span> <span data-ttu-id="6e553-108">El objeto del [contexto de turno](bot-builder-concept-activity-processing.md#turn-context) proporciona métodos para enviar mensajes al usuario.</span><span class="sxs-lookup"><span data-stu-id="6e553-108">The [turn context](bot-builder-concept-activity-processing.md#turn-context) object provides methods for sending messages back to the user.</span></span> <span data-ttu-id="6e553-109">Para más información acerca del procesamiento de actividades en general, consulte [Procesamiento de actividades](bot-builder-concept-activity-processing.md).</span><span class="sxs-lookup"><span data-stu-id="6e553-109">For more information about activity processing in general, see [Activity processing](bot-builder-concept-activity-processing.md).</span></span>

<span data-ttu-id="6e553-110">Este artículo describe cómo enviar mensajes de voz y de texto simple.</span><span class="sxs-lookup"><span data-stu-id="6e553-110">This article describes how to send simple text and speech messages.</span></span> <span data-ttu-id="6e553-111">Para enviar contenido más rico, consulte el procedimiento de [Incorporación de datos adjuntos multimedia enriquecidos](bot-builder-howto-add-media-attachments.md).</span><span class="sxs-lookup"><span data-stu-id="6e553-111">For sending richer content, see how to [add rich media attachments](bot-builder-howto-add-media-attachments.md).</span></span> <span data-ttu-id="6e553-112">Para más información sobre cómo usar los objetos de aviso, consulte cómo [pedir datos de entrada a los usuarios](bot-builder-prompts.md).</span><span class="sxs-lookup"><span data-stu-id="6e553-112">For information on how to use prompt objects, see how to [prompt users for input](bot-builder-prompts.md).</span></span>

## <a name="send-a-simple-text-message"></a><span data-ttu-id="6e553-113">Envío de un mensaje de texto simple</span><span class="sxs-lookup"><span data-stu-id="6e553-113">Send a simple text message</span></span>

<span data-ttu-id="6e553-114">Para enviar un mensaje de texto simple, especifique la cadena que desea enviar como la actividad:</span><span class="sxs-lookup"><span data-stu-id="6e553-114">To send a simple text message, specify the string you want to send as the activity:</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="6e553-115">C#</span><span class="sxs-lookup"><span data-stu-id="6e553-115">C#</span></span>](#tab/csharp)

<span data-ttu-id="6e553-116">En el método **OnTurn** del bot, utilice el método **SendActivity** del objeto de contexto de turnos para enviar una respuesta de mensaje única.</span><span class="sxs-lookup"><span data-stu-id="6e553-116">In the bot's **OnTurn** method, use the turn context object's **SendActivity** method to send a single message response.</span></span> <span data-ttu-id="6e553-117">También puede usar el método **SendActivities** del objeto para enviar varias respuestas a la vez.</span><span class="sxs-lookup"><span data-stu-id="6e553-117">You can also use the object's **SendActivities** method to send multiple responses at once.</span></span>

```cs
await context.SendActivity("Greetings from sample message.");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6e553-118">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e553-118">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="6e553-119">En el controlador de turnos del bot, utilice el método **SendActivity** del objeto de contexto de turno para enviar una respuesta de mensaje única.</span><span class="sxs-lookup"><span data-stu-id="6e553-119">In the bot's turn handler, use the turn context object's **sendActivity** method to send a single message response.</span></span> <span data-ttu-id="6e553-120">También puede usar el método **sendActivities** del objeto para enviar varias respuestas a la vez.</span><span class="sxs-lookup"><span data-stu-id="6e553-120">You can also use the object's **sendActivities** method to send multiple responses at once.</span></span>

```javascript
await context.sendActivity("Greetings from sample message.");
```

---

## <a name="send-a-spoken-message"></a><span data-ttu-id="6e553-121">Envío de un mensaje de voz</span><span class="sxs-lookup"><span data-stu-id="6e553-121">Send a spoken message</span></span>

<span data-ttu-id="6e553-122">Algunos canales admiten bots habilitados para voz, lo que les permite hablar con el usuario.</span><span class="sxs-lookup"><span data-stu-id="6e553-122">Certain channels support speech-enabled bots, allowing them to speak to the user.</span></span> <span data-ttu-id="6e553-123">Un mensaje puede tener contenido escrito y hablado.</span><span class="sxs-lookup"><span data-stu-id="6e553-123">A message can have both written and spoken content.</span></span>

> [!NOTE]
> <span data-ttu-id="6e553-124">Para los canales que no admiten voz, se omite el contenido de voz.</span><span class="sxs-lookup"><span data-stu-id="6e553-124">For those channels that don't support speech, the speech content is ignored.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="6e553-125">C#</span><span class="sxs-lookup"><span data-stu-id="6e553-125">C#</span></span>](#tab/csharp)

<span data-ttu-id="6e553-126">Usar el parámetro opcional **speak** para proporcionar el texto que se dirá como parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="6e553-126">Use the optional **speak** parameter to provide text to be spoken as part of the response.</span></span>

```cs
await context.SendActivity(
    "This is the text to be displayed.",
    "This is the text to be spoken.");
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="6e553-127">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6e553-127">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="6e553-128">Para agregar la voz, necesitará `Microsoft.Bot.Builder.MessageFactory` para construir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="6e553-128">To add speech, you'll need the `Microsoft.Bot.Builder.MessageFactory` to build the message.</span></span> <span data-ttu-id="6e553-129">`MessageFactory` se usa más con [elementos multimedia enriquecidos](bot-builder-howto-add-media-attachments.md), donde se explica más, aunque solo lo utilizaremos aquí.</span><span class="sxs-lookup"><span data-stu-id="6e553-129">`MessageFactory` is used more with [rich media](bot-builder-howto-add-media-attachments.md), where it is explained a bit more, but for now we'll just require and use it here.</span></span>

```javascript
// Require MessageFactory from botbuilder
const {MessageFactory} = require('botbuilder');

const basicMessage = MessageFactory.text('This is the text that will be displayed.', 'This is the text that will be spoken.');
await context.sendActivity(basicMessage);
```

---
