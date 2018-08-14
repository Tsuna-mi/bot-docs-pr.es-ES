---
title: Configuración de un bot para ejecutarlo en uno o más canales | Microsoft Docs
description: Obtenga información sobre cómo configurar un bot para ejecutarlo en uno o más canales mediante el Portal de Framework Bot.
keywords: canales de bot, configurar, cortana, facebook messenger, kik, slack, skype, azure portal
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: cb682bf77f801c98d00deffa0fc63249962248cd
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352924"
---
# <a name="connect-a-bot-to-channels"></a><span data-ttu-id="dfd99-104">Conexión de un bot a canales</span><span class="sxs-lookup"><span data-stu-id="dfd99-104">Connect a bot to channels</span></span>

<span data-ttu-id="dfd99-105">Un canal es una conexión entre Bot Framework y aplicaciones de comunicación.</span><span class="sxs-lookup"><span data-stu-id="dfd99-105">A channel is a connection between the Bot Framework and communication apps.</span></span> <span data-ttu-id="dfd99-106">Un bot se configura para conectarse a los canales en los que quiere que esté disponible.</span><span class="sxs-lookup"><span data-stu-id="dfd99-106">You configure a bot to connect to the channels you want it to be available on.</span></span> <span data-ttu-id="dfd99-107">Por ejemplo, un bot conectado al canal de Skype se puede agregar a una lista de contactos y los usuarios pueden interactuar con él en Skype.</span><span class="sxs-lookup"><span data-stu-id="dfd99-107">For example, a bot connected to the Skype channel can be added to a contact list and people can interact with it in Skype.</span></span> 

<span data-ttu-id="dfd99-108">Los canales incluyen muchos servicios populares, como [Cortana](bot-service-channel-connect-cortana.md), [Facebook Messenger](bot-service-channel-connect-facebook.md), [Kik](bot-service-channel-connect-kik.md) y [Slack](bot-service-channel-connect-slack.md), además de otros.</span><span class="sxs-lookup"><span data-stu-id="dfd99-108">The channels include many popular services, such as [Cortana](bot-service-channel-connect-cortana.md), [Facebook Messenger](bot-service-channel-connect-facebook.md), [Kik](bot-service-channel-connect-kik.md), and [Slack](bot-service-channel-connect-slack.md), as well as several others.</span></span> <span data-ttu-id="dfd99-109">[Skype](https://dev.skype.com/bots) y Chat en web están preconfigurados de forma automática.</span><span class="sxs-lookup"><span data-stu-id="dfd99-109">[Skype](https://dev.skype.com/bots) and Web Chat are pre-configured for you.</span></span> 

<span data-ttu-id="dfd99-110">La conexión a los canales es rápida y sencilla en [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="dfd99-110">Connecting to channels is quick and easy in the [Azure Portal](https://portal.azure.com).</span></span>

## <a name="get-started"></a><span data-ttu-id="dfd99-111">Introducción</span><span class="sxs-lookup"><span data-stu-id="dfd99-111">Get started</span></span>

<span data-ttu-id="dfd99-112">Para la mayoría de los canales, se debe proporcionar información de configuración de canal para ejecutar el bot en el canal.</span><span class="sxs-lookup"><span data-stu-id="dfd99-112">For most channels, you must provide channel configuration information to run your bot on the channel.</span></span> <span data-ttu-id="dfd99-113">La mayoría de los canales requieren que el bot tenga una cuenta en el canal y otros, como Facebook Messenger, requieren que también tenga una aplicación registrada con el canal.</span><span class="sxs-lookup"><span data-stu-id="dfd99-113">Most channels require that your bot have an account on the channel, and others, like Facebook Messenger, require your bot to have an application registered with the channel also.</span></span>

<span data-ttu-id="dfd99-114">Para configurar el bot para que se conecte a un canal, siga los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="dfd99-114">To configure your bot to connect to a channel, complete the following steps:</span></span>

1. <span data-ttu-id="dfd99-115">Inicie sesión en <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span><span class="sxs-lookup"><span data-stu-id="dfd99-115">Sign in to the <a href="https://portal.azure.com" target="_blank">Azure Portal</a>.</span></span>
1. <span data-ttu-id="dfd99-116">Seleccione el bot que quiera configurar.</span><span class="sxs-lookup"><span data-stu-id="dfd99-116">Select the bot that you want to configure.</span></span>
3. <span data-ttu-id="dfd99-117">En la hoja Servicio de bots, haga clic en **Canales** en **Administración de bots**.</span><span class="sxs-lookup"><span data-stu-id="dfd99-117">In the Bot Service blade, click **Channels** under **Bot Management**.</span></span>
4. <span data-ttu-id="dfd99-118">Haga clic en el icono del canal al que quiera agregar el bot.</span><span class="sxs-lookup"><span data-stu-id="dfd99-118">Click the icon of the channel you want to add to your bot.</span></span>

![Conexión a los canales](~/media/channels/connect-to-channels.png)

<span data-ttu-id="dfd99-120">Después de configurar el canal, los usuarios de ese canal pueden empezar a usar el bot.</span><span class="sxs-lookup"><span data-stu-id="dfd99-120">After you've configured the channel, users on that channel can start using your bot.</span></span>

## <a name="publish-a-bot"></a><span data-ttu-id="dfd99-121">Publicación de un bot</span><span class="sxs-lookup"><span data-stu-id="dfd99-121">Publish a bot</span></span>

<span data-ttu-id="dfd99-122">El proceso de publicación es diferente para cada canal.</span><span class="sxs-lookup"><span data-stu-id="dfd99-122">The publishing process is different for each channel.</span></span>

[!INCLUDE [publishing](~/includes/snippet-publish-to-channel.md)]

