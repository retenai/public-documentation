# :material-database: Datos Maestros

Los datos maestros representan la información fundamental y de referencia en el sistema Reten. Estas entidades constituyen el núcleo de datos que es compartido y reutilizado a través de los diferentes procesos y operaciones del negocio, asegurando consistencia y uniformidad en toda la plataforma.

{% from '/includes/cards.md' import section_overview %}

{% set master_data = [
    {
        'icon': 'material-store',
        'title': 'Clientes',
        'link': 'client/README.md',
        'description': 'Establecimientos comerciales y sus características'
    },
    {
        'icon': 'material-package',
        'title': 'Productos',
        'link': 'product/README.md',
        'description': 'Catálogo de productos y sus atributos'
    },
    {
        'icon': 'material-account-tie',
        'title': 'Vendedores',
        'link': 'seller/README.md',
        'description': 'Usuarios del sistema y sus permisos'
    },
    {
        'icon': 'material-ticket-percent',
        'title': 'Cupones',
        'link': 'coupon/README.md',
        'description': 'Sistema de promociones y descuentos'
    },
    {
        'icon': 'material-cart',
        'title': 'Transacciones',
        'link': 'transactions/README.md',
        'description': 'Compras realizadas por los clientes'
    },
    {
        'icon': 'material-chart-timeline-variant',
        'title': 'Eventos',
        'link': 'events/README.md',
        'description': 'Acciones y actividades del sistema para análisis retrospectivo'
    }
] %}

{{ section_overview(master_data) }}

## Características Comunes

Todas las entidades de datos maestros comparten:

- Identificación mediante ID de cliente
- Trazabilidad temporal (fechas de creación/actualización)
- Extensibilidad mediante atributos personalizados

Para asegurar la consistencia en la documentación y estructura de las entidades, se proporciona una [plantilla](./_template.md) que debe seguirse al crear nuevas entidades master-data.

## Convenciones Generales

- Campos en `snake_case`
- Fechas en formato ISO 8601 (UTC)
- IDs descriptivos y únicos por entidad
- Campos requeridos marcados como `not null`
- Referencias mediante IDs para mantener integridad referencial

Para más detalles sobre cada entidad, consulta su documentación específica en los enlaces proporcionados.