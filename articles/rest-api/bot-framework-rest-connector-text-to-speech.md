---
title: Incorporación de voz a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar voz a los mensajes mediante el servicio Bot Connector.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 91385fa3e8ae1410679ca5274e40db7fe38bafea
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306272"
---
# <a name="add-speech-to-messages"></a><span data-ttu-id="4858f-103">Incorporación de voz a mensajes</span><span class="sxs-lookup"><span data-stu-id="4858f-103">Add speech to messages</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-text-to-speech.md)
> - [Node.js](../nodejs/bot-builder-nodejs-text-to-speech.md)
> - [REST](../rest-api/bot-framework-rest-connector-text-to-speech.md)

<span data-ttu-id="4858f-107">Si está creando un bot para un canal habilitado para voz, como Cortana, puede construir mensajes que especifique el texto que dirá el bot.</span><span class="sxs-lookup"><span data-stu-id="4858f-107">If you are building a bot for a speech-enabled channel such as Cortana, you can construct messages that specify the text to be spoken by your bot.</span></span> <span data-ttu-id="4858f-108">También puede intentar influir en el estado del micrófono del cliente al especificar una [sugerencia de entrada](bot-framework-rest-connector-add-input-hints.md) para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="4858f-108">You can also attempt to influence the state of the client's microphone by specifying an [input hint](bot-framework-rest-connector-add-input-hints.md) to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span>

## <a name="specify-text-to-be-spoken-by-your-bot"></a><span data-ttu-id="4858f-109">Especificar el texto que dirá el bot</span><span class="sxs-lookup"><span data-stu-id="4858f-109">Specify text to be spoken by your bot</span></span>

<span data-ttu-id="4858f-110">Para especificar el texto que dirá el bot en un canal habilitado para voz, establezca la propiedad `speak`dentro del objeto [Activity][Activity] que representa el mensaje.</span><span class="sxs-lookup"><span data-stu-id="4858f-110">To specify text to be spoken by your bot on a speech-enabled channel, set the `speak` property within the [Activity][Activity] object that represents your message.</span></span> <span data-ttu-id="4858f-111">Puede establecer la propiedad `speak` en una cadena de texto sin formato o en una cadena con formato de <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">lenguaje de marcado de síntesis de voz (SSML)</a>, un lenguaje de marcado basado en XML que le permite controlar diversas características de la voz del bot, como la voz, la velocidad, el volumen, la pronunciación, el tono y mucho más.</span><span class="sxs-lookup"><span data-stu-id="4858f-111">You can set the `speak` property to either a plain text string or a string that is formatted as <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language (SSML)</a>, an XML-based markup language that enables you to control various characteristics of your bot's speech such as voice, rate, volume, pronunciation, pitch, and more.</span></span> 

<span data-ttu-id="4858f-112">La siguiente solicitud envía un mensaje que especifica el texto que se mostrará y el texto que se dirá, e indica que el bot [espera la entrad del usuario](bot-framework-rest-connector-add-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="4858f-112">The following request sends a message that specifies text to be displayed and text to be spoken and indicates that the bot is [expecting user input](bot-framework-rest-connector-add-input-hints.md).</span></span> <span data-ttu-id="4858f-113">Especifica la propiedad `speak` con el formato <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">SSML</a> para indicar que la palabra "sure" se debe decir con una cantidad moderada de énfasis.</span><span class="sxs-lookup"><span data-stu-id="4858f-113">It specifies the `speak` property using <a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">SSML</a> format to indicate that the word "sure" should be spoken with a moderate amount of emphasis.</span></span> <span data-ttu-id="4858f-114">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI de base; los URI de base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="4858f-114">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="4858f-115">Para obtener más información sobre cómo establecer el URI de base, consulte [Referencia de la API](bot-framework-rest-connector-api-reference.md#base-uri).</span><span class="sxs-lookup"><span data-stu-id="4858f-115">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/conversations/abcd1234/activities/5d5cdc723
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "type": "message",
    "from": {
        "id": "12345678",
        "name": "sender's name"
    },
    "conversation": {
        "id": "abcd1234",
        "name": "conversation's name"
   },
   "recipient": {
        "id": "1234abcd",
        "name": "recipient's name"
    },
    "text": "Are you sure that you want to cancel this transaction?",
    "speak": "Are you <emphasis level='moderate'>sure</emphasis> that you want to cancel this transaction?",
    "inputHint": "expectingInput",
    "replyToId": "5d5cdc723"
}
```

## <a name="input-hints"></a><span data-ttu-id="4858f-116">Sugerencias de entrada</span><span class="sxs-lookup"><span data-stu-id="4858f-116">Input hints</span></span>

<span data-ttu-id="4858f-117">Cuando envía un mensaje en un canal habilitado para voz, también puede intentar influir en el estado del micrófono del cliente al incluir una sugerencia de entrada para indicar si el bot acepta, espera o ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="4858f-117">When you send a message on a speech-enabled channel, you can attempt to influence the state of the client's microphone by also including an input hint to indicate whether your bot is accepting, expecting, or ignoring user input.</span></span> <span data-ttu-id="4858f-118">Para obtener más información, consulte [Incorporación de sugerencias de entrada a mensajes](bot-framework-rest-connector-add-input-hints.md).</span><span class="sxs-lookup"><span data-stu-id="4858f-118">For more information, see [Add input hints to messages](bot-framework-rest-connector-add-input-hints.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4858f-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4858f-119">Additional resources</span></span>

- [<span data-ttu-id="4858f-120">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="4858f-120">Create messages</span></span>](bot-framework-rest-connector-create-messages.md)
- [<span data-ttu-id="4858f-121">Envío y recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="4858f-121">Send and receive messages</span></span>](bot-framework-rest-connector-send-and-receive-messages.md)
- [<span data-ttu-id="4858f-122">Incorporación de sugerencias de entrada a mensajes</span><span class="sxs-lookup"><span data-stu-id="4858f-122">Add input hints to messages</span></span>](bot-framework-rest-connector-add-input-hints.md)
- <span data-ttu-id="4858f-123"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Lenguaje de marcado de síntesis de voz (SSML)</a></span><span class="sxs-lookup"><span data-stu-id="4858f-123"><a href="https://msdn.microsoft.com/en-us/library/hh378377(v=office.14).aspx" target="_blank">Speech Synthesis Markup Language (SSML)</a></span></span>

[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
