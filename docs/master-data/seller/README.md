# Vendedores

Los vendedores son los usuarios del CPG (Consumer Packaged Goods) que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los usuarios, sus roles, permisos y relaciones con comercios.

## Estructura de Datos

```json
{
  // Identificadores
  "seller_id": "string",         // Identificador principal (not null)
  "external_id": "string",       // Identificador externo del cliente
  "employee_id": "string",       // Identificador de empleado en CPG

  // Información básica
  "name": {
    "first_name": "string",      // Nombre (not null)
    "last_name": "string",       // Apellido (not null)
    "display_name": "string",    // Nombre para mostrar
    "full_name": "string"        // Nombre completo (not null)
  },
  "status": "string",            // Estado del vendedor (active, inactive, suspended)
  "is_active": "boolean",        // Indicador de estado activo
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",    // Fecha de creación en Reten
  "_updated_at": "timestamp",    // Fecha de última actualización en Reten
  "hire_date": "timestamp",      // Fecha de contratación (not null)
  "termination_date": "timestamp", // Fecha de término si aplica

  // Información de contacto
  "contacts": [{
    "email": "string",           // Email corporativo o personal
    "phone": "string",           // Número telefónico
    "type": "string",            // Tipo de contacto (work, personal)
    "is_primary": "boolean"      // Indica si es el contacto principal
  }],

  // Información de acceso
  "access": {
    "username": "string",        // Nombre de usuario (not null)
    "role": "string",            // Rol principal (not null)
    "permissions": ["string"],   // Lista de permisos específicos
    "last_login": "timestamp",   // Último acceso
    "requires_2fa": "boolean",   // Requiere autenticación de dos factores
    "status": "string"          // Estado de la cuenta (active, locked, etc.)
  },

  // Asignaciones y territorios
  "assignments": [{
    "type": "string",           // Tipo de asignación (territory, route, etc.)
    "code": "string",           // Código de identificación
    "name": "string",           // Nombre descriptivo
    "start_date": "timestamp",  // Fecha de inicio
    "end_date": "timestamp",    // Fecha de término si aplica
    "is_active": "boolean"      // Estado actual de la asignación
  }],

  // Ubicación
  "location": {
    "id": "string",             // ID de la ubicación
    "name": "string"            // Nombre de la ubicación
  },

  // Jerarquía organizacional
  "organization": {
    "department": "string",     // Departamento
    "position": "string",       // Cargo
    "level": "string",         // Nivel jerárquico
    "supervisor": {
      "id": "string",          // ID del supervisor directo
      "name": "string"         // Nombre del supervisor
    },
    "cost_center": "string"    // Centro de costo
  },

  // Metas y KPIs
  "targets": [{
    "period": "string",        // Periodo (monthly, quarterly, yearly)
    "type": "string",         // Tipo de meta
    "value": "number",        // Valor objetivo
    "unit": "string",         // Unidad de medida
    "progress": "number"      // Progreso actual
  }],

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string"
  }],

  // Relaciones Comerciales
  "commerce_relationships": [{
    "commerce_id": "string",      // ID del comercio
    "relationship_type": "string", // Tipo de relación (primary, secondary, temporary)
    "start_date": "timestamp",    // Fecha de inicio de la relación
    "end_date": "timestamp",      // Fecha de término (si aplica)
    "is_active": "boolean",       // Estado actual de la relación
    "assignment_type": "string",  // Tipo de asignación (direct, inherited, temporary)
    "attributes": [{              // Atributos específicos de la relación
      "key": "string",
      "value": "string"
    }]
  }]
}
```

## Campos Detallados

### Identificadores

| Campo       | Tipo   | Requerido | Descripción                             |
| ----------- | ------ | --------- | --------------------------------------- |
| seller_id   | string | Sí        | Identificador único en Reten            |
| external_id | string | No        | Identificador en el sistema del cliente |
| employee_id | string | No        | Número de empleado en CPG               |

### Información Básica

| Campo        | Tipo   | Requerido | Descripción                    |
| ------------ | ------ | --------- | ------------------------------ |
| first_name   | string | Sí        | Nombre del vendedor            |
| last_name    | string | Sí        | Apellido del vendedor          |
| display_name | string | No        | Nombre para mostrar en sistema |
| status       | string | Sí        | Estado actual del vendedor     |

**Estados Válidos del Vendedor:**

- `active`: Vendedor activo
- `inactive`: Vendedor inactivo temporalmente
- `suspended`: Cuenta suspendida
- `terminated`: Relación terminada

### Información de Acceso

| Campo        | Tipo      | Requerido | Descripción                      |
| ------------ | --------- | --------- | -------------------------------- |
| username     | string    | Sí        | Nombre de usuario para acceso    |
| role         | string    | Sí        | Rol principal asignado           |
| permissions  | array     | No        | Permisos específicos adicionales |
| requires_2fa | boolean   | No        | Autenticación de dos factores    |
| last_login   | timestamp | No        | Último acceso al sistema         |

**Roles Comunes:**

- `sales_rep`: Vendedor de campo
- `supervisor`: Supervisor de ventas
- `manager`: Gerente de territorio
- `admin`: Administrador de sistema
- `analyst`: Analista de ventas

### Asignaciones y Territorios

Las asignaciones definen las áreas de responsabilidad del vendedor:

| Campo      | Tipo      | Descripción              |
| ---------- | --------- | ------------------------ |
| type       | string    | Tipo de asignación       |
| code       | string    | Código identificador     |
| name       | string    | Nombre descriptivo       |
| start_date | timestamp | Inicio de la asignación  |
| end_date   | timestamp | Término de la asignación |
| is_active  | boolean   | Estado actual            |

**Tipos de Asignación:**

- `territory`: Territorio geográfico
- `route`: Ruta de visita
- `portfolio`: Portafolio de productos
- `channel`: Canal de ventas
- `segment`: Segmento de clientes

## Relaciones Comerciales

Los vendedores pueden tener múltiples comercios asociados, y esta relación se gestiona a través de la siguiente estructura:

```json
{
  // ... otros campos del vendedor ...
  
  "commerce_relationships": [{
    "commerce_id": "string",      // ID del comercio
    "relationship_type": "string", // Tipo de relación (primary, secondary, temporary)
    "start_date": "timestamp",    // Fecha de inicio de la relación
    "end_date": "timestamp",      // Fecha de término (si aplica)
    "is_active": "boolean",       // Estado actual de la relación
    "assignment_type": "string",  // Tipo de asignación (direct, inherited, temporary)
    "attributes": [{              // Atributos específicos de la relación
      "key": "string",
      "value": "string"
    }]
  }]
}
```

### Campos de Relación Comercial

| Campo             | Tipo      | Requerido | Descripción                               |
| ----------------- | --------- | --------- | ----------------------------------------- |
| commerce_id       | string    | Sí        | Identificador único del comercio          |
| relationship_type | string    | Sí        | Tipo de relación con el comercio          |
| start_date        | timestamp | Sí        | Inicio de la relación                     |
| end_date          | timestamp | No        | Término de la relación                    |
| is_active         | boolean   | Sí        | Indica si la relación está activa         |
| assignment_type   | string    | Sí        | Cómo fue asignado el comercio al vendedor |

### Tipos de Relación

- `primary`: Vendedor principal responsable del comercio
- `secondary`: Vendedor de apoyo o respaldo
- `temporary`: Asignación temporal (ej: cubrir vacaciones)

### Tipos de Asignación

- `direct`: Asignado directamente al vendedor
- `inherited`: Heredado por jerarquía territorial
- `temporary`: Asignación temporal por necesidad específica

### Ejemplo de Relación Comercial

```json
{
  "seller_id": "SELLER_001",
  "commerce_relationships": [{
    "commerce_id": "COMM_123",
    "relationship_type": "primary",
    "start_date": "2024-01-01T00:00:00Z",
    "is_active": true,
    "assignment_type": "direct",
    "attributes": [{
      "key": "visit_frequency",
      "value": "weekly"
    }]
  }, {
    "commerce_id": "COMM_456",
    "relationship_type": "secondary",
    "start_date": "2024-02-01T00:00:00Z",
    "is_active": true,
    "assignment_type": "inherited",
    "attributes": [{
      "key": "backup_reason",
      "value": "territory_overlap"
    }]
  }]
}
```

## Validaciones

### Validaciones Generales

### Identificadores

- `seller_id` debe ser único en todo el sistema
- `external_id` debe ser único por cliente
- `employee_id` debe ser único dentro del CPG

### Información Personal

- Nombres y apellidos no pueden estar vacíos
- El email principal debe ser corporativo
- El formato de teléfono debe ser válido

### Acceso

- Username debe ser único
- Password debe cumplir política de seguridad
- Roles deben ser válidos y activos

### Validaciones de Negocio

### Asignaciones

- No puede haber solapamiento de territorios
- Fechas de inicio deben ser válidas
- Debe haber al menos una asignación activa

### Jerarquía

- El supervisor debe existir y estar activo
- No pueden existir ciclos en la jerarquía
- Niveles deben ser consistentes

### Relaciones Comerciales

- Un comercio solo puede tener un vendedor `primary` activo a la vez
- Las fechas de relación deben ser válidas y no solaparse para el mismo tipo
- Las relaciones `temporary` deben tener fecha de término
- El `assignment_type` debe ser consistente con el tipo de relación
- Las relaciones heredadas deben corresponder con la jerarquía territorial

## Ejemplos de Uso

### Vendedor Básico

```json
{
  "seller_id": "SELLER_001",
  "external_id": "EMP_123",
  "name": {
    "first_name": "Juan",
    "last_name": "Pérez",
    "display_name": "Juan P."
  },
  "status": "active",
  "contacts": [{
    "email": "juan.perez@cpg.com",
    "phone": "+56912345678",
    "type": "work",
    "is_primary": true
  }],
  "access": {
    "username": "jperez",
    "role": "sales_rep",
    "permissions": ["view_reports", "create_orders"],
    "requires_2fa": true
  }
}
```

### Vendedor con Territorios

```json
{
  "seller_id": "SELLER_002",
  "name": {
    "first_name": "María",
    "last_name": "González"
  },
  "assignments": [{
    "type": "territory",
    "code": "ZONE_NORTH",
    "name": "Zona Norte Santiago",
    "start_date": "2024-01-01T00:00:00Z",
    "is_active": true
  }, {
    "type": "portfolio",
    "code": "PORTFOLIO_PREMIUM",
    "name": "Productos Premium",
    "start_date": "2024-01-01T00:00:00Z",
    "is_active": true
  }],
  "organization": {
    "department": "Sales",
    "position": "Senior Sales Representative",
    "level": "L3",
    "reports_to": "SELLER_005"
  }
}
```

### Vendedor con Metas

```json
{
  "seller_id": "SELLER_003",
  "targets": [{
    "period": "2024-Q1",
    "type": "sales_volume",
    "value": 100000,
    "unit": "USD",
    "progress": 75000
  }, {
    "period": "2024-Q1",
    "type": "new_customers",
    "value": 20,
    "unit": "count",
    "progress": 15
  }],
  "attributes": [{
    "key": "specialty",
    "value": "new_business"
  }, {
    "key": "certification_level",
    "value": "advanced"
  }]
}
```

## Notas de Implementación

### Gestión de Acceso

1. **Autenticación**
   - Soporte para múltiples métodos
   - Integración con SSO corporativo
   - Políticas de contraseñas

2. **Autorización**
   - Basada en roles y permisos
   - Herencia de permisos
   - Restricciones por territorio

### Seguridad

1. **Protección de Datos**
   - Encriptación de datos sensibles
   - Auditoría de accesos
   - Protección contra ataques

2. **Cumplimiento**
   - GDPR y normativas locales
   - Retención de datos
   - Privacidad por diseño

## Integración con Otros Sistemas

### APIs

1. **Endpoints Principales**
   ```
   GET    /api/v1/sellers/{seller_id}
   POST   /api/v1/sellers
   PUT    /api/v1/sellers/{seller_id}
   PATCH  /api/v1/sellers/{seller_id}
   DELETE /api/v1/sellers/{seller_id}
   ```

2. **Endpoints de Relación**
   ```
   GET    /api/v1/sellers/{seller_id}/assignments
   GET    /api/v1/sellers/{seller_id}/targets
   GET    /api/v1/sellers/{seller_id}/subordinates
   GET    /api/v1/sellers/{seller_id}/commerces
   POST   /api/v1/sellers/{seller_id}/commerces
   DELETE /api/v1/sellers/{seller_id}/commerces/{commerce_id}
   PATCH  /api/v1/sellers/{seller_id}/commerces/{commerce_id}
   ```

### Webhooks

1. **Eventos Disponibles**
   - `seller.created`
   - `seller.updated`
   - `seller.status_changed`
   - `seller.assignment_changed`
   - `seller.commerce_relationship_created`
   - `seller.commerce_relationship_updated`
   - `seller.commerce_relationship_ended`

2. **Formato de Payload**
   ```