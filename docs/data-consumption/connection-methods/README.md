# :material-connection: Métodos de Conexión

Esta sección describe el método de conexión disponible para consumir datos desde Reten hacia tu sistema. A diferencia de la carga de datos, el consumo se realiza exclusivamente a través de **APIs REST** expuestas por Reten.

{% from '/includes/cards.md' import section_overview %}

{% set connection_methods = [
    {
        'icon': 'material-api',
        'title': 'API REST',
        'link': './api-rest/README.md',
        'description': 'Endpoints REST para consultar y recibir datos en tiempo real'
    }
] %}

{{ section_overview(connection_methods) }}