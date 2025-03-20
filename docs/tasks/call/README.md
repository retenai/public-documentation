# Llamadas

Las llamadas representan los contactos telefónicos realizados por el equipo de call center con los clientes. Son esenciales para brindar soporte, realizar ventas telefónicas y mantener una comunicación efectiva a distancia.

## Estructura de Datos

```json
{
  // Heredado de la estructura base de tareas
  "task_id": "string",
  "type": "call",
  "objective": "string",
  
  // Campos específicos de llamada
  "call_details": {
    "contact": {
      "phone_number": "string",   // Número a llamar
      "alternative_numbers": ["string"], // Números alternativos
      "best_time": {             // Mejor horario
        "start": "string",       // Hora inicio (HH:mm)
        "end": "string"          // Hora fin (HH:mm)
      },
      "language": "string"       // Idioma preferido
    },
    "script": {
      "id": "string",            // ID del script
      "version": "string",       // Versión del script
      "type": "string"          // Tipo de script
    },
    "recording": {
      "required": "boolean",     // Requiere grabación
      "consent_required": "boolean", // Requiere consentimiento
      "retention_days": "number" // Días de retención
    }
  },

  // Objetivos específicos
  "objectives": {
    "sales_target": "number",    // Meta de ventas
    "survey_required": "boolean", // Requiere encuesta
    "required_topics": ["string"], // Temas a tratar
    "required_actions": [{       // Acciones requeridas
      "type": "string",         // Tipo de acción
      "description": "string",  // Descripción
      "required": "boolean"     // Es obligatorio
    }]
  },

  // Resultados específicos
  "call_results": {
    "call_data": {
      "start_time": "timestamp", // Inicio de llamada
      "end_time": "timestamp",   // Fin de llamada
      "duration": "number",      // Duración en segundos
      "attempts": [{            // Intentos de contacto
        "timestamp": "date",
        "outcome": "string",
        "note": "string"
      }]
    },
    "recording_data": {
      "recording_id": "string",  // ID de la grabación
      "url": "string",          // URL de la grabación
      "consent_obtained": "boolean" // Consentimiento obtenido
    },
    "interaction_data": {
      "topics_covered": ["string"], // Temas cubiertos
      "customer_mood": "string",   // Estado de ánimo
      "callback_requested": {      // Solicitud de rellamado
        "requested": "boolean",
        "preferred_date": "date",
        "reason": "string"
      }
    },
    "sales_data": {
      "total_amount": "number",   // Monto total vendido
      "products": [{             // Productos
        "product_id": "string",
        "quantity": "number",
        "amount": "number"
      }]
    },
    "survey_results": {},        // Resultados de encuesta
    "notes": "string",           // Notas del agente
    "follow_up": {              // Seguimiento
      "required": "boolean",
      "type": "string",
      "due_date": "date",
      "description": "string"
    }
  }
}
```

## Campos Específicos

### Contacto

| Campo               | Tipo   | Requerido | Descripción                      |
| ------------------- | ------ | --------- | -------------------------------- |
| phone_number        | string | Sí        | Número principal de contacto     |
| alternative_numbers | array  | No        | Números alternativos             |
| best_time           | object | No        | Horario preferido de contacto    |
| language            | string | No        | Idioma preferido para la llamada |

### Script y Grabación

| Campo              | Tipo    | Requerido | Descripción                                |
| ------------------ | ------- | --------- | ------------------------------------------ |
| script_id          | string  | Sí        | Identificador del script a seguir          |
| recording_required | boolean | Sí        | Si la llamada debe ser grabada             |
| consent_required   | boolean | Sí        | Si se requiere consentimiento de grabación |

## Validaciones

### Validaciones Específicas

#### Contacto
- Números de teléfono válidos y formateados
- Horarios dentro de rangos permitidos
- Idioma disponible en el sistema

#### Grabación
- Consentimiento registrado si es requerido
- Calidad mínima de audio
- Duración dentro de límites establecidos

### Reglas de Negocio

- Respetar horarios preferidos del cliente
- Límite de intentos por día/semana
- Tiempo mínimo entre intentos
- Priorización de llamadas por objetivo

## Ejemplos

### Llamada de Venta

```json
{
  "task_id": "CALL_001",
  "type": "call",
  "objective": "sales",
  "call_details": {
    "contact": {
      "phone_number": "+56912345678",
      "best_time": {
        "start": "09:00",
        "end": "18:00"
      },
      "language": "es"
    },
    "script": {
      "id": "SCRIPT_001",
      "version": "1.2",
      "type": "sales"
    },
    "recording": {
      "required": true,
      "consent_required": true,
      "retention_days": 90
    }
  },
  "objectives": {
    "sales_target": 100000,
    "required_topics": ["product_introduction", "pricing", "benefits"],
    "required_actions": [{
      "type": "quote",
      "description": "Enviar cotización por email",
      "required": true
    }]
  }
}
```

### Llamada de Soporte

```json
{
  "task_id": "CALL_002",
  "type": "call",
  "objective": "support",
  "call_details": {
    "contact": {
      "phone_number": "+56987654321",
      "alternative_numbers": ["+56987654322"],
      "language": "es"
    },
    "script": {
      "id": "SCRIPT_002",
      "version": "1.0",
      "type": "support"
    },
    "recording": {
      "required": true,
      "consent_required": true,
      "retention_days": 30
    }
  },
  "objectives": {
    "survey_required": true,
    "required_topics": ["issue_identification", "troubleshooting"],
    "required_actions": [{
      "type": "ticket",
      "description": "Crear ticket de soporte",
      "required": true
    }]
  }
}
```

## Flujo de Trabajo

### Proceso Principal

1. Preparación
   - Revisión de información del cliente
   - Verificación de script
   - Comprobación de horario

2. Ejecución
   - Marcación y conexión
   - Obtención de consentimiento
   - Desarrollo del script
   - Registro de resultados

3. Seguimiento
   - Documentación de la llamada
   - Programación de acciones
   - Actualización de estado

### Estados Específicos

- `pending`: Pendiente de llamada
- `attempting`: Intentando contactar
- `in_progress`: Llamada en curso
- `completed`: Llamada completada
- `no_answer`: Sin respuesta
- `busy`: Número ocupado
- `wrong_number`: Número equivocado
- `callback_scheduled`: Rellamada programada

## Integración

### APIs Específicas

#### Endpoints
```
GET    /api/v1/calls
POST   /api/v1/calls
PUT    /api/v1/calls/{call_id}
DELETE /api/v1/calls/{call_id}

POST   /api/v1/calls/{call_id}/start
POST   /api/v1/calls/{call_id}/end
POST   /api/v1/calls/{call_id}/results
```

### Eventos Específicos

- `call.scheduled`: Llamada programada
- `call.started`: Llamada iniciada
- `call.ended`: Llamada finalizada
- `call.recorded`: Grabación completada
- `call.callback_requested`: Rellamada solicitada

## Métricas y KPIs

### Métricas de Rendimiento
- Tasa de contactación
- Duración promedio
- Tasa de conversión
- Satisfacción del cliente
- Ventas por llamada

### Reportes
- Resumen diario de llamadas
- Análisis de resultados
- Tiempo de agentes
- Efectividad de scripts
- Calidad de llamadas

## Consideraciones Técnicas

### Optimización
- Calidad de audio
- Gestión de grabaciones
- Integración telefónica
- Latencia de sistema

### Limitaciones
- Capacidad de líneas
- Almacenamiento de grabaciones
- Horarios de operación
- Disponibilidad de agentes

## Preguntas Frecuentes

**¿Qué hacer si el cliente solicita no ser grabado?**
- Verificar si la grabación es obligatoria
- Ofrecer alternativas de atención
- Documentar la solicitud

**¿Cómo manejar llamadas que requieren seguimiento especial?**
- Crear tarea de seguimiento
- Asignar al equipo especializado
- Documentar requerimientos específicos 