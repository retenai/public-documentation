# :material-package-variant: Órdenes

Esta sección documenta los eventos relacionados con el ciclo de vida de las órdenes, desde su creación hasta su finalización.

## Descripción General

Los eventos de órdenes capturan el ciclo de vida completo de una orden desde su creación hasta su finalización. Estos eventos son fundamentales para:

- Monitorear el estado y progreso de las órdenes
- Analizar patrones de compra y comportamiento de usuarios
- Identificar problemas en el proceso de compra
- Gestionar el proceso de fulfillment
- Mantener informados a los usuarios sobre el estado de sus pedidos

## Eventos Disponibles

La siguiente tabla muestra un resumen de todos los eventos disponibles en esta sección:

| Evento                                        | Tipo                   | Descripción                  | Trigger                  |
| --------------------------------------------- | ---------------------- | ---------------------------- | ------------------------ |
| [Creación de Orden](#creacion-de-orden)       | `order_created`        | Registro de nueva orden      | Checkout completado      |
| [Confirmación de Pago](#confirmacion-de-pago) | `payment_confirmed`    | Confirmación de pago exitoso | Pago procesado           |
| [Cambio de Estado](#cambio-de-estado)         | `order_status_changed` | Actualización de estado      | Cambio en la orden       |
| [Cancelación](#cancelacion-de-orden)          | `order_cancelled`      | Cancelación de orden         | Solicitud de cancelación |

---

### Creación de Orden

!!! info "`order_created`"
    **Trigger**  
    Se dispara cuando se completa exitosamente el proceso de checkout

    **Descripción**  
    Registra la creación exitosa de una nueva orden después de completar el proceso de checkout y confirmar el pago.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "order_created",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "customer_id": "string",
    "total_amount": "number",
    "subtotal": "number",
    "tax": "number",
    "shipping_cost": "number",
    "discount_amount": "number",
    "currency": "string",
    "payment_method": "string",
    "shipping_method": "string",
    "items": [
      {
        "product_id": "string",
        "variant_id": "string",
        "quantity": "number",
        "unit_price": "number",
        "total_price": "number"
      }
    ],
    "shipping_address": {
      "street": "string",
      "city": "string",
      "state": "string",
      "country": "string",
      "postal_code": "string"
    }
  },
  "metadata": {
    "session_id": "string",
    "user_type": "guest|registered",
    "platform": "web|mobile|pos"
  }
}
```

**Campos Específicos:**

| Campo              | Tipo   | Requerido | Descripción                         |
| ------------------ | ------ | --------- | ----------------------------------- |
| `order_id`         | string | Sí        | Identificador único de la orden     |
| `customer_id`      | string | Sí        | Identificador del cliente           |
| `total_amount`     | number | Sí        | Monto total de la orden             |
| `subtotal`         | number | Sí        | Subtotal antes de impuestos y envío |
| `tax`              | number | Sí        | Monto de impuestos                  |
| `shipping_cost`    | number | Sí        | Costo de envío                      |
| `discount_amount`  | number | No        | Monto total de descuentos aplicados |
| `items`            | array  | Sí        | Lista de productos en la orden      |
| `shipping_address` | object | Sí        | Dirección de envío                  |

**Ejemplo de Uso:**
```json
{
  "event_type": "order_created",
  "data": {
    "order_id": "ORD_123",
    "customer_id": "CUST_456",
    "total_amount": 1500.00,
    "items": [
      {
        "product_id": "PROD_789",
        "quantity": 2,
        "unit_price": 750.00
      }
    ]
  }
}
```

---

### Confirmación de Pago

!!! success "`payment_confirmed`"
    **Trigger**  
    Se dispara cuando se recibe la confirmación del procesador de pagos

    **Descripción**  
    Registra la confirmación exitosa del pago de una orden.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "payment_confirmed",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "payment_id": "string",
    "amount": "number",
    "currency": "string",
    "payment_method": "string",
    "payment_status": "approved|pending|rejected",
    "transaction_id": "string",
    "installments": "number"
  },
  "metadata": {
    "payment_processor": "string",
    "payment_type": "string"
  }
}
```

**Campos Específicos:**

| Campo            | Tipo   | Requerido | Descripción                      |
| ---------------- | ------ | --------- | -------------------------------- |
| `payment_id`     | string | Sí        | Identificador del pago           |
| `amount`         | number | Sí        | Monto del pago                   |
| `payment_status` | string | Sí        | Estado del pago                  |
| `transaction_id` | string | Sí        | ID de transacción del procesador |
| `installments`   | number | No        | Número de cuotas                 |

**Ejemplo de Uso:**
```json
{
  "event_type": "payment_confirmed",
  "data": {
    "order_id": "ORD_123",
    "payment_id": "PAY_789",
    "amount": 1500.00,
    "payment_status": "approved",
    "transaction_id": "TXN_456"
  }
}
```

---

### Cambio de Estado

!!! note "`order_status_changed`"
    **Trigger**  
    Se dispara cuando cambia el estado de una orden

    **Descripción**  
    Registra cualquier cambio en el estado de una orden durante su ciclo de vida.

**Estados Posibles:**
- `pending` - Orden creada, pendiente de procesamiento
- `processing` - En proceso de preparación
- `shipped` - Enviada al cliente
- `delivered` - Entregada al cliente
- `cancelled` - Cancelada
- `refunded` - Reembolsada

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "order_status_changed",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "previous_status": "string",
    "new_status": "string",
    "reason": "string",
    "notes": "string",
    "changed_by": "string"
  },
  "metadata": {
    "requires_notification": "boolean",
    "automatic_change": "boolean"
  }
}
```

**Campos Específicos:**

| Campo             | Tipo   | Requerido | Descripción                             |
| ----------------- | ------ | --------- | --------------------------------------- |
| `previous_status` | string | Sí        | Estado anterior de la orden             |
| `new_status`      | string | Sí        | Nuevo estado de la orden                |
| `reason`          | string | No        | Razón del cambio de estado              |
| `notes`           | string | No        | Notas adicionales sobre el cambio       |
| `changed_by`      | string | Sí        | Usuario o sistema que realizó el cambio |

**Ejemplo de Uso:**
```json
{
  "event_type": "order_status_changed",
  "data": {
    "order_id": "ORD_123",
    "previous_status": "pending",
    "new_status": "processing",
    "changed_by": "system"
  }
}
```

---

### Cancelación de Orden

!!! warning "`order_cancelled`"
    **Trigger**  
    Se dispara cuando se cancela una orden

    **Descripción**  
    Registra la cancelación de una orden, ya sea por el cliente o por el sistema.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "order_cancelled",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "cancellation_reason": "string",
    "cancelled_by": "customer|system|merchant",
    "refund_amount": "number",
    "refund_status": "pending|processed|rejected",
    "items_status": [
      {
        "product_id": "string",
        "status": "cancelled|returned|restocked"
      }
    ]
  },
  "metadata": {
    "requires_refund": "boolean",
    "restock_items": "boolean"
  }
}
```

**Campos Específicos:**

| Campo                 | Tipo   | Requerido | Descripción                    |
| --------------------- | ------ | --------- | ------------------------------ |
| `cancellation_reason` | string | Sí        | Motivo de la cancelación       |
| `cancelled_by`        | string | Sí        | Quién realizó la cancelación   |
| `refund_amount`       | number | No        | Monto a reembolsar             |
| `refund_status`       | string | No        | Estado del reembolso           |
| `items_status`        | array  | Sí        | Estado de los items cancelados |

**Ejemplo de Uso:**
```json
{
  "event_type": "order_cancelled",
  "data": {
    "order_id": "ORD_123",
    "cancellation_reason": "customer_request",
    "cancelled_by": "customer",
    "refund_amount": 1500.00
  }
}
```

---

## Consideraciones Técnicas

- Los eventos de orden deben ser idempotentes
- Se debe mantener consistencia entre el estado de la orden y los pagos
- Los cambios de estado deben seguir un flujo predefinido
- Las cancelaciones deben manejar correctamente los reembolsos
- Se debe implementar un sistema de reintentos para eventos críticos
- Los eventos deben respetar las políticas de privacidad de datos

## Casos de Uso Comunes

### Análisis de Conversión

Utiliza los eventos `order_created` y `payment_confirmed` para:

1. Calcular tasas de conversión por canal
2. Identificar patrones de compra exitosos
3. Optimizar el proceso de checkout

### Monitoreo de Estados

Utiliza el evento `order_status_changed` para:

1. Identificar cuellos de botella en el proceso
2. Analizar tiempos de procesamiento
3. Mejorar la comunicación con el cliente

### Gestión de Cancelaciones

Utiliza el evento `order_cancelled` para:

1. Analizar causas comunes de cancelación
2. Optimizar políticas de stock
3. Mejorar procesos de reembolso

## Preguntas Frecuentes

### ¿Cómo manejar órdenes parcialmente canceladas?
Utilizar el evento `order_status_changed` con estados específicos para items individuales.

### ¿Qué hacer si un evento de orden falla?
Implementar un sistema de cola de reintentos con backoff exponencial.

### ¿Cómo sincronizar estados entre múltiples sistemas?
Utilizar un sistema de eventos distribuido con garantía de orden.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Estados de Orden](../integration/order_states.md)
- [Políticas de Cancelación](../integration/cancellation_policies.md)