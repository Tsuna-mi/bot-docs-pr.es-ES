---
title: Habilitar la característica de voz en un canal Chat en web | Microsoft Docs
description: Obtenga información sobre cómo habilitar la característica de voz en un control de chat en web para un bot conectado al canal Chat en web.
keywords: característica de voz, chat en web, voz, audio, micrófono
author: DeniseMak
ms.author: v-demak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: 37e18f49eb55fcb7d0bf94e96051479753eec8aa
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304953"
---
# <a name="how-to-enable-speech-in-web-chat"></a><span data-ttu-id="15e5e-104">Cómo habilitar la característica de voz en un chat en web</span><span class="sxs-lookup"><span data-stu-id="15e5e-104">How to enable speech in Web Chat</span></span>
<span data-ttu-id="15e5e-105">Puede habilitar una interfaz de voz en el control de chat en web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-105">You can enable a voice interface in the Web Chat control.</span></span> <span data-ttu-id="15e5e-106">Los usuarios interactúan con la interfaz de voz utilizando el micrófono en el control de chat en web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-106">Users interact with the voice interface by using the microphone in the Web Chat control.</span></span>

![Muestra de voz en el chat en web](~/media/bot-service-channel-webchat/webchat-sample-speech.png)

<span data-ttu-id="15e5e-108">Si el usuario escribe en lugar de decir la respuesta, Chat en web desactiva la funcionalidad de voz y el bot solo da una respuesta textual en lugar de hablar en voz alta.</span><span class="sxs-lookup"><span data-stu-id="15e5e-108">If the user types instead of speaking a response, Web Chat turns off the speech functionality and the bot gives only a textual response instead of speaking out loud.</span></span> <span data-ttu-id="15e5e-109">Para volver a habilitar la respuesta hablada, el usuario puede usar el micrófono para responder al bot la próxima vez.</span><span class="sxs-lookup"><span data-stu-id="15e5e-109">To re-enable the spoken response, the user can use the microphone to respond to the bot the next time.</span></span> <span data-ttu-id="15e5e-110">Si el micrófono está aceptando entradas, aparecerá oscurecido o rellenado.</span><span class="sxs-lookup"><span data-stu-id="15e5e-110">If the microphone is accepting input, it appears dark or filled-in.</span></span> <span data-ttu-id="15e5e-111">Si está atenuado, el usuario hace clic en él para habilitarlo.</span><span class="sxs-lookup"><span data-stu-id="15e5e-111">If it's grayed out, the user clicks on it to enable it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15e5e-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="15e5e-112">Prerequisites</span></span>

  <span data-ttu-id="15e5e-113">Antes de ejecutar la muestra, debe tener un secreto o un token de línea directa para el bot que quiera ejecutar con el control de Chat en web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-113">Before you run the sample, you need to have a Direct Line secret or token for the bot that you want to run using the Web Chat control.</span></span> 
  * <span data-ttu-id="15e5e-114">Consulte [Connect a bot to Direct Line](bot-service-channel-connect-directline.md) (Conectar un bot a la línea directa) para obtener información sobre cómo obtener un secreto de línea directa asociado al bot.</span><span class="sxs-lookup"><span data-stu-id="15e5e-114">See [Connect a bot to Direct Line](bot-service-channel-connect-directline.md) for information on getting a Direct Line secret associated with your bot.</span></span>
  * <span data-ttu-id="15e5e-115">Consulte [Generate a Direct Line token](rest-api/bot-framework-rest-direct-line-3-0-authentication.md) (Generar un token de línea directa) para obtener información sobre cómo intercambiar el secreto de un token.</span><span class="sxs-lookup"><span data-stu-id="15e5e-115">See [Generate a Direct Line token](rest-api/bot-framework-rest-direct-line-3-0-authentication.md) for information on exchanging the secret for a token.</span></span>

## <a name="customizing-web-chat-for-speech"></a><span data-ttu-id="15e5e-116">Personalizar el chat en web para la característica de voz</span><span class="sxs-lookup"><span data-stu-id="15e5e-116">Customizing Web Chat for speech</span></span>
<span data-ttu-id="15e5e-117">Para habilitar la funcionalidad de voz en Chat en web, debe personalizar el código de JavaScript que invoca el control de Chat en web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-117">To enable the speech functionality in Web Chat, you need to customize the JavaScript code that invokes the Web Chat control.</span></span> <span data-ttu-id="15e5e-118">Puede probar el Chat en web habilitado para la característica de voz de forma local, mediante los siguientes pasos.</span><span class="sxs-lookup"><span data-stu-id="15e5e-118">You can try out voice-enabled Web Chat locally using the following steps.</span></span>

1. <span data-ttu-id="15e5e-119">Descargue el [ejemplo index.html](https://aka.ms/web-chat-speech-sample).</span><span class="sxs-lookup"><span data-stu-id="15e5e-119">Download the [sample index.html](https://aka.ms/web-chat-speech-sample).</span></span> <!-- this aka.ms link needs to be updated if the sample location changes -->
2. <span data-ttu-id="15e5e-120">Edite el código en `index.html` según el tipo de soporte de voz que desee agregar.</span><span class="sxs-lookup"><span data-stu-id="15e5e-120">Edit the code in `index.html` according to the type of speech support you want to add.</span></span> <span data-ttu-id="15e5e-121">Los tipos de implementaciones de voz se describen en [Habilitar servicios de voz](#enable-speech-services).</span><span class="sxs-lookup"><span data-stu-id="15e5e-121">The types of speech implementations are described in [Enable speech services](#enable-speech-services).</span></span> 
3. <span data-ttu-id="15e5e-122">Inicie un servidor web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-122">Start a web server.</span></span> <span data-ttu-id="15e5e-123">Una forma de hacerlo es usar `npm http-server` en el símbolo del sistema Node.js.</span><span class="sxs-lookup"><span data-stu-id="15e5e-123">One way to do so is to use `npm http-server` at a Node.js command prompt.</span></span>

   * <span data-ttu-id="15e5e-124">Para instalar `http-server` de forma global para que pueda ejecutarse desde la línea de comandos, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="15e5e-124">To install `http-server` globally so it can be run from the command line, run this command:</span></span>

     ```
     npm install http-server -g
     ```

   * <span data-ttu-id="15e5e-125">Para iniciar un servidor web usando el puerto 8000, desde el directorio que contiene `index.html`, ejecute este comando:</span><span class="sxs-lookup"><span data-stu-id="15e5e-125">To start a web server using port 8000, from the directory that contains `index.html`, run this command:</span></span>

     ```
     http-server -p 8000
     ```
4. <span data-ttu-id="15e5e-126">Dirija el explorador a `http://localhost:8000/samples?parameters`.</span><span class="sxs-lookup"><span data-stu-id="15e5e-126">Aim your browser at `http://localhost:8000/samples?parameters`.</span></span> <span data-ttu-id="15e5e-127">Por ejemplo, `http://localhost:8000/samples?s=YOURDIRECTLINESECRET` invoca el bot con un secreto de línea directa.</span><span class="sxs-lookup"><span data-stu-id="15e5e-127">For example, `http://localhost:8000/samples?s=YOURDIRECTLINESECRET` invokes the bot using a Direct Line secret.</span></span> <span data-ttu-id="15e5e-128">Los parámetros que se pueden establecer en la cadena de consulta se describen en la tabla siguiente:</span><span class="sxs-lookup"><span data-stu-id="15e5e-128">The parameters that can be set in the query string are described in the following table:</span></span>

   | <span data-ttu-id="15e5e-129">Parámetro</span><span class="sxs-lookup"><span data-stu-id="15e5e-129">Parameter</span></span> | <span data-ttu-id="15e5e-130">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="15e5e-130">Description</span></span> |
   |-----------|-------------|
   | <span data-ttu-id="15e5e-131">s</span><span class="sxs-lookup"><span data-stu-id="15e5e-131">s</span></span> | <span data-ttu-id="15e5e-132">Secreto de línea directa.</span><span class="sxs-lookup"><span data-stu-id="15e5e-132">Direct Line secret.</span></span> <span data-ttu-id="15e5e-133">Consulte [Connect a bot to Direct Line](bot-service-channel-connect-directline.md) (Conectar un bot a la línea directa) para obtener información sobre cómo obtener un secreto de línea directa.</span><span class="sxs-lookup"><span data-stu-id="15e5e-133">See [Connect a bot to Direct Line](bot-service-channel-connect-directline.md) for information on getting a Direct Line secret.</span></span> |
   | <span data-ttu-id="15e5e-134">t</span><span class="sxs-lookup"><span data-stu-id="15e5e-134">t</span></span> | <span data-ttu-id="15e5e-135">Token de línea directa.</span><span class="sxs-lookup"><span data-stu-id="15e5e-135">Direct Line token.</span></span> <span data-ttu-id="15e5e-136">Consulte [Generate a Direct Line token](rest-api/bot-framework-rest-direct-line-3-0-authentication.md) (Crear un token de línea directa) para obtener información sobre cómo generar este token.</span><span class="sxs-lookup"><span data-stu-id="15e5e-136">See [Generate a Direct Line token](rest-api/bot-framework-rest-direct-line-3-0-authentication.md) for info on how to generate this token.</span></span> |
   | <span data-ttu-id="15e5e-137">dominio</span><span class="sxs-lookup"><span data-stu-id="15e5e-137">domain</span></span> | <span data-ttu-id="15e5e-138">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-138">Optional.</span></span> <span data-ttu-id="15e5e-139">Dirección URL de un punto de conexión de línea directa alternativo.</span><span class="sxs-lookup"><span data-stu-id="15e5e-139">The URL of an alternate Direct Line endpoint.</span></span>  |
   | <span data-ttu-id="15e5e-140">WebSocket</span><span class="sxs-lookup"><span data-stu-id="15e5e-140">webSocket</span></span> | <span data-ttu-id="15e5e-141">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-141">Optional.</span></span> <span data-ttu-id="15e5e-142">Establecer en "true" para usar WebSocket para recibir mensajes.</span><span class="sxs-lookup"><span data-stu-id="15e5e-142">Set to 'true' to use WebSocket to receive messages.</span></span> <span data-ttu-id="15e5e-143">El valor predeterminado es `false`.</span><span class="sxs-lookup"><span data-stu-id="15e5e-143">Default is `false`.</span></span> |
   | <span data-ttu-id="15e5e-144">userid</span><span class="sxs-lookup"><span data-stu-id="15e5e-144">userid</span></span> | <span data-ttu-id="15e5e-145">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-145">Optional.</span></span> <span data-ttu-id="15e5e-146">Id. del usuario del bot.</span><span class="sxs-lookup"><span data-stu-id="15e5e-146">The ID of the bot user.</span></span>  |
   | <span data-ttu-id="15e5e-147">nombre de usuario</span><span class="sxs-lookup"><span data-stu-id="15e5e-147">username</span></span> | <span data-ttu-id="15e5e-148">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-148">Optional.</span></span> <span data-ttu-id="15e5e-149">Nombre de usuario del usuario del bot.</span><span class="sxs-lookup"><span data-stu-id="15e5e-149">The user name of the bot's user.</span></span>  |
   | <span data-ttu-id="15e5e-150">botid</span><span class="sxs-lookup"><span data-stu-id="15e5e-150">botid</span></span> | <span data-ttu-id="15e5e-151">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-151">Optional.</span></span> <span data-ttu-id="15e5e-152">Id. del bot.</span><span class="sxs-lookup"><span data-stu-id="15e5e-152">ID of the bot.</span></span> |
   | <span data-ttu-id="15e5e-153">botname</span><span class="sxs-lookup"><span data-stu-id="15e5e-153">botname</span></span> | <span data-ttu-id="15e5e-154">Opcional.</span><span class="sxs-lookup"><span data-stu-id="15e5e-154">Optional.</span></span> <span data-ttu-id="15e5e-155">Nombre del bot.</span><span class="sxs-lookup"><span data-stu-id="15e5e-155">Name of the bot.</span></span> |


## <a name="enable-speech-services"></a><span data-ttu-id="15e5e-156">Habilitar servicios de voz</span><span class="sxs-lookup"><span data-stu-id="15e5e-156">Enable speech services</span></span>
<span data-ttu-id="15e5e-157">La personalización le permite agregar la funcionalidad de voz de cualquiera de las maneras siguientes:</span><span class="sxs-lookup"><span data-stu-id="15e5e-157">The customization allows you to add speech functionality in any of the following ways:</span></span>

* <span data-ttu-id="15e5e-158">**Característica de voz que proporciona el explorador**: utilice la funcionalidad de voz integrada en el explorador web.</span><span class="sxs-lookup"><span data-stu-id="15e5e-158">**Browser-provided speech** - Use speech functionality built into the web browser.</span></span> <span data-ttu-id="15e5e-159">En este momento, esta funcionalidad solo está disponible en el explorador Chrome.</span><span class="sxs-lookup"><span data-stu-id="15e5e-159">At this time, this functionality is only available on the Chrome browser.</span></span>
* <span data-ttu-id="15e5e-160">**Usar el servicio Bing Speech**: puede usar el servicio Bing Speech para proporcionar reconocimiento y síntesis de voz.</span><span class="sxs-lookup"><span data-stu-id="15e5e-160">**Use Bing Speech service** - You can use the Bing Speech service to provide speech recognition and synthesis.</span></span> <span data-ttu-id="15e5e-161">Esta forma de acceso a la funcionalidad de voz es compatible con varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="15e5e-161">This way of access speech functionality is supported by a variety of browsers.</span></span> <span data-ttu-id="15e5e-162">En este caso, el procesamiento se realiza en un servidor en lugar de hacerse en el explorador.</span><span class="sxs-lookup"><span data-stu-id="15e5e-162">In this case, the processing is done on a server instead of on the browser.</span></span>
* <span data-ttu-id="15e5e-163">**Crear un servicio de voz personalizado**: puede crear sus propios componentes personalizados de reconocimiento y síntesis de voz.</span><span class="sxs-lookup"><span data-stu-id="15e5e-163">**Create a custom speech service** - You can create your own custom speech recognition and voice synthesis components.</span></span>

### <a name="browser-provided-speech"></a><span data-ttu-id="15e5e-164">Característica de voz que proporciona el explorador</span><span class="sxs-lookup"><span data-stu-id="15e5e-164">Browser-provided speech</span></span>

<span data-ttu-id="15e5e-165">El siguiente código crea una instancia del reconocedor de voz y los componentes de síntesis de voz que vienen con el explorador.</span><span class="sxs-lookup"><span data-stu-id="15e5e-165">The following code instantiates speech recognizer and speech synthesis components that come with the browser.</span></span> <span data-ttu-id="15e5e-166">Este método de agregar la funcionalidad de voz no es compatible con todos los exploradores.</span><span class="sxs-lookup"><span data-stu-id="15e5e-166">This method of adding speech is not supported by all browsers.</span></span> 

> [!NOTE] 
> <span data-ttu-id="15e5e-167">Google Chrome admite el reconocedor de voz del explorador.</span><span class="sxs-lookup"><span data-stu-id="15e5e-167">Google Chrome supports the browser speech recognizer.</span></span> <span data-ttu-id="15e5e-168">Sin embargo, Chrome puede bloquear el micrófono en los siguientes casos:</span><span class="sxs-lookup"><span data-stu-id="15e5e-168">However, Chrome may block the microphone in the following cases:</span></span>
> * <span data-ttu-id="15e5e-169">Si la dirección URL de la página que contiene el Chat en web comienza con `http://` en lugar de `https://`.</span><span class="sxs-lookup"><span data-stu-id="15e5e-169">If the URL of the page that contains Web Chat begins with `http://` instead of `https://`.</span></span>
> * <span data-ttu-id="15e5e-170">Si la dirección URL es un archivo local que usa el protocolo `file://` en lugar de `http://localhost:8000`.</span><span class="sxs-lookup"><span data-stu-id="15e5e-170">If the URL is a local file using the `file://` protocol instead of `http://localhost:8000`.</span></span>

[!code-js[Specify speech options to use in-browser speech (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#BrowserSpeech)]

### <a name="bing-speech-service"></a><span data-ttu-id="15e5e-171">Servicio Bing Speech</span><span class="sxs-lookup"><span data-stu-id="15e5e-171">Bing Speech service</span></span>

<span data-ttu-id="15e5e-172">El siguiente código crea una instancia del reconocedor de voz y los componentes de síntesis de voz que usan el servicio Bing Speech.</span><span class="sxs-lookup"><span data-stu-id="15e5e-172">The following code instantiates speech recognizer and speech synthesis components that use the Bing Speech service.</span></span> <span data-ttu-id="15e5e-173">El reconocimiento y la generación de voz se llevan a cabo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="15e5e-173">The recognition and generation of speech is performed on the server.</span></span> <span data-ttu-id="15e5e-174">Este mecanismo se admite en varios exploradores.</span><span class="sxs-lookup"><span data-stu-id="15e5e-174">This mechanism is supported in multiple browsers.</span></span> 

> [!TIP]
> <span data-ttu-id="15e5e-175">Puede usar el desbloqueo del reconocimiento de voz para mejorar la precisión del reconocimiento de voz del bot si usa el servicio Bing Speech.</span><span class="sxs-lookup"><span data-stu-id="15e5e-175">You can use speech recognition priming to improve your bot's speech recognition accuracy if you use the Bing Speech service.</span></span> <span data-ttu-id="15e5e-176">Para obtener más información, consulte la entrada de blog [Speech Support in Bot Framework](https://blog.botframework.com/2017/06/26/Speech-To-Text) (compatibilidad con voz en Bot Framework).</span><span class="sxs-lookup"><span data-stu-id="15e5e-176">For more information, check out the [Speech Support in Bot Framework](https://blog.botframework.com/2017/06/26/Speech-To-Text) blog post.</span></span>

[!code-js[Specify speech options to use the Bing Speech API (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#BingSpeech)]

#### <a name="use-the-bing-speech-service-with-a-token"></a><span data-ttu-id="15e5e-177">Usar el servicio de Bing Speech con un token</span><span class="sxs-lookup"><span data-stu-id="15e5e-177">Use the Bing Speech service with a token</span></span>

<span data-ttu-id="15e5e-178">También tiene la opción de habilitar el reconocimiento de voz de Cognitive Services con un token.</span><span class="sxs-lookup"><span data-stu-id="15e5e-178">You also have the option to enable Cognitive Services speech recognition using a token.</span></span> <span data-ttu-id="15e5e-179">El token se genera en un back-end seguro mediante la clave de API.</span><span class="sxs-lookup"><span data-stu-id="15e5e-179">The token is generated in a secure back end using your API key.</span></span>

<span data-ttu-id="15e5e-180">En el ejemplo de código siguiente se muestra cómo se realiza la operación de captura tokens desde un back-end seguro para evitar la exposición de la clave de API.</span><span class="sxs-lookup"><span data-stu-id="15e5e-180">The following example code shows how the token fetch is done from a secure back end to avoid exposing the API key.</span></span>

[!code-js[Fetch a token to use with the Bing Speech API (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#FetchToken)]

### <a name="custom-speech-service"></a><span data-ttu-id="15e5e-181">Custom Speech Service</span><span class="sxs-lookup"><span data-stu-id="15e5e-181">Custom Speech service</span></span>

<span data-ttu-id="15e5e-182">También puede proporcionar su propio reconocimiento de voz personalizado que implementa ISpeechRecognizer, o la síntesis de voz que implementa ISpeechSynthesis.</span><span class="sxs-lookup"><span data-stu-id="15e5e-182">You can also provide your own custom speech recognition that implements ISpeechRecognizer or speech synthesis that implements ISpeechSynthesis.</span></span> 

[!code-js[Fetch a token to use with a custom speech service (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#CustomSpeechService)]

### <a name="pass-the-speech-options-to-web-chat"></a><span data-ttu-id="15e5e-183">Pasar las opciones de voz al Chat en web</span><span class="sxs-lookup"><span data-stu-id="15e5e-183">Pass the speech options to Web Chat</span></span>

<span data-ttu-id="15e5e-184">El código siguiente pasa las opciones de voz al control de Chat en web:</span><span class="sxs-lookup"><span data-stu-id="15e5e-184">The following code passes the speech options to the Web Chat control:</span></span>

[!code-js[Pass speech options to Web Chat (JavaScript)](./includes/code/bot-service-channel-connect-webchat-speech.js#PassSpeechOptionsToWebChat)]

## <a name="next-steps"></a><span data-ttu-id="15e5e-185">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="15e5e-185">Next steps</span></span>
<span data-ttu-id="15e5e-186">Ahora que puede habilitar la interacción de voz con el Chat en web, puede ver el método que usa el bot para crear los mensajes de voz y ajustar el estado del micrófono:</span><span class="sxs-lookup"><span data-stu-id="15e5e-186">Now that you can enable voice interaction with Web Chat, learn how your bot constructs spoken messages and adjusts the state of the microphone:</span></span>
* [<span data-ttu-id="15e5e-187">Agregar voz a los mensajes (C#)</span><span class="sxs-lookup"><span data-stu-id="15e5e-187">Add speech to messages (C#)</span></span>](dotnet/bot-builder-dotnet-text-to-speech.md)
* [<span data-ttu-id="15e5e-188">Agregar voz a los mensajes (Node.js)</span><span class="sxs-lookup"><span data-stu-id="15e5e-188">Add speech to messages (Node.js)</span></span>](nodejs/bot-builder-nodejs-text-to-speech.md)

## <a name="additional-resources"></a><span data-ttu-id="15e5e-189">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="15e5e-189">Additional resources</span></span>

* <span data-ttu-id="15e5e-190">También puede [descargar el código fuente](https://github.com/Microsoft/BotFramework-WebChat) para el control de Chat en web en GitHub.</span><span class="sxs-lookup"><span data-stu-id="15e5e-190">You can [download the source code](https://github.com/Microsoft/BotFramework-WebChat) for the web chat control on GitHub.</span></span>
* <span data-ttu-id="15e5e-191">Asimismo, la [documentación de Bing Speech API](https://docs.microsoft.com/azure/cognitive-services/speech/home) proporciona más información sobre Bing Speech API.</span><span class="sxs-lookup"><span data-stu-id="15e5e-191">The [Bing Speech API documentation](https://docs.microsoft.com/azure/cognitive-services/speech/home) provides more information on the Bing Speech API.</span></span>

