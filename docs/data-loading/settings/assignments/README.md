# :material-account-multiple: Asignaciones

Las asignaciones en Reten definen la relación entre vendedores y los usuarios que tienen a su cargo. Esta entidad permite gestionar qué vendedor es responsable de cada usuario, facilitando la organización del trabajo en terreno y el seguimiento de las relaciones comerciales.

<div style="text-align: center;">

```mermaid
erDiagram
    direction LR

    %% Estilos personalizados para las entidades - bordes redondeados y solo relleno
    classDef sellerClass fill:#e8f5e8,stroke:none,color:#000,rx:8,ry:8
    classDef assignmentClass fill:#fce4ec,stroke:none,color:#000,rx:8,ry:8
    classDef userClass fill:#fff3e0,stroke:none,color:#000,rx:8,ry:8

    Vendedor ||--o{ Asignacion : "es responsable de"
    Asignacion }o--|| Usuario : "es gestionado por"

    Asignacion {
    }

    Vendedor {
    }

    Usuario {
    }

    %% Aplicar estilos
    class Asignacion assignmentClass
    class Vendedor sellerClass
    class Usuario userClass
```

</div>

La asignación actúa como un **conector inteligente** que:

- **Define la responsabilidad** de un vendedor sobre un usuario específico
- **Establece el tipo de relación** (principal, secundaria, temporal)
- **Controla el estado** de la relación comercial
- **Permite la gestión** de múltiples vendedores por usuario
- **Facilita la transferencia** de usuarios entre vendedores

Un vendedor puede tener múltiples asignaciones de usuarios, y un usuario puede ser gestionado por diferentes vendedores según el tipo de asignación.

## Estructura de Datos

```json
{
  // Identificadores
  "assignment_id": "string",      // Identificador único de la asignación (not null)
  "seller_id": "string",         // Identificador del vendedor (not null)
  "user_id": "string",          // Identificador del usuario (not null)
  
  // Información básica
  "description": "string",       // Descripción de la asignación
  "type": "string",             // Tipo de asignación: "primary", "secondary", "temporary"
  
  // Estado
  "status": "string",            // Estado actual de la asignación (active, inactive, pending, completed, transferred)

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"            // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",    // Fecha de creación (not null)
  "updated_at": "timestamp"    // Fecha de última actualización (not null)
}
```

## Campos Detallados

### Identificadores

| Campo         | Tipo   | Requerido | Descripción                  |
| ------------- | ------ | --------- | ---------------------------- |
| assignment_id | string | Sí        | Identificador único en Reten |
| seller_id     | string | Sí        | Identificador del vendedor   |
| user_id       | string | Sí        | Identificador del usuario    |

### Información Básica

| Campo       | Tipo   | Requerido | Descripción           |
| ----------- | ------ | --------- | --------------------- |
| description | string | No        | Descripción detallada |
| type        | string | Sí        | Tipo de asignación    |

**Tipos de Asignación:**
- `primary`: Vendedor principal del usuario
- `secondary`: Vendedor secundario o de apoyo
- `temporary`: Asignación temporal o de reemplazo

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
- `seller_id` debe corresponder a un vendedor existente y activo
- `user_id` debe corresponder a un usuario existente y activo

#### Tipos y Estados
- `type` debe ser uno de los tipos válidos
- Un usuario debe tener al menos un vendedor con tipo "primary"

### Validaciones de Negocio

- Un usuario no puede tener más de un vendedor con tipo "primary" activo
- Un vendedor no puede tener asignaciones superpuestas para el mismo usuario

## Ejemplos de Uso

### Ejemplo Básico - Asignación Principal

```json
{
  "assignment_id": "ASG_001",
  "seller_id": "SLR_001",
  "user_id": "CLT_001",
  "description": "Asignación Principal Zona Norte",
  "type": "primary",
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "status": "active",
  "attributes": []
}
```

### Ejemplo Completo - Asignación Temporal

```json
{
  "assignment_id": "ASG_002",
  "seller_id": "SLR_002",
  "user_id": "CLT_001",
  "description": "Reemplazo temporal por período de vacaciones del vendedor principal",
  "type": "temporary",
  "created_at": "2024-01-10T10:00:00Z",
  "updated_at": "2024-01-10T10:00:00Z",
  "status": "completed",
  "attributes": [{
    "key": "replacement_reason",
    "value": "vacation",
    "type": "string"
  }]
}
```

### Asignación con Período

```json
{
  "assignment_id": "ASG_002",
  "seller_id": "SELLER_002",
  "route_id": "ROUTE_002",
  "status": "scheduled",
  "period": {
    "start_date": "2024-01-01",
    "end_date": "2024-12-31"
  },
  "created_at": "2024-01-10T10:00:00Z",
  "updated_at": "2024-01-10T10:00:00Z"
}
```
