## <a name="next-steps"></a>Pasos siguientes
Una vez que ha implementado su bot en la nube y lo ha probado con Bot Framework Emulator para comprobar que la implementación fue correcta, el siguiente paso en el proceso de publicación del bot dependerá de si ya ha registrado el bot con Bot Framework.

### <a name="if-you-have-already-registered-your-bot-with-the-bot-framework"></a>Si ya ha registrado el bot con Bot Framework:

1. Vuelva al <a href="https://dev.botframework.com" target="_blank">portal de Bot Framework</a> y [actualice los datos de configuración del bot](~/bot-service-manage-settings.md) para especificar el **punto de conexión de mensajería** del bot.

2. [Configure el bot para ejecutarse en uno o más canales](~/bot-service-manage-channels.md).

### <a name="if-you-have-not-yet-registered-your-bot-with-the-bot-framework"></a>Si aún no ha registrado el bot con Bot Framework:

1. [Registre el bot en Bot Framework](~/bot-service-quickstart-registration.md).

2. Actualice los valores de identificador y contraseña de aplicación de Microsoft en la configuración de la aplicación implementada para especificar los valores **appID** y **password** que se generaron para el bot durante el proceso de registro. Para buscar los valores **AppID** y **AppPassword** del bot, consulte [MicrosoftAppID y MicrosoftAppPassword](~/bot-service-manage-overview.md#microsoftappid-and-microsoftapppassword).

3. [Configure el bot para ejecutarse en uno o más canales](~/bot-service-manage-channels.md).