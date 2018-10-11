---
title: Creación de un bot con el SDK de Bot Builder para Python | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para Python.
keywords: SDK de Bot Builder, creación de un bot, inicio rápido, python, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/30/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 6b63fe2780c51e57ee16c5e3dba5a83f46566157
ms.sourcegitcommit: 3bf3dbb1a440b3d83e58499c6a2ac116fe04b2f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2018
ms.locfileid: "46707287"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-python"></a><span data-ttu-id="ae01f-104">Creación de un bot con Bot Builder SDK para Python</span><span class="sxs-lookup"><span data-stu-id="ae01f-104">Create a bot with the Bot Builder SDK for Python</span></span>

>[!NOTE] 
> <span data-ttu-id="ae01f-105">El SDK para Python está en **versión preliminar**. Visite el [repositorio de GitHub](https://github.com/Microsoft/botbuilder-python) de Python para más información.</span><span class="sxs-lookup"><span data-stu-id="ae01f-105">The Python SDK is in **preview**, visit Python [GitHub repo](https://github.com/Microsoft/botbuilder-python) for more information.</span></span> 

<span data-ttu-id="ae01f-106">Este inicio rápido le guía a través del desarrollo de un bot y, a continuación, con la prueba con Bot Framework Emulator.</span><span class="sxs-lookup"><span data-stu-id="ae01f-106">This quickstart walks you through building a bot, and then testing it with the Bot Framework Emulator.</span></span> 

## <a name="pre-requisite"></a><span data-ttu-id="ae01f-107">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="ae01f-107">Pre-requisite</span></span>
- [<span data-ttu-id="ae01f-108">Python 3.6.4</span><span class="sxs-lookup"><span data-stu-id="ae01f-108">Python 3.6.4</span></span>](https://www.python.org/downloads/) 
- [<span data-ttu-id="ae01f-109">Bot Framework Emulator</span><span class="sxs-lookup"><span data-stu-id="ae01f-109">Bot Framework Emulator</span></span>](https://github.com/Microsoft/BotFramework-Emulator/releases)

## <a name="create-a-bot"></a><span data-ttu-id="ae01f-110">Creación de un bot</span><span class="sxs-lookup"><span data-stu-id="ae01f-110">Create a bot</span></span>
<span data-ttu-id="ae01f-111">En el archivo main.py, importe los módulos estándares siguientes:</span><span class="sxs-lookup"><span data-stu-id="ae01f-111">In the main.py file, import the following standard modules:</span></span>

```python
import http.server
import json
import asyncio
```

<span data-ttu-id="ae01f-112">Y los siguientes módulos de SDK:</span><span class="sxs-lookup"><span data-stu-id="ae01f-112">And the following SDK modules:</span></span>
```python
from botbuilder.schema import (Activity, ActivityTypes)
from botframework.connector import ConnectorClient
from botframework.connector.auth import (MicrosoftAppCredentials,
                                         JwtTokenValidation, SimpleCredentialProvider)
```
<span data-ttu-id="ae01f-113">A continuación, agregue el código siguiente para crear el bot mediante ConnectorClient:</span><span class="sxs-lookup"><span data-stu-id="ae01f-113">Next, add the following code to create the bot using the ConnectorClient:</span></span>
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
            loop.run_until_complete(JwtTokenValidation.authenticate_request(
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


<span data-ttu-id="ae01f-114">Guarde main.py.</span><span class="sxs-lookup"><span data-stu-id="ae01f-114">Save main.py.</span></span> <span data-ttu-id="ae01f-115">Para ejecutar el código de ejemplo en Windows, escriba lo que se indica a continuación en la ventana de la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="ae01f-115">To run the sample on Windows, enter the following into your command line window:</span></span>
```
python main.py
```
<span data-ttu-id="ae01f-116">En el terminal local debería ver el mensaje "Started http server on localhost:9000" ("Se ha iniciado el servidor http en el host local:9000").</span><span class="sxs-lookup"><span data-stu-id="ae01f-116">In your local terminal you should see the message 'Started http server on localhost:9000'</span></span>

## <a name="start-the-emulator-and-connect-your-bot"></a><span data-ttu-id="ae01f-117">Inicio del emulador y conexión del bot</span><span class="sxs-lookup"><span data-stu-id="ae01f-117">Start the emulator and connect your bot</span></span>

<span data-ttu-id="ae01f-118">A continuación, inicie el emulador y, después, conéctese al bot en el emulador:</span><span class="sxs-lookup"><span data-stu-id="ae01f-118">Next, start the emulator and then connect to your bot in the emulator:</span></span>

1. <span data-ttu-id="ae01f-119">Haga clic en el vínculo **Open Bot** (Abrir bot) de la pestaña de bienvenida del emulador.</span><span class="sxs-lookup"><span data-stu-id="ae01f-119">Click the **Open Bot** link in the emulator "Welcome" tab.</span></span> 
2. <span data-ttu-id="ae01f-120">Seleccione el archivo .bot ubicado en el directorio donde se creó el proyecto.</span><span class="sxs-lookup"><span data-stu-id="ae01f-120">Select the .bot file located in the directory where you created the project.</span></span>

## <a name="interact-with-your-bot"></a><span data-ttu-id="ae01f-121">Interacción con el bot</span><span class="sxs-lookup"><span data-stu-id="ae01f-121">Interact with your bot</span></span>

<span data-ttu-id="ae01f-122">Envíe un mensaje al bot y este responderá con un mensaje.</span><span class="sxs-lookup"><span data-stu-id="ae01f-122">Send a message to your bot, and the bot will respond back with a message.</span></span>
<span data-ttu-id="ae01f-123">![Emulador en ejecución](../media/emulator-v4/emulator-running.png)</span><span class="sxs-lookup"><span data-stu-id="ae01f-123">![Emulator running](../media/emulator-v4/emulator-running.png)</span></span>


## <a name="next-steps"></a><span data-ttu-id="ae01f-124">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="ae01f-124">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ae01f-125">Conceptos de bots</span><span class="sxs-lookup"><span data-stu-id="ae01f-125">Bot concepts</span></span>](../v4sdk/bot-builder-basics.md)
