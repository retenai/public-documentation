# :material-database-export: Consumo de Datos

Esta sección contiene toda la información necesaria para consumir datos desde Reten hacia tu sistema. El consumo de datos es el segundo paso fundamental para implementar Reten en tu negocio, permitiéndote acceder a la información procesada y generada por la plataforma.

## [:material-connection: Métodos de Conexión](./connection-methods/README.md)

Para consumir datos desde Reten, dispones de un método de conexión unificado:

{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set connection_methods = [
    {
        'icon': 'material-api',
        'title': 'API REST',
        'link': 'connection-methods/api-rest/README.md',
        'description': 'Endpoints REST para consultar y recibir datos en tiempo real'
    }
] %}

{{ section_overview(connection_methods) }}

## :material-format-list-bulleted-type: Tipos de Datos

{% set data_types = [
    {
        'icon': 'material-checkbox-marked-circle',
        'title': 'Tareas',
        'link': './tasks/README.md',
        'description': 'Sistema de tareas, seguimiento y gestión de actividades.'
    },
    {
        'icon': 'material-account-group',
        'title': 'Estados de Usuario',
        'link': './user-states/README.md',
        'description': 'Clasificación y segmentación de usuarios según su engagement y comportamiento.'
    }
] %}

{{ section_overview(data_types) }}

## :material-step-forward-2: Flujo de Implementación

Para implementar el consumo de datos en Reten, sigue estos pasos:

!!! note "1. Configura la Autenticación"
    - Solicita credenciales de API al equipo de Reten
    - Configura tokens de acceso seguros
    - Establece permisos y scopes necesarios

!!! note "2. Implementa el Consumo de Datos"
    - Configura endpoints para consultar tareas y estados de usuario
    - Implementa polling para sincronización periódica de ambos tipos de datos
    - Establece filtros para obtener tareas específicas y segmentar usuarios por estado

!!! note "3. Procesa y Utiliza los Datos"
    - Integra las tareas en tu sistema de gestión y CRM
    - Utiliza los estados de usuario para segmentación y personalización
    - Implementa flujos de trabajo para las actividades basados en el estado del usuario
    - Configura notificaciones y alertas según el engagement del usuario

!!! note "4. Monitorea y Optimiza"
    - Revisa logs de consumo regularmente
    - Optimiza la frecuencia de consultas
    - Ajusta la configuración según tus necesidades
