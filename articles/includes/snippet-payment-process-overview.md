## <a name="payment-process-overview"></a><span data-ttu-id="28ba1-101">Información general del proceso de pago</span><span class="sxs-lookup"><span data-stu-id="28ba1-101">Payment process overview</span></span>

<span data-ttu-id="28ba1-102">El proceso de pago consta de tres partes distintas:</span><span class="sxs-lookup"><span data-stu-id="28ba1-102">The payment process comprises three distinct parts:</span></span>

1. <span data-ttu-id="28ba1-103">El bot envía una solicitud de pago.</span><span class="sxs-lookup"><span data-stu-id="28ba1-103">The bot sends a payment request.</span></span>

2. <span data-ttu-id="28ba1-104">El usuario inicia sesión con una cuenta de Microsoft para proporcionar la información de pago, de envío y de contacto.</span><span class="sxs-lookup"><span data-stu-id="28ba1-104">The user signs in with a Microsoft account to provide payment, shipping, and contact information.</span></span> <span data-ttu-id="28ba1-105">Las devoluciones de llamada se envían al bot para indicar en qué momento el bot debe realizar determinadas operaciones (actualizar la dirección de envío, actualizar la opción de envío, realizar el pago).</span><span class="sxs-lookup"><span data-stu-id="28ba1-105">Callbacks are sent to the bot to indicate when the bot needs to perform certain operations (update shipping address, update shipping option, complete payment).</span></span>

3. <span data-ttu-id="28ba1-106">El bot procesa las devoluciones de llamada que recibe, incluida la actualización de la dirección de envío, la actualización de la opción de envío y la realización del pago.</span><span class="sxs-lookup"><span data-stu-id="28ba1-106">The bot processes the callbacks that it receives, including shipping address update, shipping option update, and payment complete.</span></span> 

<span data-ttu-id="28ba1-107">El bot debe implementar solo el primer y el tercer paso de este proceso; el paso dos se lleva a cabo fuera del contexto del bot.</span><span class="sxs-lookup"><span data-stu-id="28ba1-107">Your bot must implement only step one and step three of this process; step two takes place outside the context of your bot.</span></span> 
