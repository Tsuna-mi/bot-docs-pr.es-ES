---
title: Guardar el estado y acceder a los datos | Microsoft Docs
description: Describe el administrador de estado, el estado de la conversación y el estado del usuario dentro del SDK de Bot Builder.
keywords: LUIS, estado de la conversación, estado del usuario, almacenamiento, administración del estado
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 02/15/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 56814ab12a85d18e52b0d5ec83fd81682f3b9f60
ms.sourcegitcommit: f95702d27abbd242c902eeb218d55a72df56ce56
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39306473"
---
# <a name="save-state-and-access-data"></a><span data-ttu-id="ebfab-104">Guardar el estado y acceder a los datos</span><span class="sxs-lookup"><span data-stu-id="ebfab-104">Save state and access data</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="ebfab-105">Una clave del diseño correcto de bots consiste en realizar el seguimiento del contexto de una conversación, para que el bot recuerde cosas como las respuestas a preguntas anteriores.</span><span class="sxs-lookup"><span data-stu-id="ebfab-105">A key to good bot design is to track the context of a conversation, so that your bot remembers things like the answers to previous questions.</span></span>
<span data-ttu-id="ebfab-106">En función del uso previsto del bot, es posible que incluso sea necesario realizar el seguimiento de la información de estado o almacenamiento durante más tiempo que la duración de la conversación.</span><span class="sxs-lookup"><span data-stu-id="ebfab-106">Depending on what your bot is used for, you may even need to keep track of state or store information for longer than the lifetime of the conversation.</span></span>
<span data-ttu-id="ebfab-107">El *estado* de un bot es la información que recuerda con el fin de responder de forma adecuada a los mensajes entrantes.</span><span class="sxs-lookup"><span data-stu-id="ebfab-107">A bot's *state* is information it remembers in order to respond appropriately to incoming messages.</span></span> <span data-ttu-id="ebfab-108">El SDK de Bot Builder proporciona clases para almacenar y recuperar datos de estado como un objeto asociado a un usuario o una conversación.</span><span class="sxs-lookup"><span data-stu-id="ebfab-108">The Bot Builder SDK provides classes for storing and retrieving state data as an object associated with a user or a conversation.</span></span>

* <span data-ttu-id="ebfab-109">Las **propiedades de la conversación** ayudan al bot a realizar el seguimiento de la conversación actual que tiene con el usuario.</span><span class="sxs-lookup"><span data-stu-id="ebfab-109">**Conversation properties** help your bot keep track of the current conversation the bot is having with the user.</span></span> <span data-ttu-id="ebfab-110">Si es necesario que el bot complete una secuencia de pasos o que cambie entre temas de conversación, se pueden usar las propiedades de la conversación para administrar los pasos en una secuencia o realizar el seguimiento del tema actual.</span><span class="sxs-lookup"><span data-stu-id="ebfab-110">If your bot needs to complete a sequence of steps or switch between conversation topics, you can use conversation properties to manage steps in a sequence or track the current topic.</span></span> <span data-ttu-id="ebfab-111">Como las propiedades de la conversación reflejan el estado de la conversación actual, normalmente se borran al final de una sesión, cuando el bot recibe una actividad de _fin de la conversación_.</span><span class="sxs-lookup"><span data-stu-id="ebfab-111">Since conversation properties reflect the state of the current conversation, you typically clear them at the end of a session, when the bot receives an _end of conversation_ activity.</span></span>
* <span data-ttu-id="ebfab-112">Las **propiedades de usuario** se pueden usar para muchos propósitos, como determinar dónde dejó la conversación anterior el usuario o simplemente saludar a un usuario por su nombre cuando regrese.</span><span class="sxs-lookup"><span data-stu-id="ebfab-112">**User properties** can be used for many purposes, such as determining where the user's prior conversation left off or simply greeting a returning user by name.</span></span> <span data-ttu-id="ebfab-113">Si almacena las preferencias del usuario, puede usar esa información para personalizar la conversación la próxima vez que chatee.</span><span class="sxs-lookup"><span data-stu-id="ebfab-113">If you store a user's preferences, you can use that information to customize the conversation the next time you chat.</span></span> <span data-ttu-id="ebfab-114">Por ejemplo, podría alertar al usuario sobre un artículo de noticias sobre un tema que le interesa, o bien cuando haya una cita disponible.</span><span class="sxs-lookup"><span data-stu-id="ebfab-114">For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available.</span></span> <span data-ttu-id="ebfab-115">Debe borrarlas si el bot recibe una actividad _eliminar datos de usuario_.</span><span class="sxs-lookup"><span data-stu-id="ebfab-115">You should clear them if the bot receives a _delete user data_ activity.</span></span>

<span data-ttu-id="ebfab-116">Puede usar [Almacenamiento](bot-builder-howto-v4-storage.md) para leer y escribir en el almacenamiento persistente.</span><span class="sxs-lookup"><span data-stu-id="ebfab-116">You can use [Storage](bot-builder-howto-v4-storage.md) to read from and write to persistent storage.</span></span> <span data-ttu-id="ebfab-117">Esto permite que el bot haga cosas como actualizar recursos compartidos, registrar confirmaciones de asistencia o votos, o bien leer datos meteorológicos históricos.</span><span class="sxs-lookup"><span data-stu-id="ebfab-117">This enables your bot to do things such as update shared resources, record RSVPs or votes, or read historical weather data.</span></span> <span data-ttu-id="ebfab-118">Al igual que una aplicación usa el almacenamiento para lograr sus objetivos, el bot puede hacerlo dentro de la conversación con el usuario.</span><span class="sxs-lookup"><span data-stu-id="ebfab-118">In the same way an app uses storage to achieve its objectives, your bot can do so within the conversation with your user.</span></span>

<!-- 
*Conversation state* pertains to the current conversation that the user is having with your bot. When the conversation ends, your bot deletes this data.

You can also store *user state* that persists after a conversation ends. For example, if you store a user's preferences, you can use that information to customize the conversation the next time you chat. For example, you might alert the user to a news article about a topic that interests her, or alert a user when an appointment becomes available. 
-->

<!-- You should generally avoid saving state using a global variable or function closures.
Doing so will create issues when you want to scale out your bot. Instead, use the conversation state and user state middleware that the BotBuilder SDK provides --> 


## <a name="types-of-underlying-storage"></a><span data-ttu-id="ebfab-119">Tipos de almacenamiento subyacente</span><span class="sxs-lookup"><span data-stu-id="ebfab-119">Types of underlying storage</span></span>

<span data-ttu-id="ebfab-120">El SDK proporciona software intermedio de administrador de estado de bot para conservar el estado del usuario y la conversación.</span><span class="sxs-lookup"><span data-stu-id="ebfab-120">The SDK provides bot state manager middleware to persist conversation and user state.</span></span> <span data-ttu-id="ebfab-121">Se puede acceder al estado mediante el contexto del bot.</span><span class="sxs-lookup"><span data-stu-id="ebfab-121">State can be accessed using the bot's context.</span></span> <span data-ttu-id="ebfab-122">Este administrador de estado puede usar Azure Table Storage, almacenamiento de archivos o almacenamiento en memoria como el almacenamiento de datos subyacente.</span><span class="sxs-lookup"><span data-stu-id="ebfab-122">This state manager can use Azure Table Storage, file storage, or memory storage as the underlying data storage.</span></span> <span data-ttu-id="ebfab-123">También puede crear sus propios componentes de almacenamiento para el bot.</span><span class="sxs-lookup"><span data-stu-id="ebfab-123">You can also create your own storage components for your bot.</span></span>

<span data-ttu-id="ebfab-124">Los bots creados con Azure Table Storage se pueden diseñar para que sean sin estado y escalables en varios nodos de proceso.</span><span class="sxs-lookup"><span data-stu-id="ebfab-124">Bots built using Azure Table Storage can be designed to be stateless and scalable across multiple compute nodes.</span></span>

> [!NOTE] 
> <span data-ttu-id="ebfab-125">El almacenamiento de archivos y en memoria no se escalará en todos los nodos.</span><span class="sxs-lookup"><span data-stu-id="ebfab-125">File and memory storage won't scale across nodes.</span></span>

## <a name="writing-directly-to-storage"></a><span data-ttu-id="ebfab-126">Escritura directa en el almacenamiento</span><span class="sxs-lookup"><span data-stu-id="ebfab-126">Writing directly to storage</span></span>

<span data-ttu-id="ebfab-127">También puede usar el SDK de Bot Builder para leer y escribir datos directamente en el almacenamiento, sin usar software intermedio ni el contexto del bot.</span><span class="sxs-lookup"><span data-stu-id="ebfab-127">You can also use the Bot Builder SDK to read and write data directly to storage, without using middleware or without using the bot context.</span></span> <span data-ttu-id="ebfab-128">Esto puede ser adecuado para los datos que usa el bot, que provienen de un origen externo al flujo de conversación del bot.</span><span class="sxs-lookup"><span data-stu-id="ebfab-128">This can be appropriate to data that your bot uses, that comes from a source outside your bot's conversation flow.</span></span>

<span data-ttu-id="ebfab-129">Por ejemplo, suponga que el bot permite al usuario solicitar el informe meteorológico y el bot lo recupera para una fecha concreta, leyéndolo de una base de datos externa.</span><span class="sxs-lookup"><span data-stu-id="ebfab-129">For example, let's say your bot allows the user to ask for the weather report, and your bot retrieves the weather report for a specified date, by reading it from an external database.</span></span> <span data-ttu-id="ebfab-130">El contenido de la base de datos meteorológicos no depende de la información del usuario ni del contexto de la conversación, por lo que se podría leer directamente desde el almacenamiento en lugar de usar el administrador de estado.</span><span class="sxs-lookup"><span data-stu-id="ebfab-130">The content of the weather database isn't dependent on user information or the conversation context, so you could just read it directly from storage instead of using the state manager.</span></span>  <span data-ttu-id="ebfab-131">Vea [Escritura directa en el almacenamiento](bot-builder-howto-v4-storage.md) para obtener un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ebfab-131">See [How to write directly to storage](bot-builder-howto-v4-storage.md) for an example.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebfab-132">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ebfab-132">Next steps</span></span>

<span data-ttu-id="ebfab-133">A continuación, se describirá la forma de procesar las actividades, en profundidad, y cómo responderlas.</span><span class="sxs-lookup"><span data-stu-id="ebfab-133">Next, lets get into how activities are processed, in depth, and how we respond to them.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ebfab-134">Procesamiento de actividades</span><span class="sxs-lookup"><span data-stu-id="ebfab-134">Activity Processing</span></span>](bot-builder-concept-activity-processing.md)

## <a name="additional-resources"></a><span data-ttu-id="ebfab-135">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ebfab-135">Additional resources</span></span>

- <span data-ttu-id="ebfab-136">[How to save state](bot-builder-howto-v4-state.md) (Cómo guardar el estado)</span><span class="sxs-lookup"><span data-stu-id="ebfab-136">[How to save state](bot-builder-howto-v4-state.md)</span></span>
- [<span data-ttu-id="ebfab-137">Escritura directa en el almacenamiento</span><span class="sxs-lookup"><span data-stu-id="ebfab-137">How to write directly to storage</span></span>](bot-builder-howto-v4-storage.md)
