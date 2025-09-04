# :material-checkbox-marked-circle: Tareas

Las tareas representan los momentos de contacto planificados con los clientes en el sistema Reten. Esta entidad maneja diferentes tipos de interacciones identificadas por campos específicos, permitiendo una gestión unificada de todas las actividades de outreach.

## Estructura de Datos

```json
{
  // Identificadores (requeridos)
  "task_id": "string",                 // Identificador único de la tarea (not null)
  "user_id": "string",                 // ID del usuario objetivo (not null)

  // Información temporal
  "date": "timestamp",                 // Fecha programada para la tarea (not null)
  "suggested_execution_time": "timestamp", // Hora sugerida de ejecución (not null)

  // Información del canal y prioridad
  "channel_priority": "string",        // Canal prioritario: "salesman", "callcenter", "whatsapp", "email", "sms", "push" (not null)
  "channel_secondary": "string",       // Canal alternativo: "salesman", "callcenter", "whatsapp", "email", "sms", "push"

  // Información de negocio
  "template_msg_id": "string",         // ID de plantilla de mensaje
  "score": "number",                   // Puntaje/prioridad de la tarea
  "label": "string",                   // Etiqueta descriptiva
  "reason": "string",                  // Motivo específico de la tarea

  "created_at": "timestamp",           // Fecha de creación (not null)
  "updated_at": "timestamp",           // Última actualización

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",           // Fecha de creación del registro en Reten
  "_updated_at": "timestamp"            // Fecha de última actualización del registro en Reten
}
```

## Campos Detallados

### Identificadores

| Campo   | Tipo   | Requerido | Descripción                     |
| ------- | ------ | --------- | ------------------------------- |
| task_id | string | Sí        | Identificador único de la tarea |
| user_id | string | Sí        | ID del usuario objetivo         |

### Información Temporal

| Campo                    | Tipo      | Requerido | Descripción                     |
| ------------------------ | --------- | --------- | ------------------------------- |
| date                     | timestamp | Sí        | Fecha programada para la tarea  |
| suggested_execution_time | timestamp | Sí        | Hora sugerida de ejecución      |
| created_at               | timestamp | Sí        | Fecha de creación en el sistema |
| updated_at               | timestamp | No        | Última actualización            |

### Información del Canal

| Campo             | Tipo   | Requerido | Descripción                                                                                            |
| ----------------- | ------ | --------- | ------------------------------------------------------------------------------------------------------ |
| channel_priority  | string | Sí        | Canal prioritario para ejecutar la tarea: "salesman", "callcenter", "whatsapp", "email", "sms", "push" |
| channel_secondary | string | No        | Canal alternativo: "salesman", "callcenter", "whatsapp", "email", "sms", "push"                        |

### Concepto de Prioridad de Canal

Los campos `channel_priority` y `channel_secondary` permiten definir una estrategia de contacto jerárquica:

- **`channel_priority`**: Establece el canal principal sobre el cual se debe intentar ejecutar la tarea inicialmente
- **`channel_secondary`**: Define un canal alternativo en caso de que el canal prioritario no esté disponible o falle

**Ejemplos de estrategias:**
- `channel_priority: "salesman"`, `channel_secondary: "whatsapp"` → Intentar visita primero, WhatsApp como respaldo
- `channel_priority: "callcenter"`, `channel_secondary: "email"` → Intentar llamada primero, email como respaldo
- `channel_priority: "whatsapp"`, `channel_secondary: "sms"` → Intentar WhatsApp primero, SMS como respaldo

### Información de Negocio

| Campo           | Tipo   | Requerido | Descripción                   |
| --------------- | ------ | --------- | ----------------------------- |
| template_msg_id | string | No        | ID de plantilla de mensaje    |
| score           | number | No        | Puntaje/prioridad de la tarea |
| label           | string | No        | Etiqueta descriptiva          |
| reason          | string | No        | Motivo específico de la tarea |

## Validaciones

### Validaciones Generales

#### Identificadores
- `task_id` debe ser único en todo el sistema
- `user_id` debe corresponder a un usuario existente

#### Fechas
- `date` y `suggested_execution_time` deben ser fechas válidas en el futuro
- `created_at` no puede ser posterior a `updated_at`
- Todas las fechas deben estar en formato ISO 8601

#### Canales
- `channel_priority` establece la prioridad del canal sobre el cual se debe ejecutar la tarea
- `channel_priority` debe ser uno de: "salesman", "callcenter", "whatsapp", "email", "sms", "push"
- `channel_secondary` debe ser uno de: "salesman", "callcenter", "whatsapp", "email", "sms", "push"
- `channel_secondary` puede ser el mismo que `channel_priority` o diferente

#### Información de Negocio
- `score` debe ser un número entre 0 y 100 (si se proporciona)
- `template_msg_id` debe existir en el sistema de plantillas (si se proporciona)

## Ejemplos de Uso

### Tarea de Visita de Vendedor

```json
{
  "task_id": "task_001",
  "user_id": "user_123",
  "date": "2024-01-15T10:30:00Z",
  "suggested_execution_time": "2024-01-15T14:00:00Z",
  "channel_priority": "salesman",
  "channel_secondary": "whatsapp",
  "template_msg_id": "visit_template_001",
  "score": 85,
  "label": "Cliente VIP",
  "reason": "Seguimiento mensual",
  "created_at": "2024-01-10T09:00:00Z",
  "updated_at": "2024-01-10T09:00:00Z",
  "_created_at": "2024-01-10T09:00:00Z",
  "_updated_at": "2024-01-10T09:00:00Z"
}
```

### Tarea de Llamada de Call Center

```json
{
  "task_id": "task_002",
  "user_id": "user_456",
  "date": "2024-01-16T09:00:00Z",
  "suggested_execution_time": "2024-01-16T11:30:00Z",
  "channel_priority": "callcenter",
  "channel_secondary": "whatsapp",
  "template_msg_id": "call_template_002",
  "score": 70,
  "label": "Cliente Regular",
  "reason": "Reactivación",
  "created_at": "2024-01-11T08:30:00Z",
  "updated_at": "2024-01-11T08:30:00Z",
  "_created_at": "2024-01-11T08:30:00Z",
  "_updated_at": "2024-01-11T08:30:00Z"
}
```

### Tarea de Alta Prioridad

```json
{
  "task_id": "task_003",
  "user_id": "user_789",
  "date": "2024-01-17T15:00:00Z",
  "suggested_execution_time": "2024-01-17T16:00:00Z",
  "channel_priority": "salesman",
  "channel_secondary": "email",
  "template_msg_id": "custom_template_003",
  "score": 95,
  "label": "Cliente Premium",
  "reason": "Venta cruzada",
  "created_at": "2024-01-12T10:15:00Z",
  "updated_at": "2024-01-12T10:15:00Z",
  "_created_at": "2024-01-12T10:15:00Z",
  "_updated_at": "2024-01-12T10:15:00Z"
}
```

### Tarea Digital (WhatsApp + Push)

```json
{
  "task_id": "task_004",
  "user_id": "user_999",
  "date": "2024-01-18T12:00:00Z",
  "suggested_execution_time": "2024-01-18T12:00:00Z",
  "channel_priority": "whatsapp",
  "channel_secondary": "push",
  "template_msg_id": "digital_campaign_001",
  "score": 75,
  "label": "Campaña Digital",
  "reason": "Promoción especial",
  "created_at": "2024-01-13T09:00:00Z",
  "updated_at": "2024-01-13T09:00:00Z",
  "_created_at": "2024-01-13T09:00:00Z",
  "_updated_at": "2024-01-13T09:00:00Z"
}
```

### Tarea Email (Email + SMS)

```json
{
  "task_id": "task_005",
  "user_id": "user_111",
  "date": "2024-01-19T10:00:00Z",
  "suggested_execution_time": "2024-01-19T10:00:00Z",
  "channel_priority": "email",
  "channel_secondary": "sms",
  "template_msg_id": "newsletter_template_001",
  "score": 60,
  "label": "Newsletter",
  "reason": "Comunicación regular",
  "created_at": "2024-01-14T08:00:00Z",
  "updated_at": "2024-01-14T08:00:00Z",
  "_created_at": "2024-01-14T08:00:00Z",
  "_updated_at": "2024-01-14T08:00:00Z"
}
```

## 🔄 Integración

### **Método por API REST**

#### Endpoint de Consulta
```http
GET /api/v1/tasks/
```

#### Parámetros de Consulta
- `from_date`: Fecha desde (YYYY-MM-DDTHH:mm:ssZ)
- `to_date`: Fecha hasta (YYYY-MM-DDTHH:mm:ssZ)
- `channel_priority`: Filtrar por canal ("salesman", "callcenter")

#### Ejemplo de Request
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?from_date=2024-01-01T00:00:00Z&channel_priority=salesman" \
  -H "X-API-Key: your_api_key"
```

### **Método por Base de Datos**

#### Consulta Básica
```sql
SELECT
    task_id,
    user_id,
    date,
    suggested_execution_time,
    channel_priority,
    channel_secondary,
    score,
    label,
    reason,
    created_at,
    updated_at,
    _created_at,
    _updated_at
FROM tasks
WHERE _updated_at > '2024-01-01T00:00:00Z'
ORDER BY _updated_at ASC;
```

#### Consulta por Tipo de Tarea
```sql
-- Tareas de vendedores
SELECT * FROM tasks
WHERE channel_priority = 'salesman'
AND _updated_at > '2024-01-01T00:00:00Z';

-- Tareas de call center
SELECT * FROM tasks
WHERE channel_priority = 'callcenter'
AND _updated_at > '2024-01-01T00:00:00Z';
```

### **Sincronización Incremental**

```sql
-- Obtener solo tareas actualizadas desde la última sincronización
SELECT * FROM tasks
WHERE _updated_at > :last_sync_timestamp
ORDER BY _updated_at ASC;
```

### **Python para Integración**

```python
import requests
from datetime import datetime, timedelta

class RetenTasksClient:
    def __init__(self, api_key):
        self.base_url = "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app"
        self.api_key = api_key

    def get_tasks(self, from_date=None, to_date=None, channel_priority=None):
        headers = {'X-API-Key': self.api_key}
        params = {}

        if from_date:
            params['from_date'] = from_date
        if to_date:
            params['to_date'] = to_date
        if channel_priority:
            params['channel_priority'] = channel_priority

        response = requests.get(f'{self.base_url}/api/v1/tasks/', headers=headers, params=params)
        return response.json()

# Uso
client = RetenTasksClient("your_api_key")

# Obtener todas las tareas de hoy
today = datetime.now().strftime('%Y-%m-%dT%H:%M:%SZ')
yesterday = (datetime.now() - timedelta(days=1)).strftime('%Y-%m-%dT%H:%M:%SZ')

tasks = client.get_tasks(from_date=yesterday, to_date=today)

# Obtener solo tareas de vendedores
salesman_tasks = client.get_tasks(channel_priority='salesman')
```
