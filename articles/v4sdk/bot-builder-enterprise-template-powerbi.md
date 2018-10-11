---
title: Análisis conversacional con PowerBI | Microsoft Docs
description: Aprenda cómo la plantilla del bot empresarial utiliza Application Insights para las conclusiones mediante PowerBI
author: darrenj
ms.author: darrenj
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/18/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 18dcaeaf2967a90ca3ecb8ff32c1ef6399d5f498
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46708721"
---
# <a name="enterprise-bot-template---conversational-analytics-using-powerbi-dashboard-and-application-insights"></a><span data-ttu-id="2498a-103">Plantillas del bot empresarial: análisis conversacional con el panel de PowerBI y Application Insights</span><span class="sxs-lookup"><span data-stu-id="2498a-103">Enterprise Bot Template - Conversational Analytics using PowerBI Dashboard and Application Insights</span></span>

> [!NOTE]
> <span data-ttu-id="2498a-104">Este tema se aplica a la versión v4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="2498a-104">This topics applies to v4 version of the SDK.</span></span> 

<span data-ttu-id="2498a-105">El bot implementado empieza a procesar mensajes y se ve el flujo de datos de telemetría a la instancia de Application Insights del grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="2498a-105">Once your Bot is deployed and it starts to process messages you will see telemetry flowing into the Application Insights instance within your Resource Group.</span></span> 

<span data-ttu-id="2498a-106">Estos datos de telemetría se ven en la hoja de Application Insights de Azure Portal y con Log Analytics.</span><span class="sxs-lookup"><span data-stu-id="2498a-106">This telemetry can be viewed within the Application Insights Blade in the Azure portal and using Log Analytics.</span></span> <span data-ttu-id="2498a-107">Además, PowerBI puede usarlos para proporcionar conclusiones empresariales más generales sobre el uso del bot.</span><span class="sxs-lookup"><span data-stu-id="2498a-107">In addition, the same telemetry can be used by PowerBI to provide more general business insights into the usage of your Bot.</span></span>

<span data-ttu-id="2498a-108">En la carpeta de PowerBI del proyecto que ha creado se proporciona un ejemplo de panel de PowerBI.</span><span class="sxs-lookup"><span data-stu-id="2498a-108">An example PowerBI dasboard is provided within the PowerBI folder of your created project.</span></span> <span data-ttu-id="2498a-109">Esto es a modo de ejemplo y muestra cómo se pueden empezar a crear conclusiones propias.</span><span class="sxs-lookup"><span data-stu-id="2498a-109">This is provided for example purposes and demonstrates how you can start to create your own insights.</span></span> <span data-ttu-id="2498a-110">Con el tiempo, mejoraremos estas presentaciones.</span><span class="sxs-lookup"><span data-stu-id="2498a-110">Over time we'll enhance these visualisations.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="2498a-111">Introducción</span><span class="sxs-lookup"><span data-stu-id="2498a-111">Getting Started</span></span>

- <span data-ttu-id="2498a-112">Descargue Power BI Desktop de [aquí](https://powerbi.microsoft.com/en-us/desktop/).</span><span class="sxs-lookup"><span data-stu-id="2498a-112">Download PowerBI Desktop from [here](https://powerbi.microsoft.com/en-us/desktop/)</span></span>
 
- <span data-ttu-id="2498a-113">Recupere el valor de ```Application Id``` para el recurso de Application Insights que usa el bot.</span><span class="sxs-lookup"><span data-stu-id="2498a-113">Retrieve an ```Application Id``` for the Application Insights resource used by your Bot.</span></span> <span data-ttu-id="2498a-114">Para ello, vaya a la página de acceso a la API en la sección de configuración de la hoja de Application Insights en Azure.</span><span class="sxs-lookup"><span data-stu-id="2498a-114">You can get this by navigating to the API Access page under the Configure Section of the Application Insights Azure Blade.</span></span>

<span data-ttu-id="2498a-115">Haga doble clic en el archivo de la plantilla de Power BI proporcionado ubicado en la carpeta de PowerBI de la solución.</span><span class="sxs-lookup"><span data-stu-id="2498a-115">Double click the provided PowerBI template file located within the PowerBI folder of your Solution.</span></span> <span data-ttu-id="2498a-116">Se le pedirá para el ```Application Id``` que recuperó en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="2498a-116">You'll be prompted for the ```Application Id``` that you retrieved in the previous step.</span></span> <span data-ttu-id="2498a-117">Complete la autenticación si se le solicita con sus credenciales de suscripción de Azure, quizá deba hacer clic en la opción de configuración de la cuenta profesional para iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="2498a-117">Complete authentication if prompted using your Azure subscription credentials, you may need to click on the Organizational Account setting to sign-in.</span></span>

<span data-ttu-id="2498a-118">El panel resultante está ahora vinculado a su instancia de Application Insights y debería ver información inicial detallada en él si se han enviado y recibido mensajes.</span><span class="sxs-lookup"><span data-stu-id="2498a-118">The resulting dashboard is now linked to your Application Insights instance and you should see initial insights within the dashboard if messages have been sent and received.</span></span>

><span data-ttu-id="2498a-119">Tenga en cuenta que la presentación de opiniones no mostrará datos porque el script de implementación actual no permite opiniones al publicar el modelo de LUIS.</span><span class="sxs-lookup"><span data-stu-id="2498a-119">Note the Sentiment visualisation will not show data as the currently deployment script doesn't enable Sentiment when publishing the LUIS model.</span></span> <span data-ttu-id="2498a-120">[Volver a publicar](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-publish-app) el modelo de LUIS y habilitar las opiniones funcionará.</span><span class="sxs-lookup"><span data-stu-id="2498a-120">If you [re-publish](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-publish-app) the LUIS model and enable Sentiment this will work.</span></span>

## <a name="middleware-processing"></a><span data-ttu-id="2498a-121">Procesamiento de middleware</span><span class="sxs-lookup"><span data-stu-id="2498a-121">Middleware Processing</span></span>

<span data-ttu-id="2498a-122">Los contenedores de datos de telemetría en torno a las clases QnAMaker y LuisRecognizer sirven para garantizar una salida de telemetría coherente independientemente del escenario y para habilitar el funcionamiento de un panel estándar en cada proyecto.</span><span class="sxs-lookup"><span data-stu-id="2498a-122">Telemetry wrappers around the QnAMaker and LuisRecognizer classes are provided to ensure consistent telemetry output regardless of scenario and enable a standard dashboard to work across each project.</span></span>

<span data-ttu-id="2498a-123">```TelemetryLuisRecognizer``` y ```TelemetryQnAMaker``` ofrecen propiedades en el constructor que permiten que los desarrolladores deshabiliten el registro de nombres de usuario y los mensajes originales.</span><span class="sxs-lookup"><span data-stu-id="2498a-123">```TelemetryLuisRecognizer``` and ```TelemetryQnAMaker``` both provide properties on the constructor enabling a developer to disable logging of usernames and original messages.</span></span> <span data-ttu-id="2498a-124">Sin embargo, esto reduce la cantidad de información disponible.</span><span class="sxs-lookup"><span data-stu-id="2498a-124">This will hwoever reduce the amount of insight available.</span></span>

## <a name="telemetry-captured"></a><span data-ttu-id="2498a-125">Datos de telemetría capturados</span><span class="sxs-lookup"><span data-stu-id="2498a-125">Telemetry Captured</span></span>

<span data-ttu-id="2498a-126">Con ```TelemetryLuisRecognizer``` y ```TelemetryQnAMaker``` se capturan 4 eventos distintos de telemetría que están habilitados de forma predeterminada en la plantilla empresarial.</span><span class="sxs-lookup"><span data-stu-id="2498a-126">4 distinct telemetry events are captured through use of  ```TelemetryLuisRecognizer``` and ```TelemetryQnAMaker``` which are enabled by default in the Enterprise template.</span></span> 

<span data-ttu-id="2498a-127">Cada intención de LUIS que use el proyecto llevará el prefijo LuisIntent</span><span class="sxs-lookup"><span data-stu-id="2498a-127">Each LUIS Intent used by your project wil be prefixed with LuisIntent.</span></span> <span data-ttu-id="2498a-128">para facilitar la identificación de las intenciones en el panel.</span><span class="sxs-lookup"><span data-stu-id="2498a-128">to enable easy identification of Intents by the dashboard.</span></span>

```
-BotMessageReceived
    - ActivityId
    - Channel
    - FromId
    - Conversationid
    - ConversationName
    - Locale
    - UserName
    - Text
```
  
```
-BotMessageSent
    - ActivityId,
    - Channel
    - RecipientId
    - Conversationid
    - ConversationName
    - Locale
    - ReceipientName
    - Text
```

```
- LuisIntent.*
    - ActivityId
    - Intent
    - IntentScore
    - SentimentLabel
    - SentimentScore
    - ConversationId
    - Question
```

```
- QnAMaker
    - ActivityId
    - ConversationId
    - OriginalQuestion
    - UserName
    - QnAItemFound
    - Question
    - Answer
    - Score
```