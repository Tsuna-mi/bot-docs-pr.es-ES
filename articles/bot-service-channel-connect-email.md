---
title: Conexión de un bot al correo electrónico de Office 365 | Microsoft Docs
description: Aprenda a configurar un bot para enviar y recibir correo electrónico con Office 365.
keywords: Office 365, canales de bots, correo electrónico, credenciales de correo electrónico, azure portal, correo electrónico personalizado
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 10/10/2018
ms.openlocfilehash: 3aaf66352b0562faaa5da0043dc43a7244bec796
ms.sourcegitcommit: 54ed5000c67a5b59e23b667547565dd96c7302f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49315111"
---
# <a name="connect-a-bot-to-office-365-email"></a><span data-ttu-id="af096-104">Conexión de un bot al correo electrónico de Office 365</span><span class="sxs-lookup"><span data-stu-id="af096-104">Connect a bot to Office 365 email</span></span>

<span data-ttu-id="af096-105">Los bots pueden comunicarse con los usuarios mediante el correo electrónico de Office 365 además de otros [canales](~/bot-service-manage-channels.md).</span><span class="sxs-lookup"><span data-stu-id="af096-105">Bots can communicate with users via Office 365 email in addition to other [channels](~/bot-service-manage-channels.md).</span></span> <span data-ttu-id="af096-106">Cuando se configura un bot para acceder a una cuenta de correo electrónico, recibe un mensaje cuando llega un nuevo correo.</span><span class="sxs-lookup"><span data-stu-id="af096-106">When a bot is configured to access an email account, it receives a message when a new email arrives.</span></span> <span data-ttu-id="af096-107">El bot puede responder entonces tal y como indica su lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="af096-107">The bot can then respond as indicated by its business logic.</span></span> <span data-ttu-id="af096-108">Por ejemplo, el bot podría enviar un correo electrónico de respuesta para confirmar un correo electrónico que se recibió con el mensaje "¡Hola!</span><span class="sxs-lookup"><span data-stu-id="af096-108">For example, the bot could send an email reply acknowledging an email was received with the message, "Hi!</span></span> <span data-ttu-id="af096-109">Le agradecemos su pedido.</span><span class="sxs-lookup"><span data-stu-id="af096-109">Thanks for your order!</span></span> <span data-ttu-id="af096-110">Comenzaremos a procesarlo inmediatamente".</span><span class="sxs-lookup"><span data-stu-id="af096-110">We will begin processing it immediately."</span></span>

> [!WARNING]
> <span data-ttu-id="af096-111">Es una infracción del [Código de conducta](https://www.botframework.com/Content/Microsoft-Bot-Framework-Preview-Online-Services-Agreement.htm) de Bot Framework crear "spambots", lo que incluye bots que envían correo masivo no deseado ni solicitado.</span><span class="sxs-lookup"><span data-stu-id="af096-111">It is a violation of the Bot Framework [Code of Conduct](https://www.botframework.com/Content/Microsoft-Bot-Framework-Preview-Online-Services-Agreement.htm) to create "spambots", including bots that send unwanted or unsolicited bulk email.</span></span>

## <a name="configure-email-credentials"></a><span data-ttu-id="af096-112">Configuración de las credenciales de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="af096-112">Configure email credentials</span></span>

<span data-ttu-id="af096-113">Para conectar un bot al canal de correo electrónico, debe escribir las credenciales de Office 365 en la configuración de canal de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="af096-113">You can connect a bot to the Email channel by entering Office 365 credentials in the Email channel configuration.</span></span>
<span data-ttu-id="af096-114">No se admite la autenticación federada con un proveedor que reemplace a AAD.</span><span class="sxs-lookup"><span data-stu-id="af096-114">Federated authentication using any vendor that replaces AAD is not supported.</span></span>

> [!NOTE]
> <span data-ttu-id="af096-115">No use sus propias cuentas de correo electrónico personal con bots, ya que todos los mensajes enviados a esa cuenta de correo electrónico se reenviarán al bot.</span><span class="sxs-lookup"><span data-stu-id="af096-115">You should not use your own personal email accounts for bots, as every message sent to that email account will be forwarded to the bot.</span></span> <span data-ttu-id="af096-116">Como consecuencia, el bot puede enviar una respuesta al remitente de manera inadecuada.</span><span class="sxs-lookup"><span data-stu-id="af096-116">This can result in the bot inappropriately sending a response to a sender.</span></span> <span data-ttu-id="af096-117">Por este motivo, los bots solo deben usar cuentas de correo electrónico de Office 365 dedicadas.</span><span class="sxs-lookup"><span data-stu-id="af096-117">For this reason, bots should only use dedicated O365 email accounts.</span></span>

<span data-ttu-id="af096-118">Para agregar el canal de correo electrónico, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la hoja **Canales** y luego en **Correo electrónico**.</span><span class="sxs-lookup"><span data-stu-id="af096-118">To add the Email channel, open the bot in the [Azure Portal](https://portal.azure.com/), click the **Channels** blade, and then click **Email**.</span></span> <span data-ttu-id="af096-119">Escriba sus credenciales de correo electrónico válidas y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="af096-119">Enter your valid email credentials and click **Save**.</span></span>

![Escribir las credenciales de correo electrónico](~/media/bot-service-channel-connect-email/bot-service-channel-connect-email-credentials.png)

<span data-ttu-id="af096-121">El canal de correo electrónico solo funciona actualmente con Office 365.</span><span class="sxs-lookup"><span data-stu-id="af096-121">The Email channel currently works with Office 365 only.</span></span> <span data-ttu-id="af096-122">Otros servicios de correo electrónico no se admiten actualmente.</span><span class="sxs-lookup"><span data-stu-id="af096-122">Other email services are not currently supported.</span></span>

## <a name="customize-emails"></a><span data-ttu-id="af096-123">Personalización de los mensajes de correo electrónico</span><span class="sxs-lookup"><span data-stu-id="af096-123">Customize emails</span></span>

<span data-ttu-id="af096-124">El canal de correo electrónico admite el envío de propiedades personalizadas para crear correos electrónicos personalizados más avanzados mediante la propiedad `channelData`.</span><span class="sxs-lookup"><span data-stu-id="af096-124">The Email channel supports sending custom properties to create more advanced, customized emails using the `channelData` property.</span></span>

[!INCLUDE [Email channelData table](~/includes/snippet-channelData-email.md)]

<span data-ttu-id="af096-125">El mensaje de ejemplo siguiente muestra un archivo JSON que incluye estas propiedades `channelData`.</span><span class="sxs-lookup"><span data-stu-id="af096-125">The following example message shows a JSON file that includes these `channelData` properties.</span></span>

```json
{
    "type": "message",
    "locale": "en-Us",
    "channelID": "email",
    "from": { "id": "mybot@mydomain.com", "name": "My bot"},
    "recipient": { "id": "joe@otherdomain.com", "name": "Joe Doe"},
    "conversation": { "id": "123123123123", "topic": "awesome chat" },
    "channelData":
    {
        "htmlBody": "<html><body style = /"font-family: Calibri; font-size: 11pt;/" >This is more than awesome.</body></html>",
        "subject": "Super awesome message subject",
        "importance": "high",
        "ccRecipients": "Yasemin@adatum.com;Temel@adventure-works.com"
    }
}
```

::: moniker range="azure-bot-service-3.0"
<span data-ttu-id="af096-126">Para más información sobre el uso de `channelData`, consulte el ejemplo de [Node.js](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-ChannelData) o la documentación de [.NET](~/dotnet/bot-builder-dotnet-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="af096-126">For more information about using `channelData`, see the [Node.js](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/core-ChannelData) sample or [.NET](~/dotnet/bot-builder-dotnet-channeldata.md) documentation.</span></span>
::: moniker-end

::: moniker range="azure-bot-service-4.0"
<span data-ttu-id="af096-127">Para más información acerca del uso de `channelData`, consulte [implementación de la funcionalidad específica de un canal](~/v4sdk/bot-builder-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="af096-127">For more information about using `channelData`, see [how to implement channel-specific functionality](~/v4sdk/bot-builder-channeldata.md).</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="af096-128">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="af096-128">Additional resources</span></span>

<!-- Put whole list in monikers, even though it's just the second item that needs to be different. -->
::: moniker range="azure-bot-service-3.0"
* <span data-ttu-id="af096-129">Conexión de un bot a [canales](~/bot-service-manage-channels.md)</span><span class="sxs-lookup"><span data-stu-id="af096-129">Connect a bot to [channels](~/bot-service-manage-channels.md)</span></span>
* <span data-ttu-id="af096-130">[Implementación de una funcionalidad específica de canal](dotnet/bot-builder-dotnet-channeldata.md) con Bot Builder SDK para .NET</span><span class="sxs-lookup"><span data-stu-id="af096-130">[Implement channel-specific functionality](dotnet/bot-builder-dotnet-channeldata.md) with the Bot Builder SDK for .NET</span></span>
* <span data-ttu-id="af096-131">Uso de [Channel Inspector](bot-service-channel-inspector.md) para ver cómo un canal representa una característica determinada de la aplicación de bot</span><span class="sxs-lookup"><span data-stu-id="af096-131">Use the [Channel Inspector](bot-service-channel-inspector.md) to see how a channel renders a particular feature of your bot application</span></span>
::: moniker-end
::: moniker range="azure-bot-service-4.0"
* <span data-ttu-id="af096-132">Conexión de un bot a [canales](~/bot-service-manage-channels.md)</span><span class="sxs-lookup"><span data-stu-id="af096-132">Connect a bot to [channels](~/bot-service-manage-channels.md)</span></span>
* <span data-ttu-id="af096-133">[Implementación de una funcionalidad específica de canal](~/v4sdk/bot-builder-channeldata.md) con Bot Builder SDK para .NET</span><span class="sxs-lookup"><span data-stu-id="af096-133">[Implement channel-specific functionality](~/v4sdk/bot-builder-channeldata.md) with the Bot Builder SDK for .NET</span></span>
* <span data-ttu-id="af096-134">Uso de [Channel Inspector](bot-service-channel-inspector.md) para ver cómo un canal representa una característica determinada de la aplicación de bot</span><span class="sxs-lookup"><span data-stu-id="af096-134">Use the [Channel Inspector](bot-service-channel-inspector.md) to see how a channel renders a particular feature of your bot application</span></span>
::: moniker-end
