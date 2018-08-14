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
# <a name="save-user-state-data"></a>Guardar los datos de estado de los usuarios

Cuando el bot pide una entrada a los usuarios, lo más probable es que quiera conservar parte de la información en el almacenamiento de alguna forma. Bot Builder SDK permite almacenar las entradas del usuario con *almacenamiento en memoria*, *almacenamiento de archivos* y almacenamiento de base de datos, como *CosmosDB* o *SQL*. 

En este tutorial se mostrará cómo definir el objeto de almacenamiento en la capa de software intermedio y cómo guardar las entradas de usuario en el objeto de almacenamiento de modo que se pueda conservar.

## <a name="prequisite"></a>Requisito previo 

Este tutorial se basa en el tutorial [Administración de un flujo de conversación con cascada](bot-builder-tutorial-waterfall.md).

## <a name="add-storage-to-middleware-layer"></a>Incorporación de almacenamiento a la capa de software intermedio


## <a name="save-user-input-to-storage"></a>Guardar la entrada del usuario en el almacenamiento

Para administrar la conversación de reserva de tabla, deberá definir un diálogo en **cascada** con cuatro pasos. En esta conversación, también se usarán `DatetimePrompt` y `NumberPrompt` además de `TextPrompt`.

El diálogo `reserveTable` tendrá este aspecto:

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

Ahora, está listo para enlazarlo a la lógica del bot.

## <a name="start-the-dialog"></a>Inicio del diálogo

Modifique el método `processActivity()` del bot para que tenga este aspecto:

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

En tiempo de ejecución, cada vez que el usuario envía el mensaje que contiene la cadena `reserve table`, el bot iniciará la conversación `reserveTable`.

## <a name="next-steps"></a>Pasos siguientes

??? 

> [!div class="nextstepaction"]
> [Guardar los datos de estado de los usuarios](bot-builder-tutorial-save-data.md)
