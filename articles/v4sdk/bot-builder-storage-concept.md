---
redirect_url: /bot-framework/bot-builder-howto-v4-state
ms.openlocfilehash: e5da105e32ae748383d376f90afd9aebbf4c7aa5
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46708121"
---
<a name="--"></a><span data-ttu-id="2057b-101"><!--</span><span class="sxs-lookup"><span data-stu-id="2057b-101"><!--</span></span>
---
<span data-ttu-id="2057b-102">title: Estado y almacenamiento | Microsoft Docs description: Describe el administrador de estado, el estado de la conversación y el estado del usuario en el SDK Bot Builder.</span><span class="sxs-lookup"><span data-stu-id="2057b-102">title: State and storage | Microsoft Docs description: Describes what the state manager, conversation state and user state is within the Bot Builder SDK.</span></span>
<span data-ttu-id="2057b-103">keywords: LUIS, estado de la conversación, estado del usuario, almacenamiento, administración del estado author: DeniseMak ms.author: v-demak manager: kamrani ms.topic: article ms.prod: bot-framework ms.date: 02/15/2018 monikerRange: 'azure-bot-service-4.0'</span><span class="sxs-lookup"><span data-stu-id="2057b-103">keywords: LUIS, conversation state, user state, storage, manage state author: DeniseMak ms.author: v-demak manager: kamrani ms.topic: article ms.prod: bot-framework ms.date: 02/15/2018 monikerRange: 'azure-bot-service-4.0'</span></span>
---

# <a name="state-and-storage"></a><span data-ttu-id="2057b-104">Estado y almacenamiento</span><span class="sxs-lookup"><span data-stu-id="2057b-104">State and storage</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="2057b-105">Una clave del diseño correcto de bots consiste en realizar el seguimiento del contexto de una conversación, para que el bot recuerde cosas como las respuestas a preguntas anteriores.</span><span class="sxs-lookup"><span data-stu-id="2057b-105">A key to good bot design is to track the context of a conversation, so that your bot remembers things like the answers to previous questions.</span></span>
<span data-ttu-id="2057b-106">En función del uso previsto del bot, es posible que incluso sea necesario realizar el seguimiento de la información de estado o almacenamiento durante más tiempo que la duración de la conversación.</span><span class="sxs-lookup"><span data-stu-id="2057b-106">Depending on what your bot is used for, you may even need to keep track of state or store information for longer than the lifetime of the conversation.</span></span>
<span data-ttu-id="2057b-107">El *estado* de un bot es la información que recuerda con el fin de responder de forma adecuada a los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="2057b-107">A bot's *state* is information it remembers in order to respond appropriately to incoming messages.</span></span> <span data-ttu-id="2057b-108">El SDK de Bot Builder proporciona clases para almacenar y recuperar datos de estado como un objeto asociado a un usuario o una conversación.</span><span class="sxs-lookup"><span data-stu-id="2057b-108">The Bot Builder SDK provides classes for storing and retrieving state data as an object associated with a user or a conversation.</span></span>

* <span data-ttu-id="2057b-109">Las **propiedades de la conversación** ayudan al bot a realizar el seguimiento de la conversación actual que tiene con el usuario.</span><span class="sxs-lookup"><span data-stu-id="2057b-109">**Conversation properties** help your bot keep track of the current conversation the bot is having with the user.</span></span> <span data-ttu-id="2057b-110">Si es necesario que el bot complete una secuencia de pasos o que cambie entre temas de conversación, se pueden usar las propiedades de la conversación para administrar los pasos en una secuencia o realizar el seguimiento del tema actual.</span><span class="sxs-lookup"><span data-stu-id="2057b-110">If your bot needs to complete a sequence of steps or switch between conversation topics, you can use conversation properties to manage steps in a sequence or track the current topic.</span></span> <span data-ttu-id="2057b-111">Como las propiedades de la conversación reflejan el estado de la conversación actual, normalmente se borran al final de una conversación, cuando el bot recibe una actividad de _fin de la conversación_.</span><span class="sxs-lookup"><span data-stu-id="2057b-111">Since conversation properties reflect the state of the current conversation, you typically clear them at the end of a conversation, when the bot receives an _end of conversation_ activity.</span></span>
* <span data-ttu-id="2057b-112">Las **propiedades de usuario** se pueden usar para muchos propósitos, como determinar dónde dejó la conversación anterior el usuario o simplemente saludar a un usuario por su nombre cuando regrese.</span><span class="sxs-lookup"><span data-stu-id="2057b-112">**User properties** can be used for many purposes, such as determining where the user's prior conversation left off or simply greeting a returning user by name.</span></span> <span data-ttu-id="2057b-113">Si almacena las preferencias del usuario, puede usar esa información para personalizar la conversación la próxima vez que chatee.</span><span class="sxs-lookup"><span data-stu-id="2057b-113">If you store a user's preferences, you can use that information to customize the conversation the next time you chat.</span></span> <span data-ttu-id="2057b-114">Por ejemplo, podría alertar al usuario sobre un artículo de noticias sobre un tema que le interesa, o bien cuando haya una cita disponible.</span><span class="sxs-lookup"><span data-stu-id="2057b-114">For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available.</span></span> <span data-ttu-id="2057b-115">Debe borrarlas si el bot recibe una actividad _eliminar datos de usuario_.</span><span class="sxs-lookup"><span data-stu-id="2057b-115">You should clear them if the bot receives a _delete user data_ activity.</span></span>

<span data-ttu-id="2057b-116">Puede usar [Almacenamiento](bot-builder-howto-v4-storage.md) para leer y escribir en el almacenamiento persistente.</span><span class="sxs-lookup"><span data-stu-id="2057b-116">You can use [Storage](bot-builder-howto-v4-storage.md) to read from and write to persistent storage.</span></span> <span data-ttu-id="2057b-117">Esto permite que el bot haga cosas como actualizar recursos compartidos, registrar confirmaciones de asistencia o votos, o bien leer datos meteorológicos históricos.</span><span class="sxs-lookup"><span data-stu-id="2057b-117">This enables your bot to do things such as update shared resources, record RSVPs or votes, or read historical weather data.</span></span> <span data-ttu-id="2057b-118">Al igual que una aplicación usa el almacenamiento para lograr sus objetivos, el bot puede hacerlo dentro de la conversación con el usuario.</span><span class="sxs-lookup"><span data-stu-id="2057b-118">In the same way an app uses storage to achieve its objectives, your bot can do so within the conversation with your user.</span></span>

<!-- 
*Conversation state* pertains to the current conversation that the user is having with your bot. When the conversation ends, your bot deletes this data.

You can also store *user state* that persists after a conversation ends. For example, if you store a user's preferences, you can use that information to customize the conversation the next time you chat. For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available. 
-->

<!-- You should generally avoid saving state using a global variable or function closures.
Doing so will create issues when you want to scale out your bot. Instead, use the conversation state and user state middleware that the BotBuilder SDK provides --> 

<!--
## Types of underlying storage

The SDK provides bot state manager middleware to persist conversation and user state. State can be accessed using the bot's context. This state manager can use Azure Table Storage, file storage, or memory storage as the underlying data storage. You can also create your own storage components for your bot.

Bots built using Azure Table Storage can be designed to be stateless and scalable across multiple compute nodes.

> [!NOTE] 
> File and memory storage won't scale across nodes.

## Writing directly to storage

You can also use the Bot Builder SDK to read and write data directly to storage, without using middleware or without using the bot context. This can be appropriate to data that your bot uses, that comes from a source outside your bot's conversation flow.

For example, let's say your bot allows the user to ask for the weather report, and your bot retrieves the weather report for a specified date, by reading it from an external database. The content of the weather database isn't dependent on user information or the conversation context, so you could just read it directly from storage instead of using the state manager.  See [How to write directly to storage](bot-builder-howto-v4-storage.md) for an example.

## Next steps

Next, lets get into how activities are processed, in depth, and how we respond to them.

> [!div class="nextstepaction"]
> [Activity Processing](bot-builder-concept-activity-processing.md)

## Additional resources

- [How to save state](bot-builder-howto-v4-state.md)
- [How to write directly to storage](bot-builder-howto-v4-storage.md)

-->
