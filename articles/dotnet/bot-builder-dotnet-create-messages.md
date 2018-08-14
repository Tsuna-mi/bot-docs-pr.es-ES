---
title: Crear mensajes con Bot Builder SDK para .NET | Microsoft Docs
description: Obtenga información sobre las propiedades de mensaje usadas habitualmente en Bot Builder SDK para. NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: a97d21e64ce12f6158f276889762a73ac2aff49b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306353"
---
# <a name="create-messages"></a><span data-ttu-id="08b16-103">Creación de mensajes</span><span class="sxs-lookup"><span data-stu-id="08b16-103">Create messages</span></span>

<span data-ttu-id="08b16-104">El bot enviará [actividades](bot-builder-dotnet-activities.md) de **mensajes** para comunicar información a los usuarios y, del mismo modo, recibirá actividades de **mensajes** de los usuarios.</span><span class="sxs-lookup"><span data-stu-id="08b16-104">Your bot will send **message** [activities](bot-builder-dotnet-activities.md) to communicate information to users, and likewise, will also receive **message** activities from users.</span></span> <span data-ttu-id="08b16-105">Algunos mensajes pueden constar simplemente de texto sin formato, mientras que otros pueden incluir contenido más enriquecido, como [texto que se va a decir](bot-builder-dotnet-text-to-speech.md), [acciones sugeridas](bot-builder-dotnet-add-suggested-actions.md), [datos adjuntos multimedia](bot-builder-dotnet-add-media-attachments.md), [tarjetas enriquecidas](bot-builder-dotnet-add-rich-card-attachments.md) y [datos específicos del canal](bot-builder-dotnet-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="08b16-105">Some messages may simply consist of plain text, while others may contain richer content such as [text to be spoken](bot-builder-dotnet-text-to-speech.md), [suggested actions](bot-builder-dotnet-add-suggested-actions.md), [media attachments](bot-builder-dotnet-add-media-attachments.md), [rich cards](bot-builder-dotnet-add-rich-card-attachments.md), and [channel-specific data](bot-builder-dotnet-channeldata.md).</span></span> 

<span data-ttu-id="08b16-106">En este artículo se describen algunas propiedades de mensaje usadas habitualmente.</span><span class="sxs-lookup"><span data-stu-id="08b16-106">This article describes some of the commonly-used message properties.</span></span>

## <a name="customizing-a-message"></a><span data-ttu-id="08b16-107">Personalización de un mensaje</span><span class="sxs-lookup"><span data-stu-id="08b16-107">Customizing a message</span></span>

<span data-ttu-id="08b16-108">Para tener más control sobre el formato del texto de los mensajes, puede crear un mensaje personalizado mediante el objeto [Activity](https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html) y establecer las propiedades necesarias antes de enviarlo al usuario.</span><span class="sxs-lookup"><span data-stu-id="08b16-108">To have more control over the text formatting of your messages, you can create a custom message using the [Activity](https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html) object and set the properties necessary before sending it to the user.</span></span>

<span data-ttu-id="08b16-109">En este ejemplo se muestra cómo crear un objeto `message` personalizado y establecer las propiedades `Text`, `TextFormat` y `Local`.</span><span class="sxs-lookup"><span data-stu-id="08b16-109">This sample shows how to create a custom `message` object and set the `Text`, `TextFormat`, and `Local` properties.</span></span>

[!code-csharp[Set message properties](../includes/code/dotnet-create-messages.cs#setBasicProperties)]

<span data-ttu-id="08b16-110">La propiedad `TextFormat` de un mensaje puede usarse para especificar el formato del texto.</span><span class="sxs-lookup"><span data-stu-id="08b16-110">The `TextFormat` property of a message can be used to specify the format of the text.</span></span> <span data-ttu-id="08b16-111">La propiedad `TextFormat` puede establecerse en **plain**, **markdown** o **xml**.</span><span class="sxs-lookup"><span data-stu-id="08b16-111">The `TextFormat` property can be set to **plain**, **markdown**, or **xml**.</span></span> <span data-ttu-id="08b16-112">El valor predeterminado de `TextFormat` es **markdown**.</span><span class="sxs-lookup"><span data-stu-id="08b16-112">The default value for `TextFormat` is **markdown**.</span></span> 

<span data-ttu-id="08b16-113">Para obtener una lista del formato de texto normalmente compatible, vea [Text formatting](../bot-service-channel-inspector.md#text-formatting) (Formato de texto).</span><span class="sxs-lookup"><span data-stu-id="08b16-113">For a list of commonly supported text formatting, see [Text formatting](../bot-service-channel-inspector.md#text-formatting).</span></span> <span data-ttu-id="08b16-114">Para asegurarse de que las características que quiere usar son compatibles con el canal de destino, obtenga una vista previa de las características mediante el [Inspector de canales](../bot-service-channel-inspector.md).</span><span class="sxs-lookup"><span data-stu-id="08b16-114">To ensure that the feature(s) you want to use is supported by the target channel, preview the feature(s) using the [Channel Inspector](../bot-service-channel-inspector.md).</span></span>

## <a name="attachments"></a><span data-ttu-id="08b16-115">Datos adjuntos</span><span class="sxs-lookup"><span data-stu-id="08b16-115">Attachments</span></span>

<span data-ttu-id="08b16-116">La propiedad `Attachments` de una actividad de mensajes puede usarse para enviar y recibir datos adjuntos multimedia sencillos (imagen, audio, vídeo, archivo) y tarjetas enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="08b16-116">The `Attachments` property of a message activity can be used to send and receive simple media attachments (image, audio, video, file) and rich cards.</span></span> <span data-ttu-id="08b16-117">Para más información, vea [Add media attachments to messages](bot-builder-dotnet-add-media-attachments.md) (Agregar datos adjuntos multimedia a mensajes) y [Add rich cards to messages](bot-builder-dotnet-add-rich-card-attachments.md) (Agregar tarjetas enriquecidas a mensajes).</span><span class="sxs-lookup"><span data-stu-id="08b16-117">For details, see [Add media attachments to messages](bot-builder-dotnet-add-media-attachments.md) and [Add rich cards to messages](bot-builder-dotnet-add-rich-card-attachments.md).</span></span>

## <a name="entities"></a><span data-ttu-id="08b16-118">Entidades</span><span class="sxs-lookup"><span data-stu-id="08b16-118">Entities</span></span>

<span data-ttu-id="08b16-119">La propiedad `Entities` de un mensaje es una matriz de objetos <a href="http://schema.org/" target="_blank">schema.org</a> de extremo abierto que permite el intercambio de metadatos contextuales comunes entre el canal y el bot.</span><span class="sxs-lookup"><span data-stu-id="08b16-119">The `Entities` property of a message is an array of open-ended <a href="http://schema.org/" target="_blank">schema.org</a> objects which allows the exchange of common contextual metadata between the channel and bot.</span></span>

### <a name="mention-entities"></a><span data-ttu-id="08b16-120">Entidades de mención</span><span class="sxs-lookup"><span data-stu-id="08b16-120">Mention entities</span></span>

<span data-ttu-id="08b16-121">Muchos canales ofrecen la posibilidad de que un bot o usuario "mencione" a alguien dentro del contexto de una conversación.</span><span class="sxs-lookup"><span data-stu-id="08b16-121">Many channels support the ability for a bot or user to "mention" someone within the context of a conversation.</span></span> <span data-ttu-id="08b16-122">Para mencionar a un usuario en un mensaje, rellene la propiedad `Entities` del mensaje con un objeto `Mention`.</span><span class="sxs-lookup"><span data-stu-id="08b16-122">To mention a user in a message, populate the message's `Entities` property with a `Mention` object.</span></span> <span data-ttu-id="08b16-123">El objeto `Mention` contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="08b16-123">The `Mention` object contains these properties:</span></span> 

| <span data-ttu-id="08b16-124">Propiedad</span><span class="sxs-lookup"><span data-stu-id="08b16-124">Property</span></span> | <span data-ttu-id="08b16-125">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="08b16-125">Description</span></span> | 
|----|----|
| <span data-ttu-id="08b16-126">Escriba</span><span class="sxs-lookup"><span data-stu-id="08b16-126">Type</span></span> | <span data-ttu-id="08b16-127">Tipo de entidad ("mention")</span><span class="sxs-lookup"><span data-stu-id="08b16-127">type of the entity ("mention")</span></span> | 
| <span data-ttu-id="08b16-128">Mentioned</span><span class="sxs-lookup"><span data-stu-id="08b16-128">Mentioned</span></span> | <span data-ttu-id="08b16-129">Objeto `ChannelAccount` que indica a qué usuario se ha mencionado</span><span class="sxs-lookup"><span data-stu-id="08b16-129">`ChannelAccount` object that indicates which user was mentioned</span></span> | 
| <span data-ttu-id="08b16-130">Texto</span><span class="sxs-lookup"><span data-stu-id="08b16-130">Text</span></span> | <span data-ttu-id="08b16-131">Texto de la propiedad `Activity.Text` que representa la mención en sí misma (puede estar vacío o ser nulo)</span><span class="sxs-lookup"><span data-stu-id="08b16-131">text within the `Activity.Text` property that represents the mention itself (may be empty or null)</span></span> |

<span data-ttu-id="08b16-132">En este ejemplo de código se muestra cómo se agrega una entidad `Mention` a la colección `Entities`.</span><span class="sxs-lookup"><span data-stu-id="08b16-132">This code example shows how to add a `Mention` entity to the `Entities` collection.</span></span>

[!code-csharp[set Mention](../includes/code/dotnet-create-messages.cs#setMention)]

> [!TIP]
> <span data-ttu-id="08b16-133">Al intentar determinar la intención del usuario, puede que el bot omita la parte del mensaje en la que se menciona.</span><span class="sxs-lookup"><span data-stu-id="08b16-133">When attempting to determine user intent, the  bot may want to ignore that portion of the message where it is mentioned.</span></span> <span data-ttu-id="08b16-134">Llame al método `GetMentions` y evalúe los objetos `Mention` devueltos en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="08b16-134">Call the `GetMentions` method and evaluate the `Mention` objects returned in the response.</span></span>

### <a name="place-objects"></a><span data-ttu-id="08b16-135">Objetos Place</span><span class="sxs-lookup"><span data-stu-id="08b16-135">Place objects</span></span>

<span data-ttu-id="08b16-136">La <a href="https://schema.org/Place" target="_blank">información relacionada con la ubicación</a> se puede transmitir dentro de un mensaje si se rellena la propiedad `Entities` del mensaje con un objeto `Place` o `GeoCoordinates`.</span><span class="sxs-lookup"><span data-stu-id="08b16-136"><a href="https://schema.org/Place" target="_blank">Location-related information</a> can be conveyed within a message by populating the message's `Entities` property with either a `Place` object or a `GeoCoordinates` object.</span></span> 

<span data-ttu-id="08b16-137">El objeto `Place` contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="08b16-137">The `Place` object contains these properties:</span></span>

| <span data-ttu-id="08b16-138">Propiedad</span><span class="sxs-lookup"><span data-stu-id="08b16-138">Property</span></span> | <span data-ttu-id="08b16-139">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="08b16-139">Description</span></span> | 
|----|----|
| <span data-ttu-id="08b16-140">Escriba</span><span class="sxs-lookup"><span data-stu-id="08b16-140">Type</span></span> | <span data-ttu-id="08b16-141">Tipo de entidad ("Place")</span><span class="sxs-lookup"><span data-stu-id="08b16-141">type of the entity ("Place")</span></span> |
| <span data-ttu-id="08b16-142">Dirección</span><span class="sxs-lookup"><span data-stu-id="08b16-142">Address</span></span> | <span data-ttu-id="08b16-143">Descripción u objeto `PostalAddress` (en un futuro)</span><span class="sxs-lookup"><span data-stu-id="08b16-143">description or `PostalAddress` object (future)</span></span> | 
| <span data-ttu-id="08b16-144">Geoárea</span><span class="sxs-lookup"><span data-stu-id="08b16-144">Geo</span></span> | <span data-ttu-id="08b16-145">Coordenadas geográficas</span><span class="sxs-lookup"><span data-stu-id="08b16-145">GeoCoordinates</span></span> | 
| <span data-ttu-id="08b16-146">HasMap</span><span class="sxs-lookup"><span data-stu-id="08b16-146">HasMap</span></span> | <span data-ttu-id="08b16-147">Dirección URL de un mapa u objeto `Map` (en un futuro)</span><span class="sxs-lookup"><span data-stu-id="08b16-147">URL to a map or `Map` object (future)</span></span> |
| <span data-ttu-id="08b16-148">NOMBRE</span><span class="sxs-lookup"><span data-stu-id="08b16-148">Name</span></span> | <span data-ttu-id="08b16-149">Nombre del lugar</span><span class="sxs-lookup"><span data-stu-id="08b16-149">name of the place</span></span> |

<span data-ttu-id="08b16-150">El objeto `GeoCoordinates` contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="08b16-150">The `GeoCoordinates` object contains these properties:</span></span>

| <span data-ttu-id="08b16-151">Propiedad</span><span class="sxs-lookup"><span data-stu-id="08b16-151">Property</span></span> | <span data-ttu-id="08b16-152">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="08b16-152">Description</span></span> | 
|----|----|
| <span data-ttu-id="08b16-153">Escriba</span><span class="sxs-lookup"><span data-stu-id="08b16-153">Type</span></span> | <span data-ttu-id="08b16-154">Tipo de entidad ("GeoCoordinates")</span><span class="sxs-lookup"><span data-stu-id="08b16-154">type of the entity ("GeoCoordinates")</span></span> |
| <span data-ttu-id="08b16-155">NOMBRE</span><span class="sxs-lookup"><span data-stu-id="08b16-155">Name</span></span> | <span data-ttu-id="08b16-156">Nombre del lugar</span><span class="sxs-lookup"><span data-stu-id="08b16-156">name of the place</span></span> |
| <span data-ttu-id="08b16-157">Longitud</span><span class="sxs-lookup"><span data-stu-id="08b16-157">Longitude</span></span> | <span data-ttu-id="08b16-158">Longitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="08b16-158">longitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> | 
| <span data-ttu-id="08b16-159">Latitud</span><span class="sxs-lookup"><span data-stu-id="08b16-159">Latitude</span></span> | <span data-ttu-id="08b16-160">Latitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="08b16-160">latitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> | 
| <span data-ttu-id="08b16-161">Elevation</span><span class="sxs-lookup"><span data-stu-id="08b16-161">Elevation</span></span> | <span data-ttu-id="08b16-162">Altitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="08b16-162">elevation of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> | 

<span data-ttu-id="08b16-163">En este ejemplo de código se muestra cómo se agrega una entidad `Place` a la colección `Entities`:</span><span class="sxs-lookup"><span data-stu-id="08b16-163">This code example shows how to add a `Place` entity to the `Entities` collection:</span></span>

[!code-csharp[set GeoCoordinates](../includes/code/dotnet-create-messages.cs#setGeoCoord)]

### <a name="consume-entities"></a><span data-ttu-id="08b16-164">Consumir entidades</span><span class="sxs-lookup"><span data-stu-id="08b16-164">Consume entities</span></span>

<span data-ttu-id="08b16-165">Para consumir entidades, use la palabra clave `dynamic` o clases fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="08b16-165">To consume entities, use either the `dynamic` keyword or strongly-typed classes.</span></span>

<span data-ttu-id="08b16-166">En este ejemplo de código se muestra cómo usar la palabra clave `dynamic` para procesar una entidad en la propiedad `Entities` de un mensaje:</span><span class="sxs-lookup"><span data-stu-id="08b16-166">This code example shows how to use the `dynamic` keyword to process an entity within the `Entities` property of a message:</span></span>

[!code-csharp[examine entity using dynamic keyword](../includes/code/dotnet-create-messages.cs#examineEntity1)]

<span data-ttu-id="08b16-167">En este ejemplo de código se muestra cómo usar una clase fuertemente tipada para procesar una entidad en la propiedad `Entities` de un mensaje:</span><span class="sxs-lookup"><span data-stu-id="08b16-167">This code example shows how to use a strongly-typed class to process an entity within the `Entities` property of a message:</span></span>

[!code-csharp[examine entity using typed class](../includes/code/dotnet-create-messages.cs#examineEntity2)]

## <a name="channel-data"></a><span data-ttu-id="08b16-168">Datos de canal</span><span class="sxs-lookup"><span data-stu-id="08b16-168">Channel data</span></span>

<span data-ttu-id="08b16-169">La propiedad `ChannelData` de una actividad de mensaje puede usarse para implementar la funcionalidad específica del canal.</span><span class="sxs-lookup"><span data-stu-id="08b16-169">The `ChannelData` property of a message activity can be used to implement channel-specific functionality.</span></span> <span data-ttu-id="08b16-170">Para más información, vea [Implementación de una funcionalidad específica de canal](bot-builder-dotnet-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="08b16-170">For details, see [Implement channel-specific functionality](bot-builder-dotnet-channeldata.md).</span></span>

## <a name="text-to-speech"></a><span data-ttu-id="08b16-171">Texto a voz</span><span class="sxs-lookup"><span data-stu-id="08b16-171">Text to speech</span></span>

<span data-ttu-id="08b16-172">La propiedad `Speak` de una actividad de mensaje puede usarse para especificar el texto que va a decir el bot en un canal habilitado para voz.</span><span class="sxs-lookup"><span data-stu-id="08b16-172">The `Speak` property of a message activity can be used to specify the text to be spoken by your bot on a speech-enabled channel.</span></span> <span data-ttu-id="08b16-173">La propiedad `InputHint` de una actividad de mensajes puede usarse para controlar el estado del micrófono y del cuadro de entrada del cliente (si los hay).</span><span class="sxs-lookup"><span data-stu-id="08b16-173">The `InputHint` property of a message activity can be used to control the state of the client's microphone and input box (if any).</span></span> <span data-ttu-id="08b16-174">Para más información, vea [Incorporación de voz a mensajes](bot-builder-dotnet-text-to-speech.md).</span><span class="sxs-lookup"><span data-stu-id="08b16-174">For details, see [Add speech to messages](bot-builder-dotnet-text-to-speech.md).</span></span>

## <a name="suggested-actions"></a><span data-ttu-id="08b16-175">Acciones sugeridas</span><span class="sxs-lookup"><span data-stu-id="08b16-175">Suggested actions</span></span>

<span data-ttu-id="08b16-176">La propiedad `SuggestedActions` de una actividad de mensajes puede usarse para presentar los botones que el usuario puede pulsar para proporcionar la entrada.</span><span class="sxs-lookup"><span data-stu-id="08b16-176">The `SuggestedActions` property of a message activity can be used to present buttons that the user can tap to provide input.</span></span> <span data-ttu-id="08b16-177">A diferencia de los botones que aparecen en las tarjetas enriquecidas (que permanecen visibles y accesibles para el usuario incluso después de que se pulsen), los botones que aparecen en el panel de acciones sugeridas desaparecerán una vez que el usuario haya hecho una selección.</span><span class="sxs-lookup"><span data-stu-id="08b16-177">Unlike buttons that appear within rich cards (which remain visible and accessible to the user even after being tapped), buttons that appear within the suggested actions pane will disappear after the user makes a selection.</span></span> <span data-ttu-id="08b16-178">Para más información, vea [Incorporación de acciones sugeridas a mensajes](bot-builder-dotnet-add-suggested-actions.md).</span><span class="sxs-lookup"><span data-stu-id="08b16-178">For details, see [Add suggested actions to messages](bot-builder-dotnet-add-suggested-actions.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="08b16-179">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="08b16-179">Next steps</span></span>

<span data-ttu-id="08b16-180">Un bot y un usuario pueden enviarse mensajes entre sí.</span><span class="sxs-lookup"><span data-stu-id="08b16-180">A bot and a user can send messages to each other.</span></span> <span data-ttu-id="08b16-181">Cuando el mensaje es más complejo, el bot puede enviar una tarjeta enriquecida en un mensaje al usuario.</span><span class="sxs-lookup"><span data-stu-id="08b16-181">When the message is more complex, your bot can send a rich card in a message to the user.</span></span> <span data-ttu-id="08b16-182">Las tarjetas enriquecidas abarcan muchos de los escenarios de presentación e interacción que suelen ser necesarios en la mayoría de los bots.</span><span class="sxs-lookup"><span data-stu-id="08b16-182">Rich cards cover many presentation and interaction scenarios commonly needed in most bots.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="08b16-183">Enviar una tarjeta enriquecida en un mensaje</span><span class="sxs-lookup"><span data-stu-id="08b16-183">Send a rich card in a message</span></span>](bot-builder-dotnet-add-rich-card-attachments.md)

## <a name="additional-resources"></a><span data-ttu-id="08b16-184">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="08b16-184">Additional resources</span></span>

- [<span data-ttu-id="08b16-185">Introducción a las actividades</span><span class="sxs-lookup"><span data-stu-id="08b16-185">Activities overview</span></span>](bot-builder-dotnet-activities.md)
- [<span data-ttu-id="08b16-186">Envío y recepción de actividades</span><span class="sxs-lookup"><span data-stu-id="08b16-186">Send and receive activities</span></span>](bot-builder-dotnet-connector.md)
- [<span data-ttu-id="08b16-187">Incorporación de datos adjuntos con elementos multimedia a mensajes</span><span class="sxs-lookup"><span data-stu-id="08b16-187">Add media attachments to messages</span></span>](bot-builder-dotnet-add-media-attachments.md)
- [<span data-ttu-id="08b16-188">Incorporación de tarjetas enriquecidas a mensajes</span><span class="sxs-lookup"><span data-stu-id="08b16-188">Add rich cards to messages</span></span>](bot-builder-dotnet-add-rich-card-attachments.md)
- [<span data-ttu-id="08b16-189">Incorporación de voz a mensajes</span><span class="sxs-lookup"><span data-stu-id="08b16-189">Add speech to messages</span></span>](bot-builder-dotnet-text-to-speech.md)
- [<span data-ttu-id="08b16-190">Incorporación de acciones sugeridas a mensajes</span><span class="sxs-lookup"><span data-stu-id="08b16-190">Add suggested actions to messages</span></span>](bot-builder-dotnet-add-suggested-actions.md)
- [<span data-ttu-id="08b16-191">Implementación de una funcionalidad específica de canal</span><span class="sxs-lookup"><span data-stu-id="08b16-191">Implement channel-specific functionality</span></span>](bot-builder-dotnet-channeldata.md)
- <span data-ttu-id="08b16-192"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="08b16-192"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
- <span data-ttu-id="08b16-193"><a href="/dotnet/api/microsoft.bot.connector.imessageactivity" target="_blank">Interfaz IMessageActivity</a></span><span class="sxs-lookup"><span data-stu-id="08b16-193"><a href="/dotnet/api/microsoft.bot.connector.imessageactivity" target="_blank">IMessageActivity interface</a></span></span>

