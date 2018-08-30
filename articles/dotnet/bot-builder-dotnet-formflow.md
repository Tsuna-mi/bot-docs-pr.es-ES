---
title: Características básicas de FormFlow | Microsoft Docs
description: Aprenda a guiar los flujos de conversación con FormFlow en el SDK de Bot Builder para .NET.
author: RobStand
ms.author: kamrani
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: cf3d69f7941d8c3177788bd00e4b58416a71cf5e
ms.sourcegitcommit: 2dc75701b169d822c9499e393439161bc87639d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42904629"
---
# <a name="basic-features-of-formflow"></a><span data-ttu-id="5d39f-103">Características básicas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="5d39f-103">Basic features of FormFlow</span></span>

[!INCLUDE [pre-release-label](../includes/pre-release-label-v3.md)]

<span data-ttu-id="5d39f-104">Los [cuadros de diálogo ](bot-builder-dotnet-dialogs.md) son extremadamente eficaces y flexibles, pero administrar una conversación guiada, como ordenar un sándwich, puede requerir un gran esfuerzo.</span><span class="sxs-lookup"><span data-stu-id="5d39f-104">[Dialogs](bot-builder-dotnet-dialogs.md) are very powerful and flexible, but handling a guided conversation such as ordering a sandwich can require a lot of effort.</span></span> <span data-ttu-id="5d39f-105">En cada punto de la conversación se puede dar una gran cantidad de posibilidades, como las que se detallan a continuación.</span><span class="sxs-lookup"><span data-stu-id="5d39f-105">At each point in the conversation, there are many possibilities of what will happen next.</span></span> <span data-ttu-id="5d39f-106">Por ejemplo, es posible que necesite aclarar una ambigüedad, proporcionar ayuda, retroceder o mostrar el progreso.</span><span class="sxs-lookup"><span data-stu-id="5d39f-106">For example, you may need to clarify an ambiguity, provide help, go back, or show progress.</span></span> <span data-ttu-id="5d39f-107">Al usar **FormFlow** en el SDK de Bot Builder para .NET, puede simplificar enormemente el proceso para administrar una conversación guiada como esta.</span><span class="sxs-lookup"><span data-stu-id="5d39f-107">By using **FormFlow** within the Bot Builder SDK for .NET, you can greatly simplify the process of managing a guided conversation like this.</span></span> 

<span data-ttu-id="5d39f-108">FormFlow genera automáticamente los diálogos necesarios para gestionar una conversación guiada, según las pautas que especifique.</span><span class="sxs-lookup"><span data-stu-id="5d39f-108">FormFlow automatically generates the dialogs that are necessary to manage a guided conversation, based upon guidelines that you specify.</span></span> <span data-ttu-id="5d39f-109">Aunque al usar FormFlow sacrificará parte de la flexibilidad que de otro modo obtendría creando y administrando diálogos por su cuenta, la opción de diseñar una conversación guiada con FormFlow puede reducir significativamente el tiempo que lleva desarrollar el bot.</span><span class="sxs-lookup"><span data-stu-id="5d39f-109">Although using FormFlow sacrifices some of the flexibility that you might otherwise get by creating and managing dialogs on your own, designing a guided conversation using FormFlow can significantly reduce the time it takes to develop your bot.</span></span> <span data-ttu-id="5d39f-110">Además, puede construir su bot utilizando una combinación de cuadros de diálogo que haya generado FormFlow y otros tipos de cuadros de diálogo.</span><span class="sxs-lookup"><span data-stu-id="5d39f-110">Additionally, you may construct your bot using a combination of FormFlow-generated dialogs and other types of dialogs.</span></span> <span data-ttu-id="5d39f-111">Por ejemplo, un diálogo FormFlow puede guiar al usuario a través del proceso para completar un formulario, mientras que [LuisDialog][LuisDialog] puede evaluar la entrada del usuario para determinar la intención.</span><span class="sxs-lookup"><span data-stu-id="5d39f-111">For example, a FormFlow dialog may guide the user through the process of completing a form, while a [LuisDialog][LuisDialog] may evaluate user input to determine intent.</span></span>

<span data-ttu-id="5d39f-112">En este artículo se describe cómo crear un bot que utilice las funciones básicas de FormFlow para recopilar información de un usuario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-112">This article describes how to create a bot that uses the basic features of FormFlow to collect information from a user.</span></span>

## <a id="forms-and-fields"></a><span data-ttu-id="5d39f-113">Formularios y campos</span><span class="sxs-lookup"><span data-stu-id="5d39f-113">Forms and fields</span></span>

<span data-ttu-id="5d39f-114">Para crear un bot con FormFlow, debe especificar la información que el robot debe recopilar del usuario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-114">To create a bot using FormFlow, you must specify the information that the bot needs to collect from the user.</span></span> <span data-ttu-id="5d39f-115">Por ejemplo, si el objetivo del bot es obtener el pedido de un sándwich para el usuario, entonces debe definir un formulario que contenga campos adecuados para recopilar los datos que el bot necesita para realizar el pedido.</span><span class="sxs-lookup"><span data-stu-id="5d39f-115">For example, if the bot's objective is to obtain a user's sandwich order, then you must define a form that contains fields for the data that the bot needs to fulfill the order.</span></span> <span data-ttu-id="5d39f-116">Puede definir el formulario creando una clase de C# que contenga una o más propiedades públicas para representar los datos que el bot recopilará del usuario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-116">You can define the form by creating a C# class that contains one or more public properties to represent the data that the bot will collect from the user.</span></span> <span data-ttu-id="5d39f-117">Cada propiedad debe ser uno de estos tipos de datos:</span><span class="sxs-lookup"><span data-stu-id="5d39f-117">Each property must be one of these data types:</span></span>

- <span data-ttu-id="5d39f-118">Integral (sbyte, byte, short, ushort, int, uint, long, ulong)</span><span class="sxs-lookup"><span data-stu-id="5d39f-118">Integral (sbyte, byte, short, ushort, int, uint, long, ulong)</span></span>
- <span data-ttu-id="5d39f-119">Punto flotante (float, double)</span><span class="sxs-lookup"><span data-stu-id="5d39f-119">Floating point (float, double)</span></span>
- <span data-ttu-id="5d39f-120">string</span><span class="sxs-lookup"><span data-stu-id="5d39f-120">String</span></span>
- <span data-ttu-id="5d39f-121">Datetime</span><span class="sxs-lookup"><span data-stu-id="5d39f-121">DateTime</span></span>
- <span data-ttu-id="5d39f-122">Enumeration</span><span class="sxs-lookup"><span data-stu-id="5d39f-122">Enumeration</span></span>
- <span data-ttu-id="5d39f-123">Lista de enumeraciones</span><span class="sxs-lookup"><span data-stu-id="5d39f-123">List of enumerations</span></span>

<span data-ttu-id="5d39f-124">Es necesario que cualquiera de los tipos de datos admitan valores NULL, ya que los puede usar para especificar que el campo no tiene un valor.</span><span class="sxs-lookup"><span data-stu-id="5d39f-124">Any of the data types may be nullable, which you can use to model that the field does not have a value.</span></span> <span data-ttu-id="5d39f-125">Si un campo de formulario se basa en una propiedad de enumeración que no admite valores NULL, el valor **0** de la enumeración representa **null** (es decir, indica que el campo no tiene un valor), y sus valores de enumeración deben comenzar por **1**.</span><span class="sxs-lookup"><span data-stu-id="5d39f-125">If a form field is based on an enumeration property that is not nullable, the value **0** in the enumeration represents **null** (i.e., indicates that the field does not have a value), and you should start your enumeration values at **1**.</span></span> <span data-ttu-id="5d39f-126">FormFlow ignora el resto de métodos y tipos de propiedad.</span><span class="sxs-lookup"><span data-stu-id="5d39f-126">FormFlow ignores all other property types and methods.</span></span>

<span data-ttu-id="5d39f-127">En cuanto a los objetos complejos, debe crear un formulario para la clase C# de nivel superior, y otro para el objeto complejo.</span><span class="sxs-lookup"><span data-stu-id="5d39f-127">For complex objects, you must create a form for the top-level C# class and another form for the complex object.</span></span> <span data-ttu-id="5d39f-128">Puede crear los formularios mediante la típica semántica de [diálogo](bot-builder-dotnet-dialogs.md).</span><span class="sxs-lookup"><span data-stu-id="5d39f-128">You can compose the forms together by using typical [dialog](bot-builder-dotnet-dialogs.md) semantics.</span></span> <span data-ttu-id="5d39f-129">También es posible definir un formulario implementando directamente [Advanced.IField][iField] o usando [Advanced.Field][field] y rellenando los diccionarios que se encuentran en ese elemento.</span><span class="sxs-lookup"><span data-stu-id="5d39f-129">It is also possible to define a form directly by implementing [Advanced.IField][iField] or using [Advanced.Field][field] and populating the dictionaries within it.</span></span> 

> [!NOTE]
> <span data-ttu-id="5d39f-130">Puede definir un formulario con una clase de C# o un esquema JSON.</span><span class="sxs-lookup"><span data-stu-id="5d39f-130">You can define a form by using either a C# class or JSON schema.</span></span> <span data-ttu-id="5d39f-131">En este artículo se describe cómo definir un formulario mediante una clase de C#.</span><span class="sxs-lookup"><span data-stu-id="5d39f-131">This article describes how to define a form using a C# class.</span></span> <span data-ttu-id="5d39f-132">Para obtener más información sobre cómo usar el esquema JSON, consulte [Define a form using JSON schema](bot-builder-dotnet-formflow-json-schema.md) (Definir un formulario con el esquema JSON).</span><span class="sxs-lookup"><span data-stu-id="5d39f-132">For more information about using JSON schema, see [Define a form using JSON schema](bot-builder-dotnet-formflow-json-schema.md).</span></span>

## <a name="simple-sandwich-bot"></a><span data-ttu-id="5d39f-133">Bot simple de sándwich</span><span class="sxs-lookup"><span data-stu-id="5d39f-133">Simple sandwich bot</span></span>

<span data-ttu-id="5d39f-134">En este ejemplo le mostramos un bot simple de sándwich diseñado para obtener el pedido que hizo un usuario para recibir un sándwich.</span><span class="sxs-lookup"><span data-stu-id="5d39f-134">Consider this example of a simple sandwich bot that is designed to obtain a user's sandwich order.</span></span> 

### <a id="create-class"></a> <span data-ttu-id="5d39f-135">Crear el formulario</span><span class="sxs-lookup"><span data-stu-id="5d39f-135">Create the form</span></span>

<span data-ttu-id="5d39f-136">La clase `SandwichOrder` define la forma, y las enumeraciones definen las opciones con los ingredientes para hacer el sándwich.</span><span class="sxs-lookup"><span data-stu-id="5d39f-136">The `SandwichOrder` class defines the form and the enumerations define the options for building a sandwich.</span></span> <span data-ttu-id="5d39f-137">La clase también incluye el método estático `BuildForm` que usa [FormBuilder][formBuilder] para crear el formulario y definir un mensaje de bienvenida simple.</span><span class="sxs-lookup"><span data-stu-id="5d39f-137">The class also includes the static `BuildForm` method that uses [FormBuilder][formBuilder] to create the form and define a simple welcome message.</span></span> 

<span data-ttu-id="5d39f-138">Para usar FormFlow, primero debe importar el espacio de nombres `Microsoft.Bot.Builder.FormFlow`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-138">To use FormFlow, you must first import the `Microsoft.Bot.Builder.FormFlow` namespace.</span></span>

[!code-csharp[Define form](../includes/code/dotnet-formflow.cs#defineForm)]

### <a name="connect-the-form-to-the-framework"></a><span data-ttu-id="5d39f-139">Conectar el formulario al marco</span><span class="sxs-lookup"><span data-stu-id="5d39f-139">Connect the form to the framework</span></span> 

<span data-ttu-id="5d39f-140">Para conectar el formulario al marco, debe agregarlo al controlador.</span><span class="sxs-lookup"><span data-stu-id="5d39f-140">To connect the form to the framework, you must add it to the controller.</span></span> <span data-ttu-id="5d39f-141">En este ejemplo, el método `Conversation.SendAsync` llama al método estático `MakeRootDialog`, que a su vez llama al método `FormDialog.FromForm` para crear el formulario `SandwichOrder`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-141">In this example, the `Conversation.SendAsync` method calls the static `MakeRootDialog` method, which in turn, calls the `FormDialog.FromForm` method to create the `SandwichOrder` form.</span></span> 

[!code-csharp[Connect form to framework](../includes/code/dotnet-formflow.cs#connectToFramework)]

### <a name="see-it-in-action"></a><span data-ttu-id="5d39f-142">Véalo en acción</span><span class="sxs-lookup"><span data-stu-id="5d39f-142">See it in action</span></span>

<span data-ttu-id="5d39f-143">Solo tiene que definir el formulario con una clase C# y conectarlo al marco, para habilitar FormFlow para administrar automáticamente la conversación entre el bot y el usuario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-143">By simply defining the form with a C# class and connecting it to the framework, you have enabled FormFlow to automatically manage the conversation between bot and user.</span></span> <span data-ttu-id="5d39f-144">Las interacciones de ejemplo que se muestran a continuación demuestran las capacidades de un bot que se crea mediante el uso de las funciones básicas de FormFlow.</span><span class="sxs-lookup"><span data-stu-id="5d39f-144">The example interactions shown below demonstrate the capabilities of a bot that is created by using the basic features of FormFlow.</span></span> <span data-ttu-id="5d39f-145">En cada interacción, un símbolo **>** indica el punto en el que el usuario escribe una respuesta.</span><span class="sxs-lookup"><span data-stu-id="5d39f-145">In each interaction, a **>** symbol indicates the point at which the user enters a response.</span></span> 

#### <a name="display-the-first-prompt"></a><span data-ttu-id="5d39f-146">Mostrar el primer aviso</span><span class="sxs-lookup"><span data-stu-id="5d39f-146">Display the first prompt</span></span> 

<span data-ttu-id="5d39f-147">Este formulario rellena la propiedad `SandwichOrder.Sandwich`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-147">This form populates the `SandwichOrder.Sandwich` property.</span></span> <span data-ttu-id="5d39f-148">El formulario genera automáticamente el mensaje "Seleccione un sándwich", donde la palabra "sándwich" de la solicitud deriva del nombre de propiedad `Sandwich`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-148">The form automatically generates the prompt, "Please select a sandwich", where the word "sandwich" in the prompt derives from the property name `Sandwich`.</span></span> <span data-ttu-id="5d39f-149">La enumeración `SandwichOptions` define las opciones que se presentan al usuario, y cada valor de enumeración se divide automáticamente en palabras en función de los cambios en mayúsculas, minúsculas y guiones bajos.</span><span class="sxs-lookup"><span data-stu-id="5d39f-149">The `SandwichOptions` enumeration defines the choices that are presented to the user, with each enumeration value being automatically broken into words based upon changes in case and underscores.</span></span>

```console
Please select a sandwich
1. BLT
2. Black Forest Ham
3. Buffalo Chicken
4. Chicken And Bacon Ranch Melt
5. Cold Cut Combo
6. Meatball Marinara
7. Oven Roasted Chicken
8. Roast Beef
9. Rotisserie Style Chicken
10. Spicy Italian
11. Steak And Cheese
12. Sweet Onion Teriyaki
13. Tuna
14. Turkey Breast
15. Veggie
>
```

#### <a name="provide-guidance"></a><span data-ttu-id="5d39f-150">Proporcionar una guía</span><span class="sxs-lookup"><span data-stu-id="5d39f-150">Provide guidance</span></span>

<span data-ttu-id="5d39f-151">El usuario puede escribir "ayuda" en cualquier punto de la conversación para obtener ayuda acerca de cómo completar el formulario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-151">The user can enter "help" at any point in the conversation to get guidance with filling out the form.</span></span> <span data-ttu-id="5d39f-152">Por ejemplo, si el usuario escribe "ayuda" en la solicitud correspondiente al sándwich, el bot responderá con esta guía.</span><span class="sxs-lookup"><span data-stu-id="5d39f-152">For example, if the user enters "help" at the sandwich prompt, the bot will respond with this guidance.</span></span> 

```console
> help
* You are filling in the sandwich field. Possible responses:
* You can enter a number 1-15 or words from the descriptions. (BLT, Black Forest Ham, Buffalo Chicken, Chicken And Bacon Ranch Melt, Cold Cut Combo, Meatball Marinara, Oven Roasted Chicken, Roast Beef, Rotisserie Style Chicken, Spicy Italian, Steak And Cheese, Sweet Onion Teriyaki, Tuna, Turkey Breast, and Veggie)
* Back: Go back to the previous question.
* Help: Show the kinds of responses you can enter.
* Quit: Quit the form without completing it.
* Reset: Start over filling in the form. (With defaults from your previous entries.)
* Status: Show your progress in filling in the form so far.
* You can switch to another field by entering its name. (Sandwich, Length, Bread, Cheese, Toppings, and Sauce).
```

#### <a name="advance-to-the-next-prompt"></a><span data-ttu-id="5d39f-153">Avanzar a la siguiente solicitud</span><span class="sxs-lookup"><span data-stu-id="5d39f-153">Advance to the next prompt</span></span>

<span data-ttu-id="5d39f-154">Si el usuario escribe "2" en respuesta la solicitud de sándwich inicial, el bot muestra una solicitud para ir a la siguiente propiedad que se define mediante el formulario: `SandwichOrder.Length`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-154">If the user enters "2" in response to the initial sandwich prompt, the bot then displays a prompt for the next property that is defined by the form: `SandwichOrder.Length`.</span></span>

```console
Please select a sandwich
 1. BLT
 2. Black Forest Ham
 3. Buffalo Chicken
 4. Chicken And Bacon Ranch Melt
 5. Cold Cut Combo
 6. Meatball Marinara
 7. Oven Roasted Chicken
 8. Roast Beef
 9. Rotisserie Style Chicken
 10. Spicy Italian
 11. Steak And Cheese
 12. Sweet Onion Teriyaki
 13. Tuna
 14. Turkey Breast
 15. Veggie
> 2
Please select a length (1. Six Inch, 2. Foot Long)
> 
```

#### <a name="return-to-the-previous-prompt"></a><span data-ttu-id="5d39f-155">Volver a la solicitud anterior</span><span class="sxs-lookup"><span data-stu-id="5d39f-155">Return to the previous prompt</span></span> 

<span data-ttu-id="5d39f-156">Si el usuario escribe "volver" en este punto de la conversación, el bot devolverá la solicitud anterior.</span><span class="sxs-lookup"><span data-stu-id="5d39f-156">If the user enters "back" at this point in the conversation, the bot will return the previous prompt.</span></span> <span data-ttu-id="5d39f-157">El mensaje mostrará la elección actual del usuario ("Jamón de la Selva Negra"); el usuario puede cambiar esa selección escribiendo un número diferente o confirmar esa selección con la letra "c".</span><span class="sxs-lookup"><span data-stu-id="5d39f-157">The prompt shows the user's current choice ("Black Forest Ham"); the user may change that selection by entering a different number or confirm that selection by entering "c".</span></span>

```console
> back
Please select a sandwich(current choice: Black Forest Ham)
 1. BLT
 2. Black Forest Ham
 3. Buffalo Chicken
 4. Chicken And Bacon Ranch Melt
 5. Cold Cut Combo
 6. Meatball Marinara
 7. Oven Roasted Chicken
 8. Roast Beef
 9. Rotisserie Style Chicken
 10. Spicy Italian
 11. Steak And Cheese
 12. Sweet Onion Teriyaki
 13. Tuna
 14. Turkey Breast
 15. Veggie
> c
Please select a length (1. Six Inch, 2. Foot Long)
> 
```

#### <a name="clarify-user-input"></a><span data-ttu-id="5d39f-158">Aclarar la entrada del usuario</span><span class="sxs-lookup"><span data-stu-id="5d39f-158">Clarify user input</span></span>

<span data-ttu-id="5d39f-159">Si el usuario responde con texto (en lugar de con un número) para indicar una opción, el bot automáticamente solicitará una aclaración si la entrada del usuario coincide con más de una opción.</span><span class="sxs-lookup"><span data-stu-id="5d39f-159">If the user responds with text (instead of a number) to indicate a choice, the bot will automatically ask for clarification if user input matches more than one choice.</span></span> 

```console
Please select a bread
 1. Nine Grain Wheat
 2. Nine Grain Honey Oat
 3. Italian
 4. Italian Herbs And Cheese
 5. Flatbread
> nine grain
By "nine grain" bread did you mean (1. Nine Grain Honey Oat, 2. Nine Grain Wheat)
> 1
```

<span data-ttu-id="5d39f-160">Si la entrada del usuario no coincide con ninguna de las opciones válidas, el bot automáticamente solicitará al usuario una aclaración.</span><span class="sxs-lookup"><span data-stu-id="5d39f-160">If user input does not directly match any of the valid choices, the bot will automatically prompt the user for clarification.</span></span>

```console
Please select a cheese (1. American, 2. Monterey Cheddar, 3. Pepperjack)
> amercan
"amercan" is not a cheese option.
> american smoked
For cheese I understood American. "smoked" is not an option.
```

<span data-ttu-id="5d39f-161">Si la entrada del usuario especifica múltiples opciones para una propiedad y el bot no entiende ninguna de las opciones especificadas, automáticamente solicitará al usuario una aclaración.</span><span class="sxs-lookup"><span data-stu-id="5d39f-161">If user input specifies multiple choices for a property and the bot does not understand any of the specified choices, it will automatically prompt the user for clarification.</span></span>

```console
Please select one or more toppings
 1. Banana Peppers
 2. Cucumbers
 3. Green Bell Peppers
 4. Jalapenos
 5. Lettuce
 6. Olives
 7. Pickles
 8. Red Onion
 9. Spinach
 10. Tomatoes
> peppers, lettuce and tomato
By "peppers" toppings did you mean (1. Green Bell Peppers, 2. Banana Peppers)
> 1
```

#### <a name="show-current-status"></a><span data-ttu-id="5d39f-162">Mostrar estado actual</span><span class="sxs-lookup"><span data-stu-id="5d39f-162">Show current status</span></span>

<span data-ttu-id="5d39f-163">Si el usuario escribe "estado" en cualquier punto del pedido, la respuesta del bot indicará qué valores se han especificado ya y qué valores quedan por especificar.</span><span class="sxs-lookup"><span data-stu-id="5d39f-163">If the user enters "status" at any point in the order, the bot's response will indicate which values have already been specified and which values remain to be specified.</span></span> 

```console
Please select one or more sauce
 1. Honey Mustard
 2. Light Mayonnaise
 3. Regular Mayonnaise
 4. Mustard
 5. Oil
 6. Pepper
 7. Ranch
 8. Sweet Onion
 9. Vinegar
> status
* Sandwich: Black Forest Ham
* Length: Six Inch
* Bread: Nine Grain Honey Oat
* Cheese: American
* Toppings: Lettuce, Tomatoes, and Green Bell Peppers
* Sauce: Unspecified  
```

#### <a name="confirm-selections"></a><span data-ttu-id="5d39f-164">Confirmar selecciones</span><span class="sxs-lookup"><span data-stu-id="5d39f-164">Confirm selections</span></span>

<span data-ttu-id="5d39f-165">Cuando el usuario complete el formulario, el bot le pedirá que confirme lo que ha seleccionado.</span><span class="sxs-lookup"><span data-stu-id="5d39f-165">When the user completes the form, the bot will ask the user to confirm their selections.</span></span>

```console
Please select one or more sauce
 1. Honey Mustard
 2. Light Mayonnaise
 3. Regular Mayonnaise
 4. Mustard
 5. Oil
 6. Pepper
 7. Ranch
 8. Sweet Onion
 9. Vinegar
> 1
Is this your selection?
* Sandwich: Black Forest Ham
* Length: Six Inch
* Bread: Nine Grain Honey Oat
* Cheese: American
* Toppings: Lettuce, Tomatoes, and Green Bell Peppers
* Sauce: Honey Mustard
>
```

<span data-ttu-id="5d39f-166">Si el usuario responde escribiendo "no", el bot le permite actualizar cualquiera de las selecciones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5d39f-166">If the user responds by entering "no", the bot allows the user to update any of the prior selections.</span></span> <span data-ttu-id="5d39f-167">Si el usuario responde "sí", el formulario se habrá completado y el control se devuelve al cuadro de diálogo que realiza la llamada.</span><span class="sxs-lookup"><span data-stu-id="5d39f-167">If the user responds by entering "yes", the form has been completed and control is returned to the calling dialog.</span></span> 

```console
Is this your selection?
* Sandwich: Black Forest Ham
* Length: Six Inch
* Bread: Nine Grain Honey Oat
* Cheese: American
* Toppings: Lettuce, Tomatoes, and Green Bell Peppers
* Sauce: Honey Mustard
> no
What do you want to change?
 1. Sandwich(Black Forest Ham)
 2. Length(Six Inch)
 3. Bread(Nine Grain Honey Oat)
 4. Cheese(American)
 5. Toppings(Lettuce, Tomatoes, and Green Bell Peppers)
 6. Sauce(Honey Mustard)
> 2
Please select a length (current choice: Six Inch) (1. Six Inch, 2. Foot Long)
> 2
Is this your selection?
* Sandwich: Black Forest Ham
* Length: Foot Long
* Bread: Nine Grain Honey Oat
* Cheese: American
* Toppings: Lettuce, Tomatoes, and Green Bell Peppers
* Sauce: Honey Mustard
> y
```

## <a name="handling-quit-and-exceptions"></a><span data-ttu-id="5d39f-168">Administrar la salida del proceso y las excepciones</span><span class="sxs-lookup"><span data-stu-id="5d39f-168">Handling quit and exceptions</span></span>

<span data-ttu-id="5d39f-169">Si el usuario escribe "salir" en el formulario o se produce una excepción en algún momento de la conversación, el bot deberá saber el paso en el que ocurrió el evento, el estado del formulario cuando ocurrió ese evento y qué pasos del formulario se completaron correctamente antes del evento.</span><span class="sxs-lookup"><span data-stu-id="5d39f-169">If the user enters "quit" in the form or an exception occurs at some point in the conversation, your bot will need to know the step in which the event occurred, the state of the form when the event occurred, and which steps of the form were successfully completed prior to the event.</span></span> <span data-ttu-id="5d39f-170">El formulario devuelve esta información a través de la clase `FormCanceledException<T>`.</span><span class="sxs-lookup"><span data-stu-id="5d39f-170">The form returns this information via the `FormCanceledException<T>` class.</span></span> 

<span data-ttu-id="5d39f-171">En este ejemplo de código se muestra cómo detectar la excepción y mostrar un mensaje de acuerdo con el evento que ocurrió.</span><span class="sxs-lookup"><span data-stu-id="5d39f-171">This code example shows how to catch the exception and display a message according to the event that occurred.</span></span> 

[!code-csharp[Handle exception or quit](../includes/code/dotnet-formflow.cs#handleExceptionOrQuit)]

## <a name="summary"></a><span data-ttu-id="5d39f-172">Resumen</span><span class="sxs-lookup"><span data-stu-id="5d39f-172">Summary</span></span>

<span data-ttu-id="5d39f-173">En este artículo se ha descrito cómo usar las características básicas de FormFlow para crear un bot que pueda:</span><span class="sxs-lookup"><span data-stu-id="5d39f-173">This article has described how to use the basic features of FormFlow to create a bot that can:</span></span>

- <span data-ttu-id="5d39f-174">Crear y administrar automáticamente la conversación.</span><span class="sxs-lookup"><span data-stu-id="5d39f-174">Automatically generate and manage the conversation</span></span>
- <span data-ttu-id="5d39f-175">Proporcionar una guía y ayuda claras.</span><span class="sxs-lookup"><span data-stu-id="5d39f-175">Provide clear guidance and help</span></span>
- <span data-ttu-id="5d39f-176">Comprender los números y las entradas de texto.</span><span class="sxs-lookup"><span data-stu-id="5d39f-176">Understand both numbers and textual entries</span></span>
- <span data-ttu-id="5d39f-177">Proporcionar comentarios al usuario sobre lo que entiende y lo que no.</span><span class="sxs-lookup"><span data-stu-id="5d39f-177">Provide feedback to the user regarding what is understood and what is not</span></span> 
- <span data-ttu-id="5d39f-178">Hacer preguntas aclaratorias cuando sea necesario.</span><span class="sxs-lookup"><span data-stu-id="5d39f-178">Ask clarifying questions when necessary</span></span> 
- <span data-ttu-id="5d39f-179">Permitir al usuario navegar entre pasos.</span><span class="sxs-lookup"><span data-stu-id="5d39f-179">Allow the user to navigate between steps</span></span>

<span data-ttu-id="5d39f-180">Aunque la funcionalidad básica de FormFlow es suficiente en algunos casos, le recomendamos que considere los beneficios potenciales que conlleva incorporar algunas de las características más avanzadas de FormFlow en el bot.</span><span class="sxs-lookup"><span data-stu-id="5d39f-180">Although basic FormFlow functionality is sufficient in some cases, you should consider the potential benefits of incorporating some of the more advanced features of FormFlow into your bot.</span></span> <span data-ttu-id="5d39f-181">Para obtener más información, consulte [Advanced features of FormFlow](bot-builder-dotnet-formflow-advanced.md) (Funciones avanzadas de FormFlow) y [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md) (Personalizar un formulario utilizando FormBuilder).</span><span class="sxs-lookup"><span data-stu-id="5d39f-181">For more information, see [Advanced features of FormFlow](bot-builder-dotnet-formflow-advanced.md) and [Customize a form using FormBuilder](bot-builder-dotnet-formflow-formbuilder.md).</span></span>

## <a name="sample-code"></a><span data-ttu-id="5d39f-182">Código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="5d39f-182">Sample code</span></span>

[!INCLUDE [Sample code](../includes/snippet-dotnet-formflow-samples.md)]

## <a name="next-steps"></a><span data-ttu-id="5d39f-183">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5d39f-183">Next steps</span></span>

<span data-ttu-id="5d39f-184">FormFlow simplifica el desarrollo del cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="5d39f-184">FormFlow simplifies dialog development.</span></span> <span data-ttu-id="5d39f-185">Las características avanzadas de FormFlow le permiten personalizar cómo se comporta un objeto de FormFlow.</span><span class="sxs-lookup"><span data-stu-id="5d39f-185">The advanced features of FormFlow let you customize how a FormFlow object behaves.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5d39f-186">Características avanzadas de FormFlow</span><span class="sxs-lookup"><span data-stu-id="5d39f-186">Advanced features of FormFlow</span></span>](bot-builder-dotnet-formflow-advanced.md)

## <a name="additional-resources"></a><span data-ttu-id="5d39f-187">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5d39f-187">Additional resources</span></span>

- [<span data-ttu-id="5d39f-188">Personalización de un formulario mediante FormBuilder</span><span class="sxs-lookup"><span data-stu-id="5d39f-188">Customize a form using FormBuilder</span></span>](bot-builder-dotnet-formflow-formbuilder.md)
- [<span data-ttu-id="5d39f-189">Localización del contenido del formulario</span><span class="sxs-lookup"><span data-stu-id="5d39f-189">Localize form content</span></span>](bot-builder-dotnet-formflow-localize.md)
- [<span data-ttu-id="5d39f-190">Definición de un formulario mediante esquemas JSON</span><span class="sxs-lookup"><span data-stu-id="5d39f-190">Define a form using JSON schema</span></span>](bot-builder-dotnet-formflow-json-schema.md)
- [<span data-ttu-id="5d39f-191">Personalización de la experiencia de usuario con lenguaje de patrones</span><span class="sxs-lookup"><span data-stu-id="5d39f-191">Customize user experience with pattern language</span></span>](bot-builder-dotnet-formflow-pattern-language.md)
- <span data-ttu-id="5d39f-192"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Referencia de Bot Builder SDK para .NET</a></span><span class="sxs-lookup"><span data-stu-id="5d39f-192"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[LuisDialog]: /dotnet/api/microsoft.bot.builder.dialogs.luisdialog-1

[iField]: /dotnet/api/microsoft.bot.builder.formflow.advanced.ifield-1

[field]: /dotnet/api/microsoft.bot.builder.formflow.advanced.field-1

[formBuilder]: /dotnet/api/microsoft.bot.builder.formflow.formbuilder-1
