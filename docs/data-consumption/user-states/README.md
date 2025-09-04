# :material-account-group: Estados de Usuario

Los estados de usuario representan la clasificación de los usuarios según su comportamiento y engagement con la plataforma Reten. Esta entidad permite segmentar a los usuarios en diferentes categorías que reflejan su nivel de actividad, fidelidad y potencial de conversión.

## Estructura de Datos

```json
{
  // Identificadores
  "user_id": "string",                 // Identificador único del usuario (not null)
  "kpi_name": "string",                // Nombre del KPI, siempre "user_state" (not null)

  // Información del estado
  "kpi_value": "string",               // Estado del usuario (not null)
                                       // Valores posibles: "aha", "casual", "churned", "core", "dormant", "habit", "power", "sign_up", "setup"

  // Información temporal
  "date": "string",                    // Fecha del estado en formato YYYY-MM-DD (not null)

  // Información de segmentación (opcional)
  "segment_name": "string",            // Nombre del segmento adicional
  "segment_value": "string"            // Valor del segmento adicional
}
```

## Campos Detallados

### Identificadores

| Campo    | Tipo   | Requerido | Descripción                           |
| -------- | ------ | --------- | ------------------------------------- |
| user_id  | string | Sí        | Identificador único del usuario       |
| kpi_name | string | Sí        | Nombre del KPI (siempre "user_state") |

### Información del Estado

| Campo     | Tipo   | Requerido | Descripción                                        |
| --------- | ------ | --------- | -------------------------------------------------- |
| kpi_value | string | Sí        | Estado actual del usuario según el modelo de Reten |

### Información Temporal

| Campo | Tipo   | Requerido | Descripción                                      |
| ----- | ------ | --------- | ------------------------------------------------ |
| date  | string | Sí        | Fecha en que se determinó el estado (YYYY-MM-DD) |

### Información de Segmentación

| Campo         | Tipo   | Requerido | Descripción                   |
| ------------- | ------ | --------- | ----------------------------- |
| segment_name  | string | No        | Nombre del segmento adicional |
| segment_value | string | No        | Valor del segmento adicional  |

## Estados de Usuario Disponibles

Los estados de usuario representan diferentes niveles de engagement y comportamiento:

### **Estados Principales**

| Estado      | Descripción                                                     |
| ----------- | --------------------------------------------------------------- |
| **sign_up** | Usuario recién registrado, sin actividad significativa          |
| **aha**     | Usuario que ha tenido su primer momento "AHA" de descubrimiento |
| **casual**  | Usuario con actividad esporádica y baja frecuencia              |
| **habit**   | Usuario que ha desarrollado un hábito de uso regular            |
| **core**    | Usuario altamente comprometido, núcleo del negocio              |
| **power**   | Usuario power user con máxima actividad y engagement            |
| **setup**   | Usuario en proceso de configuración inicial                     |

### **Estados de Riesgo**

| Estado      | Descripción                             |
| ----------- | --------------------------------------- |
| **dormant** | Usuario inactivo que podría reactivarse |
| **churned** | Usuario que ha abandonado la plataforma |

## Validaciones

### Validaciones Generales

#### Identificadores
- `user_id` debe ser único y corresponder a un usuario existente en el sistema
- `kpi_name` debe ser siempre "user_state"

#### Estado del Usuario
- `kpi_value` debe ser uno de los valores permitidos del enum de estados
- Los estados siguen una jerarquía lógica de engagement creciente

#### Fechas
- `date` debe estar en formato YYYY-MM-DD
- No puede ser una fecha futura

#### Segmentación
- Si se proporciona `segment_name`, debe existir `segment_value`
- Los segmentos adicionales son opcionales y dependen de la configuración del negocio

## Ejemplos de Uso

### Usuario Core Activo

```json
{
  "user_id": "user_123",
  "kpi_name": "user_state",
  "kpi_value": "core",
  "date": "2024-01-15",
  "segment_name": "engagement_level",
  "segment_value": "high"
}
```

### Usuario Nuevo en Setup

```json
{
  "user_id": "user_456",
  "kpi_name": "user_state",
  "kpi_value": "setup",
  "date": "2024-01-14"
}
```

### Usuario Churned

```json
{
  "user_id": "user_789",
  "kpi_name": "user_state",
  "kpi_value": "churned",
  "date": "2024-01-10",
  "segment_name": "churn_reason",
  "segment_value": "inactivity"
}
```

### Usuario Power User

```json
{
  "user_id": "user_999",
  "kpi_name": "user_state",
  "kpi_value": "power",
  "date": "2024-01-15",
  "segment_name": "lifetime_value",
  "segment_value": "premium"
}
```

### Usuario Casual

```json
{
  "user_id": "user_111",
  "kpi_name": "user_state",
  "kpi_value": "casual",
  "date": "2024-01-13",
  "segment_name": "frequency",
  "segment_value": "weekly"
}
```

### Usuario Dormant

```json
{
  "user_id": "user_222",
  "kpi_name": "user_state",
  "kpi_value": "dormant",
  "date": "2024-01-12",
  "segment_name": "days_since_last_activity",
  "segment_value": "60"
}
```

## 🔄 Integración

### **Método por API REST**

!!! info "📖 Documentación Completa de Integración"
    Para ejemplos detallados de implementación en Python, configuración de autenticación y estrategias avanzadas de consumo, consulta la documentación completa de la **[API REST](../../connection-methods/api-rest/README.md)**.

#### Endpoint Principal
```http
GET /api/v1/kpis/users-states
```

#### Parámetros Disponibles
- `page`: Número de página para paginación
- `limit`: Número de elementos por página (máximo recomendado: 100)
- `kpi_value`: Filtrar por estado específico del usuario
- `date`: Filtrar por fecha específica (YYYY-MM-DD)
- `user_id`: Filtrar por ID de usuario específico

#### Ejemplo de Consulta
```bash
curl -X GET "https://retenai-analytics-api-lgtxgindmq-tl.a.run.app/api/v1/kpis/users-states?date=2024-01-15&kpi_value=core" \
  -H "X-API-Key: your_api_key"
```

## Casos de Uso Comunes

### **Segmentación de Usuarios**
```python
# Obtener todos los usuarios core para campañas de upselling
params = {'kpi_value': 'core', 'limit': 1000}
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers=headers, params=params)
core_users = response.json()
```

### **Monitoreo de Churn**
```python
# Identificar usuarios en riesgo de churn
params = {'kpi_value': 'dormant', 'date': '2024-01-15'}
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers=headers, params=params)
dormant_users = response.json()
```

### **Análisis de Engagement**
```python
# Analizar distribución de estados de usuario
response = requests.get(f'{API_BASE_URL}/api/v1/kpis/users-states', headers=headers)
all_states = response.json()

# Contar usuarios por estado
state_counts = {}
for item in all_states['data']:
    state = item['kpi_value']
    state_counts[state] = state_counts.get(state, 0) + 1
```
