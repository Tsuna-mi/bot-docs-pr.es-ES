---
title: Creación de un bot con el SDK de Bot Builder para Python | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para Python.
keywords: SDK de Bot Builder, creación de un bot, inicio rápido, python, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 02/21/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: ba16adebe6bbb9b79949cd9842e975e35c3f2aa6
ms.sourcegitcommit: d486dd088b87a44fc8142f7a08877ff993861a42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928414"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-python"></a><span data-ttu-id="1236a-104">Creación de un bot con Bot Builder SDK para Python</span><span class="sxs-lookup"><span data-stu-id="1236a-104">Create a bot with the Bot Builder SDK for Python</span></span>
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

<span data-ttu-id="1236a-105">SDK de Bot Builder para Python es un marco fácil de usar para el desarrollo de bots.</span><span class="sxs-lookup"><span data-stu-id="1236a-105">The Bot Builder SDK for Python is an easy-to-use framework for developing bots.</span></span> <span data-ttu-id="1236a-106">Este inicio rápido le guía a través del desarrollo de un bot y, a continuación, con la prueba con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="1236a-106">This quickstart walks you through building a bot, and then testing it with the Bot Framework Emulator.</span></span> <span data-ttu-id="1236a-107">El SDK v4 está en versión preliminar. Visite el [repositorio de GitHub](https://github.com/Microsoft/botbuilder-python) de Python para más información.</span><span class="sxs-lookup"><span data-stu-id="1236a-107">The SDK v4 is in preview, visit Python [GitHub repo](https://github.com/Microsoft/botbuilder-python) for more information.</span></span> 

## <a name="pre-requisite"></a><span data-ttu-id="1236a-108">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="1236a-108">Pre-requisite</span></span>
- [<span data-ttu-id="1236a-109">Python 3.6.4</span><span class="sxs-lookup"><span data-stu-id="1236a-109">Python 3.6.4</span></span>](https://www.python.org/downloads/) 
- [<span data-ttu-id="1236a-110">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="1236a-110">Bot Framework Emulator</span></span>](https://github.com/Microsoft/BotFramework-Emulator/releases)

# <a name="create-a-bot"></a><span data-ttu-id="1236a-111">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="1236a-111">Create a bot</span></span>
<span data-ttu-id="1236a-112">En el archivo main.py, importe los módulos estándares siguientes:</span><span class="sxs-lookup"><span data-stu-id="1236a-112">In the main.py file, import the following standard modules:</span></span>

```python
import http.server
import json
import asyncio
```

<span data-ttu-id="1236a-113">Y los siguientes módulos de SDK:</span><span class="sxs-lookup"><span data-stu-id="1236a-113">And the following SDK modules:</span></span>
```python
from botbuilder.schema import (Activity, ActivityTypes)
from botframework.connector import ConnectorClient
from botframework.connector.auth import (MicrosoftAppCredentials,
                                         JwtTokenValidation, SimpleCredentialProvider)
```
<span data-ttu-id="1236a-114">A continuación, agregue el código siguiente para crear el bot mediante ConnectorClient:</span><span class="sxs-lookup"><span data-stu-id="1236a-114">Next, add the following code to create the bot using the ConnectorClient:</span></span>
```python
APP_ID = ''
APP_PASSWORD = ''


class BotRequestHandler(http.server.BaseHTTPRequestHandler):

    @staticmethod
    def __create_reply_activity(request_activity, text):
        return Activity(
            type=ActivityTypes.message,
            channel_id=request_activity.channel_id,
            conversation=request_activity.conversation,
            recipient=request_activity.from_property,
            from_property=request_activity.recipient,
            text=text,
            service_url=request_activity.service_url)

    def __handle_conversation_update_activity(self, activity):
        self.send_response(202)
        self.end_headers()
        if activity.members_added[0].id != activity.recipient.id:
            credentials = MicrosoftAppCredentials(APP_ID, APP_PASSWORD)
            reply = BotRequestHandler.__create_reply_activity(activity, 'Hello and welcome to the echo bot!')
            connector = ConnectorClient(credentials, base_url=reply.service_url)
            connector.conversations.send_to_conversation(reply.conversation.id, reply)

    def __handle_message_activity(self, activity):
        self.send_response(200)
        self.end_headers()
        credentials = MicrosoftAppCredentials(APP_ID, APP_PASSWORD)
        connector = ConnectorClient(credentials, base_url=activity.service_url)
        reply = BotRequestHandler.__create_reply_activity(activity, 'You said: %s' % activity.text)
        connector.conversations.send_to_conversation(reply.conversation.id, reply)

    def __handle_authentication(self, activity):
        credential_provider = SimpleCredentialProvider(APP_ID, APP_PASSWORD)
        loop = asyncio.new_event_loop()
        try:
            loop.run_until_complete(JwtTokenValidation.assert_valid_activity(
                activity, self.headers.get("Authorization"), credential_provider))
            return True
        except Exception as ex:
            self.send_response(401, ex)
            self.end_headers()
            return False
        finally:
            loop.close()

    def __unhandled_activity(self):
        self.send_response(404)
        self.end_headers()

    def do_POST(self):
        body = self.rfile.read(int(self.headers['Content-Length']))
        data = json.loads(str(body, 'utf-8'))
        activity = Activity.deserialize(data)

        if not self.__handle_authentication(activity):
            return

        if activity.type == ActivityTypes.conversation_update.value:
            self.__handle_conversation_update_activity(activity)
        elif activity.type == ActivityTypes.message.value:
            self.__handle_message_activity(activity)
        else:
            self.__unhandled_activity()


try:
    SERVER = http.server.HTTPServer(('localhost', 9000), BotRequestHandler)
    print('Started http server on localhost:9000')
    SERVER.serve_forever()
except KeyboardInterrupt:
    print('^C received, shutting down server')
    SERVER.socket.close()
```


<span data-ttu-id="1236a-115">Guarde main.py.</span><span class="sxs-lookup"><span data-stu-id="1236a-115">Save main.py.</span></span> <span data-ttu-id="1236a-116">Para ejecutar el código de ejemplo en Windows, escriba lo que se indica a continuación en la ventana de la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="1236a-116">To run the sample on Windows, enter the following into your command line window:</span></span>
```
python main.py
```
<span data-ttu-id="1236a-117">En el terminal local debería ver el mensaje "Started http server on localhost:9000" ("Se ha iniciado el servidor http en el host local:9000").</span><span class="sxs-lookup"><span data-stu-id="1236a-117">In your local terminal you should see the message 'Started http server on localhost:9000'</span></span>

### <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="1236a-118">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="1236a-118">Start the emulator and connect your bot</span></span>

<span data-ttu-id="1236a-119">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="1236a-119">Next, start the emulator and then connect to your bot in the emulator:</span></span>


1. <span data-ttu-id="1236a-120">Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="1236a-120">Click **create a new bot configuration** link in the emulator "Welcome" tab.</span></span> 

2. <span data-ttu-id="1236a-121">Escriba un **nombre de bot** y la ruta de acceso del directorio al código del bot.</span><span class="sxs-lookup"><span data-stu-id="1236a-121">Enter a **Bot name** and enter the directory path to your bot code.</span></span> <span data-ttu-id="1236a-122">El archivo de configuración del bot se guarda en esta ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="1236a-122">The bot configuration file will be saved to this path.</span></span>

3. <span data-ttu-id="1236a-123">Escriba `http://localhost:port-number/api/messages` en el campo **Endpoint URL** (Dirección URL del punto de conexión), donde *port-number* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1236a-123">Type `http://localhost:port-number/api/messages` into the **Endpoint URL** field, where *port-number* matches the port number shown in the browser where your application is running.</span></span>

4. <span data-ttu-id="1236a-124">Haga clic en **Connect** (Conectar) para conectarse al bot.</span><span class="sxs-lookup"><span data-stu-id="1236a-124">Click **Connect** to connect to your bot.</span></span> <span data-ttu-id="1236a-125">No tendrá que especificar los valores de **Microsoft App ID** (id. de la aplicación de Microsoft) ni **Microsoft App Password** (contraseña de la aplicación de Microsoft).</span><span class="sxs-lookup"><span data-stu-id="1236a-125">You won't need to specify **Microsoft App ID** and **Microsoft App Password**.</span></span> <span data-ttu-id="1236a-126">Por ahora puede dejar estos campos en blanco.</span><span class="sxs-lookup"><span data-stu-id="1236a-126">You can leave these fields blank for now.</span></span> <span data-ttu-id="1236a-127">Obtendrá esta información más adelante al registrar el bot.</span><span class="sxs-lookup"><span data-stu-id="1236a-127">You'll get this information later when you register your bot.</span></span>

<span data-ttu-id="1236a-128">Escriba **Hola** en el emulador y el bot repetirá **Ha dicho "Hola"**.</span><span class="sxs-lookup"><span data-stu-id="1236a-128">Type **Hello** in the emulator, and the bot will echo back **You said "Hello"**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1236a-129">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="1236a-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1236a-130">Conceptos básicos de bot</span><span class="sxs-lookup"><span data-stu-id="1236a-130">Basic Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
