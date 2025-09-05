# :material-message-processing: Comunicación

Esta sección contiene toda la información necesaria para integrar Reten con tus canales de comunicación. Esta funcionalidad permite que Reten ejecute tareas de comunicación de manera autónoma, enviando notificaciones por **Email**, **WhatsApp**, **SMS** o **Notificaciones Push** según las reglas y triggers configurados.

## :material-cog: Configuración de Canales

La configuración de canales se realiza internamente por el equipo de Reten a través de nuestra API de mensajería.

{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set channel_config = [
    {
        'icon': 'material-cog',
        'title': 'Configuración de Canales',
        'link': './channel-provider-configuration/README.md',
        'description': 'Documentación completa sobre la configuración interna de canales de comunicación'
    }
] %}

{{ section_overview(channel_config) }}

## :material-bell-ring: Canales Disponibles

Una vez configurado el canal por Reten, puedes implementar cualquiera de estos canales de comunicación:

{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set communication_channels = [
    {
        'icon': 'material-email',
        'title': 'Email',
        'link': './email/README.md',
        'description': 'Envío automático de correos electrónicos personalizados'
    },
    {
        'icon': 'material-message',
        'title': 'WhatsApp',
        'link': './whatsapp/README.md',
        'description': 'Mensajes automatizados vía WhatsApp Business API'
    },
    {
        'icon': 'material-message-text',
        'title': 'SMS',
        'link': './sms/README.md',
        'description': 'Notificaciones por mensaje de texto'
    },
    {
        'icon': 'material-bell',
        'title': 'Notificaciones Push',
        'link': './push/README.md',
        'description': 'Notificaciones push en aplicaciones móviles y web'
    }
] %}

{{ section_overview(communication_channels) }}

## :material-robot: Funcionamiento Automático

A diferencia de las tareas manuales disponibles en el consumo de datos, estas comunicaciones se ejecutan automáticamente por Reten según:

- **Triggers del Sistema**: Eventos específicos que gatillan comunicaciones
- **Reglas de Negocio**: Lógica configurada para determinar cuándo enviar mensajes
- **Segmentación de Usuarios**: Envío a usuarios específicos según criterios
- **Personalización**: Contenido adaptado al perfil y comportamiento del usuario

## :material-step-forward-2: Próximos Pasos

!!! note "1. Revisa la Configuración"
    - Consulta la documentación de [Configuración de Canales](./configuracion/README.md)
    - Proporciona tus credenciales al equipo de Reten

!!! note "2. Prepara tus Datos"
    - Incluye campos de contacto en tu carga de usuarios
    - Mantén actualizados los identificadores de comunicación

!!! note "3. Explora los Canales"
    - Revisa la documentación específica de cada canal disponible
    - Define tus requisitos de comunicación automática
