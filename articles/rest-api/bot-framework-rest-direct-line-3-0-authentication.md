---
title: Autenticación | Microsoft Docs
description: Aprenda a autenticar solicitudes de API en Direct Line API v3.0.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 18a5815a96e2052a54c48f6af211d8b28e20d983
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305261"
---
# <a name="authentication"></a><span data-ttu-id="414d2-103">Autenticación</span><span class="sxs-lookup"><span data-stu-id="414d2-103">Authentication</span></span>

<span data-ttu-id="414d2-104">Un cliente puede autenticar solicitudes en Direct Line API 3.0 ya sea mediante un **secreto** que se [obtiene de la página de configuración del canal de Direct Line](../bot-service-channel-connect-directline.md) en el portal de Bot Framework, o mediante un **token** que se obtiene en el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="414d2-104">A client can authenticate requests to Direct Line API 3.0 either by using a **secret** that you [obtain from the Direct Line channel configuration page](../bot-service-channel-connect-directline.md) in the Bot Framework Portal or by using a **token** that you obtain at runtime.</span></span> <span data-ttu-id="414d2-105">El secreto o token debe especificarse en el encabezado `Authorization` de cada solicitud, con este formato:</span><span class="sxs-lookup"><span data-stu-id="414d2-105">The secret or token should be specified in the `Authorization` header of each request, using this format:</span></span> 

```http
Authorization: Bearer SECRET_OR_TOKEN
```

## <a name="secrets-and-tokens"></a><span data-ttu-id="414d2-106">Secretos y tokens</span><span class="sxs-lookup"><span data-stu-id="414d2-106">Secrets and tokens</span></span>

<span data-ttu-id="414d2-107">Un **secreto** de Direct Line es una clave maestra que se puede usar para acceder a cualquier conversación que pertenezca al bot asociado.</span><span class="sxs-lookup"><span data-stu-id="414d2-107">A Direct Line **secret** is a master key that can be used to access any conversation that belongs to the associated bot.</span></span> <span data-ttu-id="414d2-108">Un **secreto** se puede usar también para obtener un **token**.</span><span class="sxs-lookup"><span data-stu-id="414d2-108">A **secret** can also be used to obtain a **token**.</span></span> <span data-ttu-id="414d2-109">Los secretos no expiran.</span><span class="sxs-lookup"><span data-stu-id="414d2-109">Secrets do not expire.</span></span> 

<span data-ttu-id="414d2-110">Un **token** de Direct Line es una clave que se puede usar para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="414d2-110">A Direct Line **token** is a key that can be used to access a single conversation.</span></span> <span data-ttu-id="414d2-111">Un token expira, pero se puede actualizar.</span><span class="sxs-lookup"><span data-stu-id="414d2-111">A token expires but can be refreshed.</span></span> 

<span data-ttu-id="414d2-112">Si va a crear una aplicación de servicio a servicio, puede que el enfoque más sencillo sea especificar el **secreto** en el encabezado `Authorization` de las solicitudes de Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="414d2-112">If you're creating a service-to-service application, specifying the **secret** in the `Authorization` header of Direct Line API requests may be simplest approach.</span></span> <span data-ttu-id="414d2-113">Si va a escribir una aplicación en la que el cliente se ejecuta en un explorador web o una aplicación móvil, puede que quiera intercambiar el secreto por un token (que solo sirve para una única conversación y expira a menos que se actualice) y especificar el **token** en el encabezado `Authorization` de las solicitudes de Direct Line API.</span><span class="sxs-lookup"><span data-stu-id="414d2-113">If you're writing an application where the client runs in a web browser or mobile app, you may want to exchange your secret for a token (which only works for a single conversation and will expire unless refreshed) and specify the **token** in the `Authorization` header of Direct Line API requests.</span></span> <span data-ttu-id="414d2-114">Elija el modelo de seguridad que mejor funcione en su caso.</span><span class="sxs-lookup"><span data-stu-id="414d2-114">Choose the security model that works best for you.</span></span>

> [!NOTE]
> <span data-ttu-id="414d2-115">Las credenciales de cliente de Direct Line son diferentes de las credenciales del bot.</span><span class="sxs-lookup"><span data-stu-id="414d2-115">Your Direct Line client credentials are different from your bot's credentials.</span></span> <span data-ttu-id="414d2-116">Esto le permite revisar las claves de forma independiente y compartir los tokens de cliente sin revelar la contraseña del bot.</span><span class="sxs-lookup"><span data-stu-id="414d2-116">This enables you to revise your keys independently and lets you share client tokens without disclosing your bot's password.</span></span> 

## <a name="get-a-direct-line-secret"></a><span data-ttu-id="414d2-117">Obtención de un secreto de Direct Line</span><span class="sxs-lookup"><span data-stu-id="414d2-117">Get a Direct Line secret</span></span>

<span data-ttu-id="414d2-118">También puede [obtener un secreto de Direct Line](../bot-service-channel-connect-directline.md) a través de la página de configuración del canal Direct Line de su bot en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>:</span><span class="sxs-lookup"><span data-stu-id="414d2-118">You can [obtain a Direct Line secret](../bot-service-channel-connect-directline.md) via the Direct Line channel configuration page for your bot in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>:</span></span>

![Configuración de Direct Line](../media/direct-line-configure.png)

## <a id="generate-token"></a> <span data-ttu-id="414d2-120">Generación de un token de Direct Line</span><span class="sxs-lookup"><span data-stu-id="414d2-120">Generate a Direct Line token</span></span>

<span data-ttu-id="414d2-121">Para generar un token de Direct Line que pueda usarse para acceder a una única conversación, primero obtenga el secreto de Direct Line de la página de configuración del canal Direct Line en el <a href="https://dev.botframework.com/" target="_blank">portal de Bot Framework</a>.</span><span class="sxs-lookup"><span data-stu-id="414d2-121">To generate a Direct Line token that can be used to access a single conversation, first obtain the Direct Line secret from the Direct Line channel configuration page in the <a href="https://dev.botframework.com/" target="_blank">Bot Framework Portal</a>.</span></span> <span data-ttu-id="414d2-122">A continuación, emita esta solicitud para intercambiar el secreto de Direct Line por un token de Direct Line:</span><span class="sxs-lookup"><span data-stu-id="414d2-122">Then issue this request to exchange your Direct Line secret for a Direct Line token:</span></span>

```http
POST https://directline.botframework.com/v3/directline/tokens/generate
Authorization: Bearer SECRET
```

<span data-ttu-id="414d2-123">En el encabezado `Authorization` de esta solicitud, reemplace **SECRET** por el valor del secreto de Direct Line.</span><span class="sxs-lookup"><span data-stu-id="414d2-123">In the `Authorization` header of this request, replace **SECRET** with the value of your Direct Line secret.</span></span>

<span data-ttu-id="414d2-124">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Generar un token.</span><span class="sxs-lookup"><span data-stu-id="414d2-124">The following snippets provide an example of the Generate Token request and response.</span></span>

### <a name="request"></a><span data-ttu-id="414d2-125">Solicitud</span><span class="sxs-lookup"><span data-stu-id="414d2-125">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/tokens/generate
Authorization: Bearer RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0
```

### <a name="response"></a><span data-ttu-id="414d2-126">Response</span><span class="sxs-lookup"><span data-stu-id="414d2-126">Response</span></span>

<span data-ttu-id="414d2-127">Si la solicitud es correcta, la respuesta contiene un `token` que es válido para una conversación y un valor `expires_in` que indica el número de segundos hasta que el token expira.</span><span class="sxs-lookup"><span data-stu-id="414d2-127">If the request is successful, the response contains a `token` that is valid for one conversation and an `expires_in` value that indicates the number of seconds until the token expires.</span></span> <span data-ttu-id="414d2-128">Para que el token siga siendo útil, debe [actualizarlo](#refresh-token) antes de que expire.</span><span class="sxs-lookup"><span data-stu-id="414d2-128">For the token to remain useful, you must [refresh the token](#refresh-token) before it expires.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "conversationId": "abc123",
  "token": "RCurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn",
  "expires_in": 1800
}
```

### <a name="generate-token-versus-start-conversation"></a><span data-ttu-id="414d2-129">Generar token frente a Iniciar conversación</span><span class="sxs-lookup"><span data-stu-id="414d2-129">Generate Token versus Start Conversation</span></span>

<span data-ttu-id="414d2-130">La operación Generar token (`POST /v3/directline/tokens/generate`) es parecida a la operación [Iniciar conversación](bot-framework-rest-direct-line-3-0-start-conversation.md) (`POST /v3/directline/conversations`) en el sentido de que ambas operaciones devuelven un `token` que se puede usar para acceder a una única conversación.</span><span class="sxs-lookup"><span data-stu-id="414d2-130">The Generate Token operation (`POST /v3/directline/tokens/generate`) is similar to the [Start Conversation](bot-framework-rest-direct-line-3-0-start-conversation.md) operation (`POST /v3/directline/conversations`) in that both operations return a `token` that can be used to access a single conversation.</span></span> <span data-ttu-id="414d2-131">Sin embargo, a diferencia de la operación Iniciar conversación, la operación Generar token no inicia la conversación o el contacto con el bot, y no crea una dirección URL de WebSocket de streaming.</span><span class="sxs-lookup"><span data-stu-id="414d2-131">However, unlike the Start Conversation operation, the Generate Token operation does not start the conversation, does not contact the bot, and does not create a streaming WebSocket URL.</span></span> 

<span data-ttu-id="414d2-132">Si va a distribuir el token a los clientes y quiere que inicien la conversación, use la operación Generar token.</span><span class="sxs-lookup"><span data-stu-id="414d2-132">If you plan to distribute the token to clients and want them to initiate the conversation, use the Generate Token operation.</span></span> <span data-ttu-id="414d2-133">Si tiene previsto iniciar inmediatamente la conversación, use en su lugar la operación [Iniciar conversación](bot-framework-rest-direct-line-3-0-start-conversation.md).</span><span class="sxs-lookup"><span data-stu-id="414d2-133">If you intend to start the conversation immediately, use the [Start Conversation](bot-framework-rest-direct-line-3-0-start-conversation.md) operation instead.</span></span>

## <a id="refresh-token"></a> <span data-ttu-id="414d2-134">Actualización de un token de Direct Line</span><span class="sxs-lookup"><span data-stu-id="414d2-134">Refresh a Direct Line token</span></span>

<span data-ttu-id="414d2-135">Un token de Direct Line se puede actualizar una cantidad ilimitada de veces, siempre y cuando no haya expirado.</span><span class="sxs-lookup"><span data-stu-id="414d2-135">A Direct Line token can be refreshed an unlimited amount of times, as long as it has not expired.</span></span> <span data-ttu-id="414d2-136">No se puede actualizar un token expirado.</span><span class="sxs-lookup"><span data-stu-id="414d2-136">An expired token cannot be refreshed.</span></span> <span data-ttu-id="414d2-137">Para actualizar un token de Direct Line, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="414d2-137">To refresh a Direct Line token, issue this request:</span></span> 

```http
POST https://directline.botframework.com/v3/directline/tokens/refresh
Authorization: Bearer TOKEN_TO_BE_REFRESHED
```

<span data-ttu-id="414d2-138">En el encabezado `Authorization` de esta solicitud, reemplace **TOKEN_TO_BE_REFRESHED** por el token de Direct Line que quiere actualizar.</span><span class="sxs-lookup"><span data-stu-id="414d2-138">In the `Authorization` header of this request, replace **TOKEN_TO_BE_REFRESHED** with the Direct Line token that you want to refresh.</span></span>

<span data-ttu-id="414d2-139">Los fragmentos de código siguientes proporcionan un ejemplo de la solicitud y respuesta de Actualizar token.</span><span class="sxs-lookup"><span data-stu-id="414d2-139">The following snippets provide an example of the Refresh Token request and response.</span></span>

### <a name="request"></a><span data-ttu-id="414d2-140">Solicitud</span><span class="sxs-lookup"><span data-stu-id="414d2-140">Request</span></span>

```http
POST https://directline.botframework.com/v3/directline/tokens/refresh
Authorization: Bearer CurR_XV9ZA.cwA.BKA.iaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xn
```

### <a name="response"></a><span data-ttu-id="414d2-141">Response</span><span class="sxs-lookup"><span data-stu-id="414d2-141">Response</span></span>

<span data-ttu-id="414d2-142">Si la solicitud tiene éxito, la respuesta contiene un nuevo `token` que es válido para la misma conversación que el token anterior, y un valor `expires_in` que indica el número de segundos hasta que expira el nuevo token.</span><span class="sxs-lookup"><span data-stu-id="414d2-142">If the request is successful, the response contains a new `token` that is valid for the same conversation as the previous token and an `expires_in` value that indicates the number of seconds until the new token expires.</span></span> <span data-ttu-id="414d2-143">Para que el nuevo token siga siendo útil, debe [actualizarlo](#refresh-token) antes de que expire.</span><span class="sxs-lookup"><span data-stu-id="414d2-143">For the new token to remain useful, you must [refresh the token](#refresh-token) before it expires.</span></span>

```http
HTTP/1.1 200 OK
[other headers]
```

```json
{
  "conversationId": "abc123",
  "token": "RCurR_XV9ZA.cwA.BKA.y8qbOF5xPGfiCpg4Fv0y8qqbOF5x8qbOF5xniaJrC8xpy8qbOF5xnR2vtCX7CZj0LdjAPGfiCpg4Fv0",
  "expires_in": 1800
}
```

## <a name="additional-resources"></a><span data-ttu-id="414d2-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="414d2-144">Additional resources</span></span>

- [<span data-ttu-id="414d2-145">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="414d2-145">Key concepts</span></span>](bot-framework-rest-direct-line-3-0-concepts.md)
- [<span data-ttu-id="414d2-146">Conectar un bot con Direct Line</span><span class="sxs-lookup"><span data-stu-id="414d2-146">Connect a bot to Direct Line</span></span>](../bot-service-channel-connect-directline.md)