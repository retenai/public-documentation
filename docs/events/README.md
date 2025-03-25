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

## Tipos de Eventos

### [Navegación y Descubrimiento](./navigation/README.md)
Eventos relacionados con la exploración de la plataforma:

- Vistas de página
- Búsquedas realizadas
- Visualización de productos
- Filtrado por categorías
- Interacción con banners/promociones
- Tiempo de permanencia

### [Carrito y Checkout](./cart/README.md)
Eventos del proceso de compra:

- Gestión del carrito (agregar/remover productos)
- Modificación de cantidades
- Proceso de checkout
- Aplicación de cupones
- Selección de métodos de pago y envío

### [Órdenes](./orders/README.md)
Eventos relacionados con órdenes:

- Creación de orden
- Procesamiento de pagos
- Confirmaciones
- Cambios de estado
- Cancelaciones y devoluciones

### [Logística y Fulfillment](./logistics/README.md)
Eventos del proceso de preparación y entrega:

- Asignación a centro de distribución
- Picking y packing
- Despacho y tracking
- Entrega y confirmaciones
- Reagendamientos

### [Cuenta y Perfil](./account/README.md)
Eventos de gestión de usuarios:

- Registro y autenticación
- Actualización de datos personales
- Gestión de preferencias
- Administración de direcciones
- Métodos de pago

### [Engagement](./engagement/README.md)
Eventos de interacción y fidelización:

- Listas de deseos
- Reseñas y calificaciones
- Compartir productos
- Interacciones con soporte
- Respuestas a encuestas

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