# Mensajería

Las tareas de mensajería representan las comunicaciones realizadas a través de canales digitales como WhatsApp, correo electrónico, SMS y notificaciones push. Permiten una comunicación asíncrona y escalable con los clientes.

## Estructura de Datos

```json
{
  // Heredado de la estructura base de tareas
  "task_id": "string",
  "type": "message",
  "objective": "string",
  
  // Campos específicos de mensajería
  "message_details": {
    "channel": {
      "type": "string",         // whatsapp, email, sms, push
      "config": {              // Configuración específica del canal
        "template_id": "string", // ID de plantilla
        "fallback_channel": "string", // Canal alternativo
        "priority": "string"    // Prioridad de envío
      }
    },
    "content": {
      "template": {
        "id": "string",        // ID del template
        "version": "string",   // Versión del template
        "variables": {}        // Variables del template
      },
      "attachments": [{       // Archivos adjuntos
        "type": "string",     // Tipo de archivo
        "url": "string",      // URL del archivo
        "name": "string",     // Nombre del archivo
        "size": "number"      // Tamaño en bytes
      }],
      "dynamic_content": {    // Contenido dinámico
        "personalization": {},
        "conditions": {}
      }
    },
    "delivery": {
      "scheduled_time": "timestamp", // Hora programada
      "expiration_time": "timestamp", // Tiempo de expiración
      "retry_policy": {      // Política de reintentos
        "max_attempts": "number",
        "interval": "number",
        "strategy": "string"
      }
    }
  },

  // Objetivos específicos
  "objectives": {
    "expected_action": "string", // Acción esperada
    "conversion_goal": {        // Meta de conversión
      "type": "string",
      "value": "number",
      "deadline": "timestamp"
    },
    "required_tracking": [{     // Seguimiento requerido
      "event": "string",
      "timeout": "number"
    }]
  },

  // Resultados específicos
  "message_results": {
    "delivery_status": {
      "status": "string",      // Estado de entrega
      "timestamp": "date",     // Hora de entrega
      "attempts": [{          // Intentos de envío
        "timestamp": "date",
        "status": "string",
        "error": "string"
      }]
    },
    "engagement_data": {
      "received_at": "timestamp", // Hora de recepción
      "read_at": "timestamp",    // Hora de lectura
      "clicked_at": "timestamp", // Hora de click
      "replied_at": "timestamp", // Hora de respuesta
      "interaction_type": "string" // Tipo de interacción
    },
    "response_data": {
      "type": "string",        // Tipo de respuesta
      "content": "string",     // Contenido de respuesta
      "sentiment": "string"    // Sentimiento detectado
    },
    "conversion_data": {
      "converted": "boolean",  // Meta alcanzada
      "value": "number",      // Valor generado
      "conversion_path": []    // Ruta de conversión
    }
  }
}
```

## Campos Específicos

### Canal y Contenido

| Campo        | Tipo   | Requerido | Descripción                    |
| ------------ | ------ | --------- | ------------------------------ |
| channel_type | string | Sí        | Tipo de canal de mensajería    |
| template_id  | string | Sí        | ID de la plantilla a utilizar  |
| variables    | object | No        | Variables para personalización |
| attachments  | array  | No        | Archivos adjuntos al mensaje   |

### Entrega y Seguimiento

| Campo           | Tipo      | Requerido | Descripción                      |
| --------------- | --------- | --------- | -------------------------------- |
| scheduled_time  | timestamp | Sí        | Momento programado para envío    |
| max_attempts    | number    | Sí        | Número máximo de intentos        |
| expiration_time | timestamp | No        | Momento en que el mensaje expira |

## Validaciones

### Validaciones Específicas

#### Canal
- Canal activo y disponible
- Plantillas aprobadas
- Límites de tamaño y formato

#### Contenido
- Variables requeridas completas
- Archivos adjuntos válidos
- Personalización correcta

### Reglas de Negocio

- Respeto de preferencias de contacto
- Frecuencia máxima de mensajes
- Horarios permitidos por canal
- Priorización de mensajes

## Ejemplos

### Mensaje de WhatsApp

```json
{
  "task_id": "MSG_001",
  "type": "message",
  "objective": "activation",
  "message_details": {
    "channel": {
      "type": "whatsapp",
      "config": {
        "template_id": "welcome_template",
        "fallback_channel": "sms",
        "priority": "high"
      }
    },
    "content": {
      "template": {
        "id": "WAPP_001",
        "version": "1.0",
        "variables": {
          "client_name": "Juan Pérez",
          "activation_code": "123456"
        }
      },
      "attachments": [{
        "type": "pdf",
        "url": "https://example.com/guide.pdf",
        "name": "guia_inicio.pdf",
        "size": 1024576
      }]
    },
    "delivery": {
      "scheduled_time": "2024-03-20T15:00:00Z",
      "expiration_time": "2024-03-20T23:59:59Z",
      "retry_policy": {
        "max_attempts": 3,
        "interval": 300,
        "strategy": "exponential"
      }
    }
  }
}
```

### Correo de Bienvenida

```json
{
  "task_id": "MSG_002",
  "type": "message",
  "objective": "onboarding",
  "message_details": {
    "channel": {
      "type": "email",
      "config": {
        "template_id": "welcome_email",
        "priority": "normal"
      }
    },
    "content": {
      "template": {
        "id": "EMAIL_001",
        "version": "2.1",
        "variables": {
          "user_name": "María González",
          "company_name": "Tienda ABC",
          "login_url": "https://app.example.com/login"
        }
      },
      "dynamic_content": {
        "personalization": {
          "recommended_products": true,
          "local_offers": true
        }
      }
    }
  }
}
```

## Flujo de Trabajo

### Proceso Principal

1. Preparación
   - Validación de canal
   - Compilación de contenido
   - Verificación de programación

2. Envío
   - Procesamiento de plantilla
   - Envío por canal principal
   - Manejo de fallback

3. Seguimiento
   - Monitoreo de entrega
   - Tracking de interacción
   - Registro de resultados

### Estados Específicos

- `draft`: Borrador
- `scheduled`: Programado
- `sending`: En proceso de envío
- `delivered`: Entregado
- `failed`: Fallido
- `expired`: Expirado
- `completed`: Completado con interacción

## Integración

### APIs Específicas

#### Endpoints
```
GET    /api/v1/messages
POST   /api/v1/messages
PUT    /api/v1/messages/{message_id}
DELETE /api/v1/messages/{message_id}

POST   /api/v1/messages/{message_id}/send
POST   /api/v1/messages/{message_id}/cancel
GET    /api/v1/messages/{message_id}/status
```

### Eventos Específicos

- `message.created`: Mensaje creado
- `message.scheduled`: Mensaje programado
- `message.sent`: Mensaje enviado
- `message.delivered`: Mensaje entregado
- `message.read`: Mensaje leído
- `message.replied`: Mensaje respondido
- `message.failed`: Envío fallido

## Métricas y KPIs

### Métricas de Rendimiento
- Tasa de entrega
- Tasa de apertura
- Tasa de respuesta
- Tasa de conversión
- Tiempo de respuesta

### Reportes
- Efectividad por canal
- Análisis de engagement
- Rendimiento de plantillas
- Patrones de respuesta
- ROI por campaña

## Consideraciones Técnicas

### Optimización
- Procesamiento en lotes
- Caché de plantillas
- Compresión de adjuntos
- Rate limiting por canal

### Limitaciones
- Cuotas por canal
- Tamaño de mensajes
- Formatos soportados
- Ventanas de envío

## Preguntas Frecuentes

**¿Qué hacer si un canal está caído?**
- Activar canal alternativo
- Notificar al equipo técnico
- Reprogramar envíos afectados

**¿Cómo manejar respuestas inesperadas?**
- Clasificar tipo de respuesta
- Escalar si es necesario
- Actualizar base de conocimiento 