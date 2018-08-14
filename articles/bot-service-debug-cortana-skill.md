---
title: Probar una habilidad de Cortana | Microsoft Docs
description: Aprenda a probar un bot de Cortana invocando una habilidad de Cortana.
keywords: SDK de Bot Builder, registrar el bot, cortana
author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/01/18
ms.openlocfilehash: 25474f821d64ea50442d9777d8f891124eb27573
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305072"
---
# <a name="test-a-cortana-skill"></a><span data-ttu-id="bbef2-104">Probar una habilidad de Cortana</span><span class="sxs-lookup"><span data-stu-id="bbef2-104">Test a Cortana skill</span></span>
 
<span data-ttu-id="bbef2-105">Si ah compilado una habilidad de Cortana con el SDK de Bot Builder, puede probarla invocándola desde Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-105">If you've built a Cortana skill using the Bot Builder SDK, you can test it by invoking it from Cortana.</span></span> <span data-ttu-id="bbef2-106">Las siguientes instrucciones le guiarán a través de los pasos necesarios para probar una habilidad de Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-106">The following instructions walk you through the steps required to try out your Cortana skill.</span></span>

## <a name="register-your-bot"></a><span data-ttu-id="bbef2-107">Registrar el bot</span><span class="sxs-lookup"><span data-stu-id="bbef2-107">Register your bot</span></span>
<span data-ttu-id="bbef2-108">Si [creó el bot](~/bot-service-quickstart.md) usando Bot Service en Azure, entonces ya estará registrado y puede omitir este paso.</span><span class="sxs-lookup"><span data-stu-id="bbef2-108">If you [created your bot](~/bot-service-quickstart.md) using Bot Service in Azure, then your bot is already registered and you can skip this step.</span></span>

<span data-ttu-id="bbef2-109">Si implementó el bot en otro lugar o si quiere probar el bot de forma local, debe [registrarlo](bot-service-quickstart-registration.md) para que pueda conectarse a Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-109">If you have deployed your bot elsewhere or if you want to test your bot locally, then you must [register](bot-service-quickstart-registration.md) your bot so that you can connect it to Cortana.</span></span> <span data-ttu-id="bbef2-110">Durante el proceso de registro, deberá proporcionar el **punto de conexión de mensajería** del bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-110">During the registration process, you will need to provide your bot's **Messaging endpoint**.</span></span> <span data-ttu-id="bbef2-111">Si decide probar el bot de forma local, debe ejecutar un software de tunelización como [ngrok](http://ngrok.com) para obtener un punto de conexión para el bot local.</span><span class="sxs-lookup"><span data-stu-id="bbef2-111">If you choose to test your bot locally, you will need to run a tunneling software such as [ngrok](http://ngrok.com) to get an endpoint for your local bot.</span></span>

## <a name="get-messaging-endpoint-using-ngrok"></a><span data-ttu-id="bbef2-112">Obtener el punto de conexión de mensajería con ngrok</span><span class="sxs-lookup"><span data-stu-id="bbef2-112">Get messaging endpoint using ngrok</span></span>

<span data-ttu-id="bbef2-113">Si está ejecutando el bot de forma local, puede obtener un punto de conexión y usarlo para realizar pruebas; para ello, ejecute un software de tunelización como [ngrok](https://ngrok.com).</span><span class="sxs-lookup"><span data-stu-id="bbef2-113">If you are running the bot locally, you can get an endpoint to use for testing by running tunneling software, such as [ngrok](https://ngrok.com).</span></span> <span data-ttu-id="bbef2-114">Para usar ngrok para obtener un punto de conexión, escriba lo siguiente desde la ventana de una consola:</span><span class="sxs-lookup"><span data-stu-id="bbef2-114">To use ngrok to get an endpoint, from a console window type:</span></span> 

```cmd
 ngrok.exe http 3979 -host-header="localhost:3979"
``` 

<span data-ttu-id="bbef2-115">Esto configura y muestra un enlace de reenvío ngrok que reenvía las solicitudes al bot, que se ejecuta en el puerto 3978.</span><span class="sxs-lookup"><span data-stu-id="bbef2-115">This configures and displays an ngrok forwarding link that forwards requests to your bot, which is running on port 3978.</span></span> <span data-ttu-id="bbef2-116">La dirección URL del enlace de reenvío debe tener el siguiente aspecto: `https://0d6c4024.ngrok.io`.</span><span class="sxs-lookup"><span data-stu-id="bbef2-116">The URL to the forwarding link should look something like this: `https://0d6c4024.ngrok.io`.</span></span>  <span data-ttu-id="bbef2-117">Anexe `/api/messages` al enlace para crear una dirección URL del punto de conexión de mensajería que tenga este formato: `https://0d6c4024.ngrok.io/api/messages`.</span><span class="sxs-lookup"><span data-stu-id="bbef2-117">Append `/api/messages` to the link, to form a messaging endpoint URL in this format: `https://0d6c4024.ngrok.io/api/messages`.</span></span> 

<span data-ttu-id="bbef2-118">Escriba esta dirección URL del punto de conexión en la sección **Configuración** de la hoja [Configuración](~/bot-service-manage-settings.md) del bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-118">Enter this endpoint URL in the **Configuration** section of your bot's [Settings](~/bot-service-manage-settings.md) blade.</span></span>

## <a name="enable-speech-recognition-priming"></a><span data-ttu-id="bbef2-119">Habilitar el desbloqueo del reconocimiento de voz</span><span class="sxs-lookup"><span data-stu-id="bbef2-119">Enable speech recognition priming</span></span>
<span data-ttu-id="bbef2-120">Si el bot utiliza una aplicación de tipo Language Understanding (LUIS), asegúrese de asociar el id. de la aplicación LUIS al servicio de bot registrado.</span><span class="sxs-lookup"><span data-stu-id="bbef2-120">If your bot uses a Language Understanding (LUIS) app, make sure you associate the LUIS application ID with your registered bot service.</span></span> <span data-ttu-id="bbef2-121">Esto permitirá al robot a reconocer expresiones verbales que se hayan definido en el modelo LUIS.</span><span class="sxs-lookup"><span data-stu-id="bbef2-121">This helps your bot recognize spoken utterances that are defined in your LUIS model.</span></span> <span data-ttu-id="bbef2-122">Para obtener más información, consulte [Speech priming](~/bot-service-manage-speech-priming.md) (Desbloqueo de voz).</span><span class="sxs-lookup"><span data-stu-id="bbef2-122">For more information, see [Speech priming](~/bot-service-manage-speech-priming.md).</span></span>

## <a name="add-the-cortana-channel"></a><span data-ttu-id="bbef2-123">Agregar el canal de Cortana</span><span class="sxs-lookup"><span data-stu-id="bbef2-123">Add the Cortana channel</span></span>
<span data-ttu-id="bbef2-124">Para agregar Cortana como un canal, siga los pasos indicados en [Connect a bot to Cortana](bot-service-channel-connect-cortana.md) (Conectar un bot a Cortana).</span><span class="sxs-lookup"><span data-stu-id="bbef2-124">To add Cortana as a channel, follow steps listed in [Connect a bot to Cortana](bot-service-channel-connect-cortana.md).</span></span>

## <a name="test-using-web-chat-control"></a><span data-ttu-id="bbef2-125">Probar el control Chat en web</span><span class="sxs-lookup"><span data-stu-id="bbef2-125">Test using Web Chat control</span></span>

<span data-ttu-id="bbef2-126">Para probar el bot con el control Chat en web integrado en Bot Service, haga clic en **Probar en Chat en web**, y escriba un mensaje para comprobar que el bot funciona.</span><span class="sxs-lookup"><span data-stu-id="bbef2-126">To test your bot using the integrated web chat control in Bot Service, click **Test in Web Chat** and type a message to verify that your bot is working.</span></span>

## <a name="test-using-emulator"></a><span data-ttu-id="bbef2-127">Hacer pruebas con el emulador</span><span class="sxs-lookup"><span data-stu-id="bbef2-127">Test using emulator</span></span>

<span data-ttu-id="bbef2-128">Para probar el bot con el [emulador](~/bot-service-debug-emulator.md), haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bbef2-128">To test your bot using the [emulator](~/bot-service-debug-emulator.md), do the following:</span></span>

1. <span data-ttu-id="bbef2-129">Ejecute el bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-129">Run the bot.</span></span>
2. <span data-ttu-id="bbef2-130">Abra el emulador y rellene la información necesaria.</span><span class="sxs-lookup"><span data-stu-id="bbef2-130">Open the emulator and fill in the necessary information.</span></span> <span data-ttu-id="bbef2-131">Para buscar los valores de **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID and MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword) (MicrosoftAppID y MicrosoftAppPassword).</span><span class="sxs-lookup"><span data-stu-id="bbef2-131">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span> 
3. <span data-ttu-id="bbef2-132">Haga clic en **Conectar** para conectar el emulador al bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-132">Click **Connect** to connect the emulator to your bot.</span></span>
4. <span data-ttu-id="bbef2-133">Escriba un mensaje para comprobar que el bot funciona.</span><span class="sxs-lookup"><span data-stu-id="bbef2-133">Type a message to verify that your bot is working.</span></span>

## <a name="test-using-cortana"></a><span data-ttu-id="bbef2-134">Hacer pruebas usando Cortana</span><span class="sxs-lookup"><span data-stu-id="bbef2-134">Test using Cortana</span></span>
<span data-ttu-id="bbef2-135">Puede invocar las habilidades de Cortana con la voz; solo diga una frase de invocación a Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-135">You can invoke your Cortana skill by speaking an invocation phrase to Cortana.</span></span> 
1. <span data-ttu-id="bbef2-136">Abra Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-136">Open Cortana.</span></span>
2. <span data-ttu-id="bbef2-137">Abra el Cuaderno de Cortana y haga clic en **Acerca de mí** para ver qué cuenta está usando en Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-137">Open the Notebook within Cortana and click **About me** to see which account you're using for Cortana.</span></span> <span data-ttu-id="bbef2-138">Asegúrese de que tiene la misma cuenta de Microsoft que utilizó para registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-138">Make sure you are signed in with the same Microsoft account that you used to register your bot.</span></span> 
   <span data-ttu-id="bbef2-139">![Iniciar sesión en el Cuaderno de Cortana](~/media/cortana/cortana-notebook.png)</span><span class="sxs-lookup"><span data-stu-id="bbef2-139">![Sign in to Cortana's notebook](~/media/cortana/cortana-notebook.png)</span></span>
2. <span data-ttu-id="bbef2-140">Haga clic en el botón del micrófono en la aplicación Cortana o en el cuadro de búsqueda "Ask me anything" (Pregúntame cualquier cosa) en Windows, y diga la [frase de invocación][InvocationNameGuidelines] del bot.</span><span class="sxs-lookup"><span data-stu-id="bbef2-140">Click on the microphone button in the Cortana app or in the "Ask me anything" search box in Windows, and say your bot's [invocation phrase][InvocationNameGuidelines].</span></span> <span data-ttu-id="bbef2-141">La frase de invocación incluye un *nombre de invocación*, que identifica de manera única la habilidad de invocar.</span><span class="sxs-lookup"><span data-stu-id="bbef2-141">The invocation phrase includes an *invocation name*, which uniquely identifies the skill to invoke.</span></span> <span data-ttu-id="bbef2-142">Por ejemplo, si el nombre de invocación de una habilidad es "Northwind Photo", una frase de invocación adecuada puede incluir "Pídele a Northwind Photo que..." o "Dile a Northwind Photo que...".</span><span class="sxs-lookup"><span data-stu-id="bbef2-142">For example, if a skill's invocation name is "Northwind Photo", a proper invocation phrase could include "Ask Northwind Photo to..." or "Tell Northwind Photo that...".</span></span>

   <span data-ttu-id="bbef2-143">Debe especificar el *Nombre de invocación* del bot cuando lo configure en Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-143">You specify your bot's *Invocation Name* when you configure it for Cortana.</span></span>
   <span data-ttu-id="bbef2-144">![Escriba el nombre de la invocación cuando configure el canal de Cortana](~/media/cortana/cortana-invocation-name-callout.png)</span><span class="sxs-lookup"><span data-stu-id="bbef2-144">![Enter the invocation name when you configure the Cortana channel](~/media/cortana/cortana-invocation-name-callout.png)</span></span>

3. <span data-ttu-id="bbef2-145">Si Cortana reconoce su frase de invocación, el bot se inicia en el lienzo de Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-145">If Cortana recognizes your invocation phrase, your bot launches in Cortana's canvas.</span></span> 

## <a name="troubleshoot"></a><span data-ttu-id="bbef2-146">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="bbef2-146">Troubleshoot</span></span>

<span data-ttu-id="bbef2-147">Si su habilidad de Cortana no se inicia, compruebe lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bbef2-147">If your Cortana skill fails to launch, check the following:</span></span>
* <span data-ttu-id="bbef2-148">Asegúrese de haber iniciado sesión en Cortana con la misma cuenta de Microsoft que usó para registrar el bot en el portal de Bot Framework.</span><span class="sxs-lookup"><span data-stu-id="bbef2-148">Make sure you are signed in to Cortana using the same Microsoft account that you used to register your bot in the Bot Framework Portal.</span></span>
* <span data-ttu-id="bbef2-149">Compruebe si el bot funciona; para ello, haga clic en **Probar en Chat en web** para abrir la ventana **Chat**, y escriba un mensaje.</span><span class="sxs-lookup"><span data-stu-id="bbef2-149">Check if the bot is working by clicking **Test in Web chat** to open the **Chat** window and typing a message to it.</span></span>
* <span data-ttu-id="bbef2-150">Compruebe si el nombre de invocación cumple con las [instrucciones][InvocationNameGuidelines] indicadas.</span><span class="sxs-lookup"><span data-stu-id="bbef2-150">Check if your invocation name meets the [guidelines][InvocationNameGuidelines].</span></span> <span data-ttu-id="bbef2-151">Si su nombre de invocación tiene más de tres palabras, es difícil de pronunciar o suena igual que otras palabras, Cortana puede tener dificultades para reconocerlo.</span><span class="sxs-lookup"><span data-stu-id="bbef2-151">If your invocation name is longer than three words, hard to pronounce, or sounds like other words, Cortana might have difficulty recognizing it.</span></span>
* <span data-ttu-id="bbef2-152">Si su habilidad usa un modelo LUIS, asegúrese de [habilitar el desbloqueo del reconocimiento de voz ](~/bot-service-manage-speech-priming.md).</span><span class="sxs-lookup"><span data-stu-id="bbef2-152">If your skill uses a LUIS model, make sure you [enable speech recognition priming](~/bot-service-manage-speech-priming.md).</span></span>

<span data-ttu-id="bbef2-153">Consulte [Enable Debugging of Cortana skills][Cortana-TestBestPractice] (Habilitar la depuración de las habilidades de Cortana) para obtener sugerencias adicionales para la solución de problemas e información sobre cómo habilitar la depuración de la habilidad en el panel de Cortana.</span><span class="sxs-lookup"><span data-stu-id="bbef2-153">See the [Enable Debugging of Cortana skills][Cortana-TestBestPractice] for additional troubleshooting tips and information on how to enable debugging of your skill in the Cortana dashboard.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="bbef2-154">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="bbef2-154">Next steps</span></span>

<span data-ttu-id="bbef2-155">Una vez que haya probado la habilidad de Cortana y comprobado que funciona adecuadamente, puede implementarla en un grupo de evaluadores beta o publicarla para el público en general.</span><span class="sxs-lookup"><span data-stu-id="bbef2-155">Once you have tested your Cortana skill and verified that it works the way you'd like it to, you can deploy it to a group of beta testers or release it to the public.</span></span> <span data-ttu-id="bbef2-156">Consulte [Publishing Cortana Skills][Cortana-Publish] (Publicar habilidades de Cortana) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="bbef2-156">See [Publishing Cortana Skills][Cortana-Publish] for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bbef2-157">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bbef2-157">Additional resources</span></span>
* <span data-ttu-id="bbef2-158">[The Cortana Skills Kit][CortanaGetStarted] (El kit de habilidades de Cortana)</span><span class="sxs-lookup"><span data-stu-id="bbef2-158">[The Cortana Skills Kit][CortanaGetStarted]</span></span>
* <span data-ttu-id="bbef2-159">[Preview features with the Channel Inspector](bot-service-channel-inspector.md) (Vista previa de las características con el Inspector de canales)</span><span class="sxs-lookup"><span data-stu-id="bbef2-159">[Preview features with the Channel Inspector](bot-service-channel-inspector.md)</span></span>

[CortanaGetStarted]: /cortana/getstarted

[BFPortal]: https://dev.botframework.com/
[CortanaDevCenter]: https://developer.microsoft.com/en-us/cortana

[CortanaSpecificEntities]: https://aka.ms/lgvcto
[CortanaAuth]: https://aka.ms/vsdqcj

[InvocationNameGuidelines]: https://aka.ms/cortana-invocation-guidelines 


[Cortana-Debug]: https://aka.ms/cortana-enable-debug
[Cortana-TestBestPractice]: https://aka.ms/cortana-test-best-practice
[Cortana-Publish]: /cortana/skills/publish-skill
