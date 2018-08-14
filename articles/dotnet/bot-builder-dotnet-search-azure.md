---
title: Creación de experiencias controladas por datos con Azure Search | Microsoft Docs
description: Aprenda a crear experiencias controladas por datos con Azure Search y ayude a los usuarios a navegar por grandes cantidades de contenido en un bot con el Bot Builder SDK para .NET y Azure Search.
author: matthewshim-ms
ms.author: v-shimma
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: c8eb1f300dbf1ad8efd9f683a2776958558ca2f2
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39306261"
---
# <a name="create-data-driven-experiences-with-azure-search"></a><span data-ttu-id="5c43a-103">Creación de experiencias controladas por datos con Azure Search</span><span class="sxs-lookup"><span data-stu-id="5c43a-103">Create data-driven experiences with Azure Search</span></span> 
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-search-azure.md)
> - [Node.js](../nodejs/bot-builder-nodejs-search-azure.md)

<span data-ttu-id="5c43a-106">Puede agregar [Azure Search](https://azure.microsoft.com/en-us/services/search/) a un bot para ayudar a los usuarios a navegar por grandes cantidades de contenido y crear una experiencia de exploración controlada por datos.</span><span class="sxs-lookup"><span data-stu-id="5c43a-106">You can add [Azure Search](https://azure.microsoft.com/en-us/services/search/) to a bot to help users navigate large amounts of content and create a data-driven exploration experience.</span></span>

<span data-ttu-id="5c43a-107">Azure Search es un servicio de Azure que ofrece búsqueda de palabras clave, lingüística integrada, puntuación personalizada, navegación por facetas y mucho más.</span><span class="sxs-lookup"><span data-stu-id="5c43a-107">Azure Search is an Azure service that offers keyword search, built-in linguistics, custom scoring, faceted navigation, and more.</span></span> <span data-ttu-id="5c43a-108">Azure Search también puede indexar el contenido desde diversos orígenes, incluidos Azure SQL DB, DocumentDB, Blob Storage y Table Storage.</span><span class="sxs-lookup"><span data-stu-id="5c43a-108">Azure Search can also index content from various sources, including Azure SQL DB, DocumentDB, Blob Storage, and Table Storage.</span></span> <span data-ttu-id="5c43a-109">Es compatible con la indexación "push" para otros orígenes de datos y puede abrir archivos PDF, documentos de Office y otros formatos que contengan datos no estructurados.</span><span class="sxs-lookup"><span data-stu-id="5c43a-109">It supports "push" indexing for other sources of data, and it can open PDFs, Office documents, and other formats containing unstructured data.</span></span> <span data-ttu-id="5c43a-110">Una vez recopilado, el contenido se coloca en un índice de Azure Search, al que luego el bot puede consultar.</span><span class="sxs-lookup"><span data-stu-id="5c43a-110">Once collected, the content goes into an Azure Search index, which the bot can then query.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="5c43a-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5c43a-111">Prerequisites</span></span>

<span data-ttu-id="5c43a-112">Instale el paquete de Nuget [Microsoft.Azure.Search](https://www.nuget.org/packages/Microsoft.Azure.Search/4.0.0-preview) en el proyecto de bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-112">Install the [Microsoft.Azure.Search](https://www.nuget.org/packages/Microsoft.Azure.Search/4.0.0-preview) Nuget package in your bot project.</span></span> 

<span data-ttu-id="5c43a-113">Los siguientes tres proyectos de C# son necesarios en la solución del bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-113">The following three C# projects are required in your bot's solution.</span></span> <span data-ttu-id="5c43a-114">Estos proyectos proporcionan funcionalidad adicional para los bots y Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5c43a-114">These projects provide additional functionality for bots and Azure Search.</span></span> <span data-ttu-id="5c43a-115">Bifurque los proyectos de [GitHub](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search) o descargue el código fuente directamente.</span><span class="sxs-lookup"><span data-stu-id="5c43a-115">Fork the projects from [GitHub](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search) or download the source code directly.</span></span>

* <span data-ttu-id="5c43a-116">[Search.Azure](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Azure) define la llamada al servicio de Azure.</span><span class="sxs-lookup"><span data-stu-id="5c43a-116">[Search.Azure](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Azure) defines the Azure Service call.</span></span> 
* <span data-ttu-id="5c43a-117">[Search.Contracts](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Contracts) define los modelos de datos e interfaces genéricos para controlar los datos.</span><span class="sxs-lookup"><span data-stu-id="5c43a-117">[Search.Contracts](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Contracts) defines generic interfaces and data models to handle data.</span></span>
* <span data-ttu-id="5c43a-118">[Search.Dialogs](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Dialogs) incluye diversos cuadros de diálogo genéricos de Bot Builder usados para consultas en Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5c43a-118">[Search.Dialogs](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search/Search.Dialogs) includes various generic Bot Builder dialogs used to query Azure Search.</span></span>

## <a name="configure-azure-search-settings"></a><span data-ttu-id="5c43a-119">Configuración de las opciones de Azure Search</span><span class="sxs-lookup"><span data-stu-id="5c43a-119">Configure Azure Search settings</span></span> 

<span data-ttu-id="5c43a-120">Configure las opciones de Azure Search en el archivo **Web.config** del proyecto con sus propias credenciales de Azure Search en los campos de valor.</span><span class="sxs-lookup"><span data-stu-id="5c43a-120">Configure the Azure Search settings in the **Web.config** file of the project using your own Azure Search credentials in the value fields.</span></span> <span data-ttu-id="5c43a-121">El constructor en la clase `AzureSearchClient` usará estos valores para registrar y enlazar el bot con el servicio de Azure.</span><span class="sxs-lookup"><span data-stu-id="5c43a-121">The constructor in the `AzureSearchClient` class will use these settings to register and bind the bot to the Azure Service.</span></span>

```xml
<appSettings>
    <add key="SearchDialogsServiceName" value="Azure-Search-Service-Name" /> <!-- replace value field with Azure Service Name --> 
    <add key="SearchDialogsServiceKey" value="Azure-Search-Service-Primary-Key" /> <!-- replace value field with Azure Service Key --> 
    <add key="SearchDialogsIndexName" value="Azure-Search-Service-Index" /> <!-- replace value field with your Azure Search Index --> 
</appSettings>
```

## <a name="create-a-search-dialog"></a><span data-ttu-id="5c43a-122">Creación de un cuadro de diálogo de búsqueda</span><span class="sxs-lookup"><span data-stu-id="5c43a-122">Create a search dialog</span></span>

<span data-ttu-id="5c43a-123">En el proyecto del bot, cree una nueva clase `AzureSearchDialog` para llamar al servicio de Azure en el bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-123">In your bot's project, create a new `AzureSearchDialog` class to call the Azure Service in your bot.</span></span> <span data-ttu-id="5c43a-124">Esta nueva clase debe heredar la clase `SearchDialog` del proyecto **Search.Dialogs**, que controla la mayoría del trabajo pesado.</span><span class="sxs-lookup"><span data-stu-id="5c43a-124">This new class must inherit the `SearchDialog` class from the **Search.Dialogs** project, which handles most of the heavy lifting.</span></span> <span data-ttu-id="5c43a-125">La invalidación `GetTopRefiners()` permite a los usuarios afinar o filtrar los resultados de la búsqueda sin tener que volver a iniciar la búsqueda desde el principio, lo que mantiene el estado del objeto de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="5c43a-125">The `GetTopRefiners()` override allows users to narrow/filter their search results without having to start the search over form the beginning, maintaining the search object's state.</span></span> <span data-ttu-id="5c43a-126">Puede agregar sus propios refinadores personalizados en la matriz `TopRefiners` para permitir que los usuarios filtren o restrinjan los resultados de la búsqueda.</span><span class="sxs-lookup"><span data-stu-id="5c43a-126">You can add your own custom refiners in the `TopRefiners` array to let your users filter or narrow down their search results.</span></span> 

```cs
[Serializable]
public class AzureSearchDialog : SearchDialog
{
    private static readonly string[] TopRefiners = { "refiner1", "refiner2", "refiner3" }; // define your own custom refiners 

    public AzureSearchDialog(ISearchClient searchClient) : base(searchClient, multipleSelection: true)
    {
    }

    protected override string[] GetTopRefiners()
    {
        return TopRefiners;
    }
}
```

## <a name="define-the-response-data-model"></a><span data-ttu-id="5c43a-127">Definición del modelo de datos de respuesta</span><span class="sxs-lookup"><span data-stu-id="5c43a-127">Define the response data model</span></span>

<span data-ttu-id="5c43a-128">La clase **SearchHit.cs** dentro del proyecto `Search.Contracts` define los datos pertinentes que deben analizarse a partir de la respuesta de Azure Search.</span><span class="sxs-lookup"><span data-stu-id="5c43a-128">The **SearchHit.cs** class within the `Search.Contracts` project defines the relevant data to be parsed from the Azure Search response.</span></span> <span data-ttu-id="5c43a-129">Para el bot, las únicas inclusiones obligatorias son la declaración de IDictionary `PropertyBag` y la creación en el constructor.</span><span class="sxs-lookup"><span data-stu-id="5c43a-129">For your bot the only mandatory inclusions are the `PropertyBag` IDictionary declaration and creation in the constructor.</span></span> <span data-ttu-id="5c43a-130">Puede definir todas las demás propiedades de esta clase en relación con las necesidades de su bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-130">You can define all other properties in this class relative to your bot's needs.</span></span> 

```cs
[Serializable]
public class SearchHit
{
    public SearchHit()
    {
        this.PropertyBag = new Dictionary<string, object>();
    }

    public IDictionary<string, object> PropertyBag { get; set; }

    // customize the fields below as needed 
    public string Key { get; set; }

    public string Title { get; set; }

    public string PictureUrl { get; set; }

    public string Description { get; set; }
}
```

## <a name="after-azure-search-responds"></a><span data-ttu-id="5c43a-131">Después de que Azure Search responde</span><span class="sxs-lookup"><span data-stu-id="5c43a-131">After Azure Search responds</span></span> 

<span data-ttu-id="5c43a-132">Tras una consulta correcta al servicio de Azure, el resultado de la búsqueda deberá analizarse para recuperar los datos pertinentes para que el bot los muestre al usuario.</span><span class="sxs-lookup"><span data-stu-id="5c43a-132">Upon a successful query to the Azure Service, the search result will need to be parsed to retrieve the relevant data for the bot to display to the user.</span></span> <span data-ttu-id="5c43a-133">Para habilitar esta opción, deberá crear una clase `SearchResultMapper`.</span><span class="sxs-lookup"><span data-stu-id="5c43a-133">To enable this, you'll need to create a `SearchResultMapper` class.</span></span> <span data-ttu-id="5c43a-134">El objeto `GenericSearchResult` creado en el constructor define una lista y diccionario para almacenar los resultados y las facetas, respectivamente, después de que cada resultado se analice con respecto a los modelos de datos de su bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-134">The `GenericSearchResult` object created in the constructor defines a list and dictionary to store results and facets respectively after each result is parsed respective to your bot's data models.</span></span> 

<span data-ttu-id="5c43a-135">Sincronice las propiedades en el método `ToSearchHit` para que coincidan con el modelo de datos en **SearchHit.cs**.</span><span class="sxs-lookup"><span data-stu-id="5c43a-135">Synchronize the properties in the `ToSearchHit` method to match the data model in **SearchHit.cs**.</span></span> <span data-ttu-id="5c43a-136">El método `ToSearchHit` se ejecutará y generará un nuevo `SearchHit` para cada resultado que se encuentre en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="5c43a-136">The `ToSearchHit` method will be executed and generate a new `SearchHit` for every result found in the response.</span></span>  

```cs
public class SearchResultMapper : IMapper<DocumentSearchResult, GenericSearchResult>
{
    public GenericSearchResult Map(DocumentSearchResult documentSearchResult)
    {
        var searchResult = new GenericSearchResult();

        searchResult.Results = documentSearchResult.Results.Select(r => ToSearchHit(r)).ToList();
        searchResult.Facets = documentSearchResult.Facets?.ToDictionary(kv => kv.Key, kv => kv.Value.Select(f => ToFacet(f)));

        return searchResult;
    }

    private static GenericFacet ToFacet(FacetResult facetResult)
    {
        return new GenericFacet
        {
            Value = facetResult.Value,
            Count = facetResult.Count.Value
        };
    }

    private static SearchHit ToSearchHit(SearchResult hit)
    {
        return new SearchHit
        {
            // custom properties defined in SearchHit.cs 
            Key = (string)hit.Document["id"],
            Title = (string)hit.Document["title"],
            PictureUrl = (string)hit.Document["thumbnail"],
            Description = (string)hit.Document["description"]
        };
    }
}
```
<span data-ttu-id="5c43a-137">Una vez que se analicen y se almacenen los resultados, la información deberá mostrarse al usuario.</span><span class="sxs-lookup"><span data-stu-id="5c43a-137">After the results are parsed and stored, the information still needs to be displayed to the user.</span></span> <span data-ttu-id="5c43a-138">La clase `SearchHitStyler` deberá administrarse para que se adapte al modelo de datos de la clase `SearchHit`.</span><span class="sxs-lookup"><span data-stu-id="5c43a-138">The `SearchHitStyler` class will need to be managed to accommodate the your data model from the `SearchHit` class.</span></span> <span data-ttu-id="5c43a-139">Por ejemplo, las propiedades `Title`, `PictureUrl` y `Description` de la clase **SearchHit.cs** se utilizan en el código de ejemplo para crear adjuntos de una nueva tarjeta.</span><span class="sxs-lookup"><span data-stu-id="5c43a-139">For example, the `Title`, `PictureUrl`, and `Description` properties from the **SearchHit.cs** class are used in the sample code to create a new card attachments.</span></span> <span data-ttu-id="5c43a-140">El código siguiente crea un nuevo archivo adjunto de tarjeta para cada objeto `SearchHit` en la lista de resultados `GenericSearchResult` para mostrar al usuario.</span><span class="sxs-lookup"><span data-stu-id="5c43a-140">The following code creates a new card attachment for every `SearchHit` object in the  `GenericSearchResult` Results list to display to the user.</span></span>   

```cs
[Serializable]
public class SearchHitStyler : PromptStyler
{
    public override void Apply<T>(ref IMessageActivity message, string prompt, IReadOnlyList<T> options, IReadOnlyList<string> descriptions = null)
    {
        var hits = options as IList<SearchHit>;
        if (hits != null)
        {
            var cards = hits.Select(h => new ThumbnailCard
            {
                Title = h.Title,
                Images = new[] { new CardImage(h.PictureUrl) },
                Buttons = new[] { new CardAction(ActionTypes.ImBack, "Pick this one", value: h.Key) },
                Text = h.Description
            });

            message.AttachmentLayout = AttachmentLayoutTypes.Carousel;
            message.Attachments = cards.Select(c => c.ToAttachment()).ToList();
            message.Text = prompt;
        }
        else
        {
            base.Apply<T>(ref message, prompt, options, descriptions);
        }
    }
}
```
<span data-ttu-id="5c43a-141">Los resultados de la búsqueda se muestran al usuario, y Azure Search se habrá agregado correctamente al bot.</span><span class="sxs-lookup"><span data-stu-id="5c43a-141">The search results are displayed to the user and you've successfully added Azure Search to your bot.</span></span>

## <a name="samples"></a><span data-ttu-id="5c43a-142">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="5c43a-142">Samples</span></span>

<span data-ttu-id="5c43a-143">Para obtener dos ejemplos completos que muestran cómo admitir Azure Search con bots usando el Bot Builder SDK para. NET, consulte el [ejemplo de bot de bienes inmuebles](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search/RealEstateBot) o el [ejemplo de bot de listas de trabajos](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search/JobListingBot) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="5c43a-143">For two complete samples that show how to support Azure Search with bots using the Bot Builder SDK for .NET, see the [Real Estate bot sample](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search/RealEstateBot) or [Job Listing bot sample](https://github.com/Microsoft/BotBuilder-Samples/tree/master/CSharp/demo-Search/JobListingBot) in GitHub.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="5c43a-144">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5c43a-144">Additional resources</span></span>
* <span data-ttu-id="5c43a-145">[Azure Search][search]</span><span class="sxs-lookup"><span data-stu-id="5c43a-145">[Azure Search][search]</span></span>
* [<span data-ttu-id="5c43a-146">Introducción a los diálogos</span><span class="sxs-lookup"><span data-stu-id="5c43a-146">Dialogs overview</span></span>](bot-builder-dotnet-dialogs.md)
* [<span data-ttu-id="5c43a-147">Ejemplos de bot de Azure Search</span><span class="sxs-lookup"><span data-stu-id="5c43a-147">Azure Search bot samples</span></span>](https://github.com/Microsoft/botBuilder-Samples/tree/master/CSharp/demo-Search)

[search]: /azure/search/search-what-is-azure-search