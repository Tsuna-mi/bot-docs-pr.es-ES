---
title: Incorporación de sugerencias de entrada a los mensajes | Microsoft Docs
description: Aprenda a agregar sugerencias de entrada a los mensajes mediante Bot Builder SDK para .NET.
author: v-ducvo
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 2bbf75f166a90d2e0a905bd269f51cef4398a2ef
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904497"
---
# <a name="add-input-hints-to-messages"></a><span data-ttu-id="0d7de-103">Incorporación de sugerencias de entrada a los mensajes</span><span class="sxs-lookup"><span data-stu-id="0d7de-103">Add input hints to messages</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-add-input-hints.md)
> - [Node.js](../nodejs/bot-builder-nodejs-send-input-hints.md)
> - [REST](../rest-api/bot-framework-rest-connector-add-input-hints.md)

<span data-ttu-id="0d7de-107">Al especificar una sugerencia de entrada para un mensaje, puede indicar si el bot acepta, espera o ignora la entrada del usuario después de que el mensaje se haya entregado al cliente.</span><span class="sxs-lookup"><span data-stu-id="0d7de-107">By specifying an input hint for a message, you can indicate whether your bot is accepting, expecting, or ignoring user input after the message is delivered to the client.</span></span> <span data-ttu-id="0d7de-108">En muchos canales, esto permite a los clientes establecer en consecuencia el estado de los controles de entrada de usuario.</span><span class="sxs-lookup"><span data-stu-id="0d7de-108">For many channels, this enables clients to set the state of user input controls accordingly.</span></span> <span data-ttu-id="0d7de-109">Por ejemplo, si la sugerencia de entrada de un mensaje indica que el bot ignora la entrada de usuario, el cliente podría desactivar el micrófono y deshabilitar el cuadro de entrada para impedir que el usuario proporcione una entrada.</span><span class="sxs-lookup"><span data-stu-id="0d7de-109">For example, if a message's input hint indicates that the bot is ignoring user input, the client may close the microphone and disable the input box to prevent the user from providing input.</span></span>

## <a name="accepting-input"></a><span data-ttu-id="0d7de-110">Aceptar entradas</span><span class="sxs-lookup"><span data-stu-id="0d7de-110">Accepting input</span></span>

<span data-ttu-id="0d7de-111">Para indicar que el bot está preparado pasivamente para la entrada, pero que no espera ninguna respuesta del usuario, establezca la sugerencia de entrada del mensaje en `builder.InputHint.acceptingInput`.</span><span class="sxs-lookup"><span data-stu-id="0d7de-111">To indicate that your bot is passively ready for input but is not awaiting a response from the user, set the message's input hint to `builder.InputHint.acceptingInput`.</span></span> <span data-ttu-id="0d7de-112">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se desactive, sin dejar de estar accesible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="0d7de-112">On many channels, this will cause the client's input box to be enabled and microphone to be closed, but still accessible to the user.</span></span> <span data-ttu-id="0d7de-113">Por ejemplo, Cortana activará el micrófono para aceptar la entrada del usuario si este mantiene presionado el botón de micrófono.</span><span class="sxs-lookup"><span data-stu-id="0d7de-113">For example, Cortana will open the microphone to accept input from the user if the user holds down the microphone button.</span></span> <span data-ttu-id="0d7de-114">En el ejemplo de código siguiente se crea un mensaje que indica que el bot acepta la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="0d7de-114">The following code example creates a message that indicates the bot is accepting user input.</span></span>

[!code-javascript[IMessage.speak](../includes/code/node-input-hints.js#InputHintAcceptingInput)]

## <a name="expecting-input"></a><span data-ttu-id="0d7de-115">Esperar entradas</span><span class="sxs-lookup"><span data-stu-id="0d7de-115">Expecting input</span></span>

<span data-ttu-id="0d7de-116">Para indicar que el bot espera una respuesta del usuario, establezca la sugerencia de entrada del mensaje en `builder.InputHint.expectingInput`.</span><span class="sxs-lookup"><span data-stu-id="0d7de-116">To indicate that your bot is awaiting a response from the user, set the message's input hint to `builder.InputHint.expectingInput`.</span></span> <span data-ttu-id="0d7de-117">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se active.</span><span class="sxs-lookup"><span data-stu-id="0d7de-117">On many channels, this will cause the client's input box to be enabled and microphone to be open.</span></span> <span data-ttu-id="0d7de-118">En el ejemplo de código siguiente se crea un aviso que indica que el bot espera los datos de entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="0d7de-118">The following code example creates a prompt that indicates the bot is expecting user input.</span></span>

[!code-javascript[Prompt](../includes/code/node-input-hints.js#InputHintExpectingInput)]

## <a name="ignoring-input"></a><span data-ttu-id="0d7de-119">Ignorar entradas</span><span class="sxs-lookup"><span data-stu-id="0d7de-119">Ignoring input</span></span>

<span data-ttu-id="0d7de-120">Para indicar que el bot no está preparado para recibir la entrada del usuario, establezca la sugerencia de entrada del mensaje en `builder.InputHint.ignoringInput`.</span><span class="sxs-lookup"><span data-stu-id="0d7de-120">To indicate that your bot is not ready to receive input from the user, set the message's input hint to `builder.InputHint.ignoringInput`.</span></span> <span data-ttu-id="0d7de-121">En muchos canales, esto hará que se deshabilite el cuadro de entrada del cliente y que el micrófono se desactive.</span><span class="sxs-lookup"><span data-stu-id="0d7de-121">On many channels, this will cause the client's input box to be disabled and microphone to be closed.</span></span> <span data-ttu-id="0d7de-122">El ejemplo de código siguiente usa el método `session.say()` para enviar un mensaje que indica que el bot ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="0d7de-122">The following code example uses the `session.say()` method to send a message that indicates the bot is ignoring user input.</span></span>

[!code-javascript[Session.say()](../includes/code/node-input-hints.js#InputHintIgnoringInput)]

## <a name="default-values-for-input-hint"></a><span data-ttu-id="0d7de-123">Valores predeterminados para las sugerencias de entrada</span><span class="sxs-lookup"><span data-stu-id="0d7de-123">Default values for input hint</span></span>

<span data-ttu-id="0d7de-124">Si no establece la sugerencia de entrada para un mensaje, Bot Builder SDK la establecerá automáticamente mediante esta lógica:</span><span class="sxs-lookup"><span data-stu-id="0d7de-124">If you do not set the input hint for a message, the Bot Builder SDK will automatically set it for you by using this logic:</span></span> 

- <span data-ttu-id="0d7de-125">Si el bot envía un aviso, en la sugerencia de datos de entrada del mensaje se especificará que el bot **espera datos de entrada**.</span><span class="sxs-lookup"><span data-stu-id="0d7de-125">If your bot sends a prompt, the input hint for the message will specify that your bot is **expecting input**.</span></span></li>
- <span data-ttu-id="0d7de-126">Si el bot envía un solo mensaje, en la sugerencia de entrada del mensaje se especificará que el bot **acepta entradas**.</span><span class="sxs-lookup"><span data-stu-id="0d7de-126">If your bot sends single message, the input hint for the message will specify that your bot is **accepting input**.</span></span></li>
- <span data-ttu-id="0d7de-127">Si el bot envía una serie de mensajes consecutivos, la sugerencia de entrada de todos los mensajes excepto el último de la serie especificará que el bot **ignora las entradas**, mientras que la sugerencia de entrada del último mensaje de la serie especificará que el bot **acepta entradas**.</span><span class="sxs-lookup"><span data-stu-id="0d7de-127">If your bot sends a series of consecutive messages, the input hint for all but the final message in the series will specify that your bot is **ignoring input**, and the input hint for the final message in the series will specify that your bot is **accepting input**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d7de-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0d7de-128">Additional resources</span></span>

- [<span data-ttu-id="0d7de-129">Incorporación de voz a los mensajes</span><span class="sxs-lookup"><span data-stu-id="0d7de-129">Add speech to messages</span></span>](bot-builder-nodejs-text-to-speech.md)
- <span data-ttu-id="0d7de-130">[Referencia de Bot Builder SDK para Node.js][SDKReference]</span><span class="sxs-lookup"><span data-stu-id="0d7de-130">[Bot Builder SDK for Node.js Reference][SDKReference]</span></span>

[SDKReference]: https://docs.botframework.com/en-us/node/builder/chat-reference/modules/_botbuilder_d_.html

