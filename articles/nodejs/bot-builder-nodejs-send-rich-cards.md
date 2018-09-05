---
title: Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes | Microsoft Docs
description: Obtenga información sobre cómo enviar tarjetas enriquecidas interactivas y atractivas mediante el SDK de Bot Builder para Node.js.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 7f94ea05fcccfe7bdeb1dec187d735cef28b1d7c
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905399"
---
# <a name="add-rich-card-attachments-to-messages"></a><span data-ttu-id="50ead-103">Incorporación de datos adjuntos de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="50ead-103">Add rich card attachments to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]


> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-rich-card-attachments.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-rich-cards.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-rich-cards.md)

<span data-ttu-id="50ead-107">Varios canales, como Skype y Facebook, admiten el envío de tarjetas gráficas enriquecidas a los usuarios con botones interactivos en los cuales el usuario hace clic para iniciar una acción.</span><span class="sxs-lookup"><span data-stu-id="50ead-107">Several channels, like Skype & Facebook, support sending rich graphical cards to users with interactive buttons that the user clicks to initiate an action.</span></span> <span data-ttu-id="50ead-108">El SDK proporciona varias clases de generadores de tarjetas y mensajes que se pueden usar para crear y enviar tarjetas.</span><span class="sxs-lookup"><span data-stu-id="50ead-108">The SDK provides several message and card builder classes which can be used to create and send cards.</span></span> <span data-ttu-id="50ead-109">El servicio Bot Framework Connector representará estas tarjetas con un esquema nativo para el canal, compatible con la comunicación multiplataforma.</span><span class="sxs-lookup"><span data-stu-id="50ead-109">The Bot Framework Connector Service will render these cards using schema native to the channel, supporting cross-platform communication.</span></span> <span data-ttu-id="50ead-110">Si el canal no es compatible con las tarjetas, como SMS, Bot Framework hará todo lo posible para representar una experiencia razonable a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="50ead-110">If the channel does not support cards, such as SMS, the Bot Framework will do its best to render a reasonable experience to users.</span></span> 

## <a name="types-of-rich-cards"></a><span data-ttu-id="50ead-111">Tipos de tarjetas enriquecidas</span><span class="sxs-lookup"><span data-stu-id="50ead-111">Types of rich cards</span></span> 
<span data-ttu-id="50ead-112">Bot Framework admite actualmente ocho tipos de tarjetas enriquecidas:</span><span class="sxs-lookup"><span data-stu-id="50ead-112">The Bot Framework currently supports eight types of rich cards:</span></span> 

| <span data-ttu-id="50ead-113">Tipo de tarjeta</span><span class="sxs-lookup"><span data-stu-id="50ead-113">Card type</span></span> | <span data-ttu-id="50ead-114">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="50ead-114">Description</span></span> |
|------|------|
| <span data-ttu-id="50ead-115"><a href="/adaptive-cards/get-started/bots">Tarjeta adaptable</a></span><span class="sxs-lookup"><span data-stu-id="50ead-115"><a href="/adaptive-cards/get-started/bots">Adaptive Card</a></span></span> | <span data-ttu-id="50ead-116">Una tarjeta personalizable que puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="50ead-116">A customizable card that can contain any combination of text, speech, images, buttons, and input fields.</span></span>  <span data-ttu-id="50ead-117">Consulte la [compatibilidad por canal](/adaptive-cards/get-started/bots#channel-status).</span><span class="sxs-lookup"><span data-stu-id="50ead-117">See [per-channel support](/adaptive-cards/get-started/bots#channel-status).</span></span> |
| <span data-ttu-id="50ead-118">[Tarjeta de animación][animationCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-118">[Animation Card][animationCard]</span></span> | <span data-ttu-id="50ead-119">Una tarjeta que puede reproducir archivos GIF animados o vídeos cortos.</span><span class="sxs-lookup"><span data-stu-id="50ead-119">A card that can play animated GIFs or short videos.</span></span> |
| <span data-ttu-id="50ead-120">[Tarjeta de audio][audioCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-120">[Audio Card][audioCard]</span></span> | <span data-ttu-id="50ead-121">Una tarjeta que se puede reproducir un archivo de audio.</span><span class="sxs-lookup"><span data-stu-id="50ead-121">A card that can play an audio file.</span></span> |
| <span data-ttu-id="50ead-122">[Tarjeta de imagen prominente][heroCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-122">[Hero Card][heroCard]</span></span> | <span data-ttu-id="50ead-123">Una tarjeta que normalmente contiene una sola imagen grande, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="50ead-123">A card that typically contains a single large image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="50ead-124">[Tarjeta de miniatura][thumbnailCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-124">[Thumbnail Card][thumbnailCard]</span></span> | <span data-ttu-id="50ead-125">Una tarjeta que normalmente contiene una sola imagen miniatura, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="50ead-125">A card that typically contains a single thumbnail image, one or more buttons, and text.</span></span>|
| <span data-ttu-id="50ead-126">[Tarjeta de recepción][receiptCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-126">[Receipt Card][receiptCard]</span></span> | <span data-ttu-id="50ead-127">Una tarjeta que permite que un bot proporcione una recepción al usuario.</span><span class="sxs-lookup"><span data-stu-id="50ead-127">A card that enables a bot to provide a receipt to the user.</span></span> <span data-ttu-id="50ead-128">Normalmente, contiene la lista de elementos que se incluyen en la recepción, la información de impuestos y del total y otro texto.</span><span class="sxs-lookup"><span data-stu-id="50ead-128">It typically contains the list of items to include on the receipt, tax and total information, and other text.</span></span> |
| <span data-ttu-id="50ead-129">[Tarjeta de inicio de sesión][signinCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-129">[Signin Card][signinCard]</span></span> | <span data-ttu-id="50ead-130">Tarjeta que permite que un bot solicite a un usuario que inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="50ead-130">A card that enables a bot to request that a user sign-in.</span></span> <span data-ttu-id="50ead-131">Normalmente contiene texto y uno o más botones en los cuales el usuario puede hacer clic para iniciar el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="50ead-131">It typically contains text and one or more buttons that the user can click to initiate the sign-in process.</span></span> |
| <span data-ttu-id="50ead-132">[Tarjeta de videollamada][videoCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-132">[Video Card][videoCard]</span></span> | <span data-ttu-id="50ead-133">Tarjeta que puede reproducir vídeos.</span><span class="sxs-lookup"><span data-stu-id="50ead-133">A card that can play videos.</span></span> |

## <a name="send-a-carousel-of-hero-cards"></a><span data-ttu-id="50ead-134">Enviar un carrusel de tarjetas de imagen prominente</span><span class="sxs-lookup"><span data-stu-id="50ead-134">Send a carousel of Hero cards</span></span>
<span data-ttu-id="50ead-135">En el ejemplo siguiente se muestra un bot para una compañía ficticia de camisetas y cómo enviar un carrusel de tarjetas en respuesta a la solicitud del usuario para "mostrar camisetas".</span><span class="sxs-lookup"><span data-stu-id="50ead-135">The following example shows a bot for a fictional t-shirt company and shows how to send a carousel of cards in response to the user saying “show shirts”.</span></span> 

```javascript
// Create your bot with a function to receive messages from the user
// Create bot and default message handler
var bot = new builder.UniversalBot(connector, function (session) {
    session.send("Hi... We sell shirts. Say 'show shirts' to see our products.");
});

// Add dialog to return list of shirts available
bot.dialog('showShirts', function (session) {
    var msg = new builder.Message(session);
    msg.attachmentLayout(builder.AttachmentLayout.carousel)
    msg.attachments([
        new builder.HeroCard(session)
            .title("Classic White T-Shirt")
            .subtitle("100% Soft and Luxurious Cotton")
            .text("Price is $25 and carried in sizes (S, M, L, and XL)")
            .images([builder.CardImage.create(session, 'http://petersapparel.parseapp.com/img/whiteshirt.png')])
            .buttons([
                builder.CardAction.imBack(session, "buy classic white t-shirt", "Buy")
            ]),
        new builder.HeroCard(session)
            .title("Classic Gray T-Shirt")
            .subtitle("100% Soft and Luxurious Cotton")
            .text("Price is $25 and carried in sizes (S, M, L, and XL)")
            .images([builder.CardImage.create(session, 'http://petersapparel.parseapp.com/img/grayshirt.png')])
            .buttons([
                builder.CardAction.imBack(session, "buy classic gray t-shirt", "Buy")
            ])
    ]);
    session.send(msg).endDialog();
}).triggerAction({ matches: /^(show|list)/i });
```
<span data-ttu-id="50ead-136">En este ejemplo se usa la clase [Message][Message] para compilar un carrusel.</span><span class="sxs-lookup"><span data-stu-id="50ead-136">This example uses the [Message][Message] class to build a carousel.</span></span>  
<span data-ttu-id="50ead-137">El carrusel está formado por una lista de clases [HeroCard] [ heroCard] que contienen una imagen, texto y un solo botón que desencadena la compra del elemento.</span><span class="sxs-lookup"><span data-stu-id="50ead-137">The carousel is comprised of a list of [HeroCard][heroCard] classes that contain an image, text, and a single button that triggers buying the item.</span></span>  
<span data-ttu-id="50ead-138">Al hacer clic en el botón **Comprar**, se desencadena el envío de un mensaje, por lo que se debe agregar un segundo diálogo para detectar el clic del botón.</span><span class="sxs-lookup"><span data-stu-id="50ead-138">Clicking the **Buy** button triggers sending a message so we need to add a second dialog to catch the button click.</span></span> 

## <a name="handle-button-input"></a><span data-ttu-id="50ead-139">Controlar la entrada del botón</span><span class="sxs-lookup"><span data-stu-id="50ead-139">Handle button input</span></span>

<span data-ttu-id="50ead-140">El diálogo `buyButtonClick` se desencadenará cada vez que se reciba un mensaje que comience por "comprar" o "agregar" y, después, contenga algún texto que incluya la palabra "camiseta".</span><span class="sxs-lookup"><span data-stu-id="50ead-140">The `buyButtonClick` dialog will be triggered any time a message is received that starts with “buy” or “add” and is followed by something containing the word “shirt”.</span></span> <span data-ttu-id="50ead-141">El diálogo se inicia mediante el uso de un par de expresiones regulares para buscar la camisa de tamaño opcional y el color que haya pedido el usuario.</span><span class="sxs-lookup"><span data-stu-id="50ead-141">The dialog starts by using a couple of regular expressions to look for the color and optional size shirt that the user asked for.</span></span>
<span data-ttu-id="50ead-142">Esta flexibilidad añadida le permite admitir los clics de botón y los mensajes de lenguaje natural del usuario, como "agrega una camisa gris de tamaño grande a mi carro de la compra".</span><span class="sxs-lookup"><span data-stu-id="50ead-142">This added flexibility lets you support both button clicks and natural language messages from the user like “please add a large gray shirt to my cart”.</span></span>
<span data-ttu-id="50ead-143">Si el color es válido, pero se desconoce el tamaño, el bot pide al usuario que elija un tamaño de una lista antes de agregar el artículo al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="50ead-143">If the color is valid but the size is unknown, the bot prompts the user to pick a size from a list before adding the item to the cart.</span></span> <span data-ttu-id="50ead-144">Una vez que el bot tiene toda la información que necesita, coloca el artículo en un carro de la compra que se ha conservado con **session.userData** y, a continuación, envía al usuario un mensaje de confirmación.</span><span class="sxs-lookup"><span data-stu-id="50ead-144">Once the bot has all the information it needs, it puts the item onto a cart that’s persisted using **session.userData** and then sends the user a confirmation message.</span></span>

```javascript
// Add dialog to handle 'Buy' button click
bot.dialog('buyButtonClick', [
    function (session, args, next) {
        // Get color and optional size from users utterance
        var utterance = args.intent.matched[0];
        var color = /(white|gray)/i.exec(utterance);
        var size = /\b(Extra Large|Large|Medium|Small)\b/i.exec(utterance);
        if (color) {
            // Initialize cart item
            var item = session.dialogData.item = { 
                product: "classic " + color[0].toLowerCase() + " t-shirt",
                size: size ? size[0].toLowerCase() : null,
                price: 25.0,
                qty: 1
            };
            if (!item.size) {
                // Prompt for size
                builder.Prompts.choice(session, "What size would you like?", "Small|Medium|Large|Extra Large");
            } else {
                //Skip to next waterfall step
                next();
            }
        } else {
            // Invalid product
            session.send("I'm sorry... That product wasn't found.").endDialog();
        }   
    },
    function (session, results) {
        // Save size if prompted
        var item = session.dialogData.item;
        if (results.response) {
            item.size = results.response.entity.toLowerCase();
        }

        // Add to cart
        if (!session.userData.cart) {
            session.userData.cart = [];
        }
        session.userData.cart.push(item);

        // Send confirmation to users
        session.send("A '%(size)s %(product)s' has been added to your cart.", item).endDialog();
    }
]).triggerAction({ matches: /(buy|add)\s.*shirt/i });
```

<!-- 

> [!NOTE]
> When sending a message that contains images, keep in mind that some channels download images before displaying a message to the user.   
> As a result, a message containing an image followed immediately by a message without images may sometimes be flipped in the user's feed.
> For information on how to avoid messages being sent out of order, see [Message ordering][MessageOrder].  

-->
## <a name="add-a-message-delay-for-image-downloads"></a><span data-ttu-id="50ead-145">Agregar un retraso de mensaje para descargas de imágenes</span><span class="sxs-lookup"><span data-stu-id="50ead-145">Add a message delay for image downloads</span></span>
<span data-ttu-id="50ead-146">Algunos canales tienden a descargar imágenes antes de mostrar un mensaje al usuario, de modo que si envía un mensaje que contiene una imagen inmediatamente seguida de un mensaje sin imágenes, a veces verá los mensajes volteados en la fuente del usuario.</span><span class="sxs-lookup"><span data-stu-id="50ead-146">Some channels tend to download images before displaying a message to the user so that if you send a message containing an image followed immediately by a message without images you’ll sometimes see the messages flipped in the user's feed.</span></span> <span data-ttu-id="50ead-147">Para minimizar la posibilidad de que esto ocurra, puede intentar asegurarse de que sus imágenes procedan de redes de entrega de contenido (CDN) y evitar el uso de imágenes demasiado grandes.</span><span class="sxs-lookup"><span data-stu-id="50ead-147">To minimize the chance of this you can try to insure that your images are coming from content deliver networks (CDNs) and avoid the use of overly large images.</span></span> <span data-ttu-id="50ead-148">En casos extremos incluso es posible que necesite insertar un retraso de 1 a 2 segundos entre el mensaje con la imagen y el siguiente.</span><span class="sxs-lookup"><span data-stu-id="50ead-148">In extreme cases you may even need to insert a 1-2 second delay between the message with the image and the one that follows it.</span></span> <span data-ttu-id="50ead-149">Puede hacer que este retraso parezca un poco más natural para el usuario mediante una llamada a **session.sendTyping()** para enviar un indicador de escritura antes de iniciar el retraso.</span><span class="sxs-lookup"><span data-stu-id="50ead-149">You can make this delay feel a bit more natural to the user by calling **session.sendTyping()** to send a typing indicator before starting your delay.</span></span> 

<!-- 
To learn more about sending a typing indicator, see [How to send a typing indicator](bot-builder-nodejs-send-typing-indicator.md).
-->

<span data-ttu-id="50ead-150">Bot Framework implementa un procesamiento por lotes para intentar evitar varios mensajes del bot que muestren que no está en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="50ead-150">The Bot Framework implements a batching to try to prevent multiple messages from the bot from being displayed out of order.</span></span> <span data-ttu-id="50ead-151"><!-- Unfortunately, not all channels can guarantee this. --> Cuando su bot envía varias respuestas al usuario, los mensajes individuales se agrupan automáticamente en un lote y se entregan al usuario como un conjunto en un esfuerzo por conservar el orden original de los mensajes.</span><span class="sxs-lookup"><span data-stu-id="50ead-151"><!-- Unfortunately, not all channels can guarantee this. --> When your bot sends multiple replies to the user, the individual messages will be automatically grouped into a batch and delivered to the user as a set in an effort to preserve the original order of the messages.</span></span> <span data-ttu-id="50ead-152">Este procesamiento por lotes automático espera un tiempo predeterminado de 250 ms después de cada llamada a **session.send()** antes de iniciar la siguiente llamada a **send()**.</span><span class="sxs-lookup"><span data-stu-id="50ead-152">This automatic batching waits a default of 250ms after every call to **session.send()** before initiating the next call to **send()**.</span></span>

<span data-ttu-id="50ead-153">El retraso de procesamiento por lotes del mensaje se puede configurar.</span><span class="sxs-lookup"><span data-stu-id="50ead-153">The message batching delay is configurable.</span></span> <span data-ttu-id="50ead-154">Para deshabilitar la lógica del procesamiento por lotes automático del SDK, establezca el retraso predeterminado en un número grande y, a continuación, llame manualmente a **sendBatch()** con una devolución de llamada para realizar la invocación una vez que se haya entregado el lote.</span><span class="sxs-lookup"><span data-stu-id="50ead-154">To disable the SDK’s auto-batching logic, set the default delay to a large number and then manually call **sendBatch()** with a callback to invoke after the batch is delivered.</span></span>

## <a name="send-an-adaptive-card"></a><span data-ttu-id="50ead-155">Enviar una tarjeta adaptable</span><span class="sxs-lookup"><span data-stu-id="50ead-155">Send an Adaptive card</span></span>

<span data-ttu-id="50ead-156">Una tarjeta adaptable puede contener cualquier combinación de texto, voz, imágenes, botones y campos de entrada.</span><span class="sxs-lookup"><span data-stu-id="50ead-156">The Adaptive Card can contain any combination of text, speech, images, buttons, and input fields.</span></span> <span data-ttu-id="50ead-157">Las tarjetas adaptables se crean con el formato JSON especificado en <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables), que proporciona control total sobre el contenido y el formato de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="50ead-157">Adaptive Cards are created using the JSON format specified in <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a>, which gives you full control over card content and format.</span></span> 

<span data-ttu-id="50ead-158">Para crear una tarjeta adaptable con Node.js, utilice la información del sitio <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables) a fin de comprender el esquema de la tarjeta adaptable, explorar los elementos de la tarjeta adaptable y ver ejemplos de JSON que puede usar para crear tarjetas de diferente composición y complejidad.</span><span class="sxs-lookup"><span data-stu-id="50ead-158">To create an Adaptive Card using Node.js, leverage the information within the <a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> site to understand Adaptive Card schema, explore Adaptive Card elements, and see JSON samples that can be used to create cards of varying composition and complexity.</span></span> <span data-ttu-id="50ead-159">Además, puede usar el visualizador interactivo para diseñar cargas útiles de la tarjeta adaptable y obtener una vista previa del resultado de la tarjeta.</span><span class="sxs-lookup"><span data-stu-id="50ead-159">Additionally, you can use the Interactive Visualizer to design Adaptive Card payloads and preview card output.</span></span>

<span data-ttu-id="50ead-160">En este ejemplo de código se muestra cómo crear un mensaje que contiene una tarjeta adaptable para un recordatorio del calendario:</span><span class="sxs-lookup"><span data-stu-id="50ead-160">This code example shows how to create a message that contains an Adaptive Card for a calendar reminder:</span></span> 

[!code-javascript[Add Adaptive Card attachment](../includes/code/node-send-card-buttons.js#addAdaptiveCardAttachment)]

<span data-ttu-id="50ead-161">La tarjeta resultante contiene tres bloques de texto, un campo de entrada (lista de opciones) y tres botones:</span><span class="sxs-lookup"><span data-stu-id="50ead-161">The resulting card contains three blocks of text, an input field (choice list), and three buttons:</span></span>

![Recordatorio del calendario de tarjeta adaptable](../media/adaptive-card-reminder.png)

## <a name="additional-resources"></a><span data-ttu-id="50ead-163">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="50ead-163">Additional resources</span></span>

* <span data-ttu-id="50ead-164">[Preview features with the Channel Inspector][inspector] (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="50ead-164">[Preview features with the Channel Inspector][inspector]</span></span>
* <span data-ttu-id="50ead-165"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a> (Tarjetas adaptables)</span><span class="sxs-lookup"><span data-stu-id="50ead-165"><a href="http://adaptivecards.io" target="_blank">Adaptive Cards</a></span></span>
* <span data-ttu-id="50ead-166">[AnimationCard][animationCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-166">[AnimationCard][animationCard]</span></span>
* <span data-ttu-id="50ead-167">[AudioCard][audioCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-167">[AudioCard][audioCard]</span></span>
* <span data-ttu-id="50ead-168">[HeroCard][heroCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-168">[HeroCard][heroCard]</span></span>
* <span data-ttu-id="50ead-169">[ThumbnailCard][thumbnailCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-169">[ThumbnailCard][thumbnailCard]</span></span>
* <span data-ttu-id="50ead-170">[ReceiptCard][receiptCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-170">[ReceiptCard][receiptCard]</span></span>
* <span data-ttu-id="50ead-171">[SigninCard][signinCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-171">[SigninCard][signinCard]</span></span>
* <span data-ttu-id="50ead-172">[VideoCard][videoCard]</span><span class="sxs-lookup"><span data-stu-id="50ead-172">[VideoCard][videoCard]</span></span>
* <span data-ttu-id="50ead-173">[Message][Message]</span><span class="sxs-lookup"><span data-stu-id="50ead-173">[Message][Message]</span></span>
* <span data-ttu-id="50ead-174">[How to send attachments](bot-builder-nodejs-send-receive-attachments.md) (Cómo enviar datos adjuntos)</span><span class="sxs-lookup"><span data-stu-id="50ead-174">[How to send attachments](bot-builder-nodejs-send-receive-attachments.md)</span></span>

[MessageOrder]: bot-builder-nodejs-manage-conversation-flow.md#message-ordering
[Message]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.message
[IMessage]: http://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.imessage

[animationCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.animationcard.html 

[audioCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.audiocard.html 

[heroCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.herocard.html

[thumbnailCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.thumbnailcard.html 

[receiptCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.receiptcard.html 

[signinCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.signincard.html 

[videoCard]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.videocard.html

[inspector]: ../bot-service-channel-inspector.md
