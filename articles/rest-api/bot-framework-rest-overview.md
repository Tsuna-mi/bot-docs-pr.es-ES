---
title: Las API REST de Bot Builder | Microsoft Docs
description: Introducción a las API REST de Bot Framework que se pueden usar para crear bots y los clientes que se conectan a ellos.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: d6d83edb390933cb8895b26efeb9775cafdb1acb
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305236"
---
# <a name="bot-framework-rest-apis"></a><span data-ttu-id="2d29a-103">Las API REST de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="2d29a-103">Bot Framework REST APIs</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-overview.md)
> - [Node.js](../nodejs/bot-builder-nodejs-overview.md)
> - [REST](../rest-api/bot-framework-rest-overview.md)

<span data-ttu-id="2d29a-107">Las API REST de Bot Framework le permiten crear bots que intercambian mensajes con canales configurados en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>, almacenar y recuperar datos de estado y conectar sus propias aplicaciones cliente a sus bots.</span><span class="sxs-lookup"><span data-stu-id="2d29a-107">The Bot Framework REST APIs enable you to build bots that exchange messages with channels configured in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>, store and retrieve state data, and connect your own client applications to your bots.</span></span> <span data-ttu-id="2d29a-108">Todos los servicios de Bot Framework usan REST y JSON estándar a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="2d29a-108">All Bot Framework services use industry-standard REST and JSON over HTTPS.</span></span>

## <a name="build-a-bot"></a><span data-ttu-id="2d29a-109">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="2d29a-109">Build a bot</span></span>

<span data-ttu-id="2d29a-110">Puede crear un bot con cualquier lenguaje de programación mediante el servicio Bot Connector para intercambiar mensajes con canales configurados en el portal de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="2d29a-110">You can create a bot with any programming language by using the Bot Connector service to exchange messages with channels configured in the Bot Framework Portal.</span></span> 

> [!TIP]
> Bot Framework proporciona bibliotecas de cliente que se pueden usar para crear bots en C# o Node.js. Para crear un bot con C#, use el [SDK de Bot Builder para C#](../dotnet/bot-builder-dotnet-overview.md). Para crear un bot mediante Node.js, use el [SDK de Bot Builder para Node.js](../nodejs/index.md). 

<span data-ttu-id="2d29a-114">Para más información sobre la creación de bots con el servicio Bot Connector, consulte [Conceptos clave](bot-framework-rest-connector-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="2d29a-114">To learn more about building bots using the Bot Connector service, see [Key concepts](bot-framework-rest-connector-concepts.md).</span></span>

## <a name="build-a-client"></a><span data-ttu-id="2d29a-115">Creación de un cliente</span><span class="sxs-lookup"><span data-stu-id="2d29a-115">Build a client</span></span>

<span data-ttu-id="2d29a-116">Para permitir que su propia aplicación cliente se comunique con el bot, puede usar Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="2d29a-116">You can enable your own client application to communicate with your bot by using the Direct Line API.</span></span> <span data-ttu-id="2d29a-117">Direct Line API implementa un mecanismo de autenticación que emplea patrones estándar de secreto/token y proporciona un esquema estable, incluso si el bot cambia la versión de su protocolo.</span><span class="sxs-lookup"><span data-stu-id="2d29a-117">The Direct Line API implements an authentication mechanism that uses standard secret/token patterns and provides a stable schema, even if your bot changes its protocol version.</span></span> <span data-ttu-id="2d29a-118">Para más información sobre el uso de Direct Line API para habilitar la comunicación entre un cliente y el bot, consulte [Conceptos clave](bot-framework-rest-direct-line-3-0-concepts.md).</span><span class="sxs-lookup"><span data-stu-id="2d29a-118">To learn more about using the Direct Line API to enable communication between a client and your bot, see [Key concepts](bot-framework-rest-direct-line-3-0-concepts.md).</span></span> 