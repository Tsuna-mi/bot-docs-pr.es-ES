---
title: Prueba y depuración de bots con Bot Framework Emulator | Microsoft Docs
description: Obtenga información sobre cómo inspeccionar, probar y depurar bots con la aplicación de escritorio Bot Framework Emulator.
keywords: transcripción, herramienta msbot, servicios de lenguaje, reconocimiento de voz
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 09/20/2018
ms.openlocfilehash: 9fb42795d789f89d0bdc74d348d50dbc0ccaa7cb
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389528"
---
# <a name="debug-with-the-emulator"></a><span data-ttu-id="99473-104">Depuración con el emulador</span><span class="sxs-lookup"><span data-stu-id="99473-104">Debug with the emulator</span></span>

<span data-ttu-id="99473-105">Bot Framework Emulator es una aplicación de escritorio que permite que los desarrolladores de bots prueben y depuren sus bots de manera local o remota.</span><span class="sxs-lookup"><span data-stu-id="99473-105">The Bot Framework Emulator is a desktop application that allows bot developers to test and debug their bots, either locally or remotely.</span></span> <span data-ttu-id="99473-106">Con el emulador, puede chatear con el bot e inspeccionar los mensajes que este envía y recibe.</span><span class="sxs-lookup"><span data-stu-id="99473-106">Using the emulator, you can chat with your bot and inspect the messages that your bot sends and receives.</span></span> <span data-ttu-id="99473-107">El emulador muestra los mensajes tal como aparecerían en la interfaz de usuario de un chat web y registra las respuestas y las solicitudes de JSON a medida que intercambia mensajes con su bot.</span><span class="sxs-lookup"><span data-stu-id="99473-107">The emulator displays messages as they would appear in a web chat UI and logs JSON requests and responses as you exchange messages with your bot.</span></span> <span data-ttu-id="99473-108">Antes de implementar el bot en la nube, ejecútelo localmente y pruébelo con el emulador.</span><span class="sxs-lookup"><span data-stu-id="99473-108">Before you deploy your bot to the cloud, run it locally and test it using the emulator.</span></span> <span data-ttu-id="99473-109">Puede probar el bot con el emulador incluso si todavía no lo ha [creado](./bot-service-quickstart.md) con Azure Bot Service ni lo ha configurado para que se ejecute en ningún canal.</span><span class="sxs-lookup"><span data-stu-id="99473-109">You can test your bot using the emulator even if you have not yet [created](./bot-service-quickstart.md) it with Azure Bot Service or configured it to run on any channels.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99473-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="99473-110">Prerequisites</span></span>
- <span data-ttu-id="99473-111">Instale el [emulador](https://github.com/Microsoft/BotFramework-Emulator/releases).</span><span class="sxs-lookup"><span data-stu-id="99473-111">Install [Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>
- <span data-ttu-id="99473-112">Instale el software de tunelización [ngrok][ngrokDownload].</span><span class="sxs-lookup"><span data-stu-id="99473-112">Install [ngrok][ngrokDownload] tunnelling software</span></span>

## <a name="connect-to-a-bot-running-on-localhost"></a><span data-ttu-id="99473-113">Una conexión a un bot que se ejecuta en un host local.</span><span class="sxs-lookup"><span data-stu-id="99473-113">Connect to a bot running on localhost</span></span>

<span data-ttu-id="99473-114">Para conectarse a un bot que se ejecuta localmente, haga clic en **Abrir bot** y seleccione el archivo .bot.</span><span class="sxs-lookup"><span data-stu-id="99473-114">To connect to a bot running locally, click **Open bot** and select the .bot file.</span></span> 

![UI del emulador](media/emulator-v4/emulator-welcome.png)

## <a name="view-detailed-message-activity-with-the-inspector"></a><span data-ttu-id="99473-116">Vista detallada de la actividad de mensaje con el inspector</span><span class="sxs-lookup"><span data-stu-id="99473-116">View detailed Message Activity with the Inspector</span></span>

<span data-ttu-id="99473-117">Envíe un mensaje al bot y este debería responder.</span><span class="sxs-lookup"><span data-stu-id="99473-117">Send message to your bot and the bot should respond back.</span></span> <span data-ttu-id="99473-118">Puede hacer clic en una burbuja de mensaje dentro de la ventana de la conversación e inspeccionar la actividad JSON sin formato con la característica **INSPECTOR** que se encuentra a la derecha de la ventana.</span><span class="sxs-lookup"><span data-stu-id="99473-118">You can click on message bubble within the conversation window and inspect the raw JSON activity using the **INSPECTOR** feature to the right of the window.</span></span> <span data-ttu-id="99473-119">Cuando se selecciona, la burbuja de mensaje cambia a color amarillo y el objeto JSON de la actividad se mostrará a la izquierda de la ventana del chat.</span><span class="sxs-lookup"><span data-stu-id="99473-119">When selected, the message bubble will turn yellow and the activity JSON object will be displayed to the left of the chat window.</span></span> <span data-ttu-id="99473-120">Esta información de JSON incluye metadatos de clave que incluyen el identificador de canal, el tipo de actividad, el identificador de la conversación, el mensaje de texto, la dirección URL del punto de conexión, etc. Puede inspeccionar las actividades de inspección enviadas desde el usuario, así como también las actividades con las que responde el bot.</span><span class="sxs-lookup"><span data-stu-id="99473-120">JSON information includes key metadata including channelID, activity type, conversation id, the text message, endpoint URL, etc. You can inspect activities sent from the user, as well as activities the bot responds with.</span></span> 

![Actividad de mensaje del emulador](media/emulator-v4/emulator-view-message-activity-02.png)

## <a name="save-and-load-conversations-with-bot-transcripts"></a><span data-ttu-id="99473-122">Guardado y carga de conversaciones con transcripciones de bots</span><span class="sxs-lookup"><span data-stu-id="99473-122">Save and load conversations with bot transcripts</span></span>

<span data-ttu-id="99473-123">Las actividades del emulador se pueden guardar como transcripciones.</span><span class="sxs-lookup"><span data-stu-id="99473-123">Activities in the emulator can be saved as transcripts.</span></span> <span data-ttu-id="99473-124">En una ventana de chat en directo abierta, seleccione **Save Transcript As** (Guardar transcripción como) en el archivo de transcripción.</span><span class="sxs-lookup"><span data-stu-id="99473-124">From an open live chat window, select **Save Transcript As** to the transacript file.</span></span> <span data-ttu-id="99473-125">El botón **Start Over** (Volver a empezar) se puede usar en cualquier momento para borrar una conversación y reiniciar una conexión con el bot.</span><span class="sxs-lookup"><span data-stu-id="99473-125">The **Start Over** button can be used any time to clear a conversation and restart a connection to the bot.</span></span>  

![Guardar las transcripciones en el emulador](media/emulator-v4/emulator-live-chat.png)

<span data-ttu-id="99473-127">Para cargar las transcripciones, simplemente seleccione **Archivo > Open Transcript File** (Abrir archivo de transcripción) y seleccione la transcripción.</span><span class="sxs-lookup"><span data-stu-id="99473-127">To load transcripts, simply select **File > Open Tranascript File** and select the transcript.</span></span> <span data-ttu-id="99473-128">Se abrirá una ventana de transcripción nueva que presentará la actividad de mensaje en la ventana de salida.</span><span class="sxs-lookup"><span data-stu-id="99473-128">A new Transcript window will open and render the message activity to the output window.</span></span> 

![Cargar transcripciones en el emulador](media/emulator-v4/emulator-load-transcript.png)

## <a name="add-services"></a><span data-ttu-id="99473-130">Agregar servicios</span><span class="sxs-lookup"><span data-stu-id="99473-130">Add services</span></span> 

<span data-ttu-id="99473-131">Puede agregar fácilmente una aplicación de LUIS o una base de conocimientos de QnA o un modelo de envío en el archivo .bot directamente desde el emulador.</span><span class="sxs-lookup"><span data-stu-id="99473-131">You can easily add a LUIS app, QnA knowledge base, or dispatch model to your .bot file directly from the emulator.</span></span> <span data-ttu-id="99473-132">Cuando el archivo .bot se cargue, seleccione el botón de servicios en el extremo izquierdo de la ventana del emulador.</span><span class="sxs-lookup"><span data-stu-id="99473-132">When the .bot file is loaded, select the services button on the far left of the emulator window.</span></span> <span data-ttu-id="99473-133">En el menú **Servicios** verá opciones para agregar LUIS, QnA Maker y Dispatch.</span><span class="sxs-lookup"><span data-stu-id="99473-133">You will see options under the **Services** menu to add LUIS, QnA Maker, and Dispatch.</span></span> 

<span data-ttu-id="99473-134">Para agregar una aplicación de servicio, solo tiene que hacer clic en el botón **+** y seleccionar el servicio que desea agregar.</span><span class="sxs-lookup"><span data-stu-id="99473-134">To add a service app, simply click on the **+** button and select the service you want to add.</span></span> <span data-ttu-id="99473-135">Se le pedirá que inicie sesión en Azure Portal para agregar el servicio al archivo de bot y conectar el servicio a la aplicación bot.</span><span class="sxs-lookup"><span data-stu-id="99473-135">You will be prompted to sign in to the Azure portal to add the service to the bot file, and connect the service to your bot application.</span></span> 

![Conexión de LUIS](media/emulator-v4/emulator-connect-luis-btn.png)

<span data-ttu-id="99473-137">Cuando se conecta cualquiera de esos servicios, puede volver a una ventana de chat activo y comprobar que los servicios están conectados y en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="99473-137">When either service is connected, you can go back to a live chat window and verify that your services are connected and working.</span></span> 

![QnA conectado](media/emulator-v4/emulator-view-message-activity.png)

## <a name="inspect-services"></a><span data-ttu-id="99473-139">Inspección de servicios</span><span class="sxs-lookup"><span data-stu-id="99473-139">Inspect services</span></span>

<span data-ttu-id="99473-140">Con el nuevo emulador v4 también puede inspeccionar las respuestas de JSON provenientes de LUIS y QnA.</span><span class="sxs-lookup"><span data-stu-id="99473-140">With the new v4 emulator you can also inspect the JSON responses from LUIS and QnA.</span></span> <span data-ttu-id="99473-141">Mediante un bot con un servicio de lenguaje conectado, puede seleccionar **trace** en la ventana LOG (Registro) en la esquina inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="99473-141">Using a bot with a connected language service, you can select **trace** in the LOG window to the bottom right.</span></span> <span data-ttu-id="99473-142">Esta herramienta nueva también proporciona características para actualizar los servicios de lenguaje directamente desde el emulador.</span><span class="sxs-lookup"><span data-stu-id="99473-142">This new tool also provides features to update your language services directly from the emulator.</span></span> 

![Inspector de LUIS](media/emulator-v4/emulator-luis-inspector.png)

<span data-ttu-id="99473-144">Con un servicio de LUIS conectado, verá que el vínculo de seguimiento especifica el **seguimiento de LUIS**.</span><span class="sxs-lookup"><span data-stu-id="99473-144">With a connected LUIS service, you'll notice that the trace link specifies **Luis Trace**.</span></span> <span data-ttu-id="99473-145">Cuando se seleccione, verá la respuesta sin formato proveniente del servicio de LUIS, que incluye intenciones y entidades junto con sus puntuaciones especificadas.</span><span class="sxs-lookup"><span data-stu-id="99473-145">When selected, you'll see the raw response from your LUIS service, which includes intents, entities along with their specified scores.</span></span> <span data-ttu-id="99473-146">También tiene la opción de volver a asignar intenciones a las expresiones del usuario.</span><span class="sxs-lookup"><span data-stu-id="99473-146">You also have the option to re-assign intents for your user utterances.</span></span> 

![Inspector de QnA](media/emulator-v4/emulator-qna-inspector.png)

<span data-ttu-id="99473-148">Con un servicio de QnA conectado, el registro mostrará el **seguimiento de QnA** y cuando esté seleccionado, podrá obtener una vista previa del par de pregunta y respuesta asociado con esa actividad, junto con una puntuación de confianza.</span><span class="sxs-lookup"><span data-stu-id="99473-148">With a connected QnA service, the log will display **QnA Trace**, and when selected you can preview the question and answer pair associated with that activity, along with a confidence score.</span></span> <span data-ttu-id="99473-149">Desde aquí, puede agregar frases de preguntas alternativas para una respuesta.</span><span class="sxs-lookup"><span data-stu-id="99473-149">From here, you can add alternative question phrasing for an answer.</span></span>

## <a name="configure-ngrok"></a><span data-ttu-id="99473-150">Configuración de ngrok</span><span class="sxs-lookup"><span data-stu-id="99473-150">Configure ngrok</span></span>

<span data-ttu-id="99473-151">Si usa Windows y ejecuta Bot Framework Emulator detrás de un firewall o de otro límite de red y quiere conectarse a un bot hospedado remotamente, debe instalar y configurar el software de tunelización **ngrok**.</span><span class="sxs-lookup"><span data-stu-id="99473-151">If you are using Windows and you are running the Bot Framework Emulator behind a firewall or other network boundary and want to connect to a bot that is hosted remotely, you must install and configure **ngrok** tunneling software.</span></span> <span data-ttu-id="99473-152">Bot Framework Emulator se integra estrechamente con el software de tunelización ngrok (desarrollado por [inconshreveable][inconshreveable]) y puede iniciarlo automáticamente cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="99473-152">The Bot Framework Emulator integrates tightly with ngrok tunnelling software (developed by [inconshreveable][inconshreveable]), and can launch it automatically when it is needed.</span></span>

<span data-ttu-id="99473-153">Abra **Configuración del emulador**, escriba la ruta de acceso a ngrok, seleccione si omitir o no ngrok para las direcciones locales y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="99473-153">Open the **Emulator Settings**, enter the path to ngrok, select whether or not to bypass ngrok for local addresses, and click **Save**.</span></span>

![ruta de acceso a ngrok](media/emulator-v4/emulator-ngrok-path.png)

## <a name="additional-resources"></a><span data-ttu-id="99473-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="99473-155">Additional resources</span></span>

<span data-ttu-id="99473-156">Bot Framework Emulator es de código abierto.</span><span class="sxs-lookup"><span data-stu-id="99473-156">The Bot Framework Emulator is open source.</span></span> <span data-ttu-id="99473-157">También puede [colaborar][EmulatorGithubContribute] en el desarrollo y [enviar errores y sugerencias][EmulatorGithubBugs].</span><span class="sxs-lookup"><span data-stu-id="99473-157">You can [contribute][EmulatorGithubContribute] to the development and [submit bugs and suggestions][EmulatorGithubBugs].</span></span>



[EmulatorGithubContribute]: https://github.com/Microsoft/BotFramework-Emulator/wiki/How-to-Contribute
[EmulatorGithubBugs]: https://github.com/Microsoft/BotFramework-Emulator/wiki/Submitting-Bugs-%26-Suggestions

[ngrokDownload]: https://ngrok.com/
[inconshreveable]: https://inconshreveable.com/
