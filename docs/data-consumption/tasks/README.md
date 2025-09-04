# :material-checkbox-marked-circle: Tareas

Las tareas en Reten representan los momentos de contacto planificados con los clientes. La API expone una única entidad `tasks` que maneja diferentes tipos de interacciones identificadas por campos como `channel_priority`, `reason` y `label`.

## API de Tareas

La API expone el endpoint `GET /api/v1/tasks/` que retorna una lista de tareas programadas según los parámetros de filtrado.

### Endpoint
```http
GET /api/v1/tasks/
```

### Parámetros de Consulta
- `from_date`: Fecha desde (ISO 8601, opcional)
- `to_date`: Fecha hasta (ISO 8601, opcional)
- `channel_priority`: Prioridad del canal ("salesman", "callcenter", opcional)

### Estructura de Respuesta

```json
[
  {
    "id": "123456789",
    "user_id": "123456789",
    "date": "2024-01-15T10:30:00Z",
    "suggested_execution_time": "2024-01-15T14:00:00Z",
    "template_msg_id": "abc123",
    "score": 85,
    "label": "Cliente VIP",
    "reason": "Seguimiento mensual",
    "channel_priority": "salesman",
    "channel_secondary": "whatsapp",
    "created_at": "2024-01-10T09:00:00Z",
    "updated_at": "2024-01-10T09:00:00Z"
  }
]
```

### Campos Principales
- `id`: Identificador único de la tarea
- `user_id`: ID del usuario objetivo
- `channel_priority`: Canal principal ("salesman", "callcenter")
- `channel_secondary`: Canal secundario ("whatsapp", "email", etc.)
- `reason`: Motivo de la tarea
- `score`: Puntaje/prioridad de la tarea
- `suggested_execution_time`: Hora sugerida de ejecución

## Tipos de Tareas

La API maneja diferentes tipos de tareas identificadas principalmente por el campo `channel_priority`:

### Visitas de Vendedor
- `channel_priority`: "salesman"
- `channel_secondary`: "whatsapp" o "email"
- Descripción: Interacciones presenciales planificadas por vendedores
- Propósito: Seguimiento de clientes, ventas, recolección de feedback

### Llamadas de Call Center
- `channel_priority`: "callcenter"
- `channel_secondary`: "whatsapp" o "sms"
- Descripción: Contactos telefónicos gestionados por el call center
- Propósito: Atención al cliente, seguimiento, reactivación

## Estados de Tarea

Las tareas pueden tener diferentes estados identificados por el campo `reason` y otros indicadores:

- **Seguimiento mensual**: `reason: "Seguimiento mensual"`
- **Cliente VIP**: `label: "Cliente VIP"`
- **Reactivación**: Basado en score y tiempo desde último contacto
- **Venta cruzada**: `reason: "Venta cruzada"`

## Ejemplos de Uso

### Consultar tareas de vendedores
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?channel_priority=salesman" \
  -H "X-API-Key: your_api_key"
```

### Filtrar por fecha
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?from_date=2024-01-01T00:00:00Z&to_date=2024-01-31T23:59:59Z" \
  -H "X-API-Key: your_api_key"
```

### Python
```python
import requests

# Consultar tareas de call center
response = requests.get(
    'https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/',
    headers={'X-API-Key': 'your_api_key'},
    params={'channel_priority': 'callcenter'}
)

tasks = response.json()
for task in tasks:
    print(f"Tarea {task['id']} - Usuario {task['user_id']} - Razón: {task['reason']}")
```
