---
title: Conexión de un bot al canal Chat en web | Microsoft Docs
description: Obtenga información sobre cómo usar el control Chat en web en la página web para un bot conectado al canal Chat en web.
keywords: chat en web, canal de bot, página web, clave secreta, IFrame, HTML
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
ms.openlocfilehash: b5a9d20c058fe425d727bf2e39597e7dd29ec077
ms.sourcegitcommit: 3cb288cf2f09eaede317e1bc8d6255becf1aec61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47389634"
---
# <a name="connect-a-bot-to-web-chat"></a><span data-ttu-id="9c2d3-104">Conexión de un bot a un Chat en web</span><span class="sxs-lookup"><span data-stu-id="9c2d3-104">Connect a bot to Web Chat</span></span>
<span data-ttu-id="9c2d3-105">Cuando se [crea un bot](bot-service-quickstart.md) con Bot Service, el canal Chat en web se configura automáticamente.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-105">When you [create a bot](bot-service-quickstart.md) with Bot Service, the Web Chat channel is automatically configured for you.</span></span> <span data-ttu-id="9c2d3-106">El canal Chat en web incluye el control Chat en web, que proporciona la capacidad para que los usuarios interactúen con el bot directamente en una página web.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-106">The Web Chat channel includes the web chat control, which provides the ability for users to interact with your bot directly in a web page.</span></span>

![Ejemplo de chat en web](~/media/bot-service-channel-webchat/webchat-sample.png)

<span data-ttu-id="9c2d3-108">El canal Chat en web del portal de Framework Bot contiene todo lo que necesita para insertar el control Chat en web en una página web.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-108">The Web Chat channel in the Bot Framework Portal contains everything you need to embed the web chat control in a web page.</span></span> <span data-ttu-id="9c2d3-109">Lo único que debe hacer para usar el control Chat en web es obtener la clave secreta del bot e insertar el control en una página web.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-109">All you have to do to use the web chat control is get your bot's secret key and embed the control in a web page.</span></span>

[!INCLUDE [Channel Inspector intro](~/includes/snippet-channel-inspector.md)]

## <a id="step-1"></a> <span data-ttu-id="9c2d3-110">Obtener la clave secreta del bot</span><span class="sxs-lookup"><span data-stu-id="9c2d3-110">Get your bot secret key</span></span>

1. <span data-ttu-id="9c2d3-111">Abra el bot en [Azure Portal](http://portal.azure.com) y haga clic en la hoja **Canales**.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-111">Open your bot in the [Azure Portal](http://portal.azure.com) and click **Channels** blade.</span></span>

2. <span data-ttu-id="9c2d3-112">Haga clic en **Editar** en el canal **Chat en web**.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-112">Click **Edit** for the **Web Chat** channel.</span></span>  
<span data-ttu-id="9c2d3-113">![Canal Chat en web](~/media/bot-service-channel-webchat/bot-service-channel-list.png)</span><span class="sxs-lookup"><span data-stu-id="9c2d3-113">![Web chat channel](~/media/bot-service-channel-webchat/bot-service-channel-list.png)</span></span>

3. <span data-ttu-id="9c2d3-114">En **Claves secretas**, haga clic en **Mostrar** para la primera clave.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-114">Under **Secret keys**, click **Show** for the first key.</span></span>  
<span data-ttu-id="9c2d3-115">![Clave secreta](~/media/bot-service-channel-webchat/secret-key.png)</span><span class="sxs-lookup"><span data-stu-id="9c2d3-115">![Secret key](~/media/bot-service-channel-webchat/secret-key.png)</span></span>

4. <span data-ttu-id="9c2d3-116">Copia la **clave secreta** y el **código para insertar**.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-116">Copy the **Secret key** and the **Embed code**.</span></span>

5. <span data-ttu-id="9c2d3-117">Haga clic en **Done**(Listo).</span><span class="sxs-lookup"><span data-stu-id="9c2d3-117">Click **Done**.</span></span>

## <a name="embed-the-web-chat-control-in-your-website"></a><span data-ttu-id="9c2d3-118">Insertar el control Chat en web en su sitio web</span><span class="sxs-lookup"><span data-stu-id="9c2d3-118">Embed the web chat control in your website</span></span>

<span data-ttu-id="9c2d3-119">Puede insertar el control Chat en web en su sitio web mediante una de las siguientes dos opciones.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-119">You can embed the web chat control in your website by using one of two options.</span></span>

### <a name="option-1---keep-your-secret-hidden-exchange-your-secret-for-a-token-and-generate-the-embed"></a><span data-ttu-id="9c2d3-120">Opción 1: Mantener el secreto oculto, intercambiar el secreto por un token y generar la inserción</span><span class="sxs-lookup"><span data-stu-id="9c2d3-120">Option 1 - Keep your secret hidden, exchange your secret for a token, and generate the embed</span></span>

<span data-ttu-id="9c2d3-121">Use esta opción si puede ejecutar una solicitud de servidor a servidor para intercambiar el secreto de chat en web por un token temporal, y si quiere hacer que sea difícil para otros desarrolladores insertar el bot en sus sitios web.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-121">Use this option if you can execute a server-to-server request to exchange your web chat secret for a temporary token, and if you want to make it difficult for other developers to embed your bot in their websites.</span></span> <span data-ttu-id="9c2d3-122">Aunque el uso de esta opción no impedirá en absoluto que otros desarrolladores inserten el bot en sus sitios web, hace que sea difícil hacerlo.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-122">Although using this option will not absolutely prevent other developers from embedding your bot in their websites, it does make it difficult for them to do so.</span></span>

<span data-ttu-id="9c2d3-123">Para intercambiar el secreto por un token y generar la inserción, siga los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="9c2d3-123">To exchange your secret for a token and generate the embed:</span></span>

1. <span data-ttu-id="9c2d3-124">Emita una solicitud **GET** para `https://webchat.botframework.com/api/tokens` y transfiera su secreto de chat en web a través del encabezado `Authorization`.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-124">Issue a **GET** request to `https://webchat.botframework.com/api/tokens` and pass your web chat secret via the `Authorization` header.</span></span> <span data-ttu-id="9c2d3-125">El encabezado `Authorization` utiliza el esquema `BotConnector` e incluye el secreto, como se muestra en la siguiente solicitud de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-125">The `Authorization` header uses the `BotConnector` scheme and includes your secret, as shown in the example request below.</span></span>

2. <span data-ttu-id="9c2d3-126">La respuesta a su solicitud **GET** contendrá el token (entre comillas) que se puede usar para iniciar una conversación al representar el control Chat en web dentro de un **IFrame**.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-126">The response to your **GET** request will contain the token (surrounded with quotation marks) that can be used to start a conversation by rendering the web chat control within an **iframe**.</span></span> <span data-ttu-id="9c2d3-127">Un token es válido solo para una conversación. Para iniciar otra conversación, debe generar un nuevo token.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-127">A token is valid for one conversation only; to start another conversation, you must generate a new token.</span></span>

3. <span data-ttu-id="9c2d3-128">Dentro del `iframe` **Código para insertar** que ha copiado del canal WebChat del portal de Bot Framework (como se describe en la sección [Obtener la clave secreta del bot](#step-1) anterior), cambie el parámetro `s=` por `t=` y reemplace "YOUR_SECRET_HERE" por su token.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-128">Within the `iframe` **Embed code** that you copied from the Web Chat channel within the Bot Framework Portal (as described in [Get your bot secret key](#step-1) above), change the `s=` parameter to `t=` and replace "YOUR_SECRET_HERE" with your token.</span></span>

> [!NOTE]
> <span data-ttu-id="9c2d3-129">Los tokens se renovarán automáticamente antes de que expiren.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-129">Tokens will automatically be renewed before they expire.</span></span> 

##### <a name="example-request"></a><span data-ttu-id="9c2d3-130">Solicitud de ejemplo</span><span class="sxs-lookup"><span data-stu-id="9c2d3-130">Example request</span></span>

<span data-ttu-id="9c2d3-131">\`\`\`requestGET https://webchat.botframework.com/api/tokens Autorización: BotConnector YOUR_SECRET_HERE</span><span class="sxs-lookup"><span data-stu-id="9c2d3-131">\`\`\`requestGET https://webchat.botframework.com/api/tokens Authorization: BotConnector YOUR_SECRET_HERE</span></span>
```

##### Example response 

```response
"IIbSpLnn8sA.dBB.MQBhAFMAZwBXAHoANgBQAGcAZABKAEcAMwB2ADQASABjAFMAegBuAHYANwA.bbguxyOv0gE.cccJjH-TFDs.ruXQyivVZIcgvosGaFs_4jRj1AyPnDt1wk1HMBb5Fuw"
```

##### <a name="example-iframe-using-token"></a><span data-ttu-id="9c2d3-132">IFrame de ejemplo (con el token)</span><span class="sxs-lookup"><span data-stu-id="9c2d3-132">Example iframe (using token)</span></span>

```html
<iframe src="https://webchat.botframework.com/embed/YOUR_BOT_ID?t=YOUR_TOKEN_HERE"></iframe>
```

##### <a name="example-html-code"></a><span data-ttu-id="9c2d3-133">Código HTML de ejemplo</span><span class="sxs-lookup"><span data-stu-id="9c2d3-133">Example html code</span></span>
```html
<!DOCTYPE html>
<html>
<body>
  <iframe id="chat" style="width: 400px; height: 400px;" src=''></iframe>
</body>
<script>

    var xhr = new XMLHttpRequest();
    xhr.open('GET', "https://webchat.botframework.com/api/tokens", true);
    xhr.setRequestHeader('Authorization', 'BotConnector ' + 'YOUR SECRET HERE');
    xhr.send();
    xhr.onreadystatechange = processRequest;

    function processRequest(e) {
      if (xhr.readyState == 4  && xhr.status == 200) {
        var response = JSON.parse(xhr.responseText);
        document.getElementById("chat").src="https://webchat.botframework.com/embed/lucas-direct-line?t="+response
      }
    }

  </script>
</html>
```

### <a id="option-2"></a> <span data-ttu-id="9c2d3-134">Opción 2: Insertar el control Chat en web en su sitio web mediante el secreto</span><span class="sxs-lookup"><span data-stu-id="9c2d3-134">Option 2 - Embed the web chat control in your website using the secret</span></span>

<span data-ttu-id="9c2d3-135">Use esta opción si quiere permitir que otros desarrolladores inserten fácilmente el bot en sus sitios web.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-135">Use this option if you want to allow other developers to easily embed your bot into their websites.</span></span> 

> [!WARNING]
> <span data-ttu-id="9c2d3-136">Si usa esta opción, otros desarrolladores podrán insertar el bot en sus sitios web simplemente copiando el código para insertar.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-136">If you use this option, other developers can embed your bot into their websites by simply copying your embed code.</span></span>

<span data-ttu-id="9c2d3-137">Para insertar el bot en su sitio web indicando el secreto en la etiqueta `iframe`, siga los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="9c2d3-137">To embed your bot in your website by specifying the secret within the `iframe` tag:</span></span>

1. <span data-ttu-id="9c2d3-138">Copie el **Código para insertar** `iframe` desde el canal Chat en web en el portal de Bot Framework (como se describe en la sección [Obtener la clave secreta del bot](#step-1) anterior).</span><span class="sxs-lookup"><span data-stu-id="9c2d3-138">Copy the `iframe` **Embed code** from the Web Chat channel within the Bot Framework Portal (as described in [Get your bot secret key](#step-1) above).</span></span>

2. <span data-ttu-id="9c2d3-139">En ese **Código para insertar**, reemplace "YOUR_SECRET_HERE" por la **clave secreta** que copió de la misma página.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-139">Within that **Embed code**, replace "YOUR_SECRET_HERE" with the **Secret key** value that you copied from the same page.</span></span>

##### <a name="example-iframe-using-secret"></a><span data-ttu-id="9c2d3-140">IFrame de ejemplo (con el secreto)</span><span class="sxs-lookup"><span data-stu-id="9c2d3-140">Example iframe (using secret)</span></span>

```html
<iframe src="https://webchat.botframework.com/embed/YOUR_BOT_ID?s=YOUR_SECRET_HERE"></iframe>
```

## <a name="style-the-web-chat-control"></a><span data-ttu-id="9c2d3-141">Estilo del control de chat web</span><span class="sxs-lookup"><span data-stu-id="9c2d3-141">Style the web chat control</span></span>

<span data-ttu-id="9c2d3-142">Puede cambiar el tamaño del control Chat en web mediante el uso del atributo `style` de `iframe` para especificar los valores `height` y `width`.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-142">You may change the size of the web chat control by using the `style` attribute of the `iframe` to specify `height` and `width`.</span></span>

```html
<iframe style="height:480px; width:402px" src="... SEE ABOVE ..."></iframe>
```

![Cliente de control de chat](~/media/chatwidget-client.png)

## <a name="additional-resources"></a><span data-ttu-id="9c2d3-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9c2d3-144">Additional resources</span></span>

<span data-ttu-id="9c2d3-145">También puede [descargar el código fuente](https://github.com/Microsoft/BotFramework-WebChat) para el control de Chat en web en GitHub.</span><span class="sxs-lookup"><span data-stu-id="9c2d3-145">You can [download the source code](https://github.com/Microsoft/BotFramework-WebChat) for the web chat control on GitHub.</span></span>
