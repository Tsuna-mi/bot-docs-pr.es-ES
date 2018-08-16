---
title: Guardar un bot de Conversation Designer | Microsoft Docs
description: Aprenda a guardar y entrenar el modelo de reconocimiento del lenguaje y a preparar el reconocimiento de voz en un bot de Conversation Designer.
author: vkannan
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ROBOTS: NoIndex, NoFollow
ms.openlocfilehash: 3a911158c379f879c0be604fb5e8ba30ab22ee44
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306221"
---
# <a name="saving-your-conversation-designer-bot"></a><span data-ttu-id="b00ca-103">Guardar el bot de Conversation Designer</span><span class="sxs-lookup"><span data-stu-id="b00ca-103">Saving your Conversation Designer bot</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b00ca-104">Conversation Designer no está disponible para todos los clientes todavía.</span><span class="sxs-lookup"><span data-stu-id="b00ca-104">Conversation Designer is not available to all customers yet.</span></span> <span data-ttu-id="b00ca-105">Habrá más información sobre la disponibilidad de Conversation Designer más adelante este año.</span><span class="sxs-lookup"><span data-stu-id="b00ca-105">More details on availability of Conversation Designer will come later this year.</span></span>

<span data-ttu-id="b00ca-106">Mientras trabaja en Conversation Designer, todo el trabajo se almacena en caché en la memoria del explorador.</span><span class="sxs-lookup"><span data-stu-id="b00ca-106">While working in Conversation Designer, all your work is cached in browser memory.</span></span> <span data-ttu-id="b00ca-107">Para confirmar los cambios, haga clic en el botón **Guardar** situado en la esquina superior izquierda del menú de navegación izquierdo.</span><span class="sxs-lookup"><span data-stu-id="b00ca-107">To commit your changes, click the **Save** button located in the upper left corner of the left navigation menu.</span></span> <span data-ttu-id="b00ca-108">Para evitar perder el trabajo, se recomienda que lo guarde con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="b00ca-108">To avoid loosing work, it is recommended that you save your work often.</span></span> <span data-ttu-id="b00ca-109">Además de hacer clic en el botón **Guardar**, también puede guardar su trabajo mediante el método abreviado de teclado **CTRL+S**.</span><span class="sxs-lookup"><span data-stu-id="b00ca-109">Besides clicking the **Save** button, you may also save your work by using the **CTRL+S** keyboard short-cut.</span></span>

## <a name="trains-luis-and-primes-speech-recognition"></a><span data-ttu-id="b00ca-110">Entrena LUIS y prepara el reconocimiento de voz</span><span class="sxs-lookup"><span data-stu-id="b00ca-110">Trains LUIS and primes speech recognition</span></span>

<span data-ttu-id="b00ca-111">Al hacer clic en el botón **Guardar** se guardarán los cambios en el bot y se llevarán a cabo algunas tareas adicionales.</span><span class="sxs-lookup"><span data-stu-id="b00ca-111">Clicking the **Save** button will save changes to the bot and performs a few additional tasks.</span></span> <span data-ttu-id="b00ca-112">A diferencia del método abreviado del teclado, si hace clic en el botón **Guardar** también indicará a Conversation Designer que debe realizar las siguientes tareas:</span><span class="sxs-lookup"><span data-stu-id="b00ca-112">Unlike the keyboard short-cut, clicking the **Save** button also instruct Conversation Designer to perform the following tasks:</span></span>

- <span data-ttu-id="b00ca-113">Entrena las nuevas intenciones y entidades de LUIS para el bot y publica el modelo de LUIS localmente en Bot Service (si es necesario).</span><span class="sxs-lookup"><span data-stu-id="b00ca-113">Trains any new LUIS intents and entities for the bot and publishes the LUIS model locally in your Bot Service (if needed).</span></span> <span data-ttu-id="b00ca-114">Estas intenciones se pueden agregar en Conversation Designer o en la aplicación de [LUIS](https://luis.ai) correspondiente del bot.</span><span class="sxs-lookup"><span data-stu-id="b00ca-114">These intents may be added in Conversation Designer or in the bot's corresponding [LUIS](https://luis.ai) app.</span></span>
- <span data-ttu-id="b00ca-115">Actualiza el entorno en tiempo de ejecución de la conversación para que use el nuevo modelo de LUIS.</span><span class="sxs-lookup"><span data-stu-id="b00ca-115">Updates the conversation runtime to use the new LUIS model.</span></span>
- <span data-ttu-id="b00ca-116">Prepara el reconocimiento de voz mediante la preparación y envío de expresiones de ejemplo a Microsoft Cognitive Services, lo cual contribuye a mejorar significativamente la precisión del reconocimiento de voz para este bot.</span><span class="sxs-lookup"><span data-stu-id="b00ca-116">Priming speech recognition by preparing and sending your example utterances to Microsoft Cognitive Services, which vastly improves speech recognition accuracy for this bot.</span></span>

## <a name="next-step"></a><span data-ttu-id="b00ca-117">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="b00ca-117">Next step</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="b00ca-118">Probar bot</span><span class="sxs-lookup"><span data-stu-id="b00ca-118">Test bot</span></span>](conversation-designer-debug-bot.md)
