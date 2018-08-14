---
title: Creación de bots con la CLI de Azure
description: Las herramientas de Bot Builder permiten administrar los recursos de bot directamente desde la línea de comandos
author: matthewshim-ms
ms.author: v-shimma
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 04/25/2018
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 20258949cd8ea403e5cc9bf774d6a3b7c1e86e7e
ms.sourcegitcommit: dcbc8ad992a3e242a11ebcdf0ee99714d919a877
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39352904"
---
# <a name="create-bots-with-azure-cli"></a>Creación de bots con la CLI de Azure

[Herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) es un conjunto de herramientas nuevo que permite administrar e interactuar con los recursos de bot directamente desde la línea de comandos. 

En este tutorial se mostrará cómo:

- Habilitar la extensión de bot de la CLI de Azure
- Crear un bot con la CLI de Azure 
- Descargar una copia local para el desarrollo
- Usar la nueva herramienta MSBot para almacenar toda la información de recursos de bot
- Administrar, crear o actualizar los modelos de LUIS y QnA con LUDown
- Conexión a servicios de LUIS y QnA Maker desde la CLI
- Implementar el bot en Azure desde la CLI

## <a name="prerequisites"></a>Requisitos previos

Para habilitar estas herramientas desde la línea de comandos, necesita Node.js instalado en el equipo: 

- [Node.js (v8.5 o superior)](https://nodejs.org/en/)

## <a name="1-enable-azure-cli"></a>1. Habilitación de la CLI de Azure

Ahora puede administrar los bots mediante la [CLI de Azure](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) como cualquier otro recurso de Azure. Para habilitar la CLI de Azure, siga los pasos siguientes:

1. [Descargue](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) la CLI de Azure si aún no la tiene. 

2. Escriba el comando siguiente para descargar el paquete de distribución Azure Bot Extension.

```azurecli
az extension add -n botservice
```

>[!TIP]
> En la actualidad, Azure Bot Extension solo admite los bots v3.
  
3. [Inicie sesión](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli?view=azure-cli-latest) en la CLI de Azure mediante la ejecución del comando siguiente.

```azurecli
az login
```
Se le pedirá un código de autenticación temporal único. Para el inicio de sesión, use un explorador web y visite [Inicio de sesión del dispositivo](https://microsoft.com/devicelogin) de Microsoft, y pegue el código proporcionado por la CLI para continuar. 

![Inicio de sesión del dispositivo de MS](media/bot-builder-tools/ms-device-login.png)

Después de iniciar sesión correctamente, verá la pantalla de bienvenida de la CLI de Azure, junto con una lista de las opciones disponibles para administrar las cuentas y los recursos.

![CLI de Azure Bot](media/bot-builder-tools/az-cli-bot.png)


 Para obtener una lista completa de los comandos de la CLI de Azure, [haga clic aquí](https://docs.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest).


## <a name="2-create-a-new-bot-from-azure-cli"></a>2. Creación de un bot desde la CLI de Azure

Con la CLI de Azure y la nueva extensión de bot, puede crear bots completamente desde la línea de comandos. 

```azurecli
az bot [command]
```
|Comandos:|  |
|----|----|
| create      |Agregar un recurso|
| delete     |Clonar un recurso|
| descargar   | Descargar el código fuente del bot|
| Publicar   |Publicar en un servicio de bot existente|
| show |Mostrar los recursos de bot existentes.|
| update| Actualizar un servicio de bot existente|

Para crear un bot desde la CLI, debe seleccionar un [grupo de recursos](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) existente, o bien crear uno. 

```azurecli
az bot create --resource-group "my-resource-group" --name "my-bot-name" --kind "my-resource-type" --description "description-of-my-bot"
```
Después de una solicitud correcta, verá el mensaje de confirmación.
```
obtained msa app id and password. Provisioning bot now.
```

> [!TIP]
> Si recibe un mensaje de error en el que se indica que **no se encontró el grupo de recursos**, es posible que deba establecer su [suscripción](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer) en la CLI de Azure. La suscripción de Azure debe coincidir con lo que se escribió al crear el grupo de recursos. Para establecerla, escriba lo siguiente:
> ```azurecli
> az account set --subscription "your-subscription-name"
> ```
> Para ver una lista de suscripciones para la cuenta, escriba el comando siguiente:
> ```azurecli
> az account list
> ```

De forma predeterminada, se crea un bot de .NET. Puede especificar el SDK de la plataforma si indica el lenguaje mediante el argumento **--lang**. En la actualidad, el paquete de extensión de bot es compatible con los SDK de C# y Node.js bot. Por ejemplo, para **crear un bot de Node.js**:

```azurecli
az bot create --resource-group "my-resource-group" --name "my-bot-name" --kind "my-resource-type" --description "description-of-my-bot" --lang Node 
```
El nuevo bot de eco se aprovisionará en el grupo de recursos en Azure; para probarlo simplemente haga clic en **Probar en chat en web** bajo el encabezado de administración de bots de la vista Bot de aplicación web. 

![Bot de eco de Azure](media/bot-builder-tools/az-echo-bot.png) 

## <a name="3-download-the-bot-locally"></a>3. Descarga local del bot

Hay dos maneras de descargar el código fuente del bot nuevo.
- Descargarlo desde Azure Portal.
- Descargarlo mediante la nueva CLI de Azure.

Para descargar el código fuente del bot desde el portal, simplemente seleccione el recurso de bot y haga clic en **Compilar** bajo la administración de bots. Hay varias opciones disponibles para administrar o recuperar el código fuente del bot de forma local. 

![Descarga del bot desde Azure Portal](media/bot-builder-tools/az-portal-manage-code.png)

Para descargar el código fuente del bot mediante la CLI, escriba el comando siguiente. El bot se descargará a un subdirectorio. Si todavía no existe el subdirectorio, el comando lo creará de forma automática.

```azurecli
az bot download --name "my-bot-name" --resource-group "my-resource-group"
```
Pero también puede especificar el directorio el que descargar el bot.
Por ejemplo: 

![Comando de descarga desde la CLI](media/bot-builder-tools/cli-bot-download-command.png)

![Descarga del bot desde la CLI](media/bot-builder-tools/cli-bot-download.png)

El comando anterior permite descargar el código fuente del bot directamente a la ubicación especificada, lo que permite desarrollarlo de forma local.


## <a name="4-store-your-bot-information-with-msbot"></a>4. Almacenamiento de la información de bot con MSBot

La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una única ubicación. Este archivo también permite que el bot se conecte a estos servicios desde la CLI. La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:

```shell
npm install -g msbot 
```

Para crear un archivo de bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:

```shell
msbot init --name name-of-my-bot --endpoint http://localhost:bot-port-number/api/messages
```
Para conectar el bot a un servicio, escriba **msbot connect** en la CLI, seguido del servicio adecuado:

```shell
msbot connect service-type
```

| Tipo de servicio | DESCRIPCIÓN |
| ------ | ----------- |
| azure  |Conectar el bot a un registro de Azure Bot Service|
|endpoint| Conectar el bot a un punto de conexión como localhost|
|luis     | Conectar el bot a una aplicación de LUIS |
| qna     |Conectar el bot a una base de conocimiento de QnA|
|help [cmd]  |Mostrar la Ayuda de [cmd]|

### <a name="connect-your-bot-to-abs-with-the-bot-file"></a>Conectar el bot a ABS con el archivo .bot

Con la herramienta MSBot instalada, puede conectar fácilmente el bot a un grupo de recursos existente en Azure Bot Service si ejecuta el comando az bot **show**. 

```azurecli
az bot show -n my-bot-name -g my-resource-group --msbot | msbot connect azure --stdin
```

Esta operación tomará el punto de conexión, Id. de la aplicación y contraseña de MSA actuales del grupo de recursos de destino y actualizará la información correspondiente en el archivo .bot. 


## <a name="5-manage-update-or-create-luis-and-qna-services-with--new-botbuilder-tools"></a>5. Administración, actualización o creación de servicios de LUIS y QnA con las nuevas herramientas de botbuilder

[Herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) es un conjunto de herramientas nuevo que permite administrar e interactuar con los recursos de bot directamente desde la línea de comandos. 

>[!TIP]
> Cada herramienta del generador de bots incluye un comando de ayuda global, accesible desde la línea de comandos si se escribe **-h** o **--help**. Este comando está disponible en cualquier momento desde cualquier acción, lo que proporcionará una representación útil de las opciones disponibles junto con sus descripciones.

### <a name="ludown"></a>LUDown
[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) permite describir y crear componentes de lenguaje eficaces para los bots mediante archivos **.lu**. El nuevo archivo .lu es un tipo de formato de marcado que usa la herramienta LUDown y genera los archivos .json específicos del servicio de destino. En la actualidad, puede usar los archivos .lu para crear una aplicación de [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app) o una base de conocimiento de [QnA](https://qnamaker.ai/Documentation/CreateKb), con formatos diferentes para cada una. LUDown está disponible como un módulo npm y se puede usar si se instala de forma global en el equipo:

```shell
npm install -g ludown
```
La herramienta LUDown se puede usar para crear modelos de .json para LUIS y QnA.  


### <a name="creating-a-luis-application-with-ludown"></a>Creación de una aplicación de LUIS con LUDown

Puede definir [intenciones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) y [entidades](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-entities) para una aplicación de LUIS, tal como lo haría desde el portal de LUIS. 

`# \<intent-name\>` describe la sección de definición de una intención nueva. Las líneas siguientes contienen [expresiones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-example-utterances) que describen esa intención.

Por ejemplo, puede crear varias intenciones LUIS en un único archivo .lu de esta forma: 

```ludown
# Greeting
Hi
Hello
Good morning
Good evening

# Help
help
I need help
please help
```

### <a name="qna-pairs-with-ludown"></a>Pares de QnA con LUDown

El formato de archivo .lu también admite pares de QnA mediante la notación siguiente: 

  ```ludown
  > This is a comment. QnA definitions have the general format:
  ### ? this-is-the-question-string
  - this-is-an-alternate-form-of-the-same-question
  - this-is-another-one
    ```markdown
    this-is-the-answer
    ```
  ```
La herramienta LUDown separará automáticamente la pregunta y las respuestas en un archivo JSON de QnA Maker que después se puede usar para crear la base de conocimiento de [QnaMaker.ai](http://qnamaker.ai).

  ```ludown
  ### ? How do I change the default message for QnA Maker?
    ```markdown
    You can change the default message if you use the QnAMakerDialog. 
    See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
    ```
  ```

También se pueden agregar varias preguntas a la misma respuesta si se agregan nuevas líneas de variaciones de preguntas para una sola respuesta. 

  ```ludown
  ### ? What is your name?
  - What should I call you?
    ```markdown
    I'm the echoBot! Nice to meet you.
    ```
  ```

### <a name="generating-json-models-with-ludown"></a>Generación de modelos .json con LUDown

Después de definir los componentes del lenguaje de LUIS o QnA en formato .lu, puede publicar en un archivo .json de LUIS, o bien un archivo .json o .tsv de QnA. Cuando se ejecuta, la herramienta LUDown buscará los archivos .lu en el mismo directorio de trabajo para analizarlos. Como la herramienta LUDown puede tener como destino archivos .lu de LUIS o QnA, solo hay que especificar para qué servicio de lenguaje se va generar, mediante el comando general **ludown parse <Service> --in <luFile>**. 

En el directorio de trabajo de ejemplo, hay dos archivos .lu para analizar: "luis-sample.lu" para crear el modelo de LUIS, y "qna-sample.lu" para crear una base de conocimiento de QnA.


#### <a name="generate-luis-json-models"></a>Generar modelos .json de LUIS

**luis-sample.lu** 
```ludown
# Greeting
- Hi
- Hello
- Good morning
- Good evening
```

Para generar un modelo de LUIS mediante LUDown, escriba lo siguiente en el directorio de trabajo actual:

```shell
ludown parse ToLuis --in ludown-file-name.lu
```

#### <a name="generate-qna-knowledge-base-json"></a>Generar el archivo .json de base de conocimiento de QnA

**qna-sample.lu**
  ```ludown
  > This is a sample ludown file for QnA Maker.

  ### ? How do I change the default message
    ```markdown
    You can change the default message if you use the QnAMakerDialog. 
    See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
    ```
  ```

De forma similar, para crear una base de conocimiento de QnA, solo hay que cambiar el destino del análisis. 

```shell
ludown parse ToQna --in ludown-file-name.lu
```

Los archivos JSON resultantes se pueden consumir mediante LUIS y QnA a través de sus portales correspondientes, o bien a través de las nuevas herramientas de la CLI. 

## <a name="6-connect-to-luis-an-qna-maker-services-from-the-cli"></a>6. Conexión a servicios de LUIS y QnA Maker desde la CLI

### <a name="connect-to-luis-from-the-cli"></a>Conectarse a LUIS desde la CLI 

En el nuevo conjunto de herramientas se incluye una [extensión LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) que permite administrar de forma independiente los recursos de LUIS. Está disponible como módulo npm que se puede descargar:

```shell
npm install -g luis-apis
```
El uso de comandos básicos para la herramienta de LUIS desde la CLI es el siguiente:

```shell
luis action-name resource-name arguments-list
```
Para conectar el bot a LUIS, deberá crear un archivo **.luisrc**. Se trata de un archivo de configuración que aprovisiona el identificador y la contraseña de la aplicación de LUIS en el punto de conexión de servicio cuando la aplicación realiza llamadas salientes. Puede crear este archivo mediante la ejecución de **luis init** como sigue:

```shell
luis init
```
En el terminal, se le pedirá que escriba la clave de creación, la región y el Id. de aplicación de LUIS antes de que la herramienta genere el archivo.  

![Comando init de LUIS](media/bot-builder-tools/luis-init.png) 


Una vez generado este archivo, la aplicación podrá consumir el archivo .json de LUIS (generado desde LUDown) mediante el comando siguiente desde la CLI: 

```shell
luis import application --in luis-app.json | msbot connect luis --stdin
```

### <a name="connect-to-qna-from-the-cli"></a>Conectarse a QnA desde la CLI

En el nuevo conjunto de herramientas se incluye una [extensión QnA](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) que permite administrar de forma independiente los recursos de LUIS. Está disponible como módulo npm que se puede descargar:

```shell
npm install -g qnamaker
```
Con la herramienta QnA Maker, puede crear, actualizar, publicar, eliminar y entrenar la base de conocimiento. Para empezar, deberá crear un archivo **.qnamakerrc**, necesario para habilitar el punto de conexión al servicio. Puede crear fácilmente este archivo si ejecuta **qnamaker init** y sigue las indicaciones, junto con el identificador de la base de conocimiento de QnA Maker. 

```shell
qnamaker init 
```
![Archivo init de QnaMaker](media/bot-builder-tools/qnamaker-init.png)

Una vez generado el archivo .qnamakerrc, se puede conectar a la base de conocimiento de QnA para consumir el archivo de base de conocimiento .json o .tsv (generado desde LUDown) mediante el comando siguiente:

```shell
qnamaker create --in qnaKB.json --msbot | msbot connect qna --stdin
```

## <a name="7-publish-to-azure-from-the-cli"></a>7. Publicación en Azure desde la CLI

Después de realizar cambios en el código fuente del bot, puede publicarlos sin problemas si ejecuta lo siguiente:

```azurecli
az bot publish --name "my-bot-name" --resource-group "my-resource-group"
```

## <a name="references"></a>Referencias
- [Código fuente de las herramientas de BotBuilder](https://github.com/Microsoft/botbuilder-tools)
- [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot)
- [ChatDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown)
- [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/ludown)
- [CLI de Azure](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)


