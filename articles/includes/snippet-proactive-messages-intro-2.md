<span data-ttu-id="29fb8-101">Un **mensaje proactivo ad hoc** es el tipo más simple de mensaje proactivo.</span><span class="sxs-lookup"><span data-stu-id="29fb8-101">An **ad hoc proactive message** is the simplest type of proactive message.</span></span>
<span data-ttu-id="29fb8-102">El bot simplemente interpone el mensaje en la conversación cada vez que se desencadena, sin tener en cuenta si el usuario está implicado actualmente en otro tema de conversación con el bot, y no intentará cambiar la conversación de ninguna manera.</span><span class="sxs-lookup"><span data-stu-id="29fb8-102">The bot simply interjects the message into the conversation whenever it is triggered, without any regard for whether the user is currently engaged in a separate topic of conversation with the bot and will not attempt to change the conversation in any way.</span></span>

<span data-ttu-id="29fb8-103">Para controlar las notificaciones más fácilmente, considere otras formas de integrar la notificación en el flujo de conversación, como establecer una marca en el estado de la conversación o agregar la notificación a una cola.</span><span class="sxs-lookup"><span data-stu-id="29fb8-103">To handle notifications more smoothly, consider other ways to integrate the notification into the conversation flow, such as setting a flag in the conversation state or adding the notification to a queue.</span></span>

<!--Snip
A **dialog-based proactive message** is more complex than an ad hoc proactive message. 
Before it can inject this type of proactive message into the conversation, 
the bot must identify the context of the existing conversation and decide how (or if)
it will resume that conversation after the message interrupts. 

For example, consider a bot that needs to initiate a survey at a given point in time. 
When that time arrives, the bot stops the existing conversation with the user and 
redirects the user to a `SurveyDialog`. 
The `SurveyDialog` is added to the top of the dialog stack and takes control of the conversation. 
When the user finishes all required tasks at the `SurveyDialog`, the `SurveyDialog` closes,
 returning control to the previous dialog, where the user can continue with the prior topic of conversation.

A dialog-based proactive message is more than just simple notification. 
In sending the notification, the bot changes the topic of the existing conversation. 
It then must decide whether to resume that conversation later, or to abandon that conversation altogether by resetting the dialog stack. 
/Snip-->