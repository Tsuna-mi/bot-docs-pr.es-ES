---
title: Creación de un bot con el SDK de Bot Builder para Java | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para Java.
keywords: SDK de Bot Builder, crear un bot, inicio rápido, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 05/02/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: ca4714d3b3988fd08021f55a4905d9426996b7eb
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305116"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-java"></a><span data-ttu-id="0bb53-104">Creación de un bot con el SDK de Bot Builder para Java</span><span class="sxs-lookup"><span data-stu-id="0bb53-104">Create a bot with the Bot Builder SDK for Java</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="0bb53-105">El SDK de Bot Builder para Java proporciona a los desarrolladores de Java una manera conocida de escribir bots.</span><span class="sxs-lookup"><span data-stu-id="0bb53-105">The Bot Builder SDK for Java provides a familiar way for Java developers to write bots.</span></span> <span data-ttu-id="0bb53-106">El SDK v4 está en versión preliminar; para más información, visite el [repositorio de GitHub](https://github.com/Microsoft/botbuilder-java) para Java.</span><span class="sxs-lookup"><span data-stu-id="0bb53-106">The SDK v4 is in preview, for more information visit Java [GitHub repo](https://github.com/Microsoft/botbuilder-java).</span></span>

> [!NOTE]
> <span data-ttu-id="0bb53-107">Nuestros ejemplos de código y documentos van dirigidos actualmente a Java versión 1.8.</span><span class="sxs-lookup"><span data-stu-id="0bb53-107">Our code samples and docs are currently targeting Java version 1.8.</span></span>

## <a name="getting-started"></a><span data-ttu-id="0bb53-108">Introducción</span><span class="sxs-lookup"><span data-stu-id="0bb53-108">Getting Started</span></span>

<span data-ttu-id="0bb53-109">El SDK v4 está formado por una serie de [bibliotecas](https://github.com/Microsoft/botbuilder-java/tree/master/libraries).</span><span class="sxs-lookup"><span data-stu-id="0bb53-109">The v4 SDK consists of a series of [libraries](https://github.com/Microsoft/botbuilder-java/tree/master/libraries).</span></span> <span data-ttu-id="0bb53-110">Para compilarlas localmente, consulte [Building the SDK](https://github.com/Microsoft/botbuilder-java/wiki/building-the-sdk) (Compilación del SDK).</span><span class="sxs-lookup"><span data-stu-id="0bb53-110">To build them locally, see [Building the SDK](https://github.com/Microsoft/botbuilder-java/wiki/building-the-sdk).</span></span>

- <span data-ttu-id="0bb53-111">Instale [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span><span class="sxs-lookup"><span data-stu-id="0bb53-111">Install [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

### <a name="create-echobot"></a><span data-ttu-id="0bb53-112">Creación de EchoBot</span><span class="sxs-lookup"><span data-stu-id="0bb53-112">Create EchoBot</span></span>

<span data-ttu-id="0bb53-113">En el archivo App.java, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="0bb53-113">In the App.java file add the following:</span></span>

```Java
package com.microsoft.bot.connector.sample;

import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.microsoft.aad.adal4j.AuthenticationException;
import com.microsoft.bot.connector.customizations.CredentialProvider;
import com.microsoft.bot.connector.customizations.CredentialProviderImpl;
import com.microsoft.bot.connector.customizations.JwtTokenValidation;
import com.microsoft.bot.connector.customizations.MicrosoftAppCredentials;
import com.microsoft.bot.connector.implementation.ConnectorClientImpl;
import com.microsoft.bot.schema.models.Activity;
import com.microsoft.bot.schema.models.ActivityTypes;
import com.microsoft.bot.schema.models.ResourceResponse;
import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

import java.io.IOException;
import java.io.InputStream;
import java.net.InetSocketAddress;
import java.net.URLDecoder;
import java.util.logging.Level;
import java.util.logging.Logger;

public class App {
    private static final Logger LOGGER = Logger.getLogger( App.class.getName() );
    private static String appId = "";       // <-- app id -->
    private static String appPassword = ""; // <-- app password -->

    public static void main( String[] args ) throws IOException {
        CredentialProvider credentialProvider = new CredentialProviderImpl(appId, appPassword);
        HttpServer server = HttpServer.create(new InetSocketAddress(3978), 0);
        server.createContext("/api/messages", new MessageHandle(credentialProvider));
        server.setExecutor(null);
        server.start();
    }

    static class MessageHandle implements HttpHandler {
        private ObjectMapper objectMapper;
        private CredentialProvider credentialProvider;
        private MicrosoftAppCredentials credentials;

        MessageHandle(CredentialProvider credentialProvider) {
            this.objectMapper = new ObjectMapper()
                    .configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false)
                    .findAndRegisterModules();
            this.credentialProvider = credentialProvider;
            this.credentials = new MicrosoftAppCredentials(appId, appPassword);
        }

        public void handle(HttpExchange httpExchange) throws IOException {
            if (httpExchange.getRequestMethod().equalsIgnoreCase("POST")) {
                Activity activity = getActivity(httpExchange);
                String authHeader = httpExchange.getRequestHeaders().getFirst("Authorization");
                try {
                    JwtTokenValidation.assertValidActivity(activity, authHeader, credentialProvider);

                    // send ack to user activity
                    httpExchange.sendResponseHeaders(202, 0);
                    httpExchange.getResponseBody().close();

                    if (activity.type().equals(ActivityTypes.MESSAGE)) {
                        // reply activity with the same text
                        ConnectorClientImpl connector = new ConnectorClientImpl(activity.serviceUrl(), this.credentials);
                        ResourceResponse response = connector.conversations().sendToConversation(activity.conversation().id(),
                                new Activity()
                                        .withType(ActivityTypes.MESSAGE)
                                        .withText("Echo: " + activity.text())
                                        .withRecipient(activity.from())
                                        .withFrom(activity.recipient())
                        );
                    }
                } catch (AuthenticationException ex) {
                    httpExchange.sendResponseHeaders(401, 0);
                    httpExchange.getResponseBody().close();
                    LOGGER.log(Level.WARNING, "Auth failed!", ex);
                } catch (Exception ex) {
                    LOGGER.log(Level.WARNING, "Execution failed", ex);
                }
            }
        }

        private String getRequestBody(HttpExchange httpExchange) throws IOException {
            StringBuilder buffer = new StringBuilder();
            InputStream stream = httpExchange.getRequestBody();
            int rByte;
            while ((rByte = stream.read()) != -1) {
                buffer.append((char)rByte);
            }
            stream.close();
            if (buffer.length() > 0) {
                return URLDecoder.decode(buffer.toString(), "UTF-8");
            }
            return "";
        }

        private Activity getActivity(HttpExchange httpExchange) {
            try {
                String body = getRequestBody(httpExchange);
                LOGGER.log(Level.INFO, body);
                return objectMapper.readValue(body, Activity.class);
            } catch (Exception ex) {
                LOGGER.log(Level.WARNING, "Failed to get activity", ex);
                return null;
            }

        }
    }
}
```

<span data-ttu-id="0bb53-114">Si usa Maven, puede copiar el archivo pom.xml de la carpeta de ejemplos en este repositorio.</span><span class="sxs-lookup"><span data-stu-id="0bb53-114">If you are using Maven you can copy the pom.xml file from the samples folder in this repo.</span></span> <span data-ttu-id="0bb53-115">Una vez que ha comenzado la ejecución del ejecutable, inicie Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="0bb53-115">Once you have started running your executable, start the Bot Framework Emulator.</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="0bb53-116">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="0bb53-116">Start the emulator and connect your bot</span></span>

<span data-ttu-id="0bb53-117">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="0bb53-117">At this point, your bot is running locally.</span></span>
<span data-ttu-id="0bb53-118">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="0bb53-118">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="0bb53-119">Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="0bb53-119">Click **create a new bot configuration** link in the emulator "Welcome" tab.</span></span> 

2. <span data-ttu-id="0bb53-120">Escriba un **nombre de bot** y la ruta de acceso del directorio al código del bot.</span><span class="sxs-lookup"><span data-stu-id="0bb53-120">Enter a **Bot name** and enter the directory path to your bot code.</span></span> <span data-ttu-id="0bb53-121">El archivo de configuración del bot se guarda en esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="0bb53-121">The bot configuration file will be saved to this path.</span></span>

3. <span data-ttu-id="0bb53-122">Escriba `http://localhost:port-number/api/messages` en el campo **Endpoint URL** (Dirección URL del punto de conexión), donde *port-number* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0bb53-122">Type `http://localhost:port-number/api/messages` into the **Endpoint URL** field, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

4. <span data-ttu-id="0bb53-123">Haga clic en **Connect** (Conectar) para conectarse al bot.</span><span class="sxs-lookup"><span data-stu-id="0bb53-123">Click **Connect** to connect to your bot.</span></span> <span data-ttu-id="0bb53-124">No tendrá que especificar el **identificador de aplicación de Microsoft** ni la **contraseña de aplicación de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="0bb53-124">You won't need to specify **Microsoft App ID** and **Microsoft App Password**.</span></span> <span data-ttu-id="0bb53-125">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="0bb53-125">You can leave these fields blank for now.</span></span> <span data-ttu-id="0bb53-126">Más adelante obtendrá esta información al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="0bb53-126">You'll get this information later when you register your bot.</span></span>

### <a name="interact-with-your-bot"></a><span data-ttu-id="0bb53-127">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="0bb53-127">Interact with your bot</span></span>
<span data-ttu-id="0bb53-128">Envíe "Hola" a su bot y el bot repetirá el mensaje.</span><span class="sxs-lookup"><span data-stu-id="0bb53-128">Send "Hi" to your bot, and the bot will echo the message back.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0bb53-129">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="0bb53-129">Next steps</span></span>

<span data-ttu-id="0bb53-130">A continuación, vaya a los conceptos que explican qué es un bot y cómo funciona.</span><span class="sxs-lookup"><span data-stu-id="0bb53-130">Next, jump into the concepts that explain a bot and how it works.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0bb53-131">Conceptos básicos de bot</span><span class="sxs-lookup"><span data-stu-id="0bb53-131">Basic Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
