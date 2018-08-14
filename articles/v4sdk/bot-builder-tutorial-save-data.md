---
title: 'Tutorial: Guardar los datos de estado de los usuarios | Microsoft Docs'
description: Obtenga información sobre cómo guardar los datos de estado de los usuario en Bot Builder SDK.
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 4/23/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 10c1cb240a22c1c16dd0d946ee55531d514f332e
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305756"
---
# <a name="save-user-state-data"></a><span data-ttu-id="b4b5d-103">Guardar los datos de estado de los usuarios</span><span class="sxs-lookup"><span data-stu-id="b4b5d-103">Save user state data</span></span>

<span data-ttu-id="b4b5d-104">Cuando el bot pide una entrada a los usuarios, lo más probable es que quiera conservar parte de la información en el almacenamiento de alguna forma.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-104">When the bot is asking users for input, chances are that you would want to persist some of the information to storage of some form.</span></span> <span data-ttu-id="b4b5d-105">Bot Builder SDK permite almacenar las entradas del usuario con *almacenamiento en memoria*, *almacenamiento de archivos* y almacenamiento de base de datos, como *CosmosDB* o *SQL*.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-105">The Bot Builder SDK allows you to store user inputs using *in-memory storage*, *file storage*, database storage such as *CosmosDB* or *SQL*.</span></span> 

<span data-ttu-id="b4b5d-106">En este tutorial se mostrará cómo definir el objeto de almacenamiento en la capa de software intermedio y cómo guardar las entradas de usuario en el objeto de almacenamiento de modo que se pueda conservar.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-106">This tutorial will show you how to define your storage object in the middleware layer and how to save user into to the storage object so that it can be persisted.</span></span>

## <a name="prequisite"></a><span data-ttu-id="b4b5d-107">Requisito previo</span><span class="sxs-lookup"><span data-stu-id="b4b5d-107">Prequisite</span></span> 

<span data-ttu-id="b4b5d-108">Este tutorial se basa en el tutorial [Administración de un flujo de conversación con cascada](bot-builder-tutorial-waterfall.md).</span><span class="sxs-lookup"><span data-stu-id="b4b5d-108">This tutorial builds on the [Manage a conversation flow with waterfall](bot-builder-tutorial-waterfall.md) tutorial.</span></span>

## <a name="add-storage-to-middleware-layer"></a><span data-ttu-id="b4b5d-109">Incorporación de almacenamiento a la capa de software intermedio</span><span class="sxs-lookup"><span data-stu-id="b4b5d-109">Add storage to middleware layer</span></span>


## <a name="save-user-input-to-storage"></a><span data-ttu-id="b4b5d-110">Guardar la entrada del usuario en el almacenamiento</span><span class="sxs-lookup"><span data-stu-id="b4b5d-110">Save user input to storage</span></span>

<span data-ttu-id="b4b5d-111">Para administrar la conversación de reserva de tabla, deberá definir un diálogo en **cascada** con cuatro pasos.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-111">To manage the table reservation conversaion, you will need to define a **waterfall** dialog with four steps.</span></span> <span data-ttu-id="b4b5d-112">En esta conversación, también se usarán `DatetimePrompt` y `NumberPrompt` además de `TextPrompt`.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-112">In this conversation, you will also be using a `DatetimePrompt` and `NumberPrompt` in additional to the `TextPrompt`.</span></span>

<span data-ttu-id="b4b5d-113">El diálogo `reserveTable` tendrá este aspecto:</span><span class="sxs-lookup"><span data-stu-id="b4b5d-113">The `reserveTable` dialog will look like this:</span></span>

```javascript
// Reserve a table:
// Help the user to reserve a table

var reservationInfo = {
    dateTime: '',
    partySize: '',
    reserveName: ''
}

dialogs.add('reserveTable', [
    async function(dc, args, next){
        await dc.context.sendActivity("Welcome to the reservation service.");
        reservationInfo = {}; // Clears any previous data
        await dc.prompt('dateTimePrompt', "Please provide a reservation date and time.");
    },
    async function(dc, result){
        reservationInfo.dateTime = result[0].value;

        // Ask for next info
        await dc.prompt('partySizePrompt', "How many people are in your party?");
    },
    async function(dc, result){
        reservationInfo.partySize = result;

        // Ask for next info
        await dc.prompt('textPrompt', "Who's name will this be under?");
    },
    async function(dc, result){
        reservationInfo.reserveName = result;

        // Confirm reservation
        var msg = `Reservation confirmed. Reservation details: 
            <br/>Date/Time: ${reservationInfo.dateTime} 
            <br/>Party size: ${reservationInfo.partySize} 
            <br/>Reservation name: ${reservationInfo.reserveName}`;
        await dc.context.sendActivity(msg);
        await dc.end();
    }
]);

```

<span data-ttu-id="b4b5d-114">Ahora, está listo para enlazarlo a la lógica del bot.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-114">Now, you are ready to hook this into the bot logic.</span></span>

## <a name="start-the-dialog"></a><span data-ttu-id="b4b5d-115">Inicio del diálogo</span><span class="sxs-lookup"><span data-stu-id="b4b5d-115">Start the dialog</span></span>

<span data-ttu-id="b4b5d-116">Modifique el método `processActivity()` del bot para que tenga este aspecto:</span><span class="sxs-lookup"><span data-stu-id="b4b5d-116">Modify your bot's `processActivity()` method to look like this:</span></span>

```javascript
// Listen for incoming activity 
server.post('/api/messages', (req, res) => {
    adapter.processActivity(req, res, async (context) => {
        if (context.activity.type === 'message') {
            // State will store all of your information 
            const state = conversationState.get(context);
            const dc = dialogs.createContext(context, state);

            if(context.activity.text.match(/hi/ig)){
                await dc.begin('greeting');
            }
            if(context.activity.text.match(/reserve table/ig)){
                await dc.begin('reserveTable');
            }
            else{
                // Continue executing the "current" dialog, if any.
                await dc.continue();
            }
        }
    });
});
```

<span data-ttu-id="b4b5d-117">En tiempo de ejecución, cada vez que el usuario envía el mensaje que contiene la cadena `reserve table`, el bot iniciará la conversación `reserveTable`.</span><span class="sxs-lookup"><span data-stu-id="b4b5d-117">At execution time, whenever the user sends the message containing the string `reserve table`, the bot will start the `reserveTable` conversation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b4b5d-118">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b4b5d-118">Next steps</span></span>

<span data-ttu-id="b4b5d-119">???</span><span class="sxs-lookup"><span data-stu-id="b4b5d-119">???</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="b4b5d-120">Guardar los datos de estado de los usuarios</span><span class="sxs-lookup"><span data-stu-id="b4b5d-120">Save user state data</span></span>](bot-builder-tutorial-save-data.md)
