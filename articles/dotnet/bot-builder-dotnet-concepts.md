---
title: Conceptos clave del SDK de Bot Builder para .NET | Microsoft Docs
description: Comprenda los conceptos clave y las herramientas para la compilación e implementación de los bots de conversación disponibles en el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 0a8c64cced0cb6c10daeedee4edfd777ff6d8e78
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304791"
---
# <a name="key-concepts-in-the-bot-builder-sdk-for-net"></a><span data-ttu-id="234c1-103">Conceptos clave del SDK de Bot Builder para .NET</span><span class="sxs-lookup"><span data-stu-id="234c1-103">Key concepts in the Bot Builder SDK for .NET</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-concepts.md)
> - [Node.js](../nodejs/bot-builder-nodejs-concepts.md)

<span data-ttu-id="234c1-106">En este artículo se presentan los conceptos clave del SDK de Bot Builder para .NET.</span><span class="sxs-lookup"><span data-stu-id="234c1-106">This article introduces key concepts in the Bot Builder SDK for .NET.</span></span>

## <a name="connector"></a><span data-ttu-id="234c1-107">Conector</span><span class="sxs-lookup"><span data-stu-id="234c1-107">Connector</span></span>

<span data-ttu-id="234c1-108">El [conector de Bot Framework](bot-builder-dotnet-connector.md) proporciona una única API REST que permite que un bot se comunique a través de varios canales, como Skype, correo electrónico, Slack y mucho más.</span><span class="sxs-lookup"><span data-stu-id="234c1-108">The [Bot Framework Connector](bot-builder-dotnet-connector.md) provides a single REST API that enables a bot to communicate across multiple channels such as Skype, Email, Slack, and more.</span></span> <span data-ttu-id="234c1-109">Facilita la comunicación entre el bot y el usuario mediante la retransmisión de mensajes del bot al canal y viceversa.</span><span class="sxs-lookup"><span data-stu-id="234c1-109">It facilitates communication between bot and user by relaying messages from bot to channel and from channel to bot.</span></span> 

<span data-ttu-id="234c1-110">En el SDK de Bot Builder para .NET, la biblioteca del [conector][connectorLibrary] habilita el acceso al conector.</span><span class="sxs-lookup"><span data-stu-id="234c1-110">In the Bot Builder SDK for .NET, the [Connector][connectorLibrary] library enables access to the Connector.</span></span> 

## <a name="activity"></a><span data-ttu-id="234c1-111">Actividad</span><span class="sxs-lookup"><span data-stu-id="234c1-111">Activity</span></span>

[!INCLUDE [Activity concept overview](../includes/snippet-dotnet-concept-activity.md)]

<span data-ttu-id="234c1-112">Para obtener más información acerca de las actividades del SDK de Bot Builder para .NET, consulte [Activities overview](bot-builder-dotnet-activities.md) (Introducción a las actividades).</span><span class="sxs-lookup"><span data-stu-id="234c1-112">For details about Activities in the Bot Builder SDK for .NET, see [Activities overview](bot-builder-dotnet-activities.md).</span></span>

## <a name="dialog"></a><span data-ttu-id="234c1-113">Diálogos</span><span class="sxs-lookup"><span data-stu-id="234c1-113">Dialog</span></span>

<span data-ttu-id="234c1-114">Al crear un bot mediante el SDK de Bot Builder para .NET, puede usar [diálogos](bot-builder-dotnet-dialogs.md) para modelar una conversación y administrar el [flujo de conversación](../bot-service-design-conversation-flow.md#dialog-stack).</span><span class="sxs-lookup"><span data-stu-id="234c1-114">When you create a bot using the Bot Builder SDK for .NET, you can use [dialogs](bot-builder-dotnet-dialogs.md) to model a conversation and manage [conversation flow](../bot-service-design-conversation-flow.md#dialog-stack).</span></span> <span data-ttu-id="234c1-115">Un diálogo puede estar formado por otros diálogos para maximizar la reutilización, y un contexto de diálogo mantiene la [pila de diálogos](../bot-service-design-conversation-flow.md) que están activos en la conversación en cualquier momento dado.</span><span class="sxs-lookup"><span data-stu-id="234c1-115">A dialog can be composed of other dialogs to maximize reuse, and a dialog context maintains the [stack of dialogs](../bot-service-design-conversation-flow.md) that are active in the conversation at any point in time.</span></span> <span data-ttu-id="234c1-116">Una conversación que consta de diálogos se puede transferir en varios equipos, lo que permite escalar la implementación del bot.</span><span class="sxs-lookup"><span data-stu-id="234c1-116">A conversation that comprises dialogs is portable across computers, which makes it possible for your bot implementation to scale.</span></span> 

<span data-ttu-id="234c1-117">En el SDK de Bot Builder para .NET, la biblioteca del [generador][builderLibrary] le permite administrar los diálogos.</span><span class="sxs-lookup"><span data-stu-id="234c1-117">In the Bot Builder SDK for .NET, the [Builder][builderLibrary] library enables you to manage dialogs.</span></span>

## <a name="formflow"></a><span data-ttu-id="234c1-118">FormFlow</span><span class="sxs-lookup"><span data-stu-id="234c1-118">FormFlow</span></span>

<span data-ttu-id="234c1-119">Puede usar [FormFlow](bot-builder-dotnet-formflow.md) en el SDK de Bot Builder para .NET para simplificar la creación de un bot de que recopila información del usuario.</span><span class="sxs-lookup"><span data-stu-id="234c1-119">You can use [FormFlow](bot-builder-dotnet-formflow.md) within the Bot Builder SDK for .NET to streamline of building a bot that collects information from the user.</span></span> <span data-ttu-id="234c1-120">Por ejemplo, un bot que recibe pedidos de sándwich debe recopilar cierta información del usuario, como tipo de pan, elección de ingredientes, tamaño, etc.</span><span class="sxs-lookup"><span data-stu-id="234c1-120">For example, a bot that takes sandwich orders must collect several pieces of information from the user such as type of bread, choice of toppings, size, and so on.</span></span> <span data-ttu-id="234c1-121">Dadas las instrucciones básicas, FormFlow puede generar automáticamente los diálogos necesarios para administrar una conversación guiada como esta.</span><span class="sxs-lookup"><span data-stu-id="234c1-121">Given basic guidelines, FormFlow can automatically generate the dialogs necessary to manage a guided conversation like this.</span></span>

## <a name="state"></a><span data-ttu-id="234c1-122">Estado</span><span class="sxs-lookup"><span data-stu-id="234c1-122">State</span></span>

[!INCLUDE [State concept overview](../includes/snippet-dotnet-concept-state.md)]

<span data-ttu-id="234c1-123">Para obtener más información acerca de cómo administrar el estado mediante el SDK de Bot Builder para .NET, consulte [Administración de datos de estado](bot-builder-dotnet-state.md).</span><span class="sxs-lookup"><span data-stu-id="234c1-123">For details about managing state using the Bot Builder SDK for .NET, see [Manage state data](bot-builder-dotnet-state.md).</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="234c1-124">Convenciones de nomenclatura</span><span class="sxs-lookup"><span data-stu-id="234c1-124">Naming conventions</span></span>

<span data-ttu-id="234c1-125">La biblioteca del SDK de Bot Builder para .NET usa convenciones de nomenclatura fuertemente tipada con PascalCase.</span><span class="sxs-lookup"><span data-stu-id="234c1-125">The Bot Builder SDK for .NET library uses strongly-typed, Pascal-cased naming conventions.</span></span> <span data-ttu-id="234c1-126">Sin embargo, los mensajes JSON que se transportan de un lado para otro a través del cable utilizan convenciones de nomenclatura con CamelCase.</span><span class="sxs-lookup"><span data-stu-id="234c1-126">However, the JSON messages that are transported back and forth over the wire use camel-case naming conventions.</span></span> <span data-ttu-id="234c1-127">Por ejemplo, la propiedad **ReplyToId** de C# se serializa como **replyToId** en el mensaje JSON que se transporta a través del cable.</span><span class="sxs-lookup"><span data-stu-id="234c1-127">For example, the C# property **ReplyToId** is serialized as **replyToId** in the JSON message that's transported over the wire.</span></span>

## <a name="security"></a><span data-ttu-id="234c1-128">Seguridad</span><span class="sxs-lookup"><span data-stu-id="234c1-128">Security</span></span>

<span data-ttu-id="234c1-129">Debe asegurarse de que solo el servicio del conector de Bot Framework pueda llamar al punto de conexión del bot.</span><span class="sxs-lookup"><span data-stu-id="234c1-129">You should ensure that your bot's endpoint can only be called by the Bot Framework Connector service.</span></span> <span data-ttu-id="234c1-130">Para obtener más información sobre este tema, consulte [Secure your bot](bot-builder-dotnet-security.md) (Protección del bot).</span><span class="sxs-lookup"><span data-stu-id="234c1-130">For more information on this topic, see [Secure your bot](bot-builder-dotnet-security.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="234c1-131">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="234c1-131">Next steps</span></span>

<span data-ttu-id="234c1-132">Ya conoce los conceptos sobre cada bot.</span><span class="sxs-lookup"><span data-stu-id="234c1-132">Now you know the concepts behind every bot.</span></span> <span data-ttu-id="234c1-133">Puede [crear un bot con Visual Studio](bot-builder-dotnet-quickstart.md) rápidamente con una plantilla.</span><span class="sxs-lookup"><span data-stu-id="234c1-133">You can quickly [build a bot using Visual Studio](bot-builder-dotnet-quickstart.md) using a template.</span></span> <span data-ttu-id="234c1-134">A continuación, estudie cada concepto clave con más detalle, empezando por los diálogos.</span><span class="sxs-lookup"><span data-stu-id="234c1-134">Next, study each key concept in more detail, starting with dialogs.</span></span>

> [!div class="nextstepaction"]
> [Conceptos clave del SDK de Bot Builder para .NET](bot-builder-dotnet-dialogs.md)

[connectorLibrary]: /dotnet/api/microsoft.bot.connector

[builderLibrary]: /dotnet/api/microsoft.bot.builder.dialogs
