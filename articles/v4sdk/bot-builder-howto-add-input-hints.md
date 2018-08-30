---
title: Incorporación de sugerencias de entrada a mensajes | Microsoft Docs
description: Obtenga información sobre cómo agregar sugerencias de entrada a mensajes mediante Bot Builder SDK.
keywords: Sugerencias de entrada, aceptar entradas, esperar entradas, ignorar entradas, voz
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/24/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 02c6cf56a0c1161fa0393880810c1481c5eb2461
ms.sourcegitcommit: f89ed979eb6321232fb21100ef376d9b0d5113c9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42914635"
---
# <a name="add-input-hints-to-messages"></a><span data-ttu-id="6a970-104">Incorporación de sugerencias de entrada a mensajes</span><span class="sxs-lookup"><span data-stu-id="6a970-104">Add input hints to messages</span></span>

[!INCLUDE [pre-release-label](~/includes/pre-release-label.md)]

<span data-ttu-id="6a970-105">Al especificar una sugerencia de entrada para un mensaje, puede indicar si el bot acepta, espera o ignora la entrada del usuario después de que el mensaje se haya entregado al cliente.</span><span class="sxs-lookup"><span data-stu-id="6a970-105">By specifying an input hint for a message, you can indicate whether your bot is accepting, expecting, or ignoring user input after the message is delivered to the client.</span></span> <span data-ttu-id="6a970-106">En muchos canales, esto permite a los clientes establecer en consecuencia el estado de los controles de entrada de usuario.</span><span class="sxs-lookup"><span data-stu-id="6a970-106">For many channels, this enables clients to set the state of user input controls accordingly.</span></span> <span data-ttu-id="6a970-107">Por ejemplo, si la sugerencia de entrada de un mensaje indica que el bot ignora la entrada de usuario, el cliente podría desactivar el micrófono y deshabilitar el cuadro de entrada para impedir que el usuario proporcione una entrada.</span><span class="sxs-lookup"><span data-stu-id="6a970-107">For example, if a message's input hint indicates that the bot is ignoring user input, the client may close the microphone and disable the input box to prevent the user from providing input.</span></span>

<span data-ttu-id="6a970-108">Asegúrese de que se incluyen las bibliotecas necesarias para las sugerencias de entrada.</span><span class="sxs-lookup"><span data-stu-id="6a970-108">Be sure the necessary libraries are included for input hints.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="6a970-109">C#</span><span class="sxs-lookup"><span data-stu-id="6a970-109">C#</span></span>](#tab/cs)

```cs
using Microsoft.Bot.Schema;
```

<!--TODO: Remove the following remark after the next release of the NuGet packages.-->

<span data-ttu-id="6a970-110">En el siguiente espacio de nombres se define la clase **MessageFactory**, que se usa en estos ejemplos.</span><span class="sxs-lookup"><span data-stu-id="6a970-110">The **MessageFactory** class, used in these examples, is defined in the following namespace.</span></span>

```cs
using Microsoft.Bot.Builder.Core.Extensions;
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="6a970-111">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a970-111">JavaScript</span></span>](#tab/js)

```javascript
const {InputHints, MessageFactory} = require('botbuilder');
```

---

## <a name="accepting-input"></a><span data-ttu-id="6a970-112">Aceptar entradas</span><span class="sxs-lookup"><span data-stu-id="6a970-112">Accepting input</span></span>

<span data-ttu-id="6a970-113">Para indicar que el bot está preparado pasivamente para la entrada, pero que no espera ninguna respuesta del usuario, establezca la sugerencia de entrada del mensaje en _accepting input_.</span><span class="sxs-lookup"><span data-stu-id="6a970-113">To indicate that your bot is passively ready for input but is not awaiting a response from the user, set the message's input hint to _accepting input_.</span></span> <span data-ttu-id="6a970-114">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se desactive, sin dejar de estar accesible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="6a970-114">On many channels, this will cause the client's input box to be enabled, and microphone to be closed but still accessible to the user.</span></span> <span data-ttu-id="6a970-115">Por ejemplo, Cortana activará el micrófono para aceptar la entrada del usuario si este mantiene presionado el botón de micrófono.</span><span class="sxs-lookup"><span data-stu-id="6a970-115">For example, Cortana will open the microphone to accept input from the user if the user holds down the microphone button.</span></span> <span data-ttu-id="6a970-116">En el código siguiente se crea un mensaje que indica que el bot acepta la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="6a970-116">The following code creates a message that indicates the bot is accepting user input.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="6a970-117">C#</span><span class="sxs-lookup"><span data-stu-id="6a970-117">C#</span></span>](#tab/cs)

```csharp
var reply = MessageFactory.Text(
    "This is the text that will be displayed.",
    "This is the text that will be spoken.",
    InputHints.AcceptingInput);
await context.SendActivity(reply).ConfigureAwait(false);
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="6a970-118">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a970-118">JavaScript</span></span>](#tab/js)

```javascript
const basicMessage = MessageFactory.text('This is the text that will be displayed.', 'This is the text that will be spoken.', InputHints.AcceptingInput);
await context.sendActivity(basicMessage);
```

---

## <a name="expecting-input"></a><span data-ttu-id="6a970-119">Esperar entradas</span><span class="sxs-lookup"><span data-stu-id="6a970-119">Expecting input</span></span>

<span data-ttu-id="6a970-120">Para indicar que el bot espera una respuesta del usuario, establezca la sugerencia de entrada del mensaje en _expecting input_.</span><span class="sxs-lookup"><span data-stu-id="6a970-120">To indicate that your bot is awaiting a response from the user, set the message's input hint to _expecting input_.</span></span> <span data-ttu-id="6a970-121">En muchos canales, esto hará que se habilite el cuadro de entrada del cliente y que el micrófono se active.</span><span class="sxs-lookup"><span data-stu-id="6a970-121">On many channels, this will cause the client's input box to be enabled and microphone to be open.</span></span> <span data-ttu-id="6a970-122">En el ejemplo de código siguiente se crea un mensaje que indica que el bot espera la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="6a970-122">The following code example creates a message that indicates the bot is expecting user input.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="6a970-123">C#</span><span class="sxs-lookup"><span data-stu-id="6a970-123">C#</span></span>](#tab/cs)

```csharp
var reply = MessageFactory.Text(
    "This is the text that will be displayed.",
    "This is the text that will be spoken.",
    InputHints.ExpectingInput);
await context.SendActivity(reply).ConfigureAwait(false);
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="6a970-124">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a970-124">JavaScript</span></span>](#tab/js)

```javascript
const basicMessage = MessageFactory.text('This is the text that will be displayed.', 'This is the text that will be spoken.', InputHints.ExpectingInput);
await context.sendActivity(basicMessage);
```

---

## <a name="ignoring-input"></a><span data-ttu-id="6a970-125">Ignorar entradas</span><span class="sxs-lookup"><span data-stu-id="6a970-125">Ignoring input</span></span>

<span data-ttu-id="6a970-126">Para indicar que el bot no está preparado para recibir la entrada del usuario, establezca la sugerencia de entrada del mensaje en _ignoring input_.</span><span class="sxs-lookup"><span data-stu-id="6a970-126">To indicate that your bot is not ready to receive input from the user, set the message's input hint to _ignoring input_.</span></span> <span data-ttu-id="6a970-127">En muchos canales, esto hará que se deshabilite el cuadro de entrada del cliente y que el micrófono se desactive.</span><span class="sxs-lookup"><span data-stu-id="6a970-127">On many channels, this will cause the client's input box to be disabled and microphone to be closed.</span></span> <span data-ttu-id="6a970-128">En el ejemplo de código siguiente se crea un mensaje que indica que el bot ignora la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="6a970-128">The following code example creates a message that indicates the bot is ignoring user input.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="6a970-129">C#</span><span class="sxs-lookup"><span data-stu-id="6a970-129">C#</span></span>](#tab/cs)

```csharp
var reply = MessageFactory.Text(
    "This is the text that will be displayed.",
    "This is the text that will be spoken.",
    InputHints.IgnoringInput);
await context.SendActivity(reply).ConfigureAwait(false);
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="6a970-130">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a970-130">JavaScript</span></span>](#tab/js)

```javascript
const basicMessage = MessageFactory.text('This is the text that will be displayed.', 'This is the text that will be spoken.', InputHints.IgnoringInput);
await context.sendActivity(basicMessage);
```

---

## <a name="default-values-for-input-hint"></a><span data-ttu-id="6a970-131">Valores predeterminados para las sugerencias de entrada</span><span class="sxs-lookup"><span data-stu-id="6a970-131">Default values for input hint</span></span>

<span data-ttu-id="6a970-132">Si no establece la sugerencia de entrada para un mensaje, Bot Builder SDK la establecerá automáticamente mediante esta lógica:</span><span class="sxs-lookup"><span data-stu-id="6a970-132">If you do not set the input hint for a message, the Bot Builder SDK will automatically set it for you by using this logic:</span></span>

- <span data-ttu-id="6a970-133">Si el bot envía un aviso, en la sugerencia de entrada del mensaje se especificará que el bot **espera una entrada**.</span><span class="sxs-lookup"><span data-stu-id="6a970-133">If your bot sends a prompt, the input hint for the message will specify that your bot is **expecting input**.</span></span></li>
- <span data-ttu-id="6a970-134">Si el bot envía un solo mensaje, en la sugerencia de entrada del mensaje se especificará que el bot **acepta entradas**.</span><span class="sxs-lookup"><span data-stu-id="6a970-134">If your bot sends a single message, the input hint for the message will specify that your bot is **accepting input**.</span></span></li>
- <span data-ttu-id="6a970-135">Si el bot envía una serie de mensajes consecutivos, la sugerencia de entrada de todos los mensajes excepto el último de la serie especificará que el bot **ignora las entradas**, mientras que la sugerencia de entrada del último mensaje de la serie especificará que el bot **acepta entradas**.</span><span class="sxs-lookup"><span data-stu-id="6a970-135">If your bot sends a series of consecutive messages, the input hint for all but the final message in the series will specify that your bot is **ignoring input**, and the input hint for the final message in the series will specify that your bot is **accepting input**.</span></span>
