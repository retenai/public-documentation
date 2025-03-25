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

### Asignación a Centro de Distribución

**Tipo de Evento:** `warehouse_assigned`

**Descripción:**  
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
| Campo                      | Tipo   | Requerido | Descripción                               |
| -------------------------- | ------ | --------- | ----------------------------------------- |
| warehouse_id               | string | Sí        | Identificador del centro de distribución  |
| warehouse_name             | string | Sí        | Nombre del centro de distribución         |
| assignment_type            | string | Sí        | Tipo de asignación                        |
| estimated_preparation_time | number | No        | Tiempo estimado de preparación en minutos |
| priority_level             | string | Sí        | Nivel de prioridad de la orden            |
| items                      | array  | Sí        | Lista de productos y sus ubicaciones      |

### Inicio de Picking

**Tipo de Evento:** `picking_started`

**Descripción:**  
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
| Campo                     | Tipo   | Requerido | Descripción                           |
| ------------------------- | ------ | --------- | ------------------------------------- |
| picker_id                 | string | Sí        | Identificador del operador de picking |
| picking_route             | array  | Sí        | Ruta optimizada de recolección        |
| estimated_completion_time | string | No        | Hora estimada de finalización         |
| picking_batch_id          | string | No        | ID del batch de picking si aplica     |

### Picking Completado

**Tipo de Evento:** `picking_completed`

**Descripción:**  
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
| Campo         | Tipo   | Requerido | Descripción                              |
| ------------- | ------ | --------- | ---------------------------------------- |
| duration      | number | Sí        | Duración del picking en minutos          |
| items_status  | array  | Sí        | Estado de recolección de cada item       |
| issues        | array  | No        | Problemas encontrados durante el picking |
| accuracy_rate | number | No        | Tasa de precisión del picking            |

### Packing Iniciado

**Tipo de Evento:** `packing_started`

**Descripción:**  
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
| Campo                | Tipo   | Requerido | Descripción                           |
| -------------------- | ------ | --------- | ------------------------------------- |
| packer_id            | string | Sí        | Identificador del operador de packing |
| packaging_type       | string | Sí        | Tipo de empaque a utilizar            |
| estimated_packages   | number | Sí        | Número estimado de paquetes           |
| special_requirements | array  | No        | Requisitos especiales de empaque      |

### Orden Despachada

**Tipo de Evento:** `order_dispatched`

**Descripción:**  
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
| Campo                   | Tipo   | Requerido | Descripción                     |
| ----------------------- | ------ | --------- | ------------------------------- |
| carrier_id              | string | Sí        | Identificador del transportista |
| tracking_number         | string | Sí        | Número de seguimiento principal |
| packages                | array  | Sí        | Información de los paquetes     |
| estimated_delivery_date | string | Sí        | Fecha estimada de entrega       |

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
Registrar la discrepancia en `picking_completed` y generar eventos de reposición.

### ¿Qué hacer si un paquete necesita ser reempacado?
Generar un nuevo evento `packing_started` con referencia al empaque original.

### ¿Cómo gestionar múltiples transportistas?
Mantener un mapeo estandarizado de estados y servicios por transportista.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Operaciones de Warehouse](../integration/warehouse_operations.md)
- [Mejores Prácticas de Fulfillment](../integration/fulfillment_best_practices.md)
