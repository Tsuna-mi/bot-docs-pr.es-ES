---
title: Administración de un flujo de conversación complejo con diálogos | Microsoft Docs
description: Aprenda a administrar un flujo de conversación complejo con diálogos en el SDK de Bot Builder para Node.js.
keywords: flujo de conversación complejo, repetir, bucle, menú, diálogos, avisos, cascadas, conjunto de diálogos
author: v-ducvo
ms.author: v-ducvo
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 7/27/2018
monikerRange: azure-bot-service-4.0
ms.openlocfilehash: 304de6783a268140bf74f95d96cd9c24e12c4c05
ms.sourcegitcommit: 1abc32353c20acd103e0383121db21b705e5eec3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40236369"
---
# <a name="manage-complex-conversation-flows-with-dialogs"></a><span data-ttu-id="4e644-104">Administración de flujos de conversación complejos con diálogos</span><span class="sxs-lookup"><span data-stu-id="4e644-104">Manage complex conversation flows with dialogs</span></span>

[!INCLUDE [pre-release-label](~/includes/pre-release-label.md)]

<span data-ttu-id="4e644-105">Puede administrar flujos de conversación simples y complejos mediante la biblioteca Dialogs.</span><span class="sxs-lookup"><span data-stu-id="4e644-105">You can manage both simple and complex conversation flows using the dialogs library.</span></span> <span data-ttu-id="4e644-106">En [flujos de conversación simples](bot-builder-dialog-manage-conversation-flow.md), el usuario comienza desde el primer paso de una *cascada*, continúa hasta el último paso y el intercambio de conversación finaliza.</span><span class="sxs-lookup"><span data-stu-id="4e644-106">In [simple conversation flows](bot-builder-dialog-manage-conversation-flow.md), the user starts from the first step of a *waterfall*, continues through to the last step, and the conversational exchange finishes.</span></span> <span data-ttu-id="4e644-107">En este artículo, usaremos diálogos para administrar conversaciones más complejas con partes que se pueden bifurcar y repetir en bucle.</span><span class="sxs-lookup"><span data-stu-id="4e644-107">In this article we will use dialogs to manage more complex conversations with portions that can branch and loop.</span></span> <span data-ttu-id="4e644-108">Para ello, vamos a usar el método _replace_ del contexto del diálogo, además de pasar argumentos entre las distintas partes del diálogo.</span><span class="sxs-lookup"><span data-stu-id="4e644-108">To do so, we'll use the dialog context's _replace_ method, as well as passing arguments between different parts of the dialog.</span></span>

<!-- TODO: We need a dialogs conceptual topic to link to, so we can reference that here, in place of describing what they are and what their features are in a how-to topic. -->

<span data-ttu-id="4e644-109">Para proporcionarle más control sobre la *pila de diálogos*, la biblioteca **Dialogs** proporciona un método _replace_.</span><span class="sxs-lookup"><span data-stu-id="4e644-109">To give you more control over the *dialog stack*, the **Dialogs** library provides a _replace_ method.</span></span> <span data-ttu-id="4e644-110">Este método permite retirar el diálogo actual de la pila, insertar un diálogo nuevo en la parte superior de la pila e iniciar el nuevo diálogo.</span><span class="sxs-lookup"><span data-stu-id="4e644-110">This method allows you to pop the current dialog off the stack, push a new dialog onto the top of the stack, and start the new dialog.</span></span> <span data-ttu-id="4e644-111">Esta característica permite proporcionar una conversación más compleja.</span><span class="sxs-lookup"><span data-stu-id="4e644-111">This feature allows you to provide a more complex conversation.</span></span> <span data-ttu-id="4e644-112">Estas técnicas se pueden usar para crear conversaciones arbitrariamente complejas.</span><span class="sxs-lookup"><span data-stu-id="4e644-112">These techniques can be used to create arbitrarily complex conversations.</span></span> <span data-ttu-id="4e644-113">Si aumenta la complejidad de la conversación hasta que los diálogos resultan difíciles de administrar, es posible crear su propio flujo de lógica de control para realizar un seguimiento de la conversación del usuario.</span><span class="sxs-lookup"><span data-stu-id="4e644-113">Should your conversation complexity increase to where dialogs become difficult to manage, it is possible to create your own flow of control logic to keep track of your user's conversation.</span></span>

<!-- TODO: This is probably a good place to add a link to the modular/composite dialogs topic. -->

<span data-ttu-id="4e644-114">En este artículo vamos a crear los diálogos para el bot de un hotel que un cliente podría utilizar para reservar una mesa o pedir el envío de alguna comida a su habitación.</span><span class="sxs-lookup"><span data-stu-id="4e644-114">In this article we'll create the dialogs for a hotel bot that a guest could use to reserve a table or order a meal to be delivered to their room.</span></span> <span data-ttu-id="4e644-115">El diálogo de nivel superior proporciona al invitado estas dos opciones.</span><span class="sxs-lookup"><span data-stu-id="4e644-115">The top-level dialog provides the guest with these two options.</span></span> <span data-ttu-id="4e644-116">Si desean reservar una mesa, el diálogo de nivel superior inicia un diálogo de reservas de mesa.</span><span class="sxs-lookup"><span data-stu-id="4e644-116">If they want to reserve a table the top-level dialog starts a table reservation dialog.</span></span> <span data-ttu-id="4e644-117">Si el cliente desea pedir cena para la habitación, el diálogo de nivel superior inicia un diálogo de pedidos de cena.</span><span class="sxs-lookup"><span data-stu-id="4e644-117">If the guest wants to order dinner, the top-level dialog starts a dinner ordering dialog.</span></span> <span data-ttu-id="4e644-118">El diálogo de pedidos de cena primero pide al cliente que seleccione elementos de comida de un menú y, a continuación, pide el número de habitación.</span><span class="sxs-lookup"><span data-stu-id="4e644-118">The dinner ordering dialog first asks the guest to select food items off a menu and then asks for their room number.</span></span> <span data-ttu-id="4e644-119">La selección de elementos de comida también es un diálogo, por lo que podemos permitir que el cliente seleccione varios elementos antes de procesar su pedido para la cena.</span><span class="sxs-lookup"><span data-stu-id="4e644-119">The selection of food items is also a dialog, so that we can allow the guest to select multiple items before processing their dinner order.</span></span>

<span data-ttu-id="4e644-120">Este diagrama muestra los diálogos que se van a crear en este artículo y su relación entre sí.</span><span class="sxs-lookup"><span data-stu-id="4e644-120">This diagram illustrates the dialogs we'll be creating in this article and their relationship to each other.</span></span>

![Ilustración de los diálogos que se usan en este artículo](~/media/complex-conversation-flows.png)

## <a name="how-to-branch"></a><span data-ttu-id="4e644-122">Cómo bifurcar</span><span class="sxs-lookup"><span data-stu-id="4e644-122">How to branch</span></span>

<span data-ttu-id="4e644-123">El contexto del diálogo mantiene una _pila de diálogos_ y, para cada diálogo de la pila, lleva el seguimiento de cuál es el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="4e644-123">The dialog context maintains a _dialog stack_ and for each dialog on the stack, tracks which step is next.</span></span> <span data-ttu-id="4e644-124">Su método _begin_ inserta un diálogo en la parte superior de la pila y su método _end_ retira el diálogo superior de la pila.</span><span class="sxs-lookup"><span data-stu-id="4e644-124">Its _begin_ method pushes a dialog onto the top of the stack, and its _end_ method pops the top dialog off the stack.</span></span>

<span data-ttu-id="4e644-125">Un diálogo puede iniciar un nuevo diálogo (también conocido como bifurcación) en el mismo conjunto de diálogos mediante una llamada al método _begin_ del contexto del diálogo, al que se proporciona el identificador del diálogo nuevo.</span><span class="sxs-lookup"><span data-stu-id="4e644-125">A dialog can start a new dialog (aka: branching) within the same dialog set by calling the dialog context's _begin_ method and providing the ID of the new dialog.</span></span> <span data-ttu-id="4e644-126">El diálogo original está todavía en la pila, pero las llamadas al método _continue_ del contexto del diálogo solo se envían al diálogo superior de la pila, el _diálogo activo_.</span><span class="sxs-lookup"><span data-stu-id="4e644-126">The original dialog is still on the stack, but calls to the dialog context's _continue_ method are only sent to the dialog that is on top of the stack, the _active dialog_.</span></span> <span data-ttu-id="4e644-127">Cuando un diálogo se retira de la pila, el contexto del diálogo se reanudará con el siguiente paso de la cascada de la pila donde se quedó.</span><span class="sxs-lookup"><span data-stu-id="4e644-127">When a dialog is popped off the stack, the dialog context will resume with the next step of the waterfall on the stack where it left off.</span></span>

<span data-ttu-id="4e644-128">Por lo tanto, puede crear una rama dentro del flujo de conversación al incluir un paso en un diálogo que puede elegir condicionalmente un diálogo para iniciar un conjunto de diálogos posibles.</span><span class="sxs-lookup"><span data-stu-id="4e644-128">Thus, you can create a branch within your conversation flow by including a step in one dialog that can conditionally choose a dialog to start out of a set of potential dialogs.</span></span>

## <a name="how-to-loop"></a><span data-ttu-id="4e644-129">Cómo repetir en bucle</span><span class="sxs-lookup"><span data-stu-id="4e644-129">How to loop</span></span>

<span data-ttu-id="4e644-130">El método _replace_ del contexto del diálogo le permite reemplazar el diálogo superior de la pila.</span><span class="sxs-lookup"><span data-stu-id="4e644-130">The dialog context's _replace_ method allows you to replace the dialog that is on top of the stack.</span></span> <span data-ttu-id="4e644-131">El estado del diálogo antiguo se descarta y el diálogo nuevo se inicia desde el principio.</span><span class="sxs-lookup"><span data-stu-id="4e644-131">The state of the old dialog is discarded and the new dialog starts from the beginning.</span></span> <span data-ttu-id="4e644-132">Puede usar este método para crear un bucle al reemplazar un diálogo por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="4e644-132">You can use this method to create a loop by replacing a dialog with itself.</span></span> <span data-ttu-id="4e644-133">Sin embargo, para [conservar el estado](bot-builder-howto-v4-state.md), necesita pasar información a la nueva instancia del diálogo en la llamada al método _replace_.</span><span class="sxs-lookup"><span data-stu-id="4e644-133">However, to [persist state](bot-builder-howto-v4-state.md) you will need to pass information to the new instance of the dialog in the call to the _replace_ method.</span></span> <span data-ttu-id="4e644-134">En el ejemplo siguiente, verá que el carro de pedidos de cena se almacena en el estado del diálogo y, cuando se repite el diálogo `orderPrompt`, se pasa el estado actual del diálogo para que el estado del diálogo nuevo pueda agregar más elementos en él.</span><span class="sxs-lookup"><span data-stu-id="4e644-134">In the following example, you will see that the dinner order cart is stored on the dialog state and when the `orderPrompt` dialog is repeated, the current dialog state is passed in so that the new dialog's state can continue adding items to it.</span></span> <span data-ttu-id="4e644-135">Si desea almacenar esta información en otros estados, consulte [Conservación de los datos del usuario](bot-builder-tutorial-persist-user-inputs.md).</span><span class="sxs-lookup"><span data-stu-id="4e644-135">If you prefer to store these information in the other states, see [Persist user data](bot-builder-tutorial-persist-user-inputs.md).</span></span>

## <a name="create-the-dialogs-for-the-hotel-bot"></a><span data-ttu-id="4e644-136">Creación de los diálogos para el bot del hotel</span><span class="sxs-lookup"><span data-stu-id="4e644-136">Create the dialogs for the hotel bot</span></span>

<span data-ttu-id="4e644-137">En esta sección vamos a crear los diálogos para administrar una conversación para el bot del hotel descrito.</span><span class="sxs-lookup"><span data-stu-id="4e644-137">In this section we'll create the dialogs to manage a conversation for the hotel bot we described.</span></span>

### <a name="install-the-dialogs-library"></a><span data-ttu-id="4e644-138">Instalación de la biblioteca de diálogos</span><span class="sxs-lookup"><span data-stu-id="4e644-138">Install the dialogs library</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-139">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-139">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-140">Comenzaremos a partir de una plantilla EchoBot básica.</span><span class="sxs-lookup"><span data-stu-id="4e644-140">We'll start from a basic EchoBot template.</span></span> <span data-ttu-id="4e644-141">Para obtener instrucciones, consulte la [guía de inicio rápido para .NET](~/dotnet/bot-builder-dotnet-quickstart.md).</span><span class="sxs-lookup"><span data-stu-id="4e644-141">For instructions, see the [quickstart for .NET](~/dotnet/bot-builder-dotnet-quickstart.md).</span></span>

<span data-ttu-id="4e644-142">Para usar diálogos, instale el paquete NuGet `Microsoft.Bot.Builder.Dialogs` para su proyecto o solución.</span><span class="sxs-lookup"><span data-stu-id="4e644-142">To use dialogs, install the `Microsoft.Bot.Builder.Dialogs` NuGet package for your project or solution.</span></span>
<span data-ttu-id="4e644-143">A continuación, haga referencia a la biblioteca Dialogs en las instrucciones using de los archivos de código según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="4e644-143">Then reference the dialogs library in using statements in your code files as necessary.</span></span>

```csharp
using Microsoft.Bot.Builder.Dialogs;
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-144">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-144">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="4e644-145">La biblioteca `botbuilder-dialogs` se puede descargar desde NPM.</span><span class="sxs-lookup"><span data-stu-id="4e644-145">The `botbuilder-dialogs` library can be downloaded from NPM.</span></span> <span data-ttu-id="4e644-146">Para instalar la biblioteca `botbuilder-dialogs`, ejecute el siguiente comando de NPM:</span><span class="sxs-lookup"><span data-stu-id="4e644-146">To install the `botbuilder-dialogs` library, run the following NPM command:</span></span>

```cmd
npm install --save botbuilder-dialogs
```

<span data-ttu-id="4e644-147">Para usar **dialogs** en el bot, inclúyalo en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="4e644-147">To use **dialogs** in your bot, include it in the bot code.</span></span> <span data-ttu-id="4e644-148">Por ejemplo, agregue esto a su archivo **app.js**:</span><span class="sxs-lookup"><span data-stu-id="4e644-148">For example, add this to your **app.js** file:</span></span>

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

---

### <a name="create-a-dialog-set"></a><span data-ttu-id="4e644-149">Creación de un conjunto de diálogos</span><span class="sxs-lookup"><span data-stu-id="4e644-149">Create a dialog set</span></span>

<span data-ttu-id="4e644-150">Cree un _conjunto de diálogos_ al que vamos a agregar todos los diálogos de este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="4e644-150">Create a _dialog set_ to which we'll add all of the dialogs for this example.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-151">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-151">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-152">Cree una clase **HotelDialog** y agregue las instrucciones using que necesitaremos.</span><span class="sxs-lookup"><span data-stu-id="4e644-152">Create a **HotelDialog** class, and add the using statements we'll need.</span></span>

```csharp
using Microsoft.Bot.Builder.Core.Extensions;
using Microsoft.Bot.Builder.Dialogs;
using Microsoft.Bot.Builder.Prompts.Choices;
using Microsoft.Bot.Schema;
using Microsoft.Recognizers.Text;
using System;
using System.Collections.Generic;
using System.Linq;
```

<span data-ttu-id="4e644-153">Derive la clase de **DialogSet** y defina los identificadores y las claves que vamos a usar para identificar los diálogos, avisos y la información de estado para este conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="4e644-153">Derive the class from **DialogSet**, and define the IDs and keys we'll use to identify the dialogs, prompts, and state information for this dialog set.</span></span>

```csharp
/// <summary>Contains the set of dialogs and prompts for the hotel bot.</summary>
public class HotelDialog : DialogSet
{
    /// <summary>The ID of the top-level dialog.</summary>
    public const string MainMenu = "mainMenu";

    /// <summary>Contains the IDs for the other dialogs in the set.</summary>
    private static class Dialogs
    {
        public const string OrderDinner = "orderDinner";
        public const string OrderPrompt = "orderPrompt";
        public const string ReserveTable = "reserveTable";
    }

    /// <summary>Contains the IDs for the prompts used by the dialogs.</summary>
    private static class Inputs
    {
        public const string Choice = "choicePrompt";
        public const string Number = "numberPrompt";
    }

    /// <summary>Contains the keys used to manage dialog state.</summary>
    private static class Outputs
    {
        public const string OrderCart = "orderCart";
        public const string OrderTotal = "orderTotal";
        public const string RoomNumber = "roomNumber";
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-154">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-154">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="4e644-155">Para usar **dialogs** en el bot, inclúyalo en el código del bot.</span><span class="sxs-lookup"><span data-stu-id="4e644-155">To use **dialogs** in your bot, include it in the bot code.</span></span>

<span data-ttu-id="4e644-156">Haga referencia a la biblioteca desde el archivo **app.js**.</span><span class="sxs-lookup"><span data-stu-id="4e644-156">Reference the library from your **app.js** file.</span></span>

```javascript
const botbuilder_dialogs = require('botbuilder-dialogs');
```

<span data-ttu-id="4e644-157">Y, a continuación, cree el conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="4e644-157">And then create the dialog set.</span></span>

```javascript
const dialogs = new botbuilder_dialogs.DialogSet();
```

---

### <a name="add-the-prompts-to-the-set"></a><span data-ttu-id="4e644-158">Adición de los avisos al conjunto</span><span class="sxs-lookup"><span data-stu-id="4e644-158">Add the prompts to the set</span></span>

<span data-ttu-id="4e644-159">Vamos a usar un elemento **ChoicePrompt** para preguntar a los clientes si desean pedir cena o reservar una mesa y también qué opción seleccionar del menú de cenas.</span><span class="sxs-lookup"><span data-stu-id="4e644-159">We'll use a **ChoicePrompt** to ask guests whether to order dinner or to reserve a table, and also which option off the dinner menu to select.</span></span> <span data-ttu-id="4e644-160">Y vamos a usar un elemento **NumberPrompt** para preguntar al cliente el número de habitación cuando pida la cena.</span><span class="sxs-lookup"><span data-stu-id="4e644-160">And, we'll use a **NumberPrompt** to ask for the guest's room number when they do order dinner.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-161">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-161">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-162">Agregue los dos avisos en el constructor **HotelDialog**.</span><span class="sxs-lookup"><span data-stu-id="4e644-162">In the **HotelDialog** constructor, add the two prompts.</span></span>

```csharp
// Add the prompts.
this.Add(Inputs.Choice, new ChoicePrompt(Culture.English));
this.Add(Inputs.Number, new NumberPrompt<int>(Culture.English));
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-163">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-163">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="4e644-164">Agregue estos dos avisos al conjunto de diálogos.</span><span class="sxs-lookup"><span data-stu-id="4e644-164">Add these two prompts to the dialog set.</span></span>

```javascript
dialogs.add('choicePrompt', new botbuilder_dialogs.ChoicePrompt());
dialogs.add('numberPrompt', new botbuilder_dialogs.NumberPrompt());
```

---

### <a name="define-some-of-the-supporting-information"></a><span data-ttu-id="4e644-165">Definición de la información auxiliar</span><span class="sxs-lookup"><span data-stu-id="4e644-165">Define some of the supporting information</span></span>

<span data-ttu-id="4e644-166">Puesto que necesitamos información sobre cada opción en el menú de cenas, vamos a configurarlo ahora.</span><span class="sxs-lookup"><span data-stu-id="4e644-166">Since we'll need information about each option on the dinner menu, let's set that up now.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-167">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-167">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-168">Cree una clase estática privada llamada **Lists** que va a contener esta información.</span><span class="sxs-lookup"><span data-stu-id="4e644-168">Create an inner static **Lists** class to contain this information.</span></span> <span data-ttu-id="4e644-169">También crearemos las clases privadas **WelcomeChoice** y **MenuChoice** para contener la información sobre cada opción.</span><span class="sxs-lookup"><span data-stu-id="4e644-169">We'll also create inner **WelcomeChoice** and **MenuChoice** classes to contain information about each option.</span></span>

<span data-ttu-id="4e644-170">También vamos a agregar información en la lista de opciones en el diálogo de bienvenida de nivel superior y vamos a crear las listas de apoyo que usaremos más adelante para pedir esta información a los clientes.</span><span class="sxs-lookup"><span data-stu-id="4e644-170">While we're at it, we'll add information for the list of options in the top-level welcome dialog, and we'll also create supporting lists that we'll use later when prompting the guests for this information.</span></span> <span data-ttu-id="4e644-171">Es un poco de trabajo por adelantado, pero hará que el código del diálogo sea más sencillo.</span><span class="sxs-lookup"><span data-stu-id="4e644-171">It's a little added work up front, but it will make the dialog code simpler.</span></span>

```csharp
/// <summary>Describes an option for the top-level dialog.</summary>
private class WelcomeChoice
{
    /// <summary>The text to show the guest for this option.</summary>
    public string Description { get; set; }

    /// <summary>The ID of the associated dialog for this option.</summary>
    public string DialogName { get; set; }
}

/// <summary>Describes an option for the food-selection dialog.</summary>
/// <remarks>We have two types of options. One represents meal items that the guest
/// can add to their order. The other represents a request to process or cancel the
/// order.</remarks>
private class MenuChoice
{
    /// <summary>The request text for cancelling the meal order.</summary>
    public const string Cancel = "Cancel order";

    /// <summary>The request text for processing the meal order.</summary>
    public const string Process = "Process order";

    /// <summary>The name of the meal item or the request.</summary>
    public string Name { get; set; }

    /// <summary>The price of the meal item; or NaN for a request.</summary>
    public double Price { get; set; }

    /// <summary>The text to show the guest for this option.</summary>
    public string Description => (double.IsNaN(Price)) ? Name : $"{Name} - ${Price:0.00}";
}

/// <summary>Contains the lists used to present options to the guest.</summary>
private static class Lists
{
    /// <summary>The options for the top-level dialog.</summary>
    public static List<WelcomeChoice> WelcomeOptions { get; } = new List<WelcomeChoice>
    {
        new WelcomeChoice { Description = "Order dinner", DialogName = Dialogs.OrderDinner },
        new WelcomeChoice { Description = "Reserve a table", DialogName = Dialogs.ReserveTable },
    };

    private static List<string> WelcomeList { get; } = WelcomeOptions.Select(x => x.Description).ToList();

    /// <summary>The choices to present in the choice prompt for the top-level dialog.</summary>
    public static List<Choice> WelcomeChoices { get; } = ChoiceFactory.ToChoices(WelcomeList);

    /// <summary>The reprompt action for the top-level dialog.</summary>
    public static Activity WelcomeReprompt
    {
        get
        {
            var reprompt = MessageFactory.SuggestedActions(WelcomeList, "Please choose an option");
            reprompt.AttachmentLayout = AttachmentLayoutTypes.List;
            return reprompt as Activity;
        }
    }

    /// <summary>The options for the food-selection dialog.</summary>
    public static List<MenuChoice> MenuOptions { get; } = new List<MenuChoice>
    {
        new MenuChoice { Name = "Potato Salad", Price = 5.99 },
        new MenuChoice { Name = "Tuna Sandwich", Price = 6.89 },
        new MenuChoice { Name = "Clam Chowder", Price = 4.50 },
        new MenuChoice { Name = MenuChoice.Process, Price = double.NaN },
        new MenuChoice { Name = MenuChoice.Cancel, Price = double.NaN },
    };

    private static List<string> MenuList { get; } = MenuOptions.Select(x => x.Description).ToList();

    /// <summary>The choices to present in the choice prompt for the food-selection dialog.</summary>
    public static List<Choice> MenuChoices { get; } = ChoiceFactory.ToChoices(MenuList);

    /// <summary>The reprompt action for the food-selection dialog.</summary>
    public static Activity MenuReprompt
    {
        get
        {
            var reprompt = MessageFactory.SuggestedActions(MenuList, "Please choose an option");
            reprompt.AttachmentLayout = AttachmentLayoutTypes.List;
            return reprompt as Activity;
        }
    }
}
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-172">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-172">JavaScript</span></span>](#tab/javascript)

<span data-ttu-id="4e644-173">Cree una constante **dinnerMenu** que va a contener esta información.</span><span class="sxs-lookup"><span data-stu-id="4e644-173">Create a **dinnerMenu** constant to contain this information.</span></span>

```javascript
const dinnerMenu = {
    choices: ["Potato Salad - $5.99", "Tuna Sandwich - $6.89", "Clam Chowder - $4.50", 
        "Process order", "Cancel"],
    "Potato Salad - $5.99": {
        Description: "Potato Salad",
        Price: 5.99
    },
    "Tuna Sandwich - $6.89": {
        Description: "Tuna Sandwich",
        Price: 6.89
    },
    "Clam Chowder - $4.50": {
        Description: "Clam Chowder",
        Price: 4.50
    }
}
```

---

### <a name="create-the-welcome-dialog"></a><span data-ttu-id="4e644-174">Creación del diálogo de bienvenida</span><span class="sxs-lookup"><span data-stu-id="4e644-174">Create the welcome dialog</span></span>

<span data-ttu-id="4e644-175">Este diálogo usa un elemento `ChoicePrompt` para mostrar el menú y espera a que el usuario elija una opción.</span><span class="sxs-lookup"><span data-stu-id="4e644-175">This dialog uses a `ChoicePrompt` to display the menu and waits for the user to choose an option.</span></span> <span data-ttu-id="4e644-176">Cuando el usuario elige `Order dinner` o `Reserve a table`, se inicia el diálogo de la opción adecuada y, cuando ese diálogo finaliza, en lugar de simplemente terminar el diálogo en el último paso y dejar que el usuario se pregunte qué está pasando, este diálogo `mainMenu` se repite a sí mismo iniciando el diálogo `mainMenu` de nuevo.</span><span class="sxs-lookup"><span data-stu-id="4e644-176">When the user chooses either `Order dinner` or `Reserve a table`, it starts the dialog for the appropriate choice and when that dialog is done, instead of just ending the dialog in the last step and leaving the user wondering what's happening, this `mainMenu` dialog repeats itself by starting the `mainMenu` dialog again.</span></span> <span data-ttu-id="4e644-177">Con esta simple estructura, el bot siempre mostrará el menú y el usuario siempre sabrá cuáles son sus opciones.</span><span class="sxs-lookup"><span data-stu-id="4e644-177">With this simple structure, the bot will always show the menu and the user will always know what their choices are.</span></span>

<span data-ttu-id="4e644-178">El diálogo **MainMenu** contiene estos pasos de cascada.</span><span class="sxs-lookup"><span data-stu-id="4e644-178">The **MainMenu** dialog contains these waterfall steps.</span></span>

* <span data-ttu-id="4e644-179">En el primer paso de la cascada, se inicializa o se borra el estado del diálogo, se saluda al cliente y se le pide que elija entre las opciones disponibles: `Order dinner` o `Reserve a table`.</span><span class="sxs-lookup"><span data-stu-id="4e644-179">In the first step of the waterfall, we initialize or clear the dialog state, greet the guest, and ask them to choose from the available options: `Order dinner` or `Reserve a table`.</span></span>
* <span data-ttu-id="4e644-180">En el segundo paso, se recupera su selección y, a continuación, se inicia el diálogo secundario asociado a esa selección.</span><span class="sxs-lookup"><span data-stu-id="4e644-180">In the second step, we're retrieving their selection and then starting the child dialog that is associated with that selection.</span></span> <span data-ttu-id="4e644-181">Una vez que el diálogo secundario finaliza, este diálogo se reanudará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="4e644-181">Once the child dialog ends, this dialog will resume with the following step.</span></span>
* <span data-ttu-id="4e644-182">En el último paso, usamos el método **DialogContext.Replace** para reemplazar este diálogo por una nueva instancia de sí mismo, lo que convierte el diálogo de bienvenida en un menú en bucle.</span><span class="sxs-lookup"><span data-stu-id="4e644-182">In the last step, we use the **DialogContext.Replace** method to replace this dialog with a new instance of itself, which effectively turns the welcome dialog into a looping menu.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-183">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-183">C#</span></span>](#tab/csharp)

```csharp
// Add the main welcome dialog.
this.Add(MainMenu, new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        // Greet the guest and ask them to choose an option.
        await dc.Context.SendActivity("Welcome to Contoso Hotel and Resort.");
        await dc.Prompt(Inputs.Choice, "How may we serve you today?", new ChoicePromptOptions
        {
            Choices = Lists.WelcomeChoices,
            RetryPromptActivity = Lists.WelcomeReprompt,
        });
    },
    async (dc, args, next) =>
    {
        // Begin a child dialog associated with the chosen option.
        var choice = (FoundChoice)args["Value"];
        var dialogId = Lists.WelcomeOptions[choice.Index].DialogName;

        await dc.Begin(dialogId, dc.ActiveDialog.State);
    },
    async (dc, args, next) =>
    {
        // Start this dialog over again.
        await dc.Replace(MainMenu);
    },
});
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-184">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-184">JavaScript</span></span>](#tab/javascript)

```javascript
// Display a menu and ask user to choose a menu item. Direct user to the item selected otherwise, show
// the menu again.
dialogs.add('mainMenu', [
    async function(dc){
        await dc.context.sendActivity("Welcome to Contoso Hotel and Resort.");
        await dc.prompt('choicePrompt', "How may we serve you today?", ['Order Dinner', 'Reserve a table']);
    },
    async function(dc, result){
        if(result.value.match(/order dinner/ig)){
            await dc.begin('orderDinner');
        }
        else if(result.value.match(/reserve a table/ig)){
            await dc.begin('reserveTable');
        }
    },
    async function(dc, result){
        // Start over
        await dc.endAll().begin('mainMenu');
    }
]);
```

---

### <a name="create-the-order-dinner-dialog"></a><span data-ttu-id="4e644-185">Creación del diálogo de pedidos de cena</span><span class="sxs-lookup"><span data-stu-id="4e644-185">Create the order dinner dialog</span></span>

<span data-ttu-id="4e644-186">En el diálogo de pedidos de cena, se da la bienvenida al cliente al "servicio de pedidos de cena" y se inicia inmediatamente el diálogo de selección de comidas, que abordaremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="4e644-186">In the order-dinner dialog, we'll welcome the guest to the "dinner order service" and immediately start the food-selection dialog, which we'll cover in the next section.</span></span> <span data-ttu-id="4e644-187">Lo importante es que, si el cliente pide al servicio que procese el pedido, el diálogo de selección de comidas devuelve la lista de elementos del pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-187">Importantly, if the guest asks the service to process their order, the food-selection dialog returns the list of items on the order.</span></span> <span data-ttu-id="4e644-188">Para finalizar el proceso completo, este diálogo solicita un número de habitación para entregar la comida y, a continuación, envía un mensaje de confirmación antes de finalizar.</span><span class="sxs-lookup"><span data-stu-id="4e644-188">To complete the whole process, this dialog then asks for a room number to which to deliver the food, and then sends a confirmation message before ending.</span></span> <span data-ttu-id="4e644-189">Si el cliente cancela su pedido, este diálogo no pide un número de habitación y finaliza inmediatamente.</span><span class="sxs-lookup"><span data-stu-id="4e644-189">If the guest cancels their order, this dialog doesn't ask for a room number and ends immediately.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-190">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-190">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-191">Agregue una estructura de datos a la clase **HotelDialog** que podremos usar para hacer un seguimiento del pedido de cena del cliente.</span><span class="sxs-lookup"><span data-stu-id="4e644-191">To the **HotelDialog** class, add a data structure we can use to track the guest's dinner order.</span></span>

```csharp
/// <summary>Contains the guest's dinner order.</summary>
private class OrderCart : List<MenuChoice> { }
```

<span data-ttu-id="4e644-192">En el constructor **HotelDialog**, agregue el diálogo de pedidos de cena.</span><span class="sxs-lookup"><span data-stu-id="4e644-192">In the **HotelDialog** constructor, add the order-dinner dialog.</span></span>

* <span data-ttu-id="4e644-193">En el primer paso, se envía un mensaje de bienvenida y se inicia el diálogo de selección de comidas.</span><span class="sxs-lookup"><span data-stu-id="4e644-193">In the first step, we send a welcome message and start the food-selection dialog.</span></span>
* <span data-ttu-id="4e644-194">En el segundo paso, comprobamos si el diálogo de selección de comidas devuelve un carro del pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-194">In the second step, we check whether the food-selection dialog returned an order cart.</span></span>
  * <span data-ttu-id="4e644-195">Si es así, se le pedirá su número de habitación y se guarda la información del carro del pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-195">If so, prompt them for their room number and save the cart information.</span></span>
  * <span data-ttu-id="4e644-196">Si no, se supone que canceló su pedido y se llama a **DialogContext.End** para finalizar este diálogo.</span><span class="sxs-lookup"><span data-stu-id="4e644-196">If not, assume they cancelled their order, and call **DialogContext.End** to end this dialog.</span></span>
* <span data-ttu-id="4e644-197">En el último paso, se registra el número de habitación y se envía un mensaje de confirmación antes de finalizar.</span><span class="sxs-lookup"><span data-stu-id="4e644-197">In the last step, record their room number and send them a confirmation message before ending.</span></span> <span data-ttu-id="4e644-198">Este es el paso en que el bot reenviaría el pedido a un servicio de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="4e644-198">This is the step in which your bot would forward the order to a processing service.</span></span>

```csharp
// Add the order-dinner dialog.
this.Add(Dialogs.OrderDinner, new WaterfallStep[]
{
    async (dc, args, next) =>
    {
        await dc.Context.SendActivity("Welcome to our Dinner order service.");

        // Start the food selection dialog.
        await dc.Begin(Dialogs.OrderPrompt);
    },
    async (dc, args, next) =>
    {
        if (args.TryGetValue(Outputs.OrderCart, out object arg) && arg is OrderCart cart)
        {
            // If there are items in the order, record the order and ask for a room number.
            dc.ActiveDialog.State[Outputs.OrderCart] = cart;
            await dc.Prompt(Inputs.Number, "What is your room number?", new PromptOptions
            {
                RetryPromptString = "Please enter your room number."
            });
        }
        else
        {
            // Otherwise, assume the order was cancelled by the guest and exit.
            await dc.End();
        }
    },
    async (dc, args, next) =>
    {
        // Get and save the guest's answer.
        var roomNumber = args["Text"] as string;
        dc.ActiveDialog.State[Outputs.RoomNumber] = roomNumber;

        // Process the dinner order using the collected order cart and room number.

        await dc.Context.SendActivity($"Thank you. Your order will be delivered to room {roomNumber} within 45 minutes.");
        await dc.End();
    },
});
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-199">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-199">JavaScript</span></span>](#tab/javascript)

```javascript
// Order dinner:
// Help user order dinner from a menu
dialogs.add('orderDinner', [
    async function (dc){
        await dc.context.sendActivity("Welcome to our Dinner order service.");
        
        await dc.begin('orderPrompt', dc.activeDialog.state.orderCart); // Prompt for orders
    },
    async function (dc, result) {
        if(result == "Cancel"){
            await dc.end();
        }
        else { 
            await dc.prompt('numberPrompt', "What is your room number?");
        }
    },
    async function(dc, result){
        await dc.context.sendActivity(`Thank you. Your order will be delivered to room ${result} within 45 minutes.`);
        await dc.end();
    }
]);
```

---

### <a name="create-the-order-prompt-dialog"></a><span data-ttu-id="4e644-200">Creación del diálogo de pedidos</span><span class="sxs-lookup"><span data-stu-id="4e644-200">Create the order prompt dialog</span></span>

<span data-ttu-id="4e644-201">En el diálogo de selección de comidas, presentaremos al cliente una lista de opciones que incluye los elementos de la cena que pueden pedir y las dos solicitudes de procesamiento de pedidos.</span><span class="sxs-lookup"><span data-stu-id="4e644-201">In the food-selection dialog, we'll present the guest with a list of options that includes both the dinner items they can order and the two order processing requests.</span></span> <span data-ttu-id="4e644-202">Este diálogo se repite hasta que el cliente decide que el bot procese o cancele su pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-202">This dialog loops until the guest chooses to have the bot process or cancel their order.</span></span>

* <span data-ttu-id="4e644-203">Cuando el cliente selecciona un elemento de la cena, lo agregamos a su _carro_.</span><span class="sxs-lookup"><span data-stu-id="4e644-203">When the guest selects a dinner item, we add it to their _cart_.</span></span>
* <span data-ttu-id="4e644-204">Si el cliente elige procesar su pedido, primero comprobamos si el carro está vacío.</span><span class="sxs-lookup"><span data-stu-id="4e644-204">If the guest chooses to process their order, we first check whether the cart is empty.</span></span>
  * <span data-ttu-id="4e644-205">Si está vacío, se envía un mensaje de error y continúa el bucle.</span><span class="sxs-lookup"><span data-stu-id="4e644-205">If it's empty, we send an error message and continue looping.</span></span>
  * <span data-ttu-id="4e644-206">En caso contrario, se finaliza este diálogo y se devuelve la información del carro al diálogo primario.</span><span class="sxs-lookup"><span data-stu-id="4e644-206">Otherwise, we end this dialog, returning the cart information to the parent dialog.</span></span>
* <span data-ttu-id="4e644-207">Si el cliente elige cancelar su pedido, se finaliza el diálogo y no se devuelve información del carro.</span><span class="sxs-lookup"><span data-stu-id="4e644-207">If the guest chooses to cancel their order, we end the dialog, returning no cart information.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-208">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-208">C#</span></span>](#tab/csharp)

<span data-ttu-id="4e644-209">En el constructor **HotelDialog**, agregue el diálogo de selección de comidas.</span><span class="sxs-lookup"><span data-stu-id="4e644-209">In the **HotelDialog** constructor, add the food-selection dialog.</span></span>

* <span data-ttu-id="4e644-210">En el primer paso se inicializa el estado del diálogo.</span><span class="sxs-lookup"><span data-stu-id="4e644-210">In the first step, we initialize the dialog state.</span></span> <span data-ttu-id="4e644-211">Si los argumentos de entrada del diálogo contienen información del carro, la guardamos en el estado del diálogo; en caso contrario, se crea y se agrega un carro vacío.</span><span class="sxs-lookup"><span data-stu-id="4e644-211">If the input arguments to the dialog contain cart information, we save that to our dialog state; otherwise, we create an empty cart and add that.</span></span> <span data-ttu-id="4e644-212">A continuación, se le pedirá al cliente que seleccione una opción del menú de cenas.</span><span class="sxs-lookup"><span data-stu-id="4e644-212">We then prompt the guest for a choice from the dinner menu.</span></span>
* <span data-ttu-id="4e644-213">En el paso siguiente, nos centramos en la opción que el cliente ha seleccionado:</span><span class="sxs-lookup"><span data-stu-id="4e644-213">In the next step, we look at the option the guest picked:</span></span>
  * <span data-ttu-id="4e644-214">Si es una solicitud para procesar el pedido, se comprueba si el carro contiene algún elemento.</span><span class="sxs-lookup"><span data-stu-id="4e644-214">If it is a request to process the order, check whether the cart contains any items.</span></span>
    * <span data-ttu-id="4e644-215">Si es así, se finaliza el diálogo y se devuelve el contenido del carro.</span><span class="sxs-lookup"><span data-stu-id="4e644-215">If so, end the dialog, returning the cart contents.</span></span>
    * <span data-ttu-id="4e644-216">En caso contrario, se envía un mensaje de error al cliente y se empieza de nuevo desde el principio del diálogo.</span><span class="sxs-lookup"><span data-stu-id="4e644-216">Otherwise, send an error message to the guest and start over from the beginning of the dialog.</span></span>
  * <span data-ttu-id="4e644-217">Si es una solicitud para cancelar el pedido, se finaliza el diálogo y se devuelve un diccionario vacío.</span><span class="sxs-lookup"><span data-stu-id="4e644-217">If it is a request to cancel the order, end the dialog, returning an empty dictionary.</span></span>
  * <span data-ttu-id="4e644-218">Si es un elemento de la cena, se agrega al carro, se envía un mensaje de estado y el diálogo vuelve a comenzar, pasando el estado actual del pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-218">If it is a dinner item, add it to the cart, send a status message, and start the dialog over, passing in the current order state.</span></span>

```csharp
// Add the food-selection dialog.
this.Add(Dialogs.OrderPrompt, new WaterfallStep[]
    {
        async (dc, args, next) =>
        {
            if (args is null || !args.ContainsKey(Outputs.OrderCart))
            {
                // First time through, initialize the order state.
                dc.ActiveDialog.State[Outputs.OrderCart] = new OrderCart();
                dc.ActiveDialog.State[Outputs.OrderTotal] = 0.0;
            }
            else
            {
                // Otherwise, set the order state to that of the arguments.
                dc.ActiveDialog.State = new Dictionary<string, object>(args);
            }

            await dc.Prompt(Inputs.Choice, "What would you like?", new ChoicePromptOptions
            {
                Choices = Lists.MenuChoices,
                RetryPromptActivity = Lists.MenuReprompt,
            });
        },
        async (dc, args, next) =>
        {
            // Get the guest's choice.
            var choice = (FoundChoice)args["Value"];
            var option = Lists.MenuOptions[choice.Index];

            // Get the current order from dialog state.
            var cart = (OrderCart)dc.ActiveDialog.State[Outputs.OrderCart];

            if (option.Name is MenuChoice.Process)
            {
                if (cart.Count > 0)
                {
                    // If there are any items in the order, then exit this dialog,
                    // and return the list of selected food items.
                    await dc.End(new Dictionary<string, object>
                    {
                        [Outputs.OrderCart] = cart
                    });
                }
                else
                {
                    // Otherwise, send an error message and restart from
                    // the beginning of this dialog.
                    await dc.Context.SendActivity(
                        "Your cart is empty. Please add at least one item to the cart.");
                    await dc.Replace(Dialogs.OrderPrompt);
                }
            }
            else if (option.Name is MenuChoice.Cancel)
            {
                await dc.Context.SendActivity("Your order has been cancelled.");

                // Exit this dialog, returning an empty property bag.
                dc.ActiveDialog.State.Clear();
                await dc.End(new Dictionary<string, object>());
            }
            else
            {
                // Add the selected food item to the order and update the order total.
                cart.Add(option);
                var total = (double)dc.ActiveDialog.State[Outputs.OrderTotal] + option.Price;
                dc.ActiveDialog.State[Outputs.OrderTotal] = total;

                await dc.Context.SendActivity($"Added {option.Name} (${option.Price:0.00}) to your order." +
                    Environment.NewLine + Environment.NewLine +
                    $"Your current total is ${total:0.00}.");

                // Present the order options again, passing in the current order state.
                await dc.Replace(Dialogs.OrderPrompt, dc.ActiveDialog.State);
            }
        },
    });
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-219">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-219">JavaScript</span></span>](#tab/javascript)

```javascript
// Helper dialog to repeatedly prompt user for orders
dialogs.add('orderPrompt', [
    async function(dc, orderCart){
        // Define a new cart of one does not exists
        if(!orderCart){
            // Initialize a new cart
            // convoState = conversationState.get(dc.context);
            dc.activeDialog.state.orderCart = {
                orders: [],
                total: 0
            };
        }
        else {
            dc.activeDialog.state.orderCart = orderCart;
        }
        await dc.prompt('choicePrompt', "What would you like?", dinnerMenu.choices);
    },
    async function(dc, choice){
        // Get state object
        // convoState = conversationState.get(dc.context);

        if(choice.value.match(/process order/ig)){
            if(dc.activeDialog.state.orderCart.orders.length > 0) {
                // Process the order
                // ...
                dc.activeDialog.state.orderCart = undefined; // Reset cart
                await dc.context.sendActivity("Processing your order.");
                await dc.end();
            }
            else {
                await dc.context.sendActivity("Your cart was empty. Please add at least one item to the cart.");
                // Ask again
                await dc.replace('orderPrompt');
            }
        }
        else if(choice.value.match(/cancel/ig)){
            //dc.activeDialog.state.orderCart = undefined; // Reset cart
            await dc.context.sendActivity("Your order has been canceled.");
            await dc.end(choice.value);
        }
        else {
            var item = dinnerMenu[choice.value];

            // Only proceed if user chooses an item from the menu
            if(!item){
                await dc.context.sendActivity("Sorry, that is not a valid item. Please pick one from the menu.");
                
                // Ask again
                await dc.replace('orderPrompt');
            }
            else {
                // Add the item to cart
                dc.activeDialog.state.orderCart.orders.push(item);
                dc.activeDialog.state.orderCart.total += item.Price;

                await dc.context.sendActivity(`Added to cart: ${choice.value}. <br/>Current total: $${dc.activeDialog.state.orderCart.total}`);

                // Ask again
                await dc.replace('orderPrompt', dc.activeDialog.state.orderCart);
            }
        }
    }
]);
```

<span data-ttu-id="4e644-220">El código de ejemplo anterior muestra que el diálogo principal `orderDinner` usa un diálogo auxiliar llamado `orderPrompt` para controlar las opciones del usuario.</span><span class="sxs-lookup"><span data-stu-id="4e644-220">The sample code above shows that the main `orderDinner` dialog uses a helper dialog named `orderPrompt` to handle user choices.</span></span> <span data-ttu-id="4e644-221">El diálogo `orderPrompt` muestra el menú de elementos de comida, pide al usuario que elija un elemento, agrega el elemento al carro y le pregunta de nuevo en un bucle.</span><span class="sxs-lookup"><span data-stu-id="4e644-221">The `orderPrompt` dialog displays the menu of food items, asks the user to choose an item, add the item to cart and prompts again in a loop.</span></span> <span data-ttu-id="4e644-222">Esto permite al usuario agregar varios artículos a su pedido.</span><span class="sxs-lookup"><span data-stu-id="4e644-222">This allows the user to add multiple items to their order.</span></span> <span data-ttu-id="4e644-223">El diálogo se repite hasta que el usuario elige `Process order` o `Cancel`.</span><span class="sxs-lookup"><span data-stu-id="4e644-223">The dialog loops until the user chooses `Process order` or `Cancel`.</span></span> <span data-ttu-id="4e644-224">En ese momento, la ejecución se transfiere al diálogo principal (el diálogo `orderDinner`).</span><span class="sxs-lookup"><span data-stu-id="4e644-224">At which point, execution is handed back to the parent dialog (the `orderDinner` dialog).</span></span> <span data-ttu-id="4e644-225">El diálogo `orderDinner` realiza algunas tareas de última hora si el usuario quiere procesar el pedido; en caso contrario, finaliza y devuelve la ejecución de nuevo a su diálogo principal (p. ej.: `mainMenu`).</span><span class="sxs-lookup"><span data-stu-id="4e644-225">The `orderDinner` dialog does some last minute house keeping if the user wants to process the order; otherwise, it ends and returns execution back to its parent dialog (e.g.: `mainMenu`).</span></span> <span data-ttu-id="4e644-226">El diálogo `mainMenu` a su vez continúa ejecutando el último paso que es simplemente volver a mostrar las opciones del menú principal.</span><span class="sxs-lookup"><span data-stu-id="4e644-226">The `mainMenu` dialog in turn continues executing the last step which is to simply redisplay the main menu choices.</span></span>

---
### <a name="create-the-reserve-table-dialog"></a><span data-ttu-id="4e644-227">Creación del diálogo de reserva de mesa</span><span class="sxs-lookup"><span data-stu-id="4e644-227">Create the reserve table dialog</span></span>

<!-- TODO: Update the "Manage simple conversation flows" topic to use a reserveTable dialog, and then reference that from here. -->

<span data-ttu-id="4e644-228">Para no extender este ejemplo, se muestra solo una implementación mínima del diálogo `reserveTable`.</span><span class="sxs-lookup"><span data-stu-id="4e644-228">To keep this example shorter, we show only a minimal implementation of the `reserveTable` dialog here.</span></span>

# <a name="ctabcsharp"></a>[<span data-ttu-id="4e644-229">C#</span><span class="sxs-lookup"><span data-stu-id="4e644-229">C#</span></span>](#tab/csharp)

```csharp
// Add the table-reservation dialog.
this.Add(Dialogs.ReserveTable, new WaterfallStep[]
    {
        // Replace this waterfall with your reservation steps.
        async (dc, args, next) =>
        {
            await dc.Context.SendActivity("Your table has been reserved.");
            await dc.End();
        }
    });
```

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="4e644-230">JavaScript</span><span class="sxs-lookup"><span data-stu-id="4e644-230">JavaScript</span></span>](#tab/javascript)

```javascript
// Reserve a table:
// Help the user to reserve a table

dialogs.add('reserveTable', [
    // Replace this waterfall with your reservation steps.
    async function(dc, args, next){
        await dc.context.sendActivity("Your table has been reserved");
        await dc.end();
    }
]);
```

---

## <a name="next-steps"></a><span data-ttu-id="4e644-231">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4e644-231">Next steps</span></span>

<span data-ttu-id="4e644-232">Puede ofrecer otras opciones en el menú, como "más información" o "ayuda" para mejorar este bot.</span><span class="sxs-lookup"><span data-stu-id="4e644-232">You can enhance this bot by offering other options like "more info" or "help" to the menu choices.</span></span> <span data-ttu-id="4e644-233">Para más información sobre la implementación de estos tipos de interrupciones, consulte [Control de las interrupciones de usuario](bot-builder-howto-handle-user-interrupt.md).</span><span class="sxs-lookup"><span data-stu-id="4e644-233">For more information on implementing these types of interruptions, see [Handle user interruptions](bot-builder-howto-handle-user-interrupt.md).</span></span>

<span data-ttu-id="4e644-234">Ahora que ha aprendido a usar diálogos, avisos y cascadas para administrar flujos de conversación complejos, echemos un vistazo a cómo podemos dividir nuestros diálogos (como los diálogos `orderDinner` y `reserveTable`) en objetos independientes, en lugar de agruparlos en un conjunto de diálogos de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="4e644-234">Now that you have learned how to use dialogs, prompts, and waterfalls to manage complex conversation flows, let's take a look at how we can break our dialogs (such as the `orderDinner` and `reserveTable` dialogs) into separate objects, instead of lumping them all together in one large dialog set.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="4e644-235">Creación de lógica de bot modular con control compuesto</span><span class="sxs-lookup"><span data-stu-id="4e644-235">Create modular bot logic with Composite Control</span></span>](bot-builder-compositcontrol.md)
