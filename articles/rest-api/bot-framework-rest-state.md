---
title: Administración de datos de estado | Microsoft Docs
description: Obtenga información sobre cómo almacenar y recuperar datos de estado con el servicio Bot State.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 1557941d4e5413108ea3ce788f7d5d684252b657
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304795"
---
# <a name="manage-state-data"></a><span data-ttu-id="85ad8-103">Administración de datos de estado</span><span class="sxs-lookup"><span data-stu-id="85ad8-103">Manage state data</span></span>

<span data-ttu-id="85ad8-104">El servicio Bot State permite que el bot almacene y recupere datos de estado asociados a un usuario, una conversación o un usuario específico en el contexto de una conversación específica.</span><span class="sxs-lookup"><span data-stu-id="85ad8-104">The Bot State service enables your bot to store and retrieve state data that is associated with a user, a conversation, or a specific user within the context of a specific conversation.</span></span> <span data-ttu-id="85ad8-105">Puede almacenar hasta 32 KB de datos para cada usuario en un canal, cada conversación en un canal y cada usuario en el contexto de una conversación en un canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-105">You may store up to 32 kilobytes of data for each user on a channel, each conversation on a channel, and each user within the context of a conversation on a channel.</span></span> <span data-ttu-id="85ad8-106">Los datos de estado se pueden usar para muchos propósitos, como determinar dónde se dejó una conversación anterior o simplemente saludar por su nombre a un usuario cuando vuelva.</span><span class="sxs-lookup"><span data-stu-id="85ad8-106">State data can be used for many purposes, such as determining where a prior conversation left off or simply greeting a returning user by name.</span></span> <span data-ttu-id="85ad8-107">Si almacena las preferencias del usuario, puede usar esa información para personalizar la conversación la próxima vez que chatee.</span><span class="sxs-lookup"><span data-stu-id="85ad8-107">If you store a user's preferences, you can use that information to customize the conversation the next time you chat.</span></span> <span data-ttu-id="85ad8-108">Por ejemplo, podría alertar al usuario sobre un artículo de noticias que trate un tema que le interese, o bien cuando haya una cita disponible.</span><span class="sxs-lookup"><span data-stu-id="85ad8-108">For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="85ad8-109">No se recomienda la API del servicio de estado de Bot Framework para entornos de producción y es posible que se encuentre en desuso en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="85ad8-109">The Bot Framework State Service API is not recommended for production environments, and may be deprecated in a future release.</span></span> <span data-ttu-id="85ad8-110">No obstante, sí que le recomendamos que actualice el código del bot para que use el almacenamiento en memoria para realizar pruebas o usar una de las **extensiones de Azure** para bots de producción.</span><span class="sxs-lookup"><span data-stu-id="85ad8-110">It is recommended that you update your bot code to use the in-memory storage for testing purposes or use one of the **Azure Extensions** for production bots.</span></span> <span data-ttu-id="85ad8-111">Para obtener más información, consulte el tema **Administración de datos de estado** para la implementación de [.NET](~/dotnet/bot-builder-dotnet-state.md) o [Node](~/nodejs/bot-builder-nodejs-state.md).</span><span class="sxs-lookup"><span data-stu-id="85ad8-111">For more information, see the **Manage state data** topic for [.NET](~/dotnet/bot-builder-dotnet-state.md) or [Node](~/nodejs/bot-builder-nodejs-state.md) implementation.</span></span>

## <a id="concurrency"></a> <span data-ttu-id="85ad8-112">Simultaneidad de datos</span><span class="sxs-lookup"><span data-stu-id="85ad8-112">Data concurrency</span></span>

<span data-ttu-id="85ad8-113">Para controlar la simultaneidad de datos que se almacenan con el servicio de Bot State, el marco usa la etiqueta de entidad (ETag) para las solicitudes `POST`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-113">To control concurrency of data that is stored using the Bot State service, the framework uses the entity tag (ETag) for `POST` requests.</span></span> <span data-ttu-id="85ad8-114">El marco no utiliza los encabezados estándar para las etiquetas ETag.</span><span class="sxs-lookup"><span data-stu-id="85ad8-114">The framework does not use the standard headers for ETags.</span></span> <span data-ttu-id="85ad8-115">En su lugar, el cuerpo de la solicitud y respuesta especifica la ETag con la propiedad `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-115">Instead, the body of the request and response specifies the ETag using the `eTag` property.</span></span> 

<span data-ttu-id="85ad8-116">Por ejemplo, si emite una solicitud `GET` para recuperar datos del usuario del almacén, la respuesta contendrá la propiedad `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-116">For example, if you issue a `GET` request to retrieve user data from the store, the response will contain the `eTag` property.</span></span> <span data-ttu-id="85ad8-117">Si cambia los datos y emite una solicitud `POST` para guardar los datos actualizados en el almacén, la solicitud puede incluir la propiedad `eTag` que especifica el mismo valor que recibió anteriormente en la respuesta de `GET`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-117">If you change the data and issue a `POST` request to save the updated data to the store, your request may include the `eTag` property that specifies the same value as you received earlier in the `GET` response.</span></span> <span data-ttu-id="85ad8-118">Si la ETag especificada en la solicitud `POST` coincide con el valor actual del almacén, el servidor guardará los datos del usuario y responderá con **HTTP 200 Success** (HTTP 200 Correcto) y especificará un nuevo valor de `eTag` en el cuerpo de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="85ad8-118">If the ETag specified in your `POST` request matches the current value in the store, the server will save the user's data and respond with **HTTP 200 Success** and specify a new `eTag` value in the body of the response.</span></span> <span data-ttu-id="85ad8-119">Si la ETag especificada en la solicitud `POST` no coincide con el valor actual del almacén, el servidor responderá con **HTTP 412 Precondition Failed** (HTTP 412 Error en la condición previa) para indicar que los datos del usuario en el almacén han cambiado desde que los guardó o recuperó por última vez.</span><span class="sxs-lookup"><span data-stu-id="85ad8-119">If the ETag specified in your `POST` request does not match the current value in the store, the server will respond with **HTTP 412 Precondition Failed** to indicate that the user's data in the store has changed since you last saved or retrieved it.</span></span>

> [!TIP]
> <span data-ttu-id="85ad8-120">Una respuesta de `GET` siempre incluirá una propiedad `eTag`, pero no es necesario especificar dicha `eTag` propiedad en las solicitudes `GET`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-120">A `GET` response will always include an `eTag` property, but you do not need to specify the `eTag` property in `GET` requests.</span></span> <span data-ttu-id="85ad8-121">Un valor de propiedad `eTag` de asterisco (`*`) indica que no ha guardado previamente los datos para la combinación especificada de canal, usuario y conversación.</span><span class="sxs-lookup"><span data-stu-id="85ad8-121">An `eTag` property value of asterisk (`*`) indicates that you have not previously saved data for the specified combination of channel, user, and conversation.</span></span>
>
> <span data-ttu-id="85ad8-122">La especificación de la propiedad `eTag` en las solicitudes `POST` es opcional.</span><span class="sxs-lookup"><span data-stu-id="85ad8-122">Specifying the `eTag` property in `POST` requests is optional.</span></span> 
> <span data-ttu-id="85ad8-123">Si la simultaneidad no es un problema para el bot, no incluya la propiedad `eTag` en las solicitudes `POST`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-123">If concurrency is not an issue for your bot, do not include the `eTag` property in `POST` requests.</span></span> 

## <a id="save-user-data"></a> <span data-ttu-id="85ad8-124">Almacenamiento de los datos del usuario</span><span class="sxs-lookup"><span data-stu-id="85ad8-124">Save user data</span></span>

<span data-ttu-id="85ad8-125">Para guardar los datos de estado de un usuario en un canal específico, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-125">To save state data for a user on a specific channel, issue this request:</span></span>

```http
POST /v3/botstate/{channelId}/users/{userId}
```

<span data-ttu-id="85ad8-126">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{userId}** por el id. del usuario en dicho canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-126">In this request URI, replace **{channelId}** with the channel’s ID and replace **{userId}** with the user's ID on that channel.</span></span> <span data-ttu-id="85ad8-127">Las propiedades `channelId` y `from` de cualquier mensaje que el bot haya recibido del usuario con anterioridad incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-127">The `channelId` and `from` properties within any message that your bot has previously received from the user will contain these IDs.</span></span> <span data-ttu-id="85ad8-128">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos del usuario en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-128">You may also choose to cache these values in a secure location so that you can access the user's data in the future without having to extract them from a message.</span></span>

<span data-ttu-id="85ad8-129">Establezca el cuerpo de la solicitud en un objeto [BotData][BotData], donde la propiedad `data` especifique los datos que quiere guardar.</span><span class="sxs-lookup"><span data-stu-id="85ad8-129">Set the body of the request to a [BotData][BotData] object, where the `data` property specifies the data that you want to save.</span></span> <span data-ttu-id="85ad8-130">Si usa etiquetas de entidad para el [control de simultaneidad](#concurrency), establezca la propiedad `eTag` en la ETag que recibió en la respuesta la última vez que guardó o recuperó los datos del usuario (lo que sea la más reciente).</span><span class="sxs-lookup"><span data-stu-id="85ad8-130">If you use entity tags for [concurrency control](#concurrency), set the `eTag` property to the ETag that you received in the response the last time you saved or retrieved the user's data (whichever is the most recent).</span></span> <span data-ttu-id="85ad8-131">Si no usa etiquetas de entidad para la simultaneidad, no incluya la propiedad `eTag` en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="85ad8-131">If you do not use entity tags for concurrency, then do not include the `eTag` property in the request.</span></span>

> [!NOTE]
> <span data-ttu-id="85ad8-132">Esta solicitud `POST` sobrescribirá los datos del usuario en el almacén solo si la ETag especificada coincide con la del servidor, o bien si no se especificó ninguna ETag en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="85ad8-132">This `POST` request will overwrite user data in the store only if the specified ETag matches the server's ETag, or if no ETag is specified in the request.</span></span>

### <a name="request"></a><span data-ttu-id="85ad8-133">Solicitud</span><span class="sxs-lookup"><span data-stu-id="85ad8-133">Request</span></span> 

<span data-ttu-id="85ad8-134">En el ejemplo siguiente se muestra una solicitud que guarda los datos de un usuario en un canal específico.</span><span class="sxs-lookup"><span data-stu-id="85ad8-134">The following example shows a request that saves data for a user on a specific channel.</span></span> <span data-ttu-id="85ad8-135">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="85ad8-135">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="85ad8-136">Para obtener más información sobre cómo establecer el URI base, consulte [API Reference](bot-framework-rest-connector-api-reference.md#base-uri) (Referencia de la API).</span><span class="sxs-lookup"><span data-stu-id="85ad8-136">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
POST https://smba.trafficmanager.net/apis/v3/botstate/abcd1234/users/12345678
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

```json
{
    "data": [
        {
            "trail": "Lake Serene",
            "miles": 8.2,
            "difficulty": "Difficult",
        },
        {
            "trail": "Rainbow Falls",
            "miles": 6.3,
            "difficulty": "Moderate",
        }
    ],
    "eTag": "a1b2c3d4"
}
```

### <a name="response"></a><span data-ttu-id="85ad8-137">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-137">Response</span></span> 

<span data-ttu-id="85ad8-138">La respuesta contendrá un objeto [BotData][BotData] con un nuevo valor `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-138">The response will contain a [BotData][BotData] object with a new `eTag` value.</span></span>

## <a name="get-user-data"></a><span data-ttu-id="85ad8-139">Obtención de los datos del usuario</span><span class="sxs-lookup"><span data-stu-id="85ad8-139">Get user data</span></span>

<span data-ttu-id="85ad8-140">Para obtener datos de estado guardados previamente del usuario en un canal específico, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-140">To get state data that has previously been saved for the user on a specific channel, issue this request:</span></span>

```http
GET /v3/botstate/{channelId}/users/{userId}
```

<span data-ttu-id="85ad8-141">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{userId}** por el id. del usuario en dicho canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-141">In this request URI, replace **{channelId}** with the channel’s ID and replace **{userId}** with the user's ID on that channel.</span></span> <span data-ttu-id="85ad8-142">Las propiedades `channelId` y `from` de cualquier mensaje que el bot haya recibido del usuario con anterioridad incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-142">The `channelId` and `from` properties within any message that your bot has previously received from the user will contain these IDs.</span></span> <span data-ttu-id="85ad8-143">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos del usuario en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-143">You may also choose to cache these values in a secure location so that you can access the user's data in the future without having to extract them from a message.</span></span>

### <a name="request"></a><span data-ttu-id="85ad8-144">Solicitud</span><span class="sxs-lookup"><span data-stu-id="85ad8-144">Request</span></span>

<span data-ttu-id="85ad8-145">En el ejemplo siguiente se muestra una solicitud que obtiene los datos que se han guardado previamente del usuario.</span><span class="sxs-lookup"><span data-stu-id="85ad8-145">The following example shows a request that gets data that has previously been saved for the user.</span></span> <span data-ttu-id="85ad8-146">En esta solicitud de ejemplo, `https://smba.trafficmanager.net/apis` representa el URI base; los URI base para las solicitudes que emite el bot pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="85ad8-146">In this example request, `https://smba.trafficmanager.net/apis` represents the base URI; the base URI for requests that your bot issues may be different.</span></span> <span data-ttu-id="85ad8-147">Para obtener más información sobre cómo establecer el URI base, consulte [API Reference](bot-framework-rest-connector-api-reference.md#base-uri) (Referencia de la API).</span><span class="sxs-lookup"><span data-stu-id="85ad8-147">For details about setting the base URI, see [API Reference](bot-framework-rest-connector-api-reference.md#base-uri).</span></span>

```http
GET https://smba.trafficmanager.net/apis/v3/botstate/abcd1234/users/12345678
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

### <a name="response"></a><span data-ttu-id="85ad8-148">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-148">Response</span></span>

<span data-ttu-id="85ad8-149">La respuesta contendrá un objeto [BotData][BotData], donde la propiedad `data` contiene los datos que ha guardado previamente para el usuario y la propiedad `eTag` contiene la ETag que puede usar en una solicitud posterior para guardar los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="85ad8-149">The response will contain a [BotData][BotData] object, where the `data` property contains the data that you have previously saved for the user and the `eTag` property contains the ETag that you may use within a subsequent request to save the user's data.</span></span> <span data-ttu-id="85ad8-150">Si no ha guardado previamente los datos del usuario, la propiedad `data` será `null` y la propiedad `eTag` contendrá un asterisco (`*`).</span><span class="sxs-lookup"><span data-stu-id="85ad8-150">If you have not previously saved data for the user, the `data` property will be `null` and the `eTag` property will contain an asterisk (`*`).</span></span>

<span data-ttu-id="85ad8-151">En este ejemplo se muestra la respuesta a la solicitud `GET`:</span><span class="sxs-lookup"><span data-stu-id="85ad8-151">This example shows the response to the `GET` request:</span></span>

```json
{
    "data": [
        {
            "trail": "Lake Serene",
            "miles": 8.2,
            "difficulty": "Difficult",
        },
        {
            "trail": "Rainbow Falls",
            "miles": 6.3,
            "difficulty": "Moderate",
        }
    ],
    "eTag": "xyz12345"
}
```

## <a name="save-conversation-data"></a><span data-ttu-id="85ad8-152">Almacenamiento de los datos de una conversación</span><span class="sxs-lookup"><span data-stu-id="85ad8-152">Save conversation data</span></span>

<span data-ttu-id="85ad8-153">Para guardar los datos de estado de una conversación en un canal específico, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-153">To save state data for a conversation on a specific channel, issue this request:</span></span>

```http
POST /v3/botstate/{channelId}/conversations/{conversationId}
```

<span data-ttu-id="85ad8-154">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{conversationId}** por el id. de la conversación.</span><span class="sxs-lookup"><span data-stu-id="85ad8-154">In this request URI, replace **{channelId}** with the channel’s ID, and replace **{conversationId}** with ID of the conversation.</span></span> <span data-ttu-id="85ad8-155">Las propiedades `channelId` y `conversation` de cualquier mensaje que el bot haya enviado o recibido con anterioridad en el contexto de la conversación incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-155">The `channelId` and `conversation` properties within any message that your bot has previously sent to or received in the context of the conversation will contain these IDs.</span></span> <span data-ttu-id="85ad8-156">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos de la conversación en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-156">You may also choose to cache these values in a secure location so that you can access the converation's data in the future without having to extract them from a message.</span></span>

<span data-ttu-id="85ad8-157">Establezca el cuerpo de la solicitud en un objeto [BotData][BotData], según se describe en [Almacenamiento de los datos del usuario](#save-user-data).</span><span class="sxs-lookup"><span data-stu-id="85ad8-157">Set the request body to a [BotData][BotData] object, as described in [Save user data](#save-user-data).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85ad8-158">Dado que la operación [Eliminación de los datos del usuario](#delete-user-data) no elimina los datos que se han almacenado mediante la operación **Almacenamiento de los datos de una conversación**, NO debe utilizar esta operación para almacenar información de identificación personal de un usuario.</span><span class="sxs-lookup"><span data-stu-id="85ad8-158">Because the [Delete user data](#delete-user-data) operation does not delete data that has been stored using the **Save conversation data** operation, you must NOT use this operation to store a user's personally identifiable information (PII).</span></span>

### <a name="response"></a><span data-ttu-id="85ad8-159">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-159">Response</span></span>

<span data-ttu-id="85ad8-160">La respuesta contendrá un objeto [BotData][BotData] con un nuevo valor `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-160">The response will contain a [BotData][BotData] object with a new `eTag` value.</span></span>

## <a name="get-conversation-data"></a><span data-ttu-id="85ad8-161">Obtención de los datos de una conversación</span><span class="sxs-lookup"><span data-stu-id="85ad8-161">Get conversation data</span></span>

<span data-ttu-id="85ad8-162">Para obtener datos de estado guardados previamente de una conversación en un canal específico, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-162">To get state data that has previously been saved for a conversation on a specific channel, issue this request:</span></span>

```http
GET /v3/botstate/{channelId}/conversations/{conversationId}
```

<span data-ttu-id="85ad8-163">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{conversationId}** por el id. de la conversación.</span><span class="sxs-lookup"><span data-stu-id="85ad8-163">In this request URI, replace **{channelId}** with the channel’s ID and replace **{conversationId}** with ID of the conversation.</span></span> <span data-ttu-id="85ad8-164">Las propiedades `channelId` y `conversation` de cualquier mensaje que el bot haya enviado o recibido con anterioridad en el contexto de la conversación incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-164">The `channelId` and `conversation` properties within any message that your bot has previously sent or received in the context of the conversation will contain these IDs.</span></span> <span data-ttu-id="85ad8-165">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos de la conversación en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-165">You may also choose to cache these values in a secure location so that you can access the converation's data in the future without having to extract them from a message.</span></span>

### <a name="response"></a><span data-ttu-id="85ad8-166">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-166">Response</span></span>

<span data-ttu-id="85ad8-167">La respuesta contendrá un objeto [BotData][BotData] con un nuevo valor `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-167">The response will contain a [BotData][BotData] object with a new `eTag` value.</span></span>

## <a name="save-private-conversation-data"></a><span data-ttu-id="85ad8-168">Almacenamiento de los datos de una conversación privada</span><span class="sxs-lookup"><span data-stu-id="85ad8-168">Save private conversation data</span></span>

<span data-ttu-id="85ad8-169">Para guardar los datos de estado de un usuario en el contexto de una conversación específica, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-169">To save state data for a user within the context of a specific conversation, issue this request:</span></span>

```http
POST /v3/botstate/{channelId}/conversations/{conversationId}/users/{userId}
```

<span data-ttu-id="85ad8-170">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{conversationId}** por el id. de la conversación, así como **{userId}** por el id. del usuario en ese canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-170">In this request URI, replace **{channelId}** with the channel’s ID, replace **{conversationId}** with ID of the conversation, and replace **{userId}** with the user's ID on that channel.</span></span> <span data-ttu-id="85ad8-171">Las propiedades `channelId`, `conversation` y `from` de cualquier mensaje que el bot haya recibido del usuario con anterioridad en el contexto de la conversación incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-171">The `channelId`, `conversation`, and `from` properties within any message that your bot has previously received from the user in the context of the conversation will contain these IDs.</span></span> <span data-ttu-id="85ad8-172">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos de la conversación en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-172">You may also choose to cache these values in a secure location so that you can access the converation's data in the future without having to extract them from a message.</span></span>

<span data-ttu-id="85ad8-173">Establezca el cuerpo de la solicitud en un objeto [BotData][BotData], según se describe en [Almacenamiento de los datos del usuario](#save-user-data).</span><span class="sxs-lookup"><span data-stu-id="85ad8-173">Set the request body to a [BotData][BotData] object, as described in [Save user data](#save-user-data).</span></span>

### <a name="response"></a><span data-ttu-id="85ad8-174">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-174">Response</span></span>

<span data-ttu-id="85ad8-175">La respuesta contendrá un objeto [BotData][BotData] con un nuevo valor `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-175">The response will contain a [BotData][BotData] object with a new `eTag` value.</span></span>

## <a name="get-private-conversation-data"></a><span data-ttu-id="85ad8-176">Obtención de los datos de una conversación privada</span><span class="sxs-lookup"><span data-stu-id="85ad8-176">Get private conversation data</span></span>

<span data-ttu-id="85ad8-177">Para obtener datos de estado guardados previamente de un usuario dentro del contexto de una conversación específica, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-177">To get state data that has previously been saved for a user within the context of a specific conversation, issue this request:</span></span> 

```http
GET /v3/botstate/{channelId}/conversations/{conversationId}/users/{userId}
```

<span data-ttu-id="85ad8-178">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{conversationId}** por el id. de la conversación, así como **{userId}** por el id. del usuario en ese canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-178">In this request URI, replace **{channelId}** with the channel’s ID, replace **{conversationId}** with ID of the conversation, and replace **{userId}** with the user's ID on that channel.</span></span> <span data-ttu-id="85ad8-179">Las propiedades `channelId`, `conversation` y `from` de cualquier mensaje que el bot haya recibido del usuario con anterioridad en el contexto de la conversación incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-179">The `channelId`, `conversation`, and `from` properties within any message that your bot has previously received from the user in the context of the conversation will contain these IDs.</span></span> <span data-ttu-id="85ad8-180">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos de la conversación en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-180">You may also choose to cache these values in a secure location so that you can access the converation's data in the future without having to extract them from a message.</span></span>

### <a name="response"></a><span data-ttu-id="85ad8-181">Response</span><span class="sxs-lookup"><span data-stu-id="85ad8-181">Response</span></span>

<span data-ttu-id="85ad8-182">La respuesta contendrá un objeto [BotData][BotData] con un nuevo valor `eTag`.</span><span class="sxs-lookup"><span data-stu-id="85ad8-182">The response will contain a [BotData][BotData] object with a new `eTag` value.</span></span>

## <a name="delete-user-data"></a><span data-ttu-id="85ad8-183">Eliminación de los datos de usuario</span><span class="sxs-lookup"><span data-stu-id="85ad8-183">Delete user data</span></span>

<span data-ttu-id="85ad8-184">Para eliminar los datos de estado de un usuario en un canal específico, emita esta solicitud:</span><span class="sxs-lookup"><span data-stu-id="85ad8-184">To delete state data for a user on a specific channel, issue this request:</span></span>

```http
DELETE /v3/botstate/{channelId}/users/{userId}
```
<span data-ttu-id="85ad8-185">En este URI de solicitud, reemplace **{channelId}** por el id. del canal y **{userId}** por el id. del usuario en dicho canal.</span><span class="sxs-lookup"><span data-stu-id="85ad8-185">In this request URI, replace **{channelId}** with the channel’s ID and replace **{userId}** with the user's ID on that channel.</span></span> <span data-ttu-id="85ad8-186">Las propiedades `channelId` y `from` de cualquier mensaje que el bot haya recibido del usuario con anterioridad incluirán estos identificadores.</span><span class="sxs-lookup"><span data-stu-id="85ad8-186">The `channelId` and `from` properties within any message that your bot has previously received from the user will contain these IDs.</span></span> <span data-ttu-id="85ad8-187">También puede almacenar en caché estos valores en una ubicación segura para que pueda acceder a los datos del usuario en el futuro sin tener que extraerlos de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="85ad8-187">You may also choose to cache these values in a secure location so that you can access the user's data in the future without having to extract them from a message.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85ad8-188">Esta operación elimina los datos almacenados anteriormente del usuario mediante la operación [Almacenamiento de los datos del usuario](#save-user-data) o [Almacenamiento de los datos de una conversación privada](#save-private-conversation-data).</span><span class="sxs-lookup"><span data-stu-id="85ad8-188">This operation deletes data that has previously been stored for the user by using either the [Save user data](#save-user-data) operation or the [Save private conversation data](#save-private-conversation-data) operation.</span></span> <span data-ttu-id="85ad8-189">NO elimina los datos que se han almacenado previamente mediante el uso de la operación [Almacenamiento de los datos de una conversación](#save-conversation-data).</span><span class="sxs-lookup"><span data-stu-id="85ad8-189">It does NOT delete data that has previously been stored by using the [Save conversation data](#save-conversation-data) operation.</span></span> <span data-ttu-id="85ad8-190">Por lo tanto, NO debe utilizar la operación **Almacenamiento de los datos de una conversación** para almacenar información de identificación personal de un usuario.</span><span class="sxs-lookup"><span data-stu-id="85ad8-190">Therefore, you must NOT use the **Save conversation data** operation to store a user's personally identifiable information (PII).</span></span>

<span data-ttu-id="85ad8-191">El bot debe ejecutar la operación **Eliminación de los datos del usuario** cuando recibe una [actividad][Activity] de tipo [deleteUserData](bot-framework-rest-connector-activities.md#deleteuserdata) o una de tipo [contactRelationUpdate](bot-framework-rest-connector-activities.md#contactrelationupdate) que indica que el bot se ha quitado de la lista de contactos del usuario.</span><span class="sxs-lookup"><span data-stu-id="85ad8-191">Your bot should execute the **Delete user data** operation when it receives an [Activity][Activity] of type [deleteUserData](bot-framework-rest-connector-activities.md#deleteuserdata) or an activity of type [contactRelationUpdate](bot-framework-rest-connector-activities.md#contactrelationupdate) that indicates the bot has been removed from the user's contact list.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="85ad8-192">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="85ad8-192">Additional resources</span></span>

- [<span data-ttu-id="85ad8-193">Conceptos clave</span><span class="sxs-lookup"><span data-stu-id="85ad8-193">Key concepts</span></span>](bot-framework-rest-connector-concepts.md)
- <span data-ttu-id="85ad8-194">[Activities overview](bot-framework-rest-connector-activities.md) (Introducción a las actividades)</span><span class="sxs-lookup"><span data-stu-id="85ad8-194">[Activities overview](bot-framework-rest-connector-activities.md)</span></span>

[BotData]: bot-framework-rest-connector-api-reference.md#botdata-object
[Activity]: bot-framework-rest-connector-api-reference.md#activity-object
