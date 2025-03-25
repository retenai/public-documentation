# :material-database: Datos Maestros

Los datos maestros representan la información fundamental y de referencia en el sistema Reten. Estas entidades constituyen el núcleo de datos que es compartido y reutilizado a través de los diferentes procesos y operaciones del negocio, asegurando consistencia y uniformidad en toda la plataforma.

{% from '/includes/cards.md' import feature_card, section_cards %}

{% call section_cards() %}
{{ feature_card(
    'material-store',
    'Clientes',
    'Establecimientos comerciales y sus características',
    'client/README.md'
) }}

{{ feature_card(
    'material-package',
    'Productos',
    'Catálogo de productos y sus atributos',
    'product/README.md'
) }}

{{ feature_card(
    'material-shape',
    'Categorías',
    'Clasificación jerárquica de productos',
    'category/README.md'
) }}

{{ feature_card(
    'material-account-tie',
    'Vendedores',
    'Usuarios del sistema y sus permisos',
    'seller/README.md'
) }}

{{ feature_card(
    'material-ticket-percent',
    'Cupones',
    'Sistema de promociones y descuentos',
    'coupon/README.md'
) }}

{{ feature_card(
    'material-cart',
    'Transacciones',
    'Compras realizadas por los clientes',
    'transactions/README.md'
) }}
{% endcall %}

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