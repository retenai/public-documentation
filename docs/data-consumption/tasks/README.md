# :material-checkbox-marked-circle: Tareas

Las tareas representan los momentos de contacto planificados con los clientes en el sistema Reten. Esta entidad maneja diferentes tipos de interacciones identificadas por campos específicos, permitiendo una gestión unificada de todas las actividades de outreach.

## Estructura de Datos

```json
{
  // Identificadores (requeridos)
  "id": "string",                      // Identificador único de la tarea (not null)
  "user_id": "string",                 // ID del usuario objetivo (not null)

  // Información temporal
  "date": "timestamp",                 // Fecha programada para la tarea (not null)
  "suggested_execution_time": "timestamp", // Hora sugerida de ejecución (not null)
  "created_at": "timestamp",           // Fecha de creación (not null)
  "updated_at": "timestamp",           // Última actualización

  // Información del canal y prioridad
  "channel_priority": "string",        // Canal principal: "salesman", "callcenter" (not null)
  "channel_secondary": "string",       // Canal secundario: "whatsapp", "email", "sms"

  // Información de negocio
  "template_msg_id": "string",         // ID de plantilla de mensaje
  "score": "number",                   // Puntaje/prioridad de la tarea
  "label": "string",                   // Etiqueta descriptiva
  "reason": "string",                  // Motivo específico de la tarea

  // Atributos personalizados - Cualquier campo adicional se almacenará aquí
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",           // Fecha de creación del registro en Reten
  "_updated_at": "timestamp"            // Fecha de última actualización del registro en Reten
}
```

## Campos Detallados

### Identificadores

| Campo   | Tipo   | Requerido | Descripción                      |
| ------- | ------ | --------- | -------------------------------- |
| id      | string | Sí        | Identificador único de la tarea  |
| user_id | string | Sí        | ID del usuario objetivo          |

### Información Temporal

| Campo                   | Tipo      | Requerido | Descripción                           |
| ----------------------- | --------- | --------- | ------------------------------------- |
| date                    | timestamp | Sí        | Fecha programada para la tarea        |
| suggested_execution_time| timestamp | Sí        | Hora sugerida de ejecución            |
| created_at              | timestamp | Sí        | Fecha de creación en el sistema       |
| updated_at              | timestamp | No        | Última actualización                  |

### Información del Canal

| Campo             | Tipo   | Requerido | Descripción                              |
| ----------------- | ------ | --------- | ---------------------------------------- |
| channel_priority  | string | Sí        | Canal principal: "salesman", "callcenter"|
| channel_secondary | string | No        | Canal secundario: "whatsapp", "email", "sms"|

### Información de Negocio

| Campo            | Tipo   | Requerido | Descripción                              |
| ---------------- | ------ | --------- | ---------------------------------------- |
| template_msg_id  | string | No        | ID de plantilla de mensaje               |
| score            | number | No        | Puntaje/prioridad de la tarea            |
| label            | string | No        | Etiqueta descriptiva                     |
| reason           | string | No        | Motivo específico de la tarea            |

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el cliente**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan de la API de analytics y que no estén definidas en el modelo estándar de tareas.

### **Cómo Funciona:**
1. **API retorna** datos con campos adicionales (ej: `custom_field`, `priority_level`, `campaign_id`)
2. **Reten detecta** automáticamente las propiedades no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas propiedades adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos del negocio**: Información particular de cada tipo de tarea
- **Metadatos de integración**: Datos específicos de la API de analytics
- **Configuraciones personalizadas**: Parámetros únicos por tipo de tarea

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "campaign_id",
    "value": "SUMMER2024",
    "type": "string"
  },
  {
    "key": "priority_level",
    "value": "5",
    "type": "number"
  },
  {
    "key": "custom_field",
    "value": "valor_personalizado",
    "type": "string"
  }
]
```

### **Tipos de Datos Soportados:**
- `string`: Texto libre
- `number`: Números enteros o decimales
- `date`: Fechas en formato ISO 8601
- `boolean`: Valores true/false

### **Ventajas:**
- **Flexibilidad total** para adaptarse a diferentes tipos de tareas
- **Extensibilidad** sin modificar el esquema principal
- **Procesamiento automático** sin intervención del cliente

## Validaciones

### Validaciones Generales

#### Identificadores
- `id` debe ser único en todo el sistema
- `user_id` debe corresponder a un usuario existente

#### Fechas
- `date` y `suggested_execution_time` deben ser fechas válidas en el futuro
- `created_at` no puede ser posterior a `updated_at`
- Todas las fechas deben estar en formato ISO 8601

#### Canales
- `channel_priority` debe ser uno de: "salesman", "callcenter"
- `channel_secondary` debe ser consistente con el canal principal
- `channel_secondary` debe ser uno de: "whatsapp", "email", "sms"

#### Información de Negocio
- `score` debe ser un número entre 0 y 100 (si se proporciona)
- `template_msg_id` debe existir en el sistema de plantillas (si se proporciona)

## Ejemplos de Uso

### Tarea de Visita de Vendedor

```json
{
  "id": "task_001",
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
  "attributes": [
    {
      "key": "visit_type",
      "value": "follow_up",
      "type": "string"
    },
    {
      "key": "expected_duration",
      "value": "30",
      "type": "number"
    }
  ],
  "_created_at": "2024-01-10T09:00:00Z",
  "_updated_at": "2024-01-10T09:00:00Z"
}
```

### Tarea de Llamada de Call Center

```json
{
  "id": "task_002",
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
  "attributes": [
    {
      "key": "call_priority",
      "value": "high",
      "type": "string"
    },
    {
      "key": "max_attempts",
      "value": "3",
      "type": "number"
    }
  ],
  "_created_at": "2024-01-11T08:30:00Z",
  "_updated_at": "2024-01-11T08:30:00Z"
}
```

### Tarea con Atributos Personalizados

```json
{
  "id": "task_003",
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
  "attributes": [
    {
      "key": "campaign_id",
      "value": "SUMMER2024",
      "type": "string"
    },
    {
      "key": "priority_level",
      "value": "5",
      "type": "number"
    },
    {
      "key": "custom_field",
      "value": "valor_personalizado",
      "type": "string"
    },
    {
      "key": "is_urgent",
      "value": "true",
      "type": "boolean"
    }
  ],
  "_created_at": "2024-01-12T10:15:00Z",
  "_updated_at": "2024-01-12T10:15:00Z"
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
    id,
    user_id,
    date,
    suggested_execution_time,
    channel_priority,
    channel_secondary,
    score,
    label,
    reason,
    attributes,
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
