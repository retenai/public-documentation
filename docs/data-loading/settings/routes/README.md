# :material-map-marker-path: Rutas

Las rutas en Reten definen cómo los vendedores visitan sus clientes asignados, permitiendo una planificación eficiente de tareas y visitas. Una ruta representa ya sea un patrón recurrente de visitas o fechas específicas programadas para visitar los clientes en la cartera del vendedor.

## Modelo de Relaciones

```mermaid

erDiagram
    direction LR
    
    Vendedor ||--o{ Ruta : tiene_asignada
    Ruta ||--o{ Cliente : visita

    Vendedor {
        string seller_id PK
        string route_id FK
    }

    Ruta {
        string route_id PK
        string client_id FK
        string pattern_type
        string status
    }

    Cliente {
        string client_id PK
    }
```

## Estructura de Datos

```json
{
  // Identificadores
  "route_id": "string",         // Identificador principal de la ruta (not null)
  "client_id": "string",        // Identificador del cliente a visitar (not null)
  
  // Información básica
  "description": "string",      // Descripción de la ruta
  
  // Configuración de la ruta
  "pattern_type": "string",     // Tipo de patrón: "recurring" o "specific" (not null)
  "visit_date": "date",         // Fecha específica de visita (requerido si pattern_type es "specific")
  "recurring_pattern": {        // Patrón recurrente (requerido si pattern_type es "recurring")
    "frequency": "string",      // "weekly", "monthly", "custom"
    "days": ["string"],        // Días de la semana ["MONDAY", "TUESDAY", etc.]
    "week_of_month": "number",  // Para patrones mensuales (1-5)
    "interval": "number",       // Número de unidades entre visitas (default: 1)
    "start_date": "date"       // Fecha de inicio del patrón (determina la secuencia de semanas)
  },

  // Estado
  "status": "string",           // Estado actual de la ruta (active, inactive, archived, draft)

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"           // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp"    // Última actualización (not null)
}
```

## Campos Detallados

### Identificadores

| Campo     | Tipo   | Requerido | Descripción                         |
| --------- | ------ | --------- | ----------------------------------- |
| route_id  | string | Sí        | Identificador único en Reten        |
| client_id | string | Sí        | Identificador del cliente a visitar |

### Información Básica

| Campo        | Tipo   | Requerido | Descripción               |
| ------------ | ------ | --------- | ------------------------- |
| description  | string | No        | Descripción detallada     |
| pattern_type | string | Sí        | Tipo de patrón de visitas |

### Configuración de la Ruta

#### Patrón Específico
| Campo      | Tipo | Requerido | Descripción                                   |
| ---------- | ---- | --------- | --------------------------------------------- |
| visit_date | date | Sí*       | Fecha específica de visita (formato ISO 8601) |

\* Requerido cuando `pattern_type` es "specific"

#### Patrón Recurrente
| Campo         | Tipo     | Requerido | Descripción                                                     |
| ------------- | -------- | --------- | --------------------------------------------------------------- |
| frequency     | string   | Sí*       | Frecuencia: "weekly", "monthly", "custom"                       |
| days          | string[] | Sí*       | Días de la semana: ["MONDAY", "TUESDAY", etc.]                  |
| week_of_month | number   | No        | Semana del mes (1-5) para patrones mensuales                    |
| interval      | number   | No        | Número de unidades (semanas/meses) entre visitas. Por defecto 1 |
| start_date    | date     | No        | Fecha de inicio del patrón. Define la secuencia de semanas      |

\* Requerido cuando `pattern_type` es "recurring"

### Estado

**Estados Válidos:**
- `active`: Ruta activa y en uso
- `inactive`: Ruta temporalmente desactivada
- `archived`: Ruta archivada y no disponible para uso
- `draft`: Ruta en proceso de creación

## Validaciones

### Validaciones Generales

#### Identificadores
- `route_id` debe ser único en todo el sistema
- `client_id` debe corresponder a un cliente existente y asignado al vendedor

#### Fechas
- `created_at` no puede ser posterior a `updated_at`
- `visit_date` debe ser válida y en formato ISO 8601 cuando pattern_type es "specific"
- Las fechas específicas de visita no pueden ser en el pasado

#### Patrones
- Si `pattern_type` es "specific", debe tener una `visit_date` válida
- Si `pattern_type` es "recurring", debe tener un `recurring_pattern` válido
- Los días en `recurring_pattern.days` deben ser días válidos de la semana

### Validaciones de Negocio

- Un vendedor no puede tener visitas programadas simultáneas en diferentes clientes
- El cliente debe pertenecer a la cartera del vendedor
- Los patrones recurrentes no pueden tener intervalos menores a 1 día

## Ejemplos de Uso

### Ejemplo Básico - Ruta Recurrente

```json
{
  // Identificadores
  "route_id": "RTE_001",
  "client_id": "CLT_001",
  
  // Información básica
  "description": "Visita semanal cliente principal zona norte",
  
  // Configuración de la ruta
  "pattern_type": "recurring",
  "recurring_pattern": {
    "frequency": "weekly",
    "days": ["MONDAY", "THURSDAY"],
    "interval": 1  // Visita todas las semanas
  },

  // Estado
  "status": "active",

  // Atributos personalizados
  "attributes": [],

  // Marcas temporales
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplo - Ruta Bi-semanal

```json
{
  // Identificadores
  "route_id": "RTE_003",
  "client_id": "CLT_003",
  
  // Información básica
  "description": "Visita quincenal cliente secundario",
  
  // Configuración de la ruta
  "pattern_type": "recurring",
  "recurring_pattern": {
    "frequency": "weekly",
    "days": ["WEDNESDAY"],
    "interval": 2,  // Visita cada dos semanas
    "start_date": "2024-01-10"  // Comienza en semana 2, por lo que las visitas serán en semanas pares
  },

  // Estado
  "status": "active",

  // Atributos personalizados
  "attributes": [],

  // Marcas temporales
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplo - Ruta con Patrón Personalizado

```json
{
  // Identificadores
  "route_id": "RTE_004",
  "client_id": "CLT_004",
  
  // Información básica
  "description": "Visita en días específicos del mes",
  
  // Configuración de la ruta
  "pattern_type": "recurring",
  "recurring_pattern": {
    "frequency": "custom",
    "days": ["MONDAY", "THURSDAY"],  // Días permitidos
    "week_of_month": [1, 3],         // Primera y tercera semana
    "start_date": "2024-01-01"       // Fecha de inicio del patrón
  },

  // Estado
  "status": "active",

  // Atributos personalizados
  "attributes": [{
    "key": "visit_duration",
    "value": "60",
    "type": "number"
  }],

  // Marcas temporales
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplo Completo - Ruta con Fecha Específica

```json
{
  // Identificadores
  "route_id": "RTE_002",
  "client_id": "CLT_002",
  
  // Información básica
  "description": "Visita especial campaña navideña",
  
  // Configuración de la ruta
  "pattern_type": "specific",
  "visit_date": "2024-12-01",

  // Estado
  "status": "active",

  // Atributos personalizados
  "attributes": [{
    "key": "campaign",
    "value": "navidad_2024",
    "type": "string"
  }, {
    "key": "priority",
    "value": "1",
    "type": "number"
  }],

  // Marcas temporales
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```
