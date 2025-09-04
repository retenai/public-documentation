# :material-api: Consumo desde API REST de Reten

!!! info "📊 API de Analytics en Producción"
    Esta documentación describe cómo consumir datos desde la API REST de Reten que incluye endpoints para tareas, KPIs, mensajes y contactos de usuarios. Disponible en [https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs](https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs).


### **Características Principales**

- **🔐 Autenticación**: API Key basada en headers
- **📡 Protocolo**: HTTP/HTTPS con RESTful endpoints
- **📊 Analytics**: Datos de tareas, KPIs, mensajes y usuarios
- **📋 Consulta**: Filtrado por fechas, prioridades y canales
- **📊 Formato**: JSON para request/response
- **🛡️ Seguridad**: Autenticación por API Key, HTTPS obligatorio

### **Base URL**

```
https://retenai-analytics-api-lgtxgindmq-tl.a.run.app
```

### **Autenticación**

```http
X-API-Key: <your_api_key>
Content-Type: application/json
```

!!! tip "Obtención de API Key"
    Para obtener tu API Key, contacta al equipo de Reten o solicita acceso a través del portal de desarrolladores.

## 📋 Endpoints Disponibles

!!! info "Otros Endpoints Disponibles"
    La API también incluye endpoints para KPIs, mensajes y contactos de usuarios. Consulta la [documentación completa](https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs) para más detalles.

### **Consulta de Tareas**

#### **Listar Tareas Programadas**
```http
GET /api/v1/tasks/
```

**Parámetros de Query:**

- `from_date`: Fecha desde (ISO 8601, opcional)
- `to_date`: Fecha hasta (ISO 8601, opcional)
- `channel_priority`: Prioridad del canal ("salesman", "callcenter", opcional)

**Ejemplo de Request:**
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?from_date=2024-01-01T00:00:00Z&channel_priority=salesman" \
  -H "X-API-Key: your_api_key"
```

**Ejemplo de Respuesta:**
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

!!! info "Documentación Interactiva"
    Para consultar la documentación completa del endpoint GET /api/v1/tasks, visita: [https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs#/tasks/get_task_api_v1_tasks__get](https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs#/tasks/get_task_api_v1_tasks__get)

### **Consulta de Estados de Usuario**

#### **Listar Estados de Usuario**
```http
GET /api/v1/kpis/users-states
```

**Parámetros de Query:**

- `page`: Número de página (por defecto: 1)
- `limit`: Número de elementos por página (por defecto: 100)
- `kpi_value`: Filtrar por valor del KPI (opcional)
- `date`: Filtrar por fecha (YYYY-MM-DD, opcional)
- `user_id`: Filtrar por ID de usuario (opcional)

**Ejemplo de Request:**
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/kpis/users-states?page=1&limit=50&date=2024-01-15" \
  -H "X-API-Key: your_api_key"
```

**Ejemplo de Respuesta:**
```json
{
  "page": 1,
  "limit": 50,
  "total": 1000,
  "next_page": 2,
  "previous_page": null,
  "data": [
    {
      "user_id": "user_123",
      "kpi_name": "user_state",
      "kpi_value": "core",
      "date": "2024-01-15",
      "segment_name": "engagement_level",
      "segment_value": "high"
    },
    {
      "user_id": "user_456",
      "kpi_name": "user_state",
      "kpi_value": "churned",
      "date": "2024-01-15",
      "segment_name": "engagement_level",
      "segment_value": "low"
    }
  ]
}
```

!!! info "Documentación Interactiva"
    Para consultar la documentación completa del endpoint GET /api/v1/kpis/users-states, visita: [https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs#/kpis/get_kpis_users_states_api_v1_kpis_users_states_get](https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs#/kpis/get_kpis_users_states_api_v1_kpis_users_states_get)

## 🔧 Ejemplos de Uso

### **Python**
```python
import requests

API_BASE_URL = "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app"
API_KEY = "your_api_key"

# Consultar todas las tareas
response = requests.get(f'{API_BASE_URL}/api/v1/tasks/', headers={'X-API-Key': API_KEY})
tasks = response.json()

# Consultar tareas por fecha
params = {'from_date': '2024-01-01T00:00:00Z', 'to_date': '2024-01-31T23:59:59Z'}
response = requests.get(f'{API_BASE_URL}/api/v1/tasks/', headers={'X-API-Key': API_KEY}, params=params)
tasks_filtradas = response.json()

# Consultar tareas por canal de prioridad
params = {'channel_priority': 'salesman'}
response = requests.get(f'{API_BASE_URL}/api/v1/tasks/', headers={'X-API-Key': API_KEY}, params=params)
tasks_salesman = response.json()

# Consultar estados de usuario
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers={'X-API-Key': API_KEY})
user_states = response.json()

# Consultar estados de usuario por fecha
params = {'date': '2024-01-15', 'limit': 100}
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers={'X-API-Key': API_KEY}, params=params)
user_states_filtrados = response.json()

# Consultar estados de usuario específico
params = {'user_id': 'user_123'}
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers={'X-API-Key': API_KEY}, params=params)
user_states_especifico = response.json()
```

### **cURL**
```bash
# Consultar todas las tareas
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/" \
  -H "X-API-Key: your_api_key"

# Consultar tareas por rango de fechas
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?from_date=2024-01-01T00:00:00Z&to_date=2024-01-31T23:59:59Z" \
  -H "X-API-Key: your_api_key"

# Consultar tareas por canal de prioridad
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/tasks/?channel_priority=salesman" \
  -H "X-API-Key: your_api_key"

# Consultar estados de usuario
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/kpis/users-states" \
  -H "X-API-Key: your_api_key"

# Consultar estados de usuario por fecha
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/kpis/users-states?date=2024-01-15&limit=100" \
  -H "X-API-Key: your_api_key"

# Consultar estados de usuario específico
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/kpis/users-states?user_id=user_123" \
  -H "X-API-Key: your_api_key"
```

### **Documentación Completa**
[https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs](https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/docs)
