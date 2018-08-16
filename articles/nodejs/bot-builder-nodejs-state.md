---
title: Administración de datos de estado | Microsoft Docs
description: Obtenga información sobre cómo guardar y recuperar datos de estado con Bot Builder SDK para Node.js.
author: DucVo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 1001f1aa2fe76127073551e98548fc20ef9e1bd7
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305564"
---
# <a name="manage-state-data"></a><span data-ttu-id="9fc4b-103">Administración de datos de estado</span><span class="sxs-lookup"><span data-stu-id="9fc4b-103">Manage state data</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-state.md)
> - [Node.js](../nodejs/bot-builder-nodejs-state.md)

[!INCLUDE [State concept overview](../includes/snippet-dotnet-concept-state.md)]

## <a name="in-memory-data-storage"></a><span data-ttu-id="9fc4b-106">Almacenamiento de datos en memoria</span><span class="sxs-lookup"><span data-stu-id="9fc4b-106">In-memory data storage</span></span>

<span data-ttu-id="9fc4b-107">El almacenamiento de datos en memoria solo está pensado para realizar pruebas.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-107">In-memory data storage is intended for testing only.</span></span> <span data-ttu-id="9fc4b-108">Este tipo de almacenamiento es volátil y temporal.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-108">This storage is volatile and temporary.</span></span> <span data-ttu-id="9fc4b-109">Los datos se borran cada vez que se reinicia el bot.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-109">The data is cleared each time the bot is restarted.</span></span> <span data-ttu-id="9fc4b-110">Para usar el almacenamiento en memoria con fines de prueba, deberá hacer dos cosas.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-110">To use the in-memory storage for testing purposes, you will need to do two things.</span></span> <span data-ttu-id="9fc4b-111">En primer lugar, cree una nueva instancia del almacenamiento en memoria:</span><span class="sxs-lookup"><span data-stu-id="9fc4b-111">First create a new instance of the in-memory storage:</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();
```

<span data-ttu-id="9fc4b-112">A continuación, establézcalo en el bot cuando cree el **UniversalBot**:</span><span class="sxs-lookup"><span data-stu-id="9fc4b-112">Then, set it to the bot when you create the **UniversalBot**:</span></span>

```javascript
var inMemoryStorage = new builder.MemoryBotStorage();
var bot = new builder.UniversalBot(connector, [..waterfall steps..])
                    .set('storage', inMemoryStorage); // Register in-memory storage 
```

<span data-ttu-id="9fc4b-113">Puede usar este método para establecer su propio almacenamiento de datos personalizado o usar cualquiera de las *extensiones de Azure*.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-113">You can use this method to set your own custom data storage or use any of the *Azure Extensions*.</span></span>

## <a name="manage-custom-data-storage"></a><span data-ttu-id="9fc4b-114">Administración del almacenamiento de datos personalizado</span><span class="sxs-lookup"><span data-stu-id="9fc4b-114">Manage custom data storage</span></span>

<span data-ttu-id="9fc4b-115">Por motivos de seguridad y rendimiento en el entorno de producción, puede implementar su propio almacenamiento de datos o considerar la posibilidad de implementar una de las siguientes opciones de almacenamiento de datos:</span><span class="sxs-lookup"><span data-stu-id="9fc4b-115">For performance and security reasons in the production environment, you may implement your own data storage or consider implementing one of the following data storage options:</span></span>

1. [<span data-ttu-id="9fc4b-116">Administración de datos de estado con Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="9fc4b-116">Manage state data with Cosmos DB</span></span>](bot-builder-nodejs-state-azure-cosmosdb.md)

2. [<span data-ttu-id="9fc4b-117">Administración de datos de estado con Table Storage</span><span class="sxs-lookup"><span data-stu-id="9fc4b-117">Manage state data with Table storage</span></span>](bot-builder-nodejs-state-azure-table-storage.md)

<span data-ttu-id="9fc4b-118">Con cualquiera de estas opciones de las [extensiones de Azure](https://www.npmjs.com/package/botbuilder-azure), el mecanismo para establecer y conservar los datos con Bot Framework SDK para Node.js es el mismo que el del almacenamiento de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-118">With either of these [Azure Extensions](https://www.npmjs.com/package/botbuilder-azure) options, the mechanism for setting and persisting data via the Bot Framework SDK for Node.js remains the same as the in-memory data storage.</span></span>

## <a name="storage-containers"></a><span data-ttu-id="9fc4b-119">Contenedores de almacenamiento</span><span class="sxs-lookup"><span data-stu-id="9fc4b-119">Storage containers</span></span>

<span data-ttu-id="9fc4b-120">En Bot Builder SDK para Node.js, el objeto `session` expone las siguientes propiedades para almacenar datos de estado.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-120">In the Bot Builder SDK for Node.js, the `session` object exposes the following properties for storing state data.</span></span>

| <span data-ttu-id="9fc4b-121">Propiedad</span><span class="sxs-lookup"><span data-stu-id="9fc4b-121">Property</span></span> | <span data-ttu-id="9fc4b-122">Ámbito</span><span class="sxs-lookup"><span data-stu-id="9fc4b-122">Scoped to</span></span> | <span data-ttu-id="9fc4b-123">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="9fc4b-123">Description</span></span> |
| ---- | ---- | ---- |
| [`userData`][userDataURL] | <span data-ttu-id="9fc4b-124">Usuario</span><span class="sxs-lookup"><span data-stu-id="9fc4b-124">User</span></span> | <span data-ttu-id="9fc4b-125">Contiene datos que se guardan para el usuario en el canal especificado.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-125">Contains data that is saved for the user on the specified channel.</span></span> <span data-ttu-id="9fc4b-126">Estos datos se conservan a lo largo de varias conversaciones.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-126">This data will persist across multiple conversations.</span></span> |
| [`privateConversationData`][privateConversationDataURL] | <span data-ttu-id="9fc4b-127">Conversation</span><span class="sxs-lookup"><span data-stu-id="9fc4b-127">Conversation</span></span> | <span data-ttu-id="9fc4b-128">Contiene datos que se guardan para el usuario en el contexto de una conversación determinada en el canal especificado.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-128">Contains data that is saved for the user within the context of a particular conversation on the specified channel.</span></span> <span data-ttu-id="9fc4b-129">Estos datos son privados para el usuario actual y se conservarán únicamente para la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-129">This data is private to the current user and will persist for the current conversation only.</span></span> <span data-ttu-id="9fc4b-130">La propiedad se borra cuando la conversación finaliza o se llama explícitamente a `endConversation`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-130">The property is cleared when the conversation ends or when `endConversation` is called explicitly.</span></span> |
| [`conversationData`][conversationDataURL] | <span data-ttu-id="9fc4b-131">Conversation</span><span class="sxs-lookup"><span data-stu-id="9fc4b-131">Conversation</span></span> | <span data-ttu-id="9fc4b-132">Contiene datos que se guardan en el contexto de una conversación determinada en el canal especificado.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-132">Contains data that is saved in the context of a particular conversation on the specified channel.</span></span> <span data-ttu-id="9fc4b-133">Estos datos se comparten con todos los usuarios que participan en la conversación y se conservarán únicamente para la conversación actual.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-133">This data is shared with all users participating in the conversation and will persist for the current conversation only.</span></span> <span data-ttu-id="9fc4b-134">La propiedad se borra cuando la conversación finaliza o se llama explícitamente a `endConversation`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-134">The property is cleared when the conversation ends or when `endConversation` is called explicitly.</span></span> |
| [`dialogData`][dialogDataURL] | <span data-ttu-id="9fc4b-135">Diálogo</span><span class="sxs-lookup"><span data-stu-id="9fc4b-135">Dialog</span></span> | <span data-ttu-id="9fc4b-136">Contiene datos que se guardan únicamente para el diálogo actual.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-136">Contains data that is saved for the current dialog only.</span></span> <span data-ttu-id="9fc4b-137">Cada diálogo mantiene su propia copia de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-137">Each dialog maintains its own copy of this property.</span></span> <span data-ttu-id="9fc4b-138">La propiedad se borra cuando el diálogo se elimina de la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-138">The property is cleared when the dialog is removed from the dialog stack.</span></span> |

<span data-ttu-id="9fc4b-139">Estas cuatro propiedades corresponden a los cuatro contenedores de almacenamiento de datos que se pueden usar para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-139">These four properties correspond to the four data storage containers that can be used to store data.</span></span> <span data-ttu-id="9fc4b-140">Las propiedades que use para almacenar los datos dependerán del ámbito adecuado para los datos que almacena, la naturaleza de los mismos y cuánto tiempo desea conservar los datos.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-140">Which properties you use to store data will depend upon the appropriate scope for the data you are storing, the nature of the data that you are storing, and how long you want the data to persist.</span></span> <span data-ttu-id="9fc4b-141">Por ejemplo, si necesita almacenar datos de usuario que estarán disponibles a lo largo de varias conversaciones, considere el uso de la propiedad `userData`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-141">For example, if you need to store user data that will be available across multiple conversations, consider using the `userData` property.</span></span> <span data-ttu-id="9fc4b-142">Si necesita almacenar temporalmente los valores de las variables locales dentro del ámbito de un diálogo, considere el uso de la propiedad `dialogData`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-142">If you need to temporarily store local variable values within the scope of a dialog, consider using the `dialogData` property.</span></span> <span data-ttu-id="9fc4b-143">Si necesita almacenar temporalmente datos que deben ser accesibles a lo largo de varios diálogos, considere el uso de la propiedad `conversationData`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-143">If you need to temporarily store data that must be accessible across multiple dialogs, consider using the `conversationData` property.</span></span>

## <a name="data-persistence"></a><span data-ttu-id="9fc4b-144">Persistencia de los datos</span><span class="sxs-lookup"><span data-stu-id="9fc4b-144">Data Persistence</span></span>

<span data-ttu-id="9fc4b-145">De forma predeterminada, los datos que se almacenan utilizando las propiedades `userData`, `privateConversationData` y `conversationData` se conservan después de que finalice la conversación.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-145">By default, data that is stored using the `userData`, `privateConversationData`, and `conversationData` properties is set to persist after the conversation ends.</span></span> <span data-ttu-id="9fc4b-146">Si no desea que los datos se conserven en el contenedor `userData`, establezca la marca `persistUserData` en **false**.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-146">If you do not want the data to persist in the `userData` container, set the `persistUserData` flag to **false**.</span></span> <span data-ttu-id="9fc4b-147">Si no desea que los datos se conserven en el contenedor `conversationData`, establezca la marca `persistConversationData` en **false**.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-147">If you do not want the data to persist in the `conversationData` container, set the `persistConversationData` flag to **false**.</span></span> 

```javascript
// Do not persist userData
bot.set(`persistUserData`, false);

// Do not persist conversationData
bot.set(`persistConversationData`, false);
```

> [!NOTE]
> No se puede deshabilitar la persistencia de datos para el contenedor `privateConversationData`; siempre se conservan.

## <a name="set-data"></a><span data-ttu-id="9fc4b-149">Establecimiento de los datos</span><span class="sxs-lookup"><span data-stu-id="9fc4b-149">Set data</span></span>

<span data-ttu-id="9fc4b-150">Puede almacenar objetos JavaScript simples si los guarda directamente en un contenedor de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-150">You can store simple JavaScript objects by saving them directly to a storage container.</span></span> <span data-ttu-id="9fc4b-151">Para un objeto complejo como `Date`, considere convertirlo a `string`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-151">For a complex object like `Date`, consider converting it to `string`.</span></span> <span data-ttu-id="9fc4b-152">Esto es porque los datos de estado se serializan y se almacenan como JSON.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-152">This is because state data is serialized and stored as JSON.</span></span> <span data-ttu-id="9fc4b-153">Los ejemplos de código siguientes muestran cómo almacenar datos primitivos, una matriz, un mapa de objetos y un objeto `Date` complejo.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-153">The following code samples show how to store primitive data, an array, an object map, and a complex `Date` object.</span></span> 

<span data-ttu-id="9fc4b-154">**Almacenamiento de datos primitivos**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-154">**Store primitive data**</span></span>

```javascript
session.userData.userName = "Kumar Sarma";
session.userData.userAge = 37;
session.userData.hasChildren = true;
```

<span data-ttu-id="9fc4b-155">**Almacenamiento de una matriz**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-155">**Store an array**</span></span>

```javascript
session.userData.profile = ["Kumar Sarma", "37", "true"];
```

<span data-ttu-id="9fc4b-156">**Almacenamiento de un mapa de objetos**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-156">**Store an object map**</span></span>

```javascript
session.userData.about = {
    "Profile": {
        "Name": "Kumar Sarma",
        "Age": 37,
        "hasChildren": true
    },
    "Job": {
        "Company": "Contoso",
        "StartDate": "June 8th, 2010",
        "Title": "Developer"
    }
}
```
<span data-ttu-id="9fc4b-157">**Almacenamiento de fecha y hora**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-157">**Store Date and Time**</span></span> 

<span data-ttu-id="9fc4b-158">Para un objeto JavaScript complejo, debe convertirlo en una cadena antes de guardarlo en el contenedor de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-158">For a complex JavaScript object, convert it to a string before saving to storage container.</span></span> 

```javascript 
var startDate = builder.EntityRecognizer.resolveTime([results.response]); 

// Date as string: "2017-08-23T05:00:00.000Z" 
session.userdata.start = startDate.toISOString(); 
``` 

### <a name="saving-data"></a><span data-ttu-id="9fc4b-159">Guardar datos</span><span class="sxs-lookup"><span data-stu-id="9fc4b-159">Saving data</span></span>

<span data-ttu-id="9fc4b-160">Los datos que se crean en cada contenedor de almacenamiento permanecerán en memoria hasta que se guarde el contenedor.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-160">Data that is created in each storage container will remain in memory until the container is saved.</span></span> <span data-ttu-id="9fc4b-161">Bot Builder SDK para Node.js envía los datos al servicio `ChatConnector` por lotes para que se guarden cuando haya mensajes que enviar.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-161">The Bot Builder SDK for Node.js sends data to the `ChatConnector` service in batches to be saved when there are messages to be sent.</span></span> <span data-ttu-id="9fc4b-162">Para guardar los datos que existan en los contenedores de almacenamiento sin enviar ningún mensaje, puede llamar manualmente al método [`save`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#save).</span><span class="sxs-lookup"><span data-stu-id="9fc4b-162">To save the data that exists in the storage containers without sending any messages, you can manually call the [`save`](https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#save) method.</span></span> <span data-ttu-id="9fc4b-163">Si no llama al método `save`, los datos que existan en los contenedores de almacenamiento se guardarán como parte del procesamiento por lotes.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-163">If you do not call the `save` method, the data that exists in the storage containers will be persisted as part of the batch processing.</span></span>

```javascript
session.userData.favoriteColor = "Red";
session.userData.about.job.Title = "Senior Developer"; 
session.save();
```

## <a name="get-data"></a><span data-ttu-id="9fc4b-164">Obtener los datos</span><span class="sxs-lookup"><span data-stu-id="9fc4b-164">Get data</span></span>

<span data-ttu-id="9fc4b-165">Para acceder a los datos guardados en un contenedor de almacenamiento determinado, basta con hacer referencia a la propiedad correspondiente.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-165">To access the data that is saved in a particular storage container, simply reference the corresponding property.</span></span> <span data-ttu-id="9fc4b-166">Los ejemplos de código siguientes muestran cómo acceder a datos que se almacenaron como datos primitivos, una matriz, un mapa de objetos y un objeto Date complejo.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-166">The following code samples show how to access data that was previously stored as primitive data, an array, an object map, and a complex Date object.</span></span>

<span data-ttu-id="9fc4b-167">**Acceso a datos primitivos**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-167">**Access primitive data**</span></span>

```javascript
var userName = session.userData.userName;
var userAge = session.userData.userAge;
var hasChildren = session.userData.hasChildren;
```

<span data-ttu-id="9fc4b-168">**Acceso a una matriz**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-168">**Access an array**</span></span>

```javascript
var userProfile = session.userData.userProfile;

session.send("User Profile:");
for(int i = 0; i < userProfile.length, i++){
    session.send(userProfile[i]);
}
```

<span data-ttu-id="9fc4b-169">**Acceso a un mapa de objetos**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-169">**Access an object map**</span></span>

```javascript
var about = session.userData.about;

session.send("User %s works at %s.", about.Profile.Name, about.Job.Company);
```

<span data-ttu-id="9fc4b-170">**Acceso a un objeto Date**</span><span class="sxs-lookup"><span data-stu-id="9fc4b-170">**Access a Date object**</span></span> 

<span data-ttu-id="9fc4b-171">Recupere los datos de fecha como una cadena y, a continuación, conviértala en un objeto Date de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-171">Retrieve date data as string then convert it into a JavaScript's Date object.</span></span> 

```javascript 
// startDate as a JavaScript Date object. 
var startDate = new Date(session.userdata.start); 
``` 

## <a name="delete-data"></a><span data-ttu-id="9fc4b-172">Eliminación de datos</span><span class="sxs-lookup"><span data-stu-id="9fc4b-172">Delete data</span></span>

<span data-ttu-id="9fc4b-173">De forma predeterminada, los datos que se almacenan en el contenedor `dialogData` se borran cuando se elimina un diálogo de la pila de diálogos.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-173">By default, data that is stored in the `dialogData` container is cleared when a dialog is removed from the dialog stack.</span></span> <span data-ttu-id="9fc4b-174">Del mismo modo, los datos que se almacenan en los contenedores `conversationData` y `privateConversationData` se borran cuando se llama al método `endConversation`.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-174">Likewise, data that is stored in the `conversationData` container and `privateConversationData` container is cleared when the `endConversation` method is called.</span></span> <span data-ttu-id="9fc4b-175">Sin embargo, para eliminar los datos almacenados en el contenedor `userData`, debe eliminarlo explícitamente.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-175">However, to delete data stored in the `userData` container, you have to explicitly clear it.</span></span>

<span data-ttu-id="9fc4b-176">Para borrar explícitamente los datos almacenados en cualquiera de los contenedores de almacenamiento, simplemente restablezca el contenedor como se muestra en el siguiente ejemplo de código.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-176">To explicitly clear the data that is stored in any of the storage containers, simply reset the container as shown in the following code sample.</span></span> 

```javascript
// Clears data stored in container.
session.userData = {}; 
session.privateConversationData = {};
session.conversationData = {};
session.dialogData = {};
```

<span data-ttu-id="9fc4b-177">No establezca nunca un contenedor de datos en `null` ni lo elimine del objeto `session`, ya que hacerlo provocará errores la próxima vez que intente acceder al contenedor.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-177">Never set a data container `null` or remove it from the `session` object, as doing so will cause errors the next time you try to access the container.</span></span> <span data-ttu-id="9fc4b-178">Además, es posible que desee llamar manualmente a `session.save();` después de borrar manualmente un contenedor en memoria para borrar los datos correspondientes que se han guardado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-178">Also, you may want to manually call `session.save();` after you manually clear a container in memory, to clear any corresponding data that has previously been persisted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9fc4b-179">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9fc4b-179">Next steps</span></span>

<span data-ttu-id="9fc4b-180">Ahora que sabe cómo administrar los datos de estado de usuario, echemos un vistazo a cómo puede usarlos para administrar mejor el flujo de conversación.</span><span class="sxs-lookup"><span data-stu-id="9fc4b-180">Now that you understand how to manage user state data, let's take a look at how you can use it to better manage conversation flow.</span></span>

> [!div class="nextstepaction"]
> [Administración del flujo de conversación](bot-builder-nodejs-dialog-manage-conversation-flow.md)

## <a name="additional-resources"></a><span data-ttu-id="9fc4b-182">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9fc4b-182">Additional resources</span></span>
- [<span data-ttu-id="9fc4b-183">Petición de datos de entrada al usuario</span><span class="sxs-lookup"><span data-stu-id="9fc4b-183">Prompt user for input</span></span>](bot-builder-nodejs-dialog-prompt.md)

[userDataURL]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata
[conversationDataURL]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#conversationdata
[privateConversationDataURL]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#privateconversationdata
[dialogDataURL]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.session.html#dialogdata

[ChatConnector]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.chatconnector.html
