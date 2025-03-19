# Eventos en Reten

Este directorio contiene la documentación detallada de todos los eventos que pueden ser enviados a Reten.

## Estructura General de Eventos

Todos los eventos en Reten comparten la siguiente estructura base:

```json
{
  "event_id": "string",         // Identificador único del evento
  "event_type": "string",       // Tipo de evento (ver categorías)
  "event_date": "timestamp",    // Fecha y hora del evento
  "user_id": "string",         // ID del usuario que realizó la acción
  "_created_at": "timestamp",   // Fecha de creación en Reten
  "_updated_at": "timestamp",   // Fecha de última actualización en Reten
  "data_object_id": "string",   // ID del objeto relacionado
  "attributes": [{             // Atributos adicionales del evento
    "key": "string",
    "value": "string"
  }]
}
```

## Categorías de Eventos

1. [Eventos de Venta](./sales_events.md)
   - Eventos relacionados con el proceso de compra
   - Tracking de carrito
   - Procesos de checkout y pago

2. [Eventos de Producto](./product_events.md)
   - Visualizaciones
   - Interacciones
   - Impresiones

3. [Eventos de Usuario](./user_events.md)
   - Creación de cuenta
   - Autorizaciones
   - Login/Logout

4. [Eventos de Entrega](./delivery_events.md)
   - Confirmaciones
   - Estados de envío
   - Tracking

5. [Eventos de Marketing](./marketing_events.md)
   - Suscripciones
   - Interacciones con campañas
   - Respuestas a comunicaciones

## Convenciones de Nomenclatura

### Tipos de Evento
- Los tipos de evento deben estar en `snake_case`
- Deben ser descriptivos y seguir el formato `objeto_accion`
- Ejemplos: `cart_started`, `product_viewed`, `order_confirmed`

### Atributos
- Las claves de atributos deben estar en `snake_case`
- Los valores deben seguir el tipo de dato especificado
- Fechas en formato ISO 8601
- Montos en números decimales (float64)

## Validación de Eventos

Cada evento pasa por un proceso de validación que verifica:
1. Estructura correcta según el tipo de evento
2. Presencia de campos requeridos
3. Tipos de datos correctos
4. Valores dentro de rangos permitidos

## Integración

Para enviar eventos a Reten, consulta la [documentación de integración](../integration/events_api.md) que incluye:
- Endpoints de la API
- Autenticación
- Manejo de errores
- Rate limits
- Ejemplos de implementación 