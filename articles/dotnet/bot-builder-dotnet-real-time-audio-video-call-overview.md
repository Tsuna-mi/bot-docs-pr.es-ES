---
title: Compilación de un bot de elementos multimedia en tiempo real para Skype | Microsoft Docs
description: Obtenga información sobre cómo compilar un bot que realiza las llamadas de audio y vídeo en tiempo real con Skype, mediante el SDK del generador de bots para .NET y el SDK de RealTimeMediaCalling del generador de bots para .NET.
author: MalarGit
ms.author: malarch
manager: kamrani
ms.topic: article
ms.prod: bot-framework
ms.date: 12/13/17
monikerRange: azure-bot-service-3.0
ms.openlocfilehash: 3e990da2abcb63c695cc79d5d8d9af40d8966cfa
ms.sourcegitcommit: f576981342fb3361216675815714e24281e20ddf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39305468"
---
# <a name="build-a-real-time-media-bot-for-skype"></a><span data-ttu-id="25b8f-103">Compilar de un bot de elementos multimedia en tiempo real para Skype</span><span class="sxs-lookup"><span data-stu-id="25b8f-103">Build a real-time media bot for Skype</span></span>

<span data-ttu-id="25b8f-104">La Plataforma de elementos multimedia en tiempo real para bots es una funcionalidad avanzada que permite que el bot envíe y reciba contenido de voz y vídeo trama a trama y fotograma a fotograma.</span><span class="sxs-lookup"><span data-stu-id="25b8f-104">The Real-Time Media Platform for Bots is an advanced capability which allows the bot to send and receive voice and video content frame by frame.</span></span> <span data-ttu-id="25b8f-105">El bot tiene acceso "sin procesar" a las modalidades de voz, vídeo y pantalla compartida en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25b8f-105">The bot has "raw" access to the voice, video and screen-sharing real-time modalities.</span></span> <span data-ttu-id="25b8f-106">En este artículo se proporciona información general sobre la compilación de un bot que realiza llamadas de audio y vídeo y el acceso a las modalidades en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25b8f-106">This article provides an overview of building an audio/video calling bot and accessing the real-time modalities.</span></span>

<span data-ttu-id="25b8f-107">En este artículo, el bot se ejecuta en un servicio en la nube de Azure, como un rol web o un rol de trabajo que autohospeda la plataforma ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="25b8f-107">In this article, the bot is running in an Azure Cloud Service, as either a Web Role or a Worker Role self-hosting the ASP.NET Web API framework.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="25b8f-108">El contenido de este artículo es preliminar. Consulte la carpeta de ejemplos del repositorio<a href="https://github.com/Microsoft/BotBuilder-RealTimeMediaCalling">BotBuilder-RealTimeMediaCalling</a> de GitHub para ver todo el código de los bots de elementos multimedia en tiempo real de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="25b8f-108">This article content is preliminary; please refer to the Samples folder in the <a href="https://github.com/Microsoft/BotBuilder-RealTimeMediaCalling">BotBuilder-RealTimeMediaCalling</a> GitHub repository for the full code of sample real-time media bots.</span></span>

## <a name="configure-the-service-hosting-the-real-time-media-bot"></a><span data-ttu-id="25b8f-109">Configuración del servicio que hospeda el bot de elementos multimedia en tiempo real</span><span class="sxs-lookup"><span data-stu-id="25b8f-109">Configure the service hosting the real-time media bot</span></span>

<span data-ttu-id="25b8f-110">Para usar la plataforma de elementos multimedia en tiempo real, se necesitan las configuraciones de servicio siguientes.</span><span class="sxs-lookup"><span data-stu-id="25b8f-110">In order to use the Real-Time Media Platform, the following service configurations are necessary.</span></span>

* <span data-ttu-id="25b8f-111">El bot debe conocer el nombre de dominio completo (FQDN) de su servicio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-111">The bot must know the Fully-Qualified Domain Name (FQDN) of its service.</span></span> <span data-ttu-id="25b8f-112">No lo proporciona la API Azure RoleEnvironment; en su lugar, el nombre de dominio completo se debe almacenar en la configuración de servicio en la nube del bot y se debe leer durante el inicio del servicio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-112">This is not provided by the Azure RoleEnvironment API; instead, the FQDN must be stored in the bot's Cloud Service configuration and read during service startup.</span></span>

* <span data-ttu-id="25b8f-113">Bot Service debe tener un certificado emitido por una entidad de certificación reconocida.</span><span class="sxs-lookup"><span data-stu-id="25b8f-113">Bot Service must be have a certificate issued by a recognized Certificate Authority.</span></span> <span data-ttu-id="25b8f-114">La huella digital del certificado se debe almacenar en la configuración del servicio en la nube del bot y se debe leer durante el inicio del servicio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-114">The thumbprint of the certificate must be stored in the bot's Cloud Service configuration and read during service startup.</span></span>

* <span data-ttu-id="25b8f-115">Se debe aprovisionar un <a href="/azure/cloud-services/cloud-services-enable-communication-role-instances#instance-input-endpoint">punto de conexión de entrada de instancia</a> público.</span><span class="sxs-lookup"><span data-stu-id="25b8f-115">A public <a href="/azure/cloud-services/cloud-services-enable-communication-role-instances#instance-input-endpoint">instance input endpoint</a> must be provisioned.</span></span> <span data-ttu-id="25b8f-116">Esto asigna un puerto público único a cada instancia de máquina virtual (VM) en el servicio del bot.</span><span class="sxs-lookup"><span data-stu-id="25b8f-116">This assigns a unique public port to each virtual machine (VM) instance in the bot's service.</span></span> <span data-ttu-id="25b8f-117">Este puerto lo usa la plataforma de elementos multimedia en tiempo real para comunicarse con la nube de llamadas de Skype.</span><span class="sxs-lookup"><span data-stu-id="25b8f-117">This port is used by the Real-Time Media Platform to communicate with the Skype Calling Cloud.</span></span>
  ```xml
  <InstanceInputEndpoint name="InstanceMediaControlEndpoint" protocol="tcp" localPort="20100">
    <AllocatePublicPortFrom>
    <FixedPortRange max="20200" min="20101" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
  ```

  <span data-ttu-id="25b8f-118">También es útil para crear otro punto de conexión de entrada de instancia para devoluciones de llamada y notificaciones relacionadas con las llamadas.</span><span class="sxs-lookup"><span data-stu-id="25b8f-118">It is also useful to create another instance input endpoint for call related callbacks and notifications.</span></span> <span data-ttu-id="25b8f-119">Usar un punto de conexión de entrada de instancia garantiza que las devoluciones de llamada y las notificaciones se entreguen a la misma instancia de VM en la implementación del servicio que hospeda la sesión multimedia en tiempo real para la llamada.</span><span class="sxs-lookup"><span data-stu-id="25b8f-119">Using an instance input endpoint ensures the callbacks and notifications are delivered to the same VM instance in the service deployment that is hosting the real-time media session for the call.</span></span>
  ```xml
  <InstanceInputEndpoint name="InstanceCallControlEndpoint" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
    <FixedPortRange max="10200" min="10101" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
  ```

* <span data-ttu-id="25b8f-120">Cada instancia de VM debe tener una dirección IP pública de nivel de instancia (ILPIP).</span><span class="sxs-lookup"><span data-stu-id="25b8f-120">Each VM instance must have an instance-level public IP address (ILPIP).</span></span> <span data-ttu-id="25b8f-121">Durante el inicio, el bot debe detectar la dirección ILPIP asignada a cada instancia de servicio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-121">During startup, the bot must discover the ILPIP address assigned to each service instance.</span></span> <span data-ttu-id="25b8f-122">Consulte <a href="/azure/virtual-network/virtual-networks-instance-level-public-ip">ILPIP</a> para más información sobre cómo obtener y configurar una dirección ILPIP.</span><span class="sxs-lookup"><span data-stu-id="25b8f-122">See <a href="/azure/virtual-network/virtual-networks-instance-level-public-ip">ILPIP</a> for more information about obtaining and configuring an ILPIP.</span></span>
  ```xml
  <NetworkConfiguration>
  <AddressAssignments>
    <InstanceAddress roleName="WorkerRole">
    <PublicIPs>
        <PublicIP name="InstancePublicIP" domainNameLabel="InstancePublicIP" />
    </PublicIPs>
    </InstanceAddress>
  </AddressAssignments>
  </NetworkConfiguration>
  ```

* <span data-ttu-id="25b8f-123">Durante el inicio de la instancia de servicio, el script `MediaPlatformStartupScript.bat` (proporcionado como parte del paquete NuGet) se debe ejecutar como una tarea de inicio con privilegios elevados.</span><span class="sxs-lookup"><span data-stu-id="25b8f-123">During service instance startup, the script `MediaPlatformStartupScript.bat` (provided as a part of Nuget package) needs to be run as a Startup task under elevated privileges.</span></span> <span data-ttu-id="25b8f-124">La ejecución del script se debe completar antes de llamar al método de inicialización de la plataforma.</span><span class="sxs-lookup"><span data-stu-id="25b8f-124">The script execution must complete before the platform’s initialization method is called.</span></span> 

```xml
<Startup>
<Task commandLine="MediaPlatformStartupScript.bat" executionContext="elevated" taskType="simple" />      
</Startup> 
```

## <a name="initialize-the-media-platform-on-service-startup"></a><span data-ttu-id="25b8f-125">Inicialización de la plataforma multimedia al inicio del servicio</span><span class="sxs-lookup"><span data-stu-id="25b8f-125">Initialize the media platform on service startup</span></span>

<span data-ttu-id="25b8f-126">Durante el inicio de la instancia de servicio, se debe inicializar la plataforma multimedia en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25b8f-126">During service instance startup, the Real-Time Media Platform must be initialized.</span></span> <span data-ttu-id="25b8f-127">Esto debe hacerse solo una vez antes de que el bot pueda aceptar las llamadas de Skype de audio y vídeo en esa instancia.</span><span class="sxs-lookup"><span data-stu-id="25b8f-127">This must be done only once before the bot can accept any audio/video Skype calls on that instance.</span></span> <span data-ttu-id="25b8f-128">La inicialización de la plataforma multimedia requiere proporcionar varias opciones de configuración del servicio, incluido el nombre de dominio completo del servicio, la dirección ILPIP pública, el valor del puerto del punto de conexión de entrada de instancia y el **identificador de aplicación de Microsoft** del bot.</span><span class="sxs-lookup"><span data-stu-id="25b8f-128">Initialization of the media platform requires providing various service configuration settings, including the service's FQDN, the public ILPIP address, the instance input endpoint port value, and the bot's **Microsoft App ID**.</span></span>

> [!NOTE]
> <span data-ttu-id="25b8f-129">Para buscar loPara buscar los valores **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span><span class="sxs-lookup"><span data-stu-id="25b8f-129">To find your bot's **AppID** and **AppPassword**, see [MicrosoftAppID and MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).</span></span>

```cs
var mediaPlatformSettings = new MediaPlatformSettings()
{
    MediaPlatformInstanceSettings = new MediaPlatformInstanceSettings()
    {
        CertificateThumbprint = certificateThumbprint,
        InstanceInternalPort = instanceMediaControlEndpointInternalPort,
        InstancePublicIPAddress = instancePublicIPAddress,
        InstancePublicPort = instanceMediaControlEndpointPublicPort,
        ServiceFqdn = serviceFqdn
    },

    ApplicationId = MicrosoftAppId
};

MediaPlatform.Initialize(mediaPlatformSettings);            
```

## <a name="register-to-receive-incoming-call-requests"></a><span data-ttu-id="25b8f-130">Registro para recibir solicitudes de llamadas entrantes</span><span class="sxs-lookup"><span data-stu-id="25b8f-130">Register to receive incoming call requests</span></span>

<span data-ttu-id="25b8f-131">Defina una clase `CallController`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-131">Define a `CallController` class.</span></span> <span data-ttu-id="25b8f-132">Esto permite que Bot Service se registre para las llamadas entrantes de Skype y se asegure de que las solicitudes de devolución de llamada y notificación se reenvían al objeto `RealTimeMediaCall` correspondiente.</span><span class="sxs-lookup"><span data-stu-id="25b8f-132">This enables Bot Service to register for incoming Skype calls, and ensure callback and notification requests are forwarded to the appropriate `RealTimeMediaCall` object.</span></span>

```cs
[BotAuthentication]
[RoutePrefix("api/calling")]
public class CallController : ApiController
{
    static CallController()
    {
        RealTimeMediaCalling.RegisterRealTimeMediaCallingBot(
            c => { return new RealTimeMediaCall(c); },
            new RealTimeMediaCallingBotServiceSettings()
        );
    }

    [Route("call")]
    public async Task<HttpResponseMessage> OnIncomingCallAsync()
    {
        // forwards the incoming call to the associated RealTimeMediaCall object
        return await RealTimeMediaCalling.SendAsync(this.Request, RealTimeMediaCallRequestType.IncomingCall);
    }

    [Route("callback")]
    public async Task<HttpResponseMessage> OnCallbackAsync()
    {
        // forwards the incoming callback to the associated RealTimeMediaCall object
        return await RealTimeMediaCalling.SendAsync(this.Request, RealTimeMediaCallRequestType.CallingEvent);
    }

    [Route("notification")]
    public async Task<HttpResponseMessage> OnNotificationAsync()
    {
        // forwards the incoming notification to the associated RealTimeMediaCall object
        return await RealTimeMediaCalling.SendAsync(this.Request, RealTimeMediaCallRequestType.NotificationEvent);
    }
}
```

<span data-ttu-id="25b8f-133">`RealTimeMediaCallingBotServiceSettings` implementa `IRealTimeMediaCallServiceSettings` y proporciona vínculos de webhook para eventos de devolución de llamada y notificación.</span><span class="sxs-lookup"><span data-stu-id="25b8f-133">`RealTimeMediaCallingBotServiceSettings` implements `IRealTimeMediaCallServiceSettings` and provides webhook links for callback and notification events.</span></span>

## <a name="register-for-incoming-events-for-the-call"></a><span data-ttu-id="25b8f-134">Registro para los eventos entrantes de la llamada</span><span class="sxs-lookup"><span data-stu-id="25b8f-134">Register for incoming events for the call</span></span>

<span data-ttu-id="25b8f-135">Defina una clase `RealTimeMediaCall` que implemente `IRealTimeMediaCall`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-135">Define a `RealTimeMediaCall` class that implements `IRealTimeMediaCall`.</span></span> <span data-ttu-id="25b8f-136">Para cada llamada que el bot recibe, Bot Framework crea una instancia de `RealTimeMediaCall`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-136">For each call that is received by the bot, an instance of `RealTimeMediaCall` is created by the Bot Framework.</span></span> <span data-ttu-id="25b8f-137">El objeto `IRealTimeMediaCallService` pasado al constructor permite que el bot se registre para eventos con el fin de controlar los eventos asociados con la llamada multimedia en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25b8f-137">The `IRealTimeMediaCallService` object passed to the constructor allows the bot to register for events to handle events associated with the real-time media call.</span></span>

```cs
internal class RealTimeMediaCall : IRealTimeMediaCall
{
     public RealTimeMediaCall(IRealTimeMediaCallService callService)
     {
         if (callService == null)
             throw new ArgumentNullException(nameof(callService));

         CallService = callService;
         CorrelationId = callService.CorrelationId;
         CallId = CorrelationId + ":" + Guid.NewGuid().ToString();

         // Register for the call events
         CallService.OnIncomingCallReceived += OnIncomingCallReceived;
         CallService.OnAnswerAppHostedMediaCompleted += OnAnswerAppHostedMediaCompleted;
         CallService.OnCallStateChangeNotification += OnCallStateChangeNotification;
         CallService.OnRosterUpdateNotification += OnRosterUpdateNotification;
         CallService.OnCallCleanup += OnCallCleanup;
     }
}
```

## <a name="create-audio-and-video-sockets"></a><span data-ttu-id="25b8f-138">Creación de sockets de audio y vídeo</span><span class="sxs-lookup"><span data-stu-id="25b8f-138">Create audio and video sockets</span></span>
<span data-ttu-id="25b8f-139">Antes de que el bot pueda aceptar una llamada entrante de audio o vídeo de Skype, debe crear los objetos `AudioSocket` y `VideoSocket` para admitir el envío y la recepción de elementos multimedia en tiempo real.</span><span class="sxs-lookup"><span data-stu-id="25b8f-139">Before the bot can accept an incoming audio/video Skype call, it must create `AudioSocket` and `VideoSocket` objects in order to support sending and receiving real-time media.</span></span> <span data-ttu-id="25b8f-140">(Si el bot no quiere admitir el vídeo, debe crear solo un objeto AudioSocket).</span><span class="sxs-lookup"><span data-stu-id="25b8f-140">(If the bot does not want to support video, then it should create only an AudioSocket.)</span></span>

<span data-ttu-id="25b8f-141">El bot debe decidir por adelantado qué modalidades quiere admitir y deberá crear los objetos AudioSocket y VideoSocket adecuados.</span><span class="sxs-lookup"><span data-stu-id="25b8f-141">The bot must decide upfront which modalities it wants to support, and create the appropriate AudioSocket and VideoSocket objects.</span></span> <span data-ttu-id="25b8f-142">El bot no puede cambiar las modalidades que admite para la llamada una vez que se acepta la llamada entrante.</span><span class="sxs-lookup"><span data-stu-id="25b8f-142">The bot cannot change what modalities it supports for the call after the incoming call is accepted.</span></span>

<span data-ttu-id="25b8f-143">Para cada objeto AudioSocket y VideoSocket, el bot especifica si el socket multimedia es para admitir tanto el envío como la recepción de elementos multimedia o solo el envío o la recepción.</span><span class="sxs-lookup"><span data-stu-id="25b8f-143">For each AudioSocket and VideoSocket, the bot specifies whether the media socket is to support both sending and receiving media, or sending or receiving only.</span></span> <span data-ttu-id="25b8f-144">Por ejemplo, es posible que el bot quiera enviar y recibir audio ("Sendrecv"), pero solo enviar vídeo ("Sendonly").</span><span class="sxs-lookup"><span data-stu-id="25b8f-144">For example, the bot may wish to send and receive audio ("Sendrecv"), but send only for video ("Sendonly").</span></span> <span data-ttu-id="25b8f-145">El bot también debe especificar los formatos multimedia que se admiten para cada socket multimedia.</span><span class="sxs-lookup"><span data-stu-id="25b8f-145">The bot must also specify what media formats are supported for each media socket.</span></span> <span data-ttu-id="25b8f-146">Para un objeto AudioSocket, el formato actualmente compatible es "Pcm16K": (firmado) codificación PCM de 16 bits, frecuencia de muestreo de 16 KHz.</span><span class="sxs-lookup"><span data-stu-id="25b8f-146">For an AudioSocket, the currently supported format is "Pcm16K": (signed) 16-bit PCM encoding, 16KHz sampling rate.</span></span> <span data-ttu-id="25b8f-147">Para un objeto VideoSocket, los formatos multimedia para envío y recepción se especifican por separado.</span><span class="sxs-lookup"><span data-stu-id="25b8f-147">For a VideoSocket, media formats for sending and receiving are specified separately.</span></span> <span data-ttu-id="25b8f-148">Solo se admite el formato "NV12" para recibir vídeo, mientras que se admiten varios formatos distintos para envío.</span><span class="sxs-lookup"><span data-stu-id="25b8f-148">Only the "NV12" format is supported for receiving video, while several different formats are supported for sending.</span></span>

```cs
_audioSocket = new AudioSocket(new AudioSocketSettings
{
    StreamDirections = StreamDirection.Sendrecv,
    SupportedAudioFormat = AudioFormat.Pcm16K,
    CallId = correlationId
});

_videoSocket = new VideoSocket(new VideoSocketSettings
{
    StreamDirections = StreamDirection.Sendrecv,
    ReceiveColorFormat = VideoColorFormat.NV12,
    SupportedSendVideoFormats = new VideoFormat[]
    {
        VideoFormat.Yuy2_1280x720_30Fps,
        VideoFormat.Yuy2_720x1280_30Fps,
    },
    CallId = correlationId
});

_audioSocket.AudioMediaReceived += OnAudioMediaReceived;
_audioSocket.AudioSendStatusChanged += OnAudioSendStatusChanged;
_audioSocket.DominantSpeakerChanged += OnDominantSpeakerChanged;
_videoSocket.VideoMediaReceived += OnVideoMediaReceived;
_videoSocket.VideoSendStatusChanged += OnVideoSendStatusChanged;
```                             

## <a name="create-a-mediaconfiguration"></a><span data-ttu-id="25b8f-149">Creación de un objeto MediaConfiguration</span><span class="sxs-lookup"><span data-stu-id="25b8f-149">Create a MediaConfiguration</span></span>
<span data-ttu-id="25b8f-150">Una vez que se crean los sockets multimedia, el bot debe crear un objeto `MediaConfiguration` que es necesario para asociar los sockets multimedia con una llamada entrante de audio o vídeo de Skype.</span><span class="sxs-lookup"><span data-stu-id="25b8f-150">Once the media sockets have been created, the bot must next create a `MediaConfiguration` object which is needed to associate the media sockets with an incoming audio/video Skype call.</span></span>

```cs
var mediaConfiguration = MediaPlatform.CreateMediaConfiguration(_audioSocket, _videoSocket);
```

##  <a name="answer-an-incoming-audiovideo-call"></a><span data-ttu-id="25b8f-151">Respuesta a una llamada de audio o vídeo entrante</span><span class="sxs-lookup"><span data-stu-id="25b8f-151">Answer an incoming audio/video call</span></span>
<span data-ttu-id="25b8f-152">El evento `OnIncomingCallReceived` se invoca para permitir que el bot acepte la llamada de audio o vídeo entrante de Skype.</span><span class="sxs-lookup"><span data-stu-id="25b8f-152">The `OnIncomingCallReceived` event is invoked to allow the bot to accept the incoming Skype audio/video call.</span></span> <span data-ttu-id="25b8f-153">Para hacerlo, el bot crea un objeto `AnswerAppHostedMedia` con el objeto `MediaConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-153">To do so, the bot creates an `AnswerAppHostedMedia` object with the `MediaConfiguration` object.</span></span> <span data-ttu-id="25b8f-154">El bot se registra para las notificaciones asociadas con esta llamada de Skype.</span><span class="sxs-lookup"><span data-stu-id="25b8f-154">The bot registers for notifications associated with this Skype call.</span></span>

```cs
private Task OnIncomingCallReceived(RealTimeMediaIncomingCallEvent incomingCallEvent)
{
    // ... create Audio/VideoSocket objects and MediaConfiguration ...

    incomingCallEvent.RealTimeMediaWorkflow.Actions = new ActionBase[]
    {
        new AnswerAppHostedMedia
        {
            MediaConfiguration = mediaConfiguration,
            OperationId = Guid.NewGuid().ToString()
        }
    };

    // subscribe for roster and call state changes
    incomingCallEvent.RealTimeMediaWorkflow.NotificationSubscriptions = new NotificationType[]
    {
        NotificationType.CallStateChange,
        NotificationType.RosterUpdate
    };
}
```

## <a name="outcome-of-the-call"></a><span data-ttu-id="25b8f-155">Resultado de la llamada</span><span class="sxs-lookup"><span data-stu-id="25b8f-155">Outcome of the call</span></span>
<span data-ttu-id="25b8f-156">`OnAnswerAppHostedMediaCompleted` se produce cuando se completa la acción `AnswerAppHostedMedia`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-156">`OnAnswerAppHostedMediaCompleted` is raised when the `AnswerAppHostedMedia` action completes.</span></span> <span data-ttu-id="25b8f-157">La propiedad `Outcome` en `AnswerAppHostedMediaOutcomeEvent` indica si la acción se realizó correctamente o con errores.</span><span class="sxs-lookup"><span data-stu-id="25b8f-157">The `Outcome` property in the `AnswerAppHostedMediaOutcomeEvent` indicates success or failure.</span></span> <span data-ttu-id="25b8f-158">Si no se puede establecer la llamada, el bot debe eliminar los objetos AudioSocket y VideoSocket que creó para la llamada.</span><span class="sxs-lookup"><span data-stu-id="25b8f-158">If the call cannot be established, the bot should dispose the AudioSocket and VideoSocket objects it created for the call.</span></span>

## <a name="receive-audio-media"></a><span data-ttu-id="25b8f-159">Recepción de elementos multimedia de audio</span><span class="sxs-lookup"><span data-stu-id="25b8f-159">Receive audio media</span></span>
<span data-ttu-id="25b8f-160">Si el objeto `AudioSocket` se creó con la capacidad de recibir audio, el evento `AudioMediaReceived` se invocará cada vez que se reciba una trama de audio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-160">If the `AudioSocket` was created with the ability to receive audio, then the `AudioMediaReceived` event will be invoked each time a frame of audio is received.</span></span> <span data-ttu-id="25b8f-161">El bot debe esperar a controlar este evento aproximadamente 50 veces por segundo, independientemente del par que podría originar el contenido de audio (debido a que los búferes de ruido aceptable se generan localmente si no se recibe audio del par).</span><span class="sxs-lookup"><span data-stu-id="25b8f-161">The bot should expect to handle this event approximately 50 times per second, regardless of the peer that could be sourcing audio content (since comfort noise buffers are generated locally if no audio is received from the peer).</span></span> <span data-ttu-id="25b8f-162">Cada paquete de contenido de audio se entrega en un objeto `AudioMediaBuffer`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-162">Each packet of audio content is delivered in an `AudioMediaBuffer` object.</span></span> <span data-ttu-id="25b8f-163">Este objeto contiene un puntero a un búfer de memoria asignados por montón nativo que contiene el contenido de audio descodificado.</span><span class="sxs-lookup"><span data-stu-id="25b8f-163">This object contains a pointer to a native heap-allocated memory buffer containing the decoded audio content.</span></span> 

```cs
void OnAudioMediaReceived(
            object sender,
            AudioMediaReceivedEventArgs args)
{
   var buffer = args.Buffer;

   // native heap-allocated memory containing decoded content
   IntPtr rawData = buffer.Data;            
}
```

<span data-ttu-id="25b8f-164">El controlador de eventos se debe devolver rápidamente.</span><span class="sxs-lookup"><span data-stu-id="25b8f-164">The event handler must return quickly.</span></span> <span data-ttu-id="25b8f-165">Se recomienda que la aplicación ponga en la cola el objeto `AudioMediaBuffer` que se procesará de manera asincrónica.</span><span class="sxs-lookup"><span data-stu-id="25b8f-165">It is recommended that the application queue the `AudioMediaBuffer` to be processed asynchronously.</span></span> <span data-ttu-id="25b8f-166">Los eventos de `OnAudioMediaReceived` se serializarán a través de la plataforma multimedia en tiempo real (es decir, el evento siguiente no se generará hasta que se devuelva el actual).</span><span class="sxs-lookup"><span data-stu-id="25b8f-166">`OnAudioMediaReceived` events will be serialized by the Real-Time Media Platform (that is, the next event will not be raised until the current one returns).</span></span> <span data-ttu-id="25b8f-167">Una vez que se consume un objeto `AudioMediaBuffer`, la aplicación debe llamar al método Dispose del búfer para que la plataforma multimedia pueda reclamar la memoria subyacente no administrada.</span><span class="sxs-lookup"><span data-stu-id="25b8f-167">Once an `AudioMediaBuffer` has been consumed, the application must call the buffer's Dispose method so that the underlying unmanaged memory can be reclaimed by the media platform.</span></span> 

```cs
   // release/dispose buffer when done 
   buffer.Dispose();
```

> [!IMPORTANT]
> <span data-ttu-id="25b8f-168">El bot no debe llamar al método Dispose del búfer hasta que haya terminado de acceder al búfer.</span><span class="sxs-lookup"><span data-stu-id="25b8f-168">The bot must not call the buffer's Dispose method until it is finished accessing the buffer.</span></span>

## <a name="receive-video-media"></a><span data-ttu-id="25b8f-169">Recepción de elementos multimedia de vídeo</span><span class="sxs-lookup"><span data-stu-id="25b8f-169">Receive video media</span></span>
<span data-ttu-id="25b8f-170">La recepción de vídeo es similar a recibir audio, excepto en que el número de búferes por segundo dependerá del valor de la velocidad de fotogramas.</span><span class="sxs-lookup"><span data-stu-id="25b8f-170">Receiving video is similar to receiving audio except that the number of buffers per second will depend on value of the frame rate.</span></span> <span data-ttu-id="25b8f-171">`VideoMediaBuffer` tiene las propiedades `VideoFormat` y `OriginalVideoFormat`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-171">`VideoMediaBuffer` has `VideoFormat` and `OriginalVideoFormat` properties.</span></span> <span data-ttu-id="25b8f-172">`OriginalVideoFormat` representa el formato original del búfer cuando se originó.</span><span class="sxs-lookup"><span data-stu-id="25b8f-172">`OriginalVideoFormat` represents the original format of the buffer when it was sourced.</span></span> <span data-ttu-id="25b8f-173">Solo está disponible cuando se reciben búferes de vídeo a través del controlador de eventos `IVideoSocket.VideoMediaReceived`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-173">It is only available when receiving video buffers via the `IVideoSocket.VideoMediaReceived` event handler.</span></span> <span data-ttu-id="25b8f-174">Si el tamaño del búfer se cambió antes de empezar la transmisión, la propiedad `OriginalVideoFormat` tendrá el ancho y el alto original, mientras que `VideoFormat` tendrá los valores actuales después del cambio de tamaño.</span><span class="sxs-lookup"><span data-stu-id="25b8f-174">If the buffer was resized before being transmitted, the `OriginalVideoFormat` property will have the original Width and Height, whereas `VideoFormat` will have the current ones after the resize.</span></span> <span data-ttu-id="25b8f-175">Si las propiedades de ancho y alto de `OriginalVideoFormat` difieren de la propiedad `VideoFormat`, el consumidor del objeto `VideoMediaBuffer` que se produjo en el evento `VideoMediaReceived` debe cambiar el tamaño del búfer para ajustarlo al tamaño de `OriginalVideoFormat`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-175">If the Width and Height properties of `OriginalVideoFormat` differ from the `VideoFormat` property, the consumer of the `VideoMediaBuffer` raised in the `VideoMediaReceived` event should resize the buffer to fit the `OriginalVideoFormat` size.</span></span> <span data-ttu-id="25b8f-176">Actualmente solo se admite el formato NV12 para recepción.</span><span class="sxs-lookup"><span data-stu-id="25b8f-176">Currently only NV12 format is supported for receive.</span></span>

## <a name="send-audio-media"></a><span data-ttu-id="25b8f-177">Envío de elementos multimedia de audio</span><span class="sxs-lookup"><span data-stu-id="25b8f-177">Send audio media</span></span>
<span data-ttu-id="25b8f-178">Si el objeto `AudioSocket` está configurado para enviar elementos multimedia, el bot se debe registrar para el controlador de eventos `AudioSendStatusChanged` en el objeto `AudioSocket` para obtener notificaciones sobre los cambios en el estado del envío.</span><span class="sxs-lookup"><span data-stu-id="25b8f-178">If the `AudioSocket` is configured to send media, the bot should register for the `AudioSendStatusChanged` event handler on the `AudioSocket` to get notifications about sending status changes.</span></span> <span data-ttu-id="25b8f-179">El bot debería empezar a enviar audio solo después de que el estado del envío cambie a Activo.</span><span class="sxs-lookup"><span data-stu-id="25b8f-179">The bot should start sending audio only after the send status changes to Active.</span></span>

```cs
void AudioSocket_OnSendStatusChanged(
             object sender,
             AudioSendStatusChangedEventArgs args)
{
    switch (args.MediaSendStatus)
    {
    case MediaSendStatus.Active:
        // notify bot to begin sending audio 
        break;
     
    case MediaSendStatus.Inactive:
        // notify bot to stop sending audio
        break;
    }
}
```

<span data-ttu-id="25b8f-180">Para enviar elementos multimedia de audio, se supone que el contenido de audio de PCM está dentro de un búfer asignado por montón nativo.</span><span class="sxs-lookup"><span data-stu-id="25b8f-180">To send audio media, it is assumed the PCM audio content is contained within a native heap-allocated buffer.</span></span> <span data-ttu-id="25b8f-181">El bot debe derivar de la clase abstracta `AudioMediaBuffer`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-181">The bot must derive from the `AudioMediaBuffer` abstract class.</span></span> <span data-ttu-id="25b8f-182">Los elementos multimedia se envían asincrónicamente a través del método `Send` del objeto AudioSocket y la plataforma llamará a `Dispose` en `AudioMediaBuffer` cuando se complete el envío.</span><span class="sxs-lookup"><span data-stu-id="25b8f-182">Media is sent asynchronously by the AudioSocket's `Send` method and the platform will call `Dispose` on the `AudioMediaBuffer` when the send completes.</span></span> <span data-ttu-id="25b8f-183">El bot no debe liberar (ni devolverse a un asignador de grupos) los recursos no administrados cuando se devuelve `Send`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-183">The bot should not release (or return to a pool allocator) the unmanaged resources when the `Send` returns.</span></span> <span data-ttu-id="25b8f-184">Debe esperar a que se llame a `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-184">It must wait for `Dispose` to be called.</span></span>

## <a name="send-video-media"></a><span data-ttu-id="25b8f-185">Envío de elementos multimedia de vídeo</span><span class="sxs-lookup"><span data-stu-id="25b8f-185">Send video media</span></span>
<span data-ttu-id="25b8f-186">Enviar elementos multimedia de vídeo es similar al envío de elementos multimedia de audio.</span><span class="sxs-lookup"><span data-stu-id="25b8f-186">Sending video media is similar to that of audio media.</span></span> <span data-ttu-id="25b8f-187">El bot se debe registrar para el evento `VideoSendStatusChanged` y esperar que `MediaSendStatus` sea `Active`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-187">The bot should register for `VideoSendStatusChanged` event and wait for `MediaSendStatus` to be `Active`.</span></span> <span data-ttu-id="25b8f-188">El bot no debe liberar ni reclamar los recursos no administrados del búfer hasta que se llame al método `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-188">The bot must not release or reclaim the buffer's unmanaged resources until the `Dispose` method is called.</span></span> <span data-ttu-id="25b8f-189">Se admiten los formatos de color RGB24, NV12 y Yuy2.</span><span class="sxs-lookup"><span data-stu-id="25b8f-189">RGB24, NV12, and Yuy2 color formats are supported.</span></span>

<span data-ttu-id="25b8f-190">Cuando se admiten varios elementos `VideoFormat` para enviar vídeo, el elemento `VideoFormat` actualmente preferido se comunica al bot a través del evento `VideoSendStatusChanged`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-190">While multiple `VideoFormat`s are supported for sending video, the `VideoFormat` that is currently preferred is communicated to the bot via the `VideoSendStatusChanged` event.</span></span> <span data-ttu-id="25b8f-191">El elemento `VideoFormat` preferido para enviar vídeo puede cambiar debido a las condiciones del ancho de banda de red o al cliente del mismo nivel que cambia de tamaño su ventana de vídeo.</span><span class="sxs-lookup"><span data-stu-id="25b8f-191">The preferred `VideoFormat` for sending video may change due to network bandwidth conditions, or the peer client resizing its video window.</span></span>

```cs
void VideoSocket_OnSendStatusChanged(
            object sender,
            VideoSendStatusChangedEventArgs args)
{
    VideoFormat preferredVideoFormat;

    switch (args.MediaSendStatus)
    {
    case MediaSendStatus.Active:
        // notify bot to begin sending audio 
        // bot is recommended to use this format for sourcing video content.
        preferredVideoFormat = args.PreferredVideoSourceFormat;
        break;
     
    case MediaSendStatus.Inactive:
        // notify bot to stop sending audio
        break;
    }
}
```

<span data-ttu-id="25b8f-192">Cada `VideoMediaBuffer` tiene una propiedad VideoFormat para indicar el formato del contenido de vídeo de ese búfer en particular.</span><span class="sxs-lookup"><span data-stu-id="25b8f-192">Each `VideoMediaBuffer` has a VideoFormat property to indicate the format of the video content for that particular buffer.</span></span> <span data-ttu-id="25b8f-193">Si bien no es necesario que la propiedad `VideoFormat` coincida con la propiedad `PreferredVideoSourceFormat` indicada en el evento `VideoSendStatusChanged`, se recomienda usar el elemento `PreferredVideoSourceFormat` indicado para evitar la pérdida de ciclos de CPU dedicados a cambiar el tamaño de los fotogramas de vídeo.</span><span class="sxs-lookup"><span data-stu-id="25b8f-193">While the `VideoFormat` property does not have to match the `PreferredVideoSourceFormat` property indicated in the `VideoSendStatusChanged` event, it is strongly recommended to use the indicated `PreferredVideoSourceFormat` to avoid wasteful CPU cycles spent on video frame resizing.</span></span>

## <a name="call-notifications"></a><span data-ttu-id="25b8f-194">Notificaciones de llamada</span><span class="sxs-lookup"><span data-stu-id="25b8f-194">Call notifications</span></span>
### <a name="call-state-changes"></a><span data-ttu-id="25b8f-195">Cambios en el estado de las llamadas</span><span class="sxs-lookup"><span data-stu-id="25b8f-195">Call state changes</span></span>
<span data-ttu-id="25b8f-196">El boto puede obtener notificaciones de cambio del estado de la llamada si se suscribe a `NotificationType.CallStateChange` en `NotificationSubscriptions` de `RealTimeMediaIncomingCallEvent.RealTimeMediaWorkflow`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-196">The bot can get call state change notifications by subscribing to the `NotificationType.CallStateChange` in `NotificationSubscriptions` of `RealTimeMediaIncomingCallEvent.RealTimeMediaWorkflow`.</span></span>

```cs
private Task OnCallStateChangeNotification(CallStateChangeNotification callStateChangeNotification)
{
    if (callStateChangeNotification.CurrentState == CallState.Terminated)
    {   
        // stop sending media and dispose the media sockets
        _audioSocket.Dispose();
        _videoSocket.Dispose();
    }

    return Task.CompletedTask;
}
 ```

### <a name="roster-update"></a><span data-ttu-id="25b8f-197">Actualización de la lista</span><span class="sxs-lookup"><span data-stu-id="25b8f-197">Roster update</span></span>
<span data-ttu-id="25b8f-198">Si el blog se agrega a una conferencia, puede escuchar la lista de la conferencia si se suscribe a `NotificationType.RosterUpdate` en `NotificationSubscriptions` de `RealTimeMediaIncomingCallEvent.RealTimeMediaWorkflow`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-198">If the bot is added to a conference, it can listen to the roster of the conference by subscribing to `NotificationType.RosterUpdate` in `NotificationSubscriptions` of `RealTimeMediaIncomingCallEvent.RealTimeMediaWorkflow`.</span></span> <span data-ttu-id="25b8f-199">El elemento `RosterUpdateNotification` tiene los detalles de todos los participantes en la conferencia.</span><span class="sxs-lookup"><span data-stu-id="25b8f-199">The `RosterUpdateNotification` has the details of all participants in the conference.</span></span> <span data-ttu-id="25b8f-200">El bot podría optar por esperar una notificación con un participante que envía vídeo y luego suscribirse `Subscribe` al vídeo del participante en uno de sus objetos `VideoSocket`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-200">The bot could choose to wait for a notification with a participant sending video and then `Subscribe` to the participant's video on one of its `VideoSocket` objects.</span></span>

```cs
private async Task OnRosterUpdateNotification(RosterUpdateNotification rosterUpdateNotification)
{
    // Find a video source in the conference to subscribe
    foreach (RosterParticipant participant in rosterUpdateNotification.Participants)
    {
        if (participant.MediaType == ModalityType.Video
            && (participant.MediaStreamDirection == "sendonly" || participant.MediaStreamDirection == "sendrecv")
            )
        {
            var videoSubscription = new VideoSubscription
            {
                ParticipantIdentity = participant.Identity,
                OperationId = Guid.NewGuid().ToString(),
                SocketId = _videoSocket.SocketId,
                VideoModality = ModalityType.Video,
                VideoResolution = ResolutionFormat.Hd720p
            };

            await CallService.Subscribe(videoSubscription).ConfigureAwait(false);
            break;
        }
    }  
}
```

## <a name="end-the-call"></a><span data-ttu-id="25b8f-201">Finalización de la llamada</span><span class="sxs-lookup"><span data-stu-id="25b8f-201">End the call</span></span>

### <a name="handle-call-termination-from-the-caller"></a><span data-ttu-id="25b8f-202">Control de la finalización de la llamada desde el autor de la llamada</span><span class="sxs-lookup"><span data-stu-id="25b8f-202">Handle call termination from the caller</span></span>
<span data-ttu-id="25b8f-203">Cuando el usuario desconecta la llamada al bot en una conversación de punto a punto o cuando el bot se quita de la conferencia, el bot recibe una notificación de cambio de estado de la llamada con el objeto CallState como Terminated (Finalizado).</span><span class="sxs-lookup"><span data-stu-id="25b8f-203">When the user disconnects the call to the bot in a peer to peer conversation or when the bot is removed from the conference, the bot gets a call state change notification with the CallState as Terminated.</span></span> <span data-ttu-id="25b8f-204">El bot debería eliminar los sockets multimedia que creó.</span><span class="sxs-lookup"><span data-stu-id="25b8f-204">The bot should dispose any media sockets it created.</span></span>

### <a name="terminate-the-call-from-the-bot"></a><span data-ttu-id="25b8f-205">Finalización de la llamada desde el bot</span><span class="sxs-lookup"><span data-stu-id="25b8f-205">Terminate the call from the bot</span></span>
<span data-ttu-id="25b8f-206">El bot puede elegir finalizar la llamada si se llama a `EndCall` en `IRealTimeMediaCallService`.</span><span class="sxs-lookup"><span data-stu-id="25b8f-206">The bot can choose to end the call by calling `EndCall` on `IRealTimeMediaCallService`.</span></span>

### <a name="handle-call-clean-up-by-the-bot-framework"></a><span data-ttu-id="25b8f-207">Control de la limpieza de llamada por parte de Bot Framework</span><span class="sxs-lookup"><span data-stu-id="25b8f-207">Handle call clean up by the Bot Framework</span></span>
<span data-ttu-id="25b8f-208">En condiciones de error (por ejemplo, si `AnswerAppHostedMediaOutcomeEvent` no se recibe dentro de un plazo razonable), Bot Framework puede finalizar la llamada.</span><span class="sxs-lookup"><span data-stu-id="25b8f-208">On error conditions (for example, if `AnswerAppHostedMediaOutcomeEvent` is not received within a reasonable time), the Bot Framework may terminate the call.</span></span> <span data-ttu-id="25b8f-209">El bot se debe registrar para el evento `OnCallCleanup` y eliminar los sockets multimedia.</span><span class="sxs-lookup"><span data-stu-id="25b8f-209">The bot should register for `OnCallCleanup` event and dispose the media sockets.</span></span>

