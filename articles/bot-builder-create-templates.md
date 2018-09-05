---
title: Creación de bots con las plantillas de Bot Builder
description: Las herramientas de Bot Builder le permiten administrar los recursos del bot directamente desde la línea de comandos
keywords: plantillas de botbuilder, node.js, python, java, .net, ludown, yeoman
author: matthewshim-ms
ms.author: v-shimma
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/25/2018
ms.openlocfilehash: 8004389aba58b5cf79f1559b3ce65d1d66c5358c
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928343"
---
# <a name="create-bots-with-botbuilder-templates"></a>Creación de bots con las plantillas de Bot Builder

> [!NOTE]
> Este tema se aplica a las versiones v3 y v4 del SDK. Consulte las notas adicionales siguientes.

Las plantillas ahora están disponibles para crear bots en cada una de las plataformas del SDK de Bot Builder: 

- Node.js
- Python
- Java
- .NET

## <a name="prerequisites"></a>Requisitos previos

Las plantillas de Python, Java y Node.js crean un bot de eco sencillo y todas están disponibles a través de [Yeoman](http://yeoman.io/).

- [Node.js](https://nodejs.org/en/) v 8.5 o superior
- [Yeoman](http://yeoman.io/)

Si aún no ha instalado Yeoman, debe instalarlo de forma global.

```shell
npm install -g yo
```

## <a name="nodejs-python-java-templates"></a>Plantillas de Node.js, Python y Java
Desde la línea de comandos, ejecute cd en una nueva carpeta de su elección. Hay dos versiones de plantillas de Node.js para Bot Builder disponibles para las versiones **v3** y **v4** del SDK, respectivamente. Las plantillas de Python y Java solo están disponibles para la v4. 

Para instalar el generador de plantillas para **Bot Builder v3 de Node.js**:

```shell
npm install generator-botbuilder
```

Para instalar el generador de plantillas para **Bot Builder v4 de Node.js**:
```shell
npm install generator-botbuilder@preview
```

Las plantillas de Python y Java **solo están disponibles para la v4**. 

Para **Python**:
```shell
npm install generator-botbuilder-python
```

Para **Java**:
```shell
npm install generator-botbuilder-java
```

Después de instalar un generador, puede ejecutar simplemente el comando **yo** en la CLI para ver una lista de generadores disponibles en Yeoman.

![Interfaz de Yeoman](media/botbuilder-templates/yeoman-generator-botbuilder.png)

Cambie al directorio de trabajo de su elección y seleccione el generador que usará. Se le pedirán varias opciones para crear el bot, como un nombre y una descripción. Una vez completados todos los mensajes, se creará la plantilla de bot de eco en el mismo directorio de trabajo.

![Plantilla de Yeoman para Node.js](media/botbuilder-templates/new-template-node.png)


## <a name="net"></a>.NET

Hay dos plantillas de .NET disponibles para las versiones **v3** y **v4** del SDK, respectivamente. Ambas están disponibles como paquetes [VSIX](https://docs.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package). Para descargarlos, haga clic en uno de los siguientes vínculos de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- [Plantilla de Bot Builder V3](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3)
- [Plantilla de Bot Builder V4](https://aka.ms/Ylcwxk)

#### <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2015 o superior](https://www.visualstudio.com/downloads/)
- [Cuenta de Azure](https://azure.microsoft.com/en-us/free/)

### <a name="install-the-templates"></a>Instalación de las plantillas

Desde el directorio de guardado, simplemente abra el paquete VSIX y la plantilla de Bot Builder se instalará en Visual Studio y estará disponible la próxima vez que la abra.

![Paquete VSIX](media/botbuilder-templates/botbuilder-vsix-template.png)

Para crear un nuevo proyecto de bot con la plantilla, simplemente abra Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto** y, desde Visual C#, seleccione **Bot Framework** > Simple Echo Bot Application (Aplicación de bot de eco sencilla). Esto creará localmente un nuevo proyecto de bot de eco que puede modificar como quiera. 

![Plantilla de bot de .NET](media/botbuilder-templates/new-template-dotnet.png)

## <a name="store-your-bot-information-with-msbot"></a>Almacenamiento de la información del bot con MSBot

La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una única ubicación. Este archivo también permite que el bot se conecte a estos servicios desde la CLI. La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:

```shell
npm install -g msbot 
```

Para crear un archivo bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:

```shell
msbot init --name TestBot --endpoint http://localhost:9499/api/messages
```
Para conectar el bot a un servicio, escriba **msbot connect** en la CLI, seguido del servicio adecuado:

```shell
msbot connect [Service]
```

| Servicio | DESCRIPCIÓN |
| ------ | ----------- |
| azure  |Conecta el bot a un registro de Azure Bot Service.|
|localhost| Conecta el bot a un punto de conexión de host local.|
|luis     | Conecta el bot a una aplicación de LUIS. |
| qna     |Conecta el bot a una base de conocimiento de QnA.|
|help [cmd]  |Muestra la ayuda para [cmd].|

### <a name="connect-your-bot-to-abs-with-the-bot-file"></a>Conexión del bot a ABS con el archivo .bot

Con la herramienta MSBot instalada, puede conectar fácilmente el bot a un grupo de recursos existente en Azure Bot Service si ejecuta el comando az bot **show**. 

```azurecli
az bot show -n <botname> -g <resourcegroup> --msbot | msbot connect azure --stdin
```

Esta operación tomará el punto de conexión, el identificador de la aplicación y la contraseña de MSA actuales del grupo de recursos de destino y actualizará la información correspondiente en el archivo .bot. 


## <a name="manage-update-or-create-luis-and-qna-services-with--new-botbuilder-tools"></a>Administración, actualización o creación de servicios de LUIS y QnA con las nuevas herramientas de Bot Builder

Las [herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) son un conjunto de herramientas nuevo que permite administrar los recursos de bot e interactuar con ellos directamente desde la línea de comandos. 

>[!TIP]
> Cada herramienta del Bot Builder incluye un comando de ayuda global al que se puede acceder desde la línea de comandos si se escribe **-h** o **--help**. Este comando está disponible en cualquier momento desde cualquier acción y le proporcionará una útil visualización de las opciones disponibles junto con sus descripciones. 

### <a name="ludown"></a>LUDown
[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) permite describir y crear componentes de lenguaje eficaces para los bots mediante archivos **.lu**. El nuevo archivo .lu es un tipo de formato Markdown que la herramienta LUDown usa para generar los archivos .json específicos del servicio de destino. Actualmente, puede usar los archivos .lu para crear una aplicación de [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app) o una base de conocimiento de [QnA](https://qnamaker.ai/Documentation/CreateKb) con formatos diferentes para cada una. LUDown está disponible como un módulo npm y se puede usar si se instala de forma global en la máquina:

```shell
npm install -g ludown
```
La herramienta LUDown se puede usar para crear modelos de .json para LUIS y QnA.  


### <a name="creating-a-luis-application-with-ludown"></a>Creación de una aplicación de LUIS con LUDown

Puede definir [intenciones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) y [entidades](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-entities) para una aplicación de LUIS, tal como lo haría desde el portal de LUIS. 

"#\<nombre-de-intención\>" describe la sección en que se define una nueva intención. Todas las líneas siguientes enumeran las [expresiones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-example-utterances) que describen dicha intención.

Por ejemplo, puede crear varias intenciones LUIS en un único archivo .lu de esta forma: 

```LUDown
# Greeting
- Hi
- Hello
- Good morning
- Good evening

# Help
- help
- I need help
- please help
```

### <a name="qna-pairs-with-ludown"></a>Pares de QnA con LUDown

El formato de archivo .lu también admite pares de QnA mediante la notación siguiente: 

```LUDown
> comment
### ? question ?
  ```markdown
    answer
  ```

La herramienta LUDown separará automáticamente la pregunta y las respuestas en un archivo JSON de QnA Maker que, después, se puede usar para crear una nueva base de conocimiento [QnaMaker.ai](http://qnamaker.ai).

```LUDown
### ? How do I change the default message for QnA Maker?
  ```markdown
  You can change the default message if you use the QnAMakerDialog. 
  See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
  ```

También se pueden agregar varias preguntas a la misma respuesta si se agregan nuevas líneas de variaciones de preguntas para una sola respuesta. 

```LUDown
### ? What is your name?
- What should I call you?
  ```markdown
    I'm the echoBot! Nice to meet you.
  ```

### <a name="generating-json-models-with-ludown"></a>Generación de modelos .json con LUDown

Después de definir los componentes del lenguaje de LUIS o QnA en formato .lu, puede publicarlos en un archivo .json de LUIS, o bien .json o .tsv de QnA. Cuando se ejecute, la herramienta LUDown buscará los archivos .lu en el mismo directorio de trabajo para analizarlos. Dado que la herramienta LUDown puede tener como destino LUIS o QnA con archivos .lu, solo hay que especificar para qué servicio de lenguaje se va a generar mediante el comando general **ludown parse <Service> --in <luFile>**. 

En el directorio de trabajo de ejemplo, hay dos archivos .lu para analizar: "1.lu", para crear el modelo de LUIS, y "qna1.lu", para crear una base de conocimiento de QnA.

#### <a name="generate-luis-json-models"></a>Generación de modelos .json para LUIS

Para generar un modelo de LUIS mediante LUDown, escriba lo siguiente en el directorio de trabajo actual:

```shell
ludown parse ToLuis --in <luFile> 
```

#### <a name="generate-qna-knowledge-base-json"></a>Generación del archivo .json de la base de conocimiento de QnA

De forma similar, para crear una base de conocimiento de QnA, solo hay que cambiar el destino del análisis. 

```shell
ludown parse ToQna --in <luFile> 
```

LUIS y QnA pueden usar los archivos JSON resultantes a través de sus portales correspondientes, o bien a través de las nuevas herramientas de la CLI. 

## <a name="connect-to-luis-an-qna-maker-services-from-the-cli"></a>Conexión a servicios de LUIS y QnA Maker desde la CLI

### <a name="connect-to-luis-from-the-cli"></a>Conexión a LUIS desde la CLI 

En el nuevo conjunto de herramientas se incluye una [extensión de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) que permite administrar de forma independiente los recursos de LUIS. Está disponible como módulo npm que se puede descargar de la siguiente manera:

```shell
npm install -g luis-apis
```
El uso de comandos básicos para la herramienta de LUIS desde la CLI es el siguiente:

```shell
luis <action> <resource> <args...>
```
Para conectar el bot a LUIS, deberá crear un archivo **.luisrc**. Se trata de un archivo de configuración que aprovisiona el identificador y la contraseña de la aplicación de LUIS para el punto de conexión de servicio cuando la aplicación realiza llamadas salientes. Puede crear este archivo mediante la ejecución de **luis init** como se muestra a continuación:

```shell
luis init
```
En el terminal, se le pedirá que escriba la clave de creación de LUIS, la región y el identificador de la aplicación antes de que la herramienta genere el archivo.  

![Comando init de LUIS](media/bot-builder-tools/luis-init.png) 


Una vez que se genere este archivo, la aplicación podrá utilizar el archivo .json de LUIS (generado desde LUDown) mediante el comando siguiente de la CLI.

```shell
luis import application --in luis-app.json | msbot connect luis --stdin
```

### <a name="connect-to-qna-from-the-cli"></a>Conexión a QnA desde la CLI

En el nuevo conjunto de herramientas se incluye una [extensión de QnA](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) que permite administrar de forma independiente los recursos de LUIS. Está disponible como módulo npm que se puede descargar.

```shell
npm install -g qnamaker
```
Con la herramienta QnA Maker, puede crear, actualizar, publicar, eliminar y entrenar la base de conocimiento. Para empezar, deberá crear un archivo **.qnamakerrc**, que es necesario para habilitar el punto de conexión al servicio. Puede crear fácilmente este archivo si ejecuta **qnamaker init** y sigue las indicaciones, además de aprovisionar el identificador de la base de conocimiento de QnA Maker. 

```shell
qnamaker init 
```
![Comando init de QnA Maker](media/bot-builder-tools/qnamaker-init.png)

Una vez que se genere el archivo .qnamakerrc, se puede conectar a la base de conocimiento de QnA para consumir el archivo .json/.tsv de la base de conocimiento (generado desde LUDown) mediante el comando siguiente.

```shell
qnamaker create --in qnaKB.json --msbot | msbot connect qna --stdin
```

## <a name="references"></a>Referencias
- [Código fuente de las herramientas de Bot Builder](https://github.com/Microsoft/botbuilder-tools)
- [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot)
- [ChatDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown)
- [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown)
- [CLI de Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
