---
title: Creación de un bot con el SDK de Bot Builder para Python | Microsoft Docs
description: Cree un bot de forma rápida con el SDK de Bot Builder para Python.
keywords: SDK de Bot Builder, creación de un bot, inicio rápido, python, introducción
author: jonathanfingold
ms.author: jonathanfingold
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 08/21/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 6458bac5140fae14e8925e7af37aa8ac4ef1f1f5
ms.sourcegitcommit: 7b5675bbf7f1c2432bfc831ee5d627f6e5659e01
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/01/2018
ms.locfileid: "43381001"
---
# <a name="create-a-bot-with-the-bot-builder-sdk-for-python"></a>Creación de un bot con Bot Builder SDK para Python
[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

SDK de Bot Builder para Python es un marco fácil de usar para el desarrollo de bots. Este inicio rápido le guía a través del desarrollo de un bot y, a continuación, con la prueba con Bot Framework Emulator. El SDK v4 está en versión preliminar. Visite el [repositorio de GitHub](https://github.com/Microsoft/botbuilder-python) de Python para más información. 

## <a name="pre-requisite"></a>Requisito previo
- [Python 3.6.4](https://www.python.org/downloads/) 
- [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)

# <a name="create-a-bot"></a>Creación de un bot
En el archivo main.py, importe los módulos estándares siguientes:

```python
import http.server
import json
import asyncio
```

Y los siguientes módulos de SDK:
```python
from botbuilder.schema import (Activity, ActivityTypes)
from botframework.connector import ConnectorClient
from botframework.connector.auth import (MicrosoftAppCredentials,
                                         JwtTokenValidation, SimpleCredentialProvider)
```
A continuación, agregue el código siguiente para crear el bot mediante ConnectorClient:
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


Guarde main.py. Para ejecutar el código de ejemplo en Windows, escriba lo que se indica a continuación en la ventana de la línea de comandos:
```
python main.py
```
En el terminal local debería ver el mensaje "Started http server on localhost:9000" ("Se ha iniciado el servidor http en el host local:9000").

### <a name="start-the-emulator-and-connect-your-bot"></a>Inicio del emulador y conexión del bot

A continuación, inicie el emulador y, después, conéctese al bot en el emulador:


1. Haga clic en el vínculo **create a new bot configuration** (Crear configuración de bot) en la pestaña de bienvenida del emulador. 

2. Escriba un **nombre de bot** y la ruta de acceso del directorio al código del bot. El archivo de configuración del bot se guarda en esta ruta de acceso.

3. Escriba `http://localhost:port-number/api/messages` en el campo **Endpoint URL** (Dirección URL del punto de conexión), donde *port-number* coincide con el número de puerto que se muestra en el explorador en el que se ejecuta la aplicación.

4. Haga clic en **Connect** (Conectar) para conectarse al bot. No tendrá que especificar los valores de **Microsoft App ID** (id. de la aplicación de Microsoft) ni **Microsoft App Password** (contraseña de la aplicación de Microsoft). Por ahora puede dejar estos campos en blanco. Obtendrá esta información más adelante al registrar el bot.

Escriba **Hola** en el emulador y el bot repetirá **Ha dicho "Hola"**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Conceptos básicos de bot](../v4sdk/bot-builder-basics.md)
