---
title: Autenticación | Microsoft Docs
description: Aprenda a autenticar solicitudes de API en Direct Line API v1.1.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 6e83cc45e925f5d94b70260de6ac54e6f4052ca4
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305113"
---
# <a name="authentication"></a><span data-ttu-id="0629c-103">Autenticación</span><span class="sxs-lookup"><span data-stu-id="0629c-103">Authentication</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0629c-104">En este artículo se describe la autenticación en Direct Line API 1.1.</span><span class="sxs-lookup"><span data-stu-id="0629c-104">This article describes authentication in Direct Line API 1.1.</span></span> <span data-ttu-id="0629c-105">Si va a crear una nueva conexión entre la aplicación cliente y el bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-authentication.md) en su lugar.</span><span class="sxs-lookup"><span data-stu-id="0629c-105">If you are creating a new connection between your client application and bot, use [Direct Line API 3.0](bot-framework-rest-direct-line-3-0-authentication.md) instead.</span></span>

<span data-ttu-id="0629c-106">Un cliente puede autenticar solicitudes a Direct Line API 1.1 mediante un **secreto** que se [obtiene de la página de configuración del canal de Direct Line](../bot-service-channel-connect-directline.md) en el portal de Bot Framework o mediante un **token** que se obtiene en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="0629c-106">A client can authenticate requests to Direct Line API 1.1 either by using a **secret** that you [obtain from the Direct Line channel configuration page](../bot-service-channel-connect-directline.md) in the Bot Framework Portal or by using a **token** that you obtain at runtime.</span></span>

<span data-ttu-id="0629c-107">El secreto o token debe especificarse en el encabezado `Authorization` de cada solicitud, mediante los esquemas "Bearer" o "BotConnector".</span><span class="sxs-lookup"><span data-stu-id="0629c-107">The secret or token should be specified in the `Authorization` header of each request, using either the "Bearer" scheme or the "BotConnector" scheme.</span></span> 

<span data-ttu-id="0629c-108">**Esquema Bearer**:</span><span class="sxs-lookup"><span data-stu-id="0629c-108">**Bearer scheme**:</span></span>
```http
Authorization: Bearer SECRET_OR_TOKEN
```

<span data-ttu-id="0629c-109">**Esquema BotConnector**:</span><span class="sxs-lookup"><span data-stu-id="0629c-109">**BotConnector scheme**:</span></span>
```http
Authorization: BotConnector SECRET_OR_TOKEN
```

## <a name="secrets-and-tokens"></a><span data-ttu-id="0629c-110">Secretos y tokens</span><span class="sxs-lookup"><span data-stu-id="0629c-110">Secrets and tokens</span></span>

<span data-ttu-id="0629c-111">Un **secreto** de Direct Line es una clave maestra que se puede usar para acceder a cualquier conversación que pertenezca al bot asociado.</span><span class="sxs-lookup"><span data-stu-id="0629c-111">A Direct Line **secret** is a master key that can be used to access any conversation that belongs to the associated bot.</span></span> <span data-ttu-id="0629c-112">Un **secreto** se puede usar también para obtener un **token**.</span><span class="sxs-lookup"><span data-stu-id="0629c-112">A **secret** can also be used to obtain a **token**.</span></span> <span data-ttu-id="0629c-113">Los secretos no expiran.</span><span class="sxs-lookup"><span data-stu-id="0629c-113">Secrets do not expire.</span></span> 

<span data-ttu-id="0629c-114">Un **token** de Direct Line es una clave que se puede usar para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="0629c-114">A Direct Line **token** is a key that can be used to access a single conversation.</span></span> <span data-ttu-id="0629c-115">Un token expira, pero se puede actualizar.</span><span class="sxs-lookup"><span data-stu-id="0629c-115">A token expires but can be refreshed.</span></span> 

<span data-ttu-id="0629c-116">Si va a crear una aplicación de servicio a servicio, puede que el enfoque más sencillo sea especificar el **secreto** en el encabezado `Authorization` de las solicitudes de Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="0629c-116">If you're creating a service-to-service application, specifying the **secret** in the `Authorization` header of Direct Line API requests may be simplest approach.</span></span> <span data-ttu-id="0629c-117">Si va a escribir una aplicación en la que el cliente se ejecuta en un explorador web o una aplicación móvil, puede que quiera intercambiar el secreto por un token (que solo sirve para una única conversación y expira a menos que se actualice) y especificar el **token** en el encabezado `Authorization` de las solicitudes de Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="0629c-117">If you're writing an application where the client runs in a web browser or mobile app, you may want to exchange your secret for a token (which only works for a single conversation and will expire unless refreshed) and specify the **token** in the `Authorization` header of Direct Line API requests.</span></span> <span data-ttu-id="0629c-118">Elija el modelo de seguridad que mejor funcione en su caso.</span><span class="sxs-lookup"><span data-stu-id="0629c-118">Choose the security model that works best for you.</span></span>

> [!NOTE]
> <span data-ttu-id="0629c-119">Las credenciales de cliente de Direct Line son diferentes de las credenciales del bot.</span><span class="sxs-lookup"><span data-stu-id="0629c-119">Your Direct Line client credentials are different from your bot's credentials.</span></span> <span data-ttu-id="0629c-120">Esto le permite revisar las claves de forma independiente y compartir los tokens de cliente sin revelar la contraseña del bot.</span><span class="sxs-lookup"><span data-stu-id="0629c-120">This enables you to revise your keys independently and lets you share client tokens without disclosing your bot's password.</span></span> 

## <a name="get-a-direct-line-secret"></a><span data-ttu-id="0629c-121">Obtención de un secreto de Direct Line</span><span class="sxs-lookup"><span data-stu-id="0629c-121">Get a Direct Line secret</span></span>

<span data-ttu-id="0629c-122">También puede [obtener un secreto de Direct Line](../bot-service-channel-connect-directline.md) a través de la página de configuración del canal Direct Line de su bot en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>:</span><span class="sxs-lookup"><span data-stu-id="0629c-122">You can [obtain a Direct Line secret](../bot-service-channel-connect-directline.md) via the Direct Line channel configuration page for your bot in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>:</span></span>

![Configuración de Direct Line](../media/direct-line-configure.png)

## <a id="generate-token"></a> <span data-ttu-id="0629c-124">Generación de un token de Direct Line</span><span class="sxs-lookup"><span data-stu-id="0629c-124">Generate a Direct Line token</span></span>

<span data-ttu-id="0629c-125">Para generar un token de Direct Line que pueda usarse para acceder a una única conversación, primero obtenga el secreto de Direct Line de la página de configuración del canal Direct Line en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>.</span><span class="sxs-lookup"><span data-stu-id="0629c-125">To generate a Direct Line token that can be used to access a single conversation, first obtain the Direct Line secret from the Direct Line channel configuration page in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>.</span></span> <span data-ttu-id="0629c-126">A continuación, emita esta solicitud para intercambiar el secreto de Direct Line por un token de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="0629c-126">Then issue this request to exchange your Direct Line secret for a Direct Line token:</span></span>

```http
POST https://directline.botframework.com/api/tokens/conversation
Authorization: Bearer SECRET
```

<span data-ttu-id="0629c-127">En el encabezado `Authorization` de esta solicitud, reemplace **SECRET** por el valor del secreto de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="0629c-127">In the `Authorization` header of this request, replace **SECRET** with the value of your Direct Line secret.</span></span>

<span data-ttu-id="0629c-128">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Generar un token.</span><span class="sxs-lookup"><span data-stu-id="0629c-128">The following snippets provide an example of the Generate Token request and response.</span></span>

### <a name="request"></a><span data-ttu-id="0629c-129">Solicitud</span><span class="sxs-lookup"><span data-stu-id="0629c-129">Request</span></span>

```http
POST https://directline.botframework.com/api/tokens/conversation
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
```

### <a name="response"></a><span data-ttu-id="0629c-130">Response</span><span class="sxs-lookup"><span data-stu-id="0629c-130">Response</span></span>

<span data-ttu-id="0629c-131">Si la solicitud es correcta, la respuesta contiene un token válido para una conversación.</span><span class="sxs-lookup"><span data-stu-id="0629c-131">If the request is successful, the response contains a token that is valid for one conversation.</span></span> <span data-ttu-id="0629c-132">El token expira en 30 minutos.</span><span class="sxs-lookup"><span data-stu-id="0629c-132">The token will expire in 30 minutes.</span></span> <span data-ttu-id="0629c-133">Para que el token siga siendo útil, debe [actualizarlo](#refresh-token) antes de que expire.</span><span class="sxs-lookup"><span data-stu-id="0629c-133">For the token to remain useful, you must [refresh the token](#refresh-token) before it expires.</span></span>

```http
HTTP/1.1 200 OK
[other headers]

"RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn"
```

### <a name="generate-token-versus-start-conversation"></a><span data-ttu-id="0629c-134">Generar token frente a Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="0629c-134">Generate Token versus Start Conversation</span></span>

<span data-ttu-id="0629c-135">La operación Generar token (`POST /api/tokens/conversation`) es parecida a la operación [Iniciar conversación](bot-framework-rest-direct-line-1-1-start-conversation.md) (`POST /api/conversations`) en el sentido de que ambas operaciones devuelven un `token` que se puede usar para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="0629c-135">The Generate Token operation (`POST /api/tokens/conversation`) is similar to the [Start Conversation](bot-framework-rest-direct-line-1-1-start-conversation.md) operation (`POST /api/conversations`) in that both operations return a `token` that can be used to access a single conversation.</span></span> <span data-ttu-id="0629c-136">Sin embargo, a diferencia de la operación Iniciar conversación, la operación Generar token no inicia la conversación o el contacto con el bot.</span><span class="sxs-lookup"><span data-stu-id="0629c-136">However, unlike the Start Conversation operation, the Generate Token operation does not start the conversation or contact the bot.</span></span> 

<span data-ttu-id="0629c-137">Si va a distribuir el token a los clientes y quiere que inicien la conversación, use la operación Generar token.</span><span class="sxs-lookup"><span data-stu-id="0629c-137">If you plan to distribute the token to clients and want them to initiate the conversation, use the Generate Token operation.</span></span> <span data-ttu-id="0629c-138">Si tiene previsto iniciar inmediatamente la conversación, use en su lugar la operación [Iniciar conversación](bot-framework-rest-direct-line-1-1-start-conversation.md).</span><span class="sxs-lookup"><span data-stu-id="0629c-138">If you intend to start the conversation immediately, use the [Start Conversation](bot-framework-rest-direct-line-1-1-start-conversation.md) operation instead.</span></span>

## <a id="refresh-token"></a> <span data-ttu-id="0629c-139">Actualización de un token de Direct Line</span><span class="sxs-lookup"><span data-stu-id="0629c-139">Refresh a Direct Line token</span></span>

<span data-ttu-id="0629c-140">Un token de Direct Line es válido durante 30 minutos desde el momento en que se genera y se puede actualizar una cantidad ilimitada de veces, siempre y cuando no haya expirado.</span><span class="sxs-lookup"><span data-stu-id="0629c-140">A Direct Line token is valid for 30 minutes from the time it is generated and can be refreshed an unlimited amount of times, as long as it has not expired.</span></span> <span data-ttu-id="0629c-141">No se puede actualizar un token expirado.</span><span class="sxs-lookup"><span data-stu-id="0629c-141">An expired token cannot be refreshed.</span></span> <span data-ttu-id="0629c-142">Para actualizar un token de Direct Line, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="0629c-142">To refresh a Direct Line token, issue this request:</span></span>

```http
POST https://directline.botframework.com/api/tokens/{conversationId}/renew
Authorization: Bearer TOKEN_TO_BE_REFRESHED
```

<span data-ttu-id="0629c-143">En el URI de esta solicitud, reemplace **{conversationId}** por el identificador de la conversación para la que el token es válido, y en el encabezado `Authorization` de esta solicitud, reemplace **TOKEN_TO_BE_REFRESHED** por el token de Direct Line que quiere actualizar.</span><span class="sxs-lookup"><span data-stu-id="0629c-143">In the URI of this request, replace **{conversationId}** with the ID of the conversation for which the token is valid and in the `Authorization` header of this request, replace **TOKEN_TO_BE_REFRESHED** with the Direct Line token that you want to refresh.</span></span>

<span data-ttu-id="0629c-144">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Actualizar token.</span><span class="sxs-lookup"><span data-stu-id="0629c-144">The following snippets provide an example of the Refresh Token request and response.</span></span>

### <a name="request"></a><span data-ttu-id="0629c-145">Solicitud</span><span class="sxs-lookup"><span data-stu-id="0629c-145">Request</span></span>

```http
POST https://directline.botframework.com/api/tokens/abc123/renew
Authorization: Bearer CurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn
```

### <a name="response"></a><span data-ttu-id="0629c-146">Response</span><span class="sxs-lookup"><span data-stu-id="0629c-146">Response</span></span>

<span data-ttu-id="0629c-147">Si la solicitud es correcta, la respuesta contiene un nuevo token que es válido para la misma conversación que el token anterior.</span><span class="sxs-lookup"><span data-stu-id="0629c-147">If the request is successful, the response contains a new token that is valid for the same conversation as the previous token.</span></span> <span data-ttu-id="0629c-148">El token nuevo expirará en 30 minutos.</span><span class="sxs-lookup"><span data-stu-id="0629c-148">The new token will expire in 30 minutes.</span></span> <span data-ttu-id="0629c-149">Para que el nuevo token siga siendo útil, debe [actualizarlo](#refresh-token) antes de que expire.</span><span class="sxs-lookup"><span data-stu-id="0629c-149">For the new token to remain useful, you must [refresh the token](#refresh-token) before it expires.</span></span>

```http
HTTP/1.1 200 OK
[other headers]

"RCurR_XV9ZA.cwA.BKA.y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xniaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0"
```

## <a name="additional-resources"></a><span data-ttu-id="0629c-150">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0629c-150">Additional resources</span></span>

- [<span data-ttu-id="0629c-151">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="0629c-151">Key concepts</span></span>](bot-framework-rest-direct-line-1-1-concepts.md)
- [<span data-ttu-id="0629c-152">Conectar un bot con Direct Line</span><span class="sxs-lookup"><span data-stu-id="0629c-152">Connect a bot to Direct Line</span></span>](../bot-service-channel-connect-directline.md)