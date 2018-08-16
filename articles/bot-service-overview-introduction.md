---
title: Acerca de Bot Service | Microsoft Docs
description: Obtenga información acerca de Bot Service, un servicio para compilar, conectar, probar, implementar, supervisar y administrar bots.
keywords: información general, introducción, SDK, esquema
author: Kaiqb
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/03/2018
ms.openlocfilehash: b6326ac152112ff1df01470db1f525d4bf241af4
ms.sourcegitcommit: 67445b42796d90661afc643c6bb6533e9a662cbc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39574611"
---
::: moniker range="azure-bot-service-3.0"

# <a name="azure-bot-service"></a><span data-ttu-id="afaa9-104">Azure Bot Service</span><span class="sxs-lookup"><span data-stu-id="afaa9-104">Azure Bot Service</span></span>

[!INCLUDE [pre-release-label](includes/pre-release-label-v3.md)]

<span data-ttu-id="afaa9-105">Azure Bot Service proporciona herramientas para crear, probar, implementar y administrar bots inteligentes desde un único lugar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-105">Azure Bot Service provides tools to build, test, deploy, and manage intelligent bots all in one place.</span></span> <span data-ttu-id="afaa9-106">Mediante la plataforma modular y extensible que proporciona el SDK, los desarrolladores pueden aprovechar las plantillas para crear bots que proporcionen voz, reconocimiento del lenguaje, preguntas y respuestas, etc.</span><span class="sxs-lookup"><span data-stu-id="afaa9-106">Through the modular and extensible framework provided by the SDK, developers can leverage templates to create bots that provide speech, language understanding, question and answer, and more.</span></span>  

## <a name="what-is-a-bot"></a><span data-ttu-id="afaa9-107">¿Qué es un bot?</span><span class="sxs-lookup"><span data-stu-id="afaa9-107">What is a bot?</span></span>
<span data-ttu-id="afaa9-108">Un bot es una aplicación con la que los usuarios interactúan de forma conversacional mediante texto, gráficos (tarjetas) o voz.</span><span class="sxs-lookup"><span data-stu-id="afaa9-108">A bot is an app that users interact with in a conversational way using text, graphics (cards), or speech.</span></span> <span data-ttu-id="afaa9-109">Puede ser un diálogo sencillo de pregunta y respuesta, o un bot sofisticado que permita a los usuarios interactuar con los servicios de una forma inteligente mediante la coincidencia de patrones, el seguimiento de estados y técnicas de inteligencia artificial bien integradas con los servicios empresariales existentes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-109">It may be a simple question and answer dialog, or a sophisticated bot that allows people to interact with services in an intelligent manner using pattern matching, state tracking and artificial intelligence techniques well-integrated with existing business services.</span></span> <span data-ttu-id="afaa9-110">Compruebe los [casos prácticos](https://azure.microsoft.com/services/bot-service/) de los bots.</span><span class="sxs-lookup"><span data-stu-id="afaa9-110">Check out [case studies](https://azure.microsoft.com/services/bot-service/) of bots.</span></span>  

## <a name="building-a-bot"></a><span data-ttu-id="afaa9-111">Compilación de un bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-111">Building a bot</span></span> 
<span data-ttu-id="afaa9-112">Puede decidir usar su entorno de desarrollo o sus herramientas de línea de comandos favoritas para crear el bot en C# o Node.js.</span><span class="sxs-lookup"><span data-stu-id="afaa9-112">You can choose to use your favorite development environment or command line tools to create your bot in C# or Node.js.</span></span> <span data-ttu-id="afaa9-113">Se proporcionan herramientas para las distintas etapas de desarrollo de los bots que puede usar para compilar el bot y empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-113">We provide tools for various stages of bot development that you can use to build your bot to get you started.</span></span>    

![Información general sobre el bot](media/bot-service-overview.png) 

## <a name="plan"></a><span data-ttu-id="afaa9-115">Plan</span><span class="sxs-lookup"><span data-stu-id="afaa9-115">Plan</span></span> 
<span data-ttu-id="afaa9-116">Antes de escribir código, revise las [directrices de diseño](bot-service-design-principles.md)  del bot para seguir los procedimientos recomendados e identificar las necesidades del bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-116">Before writing code, review the bot [design guidelines](bot-service-design-principles.md) for best practices and identify the needs for your bot.</span></span> <span data-ttu-id="afaa9-117">Puede crear un bot simple o incluir funcionalidades más sofisticadas como voz, reconocimiento del lenguaje, QnA o la posibilidad de extraer conocimientos a partir de distintos orígenes y proporcionar respuestas inteligentes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-117">You can create a simple bot or include more sophisticated capabilities, such as speech, language understanding, QnA, or the ability to extract knowledge from different sources and provide intelligent answers.</span></span>  

> [!TIP] 
>
> <span data-ttu-id="afaa9-118">Instale la plantilla que necesite:</span><span class="sxs-lookup"><span data-stu-id="afaa9-118">Install the template you need:</span></span>
>  - <span data-ttu-id="afaa9-119">[Plantilla VSIX](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3) de Bot Builder SDK v3 para .NET</span><span class="sxs-lookup"><span data-stu-id="afaa9-119">Bot builder .NET SDK v3 [VSIX template](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3)</span></span> 
>  - <span data-ttu-id="afaa9-120">[Plantilla Yeoman](https://www.npmjs.com/package/generator-botbuilder) de Bot Builder SDK v3 para Node.js</span><span class="sxs-lookup"><span data-stu-id="afaa9-120">Bot builder Node.js SDK v3 [Yeoman template](https://www.npmjs.com/package/generator-botbuilder)</span></span> 
>
> <span data-ttu-id="afaa9-121">Instalación de herramientas:</span><span class="sxs-lookup"><span data-stu-id="afaa9-121">Install tools:</span></span>
> - <span data-ttu-id="afaa9-122">Descargue las [herramientas de la CLI](https://github.com/Microsoft/botbuilder-tools) para crear y administrar los recursos de bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-122">Download [CLI tools](https://github.com/Microsoft/botbuilder-tools) to create and manage bot assests.</span></span> <span data-ttu-id="afaa9-123">Estas herramientas le permiten administrar el archivo de configuración del bot, la aplicación LUIS, la base de conocimiento de QnA, etc, desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="afaa9-123">These tools help you manage bot  configuration file, LUIS app, QnA knowledge base, and more from the command-line.</span></span> <span data-ttu-id="afaa9-124">Puede encontrar más información en el archivo [Léame](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-124">You can find more details in the [readme](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).</span></span>
> - <span data-ttu-id="afaa9-125">El [emulador](https://github.com/Microsoft/BotFramework-Emulator/releases) para probar el bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-125">[Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases) to test your bot</span></span>
>
> <span data-ttu-id="afaa9-126">Si es necesario, use componentes de bot como:</span><span class="sxs-lookup"><span data-stu-id="afaa9-126">If needed, use bot components, such as:</span></span>  
> - <span data-ttu-id="afaa9-127">[LUIS](https://www.luis.ai/) para agregar el reconocimiento del lenguaje a los bots</span><span class="sxs-lookup"><span data-stu-id="afaa9-127">[LUIS](https://www.luis.ai/) to add language understanding to bots</span></span>
> - <span data-ttu-id="afaa9-128">[QnA Maker](https://qnamaker.ai/) para responder a las preguntas del usuario de una manera más natural y conversacional</span><span class="sxs-lookup"><span data-stu-id="afaa9-128">[QnA Maker](https://qnamaker.ai/) to respond to user's questions in a more natural, conversational way</span></span>
> - <span data-ttu-id="afaa9-129">[Speech](https://azure.microsoft.com/services/cognitive-services/speech/) para convertir audio en texto, comprender la intención y convertir de nuevo el texto en voz</span><span class="sxs-lookup"><span data-stu-id="afaa9-129">[Speech](https://azure.microsoft.com/services/cognitive-services/speech/) to convert audio to text, understand intent, and convert text back to speech</span></span>  
> - <span data-ttu-id="afaa9-130">[Spelling](https://azure.microsoft.com/services/cognitive-services/spell-check/) para corregir errores ortográficos, reconocer la diferencia entre nombres, nombres de marca y jerga</span><span class="sxs-lookup"><span data-stu-id="afaa9-130">[Spelling](https://azure.microsoft.com/services/cognitive-services/spell-check/) to correct spelling errors, recognize the difference among names, brand names, and slang</span></span> 
> - <span data-ttu-id="afaa9-131">[Cognitive Services](bot-service-concept-intelligence.md) para otros varios componentes inteligentes</span><span class="sxs-lookup"><span data-stu-id="afaa9-131">[Cognitive Services](bot-service-concept-intelligence.md) for various other intelligent components</span></span> 


## <a name="build-your-bot"></a><span data-ttu-id="afaa9-132">Compilación del bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-132">Build your bot</span></span> 
<span data-ttu-id="afaa9-133">El bot es un servicio web que implementa una interfaz de conversación y que se comunica con Bot Service.</span><span class="sxs-lookup"><span data-stu-id="afaa9-133">Your bot is a web service that implements a conversational interface and communicates with the Bot Service.</span></span> <span data-ttu-id="afaa9-134">Puede crear esta solución en una amplia variedad de entornos y lenguajes, y le ofrecemos herramientas sencillas para empezar en Visual Studio o Yeoman, o directamente en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="afaa9-134">You can create this solution in any number of environments and languages and we offer easy getting started tools for Visual Studio or Yeoman or directly within the Azure portal.</span></span> <span data-ttu-id="afaa9-135">Mire a continuación algunas de las herramientas y servicios que puede usar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-135">Look below for some of the tools and services you can use.</span></span>

> [!TIP]
>
> <span data-ttu-id="afaa9-136">Cree un bot mediante un [SDK](~/dotnet/bot-builder-dotnet-quickstart.md), [Azure Portal](bot-service-quickstart.md) o con las [herramientas de la CLI](~/bot-builder-create-templates.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-136">Create a bot using [SDK](~/dotnet/bot-builder-dotnet-quickstart.md),  [Azure portal](bot-service-quickstart.md), or use [CLI tools](~/bot-builder-create-templates.md).</span></span>
>
> <span data-ttu-id="afaa9-137">Agregue los componentes:</span><span class="sxs-lookup"><span data-stu-id="afaa9-137">Add components:</span></span> 
> - <span data-ttu-id="afaa9-138">Agregue el modelo de reconocimiento del lenguaje denominado [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home).</span><span class="sxs-lookup"><span data-stu-id="afaa9-138">Add language understanding model [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home).</span></span> 
> - <span data-ttu-id="afaa9-139">Agregue la base de conocimiento de [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home) para responder a las preguntas del usuario.</span><span class="sxs-lookup"><span data-stu-id="afaa9-139">Add [QnA Maker](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home) knowledge base to answer questions users ask.</span></span>  
> - <span data-ttu-id="afaa9-140">Mejore la experiencia del usuario con tarjetas, voz o traducción.</span><span class="sxs-lookup"><span data-stu-id="afaa9-140">Enhace user experience with cards, speech, or translation.</span></span> 
> - <span data-ttu-id="afaa9-141">Agregue la lógica al bot mediante Bot Builder SDK.</span><span class="sxs-lookup"><span data-stu-id="afaa9-141">Add logic to your bot using the Bot Builder SDK.</span></span>   

## <a name="test-your-bot"></a><span data-ttu-id="afaa9-142">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-142">Test your bot</span></span> 
<span data-ttu-id="afaa9-143">Los bots son aplicaciones complejas con una gran cantidad de elementos diferentes que funcionan conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="afaa9-143">Bots are complex apps, with a lot of different parts working together.</span></span> <span data-ttu-id="afaa9-144">Como cualquier otra aplicación compleja, esto puede provocar algunos errores interesantes o que el bot se comporte de forma diferente a la esperada.</span><span class="sxs-lookup"><span data-stu-id="afaa9-144">Like any other complex app, this can lead to some interesting bugs or cause your bot to behave differently than expected.</span></span> <span data-ttu-id="afaa9-145">Antes de publicarlo, pruebe el bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-145">Before publishing, test your bot.</span></span>

> [!TIP] 
>
> - [<span data-ttu-id="afaa9-146">Pruebe el bot con el emulador</span><span class="sxs-lookup"><span data-stu-id="afaa9-146">Test bot with the emulator</span></span>](bot-service-debug-emulator.md)
> - [<span data-ttu-id="afaa9-147">Pruebe el bot en Chat en web</span><span class="sxs-lookup"><span data-stu-id="afaa9-147">Test bot in Web Chat</span></span>](bot-service-manage-test-webchat.md)

## <a name="publish"></a><span data-ttu-id="afaa9-148">Publicar</span><span class="sxs-lookup"><span data-stu-id="afaa9-148">Publish</span></span> 
<span data-ttu-id="afaa9-149">Cuando esté listo, publique su bot en Azure o en su propio servicio web o centro de datos.</span><span class="sxs-lookup"><span data-stu-id="afaa9-149">When you are ready, publish your bot to Azure or to your own web service or data center.</span></span> <span data-ttu-id="afaa9-150">Puede configurar la implementación continua que le permite desarrollar el bot de forma local y es útil si el bot se inserta en un control de código fuente como GitHub o Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="afaa9-150">You can set up continuous deployment that allows you to develop your bot locally and is useful if your bot is checked into a source control like GitHub or Visual Studio Team Services.</span></span> <span data-ttu-id="afaa9-151">A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="afaa9-151">As you check your changes back into your source repository, your changes will automatically be deployed to Azure.</span></span>

> [!Tip]
>
> - [<span data-ttu-id="afaa9-152">Implementación en Azure</span><span class="sxs-lookup"><span data-stu-id="afaa9-152">Deploy to Azure</span></span>](bot-service-build-continuous-deployment.md)

## <a name="connect"></a><span data-ttu-id="afaa9-153">Conectar</span><span class="sxs-lookup"><span data-stu-id="afaa9-153">Connect</span></span>          
<span data-ttu-id="afaa9-154">Conecte el bot a canales como Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, mensajes de texto o SMS, Twilio, Cortana y Skype para aumentar las interacciones y llegar a más clientes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-154">Connect your bot to channels such as Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, text/SMS, Twilio, Cortana, and Skype to increase interactions and reach more customers.</span></span>  
  
> [!TIP]
>
> - [<span data-ttu-id="afaa9-155">Elija los canales que se van a agregar</span><span class="sxs-lookup"><span data-stu-id="afaa9-155">Choose the channels to be added</span></span>](bot-service-manage-channels.md)


## <a name="evaluate"></a><span data-ttu-id="afaa9-156">Evaluate</span><span class="sxs-lookup"><span data-stu-id="afaa9-156">Evaluate</span></span> 
<span data-ttu-id="afaa9-157">Use los datos recopilados en Azure Portal para identificar oportunidades para mejorar las funcionalidades y el rendimiento de su bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-157">Use the data collected in Azure portal to identify opportunities to improve the capabilities and performance of your bot.</span></span> <span data-ttu-id="afaa9-158">Puede obtener nivel de servicio y datos de instrumentación como tráfico, latencia e integraciones.</span><span class="sxs-lookup"><span data-stu-id="afaa9-158">You can get service-level and instrumentation data like traffic, latency, and integrations.</span></span> <span data-ttu-id="afaa9-159">Analytics proporciona también informes de nivel de conversación relativos a los datos del usuario, los mensajes y los canales.</span><span class="sxs-lookup"><span data-stu-id="afaa9-159">Analytics also provides conversation-level reporting on user, message, and channel data.</span></span>

> [!Tip]
>
> - [<span data-ttu-id="afaa9-160">Recopilación de análisis</span><span class="sxs-lookup"><span data-stu-id="afaa9-160">Gather analytics</span></span>](bot-service-manage-analytics.md) 


::: moniker-end

::: moniker range="azure-bot-service-4.0"

# <a name="azure-bot-service"></a><span data-ttu-id="afaa9-161">Azure Bot Service</span><span class="sxs-lookup"><span data-stu-id="afaa9-161">Azure Bot Service</span></span>

[!INCLUDE [pre-release-label](includes/pre-release-label.md)]

<span data-ttu-id="afaa9-162">Azure Bot Service proporciona herramientas para crear, probar, implementar y administrar bots inteligentes desde un único lugar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-162">Azure Bot Service provides tools to build, test, deploy, and manage intelligent bots all in one place.</span></span> <span data-ttu-id="afaa9-163">Mediante la plataforma modular y extensible que proporciona el SDK, los desarrolladores pueden aprovechar las plantillas para crear bots que proporcionen voz, reconocimiento del lenguaje, preguntas y respuestas, etc.</span><span class="sxs-lookup"><span data-stu-id="afaa9-163">Through the modular and extensible framework provided by the SDK, developers can leverage templates to create bots that provide speech, language understanding, question and answer, and more.</span></span>  

## <a name="what-is-a-bot"></a><span data-ttu-id="afaa9-164">¿Qué es un bot?</span><span class="sxs-lookup"><span data-stu-id="afaa9-164">What is a bot?</span></span>
<span data-ttu-id="afaa9-165">Un bot es una aplicación con la que los usuarios interactúan de forma conversacional mediante texto, gráficos (tarjetas) o voz.</span><span class="sxs-lookup"><span data-stu-id="afaa9-165">A bot is an app that users interact with in a conversational way using text, graphics (cards), or speech.</span></span> <span data-ttu-id="afaa9-166">Puede ser un diálogo sencillo de pregunta y respuesta, o un bot sofisticado que permita a los usuarios interactuar con los servicios de una forma inteligente mediante la coincidencia de patrones, el seguimiento de estados y técnicas de inteligencia artificial bien integradas con los servicios empresariales existentes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-166">It may be a simple question and answer dialog, or a sophisticated bot that allows people to interact with services in an intelligent manner using pattern matching, state tracking and artificial intelligence techniques well-integrated with existing business services.</span></span> <span data-ttu-id="afaa9-167">Compruebe los [casos prácticos](https://azure.microsoft.com/services/bot-service/) de los bots.</span><span class="sxs-lookup"><span data-stu-id="afaa9-167">Check out [case studies](https://azure.microsoft.com/services/bot-service/) of bots.</span></span>  

## <a name="building-a-bot"></a><span data-ttu-id="afaa9-168">Compilación de un bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-168">Building a bot</span></span> 
<span data-ttu-id="afaa9-169">Puede decidir usar su entorno de desarrollo o sus herramientas de línea de comandos favoritas para crear el bot en C#, JavaScript, Java y Python.</span><span class="sxs-lookup"><span data-stu-id="afaa9-169">You can choose to use your favorite development environment or command line tools to create your bot in C#, JavaScript, Java, and Python.</span></span> <span data-ttu-id="afaa9-170">Se proporcionan herramientas para las distintas etapas de desarrollo de los bots que puede usar para compilar el bot y empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-170">We provide tools for various stages of bot development that you can use to build your bot to get you started.</span></span>    

![Información general sobre el bot](media/bot-service-overview.png) 

## <a name="plan"></a><span data-ttu-id="afaa9-172">Plan</span><span class="sxs-lookup"><span data-stu-id="afaa9-172">Plan</span></span> 
<span data-ttu-id="afaa9-173">Antes de escribir código, revise las [directrices de diseño](bot-service-design-principles.md)  del bot para seguir los procedimientos recomendados e identificar las necesidades del bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-173">Before writing code, review the bot [design guidelines](bot-service-design-principles.md) for best practices and identify the needs for your bot.</span></span> <span data-ttu-id="afaa9-174">Puede crear un bot simple o incluir funcionalidades más sofisticadas como voz, reconocimiento del lenguaje, QnA o la posibilidad de extraer conocimientos a partir de distintos orígenes y proporcionar respuestas inteligentes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-174">You can create a simple bot or include more sophisticated capabilities, such as speech, language understanding, QnA, or the ability to extract knowledge from different sources and provide intelligent answers.</span></span><span data-ttu-id="afaa9-175"> Para empezar a trabajar, puede usar las [herramientas de la CLI](~/bot-builder-create-templates.md), [Azure Portal](bot-service-quickstart.md) o las plantillas siguientes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-175"> To get started, you can use the [CLI tools](~/bot-builder-create-templates.md), [Azure portal](bot-service-quickstart.md), or the templates below.</span></span>

<span data-ttu-id="afaa9-176">**Elija la plantilla que necesite**</span><span class="sxs-lookup"><span data-stu-id="afaa9-176">**Grab the template you need**</span></span>

| <span data-ttu-id="afaa9-177">.NET</span><span class="sxs-lookup"><span data-stu-id="afaa9-177">.NET</span></span> | <span data-ttu-id="afaa9-178">JavaScript</span><span class="sxs-lookup"><span data-stu-id="afaa9-178">JavaScript</span></span> | 
| --- | --- | 
| [<span data-ttu-id="afaa9-179">Plantilla de VSIX</span><span class="sxs-lookup"><span data-stu-id="afaa9-179">VSIX template</span></span>](https://marketplace.visualstudio.com/items?itemName=BotBuilder.botbuilderv4) | <span data-ttu-id="afaa9-180">[Plantilla de Yeoman](https://www.npmjs.com/package/generator-botbuilder)</span><span class="sxs-lookup"><span data-stu-id="afaa9-180">[Yeoman template](https://www.npmjs.com/package/generator-botbuilder).</span></span> <span data-ttu-id="afaa9-181">Use @preview para obtener la plantilla v4.</span><span class="sxs-lookup"><span data-stu-id="afaa9-181">Use @preview to get v4 template.</span></span> |

<span data-ttu-id="afaa9-182">**Explore o instale las herramientas si es necesario**</span><span class="sxs-lookup"><span data-stu-id="afaa9-182">**Explore or install the tools, if needed**</span></span>

- <span data-ttu-id="afaa9-183">Descargue las [herramientas de la CLI](https://github.com/Microsoft/botbuilder-tools) para crear y administrar los recursos de bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-183">Download [CLI tools](https://github.com/Microsoft/botbuilder-tools) to create and manage bot assests.</span></span> <span data-ttu-id="afaa9-184">Estas herramientas le permiten administrar el archivo de configuración del bot, la aplicación LUIS, la base de conocimiento de QnA, etc, desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="afaa9-184">These tools help you manage bot  configuration file, LUIS app, QnA knowledge base, and more from the command-line.</span></span> <span data-ttu-id="afaa9-185">Puede encontrar más información en el archivo [Léame](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-185">You can find more details in the [readme](https://github.com/Microsoft/botbuilder-tools/blob/master/README.md).</span></span>
- <span data-ttu-id="afaa9-186">El [emulador](https://github.com/Microsoft/BotFramework-Emulator/releases) para probar el bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-186">[Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases) to test your bot</span></span>
- <span data-ttu-id="afaa9-187">[LUIS](https://www.luis.ai/) para agregar lenguaje natural a los bots</span><span class="sxs-lookup"><span data-stu-id="afaa9-187">[LUIS](https://www.luis.ai/) to add natural language to bots</span></span>
- <span data-ttu-id="afaa9-188">[QnA Maker](https://qnamaker.ai/) para responder a las preguntas del usuario de una manera más natural y conversacional</span><span class="sxs-lookup"><span data-stu-id="afaa9-188">[QnA Maker](https://qnamaker.ai/) to respond to user's questions in a more natural, conversational way</span></span>


## <a name="build-your-bot"></a><span data-ttu-id="afaa9-189">Compilación del bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-189">Build your bot</span></span> 
<span data-ttu-id="afaa9-190">El bot es un servicio web que implementa una interfaz de conversación y que se comunica con Bot Service.</span><span class="sxs-lookup"><span data-stu-id="afaa9-190">Your bot is a web service that implements a conversational interface and communicates with the Bot Service.</span></span> <span data-ttu-id="afaa9-191">Puede crear esta solución en una amplia variedad de entornos y lenguajes, y le ofrecemos herramientas sencillas para empezar en Visual Studio o Yeoman, o directamente en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="afaa9-191">You can create this solution in any number of environments and languages and we offer easy getting started tools for Visual Studio or Yeoman or directly within the Azure portal.</span></span> <span data-ttu-id="afaa9-192">Mire a continuación algunas de las herramientas y servicios que puede usar.</span><span class="sxs-lookup"><span data-stu-id="afaa9-192">Look below for some of the tools and services you can use.</span></span>

<span data-ttu-id="afaa9-193">Algunos de los muchos componentes disponibles son:</span><span class="sxs-lookup"><span data-stu-id="afaa9-193">Some of the many components available include:</span></span>

| <span data-ttu-id="afaa9-194">Componentes</span><span class="sxs-lookup"><span data-stu-id="afaa9-194">Components</span></span> | <span data-ttu-id="afaa9-195">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="afaa9-195">Description</span></span> |
| --- | --- |
| [<span data-ttu-id="afaa9-196">LUIS</span><span class="sxs-lookup"><span data-stu-id="afaa9-196">LUIS</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/home) | <span data-ttu-id="afaa9-197">Agregue reconocimiento del lenguaje</span><span class="sxs-lookup"><span data-stu-id="afaa9-197">Add language understanding</span></span> |
| [<span data-ttu-id="afaa9-198">QnA Maker</span><span class="sxs-lookup"><span data-stu-id="afaa9-198">QnA Maker</span></span>](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/home)  | <span data-ttu-id="afaa9-199">Agregue la base de conocimiento para responder a las preguntas del usuario</span><span class="sxs-lookup"><span data-stu-id="afaa9-199">Add a knowledge base to answer questions users ask</span></span> |
| [<span data-ttu-id="afaa9-200">Herramienta de distribución</span><span class="sxs-lookup"><span data-stu-id="afaa9-200">Dispatch tool</span></span>](~/v4sdk/bot-builder-tutorial-dispatch.md) | <span data-ttu-id="afaa9-201">Si usa más de un modelo, determine de forma inteligente cuál usar</span><span class="sxs-lookup"><span data-stu-id="afaa9-201">If using more than one model, intelligently determine when to use which one</span></span> |
| [<span data-ttu-id="afaa9-202">Medios enriquecidos</span><span class="sxs-lookup"><span data-stu-id="afaa9-202">Rich media</span></span>](v4sdk/bot-builder-howto-add-media-attachments.md) | <span data-ttu-id="afaa9-203">Mejore la experiencia del usuario con tarjetas de medios o voz</span><span class="sxs-lookup"><span data-stu-id="afaa9-203">Enhance the user experience with media cards or speech</span></span> |
| [<span data-ttu-id="afaa9-204">Voz</span><span class="sxs-lookup"><span data-stu-id="afaa9-204">Speech</span></span>](https://azure.microsoft.com/services/cognitive-services/speech/) | <span data-ttu-id="afaa9-205">Convierta audio en texto, comprenda la intención y convierta de nuevo el texto en voz</span><span class="sxs-lookup"><span data-stu-id="afaa9-205">Convert audio to text, understand intent, and convert text back to speech</span></span> |
| [<span data-ttu-id="afaa9-206">Spelling</span><span class="sxs-lookup"><span data-stu-id="afaa9-206">Spelling</span></span>](https://azure.microsoft.com/services/cognitive-services/spell-check/) | <span data-ttu-id="afaa9-207">Corrija errores ortográficos, reconozca la diferencia entre nombres, nombres de marca y jerga</span><span class="sxs-lookup"><span data-stu-id="afaa9-207">Correct spelling errors, recognize the difference among names, brand names, and slang</span></span> |

> [!NOTE]
> <span data-ttu-id="afaa9-208">La tabla anterior no es una lista completa.</span><span class="sxs-lookup"><span data-stu-id="afaa9-208">The table above is not a comprehensive list.</span></span> <span data-ttu-id="afaa9-209">Explore los artículos de la izquierda, comenzando por el [envío de mensajes](v4sdk/bot-builder-howto-send-messages.md), para obtener más funcionalidades de bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-209">Explore the articles on the left, starting with [sending messages](v4sdk/bot-builder-howto-send-messages.md), for more bot functionality.</span></span>

## <a name="test-your-bot"></a><span data-ttu-id="afaa9-210">Prueba del bot</span><span class="sxs-lookup"><span data-stu-id="afaa9-210">Test your bot</span></span> 
<span data-ttu-id="afaa9-211">Los bots son aplicaciones complejas con una gran cantidad de elementos diferentes que funcionan conjuntamente.</span><span class="sxs-lookup"><span data-stu-id="afaa9-211">Bots are complex apps, with a lot of different parts working together.</span></span> <span data-ttu-id="afaa9-212">Como cualquier otra aplicación compleja, esto puede provocar algunos errores interesantes o que el bot se comporte de forma diferente a la esperada.</span><span class="sxs-lookup"><span data-stu-id="afaa9-212">Like any other complex app, this can lead to some interesting bugs or cause your bot to behave differently than expected.</span></span> <span data-ttu-id="afaa9-213">Antes de publicarlo, pruebe el bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-213">Before publishing, test your bot.</span></span> 

<span data-ttu-id="afaa9-214">[Pruebe el bot con el emulador](bot-service-debug-emulator.md) o [Pruebe el bot en Chat en web](bot-service-manage-test-webchat.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-214">[Test bot with the emulator](bot-service-debug-emulator.md) or [Test bot in Web Chat](bot-service-manage-test-webchat.md).</span></span>

## <a name="publish"></a><span data-ttu-id="afaa9-215">Publicar</span><span class="sxs-lookup"><span data-stu-id="afaa9-215">Publish</span></span> 
<span data-ttu-id="afaa9-216">Cuando esté listo, publique su bot en Azure o en su propio servicio web o centro de datos.</span><span class="sxs-lookup"><span data-stu-id="afaa9-216">When you are ready, publish your bot to Azure or to your own web service or data center.</span></span> <span data-ttu-id="afaa9-217">Puede configurar la implementación continua que le permite desarrollar el bot de forma local y es útil si el bot se inserta en un control de código fuente como GitHub o Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="afaa9-217">You can set up continuous deployment that allows you to develop your bot locally and is useful if your bot is checked into a source control like GitHub or Visual Studio Team Services.</span></span> <span data-ttu-id="afaa9-218">A medida que inserta los cambios en el repositorio de origen, los cambios se implementarán automáticamente en Azure.</span><span class="sxs-lookup"><span data-stu-id="afaa9-218">As you check your changes back into your source repository, your changes will automatically be deployed to Azure.</span></span>

<span data-ttu-id="afaa9-219">[Implemente en Azure](bot-service-build-continuous-deployment.md) mediante la implementación continua.</span><span class="sxs-lookup"><span data-stu-id="afaa9-219">[Deploy to Azure](bot-service-build-continuous-deployment.md) via continuous deployment.</span></span>

## <a name="connect"></a><span data-ttu-id="afaa9-220">Conectar</span><span class="sxs-lookup"><span data-stu-id="afaa9-220">Connect</span></span>          
<span data-ttu-id="afaa9-221">Conecte el bot a canales como Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, mensajes de texto o SMS, Twilio, Cortana y Skype para aumentar las interacciones y llegar a más clientes.</span><span class="sxs-lookup"><span data-stu-id="afaa9-221">Connect your bot to channels such as Facebook, Messenger, Kik, Skype, Slack, Microsoft Teams, Telegram, text/SMS, Twilio, Cortana, and Skype to increase interactions and reach more customers.</span></span>

<span data-ttu-id="afaa9-222">[Elija los canales que se van a agregar](bot-service-manage-channels.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-222">[Choose the channels to be added](bot-service-manage-channels.md).</span></span>


## <a name="evaluate"></a><span data-ttu-id="afaa9-223">Evaluate</span><span class="sxs-lookup"><span data-stu-id="afaa9-223">Evaluate</span></span> 
<span data-ttu-id="afaa9-224">Use los datos recopilados en Azure Portal para identificar oportunidades para mejorar las funcionalidades y el rendimiento de su bot.</span><span class="sxs-lookup"><span data-stu-id="afaa9-224">Use the data collected in Azure portal to identify opportunities to improve the capabilities and performance of your bot.</span></span> <span data-ttu-id="afaa9-225">Puede obtener nivel de servicio y datos de instrumentación como tráfico, latencia e integraciones.</span><span class="sxs-lookup"><span data-stu-id="afaa9-225">You can get service-level and instrumentation data like traffic, latency, and integrations.</span></span> <span data-ttu-id="afaa9-226">Analytics proporciona también informes de nivel de conversación relativos a los datos del usuario, los mensajes y los canales.</span><span class="sxs-lookup"><span data-stu-id="afaa9-226">Analytics also provides conversation-level reporting on user, message, and channel data.</span></span> 

<span data-ttu-id="afaa9-227">Explore cómo [recopilar análisis](bot-service-manage-analytics.md).</span><span class="sxs-lookup"><span data-stu-id="afaa9-227">Explore how to [Gather analytics](bot-service-manage-analytics.md).</span></span>

::: moniker-end