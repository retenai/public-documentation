# :material-database-import: Carga de Datos

Esta sección contiene toda la información necesaria para cargar datos desde tu sistema hacia Reten. La carga de datos es el primer paso fundamental para implementar Reten en tu negocio.

## [:material-connection: Métodos de Conexión](./connection-methods/README.md)

Reten ofrece dos métodos principales para cargar datos:

{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set connection_methods = [
    {
        'icon': 'material-folder',
        'title': 'Archivo',
        'link': './connection-methods/file-based/README.md',
        'description': 'Carga periódica de archivos CSV en bucket compartido para sincronización de datos.'
    },
    {
        'icon': 'material-database',
        'title': 'Base de Datos',
        'link': './connection-methods/database/README.md',
        'description': 'Consulta directa a base de datos expuesta por el cliente para acceso en tiempo real.'
    }
] %}

{{ section_overview(connection_methods) }}

## :material-format-list-bulleted-type: Tipos de Datos

### [:material-database: Datos Maestros](./master-data/README.md)

Los datos maestros representan la información fundamental de tu negocio:

{% set master_data = [
    {
        'icon': 'material-account-group',
        'title': 'Clientes',
        'link': './master-data/client/README.md',
        'description': 'Información de clientes, contactos y direcciones.'
    },
    {
        'icon': 'material-cart',
        'title': 'Transacciones',
        'link': './master-data/transactions/README.md',
        'description': 'Compras realizadas por los clientes'
    },
    {
        'icon': 'material-package',
        'title': 'Productos',
        'link': './master-data/product/README.md',
        'description': 'Catálogo de productos y sus atributos'
    },
    {
        'icon': 'material-account-tie',
        'title': 'Vendedores',
        'link': './master-data/seller/README.md',
        'description': 'Equipo de ventas y fuerza comercial.'
    },
    {
        'icon': 'material-ticket-percent',
        'title': 'Cupones',
        'link': './master-data/coupon/README.md',
        'description': 'Sistema de promociones y descuentos'
    },
    {
        'icon': 'material-chart-timeline-variant',
        'title': 'Eventos',
        'link': './master-data/events/README.md',
        'description': 'Acciones y actividades del sistema para análisis retrospectivo'
    }
] %}

{{ section_overview(master_data) }}

### [:material-cog: Configuraciones](./settings/README.md)

Las configuraciones definen cómo funciona tu negocio en Reten:

{% set settings = [
    {
        'icon': 'material-account-multiple',
        'title': 'Asignaciones',
        'link': './settings/assignments/README.md',
        'description': 'Relaciones entre vendedores y clientes'
    },
    {
        'icon': 'material-map-marker-path',
        'title': 'Rutas',
        'link': './settings/routes/README.md',
        'description': 'Programación de visitas a clientes'
    }
] %}

{{ section_overview(settings) }}

## :material-step-forward-2: Flujo de Implementación

Para implementar la carga de datos en Reten, sigue estos pasos:

!!! note "1. Elige tu Método de Conexión"
    - **Archivo**: Ideal para sincronizaciones periódicas y sistemas legacy
    - **Base de Datos**: Ideal para datos en tiempo real y sistemas modernos

!!! note "2. Prepara tus Datos Maestros"
    - Define tus [Productos](../master-data/product/README.md)
    - Registra tus [Clientes](../master-data/client/README.md)
    - Informa tus [Vendedores](../master-data/seller/README.md) en caso de tener fuerza de venta

!!! note "3. Configura tu Sistema"
    - Define las [Asignaciones](../settings/assignments/README.md) entre vendedores y clientes
    - Configura las [Rutas](../settings/routes/README.md) de visita

!!! note "4. Implementa la Sincronización"
    - Configura la frecuencia de sincronización según tu método elegido
    - Monitorea los logs y confirmaciones de carga
    - Verifica que los datos se procesen correctamente
