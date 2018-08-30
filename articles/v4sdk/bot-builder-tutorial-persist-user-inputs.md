---
title: Conservación de datos de usuario | Microsoft Docs
description: Obtenga información sobre cómo guardar los datos de estado de los usuario en el almacenamiento del Bot Builder SDK.
keywords: conversación de datos de usuario, almacenamiento, datos de conversación
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/23/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 28377a1e611464012df28d3edf78d1cf01351345
ms.sourcegitcommit: 44f100a588ffda19c275b118f4f97029f12d1449
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2018
ms.locfileid: "42928373"
---
# <a name="persist-user-data"></a>Conservación de datos de usuario

[!INCLUDE [pre-release-label](../includes/pre-release-label.md)]

Cuando el bot pide una entrada a los usuarios, lo más probable es que quiera conservar parte de la información en el almacenamiento de alguna forma. El Bot Builder SDK le permite almacenar entradas de usuario mediante *almacenamiento en memoria*, *almacenamiento de archivos* o almacenamiento en bases de datos, como *CosmosDB* o *SQL*, donde los tipos de almacenamiento local se utilizan principalmente para pruebas o creación de prototipos, y los últimos tipos de almacenamiento son más adecuados para los bots de producción.

En este tutorial se mostrará cómo definir el objeto de almacenamiento y guardar las entradas de usuario para el objeto de almacenamiento de modo que se pueda conservar. 

> [!NOTE]
> Independientemente del tipo de almacenamiento que decida usar, el proceso de enlazar y conservar los datos es el mismo. En este tutorial se usa `FileStorage` como almacenamiento para conservar los datos.
> Para obtener más información sobre el estado y otros tipos de almacenamiento, consulte [Administración del estado de la conversación y del usuario](bot-builder-howto-v4-state.md).

## <a name="prequisite"></a>Requisito previo 

Este tutorial se basa en el tutorial para [reservar una mesa](bot-builder-tutorial-waterfall.md). En este tutorial, va a crear un bot que pide al usuario tres tipos de información sobre la reserva de una mesa en su restaurante. Sin embargo, no se conservó la entrada del usuario. En este tutorial se agregará la persistencia del almacenamiento de datos a ese bot.

## <a name="add-storage-to-middleware-layer"></a>Incorporación de almacenamiento a la capa de software intermedio

El Bot Builder SDK V4 controla el estado y el almacenamiento a través de software intermedio de administrador de estado. El software intermedio proporciona una capa de abstracción que permite acceder a las propiedades mediante un almacén simple de valor de clave, independiente del tipo de almacenamiento subyacente. El administrador de estado se encarga de escribir los datos en el almacenamiento y de administrar la simultaneidad, con independencia de que el tipo de almacenamiento subyacente sea en memoria, de archivos o almacenamiento de tablas de Azure. En este tutorial, se usará `FileStorage` para conservar las entradas de usuario.

El proveedor `FileStorage` viene con el paquete `bot-builder`. El ejemplo con el que empezó usa el proveedor `MemoryStorage`. Ese tipo de almacenamiento que usa almacenamiento *en memoria* volátil del bot, que se elimina cuando se reinicia el bot. Por otro lado, la biblioteca `FileStorage` se comporta de manera similar a una base de datos. Es decir, esta biblioteca escribe la información de almacenamiento en un archivo en el equipo local. Puede especificar dónde colocar este archivo de almacenamiento para que se puede inspeccionar más adelante.

Para usar `FileStorage`, busque la instrucción en el bot donde se define el objeto `conversationState` y actualícelo para crear un `new botbuilder.FileStorage("c:/temp")`. Además, puede definir la ubicación donde se debe escribir este archivo de almacenamiento. De este modo, puede encontrarlo fácilmente para inspeccionar el contenido de lo que se conservó.

# <a name="ctabcstab"></a>[C#](#tab/cstab)
```cs
var storage = new FileStorage("c:/temp");

// These two classes are simply Dictionaries to store state
options.Middleware.Add(new ConversationState<MyBot.convoState>(storage));
options.Middleware.Add(new UserState<MyBot.userState>(storage));
```

El Bot Builder SDK proporciona tres objetos de estado con ámbitos diferentes entre los que puede elegir.

| Estado | Ámbito | DESCRIPCIÓN |
| ---- | ---- | ---- |
| `dc.ActiveDialog.State` | cuadro de diálogo | Estado disponible para los pasos del cuadro de diálogo de cascada. |
| `ConversationState` | conversación | Estado disponible para la conversación actual. |
| `UserState` | user | Estado disponible para varias conversaciones. |

# <a name="javascripttabjstab"></a>[JavaScript](#tab/jstab)

**app.js**
```javascript
// Storage
const storage = new FileStorage("c:/temp");
const convoState = new ConversationState(storage);
const userState  = new UserState(storage);
adapter.use(new BotStateSet(convoState, userState));
```

El `BotStateSet` pueden administrar tanto `ConversationState` como `UserState` al mismo tiempo. Cuando llega el momento de guardar los datos de usuario, puede elegir. El Bot Builder SDK proporciona tres objetos de estado con ámbitos diferentes entre los que puede elegir.

| Estado | Ámbito | DESCRIPCIÓN |
| ---- | ---- | ---- |
| `dc.activeDialog.state` | cuadro de diálogo | Estado disponible para los pasos del cuadro de diálogo de cascada. |
| `ConversationState` | conversación | Estado disponible para la conversación actual. |
| `UserState` | user | Estado disponible para varias conversaciones. |

---
 

## <a name="persist-state"></a>Conservación del estado

Para el bot, puede escribir en cualquiera de estas tres ubicaciones de estado, según lo que guarde y cuánto tiempo debe conservarse.  

# <a name="ctabcstab"></a>[C#](#tab/cstab)

El *software intermedio del administrador de estado* controla el estado de escritura automáticamente al final de cada turno. Por lo tanto, todo lo que debe hacer en el bot es asignar los datos al objeto de estado de su elección. En este ejemplo, se usa `dc.ActiveDialog.State` para realizar el seguimiento de la entrada del usuario para la reserva. De este modo, en lugar de guardar la entrada del usuario en una variable global, se puede almacenar en un ámbito de objeto de estado temporal en el cuadro de diálogo. Este objeto solo existe en la medida en que el cuadro de diálogo esté activo; si desea conservarlo más tiempo, debe transferirlo a uno de los otros objetos de estado. En este caso, asignamos la reserva `msg` al estado de la conversación en el último paso de la cascada.

```cs
dialogs.Add("reserveTable", new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        // Prompt for the guest's name.
        await dc.Context.SendActivity("Welcome to the reservation service.");

        dc.ActiveDialog.State = new Dictionary<string, object>();

        await dc.Prompt("dateTimePrompt", "Please provide a reservation date and time.");
    },
    async(dc, args, next) =>
    {
        var dateTimeResult = ((DateTimeResult)args).Resolution.First();

        dc.ActiveDialog.State["date"] = Convert.ToDateTime(dateTimeResult.Value);
        
        // Ask for next info
        await dc.Prompt("partySizePrompt", "How many people are in your party?");

    },
    async(dc, args, next) =>
    {
        dc.ActiveDialog.State["partySize"] = (int)args["Value"];

        // Ask for next info
        await dc.Prompt("textPrompt", "Who's name will this be under?");
    },
    async(dc, args, next) =>
    {
        dc.ActiveDialog.State["name"] = args["Text"];
        string msg = "Reservation confirmed. Reservation details - " +
        $"\nDate/Time: {dc.ActiveDialog.State["date"].ToString()} " +
        $"\nParty size: {dc.ActiveDialog.State["partySize"].ToString()} " +
        $"\nReservation name: {dc.ActiveDialog.State["name"]}";

        var convo = ConversationState<convoState>.Get(dc.Context);

        // In production, you may want to store something more helpful
        convo[$"{dc.ActiveDialog.State["name"]} reservation"] = msg;

        await dc.Context.SendActivity(msg);
        await dc.End();
    }
});
```

# <a name="javascripttabjstab"></a>[JavaScript](#tab/jstab)

El *software intermedio del administrador de estado* controla el estado de escritura en el archivo automáticamente al final de cada turno. Por lo tanto, todo lo que debe hacer en el bot es asignar los datos al objeto de estado de su elección. En este ejemplo, se usa `dc.activeDialog.state` para realizar el seguimiento de la entrada del usuario en un objeto `reservervationInfo`. De este modo, en lugar de guardar la entrada del usuario en una variable global, se puede almacenar en un ámbito de objeto de estado temporal en el cuadro de diálogo. Dado que este objeto solo existe en la medida en que el cuadro de diálogo esté activo; si desea conservarlo, debe transferirlo a uno de los otros objetos de estado. En este caso, se asigna `reservationInfo` al estado `convo` en el último paso de la cascada.

```javascript
// Reserve a table:
// Help the user to reserve a table

dialogs.add('reserveTable', [
    async function(dc, args, next){
        await dc.context.sendActivity("Welcome to the reservation service.");

        dc.activeDialog.state.reservationInfo = {}; // Clears any previous data
        await dc.prompt('dateTimePrompt', "Please provide a reservation date and time.");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.dateTime = result[0].value;

        // Ask for next info
        await dc.prompt('partySizePrompt', "How many people are in your party?");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.partySize = result;

        // Ask for next info
        await dc.prompt('textPrompt', "Who's name will this be under?");
    },
    async function(dc, result){
        dc.activeDialog.state.reservationInfo.reserveName = result;
        
        // Persist data
        var convo = conversationState.get(dc.context);; // conversationState.get(dc.context);
        convo.reservationInfo = dc.activeDialog.state.reservationInfo;

        // Confirm reservation
        var msg = `Reservation confirmed. Reservation details: 
            <br/>Date/Time: ${dc.activeDialog.state.reservationInfo.dateTime} 
            <br/>Party size: ${dc.activeDialog.state.reservationInfo.partySize} 
            <br/>Reservation name: ${dc.activeDialog.state.reservationInfo.reserveName}`;
            
        await dc.context.sendActivity(msg);
        await dc.end();
    }
]);
```

---

Ahora, está listo para enlazarlo a la lógica del bot.

## <a name="start-the-dialog"></a>Inicio del cuadro de diálogo

No hay ningún código que deba cambiar aquí. Simplemente ejecute el bot y envíe el mensaje para iniciar la conversación `reserveTable`.

## <a name="check-file-storage-content"></a>Comprobación del contenido del almacenamiento de archivo

Después de ejecutar el bot y haber pasado por la conversación `reserveTable`, busque la información guardada en un archivo en la ubicación especificada (p. ej.: "C:/temp"). Al nombre de archivo se antepone "conversation!" o "user!". Puede ser útil para ordenar los archivos por fecha, para poder encontrar el último de manera más fácil.

## <a name="next-steps"></a>Pasos siguientes

Ahora que sabe cómo guardar entradas de usuario, eche un vistazo a qué tipo de entrada puede pedir al usuario mediante la biblioteca de avisos.

> [!div class="nextstepaction"]
> [Solicitud de entradas a los usuarios](~/v4sdk/bot-builder-prompts.md)
