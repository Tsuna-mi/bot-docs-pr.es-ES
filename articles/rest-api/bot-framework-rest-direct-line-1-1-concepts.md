---
title: Conceptos clave de la API Direct Line 1.1 de Bot Framework | Microsoft Docs
description: Comprenda los conceptos claves la API Direct Line 1.1 de Bot Framework.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: f37d8e9215b0a2cd640431f237d1b8c53fad576b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304788"
---
# <a name="key-concepts-in-direct-line-api-11"></a><span data-ttu-id="40d98-103">Conceptos clave de la API Direct Line 1.1 de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="40d98-103">Key concepts in Direct Line API 1.1</span></span>

<span data-ttu-id="40d98-104">Para habilitar la comunicación entre el bot y su propia aplicación cliente, use la API Direct Line.</span><span class="sxs-lookup"><span data-stu-id="40d98-104">You can enable communication between your bot and your own client application by using the Direct Line API.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="40d98-105">En este artículo se presentan los conceptos clave de la API Direct Line 1.1 y se proporciona información acerca de los recursos pertinentes para desarrolladores.</span><span class="sxs-lookup"><span data-stu-id="40d98-105">This article introduces key concepts in Direct Line API 1.1 and provides information about relevant developer resources.</span></span> <span data-ttu-id="40d98-106">Si va a crear una nueva conexión entre la aplicación cliente y un bot, use [API Direct Line 3.0](bot-framework-rest-direct-line-3-0-concepts.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="40d98-106">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-concepts.md) instead.</span></span>

## <a name="authentication"></a><span data-ttu-id="40d98-107">Autenticación</span><span class="sxs-lookup"><span data-stu-id="40d98-107">Authentication</span></span>

<span data-ttu-id="40d98-108">Las solicitudes de la API Direct Line 1.1 se pueden autenticar ya sea mediante un **secreto**, que se obtiene desde la página de configuración del canal de Direct Line en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>, o mediante un **token** , que se obtiene en el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="40d98-108">Direct Line API 1.1 requests can be authenticated either by using a **secret** that you obtain from the Direct Line channel configuration page in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a> or by using a **token** that you obtain at runtime.</span></span>  <span data-ttu-id="40d98-109">Para más información, consulte [Autenticación](bot-framework-rest-direct-line-1-1-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="40d98-109">For more information, see [Authentication](bot-framework-rest-direct-line-1-1-authentication.md).</span></span>

## <a name="starting-a-conversation"></a><span data-ttu-id="40d98-110">Inicio de una conversación</span><span class="sxs-lookup"><span data-stu-id="40d98-110">Starting a conversation</span></span>

<span data-ttu-id="40d98-111">Los clientes pueden abrir las conversaciones de Direct Line explícitamente y estas se pueden ejecutar siempre y cuando el cliente y el bot participen y tengan credenciales válidas.</span><span class="sxs-lookup"><span data-stu-id="40d98-111">Direct Line conversations are explicitly opened by clients and may run as long as the bot and client participate and have valid credentials.</span></span> <span data-ttu-id="40d98-112">Para obtener más información, consulte [Start a conversation](bot-framework-rest-direct-line-1-1-start-conversation.md) (Inicio de una conversación).</span><span class="sxs-lookup"><span data-stu-id="40d98-112">For more information, see [Start a conversation](bot-framework-rest-direct-line-1-1-start-conversation.md).</span></span>

## <a name="sending-messages"></a><span data-ttu-id="40d98-113">Envío de mensajes</span><span class="sxs-lookup"><span data-stu-id="40d98-113">Sending messages</span></span>

<span data-ttu-id="40d98-114">Con la API Direct Line 1.1, un cliente puede enviar mensajes a su bot mediante la emisión de solicitudes `HTTP POST`.</span><span class="sxs-lookup"><span data-stu-id="40d98-114">Using Direct Line API 1.1, a client can send messages to your bot by issuing `HTTP POST` requests.</span></span> <span data-ttu-id="40d98-115">Un cliente puede enviar un único mensaje por solicitud.</span><span class="sxs-lookup"><span data-stu-id="40d98-115">A client may send a single message per request.</span></span> <span data-ttu-id="40d98-116">Para obtener más información, consulte [Send a message to the bot](bot-framework-rest-direct-line-1-1-send-message.md) (Envío de un mensaje al bot).</span><span class="sxs-lookup"><span data-stu-id="40d98-116">For more information, see [Send a message to the bot](bot-framework-rest-direct-line-1-1-send-message.md).</span></span>

## <a name="receiving-messages"></a><span data-ttu-id="40d98-117">Recepción de mensajes</span><span class="sxs-lookup"><span data-stu-id="40d98-117">Receiving messages</span></span>

<span data-ttu-id="40d98-118">Con la API Direct Line 1.1, un cliente puede recibir mensajes mediante el sondeo de solicitudes `HTTP GET`.</span><span class="sxs-lookup"><span data-stu-id="40d98-118">Using Direct Line API 1.1, a client can receive messages by polling with `HTTP GET` requests.</span></span> <span data-ttu-id="40d98-119">En respuesta a cada solicitud, un cliente puede recibir varios mensajes del bot como parte de `MessageSet`.</span><span class="sxs-lookup"><span data-stu-id="40d98-119">In response to each request, a client may receive multiple messages from the bot as part of a `MessageSet`.</span></span> <span data-ttu-id="40d98-120">Para obtener más información, consulte [Receive messages from the bot](bot-framework-rest-direct-line-1-1-receive-messages.md) (Recepción de mensajes del bot).</span><span class="sxs-lookup"><span data-stu-id="40d98-120">For more information, see [Receive messages from the bot](bot-framework-rest-direct-line-1-1-receive-messages.md).</span></span>

## <a name="developer-resources"></a><span data-ttu-id="40d98-121">Recursos para el desarrollador</span><span class="sxs-lookup"><span data-stu-id="40d98-121">Developer resources</span></span>

### <a name="client-library"></a><span data-ttu-id="40d98-122">Biblioteca de cliente</span><span class="sxs-lookup"><span data-stu-id="40d98-122">Client library</span></span>

<span data-ttu-id="40d98-123">Bot Framework proporciona una biblioteca cliente que facilita el acceso a la API Direct Line 1.1 a través de C#.</span><span class="sxs-lookup"><span data-stu-id="40d98-123">The Bot Framework provides a client library that facilitates access to Direct Line API 1.1 via C#.</span></span> <span data-ttu-id="40d98-124">Para usar la biblioteca cliente dentro de un proyecto de Visual Studio, instale el <a href="https://www.nuget.org/packages/Microsoft.Bot.Connector.DirectLine/1.1.1" target="_blank">paquete NuGet v1.x </a> `Microsoft.Bot.Connector.DirectLine`.</span><span class="sxs-lookup"><span data-stu-id="40d98-124">To use the client library within a Visual Studio project, install the `Microsoft.Bot.Connector.DirectLine` <a href="https://www.nuget.org/packages/Microsoft.Bot.Connector.DirectLine/1.1.1" target="_blank">v1.x NuGet package</a>.</span></span> 

<span data-ttu-id="40d98-125">Como alternativa al uso de la biblioteca cliente de C#, puede generar su propia biblioteca cliente en el lenguaje que quiera mediante el <a href="https://docs.botframework.com/en-us/restapi/directline/swagger.json" target="_blank">archivo Swagger de la API Direct Line 1.1</a>.</span><span class="sxs-lookup"><span data-stu-id="40d98-125">As an alternative to using the C# client library, you can generate your own client library in the language of your choice by using the <a href="https://docs.botframework.com/en-us/restapi/directline/swagger.json" target="_blank">Direct Line API 1.1 Swagger file</a>.</span></span>

### <a name="web-chat-control"></a><span data-ttu-id="40d98-126">Control Chat en web</span><span class="sxs-lookup"><span data-stu-id="40d98-126">Web chat control</span></span> 

<span data-ttu-id="40d98-127">Bot Framework proporciona un control que le permite insertar un bot con tecnología de Direct Line en la aplicación cliente.</span><span class="sxs-lookup"><span data-stu-id="40d98-127">The Bot Framework provides a control that enables you to embed a Direct-Line-powered bot into your client application.</span></span> <span data-ttu-id="40d98-128">Para obtener más información, consulte <a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">Microsoft Bot Framework WebChat control</a> (Control Chat en web de Microsoft Bot Framework).</span><span class="sxs-lookup"><span data-stu-id="40d98-128">For more information, see the <a href="https://github.com/Microsoft/BotFramework-WebChat" target="_blank">Microsoft Bot Framework WebChat control</a>.</span></span>
