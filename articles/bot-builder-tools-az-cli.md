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
ms.openlocfilehash: 1eb47e76ef1bd6765d5ba93c27b97a8d9e6143db
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42905309"
---
# <a name="create-bots-with-azure-cli"></a><span data-ttu-id="62eb2-103">Creación de bots con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-103">Create bots with Azure CLI</span></span>

[!INCLUDE [pre-release-label](./includes/pre-release-label-v3.md)]

<span data-ttu-id="62eb2-104">[Herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) es un conjunto de herramientas nuevo que permite administrar e interactuar con los recursos de bot directamente desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-104">[Bot builder tools](https://github.com/microsoft/botbuilder-tools) is a new toolset which allows you to manage and interact with your bot resources directly from the command line.</span></span> 

<span data-ttu-id="62eb2-105">En este tutorial se mostrará cómo:</span><span class="sxs-lookup"><span data-stu-id="62eb2-105">In this tutorial we'll show you how to:</span></span>

- <span data-ttu-id="62eb2-106">Habilitar la extensión de bot de la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-106">Enable the Azure CLI bot extension</span></span>
- <span data-ttu-id="62eb2-107">Crear un bot con la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-107">Create a new bot using Azure CLI</span></span> 
- <span data-ttu-id="62eb2-108">Descargar una copia local para el desarrollo</span><span class="sxs-lookup"><span data-stu-id="62eb2-108">Download a local copy for development</span></span>
- <span data-ttu-id="62eb2-109">Usar la nueva herramienta MSBot para almacenar toda la información de recursos de bot</span><span class="sxs-lookup"><span data-stu-id="62eb2-109">Use the new MSBot tool to store all your bot resource information</span></span>
- <span data-ttu-id="62eb2-110">Administrar, crear o actualizar los modelos de LUIS y QnA con LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-110">Manage, create or update LUIS and QnA models with LUDown</span></span>
- <span data-ttu-id="62eb2-111">Conexión a servicios de LUIS y QnA Maker desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-111">Connect to LUIS an QnA maker services from the CLI</span></span>
- <span data-ttu-id="62eb2-112">Implementar el bot en Azure desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-112">Deploy your bot to Azure from the CLI</span></span>

## <a name="prerequisites"></a><span data-ttu-id="62eb2-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="62eb2-113">Prerequisites</span></span>

<span data-ttu-id="62eb2-114">Para habilitar estas herramientas desde la línea de comandos, necesita Node.js instalado en el equipo:</span><span class="sxs-lookup"><span data-stu-id="62eb2-114">To enable these tools from the command line, you will need Node.js installed to your machine:</span></span> 

- [<span data-ttu-id="62eb2-115">Node.js (v8.5 o superior)</span><span class="sxs-lookup"><span data-stu-id="62eb2-115">Node.js (v8.5 or greater)</span></span>](https://nodejs.org/en/)

## <a name="1-enable-azure-cli"></a><span data-ttu-id="62eb2-116">1. Habilitación de la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-116">1. Enable Azure CLI</span></span>

<span data-ttu-id="62eb2-117">Ahora puede administrar los bots mediante la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) como cualquier otro recurso de Azure.</span><span class="sxs-lookup"><span data-stu-id="62eb2-117">You can now manage bots using [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) like any other Azure resource.</span></span> <span data-ttu-id="62eb2-118">Para habilitar la CLI de Azure, siga los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="62eb2-118">To enable Azure CLI, complete the following steps:</span></span>

1. <span data-ttu-id="62eb2-119">[Descargue](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) la CLI de Azure si aún no la tiene.</span><span class="sxs-lookup"><span data-stu-id="62eb2-119">[Download](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI if you don't already have it.</span></span> 

2. <span data-ttu-id="62eb2-120">Escriba el comando siguiente para descargar el paquete de distribución Azure Bot Extension.</span><span class="sxs-lookup"><span data-stu-id="62eb2-120">Enter the following command to download the Azure Bot Extension dist package.</span></span>

```azurecli
az extension add -n botservice
```

>[!TIP]
> <span data-ttu-id="62eb2-121">En la actualidad, Azure Bot Extension solo admite los bots v3.</span><span class="sxs-lookup"><span data-stu-id="62eb2-121">The Azure Bot Extension currently only supports v3 bots.</span></span>
  
3. <span data-ttu-id="62eb2-122">[Inicie sesión](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli?view=azure-cli-latest) en la CLI de Azure mediante la ejecución del comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="62eb2-122">[Login](https://docs.microsoft.com/en-us/cli/azure/authenticate-azure-cli?view=azure-cli-latest) to Azure CLI by running the following command.</span></span>

```azurecli
az login
```
<span data-ttu-id="62eb2-123">Se le pedirá un código de autenticación temporal único.</span><span class="sxs-lookup"><span data-stu-id="62eb2-123">You will be prompted with a unique temporary auth code.</span></span> <span data-ttu-id="62eb2-124">Para el inicio de sesión, use un explorador web y visite [Inicio de sesión del dispositivo](https://microsoft.com/devicelogin) de Microsoft, y pegue el código proporcionado por la CLI para continuar.</span><span class="sxs-lookup"><span data-stu-id="62eb2-124">To signin, use a web browser and visit Microsoft [device login](https://microsoft.com/devicelogin), and paste the code provided by the CLI to continue.</span></span> 

![Inicio de sesión del dispositivo de MS](media/bot-builder-tools/ms-device-login.png)

<span data-ttu-id="62eb2-126">Después de iniciar sesión correctamente, verá la pantalla de bienvenida de la CLI de Azure, junto con una lista de las opciones disponibles para administrar las cuentas y los recursos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-126">Upon successful login, you will see the Azure CLI welcome screen, along with a list of available options to manage your account and resources.</span></span>

![CLI de Azure Bot](media/bot-builder-tools/az-cli-bot.png)


 <span data-ttu-id="62eb2-128">Para obtener una lista completa de los comandos de la CLI de Azure, [haga clic aquí](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span><span class="sxs-lookup"><span data-stu-id="62eb2-128">For a full list of Azure CLI commands, [click here](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest).</span></span>


## <a name="2-create-a-new-bot-from-azure-cli"></a><span data-ttu-id="62eb2-129">2. Creación de un bot desde la CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-129">2. Create a new bot from Azure CLI</span></span>

<span data-ttu-id="62eb2-130">Con la CLI de Azure y la nueva extensión de bot, puede crear bots completamente desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-130">Using Azure CLI and the new bot extension, you can create new bots entirely from the command line.</span></span> 

```azurecli
az bot [command]
```
|<span data-ttu-id="62eb2-131">Comandos:</span><span class="sxs-lookup"><span data-stu-id="62eb2-131">Commands</span></span>|  |
|----|----|
| <span data-ttu-id="62eb2-132">create</span><span class="sxs-lookup"><span data-stu-id="62eb2-132">create</span></span>      |<span data-ttu-id="62eb2-133">Agregar un recurso</span><span class="sxs-lookup"><span data-stu-id="62eb2-133">add a resource</span></span>|
| <span data-ttu-id="62eb2-134">delete</span><span class="sxs-lookup"><span data-stu-id="62eb2-134">delete</span></span>     |<span data-ttu-id="62eb2-135">Clonar un recurso</span><span class="sxs-lookup"><span data-stu-id="62eb2-135">clone a resource</span></span>|
| <span data-ttu-id="62eb2-136">descargar</span><span class="sxs-lookup"><span data-stu-id="62eb2-136">download</span></span>   | <span data-ttu-id="62eb2-137">Descargar el código fuente del bot</span><span class="sxs-lookup"><span data-stu-id="62eb2-137">download the bot source code</span></span>|
| <span data-ttu-id="62eb2-138">Publicar</span><span class="sxs-lookup"><span data-stu-id="62eb2-138">publish</span></span>   |<span data-ttu-id="62eb2-139">Publicar en un servicio de bot existente</span><span class="sxs-lookup"><span data-stu-id="62eb2-139">publish to an existing bot service</span></span>|
| <span data-ttu-id="62eb2-140">show</span><span class="sxs-lookup"><span data-stu-id="62eb2-140">show</span></span> |<span data-ttu-id="62eb2-141">Mostrar los recursos de bot existentes.</span><span class="sxs-lookup"><span data-stu-id="62eb2-141">show existing bot resources.</span></span>|
| <span data-ttu-id="62eb2-142">update</span><span class="sxs-lookup"><span data-stu-id="62eb2-142">update</span></span>| <span data-ttu-id="62eb2-143">Actualizar un servicio de bot existente</span><span class="sxs-lookup"><span data-stu-id="62eb2-143">Update an existing bot Service</span></span>|

<span data-ttu-id="62eb2-144">Para crear un bot desde la CLI, debe seleccionar un [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) existente, o bien crear uno.</span><span class="sxs-lookup"><span data-stu-id="62eb2-144">To create a new bot from the CLI, you need to select an existing [resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), or create a new one.</span></span> 

```azurecli
az bot create --resource-group "my-resource-group" --name "my-bot-name" --kind "my-resource-type" --description "description-of-my-bot"
```
<span data-ttu-id="62eb2-145">Después de una solicitud correcta, verá el mensaje de confirmación.</span><span class="sxs-lookup"><span data-stu-id="62eb2-145">After a successful request, you will see the confirmation message.</span></span>
```
obtained msa app id and password. Provisioning bot now.
```

> [!TIP]
> <span data-ttu-id="62eb2-146">Si recibe un mensaje de error en el que se indica que **no se encontró el grupo de recursos**, es posible que deba establecer su [suscripción](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer) en la CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="62eb2-146">If you receive an error message that the **resource group could not be found**, you may need to set your [subscription](https://docs.microsoft.com/en-us/azure/architecture/cloud-adoption-guide/adoption-intro/subscription-explainer) in Azure CLI.</span></span> <span data-ttu-id="62eb2-147">La suscripción de Azure debe coincidir con lo que se escribió al crear el grupo de recursos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-147">The Azure subscription must match the one entered when you created the resource group.</span></span> <span data-ttu-id="62eb2-148">Para establecerla, escriba lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-148">To set it enter the following.</span></span>
> ```azurecli
> az account set --subscription "your-subscription-name"
> ```
> <span data-ttu-id="62eb2-149">Para ver una lista de suscripciones para la cuenta, escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-149">To view a list of subscriptions for your account, enter the following command.</span></span>
> ```azurecli
> az account list
> ```

<span data-ttu-id="62eb2-150">De forma predeterminada, se crea un bot de .NET.</span><span class="sxs-lookup"><span data-stu-id="62eb2-150">By default, a new .NET bot will be created.</span></span> <span data-ttu-id="62eb2-151">Puede especificar el SDK de la plataforma si indica el lenguaje mediante el argumento **--lang**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-151">You can specify which platform SDK by specifying the language using the **-- lang** argument.</span></span> <span data-ttu-id="62eb2-152">En la actualidad, el paquete de extensión de bot es compatible con los SDK de C# y Node.js bot.</span><span class="sxs-lookup"><span data-stu-id="62eb2-152">Currently, the bot extension package supports C# and Node.js bot SDKs.</span></span> <span data-ttu-id="62eb2-153">Por ejemplo, para **crear un bot de Node.js**:</span><span class="sxs-lookup"><span data-stu-id="62eb2-153">For example, to **create a Node.js bot**:</span></span>

```azurecli
az bot create --resource-group "my-resource-group" --name "my-bot-name" --kind "my-resource-type" --description "description-of-my-bot" --lang Node 
```
<span data-ttu-id="62eb2-154">El nuevo bot de eco se aprovisionará en el grupo de recursos en Azure; para probarlo simplemente haga clic en **Probar en chat en web** bajo el encabezado de administración de bots de la vista Bot de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="62eb2-154">Your new echo bot will be provisioned to your resource group on Azure, to test it out simply select **Test in Webchat** under the bot management header of the Web App Bot view.</span></span> 

![Bot de eco de Azure](media/bot-builder-tools/az-echo-bot.png) 

## <a name="3-download-the-bot-locally"></a><span data-ttu-id="62eb2-156">3. Descarga local del bot</span><span class="sxs-lookup"><span data-stu-id="62eb2-156">3. Download the bot locally</span></span>

<span data-ttu-id="62eb2-157">Hay dos maneras de descargar el código fuente del bot nuevo.</span><span class="sxs-lookup"><span data-stu-id="62eb2-157">There are two ways you can download the source code for the new bot.</span></span>
- <span data-ttu-id="62eb2-158">Descargarlo desde Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="62eb2-158">Download from the Azure Portal.</span></span>
- <span data-ttu-id="62eb2-159">Descargarlo mediante la nueva CLI de Azure.</span><span class="sxs-lookup"><span data-stu-id="62eb2-159">Download using the new Azure CLI.</span></span>

<span data-ttu-id="62eb2-160">Para descargar el código fuente del bot desde el portal, simplemente seleccione el recurso de bot y haga clic en **Compilar** bajo la administración de bots.</span><span class="sxs-lookup"><span data-stu-id="62eb2-160">To download your bot source code from the portal, simply select your bot resource, and select **Build** under bot management.</span></span> <span data-ttu-id="62eb2-161">Hay varias opciones disponibles para administrar o recuperar el código fuente del bot de forma local.</span><span class="sxs-lookup"><span data-stu-id="62eb2-161">There are several different options available to manage or retrieve your bot's source code locally.</span></span> 

![Descarga del bot desde Azure Portal](media/bot-builder-tools/az-portal-manage-code.png)

<span data-ttu-id="62eb2-163">Para descargar el código fuente del bot mediante la CLI, escriba el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="62eb2-163">To download your bot source using the CLI, enter the following command.</span></span> <span data-ttu-id="62eb2-164">El bot se descargará a un subdirectorio.</span><span class="sxs-lookup"><span data-stu-id="62eb2-164">Your bot will be downloaded to a subdirectory.</span></span> <span data-ttu-id="62eb2-165">Si todavía no existe el subdirectorio, el comando lo creará de forma automática.</span><span class="sxs-lookup"><span data-stu-id="62eb2-165">If the subdirectory doesn't already exist, the command will create it for you.</span></span>

```azurecli
az bot download --name "my-bot-name" --resource-group "my-resource-group"
```
<span data-ttu-id="62eb2-166">Pero también puede especificar el directorio el que descargar el bot.</span><span class="sxs-lookup"><span data-stu-id="62eb2-166">However, you can also specify the directory to download the bot to.</span></span>
<span data-ttu-id="62eb2-167">Por ejemplo: </span><span class="sxs-lookup"><span data-stu-id="62eb2-167">For example:</span></span>

![Comando de descarga desde la CLI](media/bot-builder-tools/cli-bot-download-command.png)

![Descarga del bot desde la CLI](media/bot-builder-tools/cli-bot-download.png)

<span data-ttu-id="62eb2-170">El comando anterior permite descargar el código fuente del bot directamente a la ubicación especificada, lo que permite desarrollarlo de forma local.</span><span class="sxs-lookup"><span data-stu-id="62eb2-170">The command above allows you to download your bot's source code directly to the specified location, allowing you to develop your bot locally.</span></span>


## <a name="4-store-your-bot-information-with-msbot"></a><span data-ttu-id="62eb2-171">4. Almacenamiento de la información del bot con MSBot</span><span class="sxs-lookup"><span data-stu-id="62eb2-171">4. Store your bot information with MSBot</span></span>

<span data-ttu-id="62eb2-172">La nueva herramienta [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) permite crear un archivo **.bot**, en el que se almacenan metadatos sobre otros servicios que consume el bot, todo en una única ubicación.</span><span class="sxs-lookup"><span data-stu-id="62eb2-172">The new [MSBot](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot) tool allows you to create a **.bot** file, which stores metadata about different services your bot consumes, all in one location.</span></span> <span data-ttu-id="62eb2-173">Este archivo también permite que el bot se conecte a estos servicios desde la CLI.</span><span class="sxs-lookup"><span data-stu-id="62eb2-173">This file also enables your bot to connect to these services from the CLI.</span></span> <span data-ttu-id="62eb2-174">La herramienta está disponible como un módulo npm; para instalarlo, ejecute lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-174">The tool is available as an npm module, to install it run:</span></span>

```shell
npm install -g msbot 
```

<span data-ttu-id="62eb2-175">Para crear un archivo bot desde la CLI, escriba **msbot init** seguido del nombre del bot y la dirección URL del punto de conexión de destino, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="62eb2-175">To create a bot file, from your CLI enter **msbot init** followed by the name of your bot, and the target URL endpoint, for example:</span></span>

```shell
msbot init --name name-of-my-bot --endpoint http://localhost:bot-port-number/api/messages
```
<span data-ttu-id="62eb2-176">Para conectar el bot a un servicio, escriba **msbot connect** en la CLI, seguido del servicio adecuado:</span><span class="sxs-lookup"><span data-stu-id="62eb2-176">To connect your bot to a service, in your CLI enter **msbot connect** followed by the appropriate service:</span></span>

```shell
msbot connect service-type
```

| <span data-ttu-id="62eb2-177">Tipo de servicio</span><span class="sxs-lookup"><span data-stu-id="62eb2-177">Service type</span></span> | <span data-ttu-id="62eb2-178">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="62eb2-178">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="62eb2-179">azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-179">azure</span></span>  |<span data-ttu-id="62eb2-180">Conecta el bot a un registro de Azure Bot Service.</span><span class="sxs-lookup"><span data-stu-id="62eb2-180">connect your bot to an Azure Bot Service registration</span></span>|
|<span data-ttu-id="62eb2-181">endpoint</span><span class="sxs-lookup"><span data-stu-id="62eb2-181">endpoint</span></span>| <span data-ttu-id="62eb2-182">Conectar el bot a un punto de conexión como localhost</span><span class="sxs-lookup"><span data-stu-id="62eb2-182">connect your bot to an endpoint such as localhost</span></span>|
|<span data-ttu-id="62eb2-183">luis</span><span class="sxs-lookup"><span data-stu-id="62eb2-183">luis</span></span>     | <span data-ttu-id="62eb2-184">Conecta el bot a una aplicación de LUIS.</span><span class="sxs-lookup"><span data-stu-id="62eb2-184">connect your bot to a LUIS application</span></span> |
| <span data-ttu-id="62eb2-185">qna</span><span class="sxs-lookup"><span data-stu-id="62eb2-185">qna</span></span>     |<span data-ttu-id="62eb2-186">Conecta el bot a una base de conocimiento de QnA.</span><span class="sxs-lookup"><span data-stu-id="62eb2-186">connect your bot to a QnA Knowledgebase</span></span>|
|<span data-ttu-id="62eb2-187">help [cmd]</span><span class="sxs-lookup"><span data-stu-id="62eb2-187">help [cmd]</span></span>  |<span data-ttu-id="62eb2-188">Muestra la ayuda para [cmd].</span><span class="sxs-lookup"><span data-stu-id="62eb2-188">display help for [cmd]</span></span>|

### <a name="connect-your-bot-to-abs-with-the-bot-file"></a><span data-ttu-id="62eb2-189">Conexión del bot a ABS con el archivo .bot</span><span class="sxs-lookup"><span data-stu-id="62eb2-189">Connect your bot to ABS with the .bot file</span></span>

<span data-ttu-id="62eb2-190">Con la herramienta MSBot instalada, puede conectar fácilmente el bot a un grupo de recursos existente en Azure Bot Service si ejecuta el comando az bot **show**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-190">With the MSBot tool installed, you can easily connect your bot to an existing resource group in the Azure Bot Service by running the az bot **show** command.</span></span> 

```azurecli
az bot show -n my-bot-name -g my-resource-group --msbot | msbot connect azure --stdin
```

<span data-ttu-id="62eb2-191">Esta operación tomará el punto de conexión, Id. de la aplicación y contraseña de MSA actuales del grupo de recursos de destino y actualizará la información correspondiente en el archivo .bot.</span><span class="sxs-lookup"><span data-stu-id="62eb2-191">This will take current endpoint, MSA appID and password from the target resource group and update the information accordingly in your .bot file.</span></span> 


## <a name="5-manage-update-or-create-luis-and-qna-services-with--new-botbuilder-tools"></a><span data-ttu-id="62eb2-192">5. Administración, actualización o creación de servicios de LUIS y QnA con las nuevas herramientas de Bot Builder</span><span class="sxs-lookup"><span data-stu-id="62eb2-192">5. Manage, Update or Create LUIS and QnA services with  new botbuilder-tools</span></span>

<span data-ttu-id="62eb2-193">Las [herramientas de Bot Builder](https://github.com/microsoft/botbuilder-tools) son un conjunto de herramientas nuevo que permite administrar los recursos de bot e interactuar con ellos directamente desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-193">[Bot builder tools](https://github.com/microsoft/botbuilder-tools) is a new toolset which allows you to manage and interact with your bot resources directly from the command line.</span></span> 

>[!TIP]
> <span data-ttu-id="62eb2-194">Cada herramienta del generador de bots incluye un comando de ayuda global, accesible desde la línea de comandos si se escribe **-h** o **--help**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-194">Every bot builder tool includes a global help command, accessible from the command line by entering **-h** or **--help**.</span></span> <span data-ttu-id="62eb2-195">Este comando está disponible en cualquier momento desde cualquier acción, lo que proporcionará una representación útil de las opciones disponibles junto con sus descripciones.</span><span class="sxs-lookup"><span data-stu-id="62eb2-195">This command is available at any time from any action, which will provide a helpful display of the options available to you along with their descriptions.</span></span>

### <a name="ludown"></a><span data-ttu-id="62eb2-196">LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-196">LUDown</span></span>
<span data-ttu-id="62eb2-197">[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) permite describir y crear componentes de lenguaje eficaces para los bots mediante archivos **.lu**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-197">[LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/Ludown) allows you to describe and create powerful language components for bots using **.lu** files.</span></span> <span data-ttu-id="62eb2-198">El nuevo archivo .lu es un tipo de formato Markdown que la herramienta LUDown usa para generar los archivos .json específicos del servicio de destino.</span><span class="sxs-lookup"><span data-stu-id="62eb2-198">The new .lu file is a type of markdown format which the LUDown tool consumes and outputs .json files specific to the target service.</span></span> <span data-ttu-id="62eb2-199">Actualmente, puede usar los archivos .lu para crear una aplicación de [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) o una base de conocimiento de [QnA](https://qnamaker.ai/Documentation/CreateKb) con formatos diferentes para cada una.</span><span class="sxs-lookup"><span data-stu-id="62eb2-199">Currently, you can use .lu files to create a new [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) application or [QnA](https://qnamaker.ai/Documentation/CreateKb) knowledge base, using different formats for each.</span></span> <span data-ttu-id="62eb2-200">LUDown está disponible como un módulo npm y se puede usar si se instala de forma global en la máquina:</span><span class="sxs-lookup"><span data-stu-id="62eb2-200">LUDown is available as an npm module, and can be used by installing globally to your machine:</span></span>

```shell
npm install -g ludown
```
<span data-ttu-id="62eb2-201">La herramienta LUDown se puede usar para crear modelos de .json para LUIS y QnA.</span><span class="sxs-lookup"><span data-stu-id="62eb2-201">The LUDown tool can be used to create new .json models for both LUIS and QnA.</span></span>  


### <a name="creating-a-luis-application-with-ludown"></a><span data-ttu-id="62eb2-202">Creación de una aplicación de LUIS con LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-202">Creating a LUIS application with LUDown</span></span>

<span data-ttu-id="62eb2-203">Puede definir [intenciones](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) y [entidades](https://docs.microsoft.com/azure/cognitive-services/luis/add-entities) para una aplicación de LUIS, tal como lo haría desde el portal de LUIS.</span><span class="sxs-lookup"><span data-stu-id="62eb2-203">You can define [intents](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) and [entities](https://docs.microsoft.com/azure/cognitive-services/luis/add-entities) for a LUIS application just like you would from the LUIS portal.</span></span> 

<span data-ttu-id="62eb2-204">`# \<intent-name\>` describe la sección de definición de una intención nueva.</span><span class="sxs-lookup"><span data-stu-id="62eb2-204">`# \<intent-name\>` describes a new intent definition section.</span></span> <span data-ttu-id="62eb2-205">Las líneas siguientes contienen [expresiones](https://docs.microsoft.com/azure/cognitive-services/luis/add-example-utterances) que describen esa intención.</span><span class="sxs-lookup"><span data-stu-id="62eb2-205">Subsequent lines contain [utterances](https://docs.microsoft.com/azure/cognitive-services/luis/add-example-utterances) that describe that intent.</span></span>

<span data-ttu-id="62eb2-206">Por ejemplo, puede crear varias intenciones LUIS en un único archivo .lu de esta forma:</span><span class="sxs-lookup"><span data-stu-id="62eb2-206">For example, you can create multiple LUIS intents in a single .lu file as follows:</span></span> 

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

### <a name="qna-pairs-with-ludown"></a><span data-ttu-id="62eb2-207">Pares de QnA con LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-207">QnA pairs with LUDown</span></span>

<span data-ttu-id="62eb2-208">El formato de archivo .lu también admite pares de QnA mediante la notación siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-208">The .lu file format supports also QnA pairs using the following notation:</span></span> 

  ```ludown
  > This is a comment. QnA definitions have the general format:
  ### ? this-is-the-question-string
  - this-is-an-alternate-form-of-the-same-question
  - this-is-another-one
    ```markdown
    this-is-the-answer
    ```
  ```
<span data-ttu-id="62eb2-209">La herramienta LUDown separará automáticamente la pregunta y las respuestas en un archivo JSON de QnA Maker que, después, se puede usar para crear una nueva base de conocimiento [QnaMaker.ai](http://qnamaker.ai).</span><span class="sxs-lookup"><span data-stu-id="62eb2-209">The LUDown tool will automatically separate question and answers into a qnamaker JSON file that you can then use to create your new [QnaMaker.ai](http://qnamaker.ai) knowledge base.</span></span>

  ```ludown
  ### ? How do I change the default message for QnA Maker?
    ```markdown
    You can change the default message if you use the QnAMakerDialog. 
    See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
    ```
  ```

<span data-ttu-id="62eb2-210">También se pueden agregar varias preguntas a la misma respuesta si se agregan nuevas líneas de variaciones de preguntas para una sola respuesta.</span><span class="sxs-lookup"><span data-stu-id="62eb2-210">You can also add multiple questions to the same answer by simply adding new lines of variations of questions for a single answer.</span></span> 

  ```ludown
  ### ? What is your name?
  - What should I call you?
    ```markdown
    I'm the echoBot! Nice to meet you.
    ```
  ```

### <a name="generating-json-models-with-ludown"></a><span data-ttu-id="62eb2-211">Generación de modelos .json con LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-211">Generating .json models with LUDown</span></span>

<span data-ttu-id="62eb2-212">Después de definir los componentes del lenguaje de LUIS o QnA en formato .lu, puede publicarlos en un archivo .json de LUIS, o bien .json o .tsv de QnA.</span><span class="sxs-lookup"><span data-stu-id="62eb2-212">After you've defined LUIS or QnA language components in the .lu format, you can publish out to a LUIS .json, QnA .json, or QnA .tsv file.</span></span> <span data-ttu-id="62eb2-213">Cuando se ejecuta, la herramienta LUDown buscará los archivos .lu en el mismo directorio de trabajo para analizarlos.</span><span class="sxs-lookup"><span data-stu-id="62eb2-213">When run, the LUDown tool will look for any .lu files within the same working directory to parse.</span></span> <span data-ttu-id="62eb2-214">Como la herramienta LUDown puede tener como destino archivos .lu de LUIS o QnA, solo hay que especificar para qué servicio de lenguaje se va generar, mediante el comando general **ludown parse <Service> --in <luFile>**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-214">Since the LUDown tool can target both LUIS or QnA with .lu files, we simply need to specify which language service to generate for, using the general command **ludown parse <Service> --in <luFile>**.</span></span> 

<span data-ttu-id="62eb2-215">En el directorio de trabajo de ejemplo, hay dos archivos .lu para analizar: "luis-sample.lu" para crear el modelo de LUIS, y "qna-sample.lu" para crear una base de conocimiento de QnA.</span><span class="sxs-lookup"><span data-stu-id="62eb2-215">In our sample working directory, we have two .lu files to parse, 'luis-sample.lu' to create LUIS model, and 'qna-sample.lu' to create a QnA knowledge base.</span></span>


#### <a name="generate-luis-json-models"></a><span data-ttu-id="62eb2-216">Generar modelos .json de LUIS</span><span class="sxs-lookup"><span data-stu-id="62eb2-216">Generate LUIS .json models</span></span>

<span data-ttu-id="62eb2-217">**luis-sample.lu**</span><span class="sxs-lookup"><span data-stu-id="62eb2-217">**luis-sample.lu**</span></span> 
```ludown
# Greeting
- Hi
- Hello
- Good morning
- Good evening
```

<span data-ttu-id="62eb2-218">Para generar un modelo de LUIS mediante LUDown, escriba lo siguiente en el directorio de trabajo actual:</span><span class="sxs-lookup"><span data-stu-id="62eb2-218">To generate a LUIS model using LUDown, in your current working directory simply enter the following:</span></span>

```shell
ludown parse ToLuis --in ludown-file-name.lu
```

#### <a name="generate-qna-knowledge-base-json"></a><span data-ttu-id="62eb2-219">Generar el archivo .json de base de conocimiento de QnA</span><span class="sxs-lookup"><span data-stu-id="62eb2-219">Generate QnA Knowledge Base .json</span></span>

<span data-ttu-id="62eb2-220">**qna-sample.lu**</span><span class="sxs-lookup"><span data-stu-id="62eb2-220">**qna-sample.lu**</span></span>
  ```ludown
  > This is a sample ludown file for QnA Maker.

  ### ? How do I change the default message
    ```markdown
    You can change the default message if you use the QnAMakerDialog. 
    See [this link](https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle) for details. 
    ```
  ```

<span data-ttu-id="62eb2-221">De forma similar, para crear una base de conocimiento de QnA, solo hay que cambiar el destino del análisis.</span><span class="sxs-lookup"><span data-stu-id="62eb2-221">Similarly, to create a QnA knowledge base, you only need to change the parse target.</span></span> 

```shell
ludown parse ToQna --in ludown-file-name.lu
```

<span data-ttu-id="62eb2-222">LUIS y QnA pueden usar los archivos JSON resultantes a través de sus portales correspondientes, o bien a través de las nuevas herramientas de la CLI.</span><span class="sxs-lookup"><span data-stu-id="62eb2-222">The resulting JSON files can be consumed by LUIS and QnA either through their respective portals, or via the new CLI tools.</span></span> 

## <a name="6-connect-to-luis-an-qna-maker-services-from-the-cli"></a><span data-ttu-id="62eb2-223">6. Conexión a servicios de LUIS y QnA Maker desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-223">6. Connect to LUIS an QnA maker services from the CLI</span></span>

### <a name="connect-to-luis-from-the-cli"></a><span data-ttu-id="62eb2-224">Conexión a LUIS desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-224">Connect to LUIS from the CLI</span></span> 

<span data-ttu-id="62eb2-225">En el nuevo conjunto de herramientas se incluye una [extensión de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) que permite administrar de forma independiente los recursos de LUIS.</span><span class="sxs-lookup"><span data-stu-id="62eb2-225">Included in the new tool set is a [LUIS extension](https://github.com/Microsoft/botbuilder-tools/tree/master/LUIS) which allows you to independently manage your LUIS resources.</span></span> <span data-ttu-id="62eb2-226">Está disponible como módulo npm que se puede descargar de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="62eb2-226">It is available as an npm module which you can download:</span></span>

```shell
npm install -g luis-apis
```
<span data-ttu-id="62eb2-227">El uso de comandos básicos para la herramienta de LUIS desde la CLI es el siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-227">The basic command usage for the LUIS tool from the CLI is:</span></span>

```shell
luis action-name resource-name arguments-list
```
<span data-ttu-id="62eb2-228">Para conectar el bot a LUIS, deberá crear un archivo **.luisrc**.</span><span class="sxs-lookup"><span data-stu-id="62eb2-228">To connect your bot to LUIS, you will need to create a **.luisrc** file.</span></span> <span data-ttu-id="62eb2-229">Se trata de un archivo de configuración que aprovisiona el identificador y la contraseña de la aplicación de LUIS para el punto de conexión de servicio cuando la aplicación realiza llamadas salientes.</span><span class="sxs-lookup"><span data-stu-id="62eb2-229">This is a configuration file which provisions your LUIS appID and password to the service endpoint when your application makes outbound calls.</span></span> <span data-ttu-id="62eb2-230">Puede crear este archivo mediante la ejecución de **luis init** como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="62eb2-230">You can create this file by running **luis init** as follows:</span></span>

```shell
luis init
```
<span data-ttu-id="62eb2-231">En el terminal, se le pedirá que escriba la clave de creación de LUIS, la región y el identificador de la aplicación antes de que la herramienta genere el archivo.</span><span class="sxs-lookup"><span data-stu-id="62eb2-231">You will be prompted in the terminal to enter your LUIS authoring key, region, and appID before the tool will generate the file.</span></span>  

![Comando init de LUIS](media/bot-builder-tools/luis-init.png) 


<span data-ttu-id="62eb2-233">Una vez generado este archivo, la aplicación podrá consumir el archivo .json de LUIS (generado desde LUDown) mediante el comando siguiente desde la CLI:</span><span class="sxs-lookup"><span data-stu-id="62eb2-233">Once this file is generated, your application will be able to consume the LUIS .json file (generated from LUDown) using the following command from the CLI:</span></span> 

```shell
luis import application --in luis-app.json | msbot connect luis --stdin
```

### <a name="connect-to-qna-from-the-cli"></a><span data-ttu-id="62eb2-234">Conectarse a QnA desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-234">Connect to QnA from the CLI</span></span>

<span data-ttu-id="62eb2-235">En el nuevo conjunto de herramientas se incluye una [extensión QnA](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) que permite administrar de forma independiente los recursos de LUIS.</span><span class="sxs-lookup"><span data-stu-id="62eb2-235">Included in the new tool set is a [QnA extension](https://github.com/Microsoft/botbuilder-tools/tree/master/QnAMaker) which allows you to independently manage your LUIS resources.</span></span> <span data-ttu-id="62eb2-236">Está disponible como módulo npm que se puede descargar:</span><span class="sxs-lookup"><span data-stu-id="62eb2-236">It is available as an npm module which you can download:</span></span>

```shell
npm install -g qnamaker
```
<span data-ttu-id="62eb2-237">Con la herramienta QnA Maker, puede crear, actualizar, publicar, eliminar y entrenar la base de conocimiento.</span><span class="sxs-lookup"><span data-stu-id="62eb2-237">With the QnA maker tool, you can create, update, publish, delete, and train your knowledge base.</span></span> <span data-ttu-id="62eb2-238">Para empezar, deberá crear un archivo **.qnamakerrc**, necesario para habilitar el punto de conexión al servicio.</span><span class="sxs-lookup"><span data-stu-id="62eb2-238">To get started, you need to create a **.qnamakerrc** file is required to enable the endpoint to your service.</span></span> <span data-ttu-id="62eb2-239">Puede crear fácilmente este archivo si ejecuta **qnamaker init** y sigue las indicaciones, junto con el identificador de la base de conocimiento de QnA Maker.</span><span class="sxs-lookup"><span data-stu-id="62eb2-239">You can easily create this file by running **qnamaker init** and following the prompts along with your QnA Maker knowledge base ID.</span></span> 

```shell
qnamaker init 
```
![Archivo init de QnaMaker](media/bot-builder-tools/qnamaker-init.png)

<span data-ttu-id="62eb2-241">Una vez generado el archivo .qnamakerrc, se puede conectar a la base de conocimiento de QnA para consumir el archivo de base de conocimiento .json o .tsv (generado desde LUDown) mediante el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-241">Once your .qnamakerrc file is generated, you can now connect to your QnA knowledge base to consume the knowledge base .json/.tsv file (generated from LUDown) using the following command:</span></span>

```shell
qnamaker create --in qnaKB.json --msbot | msbot connect qna --stdin
```

## <a name="7-publish-to-azure-from-the-cli"></a><span data-ttu-id="62eb2-242">7. Publicación en Azure desde la CLI</span><span class="sxs-lookup"><span data-stu-id="62eb2-242">7. Publish to Azure from the CLI</span></span>

<span data-ttu-id="62eb2-243">Después de realizar cambios en el código fuente del bot, puede publicarlos sin problemas si ejecuta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="62eb2-243">After making changes to your bot's source code, you can seamlessly publish your changes by running the following:</span></span>

```azurecli
az bot publish --name "my-bot-name" --resource-group "my-resource-group"
```

## <a name="references"></a><span data-ttu-id="62eb2-244">Referencias</span><span class="sxs-lookup"><span data-stu-id="62eb2-244">References</span></span>
- [<span data-ttu-id="62eb2-245">Código fuente de las herramientas de Bot Builder</span><span class="sxs-lookup"><span data-stu-id="62eb2-245">BotBuilder Tools Source Code</span></span>](https://github.com/Microsoft/botbuilder-tools)
- [<span data-ttu-id="62eb2-246">MSBot</span><span class="sxs-lookup"><span data-stu-id="62eb2-246">MSBot</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/MSBot)
- [<span data-ttu-id="62eb2-247">ChatDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-247">ChatDown</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/Chatdown)
- [<span data-ttu-id="62eb2-248">LUDown</span><span class="sxs-lookup"><span data-stu-id="62eb2-248">LUDown</span></span>](https://github.com/Microsoft/botbuilder-tools/tree/master/ludown)
- [<span data-ttu-id="62eb2-249">CLI de Azure</span><span class="sxs-lookup"><span data-stu-id="62eb2-249">Azure CLI</span></span>](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)


