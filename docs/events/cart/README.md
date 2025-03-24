# Eventos de Carrito y Checkout

Esta sección documenta los eventos relacionados con el proceso de compra, desde la gestión del carrito hasta la finalización del checkout.

## Descripción General

Los eventos de carrito y checkout capturan las interacciones de los usuarios durante el proceso de compra. Estos eventos son esenciales para:
- Analizar el comportamiento de compra de los usuarios
- Identificar puntos de fricción en el proceso de checkout
- Detectar y reducir el abandono de carrito
- Optimizar la conversión de ventas
- Mejorar la experiencia de compra

## Eventos Disponibles

### Agregar al Carrito

**Tipo de Evento:** `add_to_cart`

**Descripción:**  
Registra cuando un usuario agrega un producto a su carrito de compras. Este evento se genera cada vez que se añade un nuevo producto o se modifica la cantidad de un producto existente (incremento).

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "add_to_cart",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "product_id": "string",
    "product_name": "string",
    "category_id": "string",
    "quantity": "number",
    "price": "number",
    "currency": "string",
    "variant_id": "string",
    "source_page": "product|search|category|recommendation"
  },
  "metadata": {
    "session_id": "string",
    "cart_id": "string"
  }
}
```

**Campos Específicos:**
| Campo        | Tipo   | Requerido | Descripción                               |
| ------------ | ------ | --------- | ----------------------------------------- |
| product_id   | string | Sí        | Identificador único del producto          |
| product_name | string | Sí        | Nombre del producto                       |
| category_id  | string | Sí        | Identificador de la categoría             |
| quantity     | number | Sí        | Cantidad agregada al carrito              |
| price        | number | Sí        | Precio unitario del producto              |
| currency     | string | Sí        | Moneda del precio                         |
| variant_id   | string | No        | Identificador de la variante del producto |
| source_page  | string | Sí        | Página desde donde se agregó el producto  |

### Remover del Carrito

**Tipo de Evento:** `remove_from_cart`

**Descripción:**  
Captura cuando un usuario remueve un producto de su carrito o reduce su cantidad.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "remove_from_cart",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "product_id": "string",
    "product_name": "string",
    "quantity": "number",
    "removal_type": "complete|partial",
    "remaining_quantity": "number"
  },
  "metadata": {
    "session_id": "string",
    "cart_id": "string"
  }
}
```

**Campos Específicos:**
| Campo              | Tipo   | Requerido | Descripción                           |
| ------------------ | ------ | --------- | ------------------------------------- |
| product_id         | string | Sí        | Identificador único del producto      |
| product_name       | string | Sí        | Nombre del producto                   |
| quantity           | number | Sí        | Cantidad removida del carrito         |
| removal_type       | string | Sí        | Tipo de remoción (completa o parcial) |
| remaining_quantity | number | No        | Cantidad que queda en el carrito      |

### Inicio de Checkout

**Tipo de Evento:** `begin_checkout`

**Descripción:**  
Registra cuando un usuario inicia el proceso de checkout desde su carrito.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "begin_checkout",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "cart_id": "string",
    "total_items": "number",
    "total_amount": "number",
    "currency": "string",
    "products": [
      {
        "product_id": "string",
        "quantity": "number",
        "price": "number"
      }
    ],
    "has_promocode": "boolean"
  },
  "metadata": {
    "session_id": "string",
    "user_type": "guest|registered"
  }
}
```

**Campos Específicos:**
| Campo         | Tipo    | Requerido | Descripción                               |
| ------------- | ------- | --------- | ----------------------------------------- |
| cart_id       | string  | Sí        | Identificador único del carrito           |
| total_items   | number  | Sí        | Cantidad total de items                   |
| total_amount  | number  | Sí        | Monto total del carrito                   |
| currency      | string  | Sí        | Moneda del total                          |
| products      | array   | Sí        | Lista de productos en el carrito          |
| has_promocode | boolean | No        | Indica si hay código promocional aplicado |

### Selección de Método de Pago

**Tipo de Evento:** `select_payment`

**Descripción:**  
Captura cuando un usuario selecciona un método de pago durante el checkout.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "select_payment",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "payment_method": "string",
    "payment_type": "credit_card|debit_card|transfer|crypto|other",
    "installments": "number",
    "provider": "string"
  },
  "metadata": {
    "session_id": "string",
    "cart_id": "string"
  }
}
```

**Campos Específicos:**
| Campo          | Tipo   | Requerido | Descripción                    |
| -------------- | ------ | --------- | ------------------------------ |
| payment_method | string | Sí        | Método de pago seleccionado    |
| payment_type   | string | Sí        | Tipo de método de pago         |
| installments   | number | No        | Número de cuotas seleccionadas |
| provider       | string | Sí        | Proveedor del método de pago   |

### Aplicación de Cupón

**Tipo de Evento:** `apply_coupon`

**Descripción:**  
Registra cuando un usuario aplica un cupón de descuento a su carrito.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "apply_coupon",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "coupon_code": "string",
    "discount_type": "percentage|fixed|shipping",
    "discount_value": "number",
    "original_amount": "number",
    "final_amount": "number",
    "currency": "string"
  },
  "metadata": {
    "session_id": "string",
    "cart_id": "string"
  }
}
```

**Campos Específicos:**
| Campo           | Tipo   | Requerido | Descripción                        |
| --------------- | ------ | --------- | ---------------------------------- |
| coupon_code     | string | Sí        | Código del cupón aplicado          |
| discount_type   | string | Sí        | Tipo de descuento                  |
| discount_value  | number | Sí        | Valor del descuento                |
| original_amount | number | Sí        | Monto original antes del descuento |
| final_amount    | number | Sí        | Monto final después del descuento  |

## Consideraciones Técnicas

- Los eventos deben validar la consistencia de los montos y cantidades
- Se debe mantener la atomicidad en las operaciones de carrito
- Los eventos de carrito deben persistir entre sesiones
- Se recomienda implementar un sistema de recuperación de carrito abandonado
- Los datos sensibles de pago nunca deben incluirse en los eventos
- Se debe validar la disponibilidad de stock al procesar eventos de carrito

## Casos de Uso Comunes

### Análisis de Abandono de Carrito
Utiliza los eventos `add_to_cart` y `begin_checkout` para:
1. Identificar productos con alta tasa de abandono
2. Analizar el momento del abandono en el proceso
3. Implementar estrategias de recuperación

### Optimización de Métodos de Pago
Utiliza el evento `select_payment` para:
1. Identificar métodos de pago preferidos
2. Analizar tasas de conversión por método
3. Optimizar la oferta de medios de pago

### Efectividad de Promociones
Utiliza el evento `apply_coupon` para:
1. Medir el impacto de diferentes tipos de descuento
2. Analizar el comportamiento de uso de cupones
3. Optimizar estrategias promocionales

## Preguntas Frecuentes

### ¿Cómo manejar timeouts en el proceso de checkout?
Implementar un sistema de heartbeat y validación de sesión activa.

### ¿Cómo gestionar conflictos de stock durante el checkout?
Realizar validaciones en tiempo real y notificar al usuario inmediatamente.

### ¿Cómo manejar múltiples dispositivos/pestañas?
Implementar sincronización en tiempo real del estado del carrito.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Implementación de Checkout](../integration/checkout_implementation.md)
- [Mejores Prácticas de Carrito](../integration/cart_best_practices.md)
