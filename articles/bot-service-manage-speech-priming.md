---
title: Configuración de la preparación para la voz | Microsoft Docs
description: Obtenga información sobre cómo configurar la preparación para la voz para el servicio de bots mediante Azure Portal.
keywords: preparación para la voz, reconocimiento de voz, LUIS
author: v-royhar
ms.author: v-royhar
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 2e0b85af834bc92a9da8c9f9be2794da88c2b3bc
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304855"
---
# <a name="configure-speech-priming"></a><span data-ttu-id="b40d2-104">Configuración de la preparación para la voz</span><span class="sxs-lookup"><span data-stu-id="b40d2-104">Configure speech priming</span></span>

<span data-ttu-id="b40d2-105">La preparación para la voz mejora el reconocimiento de textos orales que se usan habitualmente en el bot.</span><span class="sxs-lookup"><span data-stu-id="b40d2-105">Speech priming improves the recognition of spoken words and phrases that are commonly used in your bot.</span></span> <span data-ttu-id="b40d2-106">Para los bots compatibles con voz que usan los canales de Cortana y Chat en web, la preparación para la voz usa ejemplos especificados en las aplicaciones de Language Understanding (LUIS) para mejorar la precisión del reconocimiento de voz de palabras importantes.</span><span class="sxs-lookup"><span data-stu-id="b40d2-106">For speech-enabled bots that use the Web Chat and Cortana channels, speech priming uses examples specified in Language Understanding (LUIS) apps to improve speech recognition accuracy for important words.</span></span>

<span data-ttu-id="b40d2-107">Es posible que el bot ya se haya integrado con una aplicación de LUIS, pero también puede crear una aplicación de LUIS para asociarla al bot de preparación para la voz.</span><span class="sxs-lookup"><span data-stu-id="b40d2-107">Your bot may already be integrated with a LUIS app, or you can choose to create a LUIS app to associate with your bot for speech priming.</span></span> <span data-ttu-id="b40d2-108">La aplicación de LUIS contiene ejemplos de lo que espera que los usuarios digan al bot.</span><span class="sxs-lookup"><span data-stu-id="b40d2-108">The LUIS app contains examples of what you expect users to say to your bot.</span></span> <span data-ttu-id="b40d2-109">Las palabras importantes que quiere que el bot reconozca deben estar etiquetadas como entidades.</span><span class="sxs-lookup"><span data-stu-id="b40d2-109">Important words that you want the bot to recognize should be labeled as entities.</span></span> <span data-ttu-id="b40d2-110">Por ejemplo, en un bot de ajedrez debe asegurarse de que, cuando el usuario diga "Mover alfil", no se interprete como "Mover marfil".</span><span class="sxs-lookup"><span data-stu-id="b40d2-110">For example, in a chess bot you want to make sure that when the user says "Move knight", it isn’t interpreted as "Move night".</span></span> <span data-ttu-id="b40d2-111">La aplicación de LUIS debe incluir ejemplos en los que "alfil" se etiquete como una entidad.</span><span class="sxs-lookup"><span data-stu-id="b40d2-111">The LUIS app should include examples in which "knight" is labeled as an entity.</span></span>

> [!NOTE]
> <span data-ttu-id="b40d2-112">Para usar la preparación para la voz con el canal Chat en web, debe usar el servicio de Bing Speech.</span><span class="sxs-lookup"><span data-stu-id="b40d2-112">To use speech priming with the Web Chat channel, you must use the Bing Speech service.</span></span> <span data-ttu-id="b40d2-113">Consulte [Enable speech in the Web Chat channel](~/bot-service-channel-connect-webchat-speech.md) (Habilitación de voz en el canal Chat en web) para obtener una explicación sobre cómo usar el servicio Bing Speech.</span><span class="sxs-lookup"><span data-stu-id="b40d2-113">See [Enable speech in the Web Chat channel](~/bot-service-channel-connect-webchat-speech.md) for an explanation of how to use the Bing Speech service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b40d2-114">La preparación para la voz solo se aplica a los bots configurados para el canal Cortana o Chat en web.</span><span class="sxs-lookup"><span data-stu-id="b40d2-114">Speech priming only applies to bots configured for the Cortana channel or the Web Chat channel.</span></span>

## <a name="change-the-list-of-luis-apps-your-bot-uses"></a><span data-ttu-id="b40d2-115">Cambio de la lista de aplicaciones de LUIS que usa el bot</span><span class="sxs-lookup"><span data-stu-id="b40d2-115">Change the list of LUIS apps your bot uses</span></span>

<span data-ttu-id="b40d2-116">Para cambiar la lista de aplicaciones de LUIS que utiliza Bing Speech con el bot, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b40d2-116">To change the list of LUIS apps used by Bing Speech with your bot, do the following:</span></span>

1. <span data-ttu-id="b40d2-117">Haga clic en **Speech priming** (Preparación para la voz) en la hoja del servicio de bots.</span><span class="sxs-lookup"><span data-stu-id="b40d2-117">Click **Speech priming** on the bot service blade.</span></span> <span data-ttu-id="b40d2-118">Aparecerá la lista de aplicaciones de LUIS disponibles.</span><span class="sxs-lookup"><span data-stu-id="b40d2-118">The list of LUIS apps available to you will appear.</span></span>
2. <span data-ttu-id="b40d2-119">Seleccione las aplicaciones de LUIS que quiera que use Bing Speech.</span><span class="sxs-lookup"><span data-stu-id="b40d2-119">Select the LUIS apps you want Bing Speech to use.</span></span>
 
    <span data-ttu-id="b40d2-120">a.</span><span class="sxs-lookup"><span data-stu-id="b40d2-120">a.</span></span> <span data-ttu-id="b40d2-121">Para seleccionar una aplicación de LUIS de la lista, mantenga el puntero sobre el modelo de LUIS hasta que aparezca una casilla de verificación y, a continuación, active la casilla.</span><span class="sxs-lookup"><span data-stu-id="b40d2-121">To select a LUIS app in the list, hover over the LUIS model until a checkbox appears, then check the checkbox.</span></span>
     
    <span data-ttu-id="b40d2-122">b.</span><span class="sxs-lookup"><span data-stu-id="b40d2-122">b.</span></span> <span data-ttu-id="b40d2-123">Para seleccionar una aplicación de LUIS que no está en la lista, desplácese hacia abajo y escriba el GUID de id. de aplicación de LUIS en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="b40d2-123">To select a LUIS app that is not on the list, scroll to the bottom and enter the LUIS Application ID GUID into the text box.</span></span>
     
3. <span data-ttu-id="b40d2-124">Haga clic en **Guardar** para guardar la lista de aplicaciones de LUIS asociadas con Bing Speech para el bot.</span><span class="sxs-lookup"><span data-stu-id="b40d2-124">Click **Save** to save the list of LUIS apps associated with Bing Speech for your bot.</span></span>

![Panel de la preparación para la voz](~/media/bot-service-manage-speech-priming/speech-priming.png)

## <a name="additional-resources"></a><span data-ttu-id="b40d2-126">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b40d2-126">Additional Resources</span></span>

- <span data-ttu-id="b40d2-127">Para obtener más información sobre la habilitación de voz en el Chat en web, consulte [Enable speech in the Web Chat channel](~/bot-service-channel-connect-webchat-speech.md) (Habilitación de voz en el canal Chat en web).</span><span class="sxs-lookup"><span data-stu-id="b40d2-127">To learn more about enabling speech in Web Chat see [Enable speech in the Web Chat channel](~/bot-service-channel-connect-webchat-speech.md).</span></span>
- <span data-ttu-id="b40d2-128">Para obtener más información acerca de la preparación para la voz, consulte [Speech Support in Bot Framework – Web Chat to Directline, to Cortana](https://blog.botframework.com/2017/06/26/Speech-To-Text/) (Compatibilidad con la voz en Bot Framework: de Chat en web a Direct Line y a Cortana).</span><span class="sxs-lookup"><span data-stu-id="b40d2-128">To learn more about speech priming, see [Speech Support in Bot Framework – Web Chat to Directline, to Cortana](https://blog.botframework.com/2017/06/26/Speech-To-Text/).</span></span>
- <span data-ttu-id="b40d2-129">Para obtener más información sobre las aplicaciones de LUIS, consulte [Language Understanding Intelligent Service](https://www.luis.ai).</span><span class="sxs-lookup"><span data-stu-id="b40d2-129">To learn more about LUIS apps, see [Language Understanding Intelligent Service](https://www.luis.ai).</span></span>
