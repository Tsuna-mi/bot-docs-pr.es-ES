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
# <a name="create-bots-with-botbuilder-templates"></a><span data-ttu-id="95837-104">Creación de bots con las plantillas de Bot Builder</span><span class="sxs-lookup"><span data-stu-id="95837-104">Create bots with Botbuilder Templates</span></span>

> [!NOTE]
> <span data-ttu-id="95837-105">Este tema se aplica a las versiones v3 y v4 del SDK.</span><span class="sxs-lookup"><span data-stu-id="95837-105">This topics applies to v3 and v4 version of the SDK.</span></span> <span data-ttu-id="95837-106">Consulte las notas adicionales siguientes.</span><span class="sxs-lookup"><span data-stu-id="95837-106">See additional notes below.</span></span>

<span data-ttu-id="95837-107">Las plantillas ahora están disponibles para crear bots en cada una de las plataformas del SDK de Bot Builder:</span><span class="sxs-lookup"><span data-stu-id="95837-107">Templates are now available to create bots in each of the Botbuilder SDK platforms:</span></span> 

- <span data-ttu-id="95837-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="95837-108">Node.js</span></span>
- <span data-ttu-id="95837-109">Python</span><span class="sxs-lookup"><span data-stu-id="95837-109">Python</span></span>
- <span data-ttu-id="95837-110">Java</span><span class="sxs-lookup"><span data-stu-id="95837-110">Java</span></span>
- <span data-ttu-id="95837-111">.NET</span><span class="sxs-lookup"><span data-stu-id="95837-111">.NET</span></span>

## <a name="prerequisites"></a><span data-ttu-id="95837-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="95837-112">Prerequisites</span></span>

<span data-ttu-id="95837-113">Las plantillas de Python, Java y Node.js crean un bot de eco sencillo y todas están disponibles a través de [Yeoman](http://yeoman.io/).</span><span class="sxs-lookup"><span data-stu-id="95837-113">The Node.js, Java and Python templates all create a simple echo bot, and are all made available through [Yeoman](http://yeoman.io/).</span></span>

- <span data-ttu-id="95837-114">[Node.js](https://nodejs.org/en/) v 8.5 o superior</span><span class="sxs-lookup"><span data-stu-id="95837-114">[Node.js](https://nodejs.org/en/) v 8.5 or greater</span></span>
- [<span data-ttu-id="95837-115">Yeoman</span><span class="sxs-lookup"><span data-stu-id="95837-115">Yeoman</span></span>](http://yeoman.io/)

<span data-ttu-id="95837-116">Si aún no ha instalado Yeoman, debe instalarlo de forma global.</span><span class="sxs-lookup"><span data-stu-id="95837-116">If you haven't already installed Yeoman, it needs to be installed globally.</span></span>

```shell
npm install -g yo
```

## <a name="nodejs-python-java-templates"></a><span data-ttu-id="95837-117">Plantillas de Node.js, Python y Java</span><span class="sxs-lookup"><span data-stu-id="95837-117">Node.js, Python, Java templates</span></span>
<span data-ttu-id="95837-118">Desde la línea de comandos, ejecute cd en una nueva carpeta de su elección.</span><span class="sxs-lookup"><span data-stu-id="95837-118">From the command line, cd into a new folder of your choosing.</span></span> <span data-ttu-id="95837-119">Hay dos versiones de plantillas de Node.js para Bot Builder disponibles para las versiones **v3** y **v4** del SDK, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="95837-119">There are two available versions of the Botbuilder Node.js templates, targetting the **v3** and **v4** versions of the SDK respectively.</span></span> <span data-ttu-id="95837-120">Las plantillas de Python y Java solo están disponibles para la v4.</span><span class="sxs-lookup"><span data-stu-id="95837-120">The Python and Java templates are only available in v4.</span></span> 

<span data-ttu-id="95837-121">Para instalar el generador de plantillas para **Bot Builder v3 de Node.js**:</span><span class="sxs-lookup"><span data-stu-id="95837-121">To install the **Node.js v3 Botbuilder** template generator:</span></span>

```shell
npm install generator-botbuilder
```

<span data-ttu-id="95837-122">Para instalar el generador de plantillas para **Bot Builder v4 de Node.js**:</span><span class="sxs-lookup"><span data-stu-id="95837-122">To install the **Node.js v4 Botbuilder** template generator:</span></span>
```shell
npm install generator-botbuilder@preview
```

<span data-ttu-id="95837-123">Las plantillas de Python y Java **solo están disponibles para la v4**.</span><span class="sxs-lookup"><span data-stu-id="95837-123">The Python and Java templates are **only available in v4**.</span></span> 

<span data-ttu-id="95837-124">Para **Python**:</span><span class="sxs-lookup"><span data-stu-id="95837-124">For **Python**:</span></span>
```shell
npm install generator-botbuilder-python
```

<span data-ttu-id="95837-125">Para **Java**:</span><span class="sxs-lookup"><span data-stu-id="95837-125">For **Java**:</span></span>
```shell
npm install generator-botbuilder-java
```

<span data-ttu-id="95837-126">Después de instalar un generador, puede ejecutar simplemente el comando **yo** en la CLI para ver una lista de generadores disponibles en Yeoman.</span><span class="sxs-lookup"><span data-stu-id="95837-126">After a generator is installed, you can simply run the **yo** command in your CLI to view a list of generators available in Yeoman.</span></span>

![Interfaz de Yeoman](media/botbuilder-templates/yeoman-generator-botbuilder.png)

<span data-ttu-id="95837-128">Cambie al directorio de trabajo de su elección y seleccione el generador que usará.</span><span class="sxs-lookup"><span data-stu-id="95837-128">Switch to a working directory of your choice, and select the generator to use.</span></span> <span data-ttu-id="95837-129">Se le pedirán varias opciones para crear el bot, como un nombre y una descripción.</span><span class="sxs-lookup"><span data-stu-id="95837-129">You will be prompted for various options to create your bot, such as name and description.</span></span> <span data-ttu-id="95837-130">Una vez completados todos los mensajes, se creará la plantilla de bot de eco en el mismo directorio de trabajo.</span><span class="sxs-lookup"><span data-stu-id="95837-130">When all prompts are completed, your echo bot template will be created in the same working folder.</span></span>

![Plantilla de Yeoman para Node.js](media/botbuilder-templates/new-template-node.png)


## <a name="net"></a><span data-ttu-id="95837-132">.NET</span><span class="sxs-lookup"><span data-stu-id="95837-132">.NET</span></span>

<span data-ttu-id="95837-133">Hay dos plantillas de .NET disponibles para las versiones **v3** y **v4** del SDK, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="95837-133">Two templates are available for .NET, targetting the **v3** and **v4** versions of the SDK respectively.</span></span> <span data-ttu-id="95837-134">Ambas están disponibles como paquetes [VSIX](https://docs.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package). Para descargarlos, haga clic en uno de los siguientes vínculos de [Visual Studio Marketplace](https://marketplace.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="95837-134">Both are available as [VSIX](https://docs.microsoft.com/en-us/visualstudio/extensibility/anatomy-of-a-vsix-package) packages,  to download click one of the following links on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/).</span></span>

- [<span data-ttu-id="95837-135">Plantilla de Bot Builder V3</span><span class="sxs-lookup"><span data-stu-id="95837-135">BotBuilder V3 Template</span></span>](https://marketplace.visualstudio.com/items?itemName=BotBuilder.BotBuilderV3)
- [<span data-ttu-id="95837-136">Plantilla de Bot Builder V4</span><span class="sxs-lookup"><span data-stu-id="95837-136">BotBuilder V4 Template</span></span>](https://aka.ms/Ylcwxk)

#### <a name="prerequisites"></a><span data-ttu-id="95837-137">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="95837-137">Prerequisites</span></span>

- [<span data-ttu-id="95837-138">Visual Studio 2015 o superior</span><span class="sxs-lookup"><span data-stu-id="95837-138">Visual Studio 2015 or greater</span></span>](https://www.visualstudio.com/downloads/)
- [<span data-ttu-id="95837-139">Cuenta de Azure</span><span class="sxs-lookup"><span data-stu-id="95837-139">Azure account</span></span>](https://azure.microsoft.com/en-us/free/)

### <a name="install-the-templates"></a><span data-ttu-id="95837-140">Instalación de las plantillas</span><span class="sxs-lookup"><span data-stu-id="95837-140">Install the templates</span></span>

<span data-ttu-id="95837-141">Desde el directorio de guardado, simplemente abra el paquete VSIX y la plantilla de Bot Builder se instalará en Visual Studio y estará disponible la próxima vez que la abra.</span><span class="sxs-lookup"><span data-stu-id="95837-141">From the saved directory, simply open the VSIX package and the bot builder template will be installed to Visual Studio and made available the next time you open it.</span></span>

![Paquete VSIX](media/botbuilder-templates/botbuilder-vsix-template.png)

<span data-ttu-id="95837-143">Para crear un nuevo proyecto de bot con la plantilla, simplemente abra Visual Studio y seleccione **Archivo** > **Nuevo** > **Proyecto** y, desde Visual C#, seleccione **Bot Framework** > Simple Echo Bot Application (Aplicación de bot de eco sencilla).</span><span class="sxs-lookup"><span data-stu-id="95837-143">To create a new bot project using the template, simply open Visual Studio, and select **File** > **new** > **Project**, and from Visual C#, select **Bot Framework** > Simple Echo Bot Application.</span></span> <span data-ttu-id="95837-144">Esto creará localmente un nuevo proyecto de bot de eco que puede modificar como quiera.</span><span class="sxs-lookup"><span data-stu-id="95837-144">This will create a new echo bot project locally which you can edit as you wish.</span></span> 

![Plantilla de bot de .NET](media/botbuilder-templates/new-template-dotnet.png)

## <a name="store-your-bot-information-with-msbot"></a><span data-ttu-id="95837-146">Almacenamiento de la información del bot con MSBot</span><span class="sxs-lookup"><span data-stu-id="95837-146">Store your bot information with MSBot</span></span>

<span data-ttu-id="95837-147">La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una única ubicación.</span><span class="sxs-lookup"><span data-stu-id="95837-147">The new [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) tool allows you to create a **.bot** file, which stores metadata about different services your bot consumes, all in one location.</span></span> <span data-ttu-id="95837-148">Este archivo también permite que el bot se conecte a estos servicios desde la CLI.</span><span class="sxs-lookup"><span data-stu-id="95837-148">This file also enables your bot to connect to these services from the CLI.</span></span> <span data-ttu-id="95837-149">La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="95837-149">The tool is available as an npm module, to install it run:</span></span>

```shell
npm install -g msbot 
```

<span data-ttu-id="95837-150">Para crear un archivo bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="95837-150">To create a bot file, from your CLI enter **msbot init** followed by the name of your bot, and the target URL endpoint, for example:</span></span>

```shell
msbot init --name TestBot --endpoint http://localhost:9499/api/messages
```
<span data-ttu-id="95837-151">Para conectar el bot a un servicio, escriba **msbot connect** en la CLI, seguido del servicio adecuado:</span><span class="sxs-lookup"><span data-stu-id="95837-151">To connect your bot to a service, in your CLI enter **msbot connect** followed by the appropriate service:</span></span>

```shell
msbot connect [Service]
```

| <span data-ttu-id="95837-152">Servicio</span><span class="sxs-lookup"><span data-stu-id="95837-152">Service</span></span> | <span data-ttu-id="95837-153">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="95837-153">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="95837-154">azure</span><span class="sxs-lookup"><span data-stu-id="95837-154">azure</span></span>  |<span data-ttu-id="95837-155">Conecta el bot a un registro de Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="95837-155">connect your bot to an Azure Bot Service registration</span></span>|
|<span data-ttu-id="95837-156">localhost</span><span class="sxs-lookup"><span data-stu-id="95837-156">localhost</span></span>| <span data-ttu-id="95837-157">Conecta el bot a un punto de conexión de host local.</span><span class="sxs-lookup"><span data-stu-id="95837-157">connect your bot to a localhost endpoint</span></span>|
|<span data-ttu-id="95837-158">luis</span><span class="sxs-lookup"><span data-stu-id="95837-158">luis</span></span>     | <span data-ttu-id="95837-159">Conecta el bot a una aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="95837-159">connect your bot to a LUIS application</span></span> |
| <span data-ttu-id="95837-160">qna</span><span class="sxs-lookup"><span data-stu-id="95837-160">qna</span></span>     |<span data-ttu-id="95837-161">Conecta el bot a una base de conocimiento de QnA.</span><span class="sxs-lookup"><span data-stu-id="95837-161">connect your bot to a QnA Knowledgebase</span></span>|
|<span data-ttu-id="95837-162">help [cmd]</span><span class="sxs-lookup"><span data-stu-id="95837-162">help [cmd]</span></span>  |<span data-ttu-id="95837-163">Muestra la ayuda para [cmd].</span><span class="sxs-lookup"><span data-stu-id="95837-163">display help for [cmd]</span></span>|

### <a name="connect-your-bot-to-abs-with-the-bot-file"></a><span data-ttu-id="95837-164">Conexión del bot a ABS con el archivo .bot</span><span class="sxs-lookup"><span data-stu-id="95837-164">Connect your bot to ABS with the .bot file</span></span>

<span data-ttu-id="95837-165">Con la herramienta MSBot instalada, puede conectar fácilmente el bot a un grupo de recursos existente en Azure Bot Service si ejecuta el comando az bot **show**.</span><span class="sxs-lookup"><span data-stu-id="95837-165">With the MSBot tool installed, you can easily connect your bot to an existing resource group in the Azure Bot Service by running the az bot **show** command.</span></span> 

```azurecli
az bot show -n <botname> -g <resourcegroup> --msbot | msbot connect azure --stdin
```

<span data-ttu-id="95837-166">Esta operación tomará el punto de conexión, el identificador de la aplicación y la contraseña de MSA actuales del grupo de recursos de destino y actualizará la información correspondiente en el archivo .bot.</span><span class="sxs-lookup"><span data-stu-id="95837-166">This will take the current endpoint, MSA appID and password from the target resource group, and update the information accordingly in your .bot file.</span></span> 


## <a name="manage-update-or-create-luis-and-qna-services-with--new-botbuilder-tools"></a><span data-ttu-id="95837-167">Administración, actualización o creación de servicios de LUIS y QnA con las nuevas herramientas de Bot Builder</span><span class="sxs-lookup"><span data-stu-id="95837-167">Manage, Update or Create LUIS and QnA services with  new botbuilder-tools</span></span>

<span data-ttu-id="95837-168">Las [herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) son un conjunto de herramientas nuevo que permite administrar los recursos de bot e interactuar con ellos directamente desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="95837-168">[Bot builder tools](https://github.com/microsoft/botbuilder-tools) is a new toolset which allows you to manage and interact with your bot resources directly from the command line.</span></span> 

>[!TIP]
> <span data-ttu-id="95837-169">Cada herramienta del Bot Builder incluye un comando de ayuda global al que se puede acceder desde la línea de comandos si se escribe **-h** o **--help**.</span><span class="sxs-lookup"><span data-stu-id="95837-169">Every bot builder tool includes a global help command, accessible from the command line by entering **-h** or **--help**.</span></span> <span data-ttu-id="95837-170">Este comando está disponible en cualquier momento desde cualquier acción y le proporcionará una útil visualización de las opciones disponibles junto con sus descripciones.</span><span class="sxs-lookup"><span data-stu-id="95837-170">This command is available at any time from any action, which will provide a helpful display of the options available to you along with their descriptions</span></span> 

### <a name="ludown"></a><span data-ttu-id="95837-171">LUDown</span><span class="sxs-lookup"><span data-stu-id="95837-171">LUDown</span></span>
<span data-ttu-id="95837-172">[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) permite describir y crear componentes de lenguaje eficaces para los bots mediante archivos **.lu**.</span><span class="sxs-lookup"><span data-stu-id="95837-172">[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) allows you to describe and create powerful language components for bots using **.lu** files.</span></span> <span data-ttu-id="95837-173">El nuevo archivo .lu es un tipo de formato Markdown que la herramienta LUDown usa para generar los archivos .json específicos del servicio de destino.</span><span class="sxs-lookup"><span data-stu-id="95837-173">The new .lu file is a type of markdown format which the LUDown tool consumes and outputs .json files specific to the target service.</span></span> <span data-ttu-id="95837-174">Actualmente, puede usar los archivos .lu para crear una aplicación de [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app) o una base de conocimiento de [QnA](https://qnamaker.ai/Documentation/CreateKb) con formatos diferentes para cada una.</span><span class="sxs-lookup"><span data-stu-id="95837-174">Currently, you can use .lu files to create a new [LUIS](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app) application or [QnA](https://qnamaker.ai/Documentation/CreateKb) knowledge base, using different formats for each.</span></span> <span data-ttu-id="95837-175">LUDown está disponible como un módulo npm y se puede usar si se instala de forma global en la máquina:</span><span class="sxs-lookup"><span data-stu-id="95837-175">LUDown is available as an npm module, and can be used by installing globally to your machine:</span></span>

```shell
npm install -g ludown
```
<span data-ttu-id="95837-176">La herramienta LUDown se puede usar para crear modelos de .json para LUIS y QnA.</span><span class="sxs-lookup"><span data-stu-id="95837-176">The LUDown tool can be used to create new .json models for both LUIS and QnA.</span></span>  


### <a name="creating-a-luis-application-with-ludown"></a><span data-ttu-id="95837-177">Creación de una aplicación de LUIS con LUDown</span><span class="sxs-lookup"><span data-stu-id="95837-177">Creating a LUIS application with LUDown</span></span>

<span data-ttu-id="95837-178">Puede definir [intenciones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) y [entidades](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-entities) para una aplicación de LUIS, tal como lo haría desde el portal de LUIS.</span><span class="sxs-lookup"><span data-stu-id="95837-178">You can define [intents](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) and [entities](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-entities) for a LUIS application just like you would from the LUIS portal.</span></span> 

<span data-ttu-id="95837-179">"#\<nombre-de-intención\>" describe la sección en que se define una nueva intención.</span><span class="sxs-lookup"><span data-stu-id="95837-179">'#\<intent-name\>' describes a new intent definition section.</span></span> <span data-ttu-id="95837-180">Todas las líneas siguientes enumeran las [expresiones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-example-utterances) que describen dicha intención.</span><span class="sxs-lookup"><span data-stu-id="95837-180">Each line afterwards lists the [utterances](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-example-utterances) which describe that intent.</span></span>

<span data-ttu-id="95837-181">Por ejemplo, puede crear varias intenciones LUIS en un único archivo .lu de esta forma:</span><span class="sxs-lookup"><span data-stu-id="95837-181">For example, you can create multiple LUIS intents in a single .lu file as follows:</span></span> 

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

### <a name="qna-pairs-with-ludown"></a><span data-ttu-id="95837-182">Pares de QnA con LUDown</span><span class="sxs-lookup"><span data-stu-id="95837-182">QnA pairs with LUDown</span></span>

<span data-ttu-id="95837-183">El formato de archivo .lu también admite pares de QnA mediante la notación siguiente:</span><span class="sxs-lookup"><span data-stu-id="95837-183">The .lu file format supports also QnA pairs using the following notation:</span></span> 

```LUDown
> comment
### ? question ?
  ```markdown
    answer
  ```

<span data-ttu-id="95837-184">La herramienta LUDown separará automáticamente la pregunta y las respuestas en un archivo JSON de QnA Maker que, después, se puede usar para crear una nueva base de conocimiento [QnaMaker.ai](http://qnamaker.ai).</span><span class="sxs-lookup"><span data-stu-id="95837-184">The LUDown tool will automatically separate question and answers into a qnamaker JSON file that you can then use to create your new [QnaMaker.ai](http://qnamaker.ai) knowledge base.</span></span>

```LUDown
### ? How do I change the default message for QnA Maker?
  ```markdown
  You can change the default message if you use the QnAMakerDialog. 
  See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
  ```

<span data-ttu-id="95837-185">También se pueden agregar varias preguntas a la misma respuesta si se agregan nuevas líneas de variaciones de preguntas para una sola respuesta.</span><span class="sxs-lookup"><span data-stu-id="95837-185">You can also add multiple questions to the same answer by simply adding new lines of variations of questions for a single answer.</span></span> 

```LUDown
### ? What is your name?
- What should I call you?
  ```markdown
    I'm the echoBot! Nice to meet you.
  ```

### <a name="generating-json-models-with-ludown"></a><span data-ttu-id="95837-186">Generación de modelos .json con LUDown</span><span class="sxs-lookup"><span data-stu-id="95837-186">Generating .json models with LUDown</span></span>

<span data-ttu-id="95837-187">Después de definir los componentes del lenguaje de LUIS o QnA en formato .lu, puede publicarlos en un archivo .json de LUIS, o bien .json o .tsv de QnA.</span><span class="sxs-lookup"><span data-stu-id="95837-187">After you've defined LUIS or QnA language components in the .lu format, you can publish out to a LUIS .json, QnA .json, or QnA .tsv file.</span></span> <span data-ttu-id="95837-188">Cuando se ejecute, la herramienta LUDown buscará los archivos .lu en el mismo directorio de trabajo para analizarlos.</span><span class="sxs-lookup"><span data-stu-id="95837-188">When run, the LUDown tool will look for any .lu files within the same working directory to parse.</span></span> <span data-ttu-id="95837-189">Dado que la herramienta LUDown puede tener como destino LUIS o QnA con archivos .lu, solo hay que especificar para qué servicio de lenguaje se va a generar mediante el comando general **ludown parse <Service> --in <luFile>**.</span><span class="sxs-lookup"><span data-stu-id="95837-189">Since the LUDown tool can target both LUIS or QnA with .lu files, we simply need to specify which language service to generate for, using the general command **ludown parse <Service> -- in <luFile>**.</span></span> 

<span data-ttu-id="95837-190">En el directorio de trabajo de ejemplo, hay dos archivos .lu para analizar: "1.lu", para crear el modelo de LUIS, y "qna1.lu", para crear una base de conocimiento de QnA.</span><span class="sxs-lookup"><span data-stu-id="95837-190">In our sample working directory, we have two .lu files to parse, '1.lu' to create LUIS model, and 'qna1.lu' to create a QnA knowledge base.</span></span>

#### <a name="generate-luis-json-models"></a><span data-ttu-id="95837-191">Generación de modelos .json para LUIS</span><span class="sxs-lookup"><span data-stu-id="95837-191">Generate LUIS .json models</span></span>

<span data-ttu-id="95837-192">Para generar un modelo de LUIS mediante LUDown, escriba lo siguiente en el directorio de trabajo actual:</span><span class="sxs-lookup"><span data-stu-id="95837-192">To generate a LUIS model using LUDown, in your current working directory simply enter the following:</span></span>

```shell
ludown parse ToLuis --in <luFile> 
```

#### <a name="generate-qna-knowledge-base-json"></a><span data-ttu-id="95837-193">Generación del archivo .json de la base de conocimiento de QnA</span><span class="sxs-lookup"><span data-stu-id="95837-193">Generate QnA Knowledge Base .json</span></span>

<span data-ttu-id="95837-194">De forma similar, para crear una base de conocimiento de QnA, solo hay que cambiar el destino del análisis.</span><span class="sxs-lookup"><span data-stu-id="95837-194">Similarly, to create a QnA knowledge base, you only need to change the parse target.</span></span> 

```shell
ludown parse ToQna --in <luFile> 
```

<span data-ttu-id="95837-195">LUIS y QnA pueden usar los archivos JSON resultantes a través de sus portales correspondientes, o bien a través de las nuevas herramientas de la CLI.</span><span class="sxs-lookup"><span data-stu-id="95837-195">The resulting JSON files can be consumed by LUIS and QnA either through their respective portals, or via the new CLI tools.</span></span> 

## <a name="connect-to-luis-an-qna-maker-services-from-the-cli"></a><span data-ttu-id="95837-196">Conexión a servicios de LUIS y QnA Maker desde la CLI</span><span class="sxs-lookup"><span data-stu-id="95837-196">Connect to LUIS an QnA maker services from the CLI</span></span>

### <a name="connect-to-luis-from-the-cli"></a><span data-ttu-id="95837-197">Conexión a LUIS desde la CLI</span><span class="sxs-lookup"><span data-stu-id="95837-197">Connect to LUIS from the CLI</span></span> 

<span data-ttu-id="95837-198">En el nuevo conjunto de herramientas se incluye una [extensión de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) que permite administrar de forma independiente los recursos de LUIS.</span><span class="sxs-lookup"><span data-stu-id="95837-198">Included in the new tool set is a [LUIS extension](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) which allows you to independently manage your LUIS resources.</span></span> <span data-ttu-id="95837-199">Está disponible como módulo npm que se puede descargar de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="95837-199">It is available as an npm module which you can download:</span></span>

```shell
npm install -g luis-apis
```
<span data-ttu-id="95837-200">El uso de comandos básicos para la herramienta de LUIS desde la CLI es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="95837-200">The basic command usage for the LUIS tool from the CLI is:</span></span>

```shell
luis <action> <resource> <args...>
```
<span data-ttu-id="95837-201">Para conectar el bot a LUIS, deberá crear un archivo **.luisrc**.</span><span class="sxs-lookup"><span data-stu-id="95837-201">To connect your bot to LUIS, you will need to create a **.luisrc** file.</span></span> <span data-ttu-id="95837-202">Se trata de un archivo de configuración que aprovisiona el identificador y la contraseña de la aplicación de LUIS para el punto de conexión de servicio cuando la aplicación realiza llamadas salientes.</span><span class="sxs-lookup"><span data-stu-id="95837-202">This is a configuration file which provisions your LUIS appID and password to the service endpoint when your application makes outbound calls.</span></span> <span data-ttu-id="95837-203">Puede crear este archivo mediante la ejecución de **luis init** como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="95837-203">You can create this file by running **luis init** as follows:</span></span>

```shell
luis init
```
<span data-ttu-id="95837-204">En el terminal, se le pedirá que escriba la clave de creación de LUIS, la región y el identificador de la aplicación antes de que la herramienta genere el archivo.</span><span class="sxs-lookup"><span data-stu-id="95837-204">You will be prompted in the terminal to enter your LUIS authoring key, region, and appID before the tool will generate the file.</span></span>  

![Comando init de LUIS](media/bot-builder-tools/luis-init.png) 


<span data-ttu-id="95837-206">Una vez que se genere este archivo, la aplicación podrá utilizar el archivo .json de LUIS (generado desde LUDown) mediante el comando siguiente de la CLI.</span><span class="sxs-lookup"><span data-stu-id="95837-206">Once this file is generated, your application will be able to consume the LUIS .json file (generated from LUDown) using the following command from the CLI.</span></span>

```shell
luis import application --in luis-app.json | msbot connect luis --stdin
```

### <a name="connect-to-qna-from-the-cli"></a><span data-ttu-id="95837-207">Conexión a QnA desde la CLI</span><span class="sxs-lookup"><span data-stu-id="95837-207">Connect to QnA from the CLI</span></span>

<span data-ttu-id="95837-208">En el nuevo conjunto de herramientas se incluye una [extensión de QnA](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) que permite administrar de forma independiente los recursos de LUIS.</span><span class="sxs-lookup"><span data-stu-id="95837-208">Included in the new tool set is a [QnA extension](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) which allows you to independently manage your LUIS resources.</span></span> <span data-ttu-id="95837-209">Está disponible como módulo npm que se puede descargar.</span><span class="sxs-lookup"><span data-stu-id="95837-209">It is available as an npm module which you can download.</span></span>

```shell
npm install -g qnamaker
```
<span data-ttu-id="95837-210">Con la herramienta QnA Maker, puede crear, actualizar, publicar, eliminar y entrenar la base de conocimiento.</span><span class="sxs-lookup"><span data-stu-id="95837-210">With the QnA maker tool, you can create, update, publish, delete, and train your knowledge base.</span></span> <span data-ttu-id="95837-211">Para empezar, deberá crear un archivo **.qnamakerrc**, que es necesario para habilitar el punto de conexión al servicio.</span><span class="sxs-lookup"><span data-stu-id="95837-211">To get started, you need to create a **.qnamakerrc** file is required to enable the endpoint to your service.</span></span> <span data-ttu-id="95837-212">Puede crear fácilmente este archivo si ejecuta **qnamaker init** y sigue las indicaciones, además de aprovisionar el identificador de la base de conocimiento de QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="95837-212">You can easily create this file by running **qnamaker init** and following the prompts and provision your QnA Maker knowledge base ID.</span></span> 

```shell
qnamaker init 
```
![Comando init de QnA Maker](media/bot-builder-tools/qnamaker-init.png)

<span data-ttu-id="95837-214">Una vez que se genere el archivo .qnamakerrc, se puede conectar a la base de conocimiento de QnA para consumir el archivo .json/.tsv de la base de conocimiento (generado desde LUDown) mediante el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="95837-214">Once your .qnamakerrc file is generated, you can now connect to your QnA knowledge base to consume the knowledge base .json/.tsv file (generated from LUDown) using the following command.</span></span>

```shell
qnamaker create --in qnaKB.json --msbot | msbot connect qna --stdin
```

## <a name="references"></a><span data-ttu-id="95837-215">Referencias</span><span class="sxs-lookup"><span data-stu-id="95837-215">References</span></span>
- [<span data-ttu-id="95837-216">Código fuente de las herramientas de Bot Builder</span><span class="sxs-lookup"><span data-stu-id="95837-216">BotBuilder Tools Source Code</span></span>](https://github.com/Microsoft/botbuilder-tools)
- [<span data-ttu-id="95837-217">MSBot</span><span class="sxs-lookup"><span data-stu-id="95837-217">MSBot</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot)
- [<span data-ttu-id="95837-218">ChatDown</span><span class="sxs-lookup"><span data-stu-id="95837-218">ChatDown</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown)
- [<span data-ttu-id="95837-219">LUDown</span><span class="sxs-lookup"><span data-stu-id="95837-219">LUDown</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown)
- [<span data-ttu-id="95837-220">CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="95837-220">Azure CLI</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
