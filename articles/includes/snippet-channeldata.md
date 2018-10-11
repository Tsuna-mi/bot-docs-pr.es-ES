# <a name="implement-channel-specific-functionality"></a><span data-ttu-id="daa7a-101">Implementar una funcionalidad específica de canal</span><span class="sxs-lookup"><span data-stu-id="daa7a-101">Implement channel-specific functionality</span></span>

<span data-ttu-id="daa7a-102">Algunos canales proporcionan características que no se pueden implementar usando únicamente el texto y los datos adjuntos del mensaje.</span><span class="sxs-lookup"><span data-stu-id="daa7a-102">Some channels provide features that cannot be implemented by using only message text and attachments.</span></span> <span data-ttu-id="daa7a-103">Para implementar una funcionalidad específica de canal, puede pasar los metadatos nativos a un canal en la propiedad _channel data_ del objeto activity.</span><span class="sxs-lookup"><span data-stu-id="daa7a-103">To implement channel-specific functionality, you can pass native metadata to a channel in the activity object's _channel data_ property.</span></span> <span data-ttu-id="daa7a-104">Por ejemplo, el bot puede usar la propiedad channel data para indicar a Telegram que envíe un adhesivo o para indicar a Office 365 que envíe un correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="daa7a-104">For example, your bot can use the channel data property to instruct Telegram to send a sticker or to instruct Office365 to send an email.</span></span>

<span data-ttu-id="daa7a-105">En este artículo se describe cómo usar la propiedad channel data de la actividad de un mensaje para implementar esta funcionalidad específica de canal:</span><span class="sxs-lookup"><span data-stu-id="daa7a-105">This article describes how to use a message activity's channel data property to implement this channel-specific functionality:</span></span>

| <span data-ttu-id="daa7a-106">Canal</span><span class="sxs-lookup"><span data-stu-id="daa7a-106">Channel</span></span> | <span data-ttu-id="daa7a-107">Funcionalidad</span><span class="sxs-lookup"><span data-stu-id="daa7a-107">Functionality</span></span> |
|----|----|
| <span data-ttu-id="daa7a-108">Email</span><span class="sxs-lookup"><span data-stu-id="daa7a-108">Email</span></span> | <span data-ttu-id="daa7a-109">Envío y recepción de un correo electrónico que contiene metadatos de importancia, cuerpo y asunto.</span><span class="sxs-lookup"><span data-stu-id="daa7a-109">Send and receive an email that contains body, subject, and importance metadata</span></span> |
| <span data-ttu-id="daa7a-110">Slack</span><span class="sxs-lookup"><span data-stu-id="daa7a-110">Slack</span></span> | <span data-ttu-id="daa7a-111">Envío de mensajes de Slack de plena fidelidad.</span><span class="sxs-lookup"><span data-stu-id="daa7a-111">Send full fidelity Slack messages</span></span> |
| <span data-ttu-id="daa7a-112">Facebook</span><span class="sxs-lookup"><span data-stu-id="daa7a-112">Facebook</span></span> | <span data-ttu-id="daa7a-113">Envío de notificaciones de Facebook de forma nativa.</span><span class="sxs-lookup"><span data-stu-id="daa7a-113">Send Facebook notifications natively</span></span> |
| <span data-ttu-id="daa7a-114">Telegram</span><span class="sxs-lookup"><span data-stu-id="daa7a-114">Telegram</span></span> | <span data-ttu-id="daa7a-115">Acciones específicas de Telegram, como compartir una nota de voz o un adhesivo.</span><span class="sxs-lookup"><span data-stu-id="daa7a-115">Perform Telegram-specific actions, such as sharing a voice memo or a sticker</span></span> |
| <span data-ttu-id="daa7a-116">Kik</span><span class="sxs-lookup"><span data-stu-id="daa7a-116">Kik</span></span> | <span data-ttu-id="daa7a-117">Envío y recepción de mensajes nativos de Kik.</span><span class="sxs-lookup"><span data-stu-id="daa7a-117">Send and receive native Kik messages</span></span> |

> [!NOTE]
> <span data-ttu-id="daa7a-118">El valor de la propiedad channel data de un objeto activity es un objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="daa7a-118">The value of an activity object's channel data property is a JSON object.</span></span>
> <span data-ttu-id="daa7a-119">Por lo tanto, los ejemplos de este artículo muestran el formato esperado de la propiedad JSON `channelData` en distintos escenarios.</span><span class="sxs-lookup"><span data-stu-id="daa7a-119">Therefore, the examples in this article show the expected format of the `channelData` JSON property in various scenarios.</span></span>
> <span data-ttu-id="daa7a-120">Para crear un objeto JSON con. NET, use la clase `JObject` (.NET).</span><span class="sxs-lookup"><span data-stu-id="daa7a-120">To create a JSON object using .NET, use the `JObject` (.NET) class.</span></span>

## <a name="create-a-custom-email-message"></a><span data-ttu-id="daa7a-121">Crear un mensaje de correo electrónico personalizado</span><span class="sxs-lookup"><span data-stu-id="daa7a-121">Create a custom Email message</span></span>

<span data-ttu-id="daa7a-122">Para crear un mensaje de correo electrónico, establezca la propiedad channel data del objeto activity en un objeto JSON que contenga estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="daa7a-122">To create an email message, set the activity object's channel data property to a JSON object that contains these properties:</span></span>

| <span data-ttu-id="daa7a-123">Propiedad</span><span class="sxs-lookup"><span data-stu-id="daa7a-123">Property</span></span> | <span data-ttu-id="daa7a-124">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="daa7a-124">Description</span></span> |
|----|----|
| <span data-ttu-id="daa7a-125">bccRecipients</span><span class="sxs-lookup"><span data-stu-id="daa7a-125">bccRecipients</span></span> | <span data-ttu-id="daa7a-126">Una cadena de direcciones de correo electrónico delimitada por punto y coma (;) para agregar al campo CCO (copia carbón oculta) del mensaje.</span><span class="sxs-lookup"><span data-stu-id="daa7a-126">A semicolon (;) delimited string of email addresses to add to the message's Bcc (blind carbon copy) field.</span></span> |
| <span data-ttu-id="daa7a-127">ccRecipients</span><span class="sxs-lookup"><span data-stu-id="daa7a-127">ccRecipients</span></span> | <span data-ttu-id="daa7a-128">Una cadena de direcciones de correo electrónico delimitada por punto y coma (;) para agregar al campo CC (copia carbón) del mensaje.</span><span class="sxs-lookup"><span data-stu-id="daa7a-128">A semicolon (;) delimited string of email addresses to add to the message's Cc (carbon copy) field.</span></span> |
| <span data-ttu-id="daa7a-129">htmlBody</span><span class="sxs-lookup"><span data-stu-id="daa7a-129">htmlBody</span></span> | <span data-ttu-id="daa7a-130">Un documento HTML que especifique el cuerpo del mensaje de correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="daa7a-130">An HTML document that specifies the body of the email message.</span></span> <span data-ttu-id="daa7a-131">Consulte la documentación del canal para obtener información acerca de los atributos y elementos HTML compatibles.</span><span class="sxs-lookup"><span data-stu-id="daa7a-131">See the channel's documentation for information about supported HTML elements and attributes.</span></span> |
| <span data-ttu-id="daa7a-132">importance</span><span class="sxs-lookup"><span data-stu-id="daa7a-132">importance</span></span> | <span data-ttu-id="daa7a-133">Nivel de importancia del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="daa7a-133">The email's importance level.</span></span> <span data-ttu-id="daa7a-134">Los valores válidos son **high**, **normal** y **low**.</span><span class="sxs-lookup"><span data-stu-id="daa7a-134">Valid values are **high**, **normal**, and **low**.</span></span> <span data-ttu-id="daa7a-135">El valor predeterminado es **normal**.</span><span class="sxs-lookup"><span data-stu-id="daa7a-135">The default value is **normal**.</span></span> |
| <span data-ttu-id="daa7a-136">subject</span><span class="sxs-lookup"><span data-stu-id="daa7a-136">subject</span></span> | <span data-ttu-id="daa7a-137">Asunto del correo electrónico.</span><span class="sxs-lookup"><span data-stu-id="daa7a-137">The email's subject.</span></span> <span data-ttu-id="daa7a-138">Consulte la documentación del canal para obtener información acerca de los requisitos del campo.</span><span class="sxs-lookup"><span data-stu-id="daa7a-138">See the channel's documentation for information about field requirements.</span></span> |
| <span data-ttu-id="daa7a-139">toRecipients</span><span class="sxs-lookup"><span data-stu-id="daa7a-139">toRecipients</span></span> | <span data-ttu-id="daa7a-140">Una cadena de direcciones de correo electrónico delimitada por punto y coma (;) para agregar al campo Para del mensaje.</span><span class="sxs-lookup"><span data-stu-id="daa7a-140">A semicolon (;) delimited string of email addresses to add to the message's To field.</span></span> |

> [!NOTE]
> <span data-ttu-id="daa7a-141">Los mensajes que recibe el bot de los usuarios a través del canal de correo electrónico pueden contener una propiedad channel data que se rellena con un objeto JSON como el que se ha descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="daa7a-141">Messages that your bot receives from users via the Email channel may contain a channel data property that is populated with a JSON object like the one described above.</span></span>

<span data-ttu-id="daa7a-142">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de correo electrónico personalizado.</span><span class="sxs-lookup"><span data-stu-id="daa7a-142">This snippet shows an example of the `channelData` property for a custom email message.</span></span>

```json
"channelData": {
    "type": "message",
    "locale": "en-Us",
    "channelID": "email",
    "from": { "id": "mybot@mydomain.com", "name": "My bot"},
    "recipient": { "id": "joe@otherdomain.com", "name": "Joe Doe"},
    "conversation": { "id": "123123123123", "topic": "awesome chat" },
    "channelData":
    {
        "htmlBody": "<html><body style = /"font-family: Calibri; font-size: 11pt;/" >This is more than awesome.</body></html>",
        "subject": "Super awesome message subject",
        "importance": "high",
        "ccRecipients": "Yasemin@adatum.com;Temel@adventure-works.com"
    }
}
```

## <a name="create-a-full-fidelity-slack-message"></a><span data-ttu-id="daa7a-143">Crear un mensaje de Slack de plena fidelidad</span><span class="sxs-lookup"><span data-stu-id="daa7a-143">Create a full-fidelity Slack message</span></span>

<span data-ttu-id="daa7a-144">Para crear un mensaje de Slack de plena fidelidad, establezca la propiedad channel data del objeto activity en un objeto JSON que especifique <a href="https://api.slack.com/docs/messages" target="_blank">mensajes de Slack</a>, <a href="https://api.slack.com/docs/message-attachments" target="_blank">elementos adjuntos de Slack</a> o <a href="https://api.slack.com/docs/message-buttons" target="_blank">botones de Slack</a>.</span><span class="sxs-lookup"><span data-stu-id="daa7a-144">To create a full-fidelity Slack message, set the activity object's channel data property to a JSON object that specifies <a href="https://api.slack.com/docs/messages" target="_blank">Slack messages</a>, <a href="https://api.slack.com/docs/message-attachments" target="_blank">Slack attachments</a>, and/or <a href="https://api.slack.com/docs/message-buttons" target="_blank">Slack buttons</a>.</span></span>

> [!NOTE]
> <span data-ttu-id="daa7a-145">Para admitir botones en los mensajes de Slack, debe habilitar **Mensajes interactivos** cuando [conecte el bot](../bot-service-manage-channels.md) al canal de Slack.</span><span class="sxs-lookup"><span data-stu-id="daa7a-145">To support buttons in Slack messages, you must enable **Interactive Messages** when you [connect your bot](../bot-service-manage-channels.md) to the Slack channel.</span></span>

<span data-ttu-id="daa7a-146">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de Slack personalizado.</span><span class="sxs-lookup"><span data-stu-id="daa7a-146">This snippet shows an example of the `channelData` property for a custom Slack message.</span></span>

```json
"channelData": {
   "text": "Now back in stock! :tada:",
   "attachments": [
        {
            "title": "The Further Adventures of Slackbot",
            "author_name": "Stanford S. Strickland",
            "author_icon": "https://api.slack.com/img/api/homepage_custom_integrations-2x.png",
            "image_url": "http://i.imgur.com/OJkaVOI.jpg?1"
        },
        {
            "fields": [
                {
                    "title": "Volume",
                    "value": "1",
                    "short": true
                },
                {
                    "title": "Issue",
                    "value": "3",
                    "short": true
                }
            ]
        },
        {
            "title": "Synopsis",
            "text": "After @episod pushed exciting changes to a devious new branch back in Issue 1, Slackbot notifies @don about an unexpected deploy..."
        },
        {
            "fallback": "Would you recommend it to customers?",
            "title": "Would you recommend it to customers?",
            "callback_id": "comic_1234_xyz",
            "color": "#3AA3E3",
            "attachment_type": "default",
            "actions": [
                {
                    "name": "recommend",
                    "text": "Recommend",
                    "type": "button",
                    "value": "recommend"
                },
                {
                    "name": "no",
                    "text": "No",
                    "type": "button",
                    "value": "bad"
                }
            ]
        }
    ]
}
```

<span data-ttu-id="daa7a-147">Cuando un usuario hace clic en un botón dentro de un mensaje de Slack, el bot recibirá un mensaje de respuesta en el que la propiedad channel data se rellena con un objeto JSON `payload`.</span><span class="sxs-lookup"><span data-stu-id="daa7a-147">When a user clicks a button within a Slack message, your bot will receive a response message in which the channel data property is populated with a `payload` JSON object.</span></span>
<span data-ttu-id="daa7a-148">El objeto `payload` especifica el contenido del mensaje original, identifica el botón donde se hizo clic e identifica al usuario que hizo clic en el botón.</span><span class="sxs-lookup"><span data-stu-id="daa7a-148">The `payload` object specifies contents of the original message, identifies the button that was clicked, and identifies the user who clicked the button.</span></span>

<span data-ttu-id="daa7a-149">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` en el mensaje que recibe un bot cuando un usuario hace clic en un botón en el mensaje de Slack.</span><span class="sxs-lookup"><span data-stu-id="daa7a-149">This snippet shows an example of the `channelData` property in the message that a bot receives when a user clicks a button in the Slack message.</span></span>

```json
"channelData": {
    "payload": {
        "actions": [
            {
                "name": "recommend",
                "value": "yes"
            }
        ],
        . . .
        "original_message": "{…}",
        "response_url": "https://hooks.slack.com/actions/..."
    }
}
```

<span data-ttu-id="daa7a-150">El bot puede responder a este mensaje de la forma normal o puede registrar su respuesta directamente en punto de conexión que haya especificado la propiedad `response_url` del objeto `payload`.</span><span class="sxs-lookup"><span data-stu-id="daa7a-150">Your bot can reply to this message in the normal manner, or it can post its response directly to the endpoint that is specified by the `payload` object's `response_url` property.</span></span>
<span data-ttu-id="daa7a-151">Para obtener información sobre cuándo y cómo publicar una respuesta en `response_url`, consulte <a href="https://api.slack.com/docs/message-buttons" target="_blank">Botones de Slack</a>.</span><span class="sxs-lookup"><span data-stu-id="daa7a-151">For information about when and how to post a response to the `response_url`, see <a href="https://api.slack.com/docs/message-buttons" target="_blank">Slack Buttons</a>.</span></span>

<span data-ttu-id="daa7a-152">Puede crear botones dinámicos con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="daa7a-152">You can create dynamic buttons using the following code:</span></span>
```cs
private async Task DemoButtonsAsync(IDialogContext context)
        {
            var reply = context.MakeMessage();

            string s = @"{
                ""text"": ""Would you like to play a game ? "",
                ""attachments"": [
                    {
                        ""text"": ""Choose a game to play!"",
                        ""fallback"": ""You are unable to choose a game"",
                        ""callback_id"": ""wopr_game"",
                        ""color"": ""#3AA3E3"",
                        ""attachment_type"": ""default"",
                        ""actions"": [
                            {
                                ""name"": ""game"",
                                ""text"": ""Chess"",
                                ""type"": ""button"",
                                ""value"": ""chess""
                            },
                            {
                                ""name"": ""game"",
                                ""text"": ""Falken's Maze"",
                                ""type"": ""button"",
                                ""value"": ""maze""
                            },
                            {
                                ""name"": ""game"",
                                ""text"": ""Thermonuclear War"",
                                ""style"": ""danger"",
                                ""type"": ""button"",
                                ""value"": ""war"",
                                ""confirm"": {
                                    ""title"": ""Are you sure?"",
                                    ""text"": ""Wouldn't you prefer a good game of chess?"",
                                    ""ok_text"": ""Yes"",
                                    ""dismiss_text"": ""No""
                                }
                            }
                        ]
                    }
                ]
            }";

            reply.Text = null;
            reply.ChannelData = JObject.Parse(s);
            await context.PostAsync(reply);
            context.Wait(MessageReceivedAsync);
        }
```

<span data-ttu-id="daa7a-153">Para crear menús interactivos, use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="daa7a-153">To create interactive menus, use the following code:</span></span>
```cs
private async Task DemoMenuAsync(IDialogContext context)
        {
            var reply = context.MakeMessage();

            string s = @"{
                ""text"": ""Would you like to play a game ? "",
                ""response_type"": ""in_channel"",
                ""attachments"": [
                    {
                        ""text"": ""Choose a game to play"",
                        ""fallback"": ""If you could read this message, you'd be choosing something fun to do right now."",
                        ""color"": ""#3AA3E3"",
                        ""attachment_type"": ""default"",
                        ""callback_id"": ""game_selection"",
                        ""actions"": [
                            {
                                ""name"": ""games_list"",
                                ""text"": ""Pick a game..."",
                                ""type"": ""select"",
                                ""options"": [
                                    {
                                        ""text"": ""Hearts"",
                                        ""value"": ""menu_id_hearts""
                                    },
                                    {
                                        ""text"": ""Bridge"",
                                        ""value"": ""menu_id_bridge""
                                    },
                                    {
                                        ""text"": ""Checkers"",
                                        ""value"": ""menu_id_checkers""
                                    },
                                    {
                                        ""text"": ""Chess"",
                                        ""value"": ""menu_id_chess""
                                    },
                                    {
                                        ""text"": ""Poker"",
                                        ""value"": ""menu_id_poker""
                                    },
                                    {
                                        ""text"": ""Falken's Maze"",
                                        ""value"": ""menu_id_maze""
                                    },
                                    {
                                        ""text"": ""Global Thermonuclear War"",
                                        ""value"": ""menu_id_war""
                                    }
                                ]
                            }
                        ]
                    }
                ]
            }";

            reply.Text = null;
            reply.ChannelData = JObject.Parse(s);
            await context.PostAsync(reply);
            context.Wait(MessageReceivedAsync);
        }
```

## <a name="create-a-facebook-notification"></a><span data-ttu-id="daa7a-154">Crear una notificación de Facebook</span><span class="sxs-lookup"><span data-stu-id="daa7a-154">Create a Facebook notification</span></span>

<span data-ttu-id="daa7a-155">Para crear una notificación de Facebook, establezca la propiedad channel data del objeto activity en un objeto JSON que especifique estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="daa7a-155">To create a Facebook notification, set the activity object's channel data property to a JSON object that specifies these properties:</span></span>

| <span data-ttu-id="daa7a-156">Propiedad</span><span class="sxs-lookup"><span data-stu-id="daa7a-156">Property</span></span> | <span data-ttu-id="daa7a-157">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="daa7a-157">Description</span></span> |
|----|----|
| <span data-ttu-id="daa7a-158">notification_type</span><span class="sxs-lookup"><span data-stu-id="daa7a-158">notification_type</span></span> | <span data-ttu-id="daa7a-159">El tipo de notificación (por ejemplo, **REGULAR**, **SILENT_PUSH**, **NO_PUSH**).</span><span class="sxs-lookup"><span data-stu-id="daa7a-159">The type of notification (e.g., **REGULAR**, **SILENT_PUSH**, **NO_PUSH**).</span></span>
| <span data-ttu-id="daa7a-160">attachment</span><span class="sxs-lookup"><span data-stu-id="daa7a-160">attachment</span></span> | <span data-ttu-id="daa7a-161">Un elemento adjunto que especifica una imagen, un vídeo u otro tipo de elemento multimedia, o un adjunto con plantilla como, por ejemplo, un recibo.</span><span class="sxs-lookup"><span data-stu-id="daa7a-161">An attachment that specifies an image, video, or other multimedia type, or a templated attachment such as a receipt.</span></span> |

> [!NOTE]
> <span data-ttu-id="daa7a-162">Para obtener más información sobre el formato y el contenido de la propiedad `notification_type` y la propiedad `attachment`, consulte la <a href="https://developers.facebook.com/docs/messenger-platform/send-api-reference#guidelines" target="_blank">documentación de la API de Facebook</a>.</span><span class="sxs-lookup"><span data-stu-id="daa7a-162">For details about format and contents of the `notification_type` property and `attachment` property, see the <a href="https://developers.facebook.com/docs/messenger-platform/send-api-reference#guidelines" target="_blank">Facebook API documentation</a>.</span></span> 

<span data-ttu-id="daa7a-163">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un elemento adjunto con recibo de Facebook.</span><span class="sxs-lookup"><span data-stu-id="daa7a-163">This snippet shows an example of the `channelData` property for a Facebook receipt attachment.</span></span>

```json
"channelData": {
    "notification_type": "NO_PUSH",
    "attachment": {
        "type": "template"
        "payload": {
            "template_type": "receipt",
            . . .
        }
    }
}
```

## <a name="create-a-telegram-message"></a><span data-ttu-id="daa7a-164">Crear un mensaje de Telegram</span><span class="sxs-lookup"><span data-stu-id="daa7a-164">Create a Telegram message</span></span>

<span data-ttu-id="daa7a-165">Para crear un mensaje que implemente las acciones específicas de Telegram, como compartir una nota de voz o un adhesivo, establezca la propiedad sticker del objeto activity en un objeto JSON que especifique estas propiedades:</span><span class="sxs-lookup"><span data-stu-id="daa7a-165">To create a message that implements Telegram-specific actions, such as sharing a voice memo or a sticker, set the activity object's channel data property to a JSON object that specifies these properties:</span></span> 

| <span data-ttu-id="daa7a-166">Propiedad</span><span class="sxs-lookup"><span data-stu-id="daa7a-166">Property</span></span> | <span data-ttu-id="daa7a-167">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="daa7a-167">Description</span></span> |
|----|----|
| <span data-ttu-id="daa7a-168">estático</span><span class="sxs-lookup"><span data-stu-id="daa7a-168">method</span></span> | <span data-ttu-id="daa7a-169">El método de Telegram Bot API al que se llamará.</span><span class="sxs-lookup"><span data-stu-id="daa7a-169">The Telegram Bot API method to call.</span></span> |
| <span data-ttu-id="daa7a-170">parameters</span><span class="sxs-lookup"><span data-stu-id="daa7a-170">parameters</span></span> | <span data-ttu-id="daa7a-171">Los parámetros del método especificado.</span><span class="sxs-lookup"><span data-stu-id="daa7a-171">The parameters of the specified method.</span></span> |

<span data-ttu-id="daa7a-172">Se admiten los métodos de Telegram siguientes:</span><span class="sxs-lookup"><span data-stu-id="daa7a-172">These Telegram methods are supported:</span></span> 

- <span data-ttu-id="daa7a-173">answerInlineQuery</span><span class="sxs-lookup"><span data-stu-id="daa7a-173">answerInlineQuery</span></span>
- <span data-ttu-id="daa7a-174">editMessageCaption</span><span class="sxs-lookup"><span data-stu-id="daa7a-174">editMessageCaption</span></span>
- <span data-ttu-id="daa7a-175">editMessageReplyMarkup</span><span class="sxs-lookup"><span data-stu-id="daa7a-175">editMessageReplyMarkup</span></span>
- <span data-ttu-id="daa7a-176">editMessageText</span><span class="sxs-lookup"><span data-stu-id="daa7a-176">editMessageText</span></span>
- <span data-ttu-id="daa7a-177">forwardMessage</span><span class="sxs-lookup"><span data-stu-id="daa7a-177">forwardMessage</span></span>
- <span data-ttu-id="daa7a-178">kickChatMember</span><span class="sxs-lookup"><span data-stu-id="daa7a-178">kickChatMember</span></span>
- <span data-ttu-id="daa7a-179">sendAudio</span><span class="sxs-lookup"><span data-stu-id="daa7a-179">sendAudio</span></span>
- <span data-ttu-id="daa7a-180">sendChatAction</span><span class="sxs-lookup"><span data-stu-id="daa7a-180">sendChatAction</span></span>
- <span data-ttu-id="daa7a-181">sendContact</span><span class="sxs-lookup"><span data-stu-id="daa7a-181">sendContact</span></span>
- <span data-ttu-id="daa7a-182">sendDocument</span><span class="sxs-lookup"><span data-stu-id="daa7a-182">sendDocument</span></span>
- <span data-ttu-id="daa7a-183">sendLocation</span><span class="sxs-lookup"><span data-stu-id="daa7a-183">sendLocation</span></span>
- <span data-ttu-id="daa7a-184">sendMessage</span><span class="sxs-lookup"><span data-stu-id="daa7a-184">sendMessage</span></span>
- <span data-ttu-id="daa7a-185">sendPhoto</span><span class="sxs-lookup"><span data-stu-id="daa7a-185">sendPhoto</span></span>
- <span data-ttu-id="daa7a-186">sendSticker</span><span class="sxs-lookup"><span data-stu-id="daa7a-186">sendSticker</span></span>
- <span data-ttu-id="daa7a-187">sendVenue</span><span class="sxs-lookup"><span data-stu-id="daa7a-187">sendVenue</span></span>
- <span data-ttu-id="daa7a-188">sendVideo</span><span class="sxs-lookup"><span data-stu-id="daa7a-188">sendVideo</span></span>
- <span data-ttu-id="daa7a-189">sendVoice</span><span class="sxs-lookup"><span data-stu-id="daa7a-189">sendVoice</span></span>
- <span data-ttu-id="daa7a-190">unbanChateMember</span><span class="sxs-lookup"><span data-stu-id="daa7a-190">unbanChateMember</span></span>

<span data-ttu-id="daa7a-191">Para obtener más información sobre estos métodos de Telegram y sus parámetros, consulte la <a href="https://core.telegram.org/bots/api#available-methods" target="_blank">documentación de Telegram Bot API</a>.</span><span class="sxs-lookup"><span data-stu-id="daa7a-191">For details about these Telegram methods and their parameters, see the <a href="https://core.telegram.org/bots/api#available-methods" target="_blank">Telegram Bot API documentation</a>.</span></span>

> [!NOTE]
> <ul><li><span data-ttu-id="daa7a-192">El parámetro <code>chat_id</code> es común a todos los métodos de Telegram.</span><span class="sxs-lookup"><span data-stu-id="daa7a-192">The <code>chat_id</code> parameter is common to all Telegram methods.</span></span> <span data-ttu-id="daa7a-193">Si no especifica <code>chat_id</code> como parámetro, el marco proporcionará el identificador automáticamente.</span><span class="sxs-lookup"><span data-stu-id="daa7a-193">If you do not specify <code>chat_id</code> as a parameter, the framework will provide the ID for you.</span></span></li>
> <li><span data-ttu-id="daa7a-194">En lugar de pasar el contenido del archivo insertado, especifique el archivo mediante una dirección URL y el tipo de medio, tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="daa7a-194">Instead of passing file contents inline, specify the file using a URL and media type as shown in the example below.</span></span></li>
> <li><span data-ttu-id="daa7a-195">Dentro de cada mensaje que recibe su bot del canal de Telegram, la propiedad <code>ChannelData</code> incluirá el mensaje que su bot envió anteriormente.</span><span class="sxs-lookup"><span data-stu-id="daa7a-195">Within each message that your bot receives from the Telegram channel, the <code>ChannelData</code> property will include the message that your bot sent previously.</span></span></li></ul>

<span data-ttu-id="daa7a-196">En este fragmento de código se muestra un ejemplo de una propiedad `channelData` que especifica un único método de Telegram.</span><span class="sxs-lookup"><span data-stu-id="daa7a-196">This snippet shows an example of a `channelData` property that specifies a single Telegram method.</span></span>

```json
"channelData": {
    "method": "sendSticker",
    "parameters": {
        "sticker": {
            "url": "https://domain.com/path/gif",
            "mediaType": "image/gif",
        }
    }
}
```

<span data-ttu-id="daa7a-197">En este fragmento de código se muestra un ejemplo de una propiedad `channelData` que especifica una matriz de métodos de Telegram.</span><span class="sxs-lookup"><span data-stu-id="daa7a-197">This snippet shows an example of a `channelData` property that specifies an array of Telegram methods.</span></span>

```json
"channelData": [
    {
        "method": "sendSticker",
        "parameters": {
            "sticker": {
                "url": "https://domain.com/path/gif",
                "mediaType": "image/gif",
            }
        }
    },
    {
        "method": "sendMessage",
        "parameters": {
            "text": "<b>This message is HTML formatted.</b>",
            "parse_mode": "HTML"
        }
    }
]
```

## <a name="create-a-native-kik-message"></a><span data-ttu-id="daa7a-198">Crear un mensaje de Kik nativo</span><span class="sxs-lookup"><span data-stu-id="daa7a-198">Create a native Kik message</span></span>

<span data-ttu-id="daa7a-199">Para crear un mensaje de Kik nativo, establezca la propiedad sticker del objeto activity en un objeto JSON que especifique esta propiedad:</span><span class="sxs-lookup"><span data-stu-id="daa7a-199">To create a native Kik message, set the activity object's channel data property to a JSON object that specifies this property:</span></span>

| <span data-ttu-id="daa7a-200">Propiedad</span><span class="sxs-lookup"><span data-stu-id="daa7a-200">Property</span></span> | <span data-ttu-id="daa7a-201">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="daa7a-201">Description</span></span> |
|----|----|
| <span data-ttu-id="daa7a-202">messages</span><span class="sxs-lookup"><span data-stu-id="daa7a-202">messages</span></span> | <span data-ttu-id="daa7a-203">Una matriz de mensajes de Kik.</span><span class="sxs-lookup"><span data-stu-id="daa7a-203">An array of Kik messages.</span></span> <span data-ttu-id="daa7a-204">Para obtener más información sobre el formato de los mensajes de Kik, consulte los <a href="https://dev.kik.com/#/docs/messaging#message-formats" target="_blank">formatos de mensaje de Kik</a>.</span><span class="sxs-lookup"><span data-stu-id="daa7a-204">For details about Kik message format, see <a href="https://dev.kik.com/#/docs/messaging#message-formats" target="_blank">Kik Message Formats</a>.</span></span> |

<span data-ttu-id="daa7a-205">En este fragmento de código se muestra un ejemplo de la propiedad `channelData` para un mensaje de Kik nativo.</span><span class="sxs-lookup"><span data-stu-id="daa7a-205">This snippet shows an example of the `channelData` property for a native Kik message.</span></span>

```json
"channelData": {
    "messages": [
        {
            "chatId": "c6dd8165…",
            "type": "link",
            "to": "kikhandle",
            "title": "My Webpage",
            "text": "Some text to display",
            "url": "http://botframework.com",
            "picUrl": "http://lorempixel.com/400/200/",
            "attribution": {
                "name": "My App",
                "iconUrl": "http://lorempixel.com/50/50/"
            },
            "noForward": true,
            "kikJsData": {
                    "key": "value"
                }
        }
    ]
}
```

## <a name="additional-resources"></a><span data-ttu-id="daa7a-206">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="daa7a-206">Additional resources</span></span>

- [<span data-ttu-id="daa7a-207">Entidades y tipos de actividad</span><span class="sxs-lookup"><span data-stu-id="daa7a-207">Entities and activity types</span></span>](../bot-service-activities-entities.md)
- [<span data-ttu-id="daa7a-208">Esquema Activity de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="daa7a-208">Bot Framework Activity schema</span></span>](https://github.com/Microsoft/BotBuilder/blob/hub/specs/botframework-activity/botframework-activity.md)
