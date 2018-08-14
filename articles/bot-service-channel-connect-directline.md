---
title: Conectar un bot con Direct Line | Microsoft Docs
description: Obtenga información sobre cómo configurar la conexión de un bot con Direct Line.
keywords: Direct Line, canales de bot, cliente personalizado, conexión con canales, configuración
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/08/2018
ms.openlocfilehash: 2ec11c4cc0e12b6a130b2f703f30660c4bc9f072
ms.sourcegitcommit: f95702d27abbd242c902eeb218d55a72df56ce56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39306493"
---
# <a name="connect-a-bot-to-direct-line"></a><span data-ttu-id="e0f5c-104">Conectar un bot con Direct Line</span><span class="sxs-lookup"><span data-stu-id="e0f5c-104">Connect a bot to Direct Line</span></span>

<span data-ttu-id="e0f5c-105">Para permitir que su propia aplicación cliente se comunique con su bot, puede usar el canal de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-105">You can enable your own client application to communicate with your bot by using the Direct Line channel.</span></span> 

## <a name="add-the-direct-line-channel"></a><span data-ttu-id="e0f5c-106">Agregar el canal de Direct Line</span><span class="sxs-lookup"><span data-stu-id="e0f5c-106">Add the Direct Line channel</span></span>

<span data-ttu-id="e0f5c-107">Para agregar el canal de Direct Line, abra el bot en [Azure Portal](https://portal.azure.com/), haga clic en la pestaña **Canales** y, luego, en **Direct Line**.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-107">To add the Direct Line channel, open the bot in the [Azure Portal](https://portal.azure.com/), click the **Channels** blade, and then click **Direct Line**.</span></span>

![Agregar el canal de Direct Line](~/media/bot-service-channel-connect-directline/directline-addchannel.png)

## <a name="add-new-site"></a><span data-ttu-id="e0f5c-109">Agregar un nuevo sitio</span><span class="sxs-lookup"><span data-stu-id="e0f5c-109">Add new site</span></span>

<span data-ttu-id="e0f5c-110">Después, agregue un nuevo sitio que represente la aplicación cliente que quiere conectar con el bot.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-110">Next, add a new site that represents the client application that you want to connect to your bot.</span></span> <span data-ttu-id="e0f5c-111">Haga clic en **Add new site** (Agregar nuevo sitio), escriba un nombre para el sitio y haga clic en **Listo**.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-111">Click **Add new site**, enter a name for your site, and click **Done**.</span></span>

![Agregar un sitio de Direct Line](~/media/bot-service-channel-connect-directline/directline-addsite.png)

## <a name="manage-secret-keys"></a><span data-ttu-id="e0f5c-113">Administrar claves secretas</span><span class="sxs-lookup"><span data-stu-id="e0f5c-113">Manage secret keys</span></span>

<span data-ttu-id="e0f5c-114">Cuando se cree el sitio, Bot Framework generará unas claves secretas que la aplicación cliente puede usar para [autenticar](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md) las solicitudes de Direct Line API que emite para comunicarse con el bot.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-114">When your site is created, the Bot Framework generates secret keys that your client application can use to [authenticate](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md) the Direct Line API requests that it issues to communicate with your bot.</span></span> <span data-ttu-id="e0f5c-115">Para ver una clave en texto sin formato, haga clic en **Mostrar** junto a la clave correspondiente.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-115">To view a key in plain text, click **Show** for the corresponding key.</span></span>

![Mostrar la clave de Direct Line](~/media/bot-service-channel-connect-directline/directline-showkey.png)

<span data-ttu-id="e0f5c-117">Copie y almacene de forma segura la clave que se muestra.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-117">Copy and securely store the key that is shown.</span></span> <span data-ttu-id="e0f5c-118">Después, use la clave para [autenticar](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md) las solicitudes de Direct Line API que el cliente emite para comunicarse con el bot.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-118">Then use the key to [authenticate](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md) the Direct Line API requests that your client issues to communicate with your bot.</span></span>
<span data-ttu-id="e0f5c-119">También puede usar Direct Line API para [intercambiar la clave por un token](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md#generate-token) que el cliente pueda usar para autenticar sus solicitudes posteriores en el ámbito de una sola conversación.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-119">Alternatively, use the Direct Line API to [exchange the key for a token](~/rest-api/bot-framework-rest-direct-line-3-0-authentication.md#generate-token) that your client can use to authenticate its subsequent requests within the scope of a single conversation.</span></span>

![Copiar la clave de Direct Line](~/media/bot-service-channel-connect-directline/directline-copykey.png)

## <a name="configure-settings"></a><span data-ttu-id="e0f5c-121">Definición de la configuración</span><span class="sxs-lookup"><span data-stu-id="e0f5c-121">Configure settings</span></span>

<span data-ttu-id="e0f5c-122">Por último, configure los valores del sitio.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-122">Finally, configure settings for the site.</span></span>

- <span data-ttu-id="e0f5c-123">Seleccione la versión de protocolo de Direct Line que la aplicación cliente usará para comunicarse con el bot.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-123">Select the Direct Line protocol version that your client application will use to communicate with your bot.</span></span>

> [!TIP]
> <span data-ttu-id="e0f5c-124">Si va a crear una conexión entre la aplicación cliente y un bot, use Direct Line API 3.0.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-124">If you are creating a new connection between your client application and bot, use Direct Line API 3.0.</span></span>

<span data-ttu-id="e0f5c-125">Cuando termine, haga clic en **Listo** para guardar la configuración del sitio.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-125">When finished, click **Done** to save the site configuration.</span></span> <span data-ttu-id="e0f5c-126">Puede repetir este proceso desde el paso [Agregar un nuevo sitio](#add-new-site) para cada aplicación cliente que quiera conectar al bot.</span><span class="sxs-lookup"><span data-stu-id="e0f5c-126">You can repeat this process, beginning with [Add new site](#add-new-site), for each client application that you want to connect to your bot.</span></span>
