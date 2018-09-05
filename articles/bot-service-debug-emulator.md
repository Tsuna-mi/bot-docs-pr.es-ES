---
title: Prueba y depuración de bots con Bot Framework Emulator | Microsoft Docs
description: Obtenga información sobre cómo inspeccionar, probar y depurar bots con la aplicación de escritorio Bot Framework Emulator.
keywords: transcripción, herramienta msbot, servicios de lenguaje, reconocimiento de voz
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/30/2018
ms.openlocfilehash: 2a92281e9bbee09d8dfb00fbbc67fe6ac6b6c69b
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "42756518"
---
# <a name="debug-with-the-bot-framework-emulator"></a><span data-ttu-id="84ab6-104">Depuración con Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="84ab6-104">Debug with the Bot Framework Emulator</span></span>

<span data-ttu-id="84ab6-105">Bot Framework Emulator es una aplicación de escritorio que permite que los desarrolladores de bots prueben y depuren sus bots de manera local o remota.</span><span class="sxs-lookup"><span data-stu-id="84ab6-105">The Bot Framework Emulator is a desktop application that allows bot developers to test and debug their bots, either locally or remotely.</span></span> <span data-ttu-id="84ab6-106">Con el emulador, puede chatear con el bot e inspeccionar los mensajes que este envía y recibe.</span><span class="sxs-lookup"><span data-stu-id="84ab6-106">Using the emulator, you can chat with your bot and inspect the messages that your bot sends and receives.</span></span> <span data-ttu-id="84ab6-107">El emulador muestra los mensajes tal como aparecerían en la interfaz de usuario de un chat web y registra las respuestas y las solicitudes de JSON a medida que intercambia mensajes con su bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-107">The emulator displays messages as they would appear in a web chat UI and logs JSON requests and responses as you exchange messages with your bot.</span></span> 

> [!TIP] 
> <span data-ttu-id="84ab6-108">Antes de implementar el bot en la nube, ejecútelo localmente y pruébelo con el emulador.</span><span class="sxs-lookup"><span data-stu-id="84ab6-108">Before you deploy your bot to the cloud, run it locally and test it using the emulator.</span></span> <span data-ttu-id="84ab6-109">Puede probar el bot con el emulador incluso si todavía no lo ha [registrado](~/bot-service-quickstart-registration.md) con Bot Framework ni lo ha configurado para que se ejecute en ningún canal.</span><span class="sxs-lookup"><span data-stu-id="84ab6-109">You can test your bot using the emulator even if you have not yet [registered](~/bot-service-quickstart-registration.md) it with the Bot Framework or configured it to run on any channels.</span></span>

![UI del emulador](media/emulator-v4/emulator-welcome.png)

## <a name="download-the-bot-framework-emulator"></a><span data-ttu-id="84ab6-111">Descarga de Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="84ab6-111">Download the Bot Framework Emulator</span></span>

<span data-ttu-id="84ab6-112">Los paquetes de descarga para Mac, Windows y Linux están disponibles en la [página de versiones de GitHub](https://github.com/Microsoft/BotFramework-Emulator/releases).</span><span class="sxs-lookup"><span data-stu-id="84ab6-112">Download packages for Mac, Windows, and Linux are available via the [GitHub releases page](https://github.com/Microsoft/BotFramework-Emulator/releases).</span></span>

## <a name="connect-to-a-bot-running-on-local-host"></a><span data-ttu-id="84ab6-113">Conexión a un bot que se ejecuta en un host local</span><span class="sxs-lookup"><span data-stu-id="84ab6-113">Connect to a bot running on local host</span></span>

![Puntos de conexión del emulador](media/emulator-v4/emulator-endpoint.png)

<span data-ttu-id="84ab6-115">Para conectarse a un bot que se ejecuta localmente, seleccione la pestaña Bot Explorer (Explorador de bots) en la esquina superior izquierda.</span><span class="sxs-lookup"><span data-stu-id="84ab6-115">To connect to a bot running locally, select the Bot Explorer tab on the upper left.</span></span> <span data-ttu-id="84ab6-116">Haga clic en el icono **+** bajo la pestaña **Punto de conexión**. Aquí puede especificar el punto de conexión en el mismo puerto en que se ejecuta localmente el bot con el fin de conectarse a él.</span><span class="sxs-lookup"><span data-stu-id="84ab6-116">Click on the **+** icon under the  **Endpoint** tab. Here you can specify your endpoint to the same port your bot is running on locally in order to connect to it.</span></span> <span data-ttu-id="84ab6-117">Haga clic en **Enviar** y será le redirigirá a una ventana de chat en directo, donde podrá interactuar con el bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-117">Click **Submit**, and you will redirected to a live chat window where you can interact with your bot.</span></span>

## <a name="view-detailed-message-activity-with-the-inspector"></a><span data-ttu-id="84ab6-118">Vista detallada de la actividad de mensaje con el inspector</span><span class="sxs-lookup"><span data-stu-id="84ab6-118">View detailed Message Activity with the Inspector</span></span>

![Actividad de mensaje del emulador](media/emulator-v4/emulator-view-message-activity-02.png)

<span data-ttu-id="84ab6-120">Puede hacer clic en cualquier burbuja de mensaje dentro de la ventana de la conversación e inspeccionar la actividad JSON sin formato con la característica **INSPECTOR** que se encuentra a la derecha de la ventana.</span><span class="sxs-lookup"><span data-stu-id="84ab6-120">You can click on any message bubble within the conversation window and inspect the raw JSON activity using the **INSPECTOR** feature to the right of the window.</span></span> <span data-ttu-id="84ab6-121">Cuando se selecciona, la burbuja de mensaje cambia a color amarillo y el objeto JSON de la actividad se mostrará a la izquierda de la ventana del chat.</span><span class="sxs-lookup"><span data-stu-id="84ab6-121">When selected, the message bubble will turn yellow and the activity JSON object will be displayed to the left of the chat window.</span></span> <span data-ttu-id="84ab6-122">Esta información de JSON incluye metadatos de clave que incluyen el identificador de canal, el tipo de actividad, el identificador de la conversación, el mensaje de texto, la dirección URL del punto de conexión, etc. Puede ver las actividades de inspección enviadas desde el usuario, así como también las actividades con las que responde el bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-122">This JSON information includes key metadata including channelID, activity type, conversation id, the text message, endpoint URL, etc. You can view inspect activities sent from the user, as well as activities the bot responds with.</span></span> 

<span data-ttu-id="84ab6-123">También puede usar el [Inspector de canales](bot-service-channel-inspector.md) para obtener una vista previa de las características admitidas en canales específicos.</span><span class="sxs-lookup"><span data-stu-id="84ab6-123">You can also use the [Channel Inspector](bot-service-channel-inspector.md) to preview supported features on specific channels.</span></span>


## <a name="save-and-load-conversations-with-bot-transcripts"></a><span data-ttu-id="84ab6-124">Guardado y carga de conversaciones con transcripciones de bots</span><span class="sxs-lookup"><span data-stu-id="84ab6-124">Save and load conversations with bot transcripts</span></span>

<span data-ttu-id="84ab6-125">La actividad de mensaje en el emulador se puede guardar como transcripciones.</span><span class="sxs-lookup"><span data-stu-id="84ab6-125">Message activity in the emulator can be saved as transcripts.</span></span> <span data-ttu-id="84ab6-126">En una ventana del chat en directo abierto, puede seleccionar **Save Transcript As** (Guardar transcripción como) para asignar un nombre para el archivo de transcripción de salida y seleccionar la ubicación donde se guardará.</span><span class="sxs-lookup"><span data-stu-id="84ab6-126">From an open live chat window, you can select **Save Transcript As** to name and select the location of the output transacript file.</span></span> 

>[!TIP]
> <span data-ttu-id="84ab6-127">El botón **Start Over** (Volver a empezar) se puede usar en cualquier momento para borrar una conversación y reiniciar una conexión con el bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-127">The **Start Over** button can be used any time to clear a conversation and restart a connection to the bot.</span></span>  

![Guardar las transcripciones en el emulador](media/emulator-v4/emulator-live-chat.png)

<span data-ttu-id="84ab6-129">Para cargar las transcripciones, simplemente seleccione **Archivo** --> **Open Tranascript File** (Abrir archivo de transcripción) y seleccione la transcripción.</span><span class="sxs-lookup"><span data-stu-id="84ab6-129">To load transcripts, simply select **File** --> **Open Tranascript File** and select the transcript.</span></span> <span data-ttu-id="84ab6-130">Se abrirá una ventana de transcripción nueva que presentará la actividad de mensaje en la ventana de salida.</span><span class="sxs-lookup"><span data-stu-id="84ab6-130">A new Transcript window will open and render the message activity to the output window.</span></span> 

![Cargar transcripciones en el emulador](media/emulator-v4/emulator-load-transcript.png)

## <a name="author-transcripts-with-chatdown"></a><span data-ttu-id="84ab6-132">Creación de transcripciones con ChatDown</span><span class="sxs-lookup"><span data-stu-id="84ab6-132">Author transcripts with Chatdown</span></span>

<span data-ttu-id="84ab6-133">La herramienta [ChatDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown) es un generador de transcripciones que consume un archivo [Markdown](https://daringfireball.net/projects/markdown/syntax) para generar las transcripciones de la actividad.</span><span class="sxs-lookup"><span data-stu-id="84ab6-133">The [Chatdown](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown) tool is a transcript generator which consumes a [markdown](https://daringfireball.net/projects/markdown/syntax) file to generate activity transcripts.</span></span> <span data-ttu-id="84ab6-134">Puede crear sus propias transcripciones completamente en un formato Markdown y guardarlas como archivo **.chat** para generar las transcripciones.</span><span class="sxs-lookup"><span data-stu-id="84ab6-134">You can author your own transcripts completely in markdown format, and save them as a **.chat** file to generate transcripts.</span></span> <span data-ttu-id="84ab6-135">Esto es útil para crear rápidamente escenarios de conversación ficticios durante el desarrollo de bots.</span><span class="sxs-lookup"><span data-stu-id="84ab6-135">This is useful for quickly creating mock conversation scenarios during bot development.</span></span>  

### <a name="prerequisites"></a><span data-ttu-id="84ab6-136">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="84ab6-136">Prerequisites</span></span>

- <span data-ttu-id="84ab6-137">[Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases) versión 4 o superior</span><span class="sxs-lookup"><span data-stu-id="84ab6-137">[Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases) v.4 or greater</span></span> 
- [<span data-ttu-id="84ab6-138">Node.js</span><span class="sxs-lookup"><span data-stu-id="84ab6-138">Node.js</span></span>](https://nodejs.org/en/)
 
<span data-ttu-id="84ab6-139">ChatDown está disponible como módulo npm que requiere Node.js.</span><span class="sxs-lookup"><span data-stu-id="84ab6-139">Chatdown is available as an npm module which requires Node.js.</span></span> <span data-ttu-id="84ab6-140">Para instalar ChatDown, instálelo globalmente en la máquina.</span><span class="sxs-lookup"><span data-stu-id="84ab6-140">To install Chatdown, globally install it to your machine.</span></span> 

```
npm install -g chatdown
```
### <a name="create-and-load-transcript-transcript-files"></a><span data-ttu-id="84ab6-141">Creación y carga de archivos de transcripción</span><span class="sxs-lookup"><span data-stu-id="84ab6-141">Create and load transcript Transcript files</span></span> ###

<span data-ttu-id="84ab6-142">El siguiente es un ejemplo de cómo crear un archivo **.chat**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-142">The following is an example of a how to author a **.chat** file.</span></span> <span data-ttu-id="84ab6-143">Estos son los archivos Markdown que contienen 2 partes:</span><span class="sxs-lookup"><span data-stu-id="84ab6-143">These files are markdown which contain 2 parts:</span></span>
- <span data-ttu-id="84ab6-144">El encabezado que define los participantes de la conversación (usuario, bot)</span><span class="sxs-lookup"><span data-stu-id="84ab6-144">Header which defines the conversation participants (user, bot)</span></span>
- <span data-ttu-id="84ab6-145">La conversación de ida y vuelta entre los participantes</span><span class="sxs-lookup"><span data-stu-id="84ab6-145">The back and forth conversation between the participants</span></span>

```
user=John Doe
bot=Bot

bot: Hello!
user: hey
bot: [Typing][Delay=3000]
What can I do for you?
user: Actually nevermind, goodbye.
bot: bye!
```
<span data-ttu-id="84ab6-146">[Haga clic aquí](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown/Examples) para ver más ejemplos de los archivos .chat.</span><span class="sxs-lookup"><span data-stu-id="84ab6-146">[Click here](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown/Examples) to view more samples of .chat files.</span></span> 

<span data-ttu-id="84ab6-147">Para generar el archivo de transcripción, con el comando **chatdown** en la CLI, escriba el nombre del archivo .chat seguido de ">" y del nombre del archivo de transcripción de salida.</span><span class="sxs-lookup"><span data-stu-id="84ab6-147">To generate the transcript file, using the **chatdown** command in your CLI, enter the name of the .chat file, followed by '>' and the name of the output transcript file.</span></span> 

```
chatdown sample.chat > sample.transcript
```
## <a name="manage-bot-resources-with-the-msbot-tool"></a><span data-ttu-id="84ab6-148">Administración de los recursos de bot con la herramienta MSBot</span><span class="sxs-lookup"><span data-stu-id="84ab6-148">Manage bot resources with the MSBot tool</span></span>

<span data-ttu-id="84ab6-149">La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una sola ubicación.</span><span class="sxs-lookup"><span data-stu-id="84ab6-149">The new [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) tool allows you to create a **.bot** file, which stores metadata about different services your bot consumes, all in one location.</span></span> <span data-ttu-id="84ab6-150">Este archivo también permite que el bot se conecte a estos servicios desde la CLI. La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="84ab6-150">This file also enables your bot to connect to these services from the CLI.The tool is available as an npm module, to install it run:</span></span>

```
npm install -g msbot 
```
![Ventana de la CLI de MSBot](media/emulator-v4/msbot-cli-window.png)


<span data-ttu-id="84ab6-152">Para crear un archivo de bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="84ab6-152">To create a bot file, from your CLI enter **msbot init** followed by the name of your bot, and the target URL endpoint, for example:</span></span>

```shell
msbot init --name az-cli-bot --endpoint http://localhost:3984/api/messages
```
![Botfile](media/emulator-v4/botfile-generated.png)

><span data-ttu-id="84ab6-154">**Nota:** El bot que se usa en esta guía es un bot de eco simple que se genera a partir de la extensión de bot de la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="84ab6-154">**Note:** The bot used for this guide is a simple echo bot, generated from the Azure CLI bot extension.</span></span> <span data-ttu-id="84ab6-155">[Haga clic aquí](https://github.com/Microsoft/botbuilder-tools/tree/master/AzureCli) para más información sobre la compilación de bots con la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="84ab6-155">[Click here](https://github.com/Microsoft/botbuilder-tools/tree/master/AzureCli) to learn more about building bots with Azure CLI.</span></span> 

<span data-ttu-id="84ab6-156">Con el archivo .bot, ahora puede cargar fácilmente el bot en el emulador.</span><span class="sxs-lookup"><span data-stu-id="84ab6-156">With the .bot file, you can now easily load your bot to the emulator.</span></span> <span data-ttu-id="84ab6-157">El archivo .bot también es necesario para registrar distintos puntos de conexión y componentes de lenguaje en el bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-157">The .bot file is also required to register different endpoints and language components to your bot.</span></span> 

![Bot-File-Dropdown](media/emulator-v4/bot-file-dropdown.png)

## <a name="add-language-services"></a><span data-ttu-id="84ab6-159">Incorporación de servicios de lenguaje</span><span class="sxs-lookup"><span data-stu-id="84ab6-159">Add Language Services</span></span> 

<span data-ttu-id="84ab6-160">Puede registrar fácilmente una aplicación de LUIS o una base de conocimiento de QnA en el archivo .bot directamente desde el emulador.</span><span class="sxs-lookup"><span data-stu-id="84ab6-160">You can easily register a LUIS app or QnA knowledge base to your .bot file directly from the emulator.</span></span> <span data-ttu-id="84ab6-161">Cuando el archivo .bot se cargue, seleccione el botón de servicios en el extremo izquierdo de la ventana del emulador.</span><span class="sxs-lookup"><span data-stu-id="84ab6-161">When the .bot file is loaded, select the services button on the far left of the emulator window.</span></span> <span data-ttu-id="84ab6-162">En el menú **Servicios** verá opciones para agregar LUIS, QnA Maker, Distribución, puntos de conexión y Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="84ab6-162">You will see options under the **Services** menu to add LUIS, QnA Maker, Dispatch, endpoints, and the Azure Bot Service.</span></span> 

<span data-ttu-id="84ab6-163">Para agregar una aplicación de LUIS, simplemente haga clic en el botón **+** del menú de LUIS, escriba las credenciales de la aplicación de LUIS y haga clic en **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-163">To add a LUIS app, simply click on the **+** button on the LUIS menu, enter your LUIS app credentials, and click **Submit**.</span></span> <span data-ttu-id="84ab6-164">Con esto se registrará la aplicación de LUIS en el archivo .bot y se conectará el servicio a la aplicación de bot.</span><span class="sxs-lookup"><span data-stu-id="84ab6-164">This will register the LUIS application to the .bot file, and connect the service to your bot application.</span></span> 

![Conexión de LUIS](media/emulator-v4/emulator-connect-luis-btn.png)

<span data-ttu-id="84ab6-166">Del mismo modo, para agregar una base de conocimiento de QnA, simplemente haga clic en el botón **+** del menú de QnA, escriba las credenciales de la base de conocimiento de QnA Maker y haga clic en **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-166">Similarly, to add a QnA knowledge base, simply click on the **+** button on the QnA menu, enter your QnA Maker knowledge base credentials, and click **Submit**.</span></span> <span data-ttu-id="84ab6-167">La base de conocimiento ahora se registrará en el archivo .bot y estará lista para su uso.</span><span class="sxs-lookup"><span data-stu-id="84ab6-167">Your knowledge base will now be registered to the .bot file, and ready for use.</span></span> 

![Conexión de QnA](media/emulator-v4/emulator-connect-qna-btn.png)

<span data-ttu-id="84ab6-169">Cuando se conecta cualquiera de esos servicios, puede volver a una ventana de chat activo y comprobar que los servicios están conectados y en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="84ab6-169">When either service is connected, you can go back to a live chat window and verify that your services are connected and working.</span></span> 

![QnA conectado](media/emulator-v4/emulator-view-message-activity.png)

## <a name="inspect-language-services"></a><span data-ttu-id="84ab6-171">Inspección de los servicios de lenguaje</span><span class="sxs-lookup"><span data-stu-id="84ab6-171">Inspect Language Services</span></span>

<span data-ttu-id="84ab6-172">Con el nuevo emulador v4 también puede inspeccionar las respuestas de JSON provenientes de LUIS y QnA.</span><span class="sxs-lookup"><span data-stu-id="84ab6-172">With the new v4 emulator you can also inspect the JSON responses from LUIS and QnA.</span></span> <span data-ttu-id="84ab6-173">Mediante un bot con un servicio de lenguaje conectado, puede seleccionar **trace** en la ventana LOG (Registro) en la esquina inferior derecha.</span><span class="sxs-lookup"><span data-stu-id="84ab6-173">Using a bot with a connected language service, you can select **trace** in the LOG window to the bottom right.</span></span> <span data-ttu-id="84ab6-174">Esta herramienta nueva también proporciona características para actualizar los servicios de lenguaje directamente desde el emulador.</span><span class="sxs-lookup"><span data-stu-id="84ab6-174">This new tool also provides features to update your language services directly from the emulator.</span></span> 

![Inspector de LUIS](media/emulator-v4/emulator-luis-inspector.png)

<span data-ttu-id="84ab6-176">Con un servicio de LUIS conectado, verá que el vínculo de seguimiento especifica el **seguimiento de LUIS**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-176">With a connected LUIS service, you'll notice that the trace link specifies **Luis Trace**.</span></span> <span data-ttu-id="84ab6-177">Cuando se seleccione, verá la respuesta sin formato proveniente del servicio de LUIS, que incluye intenciones y entidades junto con sus puntuaciones especificadas.</span><span class="sxs-lookup"><span data-stu-id="84ab6-177">When selected, you'll see the raw response from your LUIS service, which includes intents, entities along with their specified scores.</span></span> <span data-ttu-id="84ab6-178">También tiene la opción de volver a asignar intenciones a las expresiones del usuario.</span><span class="sxs-lookup"><span data-stu-id="84ab6-178">You also have the option to re-assign intents for your user utterances.</span></span> 

![Inspector de QnA](media/emulator-v4/emulator-qna-inspector.png)

<span data-ttu-id="84ab6-180">Con un servicio de QnA conectado, el registro mostrará el **seguimiento de QnA** y cuando esté seleccionado, podrá obtener una vista previa del par de pregunta y respuesta asociado con esa actividad, junto con una puntuación de confianza.</span><span class="sxs-lookup"><span data-stu-id="84ab6-180">With a connected QnA service, the log will display **QnA Trace**, and when selected you can preview the question and answer pair associated with that activity, along with a confidence score.</span></span> <span data-ttu-id="84ab6-181">Desde aquí, puede agregar frases de preguntas alternativas para una respuesta.</span><span class="sxs-lookup"><span data-stu-id="84ab6-181">From here, you can add alternative question phrasing for an answer.</span></span>

[!TIP]
> <span data-ttu-id="84ab6-182">Estas características solo están disponibles para los bots de SDK de la versión 4</span><span class="sxs-lookup"><span data-stu-id="84ab6-182">These features are only available to v4 SDK bots</span></span> 


## <a name="speech-recognition"></a><span data-ttu-id="84ab6-183">Reconocimiento de voz</span><span class="sxs-lookup"><span data-stu-id="84ab6-183">Speech Recognition</span></span>
<span data-ttu-id="84ab6-184">Bot Framework Emulator admite el reconocimiento de voz a través de [Speech API de Cognitive Services](/azure/cognitive-services/Speech/home).</span><span class="sxs-lookup"><span data-stu-id="84ab6-184">The Bot Framework Emulator supports speech recognition via the [Cognitive Services Speech API](/azure/cognitive-services/Speech/home).</span></span> <span data-ttu-id="84ab6-185">Le permite emplear el bot habilitado por voz, o habilidad de Cortana, a través de la voz en el emulador durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="84ab6-185">This allows you to exercise your speech-enabled bot, or Cortana skill, via speech in the emulator during development.</span></span> <span data-ttu-id="84ab6-186">Bot Framework Emulator ofrece reconocimiento de voz sin cargo durante hasta tres horas por bot cada día.</span><span class="sxs-lookup"><span data-stu-id="84ab6-186">The Bot Framework Emulator provides speech recognition free of charge for up to three hours per bot per day.</span></span> 

## <a id="ngrok"></a> <span data-ttu-id="84ab6-187">Instalación y configuración de ngrok</span><span class="sxs-lookup"><span data-stu-id="84ab6-187">Install and configure ngrok</span></span>

<span data-ttu-id="84ab6-188">Si usa Windows y ejecuta Bot Framework Emulator detrás de un firewall o de otro límite de red y quiere conectarse a un bot hospedado remotamente, debe instalar y configurar el software de tunelización **ngrok**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-188">If you are using Windows and you are running the Bot Framework Emulator behind a firewall or other network boundary and want to connect to a bot that is hosted remotely, you must install and configure **ngrok** tunneling software.</span></span> <span data-ttu-id="84ab6-189">Bot Framework Emulator se integra estrechamente con el software de tunelización [ngrok][ngrokDownload] (desarrollado por [inconshreveable][inconshreveable]) y puede iniciarlo automáticamente cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="84ab6-189">The Bot Framework Emulator integrates tightly with [ngrok][ngrokDownload] tunnelling software (developed by [inconshreveable][inconshreveable]), and can launch it automatically when it is needed.</span></span>

<span data-ttu-id="84ab6-190">Para instalar **ngrok** en Windows y configurar el emulador para que lo use, complete estos pasos:</span><span class="sxs-lookup"><span data-stu-id="84ab6-190">To install **ngrok** on Windows and configure the emulator to use it, complete these steps:</span></span> 

1. <span data-ttu-id="84ab6-191">Descargue el ejecutable de [ngrok][ngrokDownload] en la máquina local.</span><span class="sxs-lookup"><span data-stu-id="84ab6-191">Download the [ngrok][ngrokDownload] executable to your local machine.</span></span>

2. <span data-ttu-id="84ab6-192">Abra el cuadro de diálogo Configuración de aplicación del emulador, escriba la ruta de acceso a ngrok, seleccione si omitir o no ngrok para las direcciones locales y haga clic en **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="84ab6-192">Open the emulator's App Settings dialog, enter the path to ngrok, select whether or not to bypass ngrok for local addresses, and click **Save**.</span></span>

![ruta de acceso a ngrok](media/emulator-v4/emulator-ngrok-path.png)

## <a name="additional-resources"></a><span data-ttu-id="84ab6-194">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="84ab6-194">Additional resources</span></span>

<span data-ttu-id="84ab6-195">Bot Framework Emulator es de código abierto.</span><span class="sxs-lookup"><span data-stu-id="84ab6-195">The Bot Framework Emulator is open source.</span></span> <span data-ttu-id="84ab6-196">También puede [colaborar][EmulatorGithubContribute] en el desarrollo y [enviar errores y sugerencias][EmulatorGithubBugs].</span><span class="sxs-lookup"><span data-stu-id="84ab6-196">You can [contribute][EmulatorGithubContribute] to the development and [submit bugs and suggestions][EmulatorGithubBugs].</span></span>


[EmulatorGithub]: https://github.com/Microsoft/BotFramework-Emulator
[EmulatorGithubContribute]: https://github.com/Microsoft/BotFramework-Emulator/wiki/How-to-Contribute
[EmulatorGithubBugs]: https://github.com/Microsoft/BotFramework-Emulator/wiki/Submitting-Bugs-%26-Suggestions

[ngrokDownload]: https://ngrok.com/
[inconshreveable]: https://inconshreveable.com/
[BotFrameworkDevPortal]: https://dev.botframework.com/


[EmulatorConnectPicture]: ~/media/emulator/emulator-connect_localhost_credentials.png
[EmulatorNgrokPath]: ~/media/emulator/emulator-configure_ngrok_path.png
[EmulatorNgrokMonitor]: ~/media/emulator/emulator-testbot-ngrok-monitoring.png
[EmulatorUI]: ~/media/emulator/emulator-ui-new.png

[TroubleshootingGuide]: ~/bot-service-troubleshoot-general-problems.md
[TroubleshootingAuth]: ~/bot-service-troubleshoot-authentication-problems.md
[NodeGetStarted]: ~/nodejs/bot-builder-nodejs-quickstart.md
[CSGetStarted]: ~/dotnet/bot-builder-dotnet-quickstart.md
