---
title: Envío y recepción de datos adjuntos | Microsoft Docs
description: Obtenga información sobre cómo enviar y recibir mensajes que contengan datos adjuntos con el Bot Builder SDK para Node.js.
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: be5018b43a8a015ed763d69a0448e264c5a9fe87
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306441"
---
# <a name="send-and-receive-attachments"></a><span data-ttu-id="3b283-103">Envío y recepción de datos adjuntos</span><span class="sxs-lookup"><span data-stu-id="3b283-103">Send and receive attachments</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-media-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-receive-attachments.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-media-attachments.md)

<span data-ttu-id="3b283-107">Un intercambio de mensajes entre usuario y bot puede contener adjuntos multimedia, como imágenes, vídeo, audio y archivos.</span><span class="sxs-lookup"><span data-stu-id="3b283-107">A message exchange between user and bot can contain media attachments, such as images, video, audio, and files.</span></span> <span data-ttu-id="3b283-108">Los tipos de datos adjuntos que se pueden enviar varía según el canal, pero estos son los tipos básicos:</span><span class="sxs-lookup"><span data-stu-id="3b283-108">The types of attachments that can be sent varies by channel, but these are the basic types:</span></span>

* <span data-ttu-id="3b283-109">**Multimedia y archivos**: puede enviar archivos, como imágenes, audio y vídeos, estableciendo **contentType** en el tipo MIME del [objeto IAttachment][IAttachment] y, a continuación, pasando un vínculo al archivo en **contentUrl**.</span><span class="sxs-lookup"><span data-stu-id="3b283-109">**Media and Files**: You can send files like images, audio and video by setting **contentType** to the MIME type of the [IAttachment object][IAttachment] and then passing a link to the file in **contentUrl**.</span></span>
* <span data-ttu-id="3b283-110">**Tarjetas**: puede enviar un amplio conjunto de tarjetas visuales <!-- and custom keyboards --> estableciendo **contentType** en el tipo de tarjeta deseado y, a continuación, pasando el código JSON de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="3b283-110">**Cards**: You can send a rich set of visual cards <!-- and custom keyboards --> by setting the **contentType** to the desired card's type and then pass the JSON for the card.</span></span> <span data-ttu-id="3b283-111">Si usa una de las clases de generador de tarjetas enriquecidas, como **HeroCard**, los adjuntos se rellenan automáticamente.</span><span class="sxs-lookup"><span data-stu-id="3b283-111">If you use one of the rich card builder classes like **HeroCard**, the attachment is automatically filled in for you.</span></span> <span data-ttu-id="3b283-112">Consulte [envío de una tarjeta enriquecida](bot-builder-nodejs-send-rich-cards.md) para obtener un ejemplo de esto.</span><span class="sxs-lookup"><span data-stu-id="3b283-112">See [send a rich card](bot-builder-nodejs-send-rich-cards.md) for an example of this.</span></span>

## <a name="add-a-media-attachment"></a><span data-ttu-id="3b283-113">Incorporación de adjuntos multimedia</span><span class="sxs-lookup"><span data-stu-id="3b283-113">Add a media attachment</span></span>
<span data-ttu-id="3b283-114">Se espera que el objeto del mensaje sea una instancia de [IMessage][IMessage] y lo más útil resulta enviar al usuario un mensaje como objeto cuando desea incluir adjuntos, como una imagen.</span><span class="sxs-lookup"><span data-stu-id="3b283-114">The message object is expected to be an instance of an [IMessage][IMessage] and it's most useful to send the user a message as an object when you’d like to include an attachment like an image.</span></span> <span data-ttu-id="3b283-115">Use el método [session.send()][SessionSend] para enviar mensajes en forma de objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="3b283-115">Use the [session.send()][SessionSend] method to send messages in the form of a JSON object.</span></span> 

## <a name="example"></a><span data-ttu-id="3b283-116">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3b283-116">Example</span></span>

<span data-ttu-id="3b283-117">En el ejemplo siguiente se comprueba si el usuario ha enviado un adjunto, y si lo hizo, devolverá la imagen contenida en el adjunto.</span><span class="sxs-lookup"><span data-stu-id="3b283-117">The following example checks to see if the user has sent an attachment, and if they have, it will echo back any image contained in the attachment.</span></span> <span data-ttu-id="3b283-118">Puede probar esto con Bot Framework Emulator; para ello, envíe una imagen al bot.</span><span class="sxs-lookup"><span data-stu-id="3b283-118">You can test this with the Bot Framework Emulator by sending your bot an image.</span></span>

```javascript
// Create your bot with a function to receive messages from the user
var bot = new builder.UniversalBot(connector, function (session) {
    var msg = session.message;
    if (msg.attachments && msg.attachments.length > 0) {
     // Echo back attachment
     var attachment = msg.attachments[0];
        session.send({
            text: "You sent:",
            attachments: [
                {
                    contentType: attachment.contentType,
                    contentUrl: attachment.contentUrl,
                    name: attachment.name
                }
            ]
        });
    } else {
        // Echo back users text
        session.send("You said: %s", session.message.text);
    }
});
```
## <a name="additional-resources"></a><span data-ttu-id="3b283-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3b283-119">Additional resources</span></span>

* <span data-ttu-id="3b283-120">[Características en versión preliminar con el Inspector de canales][inspector]</span><span class="sxs-lookup"><span data-stu-id="3b283-120">[Preview features with the Channel Inspector][inspector]</span></span>
* <span data-ttu-id="3b283-121">[IMessage][IMessage]</span><span class="sxs-lookup"><span data-stu-id="3b283-121">[IMessage][IMessage]</span></span>
* <span data-ttu-id="3b283-122">[Envío de una tarjeta enriquecida][SendRichCard]</span><span class="sxs-lookup"><span data-stu-id="3b283-122">[Send a rich card][SendRichCard]</span></span>
* <span data-ttu-id="3b283-123">[session.send][SessionSend]</span><span class="sxs-lookup"><span data-stu-id="3b283-123">[session.send][SessionSend]</span></span>

[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage
[SendRichCard]: bot-builder-nodejs-send-rich-cards.md
[SessionSend]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#send
[IAttachment]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.iattachment.html
[inspector]: ../bot-service-channel-inspector.md
