---
title: Conversation Designer | Microsoft Docs
description: Más información sobre Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 42e9e8137619fff2ca86f82fea1fe81aa348449b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39111564"
---
# <a name="conversation-designer"></a><span data-ttu-id="3cfc7-103">Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="3cfc7-103">Conversation Designer</span></span>
> [!IMPORTANT]
> <span data-ttu-id="3cfc7-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="3cfc7-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="3cfc7-106">Conversation Designer es una potente plataforma que proporciona un generador visual de bots, un modelo declarativo para definir estos y un runtime que facilita su creación.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-106">Conversation Designer is a powerful framework that provides a visual bot builder, a declarative model for defining bots, and a runtime that makes creating bots simple.</span></span>

<span data-ttu-id="3cfc7-107">La generación de bots conversacionales útiles requiere un enfoque unificado para la integración de controles conversacionales, reconocimiento de lenguaje, diálogos y generación de lenguaje al tiempo que se concentra en la experiencia del usuario y la integración con la lógica de negocios existente.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-107">Building useful conversational bots requires a unified approach to integrating conversational controls, language understanding, dialogues, and language generation, while at the same time maintaining focus on user experience and integration with existing business logic.</span></span> <span data-ttu-id="3cfc7-108">Conversation Designer le permite compilar bots con una ruta de acceso integrada que incluye la creación, depuración, LUIS, análisis, reconocimiento de voz y compatibilidad con los bots de Bot Framework SDK existentes.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-108">Conversation Designer enables you to build bots with an integrated path spanning authoring, debugging, LUIS, analytics, speech recognition, and compatibility with existing Bot Framework SDK bots.</span></span>

<span data-ttu-id="3cfc7-109">Mediante Conversation Designer puede compilar bots que se adapten fácilmente a las modalidades de entrada y salida disponibles y que aprovechen las ventajas de las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="3cfc7-109">Using Conversation Designer, you can build bots that adapt easily to available input/output modalities and take advantage of the following features:</span></span> 

- <span data-ttu-id="3cfc7-110">un potente runtime que minimiza la sobrecarga de la administración de estados</span><span class="sxs-lookup"><span data-stu-id="3cfc7-110">a powerful runtime that minimizes state management overhead</span></span>
- <span data-ttu-id="3cfc7-111">estados conversacionales integrados que permiten un modelado fácil y visual de una conversación</span><span class="sxs-lookup"><span data-stu-id="3cfc7-111">built-in conversational states that enable easy, visual modeling of a conversation</span></span>
- <span data-ttu-id="3cfc7-112">separación del código y la interfaz de usuario de bot</span><span class="sxs-lookup"><span data-stu-id="3cfc7-112">separation of code and bot UI</span></span>
- <span data-ttu-id="3cfc7-113">compatibilidad integrada con plataformas de inteligencia artificial como <a href="https://luis.ai" target="_blank">LUIS</a> y <a href="https://www.microsoft.com/cognitive-services/en-us/speech-api" target="_blank">reconocimiento de voz</a></span><span class="sxs-lookup"><span data-stu-id="3cfc7-113">built-in support for AI frameworks such as <a href="https://luis.ai" target="_blank">LUIS</a> and <a href="https://www.microsoft.com/cognitive-services/en-us/speech-api" target="_blank">speech recognition</a></span></span>
- <span data-ttu-id="3cfc7-114">compatibilidad integrada para la generación de lenguaje mediante plantillas de respuesta simples y condicionales</span><span class="sxs-lookup"><span data-stu-id="3cfc7-114">built-in support for language generation using simple and conditional response templates</span></span>
- <span data-ttu-id="3cfc7-115">compatibilidad con las[tarjetas adaptables](conversation-designer-adaptive-cards.md)</span><span class="sxs-lookup"><span data-stu-id="3cfc7-115">support for [Adaptive Cards](conversation-designer-adaptive-cards.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3cfc7-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3cfc7-116">Prerequisites</span></span>

- <span data-ttu-id="3cfc7-117">Conversation Designer requiere una suscripción de Azure.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-117">Conversation Designer requires an Azure subscription.</span></span> <span data-ttu-id="3cfc7-118">Empiece por <a href="https://azure.microsoft.com/en-us/" target="_blank">aquí</a></span><span class="sxs-lookup"><span data-stu-id="3cfc7-118">You can get started <a href="https://azure.microsoft.com/en-us/" target="_blank">here</a></span></span>
- <span data-ttu-id="3cfc7-119">Si aún no lo ha hecho, asegúrese de iniciar sesión en el [portal de LUIS](https://luis.ai) al menos una vez después de que haya creado una cuenta con ellos.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-119">If you haven't done so already, make sure you sign in to the [LUIS portal](https://luis.ai) at least once after you created an account with them.</span></span>
- <span data-ttu-id="3cfc7-120">Familiaridad con la programación de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-120">Familiarity with JavaScript programming.</span></span> <span data-ttu-id="3cfc7-121">Las funciones de script personalizadas se escriben en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="3cfc7-121">Custom script functions are written in JavaScript.</span></span>
- <span data-ttu-id="3cfc7-122">Microsoft Edge o Google Chrome</span><span class="sxs-lookup"><span data-stu-id="3cfc7-122">Microsoft Edge or Google Chrome</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cfc7-123">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3cfc7-123">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3cfc7-124">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="3cfc7-124">Create a bot</span></span>](conversation-designer-create-bot.md)

## <a name="additional-resources"></a><span data-ttu-id="3cfc7-125">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3cfc7-125">Additional resources</span></span>
<span data-ttu-id="3cfc7-126">Más información sobre la compilación de bots con Conversation Designer:</span><span class="sxs-lookup"><span data-stu-id="3cfc7-126">Learn more about building bots using Conversation Designer:</span></span>
- <span data-ttu-id="3cfc7-127">[Language Understanding](conversation-designer-luis.md): configuración de Language Understanding para el bot</span><span class="sxs-lookup"><span data-stu-id="3cfc7-127">[Language Understanding](conversation-designer-luis.md): Set up language understanding for your bot</span></span>
- <span data-ttu-id="3cfc7-128">[Diálogos](conversation-designer-dialogues.md): configuración de diálogos de conversación para el bot</span><span class="sxs-lookup"><span data-stu-id="3cfc7-128">[Dialogue](conversation-designer-dialogues.md): Set up Conversation dialogues for your bot</span></span>
- <span data-ttu-id="3cfc7-129">[Plantillas de respuesta](conversation-designer-response-templates.md): sugerencias rápidas sobre cómo configurar plantillas de respuesta para el bot</span><span class="sxs-lookup"><span data-stu-id="3cfc7-129">[Response templates](conversation-designer-response-templates.md): Quick tips on setting up response templates for your bot</span></span>
- <span data-ttu-id="3cfc7-130">[Tarjetas adaptables](conversation-designer-adaptive-cards.md): configuración de interacciones con objetos visuales enriquecidos para el bot mediante tarjetas adaptables</span><span class="sxs-lookup"><span data-stu-id="3cfc7-130">[Adaptive Cards](conversation-designer-adaptive-cards.md): Set up rich visual interactions for your bot using adaptive cards</span></span>
