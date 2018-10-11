---
title: Entidades y tipos de actividad | Microsoft Docs
description: Entidades y tipos de actividad.
keywords: entidades de mención, tipos de actividad, consumir entidades
author: ivorb
ms.author: v-ivorb
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 03/01/2018
ms.openlocfilehash: f6bf1d99922351a66a4e5401e744fad190746747
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389814"
---
# <a name="entities-and-activity-types"></a><span data-ttu-id="e0dbe-104">Entidades y tipos de actividad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-104">Entities and activity types</span></span>

<span data-ttu-id="e0dbe-105">Las entidades son una parte de una actividad y proporcionan información adicional sobre la actividad o la conversación.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-105">Entities are a part of an activity, and provide additional information about the activity or conversation.</span></span>

[!include[Entity boilerplate](includes/snippet-entity-boilerplate.md)]

## <a name="entities"></a><span data-ttu-id="e0dbe-106">Entidades</span><span class="sxs-lookup"><span data-stu-id="e0dbe-106">Entities</span></span>

<span data-ttu-id="e0dbe-107">La propiedad *entities* de un mensaje es una matriz de objetos <a href="http://schema.org/" target="_blank">schema.org</a> de extremo abierto que permite el intercambio de metadatos contextuales comunes entre el canal y el bot.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-107">The *entities* property of a message is an array of open-ended <a href="http://schema.org/" target="_blank">schema.org</a> objects which allows the exchange of common contextual metadata between the channel and bot.</span></span>

### <a name="mention-entities"></a><span data-ttu-id="e0dbe-108">Entidades de mención</span><span class="sxs-lookup"><span data-stu-id="e0dbe-108">Mention entities</span></span>

<span data-ttu-id="e0dbe-109">Muchos canales ofrecen la posibilidad de que un bot o usuario "mencione" a alguien dentro del contexto de una conversación.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-109">Many channels support the ability for a bot or user to "mention" someone within the context of a conversation.</span></span>
<span data-ttu-id="e0dbe-110">Para mencionar a un usuario en un mensaje, rellene la propiedad entities del mensaje con un objeto *mention*.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-110">To mention a user in a message, populate the message's entities property with a *mention* object.</span></span>
<span data-ttu-id="e0dbe-111">El objeto mention contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-111">The mention object contains these properties:</span></span>

| <span data-ttu-id="e0dbe-112">Propiedad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-112">Property</span></span> | <span data-ttu-id="e0dbe-113">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="e0dbe-113">Description</span></span> |
|----|----|
| <span data-ttu-id="e0dbe-114">Escriba</span><span class="sxs-lookup"><span data-stu-id="e0dbe-114">Type</span></span> | <span data-ttu-id="e0dbe-115">Tipo de la entidad ("mention")</span><span class="sxs-lookup"><span data-stu-id="e0dbe-115">type of the entity ("mention")</span></span> |
| <span data-ttu-id="e0dbe-116">Mentioned</span><span class="sxs-lookup"><span data-stu-id="e0dbe-116">Mentioned</span></span> | <span data-ttu-id="e0dbe-117">Objeto de cuenta de canal que indica a qué usuario se ha mencionado</span><span class="sxs-lookup"><span data-stu-id="e0dbe-117">channel account object that indicates which user was mentioned</span></span> | 
| <span data-ttu-id="e0dbe-118">Texto</span><span class="sxs-lookup"><span data-stu-id="e0dbe-118">Text</span></span> | <span data-ttu-id="e0dbe-119">Texto de la propiedad *activity.text* que representa la mención en sí misma (puede estar vacío o ser NULL)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-119">text within the *activity.text* property that represents the mention itself (may be empty or null)</span></span> |

<span data-ttu-id="e0dbe-120">En este ejemplo de código se muestra cómo se agrega una entidad mention a la colección entities:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-120">This code example shows how to add a mention entity to the entities collection.</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="e0dbe-121">C#</span><span class="sxs-lookup"><span data-stu-id="e0dbe-121">C#</span></span>](#tab/cs)
[!code-csharp[set Mention](includes/code/dotnet-create-messages.cs#setMention)]

> [!TIP]
> <span data-ttu-id="e0dbe-122">Al intentar determinar la intención del usuario, puede que el bot omita la parte del mensaje en la que se menciona.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-122">When attempting to determine user intent, the  bot may want to ignore that portion of the message where it is mentioned.</span></span> <span data-ttu-id="e0dbe-123">Llame al método `GetMentions` y evalúe los objetos `Mention` devueltos en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-123">Call the `GetMentions` method and evaluate the `Mention` objects returned in the response.</span></span>

# <a name="javascripttabjs"></a>[<span data-ttu-id="e0dbe-124">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e0dbe-124">JavaScript</span></span>](#tab/js)
```javascript
var entity = context.activity.entities;

const mention = {
    type: "Mention",
    text: "@johndoe",
    mentioned: {
        name: "John Doe",
        id: "UV341235"
    }
}

entity = [mention];
```

---

### <a name="place-objects"></a><span data-ttu-id="e0dbe-125">Objetos Place</span><span class="sxs-lookup"><span data-stu-id="e0dbe-125">Place objects</span></span>

<span data-ttu-id="e0dbe-126">La <a href="https://schema.org/Place" target="_blank">información relacionada con la ubicación</a> se puede transmitir dentro de un mensaje si se rellena la propiedad entities del mensaje con un objeto *Place* o *GeoCoordinates*.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-126"><a href="https://schema.org/Place" target="_blank">Location-related information</a> can be conveyed within a message by populating the message's entities property with either a *Place* object or a *GeoCoordinates* object.</span></span>

<span data-ttu-id="e0dbe-127">El objeto place contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-127">The place object contains these properties:</span></span>

| <span data-ttu-id="e0dbe-128">Propiedad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-128">Property</span></span> | <span data-ttu-id="e0dbe-129">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="e0dbe-129">Description</span></span> |
|----|----|
| <span data-ttu-id="e0dbe-130">Escriba</span><span class="sxs-lookup"><span data-stu-id="e0dbe-130">Type</span></span> | <span data-ttu-id="e0dbe-131">Tipo de la entidad ("Place")</span><span class="sxs-lookup"><span data-stu-id="e0dbe-131">type of the entity ("Place")</span></span> |
| <span data-ttu-id="e0dbe-132">Dirección</span><span class="sxs-lookup"><span data-stu-id="e0dbe-132">Address</span></span> | <span data-ttu-id="e0dbe-133">Descripción u objeto de dirección postal (en un futuro)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-133">description or postal address object (future)</span></span> |
| <span data-ttu-id="e0dbe-134">Geoárea</span><span class="sxs-lookup"><span data-stu-id="e0dbe-134">Geo</span></span> | <span data-ttu-id="e0dbe-135">GeoCoordinates</span><span class="sxs-lookup"><span data-stu-id="e0dbe-135">GeoCoordinates</span></span> |
| <span data-ttu-id="e0dbe-136">HasMap</span><span class="sxs-lookup"><span data-stu-id="e0dbe-136">HasMap</span></span> | <span data-ttu-id="e0dbe-137">Dirección URL de un mapa u objeto de mapa (en un futuro)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-137">URL to a map or map object (future)</span></span> |
| <span data-ttu-id="e0dbe-138">NOMBRE</span><span class="sxs-lookup"><span data-stu-id="e0dbe-138">Name</span></span> | <span data-ttu-id="e0dbe-139">Nombre del lugar</span><span class="sxs-lookup"><span data-stu-id="e0dbe-139">name of the place</span></span> |

<span data-ttu-id="e0dbe-140">El objeto geoCoordinates contiene estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-140">The geoCoordinates object contains these properties:</span></span>

| <span data-ttu-id="e0dbe-141">Propiedad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-141">Property</span></span> | <span data-ttu-id="e0dbe-142">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="e0dbe-142">Description</span></span> |
|----|----|
| <span data-ttu-id="e0dbe-143">Escriba</span><span class="sxs-lookup"><span data-stu-id="e0dbe-143">Type</span></span> | <span data-ttu-id="e0dbe-144">Tipo de la entidad ("GeoCoordinates")</span><span class="sxs-lookup"><span data-stu-id="e0dbe-144">type of the entity ("GeoCoordinates")</span></span> |
| <span data-ttu-id="e0dbe-145">NOMBRE</span><span class="sxs-lookup"><span data-stu-id="e0dbe-145">Name</span></span> | <span data-ttu-id="e0dbe-146">Nombre del lugar</span><span class="sxs-lookup"><span data-stu-id="e0dbe-146">name of the place</span></span> |
| <span data-ttu-id="e0dbe-147">Longitud</span><span class="sxs-lookup"><span data-stu-id="e0dbe-147">Longitude</span></span> | <span data-ttu-id="e0dbe-148">Longitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-148">longitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> |
| <span data-ttu-id="e0dbe-149">Longitud</span><span class="sxs-lookup"><span data-stu-id="e0dbe-149">Longitude</span></span> | <span data-ttu-id="e0dbe-150">Latitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-150">latitude of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> |
| <span data-ttu-id="e0dbe-151">Elevation</span><span class="sxs-lookup"><span data-stu-id="e0dbe-151">Elevation</span></span> | <span data-ttu-id="e0dbe-152">Altitud de la ubicación (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-152">elevation of the location (<a href="https://en.wikipedia.org/wiki/World_Geodetic_System" target="_blank">WGS 84</a>)</span></span> |

<span data-ttu-id="e0dbe-153">En este ejemplo de código se muestra cómo se agrega una entidad place a la colección entities:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-153">This code example shows how to add a place entity to the entities collection:</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="e0dbe-154">C#</span><span class="sxs-lookup"><span data-stu-id="e0dbe-154">C#</span></span>](#tab/cs)
[!code-csharp[set GeoCoordinates](includes/code/dotnet-create-messages.cs#setGeoCoord)]

# <a name="javascripttabjs"></a>[<span data-ttu-id="e0dbe-155">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e0dbe-155">JavaScript</span></span>](#tab/js)
```javascript
var entity = context.activity.entities;

const place = {
    elavation: 100,
    type: "GeoCoordinates",
    name : "myPlace",
    latitude: 123,
    longitude: 234
};

entity = [place];

```

---

### <a name="consume-entities"></a><span data-ttu-id="e0dbe-156">Consumir entidades</span><span class="sxs-lookup"><span data-stu-id="e0dbe-156">Consume entities</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="e0dbe-157">C#</span><span class="sxs-lookup"><span data-stu-id="e0dbe-157">C#</span></span>](#tab/cs)

<span data-ttu-id="e0dbe-158">Para consumir entidades, use la palabra clave `dynamic` o clases fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-158">To consume entities, use either the `dynamic` keyword or strongly-typed classes.</span></span>

<span data-ttu-id="e0dbe-159">En este ejemplo de código se muestra cómo usar la palabra clave `dynamic` para procesar una entidad en la propiedad `Entities` de un mensaje:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-159">This code example shows how to use the `dynamic` keyword to process an entity within the `Entities` property of a message:</span></span>

[!code-csharp[examine entity using dynamic keyword](includes/code/dotnet-create-messages.cs#examineEntity1)]

<span data-ttu-id="e0dbe-160">En este ejemplo de código se muestra cómo usar una clase fuertemente tipada para procesar una entidad en la propiedad `Entities` de un mensaje:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-160">This code example shows how to use a strongly-typed class to process an entity within the `Entities` property of a message:</span></span>

[!code-csharp[examine entity using typed class](includes/code/dotnet-create-messages.cs#examineEntity2)]

# <a name="javascripttabjs"></a>[<span data-ttu-id="e0dbe-161">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e0dbe-161">JavaScript</span></span>](#tab/js)

<span data-ttu-id="e0dbe-162">En este ejemplo de código se muestra cómo procesar una entidad en la propiedad `entity` de un mensaje:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-162">This code example shows how to process an entity within the `entity` property of a message:</span></span>

```javascript
if (entity[0].type === "GeoCoordinates" && entity[0].latitude > 34) {
    // do something
}
```

---

## <a name="activity-types"></a><span data-ttu-id="e0dbe-163">Tipos de actividad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-163">Activity types</span></span>

<span data-ttu-id="e0dbe-164">Este ejemplo de código se muestra cómo procesar una actividad de tipo **message**:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-164">This code example show how to process an activity of type **message**:</span></span>

# <a name="ctabcs"></a>[<span data-ttu-id="e0dbe-165">C#</span><span class="sxs-lookup"><span data-stu-id="e0dbe-165">C#</span></span>](#tab/cs)

```cs
if (context.Activity.Type == ActivityTypes.Message){
    // do something
}
```

# <a name="javascripttabjs"></a>[<span data-ttu-id="e0dbe-166">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e0dbe-166">JavaScript</span></span>](#tab/js)

```js
if(context.activity.type === 'message'){
    // do something
}
```

---

<span data-ttu-id="e0dbe-167">Las actividades pueden ser de otros tipos además de **message** (el más común).</span><span class="sxs-lookup"><span data-stu-id="e0dbe-167">Activities can be of several different types past the most common **message**.</span></span> <span data-ttu-id="e0dbe-168">Hay varios tipos de actividad:</span><span class="sxs-lookup"><span data-stu-id="e0dbe-168">There are several activity types:</span></span>

| <span data-ttu-id="e0dbe-169">Tipo de actividad</span><span class="sxs-lookup"><span data-stu-id="e0dbe-169">Activity.Type</span></span> | <span data-ttu-id="e0dbe-170">Interfaz</span><span class="sxs-lookup"><span data-stu-id="e0dbe-170">Interface</span></span> | <span data-ttu-id="e0dbe-171">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="e0dbe-171">Description</span></span> |
|-----|-----|-----|
| [<span data-ttu-id="e0dbe-172">message</span><span class="sxs-lookup"><span data-stu-id="e0dbe-172">message</span></span>](#message) | <span data-ttu-id="e0dbe-173">IMessageActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-173">IMessageActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-174">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-174">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-175">Representa una comunicación entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-175">Represents a communication between bot and user.</span></span> |
| [<span data-ttu-id="e0dbe-176">contactRelationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-176">contactRelationUpdate</span></span>](#contactrelationupdate) | <span data-ttu-id="e0dbe-177">IContactRelationUpdateActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-177">IContactRelationUpdateActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-178">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-178">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-179">Indica que el bot se agregó o quitó de la lista de contactos de un usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-179">Indicates that the bot was added or removed from a user's contact list.</span></span> |
| [<span data-ttu-id="e0dbe-180">conversationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-180">conversationUpdate</span></span>](#conversationupdate) | <span data-ttu-id="e0dbe-181">IConversationUpdateActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-181">IConversationUpdateActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-182">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-182">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-183">Indica que el bot se agregó a una conversación, que otros miembros se agregaron o se quitaron de la conversación, o bien que los metadatos de la conversación han cambiado.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-183">Indicates that the bot was added to a conversation, other members were added to or removed from the conversation, or conversation metadata has changed.</span></span> |
| [<span data-ttu-id="e0dbe-184">deleteUserData</span><span class="sxs-lookup"><span data-stu-id="e0dbe-184">deleteUserData</span></span>](#deleteuserdata) | <span data-ttu-id="e0dbe-185">N/D</span><span class="sxs-lookup"><span data-stu-id="e0dbe-185">n/a</span></span> | <span data-ttu-id="e0dbe-186">Indica a un bot que un usuario ha solicitado que el bot elimine todos los datos de usuario que haya podido almacenar.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-186">Indicates to a bot that a user has requested that the bot delete any user data it may have stored.</span></span> |
| [<span data-ttu-id="e0dbe-187">endOfConversation</span><span class="sxs-lookup"><span data-stu-id="e0dbe-187">endOfConversation</span></span>](#endofconversation) | <span data-ttu-id="e0dbe-188">IEndOfConversationActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-188">IEndOfConversationActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-189">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-189">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-190">Indica el final de una conversación.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-190">Indicates the end of a conversation.</span></span> |
| [<span data-ttu-id="e0dbe-191">event</span><span class="sxs-lookup"><span data-stu-id="e0dbe-191">event</span></span>](#event) | <span data-ttu-id="e0dbe-192">IEventActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-192">IEventActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-193">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-193">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-194">Representa una comunicación enviada a un bot que no es visible para el usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-194">Represents a communication sent to a bot that is not visible to the user.</span></span> |
| [<span data-ttu-id="e0dbe-195">installationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-195">installationUpdate</span></span>](#installationupdate) | <span data-ttu-id="e0dbe-196">IInstallationUpdateActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-196">IInstallationUpdateActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-197">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-197">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-198">Representa una instalación o desinstalación de un bot dentro de una unidad organizativa (por ejemplo, un inquilino de cliente o "equipo") de un canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-198">Represents an installation or uninstallation of a bot within an organizational unit (such as a customer tenant or "team") of a channel.</span></span> |
| [<span data-ttu-id="e0dbe-199">invoke</span><span class="sxs-lookup"><span data-stu-id="e0dbe-199">invoke</span></span>](#invoke) | <span data-ttu-id="e0dbe-200">IInvokeActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-200">IInvokeActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-201">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-201">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-202">Representa una comunicación enviada a un bot para solicitarle que realice una operación específica.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-202">Represents a communication sent to a bot to request that it perform a specific operation.</span></span> <span data-ttu-id="e0dbe-203">Este tipo de actividad está reservado para uso interno de Microsoft Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-203">This activity type is reserved for internal use by the Microsoft Bot Framework.</span></span> |
| [<span data-ttu-id="e0dbe-204">messageReaction</span><span class="sxs-lookup"><span data-stu-id="e0dbe-204">messageReaction</span></span>](#messagereaction) | <span data-ttu-id="e0dbe-205">IMessageReactionActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-205">IMessageReactionActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-206">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-206">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-207">Indica que un usuario ha reaccionado a una actividad existente.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-207">Indicates that a user has reacted to an existing activity.</span></span> <span data-ttu-id="e0dbe-208">Por ejemplo, un usuario hace clic en el botón "Me gusta" de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-208">For example, a user clicks the "Like" button on a message.</span></span> |
| [<span data-ttu-id="e0dbe-209">typing</span><span class="sxs-lookup"><span data-stu-id="e0dbe-209">typing</span></span>](#typing) | <span data-ttu-id="e0dbe-210">ITypingActivity (C#)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-210">ITypingActivity (C#)</span></span> <br> <span data-ttu-id="e0dbe-211">Activity (JS)</span><span class="sxs-lookup"><span data-stu-id="e0dbe-211">Activity (JS)</span></span> | <span data-ttu-id="e0dbe-212">Indica que el usuario o el bot en el otro extremo de la conversación está redactando una respuesta.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-212">Indicates that the user or bot on the other end of the conversation is compiling a response.</span></span> |

## <a name="message"></a><span data-ttu-id="e0dbe-213">Mensaje</span><span class="sxs-lookup"><span data-stu-id="e0dbe-213">message</span></span>

<!-- Only the last link is different. -->
::: moniker range="azure-bot-service-3.0"
<span data-ttu-id="e0dbe-214">El bot enviará actividades de mensaje para comunicar información a los usuarios y recibir actividades de mensaje de ellos.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-214">Your bot will send message activities to communicate information to and receive message activities from users.</span></span>
<span data-ttu-id="e0dbe-215">Algunos mensajes pueden constar simplemente de texto sin formato, mientras que otros pueden incluir contenido más enriquecido, como [texto que se va a decir](v4sdk/bot-builder-howto-send-messages.md#send-a-spoken-message), [acciones sugeridas](v4sdk/bot-builder-howto-add-suggested-actions.md), [datos adjuntos multimedia](v4sdk/bot-builder-howto-add-media-attachments.md), [tarjetas enriquecidas](v4sdk/bot-builder-howto-add-media-attachments.md#send-a-hero-card) y [datos específicos del canal](~/dotnet/bot-builder-dotnet-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="e0dbe-215">Some messages may simply consist of plain text, while others may contain richer content such as [text to be spoken](v4sdk/bot-builder-howto-send-messages.md#send-a-spoken-message), [suggested actions](v4sdk/bot-builder-howto-add-suggested-actions.md), [media attachments](v4sdk/bot-builder-howto-add-media-attachments.md), [rich cards](v4sdk/bot-builder-howto-add-media-attachments.md#send-a-hero-card), and [channel-specific data](~/dotnet/bot-builder-dotnet-channeldata.md).</span></span>
::: moniker-end
::: moniker range="azure-bot-service-4.0"
<span data-ttu-id="e0dbe-216">El bot enviará actividades de mensaje para comunicar información a los usuarios y recibir actividades de mensaje de ellos.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-216">Your bot will send message activities to communicate information to and receive message activities from users.</span></span>
<span data-ttu-id="e0dbe-217">Algunos mensajes pueden constar simplemente de texto sin formato, mientras que otros pueden incluir contenido más enriquecido, como [texto que se va a decir](v4sdk/bot-builder-howto-send-messages.md#send-a-spoken-message), [acciones sugeridas](v4sdk/bot-builder-howto-add-suggested-actions.md), [datos adjuntos multimedia](v4sdk/bot-builder-howto-add-media-attachments.md), [tarjetas enriquecidas](v4sdk/bot-builder-howto-add-media-attachments.md#send-a-hero-card) y [datos específicos del canal](~/v4sdk/bot-builder-channeldata.md).</span><span class="sxs-lookup"><span data-stu-id="e0dbe-217">Some messages may simply consist of plain text, while others may contain richer content such as [text to be spoken](v4sdk/bot-builder-howto-send-messages.md#send-a-spoken-message), [suggested actions](v4sdk/bot-builder-howto-add-suggested-actions.md), [media attachments](v4sdk/bot-builder-howto-add-media-attachments.md), [rich cards](v4sdk/bot-builder-howto-add-media-attachments.md#send-a-hero-card), and [channel-specific data](~/v4sdk/bot-builder-channeldata.md).</span></span>
::: moniker-end

## <a name="contactrelationupdate"></a><span data-ttu-id="e0dbe-218">contactRelationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-218">contactRelationUpdate</span></span>

<span data-ttu-id="e0dbe-219">Un bot recibe una actividad de actualización de relación de contacto siempre que se agrega o se quita de la lista de contactos de un usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-219">A bot receives a contact relation update activity whenever it is added to or removed from a user's contact list.</span></span> <span data-ttu-id="e0dbe-220">El valor de la propiedad de acción de la actividad (add | remove) indica si el bot se ha agregado o quitado de la lista de contactos del usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-220">The value of the activity's action property (add | remove) indicates whether the bot has been added or removed from the user's contact list.</span></span>

## <a name="conversationupdate"></a><span data-ttu-id="e0dbe-221">conversationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-221">conversationUpdate</span></span>

<span data-ttu-id="e0dbe-222">Un bot recibe una actividad de actualización de la conversación cada vez que se ha agregado a una conversación, se han agregado otros miembros a la conversación (o se han eliminado de ella), o bien han cambiado los metadatos de la conversación.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-222">A bot receives a conversation update activity whenever it has been added to a conversation, other members have been added to or removed from a conversation, or conversation metadata has changed.</span></span>

<span data-ttu-id="e0dbe-223">Si se han agregado miembros a la conversación, la propiedad added de los miembros de la actividad contendrá una matriz de objetos de cuenta de canal para identificar a los miembros nuevos.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-223">If members have been added to the conversation, the activity's members added property will contain an array of channel account objects to identify the new members.</span></span>

<span data-ttu-id="e0dbe-224">Para determinar si el bot se ha agregado a la conversación (es decir, es uno de los miembros nuevos), evalúe si el valor de identificador de destinatario para la actividad (es decir, el identificador del bot) coincide con la propiedad Id de cualquiera de las cuentas de la matriz members added.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-224">To determine whether your bot has been added to the conversation (i.e., is one of the new members), evaluate whether the recipient Id value for the activity (i.e., your bot's ID) matches the Id property for any of the accounts in the members added array.</span></span>

<span data-ttu-id="e0dbe-225">Si se han quitado miembros de la conversación, la propiedad removed de los miembros contendrá una matriz de objetos de cuenta de canal para identificar a los miembros que se han eliminado.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-225">If members have been removed from the conversation, the members removed property will contain an array of channel account objects to identify the removed members.</span></span>

> [!TIP]
> <span data-ttu-id="e0dbe-226">Si el bot recibe una actividad de actualización de la conversación en la que se indica que un usuario se ha unido a la conversación, puede elegir que le responda enviando un mensaje de bienvenida a ese usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-226">If your bot receives a conversation update activity indicating that a user has joined the conversation, you may choose to have it respond by sending a welcome message to that user.</span></span>

## <a name="deleteuserdata"></a><span data-ttu-id="e0dbe-227">deleteUserData</span><span class="sxs-lookup"><span data-stu-id="e0dbe-227">deleteUserData</span></span>

<span data-ttu-id="e0dbe-228">Un bot recibe una actividad de eliminación de datos de usuario cuando un usuario solicita la eliminación de cualquier dato que el bot haya conservado previamente para el usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-228">A bot receives a delete user data activity when a user requests deletion of any data that the bot has previously persisted for him or her.</span></span> <span data-ttu-id="e0dbe-229">Si el bot recibe este tipo de actividad, debe eliminar cualquier información de identificación personal (PII) que haya almacenado previamente para el usuario que ha realizado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-229">If your bot receives this type of activity, it should delete any personally identifiable information (PII) that it has previously stored for the user that made the request.</span></span>

## <a name="endofconversation"></a><span data-ttu-id="e0dbe-230">endOfConversation</span><span class="sxs-lookup"><span data-stu-id="e0dbe-230">endOfConversation</span></span>

<span data-ttu-id="e0dbe-231">Un bot recibe una actividad de finalización de la conversación para indicar que el usuario ha puesto fin a la conversación.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-231">A bot receives an end of conversation activity to indicate that the user has ended the conversation.</span></span> <span data-ttu-id="e0dbe-232">Un bot podría enviar una actividad de finalización de la conversación para indicar al usuario que la conversación está llegando a su fin.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-232">A bot may send an end of Conversation activity to indicate to the user that the conversation is ending.</span></span>

## <a name="event"></a><span data-ttu-id="e0dbe-233">event</span><span class="sxs-lookup"><span data-stu-id="e0dbe-233">event</span></span>

<span data-ttu-id="e0dbe-234">El bot puede recibir una actividad de evento de un proceso externo o un servicio que quiere transmitir información al bot sin que sea visible para los usuarios.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-234">Your bot may receive an event activity from an external process or service that wants to communicate information to your bot without that information being visible to users.</span></span> <span data-ttu-id="e0dbe-235">Normalmente, el remitente de una actividad de evento no espera que el bot acuse recibo de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-235">The sender of an event activity typically does not expect the bot to acknowledge receipt in any way.</span></span>

## <a name="installationupdate"></a><span data-ttu-id="e0dbe-236">installationUpdate</span><span class="sxs-lookup"><span data-stu-id="e0dbe-236">installationUpdate</span></span>

<span data-ttu-id="e0dbe-237">Las actividades de actualización de la instalación representan una instalación o desinstalación de un bot dentro de una unidad organizativa (por ejemplo, un inquilino de cliente o "equipo") de un canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-237">Installation update activities represent an installation or uninstallation of a bot within an organizational unit (such as a customer tenant or "team") of a channel.</span></span> <span data-ttu-id="e0dbe-238">Por lo general, las actividades de actualización de la instalación no representan la adición o eliminación de un canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-238">Installation update activities generally do not represent adding or removing a channel.</span></span> <span data-ttu-id="e0dbe-239">Los canales pueden enviar actividades de instalación cuando se agrega o se quita un bot de un inquilino, equipo u otra unidad organizativa dentro del canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-239">Channels may send installation activities when a bot is added to or removed from a tenant, team, or other organization unit within the channel.</span></span> <span data-ttu-id="e0dbe-240">Los canales no deberían enviar actividades de instalación cuando el bot se instala o se quita de un canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-240">Channels should not send installation activities when the bot is installed into or removed from a channel.</span></span>

## <a name="invoke"></a><span data-ttu-id="e0dbe-241">invoke</span><span class="sxs-lookup"><span data-stu-id="e0dbe-241">invoke</span></span>

<span data-ttu-id="e0dbe-242">El bot puede recibir una actividad invoke que representa una solicitud para que realice una operación específica.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-242">Your bot may receive an invoke activity that represents a request for it to perform a specific operation.</span></span>
<span data-ttu-id="e0dbe-243">El remitente de una actividad invoke normalmente espera que el bot confirme la recepción a través de la respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-243">The sender of an invoke activity typically expects the bot to acknowledge receipt via HTTP response.</span></span>
<span data-ttu-id="e0dbe-244">Este tipo de actividad está reservado para uso interno de Microsoft Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-244">This activity type is reserved for internal use by the Microsoft Bot Framework.</span></span>

## <a name="messagereaction"></a><span data-ttu-id="e0dbe-245">messageReaction</span><span class="sxs-lookup"><span data-stu-id="e0dbe-245">messageReaction</span></span>

<span data-ttu-id="e0dbe-246">Algunos canales enviarán actividades de reacción de mensaje al bot cuando un usuario reacciona a una actividad existente.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-246">Some channels will send message reaction activities to your bot when a user reacted to an existing activity.</span></span> <span data-ttu-id="e0dbe-247">Por ejemplo, un usuario hace clic en el botón "Me gusta" de un mensaje.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-247">For example, a user clicks the "Like" button on a message.</span></span> <span data-ttu-id="e0dbe-248">La propiedad ReplyToId indicará a qué actividad ha reaccionado al usuario.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-248">The replyToId property will indicate which activity the user reacted to.</span></span>

<span data-ttu-id="e0dbe-249">La actividad de reacción de mensaje puede corresponder a cualquier número de tipos de reacción de mensaje definidos por el canal.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-249">The message reaction activity may correspond to any number of message reaction types that the channel defined.</span></span> <span data-ttu-id="e0dbe-250">Por ejemplo, "Like" o "PlusOne" son tipos de reacción que un canal puede enviar.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-250">For example, "Like" or "PlusOne" as reaction types that a channel may send.</span></span>

## <a name="typing"></a><span data-ttu-id="e0dbe-251">typing</span><span class="sxs-lookup"><span data-stu-id="e0dbe-251">typing</span></span>

<span data-ttu-id="e0dbe-252">Un bot recibe una actividad de escritura para indicar que el usuario está escribiendo una respuesta.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-252">A bot receives a typing activity to indicate that the user is typing a response.</span></span>
<span data-ttu-id="e0dbe-253">Un bot puede enviar una actividad de escritura para indicar al usuario que está trabajando para satisfacer una solicitud o compilar una respuesta.</span><span class="sxs-lookup"><span data-stu-id="e0dbe-253">A bot may send a typing activity to indicate to the user that it is working to fulfill a request or compile a response.</span></span>

::: moniker range="azure-bot-service-3.0"
## <a name="additional-resources"></a><span data-ttu-id="e0dbe-254">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e0dbe-254">Additional resources</span></span>

- <span data-ttu-id="e0dbe-255"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Clase Activity</a></span><span class="sxs-lookup"><span data-stu-id="e0dbe-255"><a href="https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html" target="_blank">Activity class</a></span></span>
::: moniker-end
