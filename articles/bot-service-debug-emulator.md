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
ms.openlocfilehash: bc1da99c7d0f7a6431ad0c2746b8583ef7bfbd5f
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305457"
---
# <a name="debug-bots-with-the-bot-framework-emulator"></a>Depuración de bots con Bot Framework Emulator

Bot Framework Emulator es una aplicación de escritorio que permite que los desarrolladores de bots prueben y depuren sus bots de manera local o remota. Con el emulador, puede chatear con el bot e inspeccionar los mensajes que este envía y recibe. El emulador muestra los mensajes tal como aparecerían en la interfaz de usuario de un chat web y registra las respuestas y las solicitudes de JSON a medida que intercambia mensajes con su bot. 

> [!TIP] 
> Antes de implementar el bot en la nube, ejecútelo localmente y pruébelo con el emulador. Puede probar el bot con el emulador incluso si todavía no lo ha [registrado](~/bot-service-quickstart-registration.md) con Bot Framework ni lo ha configurado para que se ejecute en ningún canal.

![UI del emulador](media/emulator-v4/emulator-welcome.png)

## <a name="download-the-bot-framework-emulator"></a>Descarga de Bot Framework Emulator

Los paquetes de descarga para Mac, Windows y Linux están disponibles en la [página de versiones de GitHub](https://github.com/Microsoft/BotFramework-Emulator/releases).

## <a name="connect-to-a-bot-running-on-local-host"></a>Conexión a un bot que se ejecuta en un host local

![Puntos de conexión del emulador](media/emulator-v4/emulator-endpoint.png)

Para conectarse a un bot que se ejecuta localmente, seleccione la pestaña Bot Explorer (Explorador de bots) en la esquina superior izquierda. Haga clic en el icono **+** bajo la pestaña **Punto de conexión**. Aquí puede especificar el punto de conexión en el mismo puerto en que se ejecuta localmente el bot con el fin de conectarse a él. Haga clic en **Enviar** y será le redirigirá a una ventana de chat en directo, donde podrá interactuar con el bot.

## <a name="view-detailed-message-activity-with-the-inspector"></a>Vista detallada de la actividad de mensaje con el inspector

![Actividad de mensaje del emulador](media/emulator-v4/emulator-view-message-activity-02.png)

Puede hacer clic en cualquier burbuja de mensaje dentro de la ventana de la conversación e inspeccionar la actividad JSON sin formato con la característica **INSPECTOR** que se encuentra a la derecha de la ventana. Cuando se selecciona, la burbuja de mensaje cambia a color amarillo y el objeto JSON de la actividad se mostrará a la izquierda de la ventana del chat. Esta información de JSON incluye metadatos de clave que incluyen el identificador de canal, el tipo de actividad, el identificador de la conversación, el mensaje de texto, la dirección URL del punto de conexión, etc. Puede ver las actividades de inspección enviadas desde el usuario, así como también las actividades con las que responde el bot. 

También puede usar el [Inspector de canales](bot-service-channel-inspector.md) para obtener una vista previa de las características admitidas en canales específicos.


## <a name="save-and-load-conversations-with-bot-transcripts"></a>Guardado y carga de conversaciones con transcripciones de bots

La actividad de mensaje en el emulador se puede guardar como transcripciones. En una ventana del chat en directo abierto, puede seleccionar **Save Transcript As** (Guardar transcripción como) para asignar un nombre para el archivo de transcripción de salida y seleccionar la ubicación donde se guardará. 

>[!TIP]
> El botón **Start Over** (Volver a empezar) se puede usar en cualquier momento para borrar una conversación y reiniciar una conexión con el bot.  

![Guardar las transcripciones en el emulador](media/emulator-v4/emulator-live-chat.png)

Para cargar las transcripciones, simplemente seleccione **Archivo** --> **Open Tranascript File** (Abrir archivo de transcripción) y seleccione la transcripción. Se abrirá una ventana de transcripción nueva que presentará la actividad de mensaje en la ventana de salida. 

![Cargar transcripciones en el emulador](media/emulator-v4/emulator-load-transcript.png)

## <a name="author-transcripts-with-chatdown"></a>Creación de transcripciones con ChatDown

La herramienta [ChatDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown) es un generador de transcripciones que consume un archivo [Markdown](https://daringfireball.net/projects/markdown/syntax) para generar las transcripciones de la actividad. Puede crear sus propias transcripciones completamente en un formato Markdown y guardarlas como archivo **.chat** para generar las transcripciones. Esto es útil para crear rápidamente escenarios de conversación ficticios durante el desarrollo de bots.  

### <a name="prerequisites"></a>Requisitos previos

- [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases) versión 4 o superior 
- [Node.js](https://nodejs.org/en/)
 
ChatDown está disponible como módulo npm que requiere Node.js. Para instalar ChatDown, instálelo globalmente en la máquina. 

```
npm install -g chatdown
```
### <a name="create-and-load-transcript-transcript-files"></a>Creación y carga de archivos de transcripción ###

El siguiente es un ejemplo de cómo crear un archivo **.chat**. Estos son los archivos Markdown que contienen 2 partes:
- El encabezado que define los participantes de la conversación (usuario, bot)
- La conversación de ida y vuelta entre los participantes

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
[Haga clic aquí](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown/Examples) para ver más ejemplos de los archivos .chat. 

Para generar el archivo de transcripción, con el comando **chatdown** en la CLI, escriba el nombre del archivo .chat seguido de ">" y del nombre del archivo de transcripción de salida. 

```
chatdown sample.chat > sample.transcript
```
## <a name="manage-bot-resources-with-the-msbot-tool"></a>Administración de los recursos de bot con la herramienta MSBot

La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una sola ubicación. Este archivo también permite que el bot se conecte a estos servicios desde la CLI. La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:

```
npm install -g msbot 
```
![Ventana de la CLI de MSBot](media/emulator-v4/msbot-cli-window.png)


Para crear un archivo de bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:

```shell
msbot init --name az-cli-bot --endpoint http://localhost:3984/api/messages
```
![Botfile](media/emulator-v4/botfile-generated.png)

>**Nota:** El bot que se usa en esta guía es un bot de eco simple que se genera a partir de la extensión de bot de la CLI de Azure. [Haga clic aquí](https://github.com/Microsoft/botbuilder-tools/tree/master/AzureCli) para más información sobre la compilación de bots con la CLI de Azure. 

Con el archivo .bot, ahora puede cargar fácilmente el bot en el emulador. El archivo .bot también es necesario para registrar distintos puntos de conexión y componentes de lenguaje en el bot. 

![Bot-File-Dropdown](media/emulator-v4/bot-file-dropdown.png)

## <a name="add-language-services"></a>Incorporación de servicios de lenguaje 

Puede registrar fácilmente una aplicación de LUIS o una base de conocimiento de QnA en el archivo .bot directamente desde el emulador. Cuando el archivo .bot se cargue, seleccione el botón de servicios en el extremo izquierdo de la ventana del emulador. En el menú **Servicios** verá opciones para agregar LUIS, QnA Maker, Distribución, puntos de conexión y Azure Bot Service. 

Para agregar una aplicación de LUIS, simplemente haga clic en el botón **+** del menú de LUIS, escriba las credenciales de la aplicación de LUIS y haga clic en **Enviar**. Con esto se registrará la aplicación de LUIS en el archivo .bot y se conectará el servicio a la aplicación de bot. 

![Conexión de LUIS](media/emulator-v4/emulator-connect-luis-btn.png)

Del mismo modo, para agregar una base de conocimiento de QnA, simplemente haga clic en el botón **+** del menú de QnA, escriba las credenciales de la base de conocimiento de QnA Maker y haga clic en **Enviar**. La base de conocimiento ahora se registrará en el archivo .bot y estará lista para su uso. 

![Conexión de QnA](media/emulator-v4/emulator-connect-qna-btn.png)

Cuando se conecta cualquiera de esos servicios, puede volver a una ventana de chat activo y comprobar que los servicios están conectados y en funcionamiento. 

![QnA conectado](media/emulator-v4/emulator-view-message-activity.png)

## <a name="inspect-language-services"></a>Inspección de los servicios de lenguaje

Con el nuevo emulador v4 también puede inspeccionar las respuestas de JSON provenientes de LUIS y QnA. Mediante un bot con un servicio de lenguaje conectado, puede seleccionar **trace** en la ventana LOG (Registro) en la esquina inferior derecha. Esta herramienta nueva también proporciona características para actualizar los servicios de lenguaje directamente desde el emulador. 

![Inspector de LUIS](media/emulator-v4/emulator-luis-inspector.png)

Con un servicio de LUIS conectado, verá que el vínculo de seguimiento especifica el **seguimiento de LUIS**. Cuando se seleccione, verá la respuesta sin formato proveniente del servicio de LUIS, que incluye intenciones y entidades junto con sus puntuaciones especificadas. También tiene la opción de volver a asignar intenciones a las expresiones del usuario. 

![Inspector de QnA](media/emulator-v4/emulator-qna-inspector.png)

Con un servicio de QnA conectado, el registro mostrará el **seguimiento de QnA** y cuando esté seleccionado, podrá obtener una vista previa del par de pregunta y respuesta asociado con esa actividad, junto con una puntuación de confianza. Desde aquí, puede agregar frases de preguntas alternativas para una respuesta.

[!TIP]
> Estas características solo están disponibles para los bots de SDK de la versión 4 


## <a name="speech-recognition"></a>Reconocimiento de voz
Bot Framework Emulator admite el reconocimiento de voz a través de [Speech API de Cognitive Services](/azure/cognitive-services/Speech/home). Le permite emplear el bot habilitado por voz, o habilidad de Cortana, a través de la voz en el emulador durante el desarrollo. Bot Framework Emulator ofrece reconocimiento de voz sin cargo durante hasta tres horas por bot cada día. 

## <a id="ngrok"></a> Instalación y configuración de ngrok

Si usa Windows y ejecuta Bot Framework Emulator detrás de un firewall o de otro límite de red y quiere conectarse a un bot hospedado remotamente, debe instalar y configurar el software de tunelización **ngrok**. Bot Framework Emulator se integra estrechamente con el software de tunelización [ngrok][ngrokDownload] (desarrollado por [inconshreveable][inconshreveable]) y puede iniciarlo automáticamente cuando sea necesario.

Para instalar **ngrok** en Windows y configurar el emulador para que lo use, complete estos pasos: 

1. Descargue el ejecutable de [ngrok][ngrokDownload] en la máquina local.

2. Abra el cuadro de diálogo Configuración de aplicación del emulador, escriba la ruta de acceso a ngrok, seleccione si omitir o no ngrok para las direcciones locales y haga clic en **Guardar**.

![ruta de acceso a ngrok](media/emulator-v4/emulator-ngrok-path.png)

## <a name="additional-resources"></a>Recursos adicionales

Bot Framework Emulator es de código abierto. También puede [colaborar][EmulatorGithubContribute] en el desarrollo y [enviar errores y sugerencias][EmulatorGithubBugs].


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
