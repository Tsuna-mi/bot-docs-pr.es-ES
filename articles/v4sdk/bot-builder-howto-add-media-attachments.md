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
# <a name="add-media-to-messages"></a>Incorporación de elementos multimedia a los mensajes

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Un intercambio de mensajes entre el usuario y el bot puede contener datos adjuntos con elementos multimedia, como imágenes, vídeo, audio y archivos. El SDK de Bot Builder incluye una clase `Microsoft.Bot.Builder.MessageFactory` nueva, diseñada para facilitar la tarea de enviar mensajes enriquecidos al usuario.

## <a name="send-attachments"></a>Envío de datos adjuntos

Para enviar el contenido de usuario como una imagen o un vídeo, puede agregar datos adjuntos o una lista de datos adjuntos a un mensaje.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

La propiedad `Attachments` del objeto `Activity` contiene una matriz de objetos `Attachment` que representan los datos adjuntos con elementos multimedia y las tarjetas enriquecidas adjuntas al mensaje. Para agregar datos adjuntos con elementos multimedia a un mensaje, cree un objeto `Attachment` para la actividad `message` y establezca las propiedades `ContentType`, `ContentUrl` y `Name`. 

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

El método `Attachment` de la fábrica de mensajes también puede enviar una lista de datos adjuntos, apilados uno sobre otro.

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Para enviar al usuario un único fragmento de contenido, como una imagen o un vídeo, puede enviar elementos multimedia contenidos en una dirección URL:

```javascript
const {MessageFactory} = require('botbuilder');
let imageOrVideoMessage = MessageFactory.contentUrl('imageUrl.png', 'image/jpeg')

// Send the activity to the user.
await context.sendActivity(imageOrVideoMessage);
```

Para enviar una lista de datos adjuntos, apilados uno sobre otro: <!-- TODO: Convert the hero cards to image attachments in this example. -->

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

Si los datos adjuntos son una imagen, un audio o un vídeo, el servicio del conector comunicará los datos adjuntos al canal de una manera que permita que el [canal](~/v4sdk/bot-builder-channeldata.md) presente esos datos adjuntos dentro de la conversación. Si los datos adjuntos son un archivo, la dirección URL del archivo se presentará como un hipervínculo dentro de la conversación.

## <a name="send-a-hero-card"></a>Envío de una tarjeta de imagen prominente

Además de los datos adjuntos de imagen o vídeo sencillos, puede adjuntar una **tarjeta de imagen prominente** que le permite combinar las imágenes y los botones en un objeto y los envían al usuario.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

Para redactar un mensaje con un botón y una tarjeta de imagen prominente, puede adjuntar `HeroCard` a un mensaje:

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

Para enviar al usuario una tarjeta y un botón, puede adjuntar un `heroCard` al mensaje:

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

Una tarjeta enriquecida consta de un título, una descripción, un vínculo e imágenes. Un mensaje puede contener varias tarjetas enriquecidas, que se muestran en formato de lista o formato de carrusel. El SDK de Bot Builder actualmente admite una amplia gama de tarjetas enriquecidas. Para ver una lista de las tarjetas enriquecidas y los canales en los que son compatibles, consulte [Design UX elements](../bot-service-design-user-experience.md) (Diseño de los elementos de la experiencia del usuario).

> [!TIP]
> Para determinar el tipo de tarjetas enriquecidas que admite un canal y ver cómo el canal presenta las tarjeta enriquecidas, consulte el [Inspector de canales](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-inspector?view=azure-bot-service-4.0). Consulte la documentación del canal para obtener información sobre las limitaciones del contenido de las tarjetas (por ejemplo, el número máximo de botones o la longitud máxima del título).

## <a name="process-events-within-rich-cards"></a>Procesamiento de eventos dentro de tarjetas enriquecidas

Para procesar eventos dentro de tarjetas enriquecidas, use los objetos de _acción de tarjeta_ para especificar qué debe ocurrir cuando el usuario hace clic en un botón o pulsa una sección de la tarjeta.

Para que el funcionamiento sea correcto, asigne un tipo de acción a cada elemento en el que se puede hacer clic en la tarjeta. En esta tabla se enumeran los valores válidos para la propiedad type de un objeto de acción de tarjeta y describe el contenido esperado de la propiedad value de cada tipo.

| Escriba | Valor |
| :---- | :---- |
| openUrl | Dirección URL que se abrirá en el explorador integrado. Responde a Pulsar o Hacer clic mediante la apertura de la dirección URL. |
| imBack | Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta). Este mensaje (del usuario al bot) será visible para todos los participantes de la conversación a través de la aplicación cliente que hospeda la conversación. |
| postBack | Texto del mensaje para enviar al bot (desde el usuario que hizo clic en el botón o que pulsó la tarjeta). Algunas aplicaciones cliente pueden mostrar este texto en la fuente del mensaje, donde estará visible para todos los participantes de la conversación. |
| llamada | Destino de una llamada telefónica con este formato: `tel:123123123123` Responde a Pulsar o Hacer clic mediante el inicio de una llamada.|
| playAudio | Dirección URL del audio que se va a reproducir. Responde a Pulsar o Hacer clic mediante la reproducción del audio. |
| playVideo | Dirección URL del vídeo que se va a reproducir. Responde a Pulsar o Hacer clic mediante la reproducción del vídeo. |
| showImage | Dirección URL de la imagen que se va a mostrar. Responde a Pulsar o Hacer clic mediante la visualización de la imagen. |
| downloadFile | Dirección URL del archivo que se va a descargar.  Responde a Pulsar o Hacer clic mediante la descarga del archivo. |
| signin | Dirección URL del flujo de OAuth que se va a iniciar. Responde a Pulsar o Hacer clic mediante la iniciación del inicio de sesión. |

## <a name="hero-card-using-various-event-types"></a>Tarjeta de imagen prominente a través de varios tipos de evento

El código siguiente muestra ejemplos que usan varios eventos de tarjeta enriquecida.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

## <a name="send-an-adaptive-card"></a>Envío de una tarjeta adaptable

También puede enviar una tarjeta adaptable como datos adjuntos. No todos los canales admiten actualmente las tarjetas adaptables. Para encontrar la información más reciente sobre la compatibilidad de canales de tarjetas adaptables, consulte <a href="http://adaptivecards.io/visualizer/">Visualizador de tarjetas adaptables</a>.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)
Para usar las tarjetas adaptables, no olvide agregar el paquete `Microsoft.AdaptiveCards` de NuGet.


> [!NOTE]
> Debe probar esta característica con los canales que el bot usará para determinar si esos canales admiten las tarjetas adaptables.

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

## <a name="send-a-carousel-of-cards"></a>Envío de un carrusel de cartas

Los mensajes también pueden incluir varios datos adjuntos en un diseño de carrusel, que coloca los datos adjuntos en paralelo y permite que el usuario se desplace por ellos.

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

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

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

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

## <a name="additional-resources"></a>Recursos adicionales

[Preview features with the Channel Inspector](../bot-service-channel-inspector.md) (Vista previa de las características con el Inspector de canales)

---