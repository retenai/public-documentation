# :material-map-marker: Visitas

Las visitas representan los momentos de contacto presencial entre vendedores y clientes. Son fundamentales para mantener relaciones comerciales sólidas, realizar ventas directas, y recopilar información valiosa del terreno.

## Estructura de Datos

```json
{
  // Heredado de la estructura base de tareas
  "task_id": "string",
  "type": "visit",
  "objective": "string",
  
  // Campos específicos de visita
  "visit_details": {
    "location": {
      "address": "string",      // Dirección física
      "coordinates": {          // Coordenadas geográficas
        "latitude": "number",
        "longitude": "number"
      },
      "reference": "string"     // Punto de referencia
    },
    "route_info": {
      "route_id": "string",     // ID de la ruta asociada
      "sequence": "number",     // Orden en la ruta
      "estimated_duration": "number" // Duración estimada en minutos
    },
    "checkin": {
      "required": "boolean",    // Requiere check-in
      "radius": "number",       // Radio permitido en metros
      "photo_required": "boolean" // Requiere foto de check-in
    }
  },

  // Objetivos específicos
  "objectives": {
    "sales_target": "number",   // Meta de ventas
    "survey_required": "boolean", // Requiere encuesta
    "product_focus": ["string"], // Productos a promocionar
    "required_actions": [{      // Acciones requeridas
      "type": "string",        // Tipo de acción
      "description": "string", // Descripción
      "required": "boolean"    // Es obligatorio
    }]
  },

  // Resultados específicos
  "visit_results": {
    "checkin_data": {
      "timestamp": "date",     // Hora de check-in
      "coordinates": {         // Ubicación de check-in
        "latitude": "number",
        "longitude": "number"
      },
      "photo_url": "string"    // URL de la foto de check-in
    },
    "checkout_data": {
      "timestamp": "date",     // Hora de check-out
      "duration": "number",    // Duración real en minutos
      "coordinates": {         // Ubicación de check-out
        "latitude": "number",
        "longitude": "number"
      }
    },
    "sales_data": {
      "total_amount": "number", // Monto total vendido
      "products_sold": [{      // Productos vendidos
        "product_id": "string",
        "quantity": "number",
        "amount": "number"
      }]
    },
    "survey_results": {},      // Resultados de la encuesta
    "notes": "string",         // Notas del vendedor
    "next_visit_suggestion": { // Sugerencia próxima visita
      "suggested_date": "date",
      "reason": "string"
    }
  }
}
```

## Campos Específicos

### Ubicación y Ruta

| Campo       | Tipo   | Requerido | Descripción                      |
| ----------- | ------ | --------- | -------------------------------- |
| address     | string | Sí        | Dirección física del cliente     |
| coordinates | object | Sí        | Coordenadas geográficas          |
| route_id    | string | No        | ID de la ruta si es parte de una |
| sequence    | number | No        | Orden en la secuencia de la ruta |

### Check-in/Check-out

| Campo          | Tipo    | Requerido | Descripción                            |
| -------------- | ------- | --------- | -------------------------------------- |
| required       | boolean | Sí        | Si requiere check-in presencial        |
| radius         | number  | No        | Radio permitido para check-in (metros) |
| photo_required | boolean | No        | Si requiere foto como evidencia        |

## Validaciones

### Validaciones Específicas

#### Ubicación
- Coordenadas dentro del territorio asignado
- Radio de check-in dentro del límite permitido
- Dirección válida y geolocalizable

#### Tiempo
- Duración dentro de rangos esperados
- Check-out posterior al check-in
- Visita dentro del horario laboral

### Reglas de Negocio

- Una visita debe tener al menos un objetivo definido
- Check-in requerido antes de registrar resultados
- Fotos de check-in deben incluir metadata de ubicación
- Tiempo mínimo entre visitas al mismo cliente

## Ejemplos

### Visita de Venta Regular

```json
{
  "task_id": "VST_001",
  "type": "visit",
  "objective": "retention",
  "visit_details": {
    "location": {
      "address": "Av. Principal 123, Ciudad",
      "coordinates": {
        "latitude": -33.4569,
        "longitude": -70.6483
      }
    },
    "route_info": {
      "route_id": "RTE_001",
      "sequence": 3,
      "estimated_duration": 45
    },
    "checkin": {
      "required": true,
      "radius": 100,
      "photo_required": true
    }
  },
  "objectives": {
    "sales_target": 150000,
    "survey_required": true,
    "product_focus": ["PRD_001", "PRD_002"],
    "required_actions": [{
      "type": "stock_check",
      "description": "Verificar stock de productos",
      "required": true
    }]
  }
}
```

### Visita de Activación

```json
{
  "task_id": "VST_002",
  "type": "visit",
  "objective": "acquisition",
  "visit_details": {
    "location": {
      "address": "Calle Comercial 456, Ciudad",
      "coordinates": {
        "latitude": -33.4579,
        "longitude": -70.6493
      }
    },
    "checkin": {
      "required": true,
      "radius": 50,
      "photo_required": true
    }
  },
  "objectives": {
    "survey_required": true,
    "required_actions": [{
      "type": "registration",
      "description": "Completar registro de cliente",
      "required": true
    }, {
      "type": "training",
      "description": "Capacitación básica de la plataforma",
      "required": true
    }]
  }
}
```

## Flujo de Trabajo

### Proceso Principal

1. Preparación de la visita
   - Revisión de objetivos
   - Verificación de ruta
   - Preparación de materiales

2. Ejecución
   - Check-in en ubicación
   - Realización de actividades
   - Registro de resultados
   - Check-out

3. Seguimiento
   - Sincronización de datos
   - Programación siguiente visita
   - Actualización de métricas

### Estados Específicos

- `scheduled`: Visita programada
- `checked_in`: Vendedor en sitio
- `in_progress`: Visita en curso
- `completed`: Visita finalizada
- `incomplete`: Visita incompleta
- `cancelled`: Visita cancelada
- `rescheduled`: Visita reprogramada

## Integración

### APIs Específicas

#### Endpoints
```
GET    /api/v1/visits
POST   /api/v1/visits
PUT    /api/v1/visits/{visit_id}
DELETE /api/v1/visits/{visit_id}

POST   /api/v1/visits/{visit_id}/checkin
POST   /api/v1/visits/{visit_id}/checkout
POST   /api/v1/visits/{visit_id}/results
```

### Eventos Específicos

- `visit.scheduled`: Visita programada
- `visit.checkin`: Check-in realizado
- `visit.checkout`: Check-out realizado
- `visit.completed`: Visita completada
- `visit.cancelled`: Visita cancelada
- `visit.rescheduled`: Visita reprogramada

## Métricas y KPIs

### Métricas de Rendimiento
- Tasa de cumplimiento de visitas
- Duración promedio de visitas
- Tasa de conversión de objetivos
- Eficiencia de ruta
- Ventas por visita

### Reportes
- Resumen diario de visitas
- Cumplimiento de objetivos
- Cobertura territorial
- Efectividad de ventas
- Análisis de tiempo en ruta

## Consideraciones Técnicas

### Optimización
- Caché de datos offline
- Sincronización inteligente
- Compresión de fotos
- Batería y consumo de datos

### Limitaciones
- Conectividad intermitente
- Precisión GPS variable
- Capacidad de almacenamiento
- Duración de batería

## Preguntas Frecuentes

**¿Qué hacer si no hay señal para el check-in?**
- La app debe funcionar offline
- Datos se sincronizan al recuperar conexión
- Se registra el momento real del check-in

**¿Cómo manejar visitas que se extienden más de lo planeado?**
- Sistema ajusta automáticamente la ruta
- Notifica a clientes afectados
- Registra razones de la extensión 