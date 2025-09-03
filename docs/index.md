# :material-book-open-page-variant: Documentación Técnica de Reten

Bienvenido a la documentación técnica de Reten. Esta documentación está diseñada para proporcionar información detallada sobre la estructura, funcionamiento y uso de nuestro sistema.

## :material-clipboard-flow: Flujos
Para funcionar con Reten es necesario desarrollar integraciones en dos direcciones: **Carga de datos** y **Consumo de datos**.


{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set data_flows = [
    {
        'icon': 'material-database-import',
        'title': 'Carga de datos',
        'link': 'data-loading/README.md',
        'description': 'Tu sistema envía información a Reten.'
    },
    {
        'icon': 'material-database-export',
        'title': 'Consumo de datos',
        'link': 'data-consumption/README.md',
        'description': 'Reten envía información a tu sistema.'
    }
] %}

{{ section_overview(data_flows) }}

## 🚀 Comenzando

Para implementar Reten de manera efectiva, sigue estos pasos:

!!! note "1. Implementa la Carga de Datos"
    - Elige tu [Método de Conexión](data-loading/README.md#métodos-de-conexión) (Archivos o Base de datos)
    - Configura tus [Datos Maestros](data-loading/README.md#datos-maestros) (Clientes, Transacciones, Productos, Vendedores)
    - Establece tus [Configuraciones](data-loading/README.md#configuraciones) (Asignaciones y Rutas)

!!! note "2. Consume las Tareas"
    - Conectate a la API de Reten
    - Recibe tareas asignadas desde [Consumo de Datos](data-consumption/README.md)
