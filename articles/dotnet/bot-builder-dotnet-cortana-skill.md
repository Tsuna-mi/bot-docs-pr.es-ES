---
title: Creación de una habilidad de Cortana mediante .NET | Microsoft Docs
description: Obtenga información sobre los conceptos básicos para la creación de una habilidad de Cortana en el SDK de Bot Builder para. NET.
keywords: Bot Framework, habilidad de Cortana, voz,. NET, Bot Builder, SDK, conceptos clave, conceptos básicos
author: DeniseMak
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/2017
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: e6bfd890944cfea052e07ee99451ab90db75415b
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39304740"
---
# <a name="build-a-speech-enabled-bot-with-cortana-skills"></a><span data-ttu-id="cb40c-104">Creación de un bot habilitado para la voz con habilidades de Cortana</span><span class="sxs-lookup"><span data-stu-id="cb40c-104">Build a speech-enabled bot with Cortana skills</span></span>
> [!div class="op_single_selector"]
> - [.NET](../dotnet/bot-builder-dotnet-cortana-skill.md)
> - [Node.js](../nodejs/bot-builder-nodejs-cortana-skill.md)


<span data-ttu-id="cb40c-107">El SDK de Bot Builder para .NET le permite crear un bot habilitado para la voz al conectarlo al canal de Cortana como una habilidad de Cortana.</span><span class="sxs-lookup"><span data-stu-id="cb40c-107">The Bot Builder SDK for .NET enables you to a build speech-enabled bot by connecting it to the Cortana channel as a Cortana skill.</span></span> 


> [!TIP]
> Para obtener más información sobre qué es una habilidad y qué puede hacer, consulte [The Cortana Skills Kit][CortanaGetStarted] (El kit de habilidades de Cortana).

<span data-ttu-id="cb40c-109">La creación de una habilidad de Cortana con Bot Framework requiere muy poco conocimiento específico de Cortana y consiste básicamente en compilar un bot.</span><span class="sxs-lookup"><span data-stu-id="cb40c-109">Creating a Cortana skill using Bot Framework requires very little Cortana-specific knowledge and primarily consists of building a bot.</span></span> <span data-ttu-id="cb40c-110">Una de las posibles diferencias principales respecto a otros bots que ha creado en el pasado es que Cortana tiene un componente visual y uno de audio.</span><span class="sxs-lookup"><span data-stu-id="cb40c-110">One of the likely key differences from other bots that you may have created in the past is that Cortana has both a visual and an audio component.</span></span> <span data-ttu-id="cb40c-111">Para el componente visual, Cortana proporciona un área del lienzo para representar el contenido, como tarjetas.</span><span class="sxs-lookup"><span data-stu-id="cb40c-111">For the visual component, Cortana provides an area of the canvas for rendering content such as cards.</span></span> <span data-ttu-id="cb40c-112">Para el componente de audio, proporcione texto o SSML en los mensajes del bot para que Cortana los lea al usuario, dándole una voz.</span><span class="sxs-lookup"><span data-stu-id="cb40c-112">For the audio component, you provide text or SSML in your bot's messages, which Cortana reads to the user, giving your bot a voice.</span></span> 

> [!NOTE]
> Cortana está disponible en muchos dispositivos diferentes. Algunos tienen una pantalla mientras que otros, como los altavoces independientes, puede que no. Asegúrese de que su bot es capaz de actuar en ambos escenarios. Consulte [Cortana-specific entities][CortanaSpecificEntities] (Las entidades específicas de Cortana) para obtener información sobre cómo comprobar la información del dispositivo.

## <a name="adding-speech-to-your-bot"></a><span data-ttu-id="cb40c-117">Adición de voz al bot</span><span class="sxs-lookup"><span data-stu-id="cb40c-117">Adding speech to your bot</span></span>

<span data-ttu-id="cb40c-118">Los mensajes de voz de su bot se representan en el lenguaje de marcado de síntesis de voz (SSML).</span><span class="sxs-lookup"><span data-stu-id="cb40c-118">Spoken messages from your bot are represented as Speech Synthesis Markup Language (SSML).</span></span> <span data-ttu-id="cb40c-119">El SDK de Bot Builder permite incluir SSML en las respuestas del bot para controlar lo que dice y lo que muestra.</span><span class="sxs-lookup"><span data-stu-id="cb40c-119">The Bot Builder SDK lets you include SSML in your bot's responses to control what the bot says, in addition to what it shows.</span></span>  <span data-ttu-id="cb40c-120">También puede controlar el estado del micrófono de Cortana al especificar que el bot acepta, espera o ignora la interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-120">You can also control the state of Cortana's microphone, by specifying whether your bot is accepting, expecting, or ignoring user input.</span></span>

<span data-ttu-id="cb40c-121">Establezca la propiedad `Speak` del objeto `IMessageActivity` para especificar el mensaje que Cortana dirá.</span><span class="sxs-lookup"><span data-stu-id="cb40c-121">Set the `Speak` property of the `IMessageActivity` object to specify a message for Cortana to say.</span></span> <span data-ttu-id="cb40c-122">Si se especifica texto sin formato, Cortana determinará cómo se pronuncian las palabras.</span><span class="sxs-lookup"><span data-stu-id="cb40c-122">If you specify plain text, Cortana determines how the words are pronounced.</span></span> 

```cs
Activity reply = activity.CreateReply("This is the text that Cortana displays."); 
reply.Speak = "This is the text that Cortana will say.";
```

<span data-ttu-id="cb40c-123">Si desea más control sobre el timbre, el tono y énfasis, dele formato de [lenguaje de marcado de síntesis de voz (SSML)](http://www.w3.org/TR/speech-synthesis/) a la propiedad `Speak`.</span><span class="sxs-lookup"><span data-stu-id="cb40c-123">If you want more control over pitch, tone, and emphasis, format the `Speak` property as [Speech Synthesis Markup Language (SSML)](http://www.w3.org/TR/speech-synthesis/).</span></span>  

<span data-ttu-id="cb40c-124">El ejemplo de código siguiente especifica que la palabra “texto” se debe pronunciar con una cantidad de énfasis moderada:</span><span class="sxs-lookup"><span data-stu-id="cb40c-124">The following code example specifies that the word "text" should be spoken with a moderate amount of emphasis:</span></span>
```cs
Activity reply = activity.CreateReply("This is the text that will be displayed.");
reply.Speak = "<speak version=\"1.0\" xmlns=\"http://www.w3.org/2001/10/synthesis\" xml:lang=\"en-US\">This is the <emphasis level=\"moderate\">text</emphasis> that will be spoken.</speak>";
```


<span data-ttu-id="cb40c-125">La propiedad **InputHint** ayuda a indicar a Cortana si su bot está esperando una entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-125">The **InputHint** property helps indicate to Cortana whether your bot is expecting input.</span></span> <span data-ttu-id="cb40c-126">El valor predeterminado es **ExpectingInput** para un mensaje y **AcceptingInput** para otros tipos de respuestas.</span><span class="sxs-lookup"><span data-stu-id="cb40c-126">The default value is **ExpectingInput** for a prompt, and **AcceptingInput** for other types of responses.</span></span>


| <span data-ttu-id="cb40c-127">Valor</span><span class="sxs-lookup"><span data-stu-id="cb40c-127">Value</span></span> | <span data-ttu-id="cb40c-128">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="cb40c-128">Description</span></span> |
|------|------|
| <span data-ttu-id="cb40c-129">**AcceptingInput**</span><span class="sxs-lookup"><span data-stu-id="cb40c-129">**AcceptingInput**</span></span> | <span data-ttu-id="cb40c-130">El bot está listo para recibir información de forma pasiva, pero no está esperando una respuesta.</span><span class="sxs-lookup"><span data-stu-id="cb40c-130">Your bot is passively ready for input but is not waiting on a response.</span></span> <span data-ttu-id="cb40c-131">Cortana acepta la entrada del usuario si el usuario mantiene presionado el botón del micrófono.</span><span class="sxs-lookup"><span data-stu-id="cb40c-131">Cortana accepts input from the user if the user holds down the microphone button.</span></span>|
| <span data-ttu-id="cb40c-132">**ExpectingInput**</span><span class="sxs-lookup"><span data-stu-id="cb40c-132">**ExpectingInput**</span></span> | <span data-ttu-id="cb40c-133">Indica que el bot está esperando una respuesta del usuario de forma activa.</span><span class="sxs-lookup"><span data-stu-id="cb40c-133">Indicates that the bot is actively expecting a response from the user.</span></span> <span data-ttu-id="cb40c-134">Cortana escucha hasta que el usuario habla por el micrófono.</span><span class="sxs-lookup"><span data-stu-id="cb40c-134">Cortana listens for the user to speak into the microphone.</span></span>  |
| <span data-ttu-id="cb40c-135">**IgnoringInput**</span><span class="sxs-lookup"><span data-stu-id="cb40c-135">**IgnoringInput**</span></span> | <span data-ttu-id="cb40c-136">Cortana está ignorando las entradas.</span><span class="sxs-lookup"><span data-stu-id="cb40c-136">Cortana is ignoring input.</span></span> <span data-ttu-id="cb40c-137">Su bot puede enviar esta sugerencia si se está procesando activamente una solicitud e ignorará la entrada de los usuarios hasta que se complete la solicitud.</span><span class="sxs-lookup"><span data-stu-id="cb40c-137">Your bot may send this hint if it is actively processing a request and will ignore input from users until the request is complete.</span></span>  |

<!-- TODO: tip about time limit and batching -->

<span data-ttu-id="cb40c-138">En este ejemplo se muestra cómo puede informar a Cortana que se espera la intervención del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-138">This example shows how to let Cortana know that user input is expected.</span></span> <span data-ttu-id="cb40c-139">El micrófono quedará encendido.</span><span class="sxs-lookup"><span data-stu-id="cb40c-139">The microphone will be left open.</span></span>
```cs
// Add an InputHint to let Cortana know to expect user input
Activity reply = activity.CreateReply("This is the text that will be displayed."); 
reply.Speak = "This is the text that will be spoken.";
reply.InputHint = InputHints.ExpectingInput;
```



## <a name="display-cards-in-cortana"></a><span data-ttu-id="cb40c-140">Visualización de tarjetas en Cortana</span><span class="sxs-lookup"><span data-stu-id="cb40c-140">Display cards in Cortana</span></span>

<span data-ttu-id="cb40c-141">Además de las respuestas de voz, Cortana puede mostrar datos adjuntos de tarjeta.</span><span class="sxs-lookup"><span data-stu-id="cb40c-141">In addition to spoken responses, Cortana can also display card attachments.</span></span> <span data-ttu-id="cb40c-142">Cortana es compatible con las siguientes tarjetas enriquecidas:</span><span class="sxs-lookup"><span data-stu-id="cb40c-142">Cortana supports the following rich cards:</span></span>

| <span data-ttu-id="cb40c-143">Tipo de tarjeta</span><span class="sxs-lookup"><span data-stu-id="cb40c-143">Card type</span></span> | <span data-ttu-id="cb40c-144">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="cb40c-144">Description</span></span> |
|----|----|
| <span data-ttu-id="cb40c-145">[HeroCard][heroCard]</span><span class="sxs-lookup"><span data-stu-id="cb40c-145">[HeroCard][heroCard]</span></span> | <span data-ttu-id="cb40c-146">Una tarjeta que normalmente contiene una sola imagen grande, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="cb40c-146">A card that typically contains a single large image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="cb40c-147">[ThumbnailCard][thumbnailCard]</span><span class="sxs-lookup"><span data-stu-id="cb40c-147">[ThumbnailCard][thumbnailCard]</span></span> | <span data-ttu-id="cb40c-148">Una tarjeta que normalmente contiene una sola imagen miniatura, uno o varios botones y texto.</span><span class="sxs-lookup"><span data-stu-id="cb40c-148">A card that typically contains a single thumbnail image, one or more buttons, and text.</span></span> |
| <span data-ttu-id="cb40c-149">[ReceiptCard][receiptCard]</span><span class="sxs-lookup"><span data-stu-id="cb40c-149">[ReceiptCard][receiptCard]</span></span> | <span data-ttu-id="cb40c-150">Una tarjeta que permite que un bot proporcione un recibo al usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-150">A card that enables a bot to provide a receipt to the user.</span></span> <span data-ttu-id="cb40c-151">Normalmente, contiene la lista de elementos que se incluyen en el recibo, la información de impuestos y del total y texto adicional.</span><span class="sxs-lookup"><span data-stu-id="cb40c-151">It typically contains the list of items to include on the receipt, tax and total information, and other text.</span></span> |
| <span data-ttu-id="cb40c-152">[SignInCard][signinCard]</span><span class="sxs-lookup"><span data-stu-id="cb40c-152">[SignInCard][signinCard]</span></span> | <span data-ttu-id="cb40c-153">Una tarjeta que permite al bot solicitar el inicio de sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-153">A card that enables a bot to request that a user sign-in.</span></span> <span data-ttu-id="cb40c-154">Normalmente contiene texto y uno o más botones en los cuales el usuario puede hacer clic para comenzar el proceso de inicio de sesión.</span><span class="sxs-lookup"><span data-stu-id="cb40c-154">It typically contains text and one or more buttons that the user can click to initiate the sign-in process.</span></span> |


<span data-ttu-id="cb40c-155">Consulte [Card design best practices][CardDesign] (Prácticas recomendadas para el diseño de tarjetas) para ver el diseño de estas tarjetas en Cortana.</span><span class="sxs-lookup"><span data-stu-id="cb40c-155">See [Card design best practices][CardDesign] to see what these cards look like inside Cortana.</span></span> <span data-ttu-id="cb40c-156">Para obtener un ejemplo de cómo usar una tarjeta enriquecida en un bot, consulte [Add rich card attachments to messages](bot-builder-dotnet-add-rich-card-attachments.md) (Adición de tarjetas enriquecidas a mensajes).</span><span class="sxs-lookup"><span data-stu-id="cb40c-156">For an example of how to use a rich card in a bot, see [Add rich card attachments to messages](bot-builder-dotnet-add-rich-card-attachments.md).</span></span> 

<!--
The following code demonstrates how to add the `Speak` and `InputHint` properties to a message containing a `HeroCard`.
-->


## <a name="sample-rollerskill"></a><span data-ttu-id="cb40c-157">Ejemplo: RollerSkill</span><span class="sxs-lookup"><span data-stu-id="cb40c-157">Sample: RollerSkill</span></span>
<span data-ttu-id="cb40c-158">El código en las secciones siguientes procede de una habilidad de Cortana de ejemplo para lanzar los dados.</span><span class="sxs-lookup"><span data-stu-id="cb40c-158">The code in the following sections comes from a sample Cortana skill for rolling dice.</span></span> <span data-ttu-id="cb40c-159">Descargue el código completo para el bot desde el [repositorio de ejemplos de Bot Builder](https://github.com/Microsoft/BotBuilder-Samples/).</span><span class="sxs-lookup"><span data-stu-id="cb40c-159">Download the full code for the bot from the [BotBuilder-Samples repository](https://github.com/Microsoft/BotBuilder-Samples/).</span></span>

<span data-ttu-id="cb40c-160">La habilidad se invoca al decir el [nombre de invocación][InvocationNameGuidelines] a Cortana.</span><span class="sxs-lookup"><span data-stu-id="cb40c-160">You invoke the skill by saying its [invocation name][InvocationNameGuidelines] to Cortana.</span></span> <span data-ttu-id="cb40c-161">Para la habilidad Roller, después de que [agregue el bot al canal de Cortana][CortanaChannel] y lo registre como una habilidad de Cortana, puede invocarlo si dice a Cortana “Pedir a Roller” o “Pedir a Roller que lance los dados”.</span><span class="sxs-lookup"><span data-stu-id="cb40c-161">For the roller skill, after you [add the bot to the Cortana channel][CortanaChannel] and register it as a Cortana skill, you can invoke it by telling Cortana "Ask Roller" or "Ask Roller to roll dice".</span></span>

### <a name="explore-the-code"></a><span data-ttu-id="cb40c-162">Exploración del código</span><span class="sxs-lookup"><span data-stu-id="cb40c-162">Explore the code</span></span>



<span data-ttu-id="cb40c-163">Para invocar los diálogos correspondientes, los controladores de actividad definidos en `RootDispatchDialog.cs` usan expresiones regulares para establecer coincidencias con la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="cb40c-163">To invoke the appropriate dialogs, the activity handlers defined in `RootDispatchDialog.cs` use regular expressions to match the user's input.</span></span> <span data-ttu-id="cb40c-164">Por ejemplo, el controlador en el ejemplo siguiente se desencadena si el usuario dice algo como "Me gustaría lanzar los dados".</span><span class="sxs-lookup"><span data-stu-id="cb40c-164">For example, the handler in the following example is triggered if the user says something like "I'd like to roll some dice".</span></span> <span data-ttu-id="cb40c-165">Los sinónimos se incluyen en la expresión regular para que las expresiones parecidas desencadenen el diálogo.</span><span class="sxs-lookup"><span data-stu-id="cb40c-165">Synonyms are included in the regular expression so that similar utterances will trigger the dialog.</span></span>
```cs
        [RegexPattern("(roll|role|throw|shoot).*(dice|die|dye|bones)")]
        [RegexPattern("new game")]
        [ScorableGroup(1)]
        public async Task NewGame(IDialogContext context, IActivity activity)
        {
            context.Call(new CreateGameDialog(), AfterGameCreated);
        }
```

<span data-ttu-id="cb40c-166">El diálogo `CreateGameDialog` configura un juego personalizado para que el bot lo reproduzca.</span><span class="sxs-lookup"><span data-stu-id="cb40c-166">The `CreateGameDialog` dialog sets up a custom game for the bot to play.</span></span> <span data-ttu-id="cb40c-167">Usa `PromptDialog` para preguntar al usuario cuántos lados quiere que tengan los dados y cuántos quiere lanzar.</span><span class="sxs-lookup"><span data-stu-id="cb40c-167">It uses a `PromptDialog` to ask the user how many sides they want the dice to have and then how many should be rolled.</span></span> <span data-ttu-id="cb40c-168">Tenga en cuenta que el objeto `PromptOptions` que se usa para inicializar el mensaje contiene una propiedad `speak` para la versión oral del mensaje.</span><span class="sxs-lookup"><span data-stu-id="cb40c-168">Note that the `PromptOptions` object that is used to initialize the prompt contains a `speak` property for the spoken version of the prompt.</span></span>

```cs
    [Serializable]
    public class CreateGameDialog : IDialog<GameData>
    {
        public async Task StartAsync(IDialogContext context)
        {
            context.UserData.SetValue<GameData>(Utils.GameDataKey, new GameData());

            var descriptions = new List<string>() { "4 Sides", "6 Sides", "8 Sides", "10 Sides", "12 Sides", "20 Sides" };
            var choices = new Dictionary<string, IReadOnlyList<string>>()
             {
                { "4", new List<string> { "four", "for", "4 sided", "4 sides" } },
                { "6", new List<string> { "six", "sex", "6 sided", "6 sides" } },
                { "8", new List<string> { "eight", "8 sided", "8 sides" } },
                { "10", new List<string> { "ten", "10 sided", "10 sides" } },
                { "12", new List<string> { "twelve", "12 sided", "12 sides" } },
                { "20", new List<string> { "twenty", "20 sided", "20 sides" } }
            };

            var promptOptions = new PromptOptions<string>(
                Resources.ChooseSides,
                choices: choices,
                descriptions: descriptions,
                speak: SSMLHelper.Speak(Utils.RandomPick(Resources.ChooseSidesSSML))); // spoken prompt

            PromptDialog.Choice(context, this.DiceChoiceReceivedAsync, promptOptions);
        }

        private async Task DiceChoiceReceivedAsync(IDialogContext context, IAwaitable<string> result)
        {
            GameData game;
            if (context.UserData.TryGetValue<GameData>(Utils.GameDataKey, out game))
            {
                int sides;
                if (int.TryParse(await result, out sides))
                {
                    game.Sides = sides;
                    context.UserData.SetValue<GameData>(Utils.GameDataKey, game);
                }

                var promptText = string.Format(Resources.ChooseCount, sides);

                var promptOption = new PromptOptions<long>(promptText, choices: null, speak: SSMLHelper.Speak(Utils.RandomPick(Resources.ChooseCountSSML)));

                var prompt = new PromptDialog.PromptInt64(promptOption);
                context.Call<long>(prompt, this.DiceNumberReceivedAsync);
            }
        }

        private async Task DiceNumberReceivedAsync(IDialogContext context, IAwaitable<long> result)
        {
            GameData game;
            if (context.UserData.TryGetValue<GameData>(Utils.GameDataKey, out game))
            {
                game.Count = await result;
                context.UserData.SetValue<GameData>(Utils.GameDataKey, game);
            }

            context.Done(game);
        }
    }
```

<span data-ttu-id="cb40c-169">`PlayGameDialog` representa los resultados, los muestra en `HeroCard` y crea un mensaje de voz para decirlo con el método `Speak`.</span><span class="sxs-lookup"><span data-stu-id="cb40c-169">The `PlayGameDialog` renders the results both by displaying them in a `HeroCard` and building a spoken message to say using the `Speak` method.</span></span>

```cs
   [Serializable]
    public class PlayGameDialog : IDialog<object>
    {
        private const string RollAgainOptionValue = "roll again";

        private const string NewGameOptionValue = "new game";

        private GameData gameData;

        public PlayGameDialog(GameData gameData)
        {
            this.gameData = gameData;
        }

        public async Task StartAsync(IDialogContext context)
        {
            if (this.gameData == null)
            {
                if (!context.UserData.TryGetValue<GameData>(Utils.GameDataKey, out this.gameData))
                {
                    // User started session with "roll again" so let's just send them to
                    // the 'CreateGameDialog'
                    context.Done<object>(null);
                }
            }

            int total = 0;
            var randomGenerator = new Random();
            var rolls = new List<int>();

            // Generate Rolls
            for (int i = 0; i < this.gameData.Count; i++)
            {
                var roll = randomGenerator.Next(1, this.gameData.Sides);
                total += roll;
                rolls.Add(roll);
            }

            // Format rolls results
            var result = string.Join(" . ", rolls.ToArray());
            bool multiLine = rolls.Count > 5;

            var card = new HeroCard()
            {
                Subtitle = string.Format(
                    this.gameData.Count > 1 ? Resources.CardSubtitlePlural : Resources.CardSubtitleSingular,
                    this.gameData.Count,
                    this.gameData.Sides),
                Buttons = new List<CardAction>()
                {
                    new CardAction(ActionTypes.ImBack, "Roll Again", value: RollAgainOptionValue),
                    new CardAction(ActionTypes.ImBack, "New Game", value: NewGameOptionValue)
                }
            };

            if (multiLine)
            {
                card.Text = result;
            }
            else
            {
                card.Title = result;
            }

            var message = context.MakeMessage();
            message.Attachments = new List<Attachment>()
            {
                card.ToAttachment()
            };

            // Determine bots reaction for speech purposes
            string reaction = "normal";

            var min = this.gameData.Count;
            var max = this.gameData.Count * this.gameData.Sides;
            var score = total / max;
            if (score == 1)
            {
                reaction = "Best";
            }
            else if (score == 0)
            {
                reaction = "Worst";
            }
            else if (score <= 0.3)
            {
                reaction = "Bad";
            }
            else if (score >= 0.8)
            {
                reaction = "Good";
            }

            // Check for special craps rolls
            if (this.gameData.Type == "Craps")
            {
                switch (total)
                {
                    case 2:
                    case 3:
                    case 12:
                        reaction = "CrapsLose";
                        break;
                    case 7:
                        reaction = "CrapsSeven";
                        break;
                    case 11:
                        reaction = "CrapsEleven";
                        break;
                    default:
                        reaction = "CrapsRetry";
                        break;
                }
            }

            // Build up spoken response
            var spoken = string.Empty;
            if (this.gameData.Turns == 0)
            {
                spoken += Utils.RandomPick(Resources.ResourceManager.GetString($"Start{this.gameData.Type}GameSSML"));
            }

            spoken += Utils.RandomPick(Resources.ResourceManager.GetString($"{reaction}RollReactionSSML"));

            message.Speak = SSMLHelper.Speak(spoken);

            // Increment number of turns and store game to roll again
            this.gameData.Turns++;
            context.UserData.SetValue<GameData>(Utils.GameDataKey, this.gameData);

            // Send card and bots reaction to user.
            message.InputHint = InputHints.AcceptingInput;
            await context.PostAsync(message);

            context.Done<object>(null);
        }
    }
```
## <a name="next-steps"></a><span data-ttu-id="cb40c-170">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="cb40c-170">Next steps</span></span>

<span data-ttu-id="cb40c-171">Si su bot se ejecuta localmente o se implementa en la nube, puede invocarlo desde Cortana.</span><span class="sxs-lookup"><span data-stu-id="cb40c-171">If your bot is running locally or deployed in the cloud, you can invoke it from Cortana.</span></span> <span data-ttu-id="cb40c-172">Consulte [Test a Cortana skill](../bot-service-debug-cortana-skill.md) (Prueba de una habilidad de Cortana) para conocer los pasos necesarios para probar su habilidad de Cortana.</span><span class="sxs-lookup"><span data-stu-id="cb40c-172">See [Test a Cortana skill](../bot-service-debug-cortana-skill.md) for the steps required to try out your Cortana skill.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cb40c-173">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cb40c-173">Additional resources</span></span>
* <span data-ttu-id="cb40c-174">[The Cortana Skills Kit][CortanaGetStarted] (El kit de habilidades de Cortana)</span><span class="sxs-lookup"><span data-stu-id="cb40c-174">[The Cortana Skills Kit][CortanaGetStarted]</span></span>
* <span data-ttu-id="cb40c-175">[Add speech to messages](bot-builder-dotnet-text-to-speech.md) (Incorporación de voz a los mensajes)</span><span class="sxs-lookup"><span data-stu-id="cb40c-175">[Add speech to messages](bot-builder-dotnet-text-to-speech.md)</span></span>
* <span data-ttu-id="cb40c-176">[SSML Reference][SSMLRef] (Referencias de SSML)</span><span class="sxs-lookup"><span data-stu-id="cb40c-176">[SSML Reference][SSMLRef]</span></span>
* <span data-ttu-id="cb40c-177">[Prácticas recomendadas para diseño de voz de Cortana][VoiceDesign]</span><span class="sxs-lookup"><span data-stu-id="cb40c-177">[Voice design best practices for Cortana][VoiceDesign]</span></span>
* <span data-ttu-id="cb40c-178">[Prácticas recomendadas para diseño de tarjetas de Cortana][CardDesign]</span><span class="sxs-lookup"><span data-stu-id="cb40c-178">[Card design best practices for Cortana][CardDesign]</span></span>
* <span data-ttu-id="cb40c-179">[Centro de desarrollo de Cortana][CortanaDevCenter]</span><span class="sxs-lookup"><span data-stu-id="cb40c-179">[Cortana Dev Center][CortanaDevCenter]</span></span>
* <span data-ttu-id="cb40c-180">[Prácticas recomendadas para probar y depurar a Cortana][Cortana-TestBestPractice]</span><span class="sxs-lookup"><span data-stu-id="cb40c-180">[Testing and debugging best practices for Cortana][Cortana-TestBestPractice]</span></span>
* <span data-ttu-id="cb40c-181"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a> (Referencias del SDK de Bot Builder para .NET)</span><span class="sxs-lookup"><span data-stu-id="cb40c-181"><a href="/dotnet/api/?view=botbuilder-3.11.0" target="_blank">Bot Builder SDK for .NET Reference</a></span></span>

[CortanaGetStarted]: /cortana/getstarted
[BFPortal]: https://dev.botframework.com/

[SSMLRef]: https://aka.ms/cortana-ssml
[CortanaDevCenter]: https://developer.microsoft.com/en-us/cortana

[CortanaSpecificEntities]: https://aka.ms/lgvcto
[CortanaAuth]: https://aka.ms/vsdqcj

[InvocationNameGuidelines]: https://aka.ms/cortana-invocation-guidelines  
[VoiceDesign]: https://aka.ms/cortana-design-voice
[CardDesign]: https://aka.ms/cortana-design-card
[Cortana-Debug]: https://aka.ms/cortana-enable-debug
[Cortana-Publish]: https://aka.ms/cortana-publish


[CortanaTry]: https://aka.ms/try-cortana-bot
[CortanaChannel]: https://aka.ms/bot-cortana-channel
[Cortana-TestBestPractice]: https://aka.ms/cortana-test-best-practice

[heroCard]: /dotnet/api/microsoft.bot.connector.herocard

[thumbnailCard]: /dotnet/api/microsoft.bot.connector.thumbnailcard 

[receiptCard]: /dotnet/api/microsoft.bot.connector.receiptcard 

[signinCard]: /dotnet/api/microsoft.bot.connector.signincard 


