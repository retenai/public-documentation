# :material-chart-timeline-variant: Eventos

Los eventos en Reten representan acciones y cambios significativos que ocurren en la plataforma del cliente. Estos eventos capturan el journey completo del cliente final, desde la navegación inicial hasta la entrega del producto.

{% from '/includes/cards.md' import feature_card, section_cards %}

{% call section_cards() %}
{{ feature_card(
    'material-package-variant',
    'Órdenes',
    'Eventos del ciclo de vida de órdenes',
    'orders/README.md'
) }}

{{ feature_card(
    'material-truck-delivery',
    'Logística',
    'Eventos de preparación y entrega',
    'logistics/README.md'
) }}

{{ feature_card(
    'material-account',
    'Cuenta',
    'Eventos de gestión de usuarios',
    'account/README.md'
) }}

{{ feature_card(
    'material-heart',
    'Engagement',
    'Eventos de interacción y fidelización',
    'engagement/README.md'
) }}

{{ feature_card(
    'material-cart',
    'Carrito',
    'Eventos relacionados con el carrito de compras',
    'cart/README.md'
) }}

{{ feature_card(
    'material-navigation',
    'Navegación',
    'Eventos de interacción con la plataforma',
    'navigation/README.md'
) }}
{% endcall %}

## Estructura General

Todos los eventos comparten una estructura base:

```json
{
  "event_id": "string",        // Identificador único del evento
  "event_type": "string",      // Tipo de evento
  "timestamp": "string",       // Fecha y hora del evento
  "source": "string",          // Origen del evento
  "data": {},                  // Datos específicos del evento
  "metadata": {}               // Metadatos adicionales
}
```

## Consumo de Eventos

Los eventos pueden ser consumidos a través de:
- Webhooks
- API REST
- Streaming en tiempo real

Para más detalles sobre la integración con eventos, consulta la [documentación de la API de eventos](../integration/events_api.md). 