# Eventos

Los eventos en Reten representan acciones y cambios significativos que ocurren en el sistema. Cada tipo de evento tiene su propia estructura y propósito específico.

## Tipos de Eventos

### [Marketing](./marketing_events.md)
Eventos relacionados con campañas y comunicaciones:
- Envío de comunicaciones
- Respuestas y engagement
- Seguimiento de campañas

### [Ventas](./sales_events.md)
Eventos del proceso de venta:
- Creación de órdenes
- Confirmaciones de pago
- Estados de transacción

### [Productos](./product_events.md)
Eventos relacionados con productos:
- Actualizaciones de catálogo
- Cambios de precio
- Gestión de inventario

### [Usuario](./user_events.md)
Eventos de interacción de usuarios:
- Registro y autenticación
- Actualizaciones de perfil
- Preferencias y configuraciones

### [Entregas](./delivery_events.md)
Eventos del proceso de entrega:
- Estados de envío
- Confirmaciones de entrega
- Incidencias logísticas

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