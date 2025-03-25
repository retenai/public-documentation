# :material-truck-delivery: Logística

Esta sección documenta los eventos relacionados con el proceso de preparación y entrega de pedidos.

## Descripción General

Los eventos de logística y fulfillment capturan todo el proceso de preparación y entrega de pedidos, desde la asignación al centro de distribución hasta la entrega final al cliente. Estos eventos son fundamentales para:

- Monitorear el proceso de preparación de pedidos
- Realizar seguimiento en tiempo real de envíos
- Optimizar rutas y tiempos de entrega
- Gestionar la capacidad de los centros de distribución
- Mantener informados a los clientes sobre el estado de sus pedidos
- Analizar y mejorar la eficiencia operacional

## Eventos Disponibles

La siguiente tabla muestra un resumen de todos los eventos disponibles en esta sección:

| Evento                                                                      | Tipo                 | Descripción              | Trigger                      |
| --------------------------------------------------------------------------- | -------------------- | ------------------------ | ---------------------------- |
| [Asignación a Centro de Distribución](#asignacion-a-centro-de-distribucion) | `warehouse_assigned` | Asignación de orden a CD | Orden lista para preparación |
| [Inicio de Picking](#inicio-de-picking)                                     | `picking_started`    | Inicio de recolección    | Orden asignada a picker      |
| [Picking Completado](#picking-completado)                                   | `picking_completed`  | Fin de recolección       | Picking finalizado           |
| [Packing Iniciado](#packing-iniciado)                                       | `packing_started`    | Inicio de empaque        | Picking completado           |
| [Orden Despachada](#orden-despachada)                                       | `order_dispatched`   | Entrega a transportista  | Packing completado           |

---

### Asignación a Centro de Distribución

!!! info "`warehouse_assigned`"
    **Trigger**  
    Se dispara cuando una orden es asignada a un centro de distribución específico para su preparación

    **Descripción**  
    Registra cuando una orden es asignada a un centro de distribución específico para su preparación.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "warehouse_assigned",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "warehouse_id": "string",
    "warehouse_name": "string",
    "assignment_type": "automatic|manual",
    "estimated_preparation_time": "number",
    "priority_level": "high|medium|low",
    "items": [
      {
        "product_id": "string",
        "quantity": "number",
        "stock_location": "string"
      }
    ]
  },
  "metadata": {
    "assignment_reason": "string",
    "stock_availability": "boolean"
  }
}
```

**Campos Específicos:**

| Campo                        | Tipo   | Requerido | Descripción                               |
| ---------------------------- | ------ | --------- | ----------------------------------------- |
| `warehouse_id`               | string | Sí        | Identificador del centro de distribución  |
| `warehouse_name`             | string | Sí        | Nombre del centro de distribución         |
| `assignment_type`            | string | Sí        | Tipo de asignación                        |
| `estimated_preparation_time` | number | No        | Tiempo estimado de preparación en minutos |
| `priority_level`             | string | Sí        | Nivel de prioridad de la orden            |
| `items`                      | array  | Sí        | Lista de productos y sus ubicaciones      |

**Ejemplo de Uso:**
```json
{
  "event_type": "warehouse_assigned",
  "data": {
    "order_id": "ORD_123",
    "warehouse_id": "WH_001",
    "warehouse_name": "Centro Principal",
    "assignment_type": "automatic",
    "priority_level": "high",
    "items": [
      {
        "product_id": "PROD_789",
        "quantity": 2,
        "stock_location": "A-12-3"
      }
    ]
  }
}
```

---

### Inicio de Picking

!!! note "`picking_started`"
    **Trigger**  
    Se dispara cuando comienza el proceso de recolección de productos

    **Descripción**  
    Registra el inicio del proceso de recolección de productos en el centro de distribución.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "picking_started",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "warehouse_id": "string",
    "picker_id": "string",
    "picking_route": [
      {
        "location": "string",
        "products": [
          {
            "product_id": "string",
            "quantity": "number",
            "bin_location": "string"
          }
        ]
      }
    ],
    "estimated_completion_time": "string"
  },
  "metadata": {
    "picking_batch_id": "string",
    "picking_type": "single|batch"
  }
}
```

**Campos Específicos:**

| Campo                       | Tipo   | Requerido | Descripción                           |
| --------------------------- | ------ | --------- | ------------------------------------- |
| `picker_id`                 | string | Sí        | Identificador del operador de picking |
| `picking_route`             | array  | Sí        | Ruta optimizada de recolección        |
| `estimated_completion_time` | string | No        | Hora estimada de finalización         |
| `picking_batch_id`          | string | No        | ID del batch de picking si aplica     |

**Ejemplo de Uso:**
```json
{
  "event_type": "picking_started",
  "data": {
    "order_id": "ORD_123",
    "warehouse_id": "WH_001",
    "picker_id": "PICK_456",
    "picking_route": [
      {
        "location": "A-12-3",
        "products": [
          {
            "product_id": "PROD_789",
            "quantity": 2,
            "bin_location": "A-12-3"
          }
        ]
      }
    ]
  }
}
```

---

### Picking Completado

!!! success "`picking_completed`"
    **Trigger**  
    Se dispara cuando finaliza el proceso de recolección de productos

    **Descripción**  
    Registra la finalización del proceso de recolección de productos.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "picking_completed",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "warehouse_id": "string",
    "picker_id": "string",
    "duration": "number",
    "items_status": [
      {
        "product_id": "string",
        "requested_quantity": "number",
        "picked_quantity": "number",
        "status": "complete|partial|not_found"
      }
    ],
    "issues": [
      {
        "type": "stock_mismatch|location_error|product_damage",
        "product_id": "string",
        "description": "string"
      }
    ]
  },
  "metadata": {
    "accuracy_rate": "number",
    "requires_validation": "boolean"
  }
}
```

**Campos Específicos:**

| Campo           | Tipo   | Requerido | Descripción                              |
| --------------- | ------ | --------- | ---------------------------------------- |
| `duration`      | number | Sí        | Duración del picking en minutos          |
| `items_status`  | array  | Sí        | Estado de recolección de cada item       |
| `issues`        | array  | No        | Problemas encontrados durante el picking |
| `accuracy_rate` | number | No        | Tasa de precisión del picking            |

**Ejemplo de Uso:**
```json
{
  "event_type": "picking_completed",
  "data": {
    "order_id": "ORD_123",
    "warehouse_id": "WH_001",
    "picker_id": "PICK_456",
    "duration": 15,
    "items_status": [
      {
        "product_id": "PROD_789",
        "requested_quantity": 2,
        "picked_quantity": 2,
        "status": "complete"
      }
    ]
  }
}
```

---

### Packing Iniciado

!!! note "`packing_started`"
    **Trigger**  
    Se dispara cuando comienza el proceso de empaquetado

    **Descripción**  
    Registra el inicio del proceso de empaquetado de la orden.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "packing_started",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "warehouse_id": "string",
    "packer_id": "string",
    "packaging_type": "string",
    "estimated_packages": "number",
    "special_requirements": [
      {
        "type": "fragile|refrigerated|oversized",
        "instructions": "string"
      }
    ]
  },
  "metadata": {
    "packing_station": "string",
    "priority_level": "string"
  }
}
```

**Campos Específicos:**

| Campo                  | Tipo   | Requerido | Descripción                           |
| ---------------------- | ------ | --------- | ------------------------------------- |
| `packer_id`            | string | Sí        | Identificador del operador de packing |
| `packaging_type`       | string | Sí        | Tipo de empaque a utilizar            |
| `estimated_packages`   | number | Sí        | Número estimado de paquetes           |
| `special_requirements` | array  | No        | Requisitos especiales de empaque      |

**Ejemplo de Uso:**
```json
{
  "event_type": "packing_started",
  "data": {
    "order_id": "ORD_123",
    "warehouse_id": "WH_001",
    "packer_id": "PACK_789",
    "packaging_type": "box_medium",
    "estimated_packages": 1,
    "special_requirements": [
      {
        "type": "fragile",
        "instructions": "Empacar con material de protección adicional"
      }
    ]
  }
}
```

---

### Orden Despachada

!!! success "`order_dispatched`"
    **Trigger**  
    Se dispara cuando la orden es entregada al transportista

    **Descripción**  
    Registra cuando una orden es entregada al transportista para su envío.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "order_dispatched",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "order_id": "string",
    "warehouse_id": "string",
    "carrier_id": "string",
    "tracking_number": "string",
    "packages": [
      {
        "package_id": "string",
        "weight": "number",
        "dimensions": {
          "length": "number",
          "width": "number",
          "height": "number"
        },
        "tracking_number": "string"
      }
    ],
    "estimated_delivery_date": "string",
    "shipping_service": "string"
  },
  "metadata": {
    "dispatch_batch_id": "string",
    "route_id": "string"
  }
}
```

**Campos Específicos:**

| Campo                     | Tipo   | Requerido | Descripción                     |
| ------------------------- | ------ | --------- | ------------------------------- |
| `carrier_id`              | string | Sí        | Identificador del transportista |
| `tracking_number`         | string | Sí        | Número de seguimiento principal |
| `packages`                | array  | Sí        | Información de los paquetes     |
| `estimated_delivery_date` | string | Sí        | Fecha estimada de entrega       |

**Ejemplo de Uso:**
```json
{
  "event_type": "order_dispatched",
  "data": {
    "order_id": "ORD_123",
    "warehouse_id": "WH_001",
    "carrier_id": "CARR_001",
    "tracking_number": "TRACK123456",
    "packages": [
      {
        "package_id": "PKG_001",
        "weight": 2.5,
        "dimensions": {
          "length": 30,
          "width": 20,
          "height": 15
        },
        "tracking_number": "TRACK123456-1"
      }
    ],
    "estimated_delivery_date": "2024-03-22T15:00:00Z"
  }
}
```

---

## Consideraciones Técnicas

- Los eventos deben ser procesados en orden cronológico
- Se debe mantener la trazabilidad completa del proceso
- Los tiempos estimados deben actualizarse en tiempo real
- Se debe validar la consistencia del inventario en cada paso
- Los eventos deben incluir información suficiente para auditorías
- Se debe considerar la gestión de excepciones en cada etapa

## Casos de Uso Comunes

### Optimización de Picking

Utiliza los eventos `picking_started` y `picking_completed` para:

1. Analizar tiempos de picking por operador
2. Optimizar rutas de recolección
3. Identificar productos problemáticos

### Gestión de Capacidad

Utiliza el evento `warehouse_assigned` para:

1. Balancear la carga entre centros de distribución
2. Predecir necesidades de personal
3. Optimizar la asignación de recursos

### Monitoreo de Envíos

Utiliza el evento `order_dispatched` para:

1. Realizar seguimiento de envíos
2. Calcular tiempos de entrega
3. Gestionar excepciones en la entrega

## Preguntas Frecuentes

### ¿Cómo manejar productos faltantes durante el picking?
Se debe registrar la discrepancia en `picking_completed` y generar eventos de reposición.

### ¿Qué hacer si un paquete necesita ser reempacado?
Generar un nuevo evento `packing_started` con referencia al empaque original.

### ¿Cómo gestionar múltiples transportistas?
Mantener un mapeo estandarizado de estados y servicios por transportista.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Operaciones de Warehouse](../integration/warehouse_operations.md)
- [Mejores Prácticas de Fulfillment](../integration/fulfillment_best_practices.md)
