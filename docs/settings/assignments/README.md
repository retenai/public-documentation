# Asignaciones

Las asignaciones en Reten definen la relación entre vendedores y los clientes que tienen a su cargo. Esta entidad permite gestionar qué vendedor es responsable de cada cliente, facilitando la organización del trabajo en terreno y el seguimiento de las relaciones comerciales.

## Estructura de Datos

```json
{
  // Identificadores
  "assignment_id": "string",      // Identificador único de la asignación (not null)
  "external_id": "string",        // Identificador externo del cliente
  "seller_id": "string",         // Identificador del vendedor (not null)
  "client_id": "string",          // Identificador del cliente (not null)
  
  // Información básica
  "name": {
    "display_name": "string",    // Nombre para mostrar (not null)
    "short_name": "string"       // Nombre corto (opcional)
  },
  "description": "string",       // Descripción de la asignación
  
  // Configuración de la asignación
  "role": {
    "type": "string",           // Tipo de rol: "primary", "secondary", "temporary"
    "permissions": ["string"]    // Permisos específicos para este cliente
  },

  // Período de vigencia
  "validity_period": {
    "start_date": "date",       // Fecha de inicio de la asignación
    "end_date": "date",         // Fecha de término (null si es indefinida)
    "is_permanent": "boolean"    // Indica si la asignación es permanente
  },

  // Métricas y objetivos
  "targets": {
    "visit_frequency": "number", // Frecuencia esperada de visitas (días)
    "sales_goal": "number",      // Meta de ventas
    "custom_kpis": [{           // KPIs personalizados
      "name": "string",
      "value": "number",
      "unit": "string"
    }]
  },
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",    // Fecha de creación en Reten (not null)
  "_updated_at": "timestamp",    // Fecha de última actualización en Reten (not null)

  // Estado
  "status": "string",            // Estado actual de la asignación
  "is_active": "boolean",        // Indicador de estado activo

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"            // Tipo de valor (string, number, date, boolean)
  }]
}
```

## Campos Detallados

### Identificadores

| Campo         | Tipo   | Requerido | Descripción                             |
| ------------- | ------ | --------- | --------------------------------------- |
| assignment_id | string | Sí        | Identificador único en Reten            |
| external_id   | string | No        | Identificador en el sistema del cliente |
| seller_id     | string | Sí        | Identificador del vendedor              |
| client_id     | string | Sí        | Identificador del cliente               |

### Información Básica

| Campo             | Tipo   | Requerido | Descripción                   |
| ----------------- | ------ | --------- | ----------------------------- |
| name.display_name | string | Sí        | Nombre completo de asignación |
| name.short_name   | string | No        | Nombre corto para referencia  |
| description       | string | No        | Descripción detallada         |
| role.type         | string | Sí        | Tipo de rol en la asignación  |

### Estado

**Estados Válidos:**
- `active`: Asignación activa y vigente
- `inactive`: Asignación temporalmente suspendida
- `pending`: Asignación pendiente de inicio
- `completed`: Asignación finalizada
- `transferred`: Asignación transferida a otro vendedor

## Validaciones

### Validaciones Generales

#### Identificadores
- `assignment_id` debe ser único en todo el sistema
- `external_id` debe ser único por cliente
- `seller_id` debe corresponder a un vendedor existente y activo
- `client_id` debe corresponder a un cliente existente y activo

#### Fechas
- `validity_period.start_date` no puede ser posterior a `validity_period.end_date`
- `validity_period.end_date` debe ser posterior a la fecha actual si está definida
- Si `is_permanent` es true, `end_date` debe ser null

#### Roles y Permisos
- `role.type` debe ser uno de los tipos válidos
- Los permisos deben ser consistentes con el tipo de rol
- Un cliente debe tener al menos un vendedor con rol "primary"

### Validaciones de Negocio

- Un cliente no puede tener más de un vendedor con rol "primary" activo
- Un vendedor no puede tener asignaciones superpuestas para el mismo cliente
- Las metas y KPIs deben ser valores positivos
- La frecuencia de visitas debe ser coherente con la capacidad del vendedor

## Ejemplos de Uso

### Ejemplo Básico - Asignación Principal

```json
{
  "assignment_id": "ASG_001",
  "external_id": "EXT_123",
  "seller_id": "SLR_001",
  "client_id": "CLT_001",
  "name": {
    "display_name": "Asignación Principal Zona Norte",
    "short_name": "Norte-Principal"
  },
  "role": {
    "type": "primary",
    "permissions": ["visit", "sell", "collect", "report"]
  },
  "validity_period": {
    "start_date": "2024-03-01",
    "is_permanent": true
  },
  "targets": {
    "visit_frequency": 7,
    "sales_goal": 1000000
  },
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "_created_at": "2024-03-19T10:00:00Z",
  "_updated_at": "2024-03-19T10:00:00Z",
  "status": "active",
  "is_active": true,
  "attributes": []
}
```

### Ejemplo Completo - Asignación Temporal

```json
{
  "assignment_id": "ASG_002",
  "external_id": "EXT_124",
  "seller_id": "SLR_002",
  "client_id": "CLT_001",
  "name": {
    "display_name": "Reemplazo Temporal Vacaciones",
    "short_name": "Reemplazo-Verano"
  },
  "description": "Asignación temporal por período de vacaciones del vendedor principal",
  "role": {
    "type": "temporary",
    "permissions": ["visit", "sell"]
  },
  "validity_period": {
    "start_date": "2024-01-15",
    "end_date": "2024-02-15",
    "is_permanent": false
  },
  "targets": {
    "visit_frequency": 7,
    "sales_goal": 800000,
    "custom_kpis": [{
      "name": "customer_satisfaction",
      "value": 4.5,
      "unit": "stars"
    }]
  },
  "created_at": "2024-01-10T10:00:00Z",
  "updated_at": "2024-01-10T10:00:00Z",
  "_created_at": "2024-01-10T10:00:00Z",
  "_updated_at": "2024-01-10T10:00:00Z",
  "status": "completed",
  "is_active": false,
  "attributes": [{
    "key": "replacement_reason",
    "value": "vacation",
    "type": "string"
  }]
}
```

## Notas de Implementación

### Gestión de Cambios

#### Actualización de Datos
- Los cambios en asignaciones activas deben notificar a los vendedores afectados
- Mantener historial de cambios de asignaciones
- Registrar motivos de cambios de estado

#### Transiciones
- Documentar proceso de transferencia entre vendedores
- Manejar período de transición para cambios permanentes
- Gestionar solapamiento en asignaciones temporales

### Consideraciones de Seguridad

#### Protección de Datos
- Validar permisos por territorio/zona
- Registrar accesos y modificaciones
- Proteger información comercial sensible

#### Compliance
- Respetar límites de carga de trabajo
- Mantener registro de asignaciones históricas
- Validar conflictos de interés

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales
```
GET    /api/v1/assignments
GET    /api/v1/assignments/{assignment_id}
POST   /api/v1/assignments
PUT    /api/v1/assignments/{assignment_id}
DELETE /api/v1/assignments/{assignment_id}

GET    /api/v1/sellers/{seller_id}/assignments
GET    /api/v1/clients/{client_id}/assignments
```

### Webhooks

#### Eventos Disponibles
- `assignment.created`
- `assignment.updated`
- `assignment.deleted`
- `assignment.status_changed`
- `assignment.transferred`

#### Formato de Payload
```json
{
  "event": "assignment.updated",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "assignment_id": "string",
    "changes": [{
      "field": "string",
      "old_value": "any",
      "new_value": "any"
    }]
  }
}
```

## Preguntas Frecuentes

**¿Cómo manejar reemplazos temporales?**
- Crear nueva asignación con rol "temporary"
- Definir período de vigencia específico
- Mantener asignación principal en estado "active"

**¿Cómo gestionar transferencias permanentes?**
- Finalizar asignación actual (status: "transferred")
- Crear nueva asignación con nuevo vendedor
- Mantener historial de transferencia

**¿Cómo afectan las asignaciones a las rutas?**
- Las rutas solo pueden incluir clientes asignados al vendedor
- Los cambios en asignaciones pueden requerir actualización de rutas
- Se validan permisos de visita antes de crear rutas
