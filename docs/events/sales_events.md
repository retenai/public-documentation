# Eventos de Venta

Los eventos de venta registran todas las interacciones relacionadas con el proceso de compra, desde la creación del carrito hasta la confirmación del pago.

## Eventos Disponibles

### `cart_started`

Registra el inicio de un nuevo carrito de compras.

**Cuándo enviarlo:**
- Al crear un nuevo carrito de compras
- Al iniciar una nueva sesión de compra

**Estructura específica:**
```json
{
  // Campos base obligatorios
  "event_type": "cart_started",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  // Datos específicos del evento
  "event_data": {
    "cart_id": "string",
    "source": "string",      // web, app, pos
    "initial_items": [{      // Opcional
      "item_id": "string",
      "quantity": "float64"
    }]
  }
}
```

**Campos Requeridos:**
- `cart_id`: Identificador único del carrito
- `source`: Origen del carrito

---

### `product_add_to_cart`

Registra la adición de un producto al carrito.

**Cuándo enviarlo:**
- Cada vez que se agrega un producto al carrito
- Cuando se modifica la cantidad de un producto existente (incremento)

**Estructura específica:**
```json
{
  "event_type": "product_add_to_cart",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "cart_id": "string",
    "item": {
      "item_id": "string",
      "name": "string",
      "price": "float64",
      "quantity": "float64",
      "category": "string",
      "attributes": [{
        "key": "string",
        "value": "string"
      }]
    },
    "cart_state": {
      "total_items": "integer",
      "total_amount": "float64"
    }
  }
}
```

**Campos Requeridos:**
- `cart_id`: Identificador del carrito
- `item`: Información completa del producto agregado
- `cart_state`: Estado actual del carrito

---

### `checkout_started`

Registra el inicio del proceso de checkout.

**Cuándo enviarlo:**
- Cuando el usuario inicia el proceso de checkout
- Al entrar a la página/vista de checkout

**Estructura específica:**
```json
{
  "event_type": "checkout_started",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "cart_id": "string",
    "pricing": {
      "net_amount": "float64",
      "final_amount": "float64",
      "discount_amount": "float64",
      "shipping_amount": "float64",
      "tax_amount": "float64"
    },
    "items": [{
      "item_id": "string",
      "name": "string",
      "price": "float64",
      "quantity": "float64",
      "category": "string"
    }],
    "coupon_code": "string"
  }
}
```

**Campos Requeridos:**
- `cart_id`: Identificador del carrito
- `pricing`: Desglose de precios
- `items`: Lista de productos en el carrito

---

### `payment_completed`

Registra la finalización exitosa de un pago.

**Cuándo enviarlo:**
- Cuando se confirma el pago exitoso
- Después de recibir la confirmación del procesador de pagos

**Estructura específica:**
```json
{
  "event_type": "payment_completed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "order_id": "string",
    "cart_id": "string",
    "payment_details": {
      "method": "string",
      "amount": "float64",
      "status": "string",
      "date": "timestamp"
    },
    "billing_address": {
      "city": "string",
      "name": "string",
      "street": "string",
      "commune": "string",
      "region": "string",
      "number": "string",
      "dept_number": "string"
    },
    "shipping_address": {
      "city": "string",
      "name": "string",
      "street": "string",
      "commune": "string",
      "region": "string",
      "number": "string",
      "dept_number": "string"
    }
  }
}
```

**Campos Requeridos:**
- `order_id`: Identificador de la orden
- `payment_details`: Detalles completos del pago
- `billing_address`: Dirección de facturación

## Flujo de Eventos

El flujo típico de eventos de venta sigue este orden:

1. `cart_started`
2. `product_add_to_cart` (puede ocurrir múltiples veces)
3. `product_remove_cart` (opcional)
4. `checkout_started`
5. `payment_initiated`
6. `payment_completed` o `payment_failed`
7. `order_created` (si el pago es exitoso)
8. `order_confirmed`

## Validaciones Específicas

Cada evento de venta debe cumplir con las siguientes validaciones:

1. **Consistencia de IDs**
   - `cart_id` debe ser consistente en todos los eventos relacionados
   - `order_id` debe ser único por orden

2. **Montos**
   - Todos los montos deben ser positivos
   - `final_amount` debe ser igual a `net_amount` - `discount_amount` + `shipping_amount` + `tax_amount`

3. **Temporalidad**
   - Los eventos deben seguir una secuencia temporal lógica
   - Las fechas no pueden ser futuras

## Ejemplos de Implementación

### JavaScript
```javascript
const sendSalesEvent = async (eventType, eventData) => {
  await retenClient.events.track({
    event_type: eventType,
    event_date: new Date().toISOString(),
    user_id: getCurrentUserId(),
    event_data: eventData
  });
};
```

### Python
```python
def send_sales_event(event_type, event_data):
    reten_client.events.track(
        event_type=event_type,
        event_date=datetime.now().isoformat(),
        user_id=get_current_user_id(),
        event_data=event_data
    )
``` 