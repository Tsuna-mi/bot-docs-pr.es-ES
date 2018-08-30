---
title: Administrar datos de estado | Microsoft Docs
description: Obtenga información sobre cómo guardar y recuperar datos con el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3a924503ecadc9f56fa2543881c116f7fbbb4d9a
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904333"
---
# <a name="manage-state-data"></a><span data-ttu-id="20f4a-103">Administración de datos de estado</span><span class="sxs-lookup"><span data-stu-id="20f4a-103">Manage state data</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-state.md)
> - [Node.js](../nodejs/bot-builder-nodejs-state.md)

[!INCLUDE [State concept overview](../includes/snippet-dotnet-concept-state.md)]

## <a name="in-memory-data-storage"></a><span data-ttu-id="20f4a-106">Almacenamiento de datos en memoria</span><span class="sxs-lookup"><span data-stu-id="20f4a-106">In-memory data storage</span></span>

<span data-ttu-id="20f4a-107">El almacenamiento de datos en memoria solo está pensado para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="20f4a-107">In-memory data storage is intended for testing only.</span></span> <span data-ttu-id="20f4a-108">Este tipo de almacenamiento es volátil y temporal.</span><span class="sxs-lookup"><span data-stu-id="20f4a-108">This storage is volatile and temporary.</span></span> <span data-ttu-id="20f4a-109">Los datos se borran cada vez que se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="20f4a-109">The data is cleared each time the bot is restarted.</span></span> <span data-ttu-id="20f4a-110">Para usar el almacenamiento en la memoria para hacer pruebas, necesitará:</span><span class="sxs-lookup"><span data-stu-id="20f4a-110">To use the in-memory storage for testing purposes, you will need to:</span></span> 

<span data-ttu-id="20f4a-111">Instale los siguientes paquetes NuGet:</span><span class="sxs-lookup"><span data-stu-id="20f4a-111">Install the following NuGet packages:</span></span> 
- <span data-ttu-id="20f4a-112">Microsoft.Bot.Builder.Azure</span><span class="sxs-lookup"><span data-stu-id="20f4a-112">Microsoft.Bot.Builder.Azure</span></span>
- <span data-ttu-id="20f4a-113">Autofac.WebApi2</span><span class="sxs-lookup"><span data-stu-id="20f4a-113">Autofac.WebApi2</span></span>

<span data-ttu-id="20f4a-114">En el método **Application_Start**, cree una nueva instancia del almacenamiento en memoria y registre el nuevo almacén de datos:</span><span class="sxs-lookup"><span data-stu-id="20f4a-114">In the **Application_Start** method, create a new instance of the in-memory storage, and register the new data store:</span></span>

```cs
// Global.asax file

var store = new InMemoryDataStore();

Conversation.UpdateContainer(
           builder =>
           {
               builder.Register(c => store)
                         .Keyed<IBotDataStore<BotData>>(AzureModule.Key_DataStore)
                         .AsSelf()
                         .SingleInstance();

               builder.Register(c => new CachingBotDataStore(store,
                          CachingBotDataStoreConsistencyPolicy
                          .ETagBasedConsistency))
                          .As<IBotDataStore<BotData>>()
                          .AsSelf()
                          .InstancePerLifetimeScope();


           });
GlobalConfiguration.Configure(WebApiConfig.Register);

```

<span data-ttu-id="20f4a-115">Puede usar este método para establecer su propio almacenamiento de datos personalizado o usar cualquiera de las *extensiones de Azure*.</span><span class="sxs-lookup"><span data-stu-id="20f4a-115">You can use this method to set your own custom data storage or use any of the *Azure Extensions*.</span></span>

## <a name="manage-custom-data-storage"></a><span data-ttu-id="20f4a-116">Administración del almacenamiento de datos personalizado</span><span class="sxs-lookup"><span data-stu-id="20f4a-116">Manage custom data storage</span></span>

<span data-ttu-id="20f4a-117">Por motivos de seguridad y rendimiento en el entorno de producción, puede implementar su propio almacenamiento de datos o considerar la posibilidad de implementar una de las siguientes opciones de almacenamiento de datos:</span><span class="sxs-lookup"><span data-stu-id="20f4a-117">For performance and security reasons in the production environment, you may implement your own data storage or consider implementing one of the following data storage options:</span></span>

1. [<span data-ttu-id="20f4a-118">Administración de datos de estado con Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="20f4a-118">Manage state data with Cosmos DB</span></span>](bot-builder-dotnet-state-azure-cosmosdb.md)

2. [<span data-ttu-id="20f4a-119">Administración de datos de estado con Table Storage</span><span class="sxs-lookup"><span data-stu-id="20f4a-119">Manage state data with Table storage</span></span>](bot-builder-dotnet-state-azure-table-storage.md)

<span data-ttu-id="20f4a-120">Con cualquiera de estas opciones de las [extensiones de Azure](https://www.nuget.org/packages/Microsoft.Bot.Builder.Azure/), el mecanismo para establecer y conservar los datos a través del SDK de Bot Framework para .NET es el mismo que el del almacenamiento de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="20f4a-120">With either of these [Azure Extensions](https://www.nuget.org/packages/Microsoft.Bot.Builder.Azure/) options, the mechanism for setting and persisting data via the Bot Framework SDK for .NET remains the same as the in-memory data storage.</span></span>

## <a name="bot-state-methods"></a><span data-ttu-id="20f4a-121">Métodos de estado del bot</span><span class="sxs-lookup"><span data-stu-id="20f4a-121">Bot state methods</span></span>

<span data-ttu-id="20f4a-122">En esta tabla se enumeran los métodos que se pueden usar para administrar los datos de estado.</span><span class="sxs-lookup"><span data-stu-id="20f4a-122">This table lists the methods that you can use to manage state data.</span></span>

| <span data-ttu-id="20f4a-123">Método</span><span class="sxs-lookup"><span data-stu-id="20f4a-123">Method</span></span> | <span data-ttu-id="20f4a-124">Ámbito</span><span class="sxs-lookup"><span data-stu-id="20f4a-124">Scoped to</span></span> | <span data-ttu-id="20f4a-125">Objetivo</span><span class="sxs-lookup"><span data-stu-id="20f4a-125">Objective</span></span> |                                                
|----|----|----|
| `GetUserData` | <span data-ttu-id="20f4a-126">Usuario</span><span class="sxs-lookup"><span data-stu-id="20f4a-126">User</span></span> | <span data-ttu-id="20f4a-127">Obtener datos de estado, guardados previamente, del usuario de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-127">Get state data that has previously been saved for the user on the specified channel</span></span> |
| `GetConversationData` | <span data-ttu-id="20f4a-128">Conversation</span><span class="sxs-lookup"><span data-stu-id="20f4a-128">Conversation</span></span> | <span data-ttu-id="20f4a-129">Obtener datos de estado, guardados previamente, de la conversación de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-129">Get state data that has previously been saved for the conversation on the specified channel</span></span> |
| `GetPrivateConversationData` | <span data-ttu-id="20f4a-130">Usuario y de conversación</span><span class="sxs-lookup"><span data-stu-id="20f4a-130">User and Conversation</span></span> | <span data-ttu-id="20f4a-131">Obtener datos de estado, guardados previamente, del usuario de una conversación de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-131">Get state data that has previously been saved for the user within the conversation on the specified channel</span></span> |
| `SetUserData` | <span data-ttu-id="20f4a-132">Usuario</span><span class="sxs-lookup"><span data-stu-id="20f4a-132">User</span></span> | <span data-ttu-id="20f4a-133">Guardar datos de estado del usuario de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-133">Save state data for the user on the specified channel</span></span> |
| `SetConversationData` | <span data-ttu-id="20f4a-134">Conversation</span><span class="sxs-lookup"><span data-stu-id="20f4a-134">Conversation</span></span> | <span data-ttu-id="20f4a-135">Guardar datos de estado de la conversación de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-135">Save state data for the conversation on the specified channel.</span></span> <br/><br/><span data-ttu-id="20f4a-136">**Nota**: Como el método `DeleteStateForUser` no elimina los datos que se han almacenado mediante el método `SetConversationData`, NO debe usar este método para almacenar información personal identificable (PII) de un usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-136">**Note**: Because the `DeleteStateForUser` method does not delete data that has been stored using the `SetConversationData` method, you must NOT use this method to store a user's personally identifiable information (PII).</span></span> |
| `SetPrivateConversationData` | <span data-ttu-id="20f4a-137">Usuario y de conversación</span><span class="sxs-lookup"><span data-stu-id="20f4a-137">User and Conversation</span></span> | <span data-ttu-id="20f4a-138">Guardar datos de estado del usuario en una conversación de un canal específico.</span><span class="sxs-lookup"><span data-stu-id="20f4a-138">Save state data for the user within the conversation on the specified channel</span></span> |
| `DeleteStateForUser` | <span data-ttu-id="20f4a-139">Usuario</span><span class="sxs-lookup"><span data-stu-id="20f4a-139">User</span></span> | <span data-ttu-id="20f4a-140">Eliminar datos de estado del usuario, que se hayan almacenado previamente mediante el método `SetUserData` o `SetPrivateConversationData`.</span><span class="sxs-lookup"><span data-stu-id="20f4a-140">Delete state data for the user that has previously been stored by using either the `SetUserData` method or the `SetPrivateConversationData` method.</span></span> <br/><br/><span data-ttu-id="20f4a-141">**Nota**: El bot debe llamar a este método cuando reciba una actividad de tipo [deleteUserData](bot-builder-dotnet-activities.md#deleteuserdata) o una actividad de tipo [contactRelationUpdate](bot-builder-dotnet-activities.md#contactrelationupdate) que indique que el bot ha sido eliminado de la lista de contactos del usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-141">**Note**: Your bot should call this method when it receives an activity of type [deleteUserData](bot-builder-dotnet-activities.md#deleteuserdata) or an activity of type [contactRelationUpdate](bot-builder-dotnet-activities.md#contactrelationupdate) that indicates the bot has been removed from the user's contact list.</span></span> |

<span data-ttu-id="20f4a-142">Si el bot guarda los datos de estado mediante uno de los métodos "**Set...Data**", los mensajes futuros que reciba el bot en el mismo contexto contendrán esos datos, a los que el bot puede acceder con el correspondiente método "**Get...Data**".</span><span class="sxs-lookup"><span data-stu-id="20f4a-142">If your bot saves state data by using one of the "**Set...Data**" methods, future messages that your bot receives in the same context will contain that data, which your bot can access by using the corresponding "**Get...Data**" method.</span></span>

## <a name="useful-properties-for-managing-state-data"></a><span data-ttu-id="20f4a-143">Propiedades útiles para administrar los datos de estado</span><span class="sxs-lookup"><span data-stu-id="20f4a-143">Useful properties for managing state data</span></span>

<span data-ttu-id="20f4a-144">Cada objeto [Activity][Activity] contiene propiedades que usará para administrar datos de estado.</span><span class="sxs-lookup"><span data-stu-id="20f4a-144">Each [Activity][Activity] object contains properties that you will use to manage state data.</span></span>

| <span data-ttu-id="20f4a-145">Propiedad</span><span class="sxs-lookup"><span data-stu-id="20f4a-145">Property</span></span> | <span data-ttu-id="20f4a-146">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="20f4a-146">Description</span></span> | <span data-ttu-id="20f4a-147">Caso de uso</span><span class="sxs-lookup"><span data-stu-id="20f4a-147">Use case</span></span> |
|----|----|----|
| `From` | <span data-ttu-id="20f4a-148">Identifica un usuario en un canal de forma única.</span><span class="sxs-lookup"><span data-stu-id="20f4a-148">Uniquely identifies a user on a channel</span></span> | <span data-ttu-id="20f4a-149">Almacenar y recuperar datos de estado que estén asociados a un usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-149">Storing and retrieving state data that is associated with a user</span></span> |
| `Conversation` | <span data-ttu-id="20f4a-150">Identifica una conversación de forma única.</span><span class="sxs-lookup"><span data-stu-id="20f4a-150">Uniquely identifies a conversation</span></span> | <span data-ttu-id="20f4a-151">Almacenar y recuperar datos de estado que estén asociados a una conversación.</span><span class="sxs-lookup"><span data-stu-id="20f4a-151">Storing and retrieving state data that is associated with a conversation</span></span> |
| <span data-ttu-id="20f4a-152">`From` y `Conversation`</span><span class="sxs-lookup"><span data-stu-id="20f4a-152">`From` and `Conversation`</span></span> | <span data-ttu-id="20f4a-153">Identifica un usuario y una conversación de forma única.</span><span class="sxs-lookup"><span data-stu-id="20f4a-153">Uniquely identifies a user and conversation</span></span> | <span data-ttu-id="20f4a-154">Almacenar y recuperar datos de estado que estén asociados a un usuario específico dentro del contexto de una conversación concreta.</span><span class="sxs-lookup"><span data-stu-id="20f4a-154">Storing and retrieving state data that is associated with a specific user within the context of a specific conversation</span></span> |

> [!NOTE]
> Puede usar estos valores de propiedad como claves, incluso si decide almacenar los datos de estado en su propia base de datos, en lugar de usar el almacén de datos de estado de Bot Framework.

## <a id="state-client"></a> <span data-ttu-id="20f4a-156">Crear un cliente de estado</span><span class="sxs-lookup"><span data-stu-id="20f4a-156">Create a state client</span></span>

<span data-ttu-id="20f4a-157">El objeto `StateClient` le permite administrar datos de estado mediante el SDK de Bot Builder para .NET.</span><span class="sxs-lookup"><span data-stu-id="20f4a-157">The `StateClient` object enables you to manage state data using the Bot Builder SDK for .NET.</span></span> <span data-ttu-id="20f4a-158">Si tiene acceso a un mensaje que pertenece al mismo contexto en el que quiere administrar los datos de estado, puede crear un cliente de estado llamando al método `GetStateClient` en el objeto `Activity`.</span><span class="sxs-lookup"><span data-stu-id="20f4a-158">If you have access to a message that belongs to the same context in which you want to manage state data, you can create a state client by calling the `GetStateClient` method on the `Activity` object.</span></span>

[!code-csharp[Get State client](../includes/code/dotnet-state.cs#getStateClient1)]

<span data-ttu-id="20f4a-159">Si no tiene acceso a un mensaje que pertenece al mismo contexto en el que desea administrar los datos de estado, puede crear un cliente de estado creando una nueva instancia de la clase `StateClient`.</span><span class="sxs-lookup"><span data-stu-id="20f4a-159">If you do not have access to a message that belongs to the same context in which you want to manage state data, you can create a state client by simply creating a new instance of the `StateClient` class.</span></span> <span data-ttu-id="20f4a-160">En este ejemplo, `microsoftAppId` y `microsoftAppPassword` son las credenciales de autenticación de Bot Framework que adquiere para su bot durante el proceso de [creación del bot](../bot-service-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="20f4a-160">In this example, `microsoftAppId` and `microsoftAppPassword` are the Bot Framework authentication credentials that you acquire for your bot during the [bot creation](../bot-service-quickstart.md) process.</span></span>

> [!NOTE]
> Para buscar los valores **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).

[!code-csharp[Get State client](../includes/code/dotnet-state.cs#getStateClient2)]

> [!NOTE]
> El cliente de estado predeterminado se almacena en un servicio central. Para algunos canales, es posible que quiera utilizar una API de estado alojada en el canal, de modo que los datos de estado se puedan almacenar en un almacén compatible que suministre el canal.

## <a name="get-state-data"></a><span data-ttu-id="20f4a-164">Obtener los datos de estado</span><span class="sxs-lookup"><span data-stu-id="20f4a-164">Get state data</span></span>

<span data-ttu-id="20f4a-165">Cada uno de los métodos "**Get...Data**" devuelve un objeto `BotData` que contiene los datos de estado del usuario o la conversación especificados.</span><span class="sxs-lookup"><span data-stu-id="20f4a-165">Each of the "**Get...Data**" methods returns a `BotData` object that contains the state data for the specified user and/or conversation.</span></span> <span data-ttu-id="20f4a-166">Para obtener un valor de propiedad específico de un objeto `BotData`, llame al método `GetProperty`.</span><span class="sxs-lookup"><span data-stu-id="20f4a-166">To get a specific property value from a `BotData` object, call the `GetProperty` method.</span></span> 

<span data-ttu-id="20f4a-167">En el siguiente ejemplo de código se muestra cómo obtener una propiedad escrita a partir de los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-167">The following code example shows how to get a typed property from user data.</span></span> 

[!code-csharp[Get state property](../includes/code/dotnet-state.cs#getProperty1)]

<span data-ttu-id="20f4a-168">En el siguiente ejemplo de código se muestra cómo obtener una propiedad de un tipo complejo a partir de los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-168">The following code example shows how to get a property from a complex type within user data.</span></span>

[!code-csharp[Get state property](../includes/code/dotnet-state.cs#getProperty2)]

<span data-ttu-id="20f4a-169">Si no existen datos de estado para el usuario o la conversación que se especifica en una llamada del método "**Get...Data**", el objeto `BotData` que se devuelve contendrá estos valores de propiedad:</span><span class="sxs-lookup"><span data-stu-id="20f4a-169">If no state data exists for the user and/or conversation that is specified for a "**Get...Data**" method call, the `BotData` object that is returned will contain these property values:</span></span> 
- <span data-ttu-id="20f4a-170">`BotData.Data` = null</span><span class="sxs-lookup"><span data-stu-id="20f4a-170">`BotData.Data` = null</span></span>
- <span data-ttu-id="20f4a-171">`BotData.ETag` = "\*"</span><span class="sxs-lookup"><span data-stu-id="20f4a-171">`BotData.ETag` = "\*"</span></span>

## <a name="save-state-data"></a><span data-ttu-id="20f4a-172">Guardar los datos</span><span class="sxs-lookup"><span data-stu-id="20f4a-172">Save state data</span></span>

<span data-ttu-id="20f4a-173">Para guardar datos de estado, primero obtenga el objeto `BotData` llamando al método apropiado "**Get...Data**"; a continuación, actualícelo llamando al método `SetProperty` para cada propiedad que quiera actualizar, y guárdelo llamando al método apropiado "**Set...Data**".</span><span class="sxs-lookup"><span data-stu-id="20f4a-173">To save state data, first get the `BotData` object by calling the appropriate "**Get...Data**" method, then update it by calling the `SetProperty` method for each property you want to update, and save it by calling the appropriate "**Set...Data**" method.</span></span> 

> [!NOTE]
> Puede almacenar hasta 32 KB de datos para cada usuario en un canal, cada conversación en un canal y cada usuario en el contexto de una conversación en un canal. 

<span data-ttu-id="20f4a-175">En el siguiente ejemplo de código se muestra cómo guardar una propiedad escrita a partir de los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-175">The following code example shows how to save a typed property in user data.</span></span>

[!code-csharp[Set state property](../includes/code/dotnet-state.cs#setProperty1)]

<span data-ttu-id="20f4a-176">En el siguiente ejemplo de código se muestra cómo guardar una propiedad de un tipo complejo a partir de los datos del usuario.</span><span class="sxs-lookup"><span data-stu-id="20f4a-176">The following code example shows how to save a property in a complex type within user data.</span></span> 

[!code-csharp[Set state property](../includes/code/dotnet-state.cs#setProperty2)]

## <a name="handle-concurrency-issues"></a><span data-ttu-id="20f4a-177">Administrar los problemas de simultaneidad</span><span class="sxs-lookup"><span data-stu-id="20f4a-177">Handle concurrency issues</span></span>

<span data-ttu-id="20f4a-178">El bot puede recibir una respuesta de error con el código de estado HTTP **412 Error de condición previa** cuando intenta guardar datos de estado, si es que otra instancia del bot ha cambiado los datos.</span><span class="sxs-lookup"><span data-stu-id="20f4a-178">Your bot may receive an error response with HTTP status code **412 Precondition Failed** when it attempts to save state data, if another instance of the bot has changed the data.</span></span> <span data-ttu-id="20f4a-179">Puede diseñar su bot para que tenga en cuenta este escenario, tal como se muestra en el siguiente ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="20f4a-179">You can design your bot to account for this scenario, as shown in the following code example.</span></span>

[!code-csharp[Handle exception saving state](../includes/code/dotnet-state.cs#handleException)]

## <a name="additional-resources"></a><span data-ttu-id="20f4a-180">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="20f4a-180">Additional resources</span></span>

- <span data-ttu-id="20f4a-181">[Bot Framework troubleshooting guide](../bot-service-troubleshoot-general-problems.md) (Guía de solución de problemas de Bot Framework)</span><span class="sxs-lookup"><span data-stu-id="20f4a-181">[Bot Framework troubleshooting guide](../bot-service-troubleshoot-general-problems.md)</span></span>
- <span data-ttu-id="20f4a-182"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a> (Referencias del SDK de Bot Builder para .NET)</span><span class="sxs-lookup"><span data-stu-id="20f4a-182"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[Activity]: https://docs.botframework.com/en-us/csharp/builder/sdkreference/dc/d2f/class_microsoft_1_1_bot_1_1_connector_1_1_activity.html
