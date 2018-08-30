---
title: Creación de experiencias controladas por datos con Azure Search | Microsoft Docs
description: Obtenga información sobre cómo crear experiencias controladas por datos con Azure Search y ayude a los usuarios a navegar por grandes cantidades de contenido en un bot con el SDK de Bot Builder para Node.js y Azure Search.
author: matthewshim-ms
ms.author: v-shimma
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: e9f07cdd4616a2649dca31f096eca3377cd46b7d
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904239"
---
# <a name="create-data-driven-experiences-with-azure-search"></a><span data-ttu-id="8881e-103">Creación de experiencias controladas por datos con Azure Search</span><span class="sxs-lookup"><span data-stu-id="8881e-103">Create data-driven experiences with Azure Search</span></span> 

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-search-azure.md)
> - [Node.js](../nodejs/bot-builder-nodejs-search-azure.md)

<span data-ttu-id="8881e-106">Puede agregar [Azure Search][search] al bot para ayudar al usuario a navegar por grandes cantidades de contenido y crear una experiencia de exploración controlada por datos para los usuarios del bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-106">You can add [Azure Search][search] to your bot to help the user navigate large amounts of content and create a data-driven exploration experience for users of your bot.</span></span>

<span data-ttu-id="8881e-107">Azure Search es un servicio de Azure que ofrece búsqueda de palabras clave, lingüística integrada, puntuación personalizada, navegación por facetas y mucho más.</span><span class="sxs-lookup"><span data-stu-id="8881e-107">Azure Search is an Azure service that offers keyword search, built-in linguistics, custom scoring, faceted navigation and more.</span></span> <span data-ttu-id="8881e-108">Azure Search también puede indexar el contenido desde diversos orígenes, incluidos Azure SQL DB, DocumentDB, Blob Storage y Table Storage.</span><span class="sxs-lookup"><span data-stu-id="8881e-108">Azure Search can also index content from various sources, including Azure SQL DB, DocumentDB, Blob Storage, and Table Storage.</span></span> <span data-ttu-id="8881e-109">Es compatible con la indexación "push" para otros orígenes de datos y puede abrir archivos PDF, documentos de Office y otros formatos que contengan datos no estructurados.</span><span class="sxs-lookup"><span data-stu-id="8881e-109">It supports "push" indexing for other sources of data, and it can open PDFs, Office documents, and other formats containing unstructured data.</span></span> <span data-ttu-id="8881e-110">Una vez recopilado, el contenido se coloca en un índice de Azure Search, al que luego el bot puede consultar.</span><span class="sxs-lookup"><span data-stu-id="8881e-110">Once collected, the content goes into an Azure Search index, which the bot can then query.</span></span>

## <a name="install-dependencies"></a><span data-ttu-id="8881e-111">Instalación de dependencias</span><span class="sxs-lookup"><span data-stu-id="8881e-111">Install dependencies</span></span>

<span data-ttu-id="8881e-112">Desde un símbolo del sistema, vaya al directorio del proyecto del bot e instale los módulos siguientes con el Administrador de paquetes para Node (NPM):</span><span class="sxs-lookup"><span data-stu-id="8881e-112">From a command prompt, navigate to your bot's project directory and install the following modules with the Node Package Manager (NPM):</span></span>

* [<span data-ttu-id="8881e-113">bluebird</span><span class="sxs-lookup"><span data-stu-id="8881e-113">bluebird</span></span>](https://www.npmjs.com/package/bluebird)
* [<span data-ttu-id="8881e-114">lodash</span><span class="sxs-lookup"><span data-stu-id="8881e-114">lodash</span></span>](https://www.npmjs.com/package/lodash)
* [<span data-ttu-id="8881e-115">Solicitud</span><span class="sxs-lookup"><span data-stu-id="8881e-115">request</span></span>](https://www.npmjs.com/package/request)

## <a name="prerequisites"></a><span data-ttu-id="8881e-116">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8881e-116">Prerequisites</span></span>

<span data-ttu-id="8881e-117">Se **requiere** lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8881e-117">The following are **required**:</span></span> 
- <span data-ttu-id="8881e-118">Una suscripción de Azure y una clave principal de Azure Search.</span><span class="sxs-lookup"><span data-stu-id="8881e-118">Have an Azure subscription and an Azure Search Primary Key.</span></span> <span data-ttu-id="8881e-119">Puede encontrarlo en Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="8881e-119">You can find this in the Azure portal.</span></span>
- <span data-ttu-id="8881e-120">Copiar la biblioteca [SearchDialogLibrary](https://github.com/Microsoft/botBuilder-Samples/tree/master/Node/demo-Search/SearchDialogLibrary) en el directorio del proyecto del bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-120">Copy the [SearchDialogLibrary](https://github.com/Microsoft/botBuilder-Samples/tree/master/Node/demo-Search/SearchDialogLibrary) library to your bot's project directory.</span></span> <span data-ttu-id="8881e-121">Esta biblioteca contiene diálogos generales para la búsqueda del usuario, pero se pueden personalizar según sea necesario para adaptarlos al bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-121">This library contains general dialogs for the user to search, but can be customized as needed to suit your bot.</span></span> 

- <span data-ttu-id="8881e-122">Copiar la biblioteca [SearchProviders](https://github.com/Microsoft/botBuilder-Samples/tree/master/Node/demo-Search/SearchProviders) en el directorio del proyecto del bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-122">Copy the [SearchProviders](https://github.com/Microsoft/botBuilder-Samples/tree/master/Node/demo-Search/SearchProviders) library to your bot's project directory.</span></span> <span data-ttu-id="8881e-123">Esta biblioteca contiene todos los componentes necesarios para crear una solicitud y enviarla a Azure Search.</span><span class="sxs-lookup"><span data-stu-id="8881e-123">This library contains all of the components required to create a request and submit it to Azure Search.</span></span>

## <a name="connect-to-the-azure-service"></a><span data-ttu-id="8881e-124">Conexión al servicio de Azure</span><span class="sxs-lookup"><span data-stu-id="8881e-124">Connect to the Azure Service</span></span> 

<span data-ttu-id="8881e-125">En el archivo de programa principal del bot (por ejemplo, app.js), cree las rutas de acceso de referencia a las dos bibliotecas que instaló anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8881e-125">In your bot's main program file (e.g.: app.js), create the reference paths to the two libraries you installed previously.</span></span> 

```javascript
var SearchLibrary = require('./SearchDialogLibrary');
var AzureSearch = require('./SearchProviders/azure-search');
```

<span data-ttu-id="8881e-126">Agregue el código de ejemplo siguiente al bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-126">Add the following sample code to your bot.</span></span> <span data-ttu-id="8881e-127">En el objeto `AzureSearch`, pase su propia configuración de Azure Search al método `.create`.</span><span class="sxs-lookup"><span data-stu-id="8881e-127">In the `AzureSearch` object, pass in your own Azure Search settings into the `.create` method.</span></span> <span data-ttu-id="8881e-128">En tiempo de ejecución, esto enlazará el bot con el servicio de Azure Search y esperará una consulta de usuario completada en forma de `Promise`.</span><span class="sxs-lookup"><span data-stu-id="8881e-128">At run time, this will bind your bot to the Azure Search service and wait for a completed user query in the form of a `Promise`.</span></span>  

```javascript
// Azure Search
var azureSearchClient = AzureSearch.create('Your-Azure-Search-Service-Name', 'Your-Azure-Search-Primary-Key', 'Your-Azure-Search-Service-Index');
var ResultsMapper = SearchLibrary.defaultResultsMapper(ToSearchHit);
```

 <span data-ttu-id="8881e-129">La referencia `azureSearchClient` crea el modelo de Azure Search, que pasa la configuración de autorización del servicio de Azure desde la configuración `.env` del bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-129">The `azureSearchClient` reference creates the Azure Search model, which passes the Azure Service's authorization settings from the bot's `.env` settings.</span></span> 
 <span data-ttu-id="8881e-130">`ResultsMapper` analiza el objeto de respuesta de Azure y asigna los datos tal como se definen en el método `ToSearchHit`.</span><span class="sxs-lookup"><span data-stu-id="8881e-130">`ResultsMapper` parses the Azure response object and maps the data as we define in `ToSearchHit` method.</span></span> <span data-ttu-id="8881e-131">Para ver un ejemplo de implementación de este método, consulte [Después de que Azure Search responde](#after-azure-search-responds).</span><span class="sxs-lookup"><span data-stu-id="8881e-131">For an implementation example of this method, see [After Azure Search responds](#after-azure-search-responds).</span></span>

## <a name="register-the-search-library"></a><span data-ttu-id="8881e-132">Registro de la biblioteca de búsqueda</span><span class="sxs-lookup"><span data-stu-id="8881e-132">Register the search library</span></span>
<span data-ttu-id="8881e-133">Puede personalizar los diálogos de búsqueda directamente en el propio módulo `SearchLibrary`.</span><span class="sxs-lookup"><span data-stu-id="8881e-133">You can customize your search dialogs directly in the `SearchLibrary` module itself.</span></span> <span data-ttu-id="8881e-134">`SearchLibrary` realiza la mayoría del trabajo pesado, incluida la llamada a Azure Search.</span><span class="sxs-lookup"><span data-stu-id="8881e-134">The `SearchLibrary` performs most of the heavy lifting, including making the call to Azure Search.</span></span> 

<span data-ttu-id="8881e-135">Agregue el código siguiente en el archivo de programa principal del bot para registrar la biblioteca de diálogos de búsqueda con el bot.</span><span class="sxs-lookup"><span data-stu-id="8881e-135">Add the following code in your bot's main program file to register the Search Dialogs library with your bot.</span></span> 

```javascript
bot.library(SearchLibrary.create({
    multipleSelection: true,
    search: function (query) { return azureSearchClient.search(query).then(ResultsMapper); },
    refiners: ['refiner1', 'refiner2', 'refiner3'], // customize your own refiners 
    refineFormatter: function (refiners) {
        return _.zipObject(
            refiners.map(function (r) { return 'By ' + _.capitalize(r); }),
            refiners);
    }
}));
```
<span data-ttu-id="8881e-136">`SearchLibrary` no solo almacena todos los diálogos relacionados con la búsqueda, sino que también toma la consulta del usuario para enviarla a Azure Search.</span><span class="sxs-lookup"><span data-stu-id="8881e-136">The `SearchLibrary` not only stores all of your search related dialogs, but also takes the user query to submit to Azure Search.</span></span> <span data-ttu-id="8881e-137">Deberá definir sus propios refinadores en la matriz `refiners` para especificar las entidades que desea permitir que el usuario restrinja o filtre los resultados de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="8881e-137">You will need to define your own refiners in the `refiners` array to specify entities you wish to allow your user to narrow or filter their search results.</span></span>  

## <a name="create-a-search-dialog"></a><span data-ttu-id="8881e-138">Creación de un diálogo de búsqueda</span><span class="sxs-lookup"><span data-stu-id="8881e-138">Create a search dialog</span></span>

<span data-ttu-id="8881e-139">Puede elegir estructurar los diálogos como quiera.</span><span class="sxs-lookup"><span data-stu-id="8881e-139">You may choose to structure your dialogs however you want.</span></span> <span data-ttu-id="8881e-140">El único requisito para configurar un diálogo de Azure Search es invocar el método `.begin` del objeto `SearchLibrary`, pasando el objeto `session` generado por el SDK de Bot Builder.</span><span class="sxs-lookup"><span data-stu-id="8881e-140">The only requirement to setting up an Azure Search dialog is to invoke the `.begin` method from the `SearchLibrary` object, passing in the `session` object generated by the Bot Builder SDK.</span></span> 

```javascript
function (session) {
        // Trigger Azure Search dialogs 
        SearchLibrary.begin(session);
    },
    function (session, args) {
        // Process selected search results
        session.send(
            'Search Completed!',
            args.selection.map(  ); // format your response 
    }
```
<span data-ttu-id="8881e-141">Para más información sobre los diálogos, consulte [Manage a conversation with dialogs](bot-builder-nodejs-dialog-manage-conversation.md) (Administración de una conversación con diálogos).</span><span class="sxs-lookup"><span data-stu-id="8881e-141">For more information about dialogs, see [Manage a conversation with dialogs](bot-builder-nodejs-dialog-manage-conversation.md).</span></span>

## <a name="after-azure-search-responds"></a><span data-ttu-id="8881e-142">Después de que Azure Search responde</span><span class="sxs-lookup"><span data-stu-id="8881e-142">After Azure Search responds</span></span> 

<span data-ttu-id="8881e-143">Una vez que se resuelve una instancia correcta de Azure Search, ahora debe almacenar los datos que quiere desde el objeto de respuesta y mostrarlos de manera significativa para el usuario.</span><span class="sxs-lookup"><span data-stu-id="8881e-143">Once a successful Azure Search resolves, you now need to store the data you want from the response object, and display it in a meaningful way to the user.</span></span>

> [!TIP]
> Considere incluir el [módulo util][NodeUtil]. Lo ayudará a dar formato a la respuesta de Azure Search y a asignarla.

<span data-ttu-id="8881e-146">En el archivo de programa principal del bot, cree un método `ToSearchHit`.</span><span class="sxs-lookup"><span data-stu-id="8881e-146">In your bot's main program file, create a `ToSearchHit` method.</span></span> <span data-ttu-id="8881e-147">Este método devuelve un objeto que da formato a los datos relevantes que necesita de la respuesta de Azure.</span><span class="sxs-lookup"><span data-stu-id="8881e-147">This method returns an object which formats the relevant data you need from the Azure Response.</span></span> <span data-ttu-id="8881e-148">El código siguiente muestra cómo puede definir sus propios parámetros en el método `ToSearchHit`.</span><span class="sxs-lookup"><span data-stu-id="8881e-148">The following code shows how you can define your own parameters in the `ToSearchHit` method.</span></span> 
 
 ```javascript
 function ToSearchHit(azureResponse) {
     return {
         // define your own parameters 
         key: azureResponse.id,
         title: azureResponse.title,
         description: azureResponse.description,
         imageUrl: azureResponse.thumbnail
     };
 }
```
<span data-ttu-id="8881e-149">Una vez hecho esto, todo lo que tiene que hacer es mostrar los datos al usuario.</span><span class="sxs-lookup"><span data-stu-id="8881e-149">After this is done, all you need to do is display the data to the user.</span></span> 

 <span data-ttu-id="8881e-150">En el archivo **index.js** del proyecto **SearchDialogLibrary**, el método `searchHitAsCard` analiza cada respuesta de Azure Search y crea un nuevo objeto de tarjeta para mostrar al usuario.</span><span class="sxs-lookup"><span data-stu-id="8881e-150">In the **index.js** file of the **SearchDialogLibrary** project, the `searchHitAsCard` method parses each response from the Azure Search and creates a new card object to display to the user.</span></span> <span data-ttu-id="8881e-151">Los campos definidos en el método `ToSearchHit` desde el archivo de programa principal del bot se debe sincronizar con las propiedades en el método `searchHitAsCard`.</span><span class="sxs-lookup"><span data-stu-id="8881e-151">The fields you defined in the `ToSearchHit` method from your bot's main program file needs to synced with the properties in the `searchHitAsCard` method.</span></span> 

<span data-ttu-id="8881e-152">A continuación se muestra cómo y dónde se usan los parámetros definidos del método `ToSearchHit` para generar una interfaz de usuario de los datos adjuntos de la tarjeta para presentarlos al usuario.</span><span class="sxs-lookup"><span data-stu-id="8881e-152">The following shows how and where your defined parameters from the `ToSearchHit` method are used to build a card attachment UI to render to the user.</span></span> 

```javascript
function searchHitAsCard(showSave, searchHit) {
    var buttons = showSave
        ? [new builder.CardAction().type('imBack').title('Save').value(searchHit.key)]
        : [];

    var card = new builder.HeroCard()
        .title(searchHit.title) 
        .buttons(buttons);

    if (searchHit.description) {
        card.subtitle(searchHit.description);
    }

    if (searchHit.imageUrl) {
        card.images([new builder.CardImage().url(searchHit.imageUrl)]);
    }

    return card;
}
```

## <a name="sample-code"></a><span data-ttu-id="8881e-153">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="8881e-153">Sample code</span></span>

<span data-ttu-id="8881e-154">Para ver dos ejemplos completos que muestran cómo admitir Azure Search con bots usando el SDK de Bot Builder para Node.js, consulte el [ejemplo de bot de bienes inmuebles](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-Search/RealEstateBot) o el [ejemplo de bot de listas de trabajos](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-Search/JobListingBot) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="8881e-154">For two complete samples that show how to support Azure Search with bots using the Bot Builder SDK for Node.js, see the [Real Estate Bot sample](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-Search/RealEstateBot) or [Job Listing Bot sample](https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/demo-Search/JobListingBot) in GitHub.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="8881e-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8881e-155">Additional resources</span></span>

* <span data-ttu-id="8881e-156">[Azure Search][search]</span><span class="sxs-lookup"><span data-stu-id="8881e-156">[Azure Search][search]</span></span>
* <span data-ttu-id="8881e-157">[Nodo Util][NodeUtil]</span><span class="sxs-lookup"><span data-stu-id="8881e-157">[Node Util][NodeUtil]</span></span>
* [<span data-ttu-id="8881e-158">Diálogos</span><span class="sxs-lookup"><span data-stu-id="8881e-158">Dialogs</span></span>](bot-builder-nodejs-dialog-manage-conversation.md)

[NodeUtil]: https://nodejs.org/api/util.html
[search]: /azure/search/search-what-is-azure-search
