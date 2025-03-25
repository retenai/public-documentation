# Documentación Técnica de Reten

Bienvenido a la documentación técnica de Reten. Esta documentación está diseñada para proporcionar información detallada sobre la estructura, funcionamiento y uso de nuestro sistema.

!!! tip "Acceso Rápido"
    - 🚀 [Comenzar ahora](#comenzando)
    - 📚 [Explorar APIs](master-data/client/README.md#apis)
    - 💡 [Ver ejemplos](master-data/client/README.md#ejemplos-de-uso)
    - 🤝 [Contribuir](#contribuir)

## Estructura de la Documentación

La documentación está organizada siguiendo el flujo natural de implementación y uso del sistema:

{% from 'includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set main_sections = [
    {
        'icon': 'material-database',
        'title': 'Datos Maestros',
        'link': 'master-data/README.md',
        'description': 'Gestión de clientes, productos, categorías y otros datos fundamentales del sistema.'
    },
    {
        'icon': 'material-cog',
        'title': 'Configuraciones',
        'link': 'settings/README.md',
        'description': 'Personalización del sistema, asignaciones, rutas y notificaciones.'
    },
    {
        'icon': 'material-checkbox-marked-circle',
        'title': 'Tareas',
        'link': 'tasks/README.md',
        'description': 'Sistema de tareas, seguimiento y gestión de actividades.'
    },
    {
        'icon': 'material-chart-timeline-variant',
        'title': 'Eventos',
        'link': 'events/README.md',
        'description': 'Seguimiento de acciones y eventos del sistema en tiempo real.'
    }
] %}

{{ section_overview(main_sections) }}

{% call section_cards('📊 Datos Maestros', 'master-data/README.md') %}
{{ feature_card(
    'material-store',
    'Clientes',
    'Establecimientos comerciales y sus características',
    'master-data/client/README.md'
) }}

{{ feature_card(
    'material-package',
    'Productos',
    'Catálogo de productos y sus atributos',
    'master-data/product/README.md'
) }}

{{ feature_card(
    'material-shape',
    'Categorías',
    'Clasificación jerárquica de productos',
    'master-data/category/README.md'
) }}

{{ feature_card(
    'material-account-tie',
    'Vendedores',
    'Usuarios del sistema y sus permisos',
    'master-data/seller/README.md'
) }}

{{ feature_card(
    'material-ticket-percent',
    'Cupones',
    'Sistema de promociones y descuentos',
    'master-data/coupon/README.md'
) }}

{{ feature_card(
    'material-cart',
    'Transacciones',
    'Compras realizadas por los clientes',
    'master-data/transactions/README.md'
) }}
{% endcall %}

{% call section_cards('⚙️ Configuraciones', 'settings/README.md') %}
{{ feature_card(
    'material-account-group',
    'Asignaciones',
    'Relaciones entre vendedores y clientes',
    'settings/assignments/README.md'
) }}

{{ feature_card(
    'material-map-marker-path',
    'Rutas',
    'Programación de visitas a clientes',
    'settings/routes/README.md'
) }}

{{ feature_card(
    'material-bell',
    'Suscripciones',
    'Gestión de comunicaciones y notificaciones',
    'settings/subscription/README.md'
) }}
{% endcall %}

{% call section_cards('✅ Tareas', 'tasks/README.md') %}
{{ feature_card(
    'material-clipboard-list',
    'Tareas',
    'Instrucciones y momentos oportunos para contactar clientes',
    'tasks/README.md'
) }}

{{ feature_card(
    'material-chart-timeline',
    'Seguimiento',
    'Estado y progreso de tareas',
    'tasks/tracking/README.md'
) }}
{% endcall %}

{% call section_cards('📈 Eventos', 'events/README.md') %}
{{ feature_card(
    'material-shopping',
    'Órdenes',
    'Eventos del ciclo de vida de órdenes',
    'events/order/README.md'
) }}

{{ feature_card(
    'material-truck-delivery',
    'Logística',
    'Eventos de preparación y entrega',
    'events/logistics/README.md'
) }}

{{ feature_card(
    'material-account',
    'Cuenta',
    'Eventos de gestión de usuarios',
    'events/account/README.md'
) }}

{{ feature_card(
    'material-heart',
    'Engagement',
    'Eventos de interacción y fidelización',
    'events/engagement/README.md'
) }}

{{ feature_card(
    'material-cart',
    'Carrito',
    'Eventos relacionados con el carrito de compras',
    'events/cart/README.md'
) }}

{{ feature_card(
    'material-navigation',
    'Navegación',
    'Eventos de interacción con la plataforma',
    'events/navigation/README.md'
) }}
{% endcall %}

{% call feature_highlights('✨ Características Principales') %}
{{ highlight_item(
    'material-book-open-variant',
    'Documentación Completa',
    'Cada sección está documentada con ejemplos y casos de uso'
) }}

{{ highlight_item(
    'material-check-decagram',
    'Validaciones',
    'Incluye reglas de validación y restricciones de datos'
) }}

{{ highlight_item(
    'material-code-tags',
    'Ejemplos',
    'Código de ejemplo para implementaciones comunes'
) }}

{{ highlight_item(
    'material-compass',
    'Guías',
    'Instrucciones paso a paso para integraciones'
) }}
{% endcall %}

## 🚀 Comenzando

Para implementar Reten de manera efectiva, sigue estos pasos:

!!! note "1. Configura tus Datos Maestros"
    - Define tus [Categorías](master-data/category/README.md) y [Productos](master-data/product/README.md)
    - Registra tus [Clientes](master-data/client/README.md) y [Vendedores](master-data/seller/README.md)

!!! note "2. Establece las Configuraciones"
    - Define las [Asignaciones](settings/assignments/README.md) entre vendedores y clientes
    - Configura las [Rutas](settings/routes/README.md) de visita
    - Gestiona las [Suscripciones](settings/subscription/README.md)

!!! note "3. Gestiona las Tareas"
    - Comprende el sistema de [Tareas](tasks/README.md)
    - Implementa el [Seguimiento](tasks/tracking/README.md)

!!! note "4. Monitorea los Eventos"
    - Integra los [Eventos](events/README.md) relevantes para tu negocio
    - Utiliza la información para optimizar el sistema

## 🤝 Contribuir

<div class="reten-container reten-container--bordered" markdown>
Si encuentras algún error o tienes sugerencias para mejorar la documentación, por favor:

1. Crea un issue en el repositorio
2. Propón cambios mediante pull requests
3. Contacta al equipo de desarrollo
</div>

## 💬 Soporte

<div class="reten-container" markdown>
Para obtener ayuda adicional:

- Revisa la documentación detallada de cada sección
- Contacta al equipo de soporte técnico
- Consulta los ejemplos de implementación
</div>
