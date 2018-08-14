---
title: Incorporación de elementos multimedia a los mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar elementos multimedia a los mensajes mediante el SDK de Bot Builder.
keywords: elementos multimedia, mensajes, imágenes, audio, vídeo, archivos, MessageFactory, mensajes enriquecidos
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/03/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: e1b25a3b5c090cbb13b4c27279745a81da64e6c4
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305449"
---
# <a name="add-media-to-messages"></a><span data-ttu-id="dbc65-104">Incorporación de elementos multimedia a los mensajes</span><span class="sxs-lookup"><span data-stu-id="dbc65-104">Add media to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="dbc65-105">Un intercambio de mensajes entre el usuario y el bot puede contener datos adjuntos con elementos multimedia, como imágenes, vídeo, audio y archivos.</span><span class="sxs-lookup"><span data-stu-id="dbc65-105">A message exchange between user and bot can contain media attachments, such as images, video, audio, and files.</span></span> <span data-ttu-id="dbc65-106">El SDK de Bot Builder incluye una clase `Microsoft.Bot.Builder.MessageFactory` nueva, diseñada para facilitar la tarea de enviar mensajes enriquecidos al usuario.</span><span class="sxs-lookup"><span data-stu-id="dbc65-106">The Bot Builder SDK includes a new `Microsoft.Bot.Builder.MessageFactory` class, designed to ease the task of sending rich messages to the user.</span></span>

## <a name="send-attachments"></a><span data-ttu-id="dbc65-107">Envío de datos adjuntos</span><span class="sxs-lookup"><span data-stu-id="dbc65-107">Send attachments</span></span>

<span data-ttu-id="dbc65-108">Para enviar el contenido de usuario como una imagen o un vídeo, puede agregar datos adjuntos o una lista de datos adjuntos a un mensaje.</span><span class="sxs-lookup"><span data-stu-id="dbc65-108">To send the user content like an image or a video, you can add an attachment or list of attachments to a message.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="dbc65-109">C#</span><span class="sxs-lookup"><span data-stu-id="dbc65-109">C#</span></span>](#tab/csharp)

<span data-ttu-id="dbc65-110">La propiedad `Attachments` del objeto `Activity` contiene una matriz de objetos `Attachment` que representan los datos adjuntos con elementos multimedia y las tarjetas enriquecidas adjuntas al mensaje.</span><span class="sxs-lookup"><span data-stu-id="dbc65-110">The `Attachments` property of the `Activity` object contains an array of `Attachment` objects that represent the media attachments and rich cards attached to the message.</span></span> <span data-ttu-id="dbc65-111">Para agregar datos adjuntos con elementos multimedia a un mensaje, cree un objeto `Attachment` para la actividad `message` y establezca las propiedades `ContentType`, `ContentUrl` y `Name`.</span><span class="sxs-lookup"><span data-stu-id="dbc65-111">To add a media attachment to a message, create an `Attachment` object for the `message` activity and set the `ContentType`, `ContentUrl`, and `Name` properties.</span></span> 

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and add an attachment.
var activity = MessageFactory.Attachment(
    new Attachment { ContentUrl = "imageUrl.png", ContentType = "image/png" }
);

// Send the activity to the user.
await context.SendActivity(activity);
```

<span data-ttu-id="dbc65-112">El método `Attachment` de la fábrica de mensajes también puede enviar una lista de datos adjuntos, apilados uno sobre otro.</span><span class="sxs-lookup"><span data-stu-id="dbc65-112">The message factory's `Attachment` method can also send a list of attachments, stacked on on top of another.</span></span>

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and add an attachment.
var activity = MessageFactory.Attachment(new Attachment[]
{
    new Attachment { ContentUrl = "imageUrl1.png", ContentType = "image/png" },
    new Attachment { ContentUrl = "imageUrl2.png", ContentType = "image/png" },
    new Attachment { ContentUrl = "imageUrl3.png", ContentType = "image/png" }
});

// Send the activity to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="dbc65-113">JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc65-113">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="dbc65-114">Para enviar al usuario un único fragmento de contenido, como una imagen o un vídeo, puede enviar elementos multimedia contenidos en una dirección URL:</span><span class="sxs-lookup"><span data-stu-id="dbc65-114">To send the user a single piece of content like an image or a video, you can send media contained in a URL:</span></span>

```javascript
const {MessageFactory} = require('botbuilder');
let imageOrVideoMessage = MessageFactory.contentUrl('imageUrl.png', 'image/jpeg')

// Send the activity to the user.
await context.sendActivity(imageOrVideoMessage);
```

<span data-ttu-id="dbc65-115">Para enviar una lista de datos adjuntos, apilados uno sobre otro: <!-- TODO: Convert the hero cards to image attachments in this example. --></span><span class="sxs-lookup"><span data-stu-id="dbc65-115">To send a list of attachments, stacked one on top of another: <!-- TODO: Convert the hero cards to image attachments in this example. --></span></span>

```javascript
// require MessageFactory and CardFactory from botbuilder.
const {MessageFactory} = require('botbuilder');
const {CardFactory} = require('botbuilder');

let messageWithCarouselOfCards = MessageFactory.list([
    CardFactory.heroCard('title1', ['imageUrl1'], ['button1']),
    CardFactory.heroCard('title2', ['imageUrl2'], ['button2']),
    CardFactory.heroCard('title3', ['imageUrl3'], ['button3'])
]);

await context.sendActivity(messageWithCarouselOfCards);
```

---

<span data-ttu-id="dbc65-116">Si los datos adjuntos son una imagen, un audio o un vídeo, el servicio del conector comunicará los datos adjuntos al canal de una manera que permita que el [canal](~/v4sdk/bot-builder-channeldata.md) presente esos datos adjuntos dentro de la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbc65-116">If an attachment is an image, audio, or video, the Connector service will communicate attachment data to the channel in a way that enables the [channel](~/v4sdk/bot-builder-channeldata.md) to render that attachment within the conversation.</span></span> <span data-ttu-id="dbc65-117">Si los datos adjuntos son un archivo, la dirección URL del archivo se presentará como un hipervínculo dentro de la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbc65-117">If the attachment is a file, the file URL will be rendered as a hyperlink within the conversation.</span></span>

## <a name="send-a-hero-card"></a><span data-ttu-id="dbc65-118">Envío de una tarjeta de imagen prominente</span><span class="sxs-lookup"><span data-stu-id="dbc65-118">Send a hero card</span></span>

<span data-ttu-id="dbc65-119">Además de los datos adjuntos de imagen o vídeo sencillos, puede adjuntar una **tarjeta de imagen prominente** que le permite combinar las imágenes y los botones en un objeto y los envían al usuario.</span><span class="sxs-lookup"><span data-stu-id="dbc65-119">Besides simple image or video attachments, you can attach a **hero card**, which allows you to combine images and buttons in one object, and send them to the user.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="dbc65-120">C#</span><span class="sxs-lookup"><span data-stu-id="dbc65-120">C#</span></span>](#tab/csharp)

<span data-ttu-id="dbc65-121">Para redactar un mensaje con un botón y una tarjeta de imagen prominente, puede adjuntar `HeroCard` a un mensaje:</span><span class="sxs-lookup"><span data-stu-id="dbc65-121">To compose a message with a hero card and button, you can attach a `HeroCard` to a message:</span></span>

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and attach a Hero card.
var activity = MessageFactory.Attachment(
    new HeroCard(
        title: "White T-Shirt",
        images: new CardImage[] { new CardImage(url: "imageUrl.png") },
        buttons: new CardAction[]
        {
            new CardAction(title: "buy", type: ActionTypes.ImBack, value: "buy")
        })
    .ToAttachment());

// Send the activity as a reply to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="dbc65-122">JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc65-122">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="dbc65-123">Para enviar al usuario una tarjeta y un botón, puede adjuntar un `heroCard` al mensaje:</span><span class="sxs-lookup"><span data-stu-id="dbc65-123">To send the user a card and button, you can attach a `heroCard` to the message:</span></span>

```javascript
// require MessageFactory and CardFactory from botbuilder.
const {MessageFactory} = require('botbuilder');
const {CardFactory} = require('botbuilder');

const message = MessageFactory.attachment(
     CardFactory.heroCard(
        'White T-Shirt',
        ['https://example.com/whiteShirt.jpg'],
        ['buy']
    )
 );

await context.sendActivity(message);
```

---

<!--Lifted from the RESP API documentation-->

<span data-ttu-id="dbc65-124">Una tarjeta enriquecida consta de un título, una descripción, un vínculo e imágenes.</span><span class="sxs-lookup"><span data-stu-id="dbc65-124">A rich card comprises a title, description, link, and images.</span></span> <span data-ttu-id="dbc65-125">Un mensaje puede contener varias tarjetas enriquecidas, que se muestran en formato de lista o formato de carrusel.</span><span class="sxs-lookup"><span data-stu-id="dbc65-125">A message can contain multiple rich cards, displayed in either list format or carousel format.</span></span> <span data-ttu-id="dbc65-126">El SDK de Bot Builder actualmente admite una amplia gama de tarjetas enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="dbc65-126">The Bot Builder SDK currently supports a wide range of rich cards.</span></span> <span data-ttu-id="dbc65-127">Para ver una lista de las tarjetas enriquecidas y los canales en los que son compatibles, consulte [Design UX elements](../bot-service-design-user-experience.md) (Diseño de los elementos de la experiencia del usuario).</span><span class="sxs-lookup"><span data-stu-id="dbc65-127">To see a listing of rich cards and channels in which they are supported, see [Design UX elements](../bot-service-design-user-experience.md).</span></span>

> [!TIP]
> <span data-ttu-id="dbc65-128">Para determinar el tipo de tarjetas enriquecidas que admite un canal y ver cómo el canal presenta las tarjeta enriquecidas, consulte el [Inspector de canales](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-inspector?view=azure-bot-service-4.0).</span><span class="sxs-lookup"><span data-stu-id="dbc65-128">To determine the type of rich cards that a channel supports and see how the channel renders rich cards, see the [Channel Inspector](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-inspector?view=azure-bot-service-4.0).</span></span> <span data-ttu-id="dbc65-129">Consulte la documentación del canal para obtener información sobre las limitaciones del contenido de las tarjetas (por ejemplo, el número máximo de botones o la longitud máxima del título).</span><span class="sxs-lookup"><span data-stu-id="dbc65-129">Consult the channel's documentation for information about limitations on the contents of cards (for example, the maximum number of buttons or maximum length of title).</span></span>

## <a name="process-events-within-rich-cards"></a><span data-ttu-id="dbc65-130">Procesamiento de eventos dentro de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="dbc65-130">Process events within rich cards</span></span>

<span data-ttu-id="dbc65-131">Para procesar eventos dentro de tarjetas enriquecidas, use los objetos de _acción de tarjeta_ para especificar qué debe ocurrir cuando el usuario hace clic en un botón o pulsa una sección de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbc65-131">To process events within rich cards, use _card action_ objects to specify what should happen when the user clicks a button or taps a section of the card.</span></span>

<span data-ttu-id="dbc65-132">Para que el funcionamiento sea correcto, asigne un tipo de acción a cada elemento en el que se puede hacer clic en la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="dbc65-132">To function correctly, assign an action type to each clickable item on the card.</span></span> <span data-ttu-id="dbc65-133">En esta tabla se enumeran los valores válidos para la propiedad type de un objeto de acción de tarjeta y describe el contenido esperado de la propiedad value de cada tipo.</span><span class="sxs-lookup"><span data-stu-id="dbc65-133">This table lists the valid values for the type property of a card action object and describes the expected contents of the value property for each type.</span></span>

| <span data-ttu-id="dbc65-134">Escriba</span><span class="sxs-lookup"><span data-stu-id="dbc65-134">Type</span></span> | <span data-ttu-id="dbc65-135">Valor</span><span class="sxs-lookup"><span data-stu-id="dbc65-135">Value</span></span> |
| :---- | :---- |
| <span data-ttu-id="dbc65-136">openUrl</span><span class="sxs-lookup"><span data-stu-id="dbc65-136">openUrl</span></span> | <span data-ttu-id="dbc65-137">Dirección URL que se abrirá en el explorador integrado.</span><span class="sxs-lookup"><span data-stu-id="dbc65-137">URL to be opened in the built-in browser.</span></span> <span data-ttu-id="dbc65-138">Responde a Pulsar o Hacer clic mediante la apertura de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="dbc65-138">Responds to Tap or Click by opening the URL.</span></span> |
| <span data-ttu-id="dbc65-139">imBack</span><span class="sxs-lookup"><span data-stu-id="dbc65-139">imBack</span></span> | <span data-ttu-id="dbc65-140">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="dbc65-140">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="dbc65-141">Este mensaje (del usuario al bot) será visible para todos los participantes de la conversación a través de la aplicación cliente que hospeda la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbc65-141">This message (from user to bot) will be visible to all conversation participants via the client application that is hosting the conversation.</span></span> |
| <span data-ttu-id="dbc65-142">postBack</span><span class="sxs-lookup"><span data-stu-id="dbc65-142">postBack</span></span> | <span data-ttu-id="dbc65-143">Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta).</span><span class="sxs-lookup"><span data-stu-id="dbc65-143">Text of the message to send to the bot (from the user who clicked the button or tapped the card).</span></span> <span data-ttu-id="dbc65-144">Algunas aplicaciones cliente pueden mostrar este texto en la fuente del mensaje, donde estará visible para todos los participantes de la conversación.</span><span class="sxs-lookup"><span data-stu-id="dbc65-144">Some client applications may display this text in the message feed, where it will be visible to all conversation participants.</span></span> |
| <span data-ttu-id="dbc65-145">llamada</span><span class="sxs-lookup"><span data-stu-id="dbc65-145">call</span></span> | <span data-ttu-id="dbc65-146">Destino de una llamada telefónica con este formato: `tel:123123123123` Responde a Pulsar o Hacer clic mediante el inicio de una llamada.</span><span class="sxs-lookup"><span data-stu-id="dbc65-146">Destination for a phone call in this format: `tel:123123123123` Responds to Tap or Click by initiating a call.</span></span>|
| <span data-ttu-id="dbc65-147">playAudio</span><span class="sxs-lookup"><span data-stu-id="dbc65-147">playAudio</span></span> | <span data-ttu-id="dbc65-148">Dirección URL del audio que se va a reproducir.</span><span class="sxs-lookup"><span data-stu-id="dbc65-148">URL of audio to be played.</span></span> <span data-ttu-id="dbc65-149">Responde a Pulsar o Hacer clic mediante la reproducción del audio.</span><span class="sxs-lookup"><span data-stu-id="dbc65-149">Responds to Tap or Click by playing the audio.</span></span> |
| <span data-ttu-id="dbc65-150">playVideo</span><span class="sxs-lookup"><span data-stu-id="dbc65-150">playVideo</span></span> | <span data-ttu-id="dbc65-151">Dirección URL del vídeo que se va a reproducir.</span><span class="sxs-lookup"><span data-stu-id="dbc65-151">URL of video to be played.</span></span> <span data-ttu-id="dbc65-152">Responde a Pulsar o Hacer clic mediante la reproducción del vídeo.</span><span class="sxs-lookup"><span data-stu-id="dbc65-152">Responds to Tap or Click by playing the video.</span></span> |
| <span data-ttu-id="dbc65-153">showImage</span><span class="sxs-lookup"><span data-stu-id="dbc65-153">showImage</span></span> | <span data-ttu-id="dbc65-154">Dirección URL de la imagen que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="dbc65-154">URL of image to be displayed.</span></span> <span data-ttu-id="dbc65-155">Responde a Pulsar o Hacer clic mediante la visualización de la imagen.</span><span class="sxs-lookup"><span data-stu-id="dbc65-155">Responds to Tap or Click by displaying the image.</span></span> |
| <span data-ttu-id="dbc65-156">downloadFile</span><span class="sxs-lookup"><span data-stu-id="dbc65-156">downloadFile</span></span> | <span data-ttu-id="dbc65-157">Dirección URL del archivo que se va a descargar.</span><span class="sxs-lookup"><span data-stu-id="dbc65-157">URL of file to be downloaded.</span></span>  <span data-ttu-id="dbc65-158">Responde a Pulsar o Hacer clic mediante la descarga del archivo.</span><span class="sxs-lookup"><span data-stu-id="dbc65-158">Responds to Tap or Click by downloading the file.</span></span> |
| <span data-ttu-id="dbc65-159">signin</span><span class="sxs-lookup"><span data-stu-id="dbc65-159">signin</span></span> | <span data-ttu-id="dbc65-160">Dirección URL del flujo de OAuth que se va a iniciar.</span><span class="sxs-lookup"><span data-stu-id="dbc65-160">URL of OAuth flow to be initiated.</span></span> <span data-ttu-id="dbc65-161">Responde a Pulsar o Hacer clic mediante la iniciación del inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="dbc65-161">Responds to Tap or Click by initiating signin.</span></span> |

## <a name="hero-card-using-various-event-types"></a><span data-ttu-id="dbc65-162">Tarjeta de imagen prominente a través de varios tipos de evento</span><span class="sxs-lookup"><span data-stu-id="dbc65-162">Hero card using various event types</span></span>

<span data-ttu-id="dbc65-163">El código siguiente muestra ejemplos que usan varios eventos de tarjeta enriquecida.</span><span class="sxs-lookup"><span data-stu-id="dbc65-163">The following code shows examples using various rich card events.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="dbc65-164">C#</span><span class="sxs-lookup"><span data-stu-id="dbc65-164">C#</span></span>](#tab/csharp)

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and attach a Hero card.
var activity = MessageFactory.Attachment(
    new HeroCard(
        title: "Holler Back Buttons",
        images: new CardImage[] { new CardImage(url: "imageUrl.png") },
        buttons: new CardAction[]
        {
            new CardAction(title: "Shout Out Loud", type: ActionTypes.imBack, value: "You can ALL hear me!"),
            new CardAction(title: "Much Quieter", type: ActionTypes.postBack, value: "Shh! My Bot friend hears me."),
            new CardAction(title: "Show me how to Holler", type: ActionTypes.openURL, value: $"https://en.wikipedia.org/wiki/{cardContent.Key}")
        })
    .ToAttachment());

// Send the activity as a reply to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="dbc65-165">JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc65-165">JavaScript</span></span>](#tab/javascript)

```javascript
const {ActionTypes} = require("botbuilder");
```

```javascript
const hero = MessageFactory.attachment(
    CardFactory.heroCard(
        'Holler Back Buttons',
        ['https://example.com/whiteShirt.jpg'],
        [{
            type: ActionTypes.ImBack,
            title: 'ImBack',
            value: 'You can ALL hear me! Shout Out Loud'
        },
        {
            type: ActionTypes.PostBack,
            title: 'PostBack',
            value: 'Shh! My Bot friend hears me. Much Quieter'
        },
        {
            type: ActionTypes.OpenUrl,
            title: 'OpenUrl',
            value: 'https://en.wikipedia.org/wiki/{cardContent.Key}'
        }]
    )
);

await context.sendActivity(hero);

```

---

## <a name="send-an-adaptive-card"></a><span data-ttu-id="dbc65-166">Envío de una tarjeta adaptable</span><span class="sxs-lookup"><span data-stu-id="dbc65-166">Send an Adaptive Card</span></span>

<span data-ttu-id="dbc65-167">También puede enviar una tarjeta adaptable como datos adjuntos.</span><span class="sxs-lookup"><span data-stu-id="dbc65-167">You can also send an Adaptive Card as an attachment.</span></span> <span data-ttu-id="dbc65-168">No todos los canales admiten actualmente las tarjetas adaptables.</span><span class="sxs-lookup"><span data-stu-id="dbc65-168">Currently not all channels support adaptive cards.</span></span> <span data-ttu-id="dbc65-169">Para encontrar la información más reciente sobre la compatibilidad de canales de tarjetas adaptables, consulte <a href="http://adaptivecards.io/visualizer/">Visualizador de tarjetas adaptables</a>.</span><span class="sxs-lookup"><span data-stu-id="dbc65-169">To find the latest information on Adaptive Card channel support, see the <a href="http://adaptivecards.io/visualizer/">Adaptive Cards Visualizer</a>.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="dbc65-170">C#</span><span class="sxs-lookup"><span data-stu-id="dbc65-170">C#</span></span>](#tab/csharp)
<span data-ttu-id="dbc65-171">Para usar las tarjetas adaptables, no olvide agregar el paquete `Microsoft.AdaptiveCards` de NuGet.</span><span class="sxs-lookup"><span data-stu-id="dbc65-171">To use adaptive cards, be sure to add the `Microsoft.AdaptiveCards` NuGet package.</span></span>


> [!NOTE]
> <span data-ttu-id="dbc65-172">Debe probar esta característica con los canales que el bot usará para determinar si esos canales admiten las tarjetas adaptables.</span><span class="sxs-lookup"><span data-stu-id="dbc65-172">You should test this feature with the channels your bot will use to determine whether those channels support adaptive cards.</span></span>

```csharp
using AdaptiveCards;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and attach an Adaptive Card.
var card = new AdaptiveCard();
card.Body.Add(new TextBlock()
{
    Text = "<title>",
    Size = TextSize.Large,
    Wrap = true,
    Weight = TextWeight.Bolder
});
card.Body.Add(new TextBlock() { Text = "<message text>", Wrap = true });
card.Body.Add(new TextInput()
{
    Id = "Title",
    Value = string.Empty,
    Style = TextInputStyle.Text,
    Placeholder = "Title",
    IsRequired = true,
    MaxLength = 50
});
card.Actions.Add(new SubmitAction() { Title = "Submit", DataJson = "{ Action:'Submit' }" });
card.Actions.Add(new SubmitAction() { Title = "Cancel", DataJson = "{ Action:'Cancel'}" });

var activity = MessageFactory.Attachment(new Attachment(AdaptiveCard.ContentType, content: card));

// Send the activity as a reply to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="dbc65-173">JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc65-173">JavaScript</span></span>](#tab/javascript)

```javascript
const {CardFactory} = require("botbuilder");

const message = CardFactory.adaptiveCard({
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0",
    "type": "AdaptiveCard",
    "speak": "Your flight is confirmed for you from San Francisco to Amsterdam on Friday, October 10 8:30 AM",
    "body": [
        {
            "type": "TextBlock",
            "text": "Passenger",
            "weight": "bolder",
            "isSubtle": false
        },
        {
            "type": "TextBlock",
            "text": "Sarah Hum",
            "separator": true
        },
        {
            "type": "TextBlock",
            "text": "1 Stop",
            "weight": "bolder",
            "spacing": "medium"
        },
        {
            "type": "TextBlock",
            "text": "Fri, October 10 8:30 AM",
            "weight": "bolder",
            "spacing": "none"
        },
        {
            "type": "ColumnSet",
            "separator": true,
            "columns": [
                {
                    "type": "Column",
                    "width": 1,
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "San Francisco",
                            "isSubtle": true
                        },
                        {
                            "type": "TextBlock",
                            "size": "extraLarge",
                            "color": "accent",
                            "text": "SFO",
                            "spacing": "none"
                        }
                    ]
                },
                {
                    "type": "Column",
                    "width": "auto",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": " "
                        },
                        {
                            "type": "Image",
                            "url": "http://messagecardplayground.azurewebsites.net/assets/airplane.png",
                            "size": "small",
                            "spacing": "none"
                        }
                    ]
                },
                {
                    "type": "Column",
                    "width": 1,
                    "items": [
                        {
                            "type": "TextBlock",
                            "horizontalAlignment": "right",
                            "text": "Amsterdam",
                            "isSubtle": true
                        },
                        {
                            "type": "TextBlock",
                            "horizontalAlignment": "right",
                            "size": "extraLarge",
                            "color": "accent",
                            "text": "AMS",
                            "spacing": "none"
                        }
                    ]
                }
            ]
        },
        {
            "type": "ColumnSet",
            "spacing": "medium",
            "columns": [
                {
                    "type": "Column",
                    "width": "1",
                    "items": [
                        {
                            "type": "TextBlock",
                            "text": "Total",
                            "size": "medium",
                            "isSubtle": true
                        }
                    ]
                },
                {
                    "type": "Column",
                    "width": 1,
                    "items": [
                        {
                            "type": "TextBlock",
                            "horizontalAlignment": "right",
                            "text": "$1,032.54",
                            "size": "medium",
                            "weight": "bolder"
                        }
                    ]
                }
            ]
        }
    ]
});

// send adaptive card as attachment 
await context.sendActivity({ attachments: [message] })
```

---

## <a name="send-a-carousel-of-cards"></a><span data-ttu-id="dbc65-174">Envío de un carrusel de cartas</span><span class="sxs-lookup"><span data-stu-id="dbc65-174">Send a carousel of cards</span></span>

<span data-ttu-id="dbc65-175">Los mensajes también pueden incluir varios datos adjuntos en un diseño de carrusel, que coloca los datos adjuntos en paralelo y permite que el usuario se desplace por ellos.</span><span class="sxs-lookup"><span data-stu-id="dbc65-175">Messages can also include multiple attachments in a carousel layout, which places the attachments side by side and allows the user to scroll across.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="dbc65-176">C#</span><span class="sxs-lookup"><span data-stu-id="dbc65-176">C#</span></span>](#tab/csharp)

```csharp
using Microsoft.Bot.Builder;
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Schema;

// Create the activity and attach a set of Hero cards.
var activity = MessageFactory.Carousel(
    new Attachment[]
    {
        new HeroCard(
            title: "title1",
            images: new CardImage[] { new CardImage(url: "imageUrl1.png") },
            buttons: new CardAction[]
            {
                new CardAction(title: "button1", type: ActionTypes.ImBack, value: "item1")
            })
        .ToAttachment(),
        new HeroCard(
            title: "title2",
            images: new CardImage[] { new CardImage(url: "imageUrl2.png") },
            buttons: new CardAction[]
            {
                new CardAction(title: "button2", type: ActionTypes.ImBack, value: "item2")
            })
        .ToAttachment(),
        new HeroCard(
            title: "title3",
            images: new CardImage[] { new CardImage(url: "imageUrl3.png") },
            buttons: new CardAction[]
            {
                new CardAction(title: "button3", type: ActionTypes.ImBack, value: "item3")
            })
        .ToAttachment()
    });

// Send the activity as a reply to the user.
await context.SendActivity(activity);
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="dbc65-177">JavaScript</span><span class="sxs-lookup"><span data-stu-id="dbc65-177">JavaScript</span></span>](#tab/javascript)

```javascript
// require MessageFactory and CardFactory from botbuilder.
const {MessageFactory} = require('botbuilder');
const {CardFactory} = require('botbuilder');

//  init message object
let messageWithCarouselOfCards = MessageFactory.carousel([
    CardFactory.heroCard('title1', ['imageUrl1'], ['button1']),
    CardFactory.heroCard('title2', ['imageUrl2'], ['button2']),
    CardFactory.heroCard('title3', ['imageUrl3'], ['button3'])
]);

await context.sendActivity(messageWithCarouselOfCards);
```

---

## <a name="additional-resources"></a><span data-ttu-id="dbc65-178">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="dbc65-178">Additional resources</span></span>

<span data-ttu-id="dbc65-179">[Preview features with the Channel Inspector](../bot-service-channel-inspector.md) (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="dbc65-179">[Preview features with the Channel Inspector](../bot-service-channel-inspector.md)</span></span>

---