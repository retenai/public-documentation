# :material-account-tie: Vendedores

Los vendedores son los usuarios del CPG (Consumer Packaged Goods) que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los usuarios, sus roles, permisos y relaciones con comercios.

## Estructura de Datos

```json
{
  // Identificadores
  "seller_id": "string",         // Identificador único interno (not null)
  "route_id": "string",          // Identificador de ruta o código de vendedor (not null)

  // Información básica
  "name": {
    "first_name": "string",      // Nombre (not null)
    "last_name": "string",       // Apellido (not null)
    "display_name": "string",    // Nombre para mostrar
    "full_name": "string"        // Nombre completo (not null)
  },
  "status": "string",            // Estado del vendedor (active, inactive, suspended, terminated)
  
  // Fechas relevantes
  "dates": {
    "hire_date": "timestamp",      // Fecha de contratación (not null)
    "termination_date": "timestamp" // Fecha de término si aplica
  },

  // Información de contacto
  "contacts": [{
    "email": "string",           // Email corporativo o personal
    "phone": "string",           // Número telefónico
    "type": "string",            // Tipo de contacto (work, personal)
    "is_primary": "boolean"      // Indica si es el contacto principal
  }],

  // Información organizacional
  "organization": {
    "type": "string",           // Tipo de vendedor (salesman, callcenter, supervisor)
    "supervisor_id": "string",   // ID del supervisor directo
    "business_unit": {
      "id": "string",           // ID de la unidad de negocio
      "name": "string",         // Nombre de la unidad de negocio
      "code": "string",         // Código interno de la unidad de negocio
      "type": "string",         // Tipo de unidad (business_unit, division, sub_division)
      "parent_unit": {          // Unidad de negocio padre (si aplica)
        "id": "string",
        "name": "string",
        "code": "string"
      }
    }
  },

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"    // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp"      // Fecha de última actualización en sistema cliente
}
```

## Campos Detallados

### Identificadores

| Campo     | Tipo   | Requerido | Descripción                                 |
| --------- | ------ | --------- | ------------------------------------------- |
| seller_id | string | Sí        | Identificador único interno en Reten        |
| route_id  | string | Sí        | Identificador de ruta o código del vendedor |

### Información Básica

| Campo        | Tipo   | Requerido | Descripción                    |
| ------------ | ------ | --------- | ------------------------------ |
| first_name   | string | Sí        | Nombre del vendedor            |
| last_name    | string | Sí        | Apellido del vendedor          |
| display_name | string | No        | Nombre para mostrar en sistema |
| status       | string | Sí        | Estado actual del vendedor     |

### Fechas

| Campo            | Tipo      | Requerido | Descripción                |
| ---------------- | --------- | --------- | -------------------------- |
| hire_date        | timestamp | Sí        | Fecha de contratación      |
| termination_date | timestamp | No        | Fecha de término si aplica |

**Estados Válidos del Vendedor:**

- `active`: Vendedor activo
- `inactive`: Vendedor inactivo temporalmente
- `suspended`: Cuenta suspendida
- `terminated`: Relación terminada

### Información Organizacional

| Campo              | Tipo   | Requerido | Descripción                      |
| ------------------ | ------ | --------- | -------------------------------- |
| organization.type  | string | Sí        | Tipo de vendedor                 |
| supervisor_id      | string | No        | ID del supervisor directo        |
| business_unit.id   | string | Sí        | ID de la unidad de negocio       |
| business_unit.name | string | Sí        | Nombre de la unidad de negocio   |
| business_unit.code | string | Sí        | Código interno de la unidad      |
| business_unit.type | string | Sí        | Tipo de unidad de negocio        |
| parent_unit.id     | string | No        | ID de la unidad de negocio padre |

**Tipos de Vendedor:**

- `salesman`: Vendedor de campo que realiza visitas presenciales
- `callcenter`: Vendedor que gestiona clientes vía telefónica
- `supervisor`: Responsable de gestionar y supervisar otros vendedores

**Tipos de Unidad de Negocio:**

- `business_unit`: Región o territorio principal
- `division`: Zona dentro de una región
- `sub_division`: Sector específico dentro de una zona

## Validaciones

### Validaciones Generales

#### Identificadores

- `seller_id` debe ser único en todo el sistema
- `route_id` debe ser único dentro del mismo CPG
- El formato de `route_id` debe seguir el patrón definido por el CPG (ej: "R001", "V123", etc.)

#### Información Personal

- Nombres y apellidos no pueden estar vacíos
- El email principal debe ser corporativo
- El formato de teléfono debe ser válido

### Validaciones de Negocio

#### Jerarquía

- El supervisor debe existir y estar activo
- No pueden existir ciclos en la jerarquía
- La unidad de negocio debe existir y estar activa
- La unidad de negocio padre debe existir si se especifica
- El tipo de unidad debe ser válido
- Los supervisores solo pueden tener el tipo "supervisor"
- Un vendedor tipo "supervisor" debe tener al menos un subordinado

## Ejemplos de Uso

### Vendedor Básico

```json
{
  "seller_id": "SELLER_001",
  "route_id": "R001",
  "name": {
    "first_name": "Juan",
    "last_name": "Pérez",
    "display_name": "Juan P."
  },
  "status": "active",
  "dates": {
    "hire_date": "2024-01-01T00:00:00Z",
    "termination_date": null
  },
  "contacts": [{
    "email": "juan.perez@cpg.com",
    "phone": "+56912345678",
    "type": "work",
    "is_primary": true
  }]
}
```

### Vendedor con Jerarquía y Unidad de Negocio

```json
{
  "seller_id": "SELLER_002",
  "route_id": "V123",
  "name": {
    "first_name": "María",
    "last_name": "González"
  },
  "organization": {
    "type": "supervisor",
    "supervisor_id": "SELLER_005",
    "business_unit": {
      "id": "BU_NORTE",
      "name": "Zona Norte",
      "code": "ZN",
      "type": "division",
      "parent_unit": {
        "id": "BU_METRO",
        "name": "Región Metropolitana",
        "code": "RM"
      }
    }
  }
}
```

### Vendedor con Atributos Personalizados

```json
{
  "seller_id": "SELLER_003",
  "attributes": [{
    "key": "specialty",
    "value": "new_business",
    "type": "string"
  }, {
    "key": "certification_level",
    "value": "advanced",
    "type": "string"
  }]
}
```

## Notas de Implementación

### Seguridad

#### Protección de Datos

- Encriptación de datos sensibles
- Auditoría de accesos
- Protección contra ataques

#### Cumplimiento

- GDPR y normativas locales
- Retención de datos
- Privacidad por diseño

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales

```
GET    /api/v1/sellers/{seller_id}
POST   /api/v1/sellers
PUT    /api/v1/sellers/{seller_id}
PATCH  /api/v1/sellers/{seller_id}
DELETE /api/v1/sellers/{seller_id}
```

#### Endpoints de Relación

```
GET    /api/v1/sellers/{seller_id}/subordinates
```

### Webhooks

#### Eventos Disponibles

- `seller.created`
- `seller.updated`
- `seller.status_changed`

#### Formato de Payload

```json
{
  "event": "seller.status_changed",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "seller_id": "string",
    "old_status": "string",
    "new_status": "string",
    "reason": "string",
    "metadata": {
      "updated_by": "string",
      "source": "string"
    }
  }
}
```

## Preguntas Frecuentes

### ¿Cuál es la diferencia entre seller_id y route_id?

- `seller_id` es el identificador único de la persona (vendedor) en el sistema
- `route_id` es el identificador de la ruta de venta asignada al vendedor
- Un vendedor (`seller_id`) puede tener asignada una ruta (`route_id`) específica
- Las búsquedas se pueden realizar por cualquiera de los dos identificadores para encontrar la relación vendedor-ruta

### ¿Cómo gestionar vendedores temporales?

- Usar `status` y fechas de vigencia
- Mantener permisos limitados
- Establecer fecha de término clara

### ¿Cómo funciona la herencia de territorios?

- Los supervisores heredan visibilidad de sus subordinados
- Se mantiene trazabilidad del origen de la herencia

### ¿Cómo manejar conflictos de asignación?

- Las asignaciones de clientes se gestionan a través de la entidad Asignaciones
- Priorizar asignaciones directas sobre heredadas
- Resolver conflictos según jerarquía
- Documentar razones de cambios