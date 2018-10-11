---
title: Creación de un bot con el SDK de Bot Builder para Java | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para Java.
keywords: SDK de Bot Builder, crear un bot, inicio rápido, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/30/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: bcfc1c76199d8bc729376bbbfe229b0781eb82ab
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707241"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-java"></a><span data-ttu-id="4f6cf-104">Creación de un bot con el SDK de Bot Builder para Java</span><span class="sxs-lookup"><span data-stu-id="4f6cf-104">Create a bot with the Bot Builder SDK for Java</span></span> 
> [!NOTE] 
> <span data-ttu-id="4f6cf-105">El SDK v4 para Java está en **versión preliminar**.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-105">The Java SDK v4 is in **preview**.</span></span> <span data-ttu-id="4f6cf-106">Para más información, visite el [repositorio de GitHub](https://github.com/Microsoft/botbuilder-java) para Java.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-106">For more information, visit Java [GitHub repo](https://github.com/Microsoft/botbuilder-java).</span></span> <span data-ttu-id="4f6cf-107">Nuestros ejemplos de código y documentos van dirigidos actualmente a Java versión 1.8.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-107">Our code samples and docs are currently targeting Java version 1.8.</span></span>

## <a name="getting-started"></a><span data-ttu-id="4f6cf-108">Introducción</span><span class="sxs-lookup"><span data-stu-id="4f6cf-108">Getting Started</span></span>

<span data-ttu-id="4f6cf-109">El SDK v4 para Java consta de una serie de [bibliotecas](https://github.com/Microsoft/botbuilder-java/tree/master/libraries).</span><span class="sxs-lookup"><span data-stu-id="4f6cf-109">The Java SDK v4 consists of a series of [libraries](https://github.com/Microsoft/botbuilder-java/tree/master/libraries).</span></span> <span data-ttu-id="4f6cf-110">Para compilarlas localmente, consulte [Building the SDK](https://github.com/Microsoft/botbuilder-java/wiki/building-the-sdk) (Compilación del SDK).</span><span class="sxs-lookup"><span data-stu-id="4f6cf-110">To build them locally, see [Building the SDK](https://github.com/Microsoft/botbuilder-java/wiki/building-the-sdk).</span></span>

- <span data-ttu-id="4f6cf-111">Instale [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span><span class="sxs-lookup"><span data-stu-id="4f6cf-111">Install [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)</span></span>

## <a name="create-echobot"></a><span data-ttu-id="4f6cf-112">Creación de EchoBot</span><span class="sxs-lookup"><span data-stu-id="4f6cf-112">Create EchoBot</span></span>

<span data-ttu-id="4f6cf-113">En el archivo App.java, agregue lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4f6cf-113">In the App.java file add the following:</span></span>

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

<span data-ttu-id="4f6cf-114">Si usa Maven, puede copiar el archivo pom.xml de la carpeta de ejemplos en este repositorio.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-114">If you are using Maven you can copy the pom.xml file from the samples folder in this repo.</span></span> <span data-ttu-id="4f6cf-115">Ejecute el archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-115">Run your executable.</span></span> <span data-ttu-id="4f6cf-116">En este momento, el bot se ejecuta de forma local.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-116">At this point, your bot is running locally.</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="4f6cf-117">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="4f6cf-117">Start the emulator and connect your bot</span></span>

<span data-ttu-id="4f6cf-118">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="4f6cf-118">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="4f6cf-119">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-119">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="4f6cf-120">Seleccione el archivo .bot ubicado en el directorio donde se creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-120">Select the .bot file located in the directory where you created the project.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="4f6cf-121">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="4f6cf-121">Interact with your bot</span></span>

<span data-ttu-id="4f6cf-122">Envíe un mensaje al bot y este responderá con un mensaje.</span><span class="sxs-lookup"><span data-stu-id="4f6cf-122">Send message to your bot, and the bot will respond back with a message.</span></span>
<span data-ttu-id="4f6cf-123">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="4f6cf-123">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f6cf-124">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4f6cf-124">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4f6cf-125">Conceptos de bots</span><span class="sxs-lookup"><span data-stu-id="4f6cf-125">Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
