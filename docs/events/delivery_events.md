# Eventos de Entrega

Los eventos de entrega registran todas las interacciones relacionadas con el proceso de entrega de productos, desde la confirmación hasta la entrega final.

## Eventos Disponibles

### `delivery_confirmed`

Registra la confirmación de una entrega por parte del comercio.

**Cuándo enviarlo:**
- Cuando el comercio confirma que puede realizar la entrega
- Al asignar un pedido a un repartidor
- Al generar una guía de despacho

**Estructura específica:**
```json
{
  "event_type": "delivery_confirmed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "order": {
      "order_id": "string",
      "order_date": "timestamp"
    },
    "delivery": {
      "delivery_id": "string",
      "type": "string",          // own_fleet, third_party, pickup
      "estimated_date": "timestamp",
      "time_slot": {
        "start": "timestamp",
        "end": "timestamp"
      }
    },
    "shipping_details": {
      "address": {
        "street": "string",
        "number": "string",
        "city": "string",
        "region": "string",
        "country": "string",
        "postal_code": "string",
        "coordinates": {
          "lat": "float64",
          "lng": "float64"
        }
      },
      "contact": {
        "name": "string",
        "phone": "string",
        "email": "string"
      }
    }
  }
}
```

**Campos Requeridos:**
- `order`: Información de la orden
- `delivery`: Detalles de la entrega
- `shipping_details`: Información de envío

---

### `delivery_status_updated`

Registra actualizaciones en el estado de una entrega.

**Cuándo enviarlo:**
- Cada vez que cambia el estado de la entrega
- Al recibir actualizaciones del servicio de delivery
- Al actualizar la ubicación o ETA

**Estructura específica:**
```json
{
  "event_type": "delivery_status_updated",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "delivery": {
      "delivery_id": "string",
      "order_id": "string"
    },
    "status": {
      "code": "string",         // preparing, in_transit, delivered, failed
      "description": "string",
      "timestamp": "timestamp",
      "location": {             // opcional
        "lat": "float64",
        "lng": "float64",
        "address": "string"
      }
    },
    "metadata": {
      "updated_by": "string",   // system, driver, admin
      "reason_code": "string",  // opcional
      "notes": "string"         // opcional
    }
  }
}
```

**Campos Requeridos:**
- `delivery`: Identificadores de la entrega
- `status`: Nuevo estado y detalles
- `metadata`: Información sobre la actualización

---

### `delivery_completed`

Registra la finalización exitosa de una entrega.

**Cuándo enviarlo:**
- Cuando se confirma la entrega al cliente
- Al recibir confirmación de recepción
- Al completar una entrega en punto de retiro

**Estructura específica:**
```json
{
  "event_type": "delivery_completed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "delivery": {
      "delivery_id": "string",
      "order_id": "string",
      "completion_time": "timestamp"
    },
    "confirmation": {
      "type": "string",         // signature, photo, code
      "proof": {
        "url": "string",        // opcional
        "type": "string",       // opcional
        "verified": "boolean"
      },
      "receiver": {
        "name": "string",
        "relationship": "string" // self, family, doorman
      }
    },
    "rating": {                 // opcional
      "score": "integer",
      "comments": "string"
    }
  }
}
```

**Campos Requeridos:**
- `delivery`: Información de la entrega completada
- `confirmation`: Detalles de la confirmación de entrega

---

### `delivery_failed`

Registra cuando una entrega no puede completarse.

**Cuándo enviarlo:**
- Cuando no se puede realizar la entrega
- Al reportar un problema en la entrega
- Cuando el cliente no está disponible

**Estructura específica:**
```json
{
  "event_type": "delivery_failed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "delivery": {
      "delivery_id": "string",
      "order_id": "string",
      "attempt_number": "integer"
    },
    "failure": {
      "reason_code": "string",    // no_one_home, wrong_address, rejected
      "description": "string",
      "evidence": {               // opcional
        "type": "string",
        "url": "string"
      }
    },
    "resolution": {
      "action": "string",         // reschedule, return, cancel
      "notes": "string",
      "next_attempt": "timestamp" // opcional
    }
  }
}
```

**Campos Requeridos:**
- `delivery`: Información de la entrega
- `failure`: Razón del fallo
- `resolution`: Plan de acción

## Flujo de Eventos

El flujo típico de una entrega sigue este orden:

1. `delivery_confirmed`
2. `delivery_status_updated` (múltiples actualizaciones)
3. `delivery_completed` o `delivery_failed`

## Validaciones Específicas

1. **Consistencia de IDs**
   - `delivery_id` debe ser único
   - `order_id` debe corresponder a una orden existente

2. **Estados**
   - Las transiciones de estado deben ser válidas
   - No se pueden saltar estados obligatorios

3. **Temporalidad**
   - Las fechas deben seguir una secuencia lógica
   - Los timestamps deben ser consistentes con el flujo

## Ejemplos de Implementación

### JavaScript
```javascript
const sendDeliveryEvent = async (eventType, deliveryData) => {
  await retenClient.events.track({
    event_type: eventType,
    event_date: new Date().toISOString(),
    user_id: getCurrentUserId(),
    event_data: {
      ...deliveryData,
      metadata: {
        source_system: getSystemInfo(),
        tracking_enabled: isTrackingEnabled()
      }
    }
  });
};
```

### Python
```python
def send_delivery_event(event_type, delivery_data):
    reten_client.events.track(
        event_type=event_type,
        event_date=datetime.now().isoformat(),
        user_id=get_current_user_id(),
        event_data={
            **delivery_data,
            "metadata": {
                "source_system": get_system_info(),
                "tracking_enabled": is_tracking_enabled()
            }
        }
    )
``` 