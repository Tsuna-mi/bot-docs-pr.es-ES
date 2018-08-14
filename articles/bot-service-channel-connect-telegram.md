---
title: Creación de un bot de Telegram | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot a Telegram.
keywords: configurar bot, Telegram, canal de bots, bot de Telegram, token de acceso
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: ccd6bbbf30a9af83d6c687ee68f94d2f31c1c9cd
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305105"
---
# <a name="connect-a-bot-to-telegram"></a><span data-ttu-id="39b87-104">Conexión de un bot a Telegram</span><span class="sxs-lookup"><span data-stu-id="39b87-104">Connect a bot to Telegram</span></span>

<span data-ttu-id="39b87-105">Puede configurar el bot para que se comunique con las personas mediante la aplicación de mensajería Telegram.</span><span class="sxs-lookup"><span data-stu-id="39b87-105">You can configure your bot to communicate with people using the Telegram messaging app.</span></span>

[!INCLUDE [Channel Inspector intro](~/includes/snippet-channel-inspector.md)]

## <a name="visit-the-bot-father-to-create-a-new-telegram-bot"></a><span data-ttu-id="39b87-106">Visite BotFather para crear un nuevo bot de Telegram</span><span class="sxs-lookup"><span data-stu-id="39b87-106">Visit the Bot Father to create a new Telegram bot</span></span>

<span data-ttu-id="39b87-107"><a href="https://telegram.me/botfather" target="_blank">Creae un nuevo bot de Telegram</a> con BotFather.</span><span class="sxs-lookup"><span data-stu-id="39b87-107"><a href="https://telegram.me/botfather" target="_blank">Create a new Telegram bot</a> using the Bot Father.</span></span>

![Visitar BotFather](~/media/channels/tg-StepVisitBotFather.png)

## <a name="create-a-new-telegram-bot"></a><span data-ttu-id="39b87-109">Creación de un nuevo bot de Telegram</span><span class="sxs-lookup"><span data-stu-id="39b87-109">Create a new Telegram bot</span></span>
<span data-ttu-id="39b87-110">Para crear un nuevo bot de Telegram, envíe el comando `/newbot`.</span><span class="sxs-lookup"><span data-stu-id="39b87-110">To create a new Telegram bot, send command `/newbot`.</span></span>

![Crear nuevo bot](~/media/channels/tg-StepNewBot.png)

### <a name="specify-a-friendly-name"></a><span data-ttu-id="39b87-112">Especificar un nombre descriptivo</span><span class="sxs-lookup"><span data-stu-id="39b87-112">Specify a friendly name</span></span>

<span data-ttu-id="39b87-113">Asigne un nombre descriptivo al bot de Telegram.</span><span class="sxs-lookup"><span data-stu-id="39b87-113">Give the Telegram bot a friendly name.</span></span>

![Dar un nombre descriptivo al bot](~/media/channels/tg-StepNameBot.png)

### <a name="specify-a-username"></a><span data-ttu-id="39b87-115">Especificar un nombre de usuario</span><span class="sxs-lookup"><span data-stu-id="39b87-115">Specify a username</span></span>

<span data-ttu-id="39b87-116">Asigne un nombre de usuario único al bot de Telegram.</span><span class="sxs-lookup"><span data-stu-id="39b87-116">Give the Telegram bot a unique username.</span></span>

![Dar un nombre único al bot](~/media/channels/tg-StepUsername.png)

### <a name="copy-the-access-token"></a><span data-ttu-id="39b87-118">Copiar el token de acceso</span><span class="sxs-lookup"><span data-stu-id="39b87-118">Copy the access token</span></span>

<span data-ttu-id="39b87-119">Copie el token de acceso del bot de Telegram.</span><span class="sxs-lookup"><span data-stu-id="39b87-119">Copy the Telegram bot's access token.</span></span>

![Copiar token de acceso](~/media/channels/tg-StepBotCreated.png)

## <a name="enter-the-telegram-bots-access-token"></a><span data-ttu-id="39b87-121">Escribir el token de acceso del bot de Telegram</span><span class="sxs-lookup"><span data-stu-id="39b87-121">Enter the Telegram bot's access token</span></span>

<span data-ttu-id="39b87-122">Pegue el token que copió anteriormente en el campo **Token de acceso** y haga clic en **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="39b87-122">Paste the token you copied previously into the **Access Token** field and click **Submit**.</span></span>

## <a name="enable-the-bot"></a><span data-ttu-id="39b87-123">Habilitar el bot</span><span class="sxs-lookup"><span data-stu-id="39b87-123">Enable the bot</span></span>
<span data-ttu-id="39b87-124">Seleccione **Enable this bot on Telegram** (Habilitar este bot en Telegram).</span><span class="sxs-lookup"><span data-stu-id="39b87-124">Check **Enable this bot on Telegram**.</span></span> <span data-ttu-id="39b87-125">A continuación, haga clic en **I'm done configuring Telegram** (Finalicé la configuración de Telegram).</span><span class="sxs-lookup"><span data-stu-id="39b87-125">Then click **I'm done configuring Telegram**.</span></span>

<span data-ttu-id="39b87-126">Cuando haya completado estos pasos, el bot se configurará correctamente para comunicarse con los usuarios en Telegram.</span><span class="sxs-lookup"><span data-stu-id="39b87-126">When you have completed these steps, your bot will be successfully configured to communicate with users in Telegram.</span></span>
