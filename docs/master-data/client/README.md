# Clientes

Los clientes son los establecimientos, negocios o personas que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los clientes.

## Estructura de Datos

```json
{
  // Identificadores
  "user_id": "string",           // Identificador principal (not null)
  "external_id": "string",       // Identificador externo del cliente

  // Información básica
  "name": "string",              // Nombre del cliente
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",    // Fecha de creación en Reten
  "_updated_at": "timestamp",    // Fecha de última actualización en Reten
  "sign_up_date": "timestamp",   // Fecha de registro (not null)
  "set_up_date": "timestamp",    // Fecha de configuración inicial

  // Ubicación
  "country": "string",           // País del cliente

  // Información de contacto y personas relacionadas
  "contacts": [{
    "email": "string",           // Correo electrónico
    "phone": "string",           // Número telefónico
    "type": "string",            // primary, secondary, billing, etc.
    "personal_info": {           // Información personal del contacto
      "first_name": "string",    // Nombre
      "last_name": "string",     // Apellido
      "country": "string",       // País de residencia
      "date_of_birth": "timestamp", // Fecha de nacimiento
      "role": "string"           // Rol en la organización (CEO, gerente, dueño, etc.)
    }
  }],

  // Direcciones
  "addresses": [{
    "type": "string",           // shipping, billing, warehouse, etc.
    "name": "string",           // Nombre de la dirección
    "street": "string",         // Calle
    "commune": "string",        // Comuna
    "city": "string",          // Ciudad
    "region": "string",        // Región
    "number": "string",        // Número
    "dept_number": "string",   // Número de departamento/oficina
    "lat": "string",          // Latitud
    "long": "string",         // Longitud
    "is_default": "boolean"   // Indica si es la dirección principal
  }],

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string"
  }],

  // Grupos y clasificaciones
  "groups": [{
    "group_type": "string",
    "attributes": [{
      "key": "string",
      "value": "string"
    }]
  }],

  // IDs adicionales
  "miscellaneous_id": [{
    "platform": "string",      // Plataforma relacionada
    "misc_id": "string"       // ID en esa plataforma
  }]
}
```

## Campos Detallados

### Identificadores

| Campo       | Tipo   | Requerido | Descripción                             |
| ----------- | ------ | --------- | --------------------------------------- |
| user_id     | string | Sí        | Identificador único en Reten            |
| external_id | string | No        | Identificador en el sistema del cliente |

### Información Básica

| Campo   | Tipo   | Requerido | Descripción                 |
| ------- | ------ | --------- | --------------------------- |
| name    | string | Sí        | Nombre del cliente          |
| country | string | No        | País donde opera el cliente |

### Marcas Temporales

| Campo        | Tipo      | Requerido | Descripción                             |
| ------------ | --------- | --------- | --------------------------------------- |
| created_at   | timestamp | Sí        | Fecha de creación en sistema cliente    |
| updated_at   | timestamp | No        | Última actualización en sistema cliente |
| _created_at  | timestamp | Sí        | Fecha de creación en Reten              |
| _updated_at  | timestamp | No        | Última actualización en Reten           |
| sign_up_date | timestamp | Sí        | Fecha de registro inicial               |
| set_up_date  | timestamp | No        | Fecha de configuración completada       |

### Contactos

Los contactos se manejan como un array para permitir múltiples personas relacionadas con el cliente. Para clientes que son personas naturales, la lista contendrá un único contacto.

| Campo         | Tipo   | Descripción                               |
| ------------- | ------ | ----------------------------------------- |
| email         | string | Dirección de correo electrónico           |
| phone         | string | Número telefónico                         |
| type          | string | Tipo de contacto (primary, billing, etc.) |
| personal_info | object | Información personal del contacto         |

**Información Personal del Contacto:**

| Campo         | Tipo      | Descripción            |
| ------------- | --------- | ---------------------- |
| first_name    | string    | Nombre del contacto    |
| last_name     | string    | Apellido del contacto  |
| country       | string    | País de residencia     |
| date_of_birth | timestamp | Fecha de nacimiento    |
| role          | string    | Rol en la organización |

**Tipos de Contacto Válidos:**

- `primary`: Contacto principal (requerido)
- `billing`: Contacto de facturación
- `technical`: Contacto técnico
- `marketing`: Contacto de marketing
- `support`: Contacto de soporte

**Roles Comunes:**

- `owner`: Dueño o propietario
- `ceo`: Director ejecutivo
- `manager`: Gerente
- `administrator`: Administrador
- `employee`: Empleado
- `representative`: Representante legal

### Direcciones

Las direcciones se manejan como un array para permitir múltiples ubicaciones:

| Campo       | Tipo    | Descripción                         |
| ----------- | ------- | ----------------------------------- |
| type        | string  | Tipo de dirección                   |
| name        | string  | Nombre identificativo               |
| street      | string  | Nombre de la calle                  |
| number      | string  | Número exterior                     |
| dept_number | string  | Número interior/departamento        |
| commune     | string  | Comuna o municipio                  |
| city        | string  | Ciudad                              |
| region      | string  | Región o estado                     |
| lat         | string  | Latitud                             |
| long        | string  | Longitud                            |
| is_default  | boolean | Indica si es la dirección principal |

**Tipos de Dirección Válidos:**

- `billing`: Dirección de facturación
- `shipping`: Dirección de envío
- `warehouse`: Almacén o bodega
- `office`: Oficina
- `store`: Tienda física

### Grupos y Clasificaciones

Los grupos permiten categorizar y segmentar clientes:

```json
{
  "groups": [{
    "group_type": "channel",
    "attributes": [{
      "key": "category",
      "value": "retail"
    }, {
      "key": "sub_category",
      "value": "convenience_store"
    }]
  }, {
    "group_type": "segment",
    "attributes": [{
      "key": "size",
      "value": "medium"
    }, {
      "key": "potential",
      "value": "high"
    }]
  }]
}
```

**Tipos de Grupo Comunes:**

- `channel`: Canal de ventas
- `segment`: Segmentación de mercado
- `territory`: División territorial
- `portfolio`: Portafolio de productos

## Validaciones

### Validaciones Generales

### Identificadores

- `user_id` debe ser único en todo el sistema
- `external_id` debe ser único por cliente

### Fechas

- `created_at` no puede ser posterior a `updated_at`
- `sign_up_date` no puede ser posterior a `set_up_date`
- Todas las fechas deben ser válidas y en formato ISO 8601

### Contactos

- Debe existir al menos un contacto de tipo `primary`
- Los emails deben ser únicos dentro del array de contactos
- Los emails deben tener formato válido
- Los teléfonos deben tener formato válido según el país

### Direcciones

- Debe existir al menos una dirección marcada como `is_default: true`
- Las coordenadas geográficas deben ser válidas si se proporcionan
- Los tipos de dirección deben ser valores predefinidos

### Validaciones de Negocio

### Configuración Inicial

- `set_up_date` solo se establece cuando se completan todos los campos requeridos
- La configuración requiere al menos una dirección y un contacto válidos

### Grupos

- Los `group_type` deben corresponder a tipos predefinidos
- Los atributos deben ser válidos para el tipo de grupo

### Atributos

- Las claves de atributos deben ser únicas por cliente
- Los valores deben corresponder al tipo esperado

## Ejemplos de Uso

### Cliente Persona Natural

```json
{
  "user_id": "COM_001",
  "external_id": "CLIENT_123",
  "name": "José Pérez",
  "created_at": "2024-03-19T10:00:00Z",
  "sign_up_date": "2024-03-19T10:00:00Z",
  "country": "CL",
  "contacts": [{
    "type": "primary",
    "email": "jose.perez@email.com",
    "phone": "+56912345678",
    "personal_info": {
      "first_name": "José",
      "last_name": "Pérez",
      "country": "CL",
      "date_of_birth": "1980-05-15T00:00:00Z",
      "role": "owner"
    }
  }],
  "addresses": [{
    "type": "home",
    "name": "Casa",
    "street": "Los Alerces",
    "number": "123",
    "city": "Santiago",
    "region": "Metropolitana",
    "is_default": true
  }]
}
```

### Cliente Empresa con Múltiples Contactos

```json
{
  "user_id": "COM_002",
  "name": "Supermercados El Sol",
  "contacts": [{
    "type": "primary",
    "email": "gerencia@elsol.cl",
    "phone": "+56922334455",
    "personal_info": {
      "first_name": "María",
      "last_name": "González",
      "country": "CL",
      "role": "ceo"
    }
  }, {
    "type": "billing",
    "email": "finanzas@elsol.cl",
    "phone": "+56922334456",
    "personal_info": {
      "first_name": "Juan",
      "last_name": "Silva",
      "country": "CL",
      "role": "administrator"
    }
  }, {
    "type": "technical",
    "email": "sistemas@elsol.cl",
    "phone": "+56922334457",
    "personal_info": {
      "first_name": "Pedro",
      "last_name": "Rojas",
      "country": "CL",
      "role": "manager"
    }
  }],
  // ... resto de la información ...
}
```

### Cliente con Clasificaciones Complejas

```json
{
  "user_id": "COM_003",
  "name": "Distribuidora Mayor",
  "groups": [{
    "group_type": "channel",
    "attributes": [{
      "key": "category",
      "value": "wholesale"
    }]
  }, {
    "group_type": "segment",
    "attributes": [{
      "key": "size",
      "value": "large"
    }, {
      "key": "credit_score",
      "value": "A+"
    }]
  }, {
    "group_type": "territory",
    "attributes": [{
      "key": "zone",
      "value": "north"
    }, {
      "key": "manager",
      "value": "MANAGER_001"
    }]
  }]
}
```

## Notas de Implementación

### Gestión de Cambios

#### Actualización de Datos

- `_updated_at` se actualiza automáticamente
- Los cambios en arrays deben mantener la integridad
- Se debe mantener historial de cambios importantes

#### Sincronización

- Los cambios deben propagarse a sistemas integrados
- Se deben manejar conflictos de actualización
- Implementar mecanismos de retry para fallos

### Consideraciones de Seguridad

#### Datos Sensibles

- Información personal debe estar encriptada
- Acceso controlado a datos financieros
- Logs de auditoría para cambios sensibles

#### Compliance

- Cumplimiento con regulaciones locales
- Manejo de consentimientos y permisos
- Retención de datos según normativa

### Optimización

#### Indexación

- Índices en campos de búsqueda frecuente
- Índices compuestos para queries comunes
- Optimización de búsqueda por geolocalización

#### Caché

- Cacheo de datos frecuentemente accedidos
- Invalidación selectiva de caché
- Estrategias de precarga

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales

```
GET    /api/v1/commerces/{user_id}
POST   /api/v1/commerces
PUT    /api/v1/commerces/{user_id}
PATCH  /api/v1/commerces/{user_id}
DELETE /api/v1/commerces/{user_id}
```

#### Endpoints de Relación

```
GET    /api/v1/commerces/{user_id}/contacts
GET    /api/v1/commerces/{user_id}/addresses
GET    /api/v1/commerces/{user_id}/groups
```

### Webhooks

#### Eventos Disponibles

- `commerce.created`
- `commerce.updated`
- `commerce.deleted`
- `commerce.status_changed`

#### Formato de Payload

```json
{
  "event": "commerce.updated",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "user_id": "string",
    "changes": [{
      "field": "string",
      "old_value": "any",
      "new_value": "any"
    }]
  }
}
```

## Preguntas Frecuentes

**¿Cómo manejar múltiples nombres comerciales?**

   - Usar el campo `name` para el nombre principal
   - Usar atributos para nombres alternativos
   - Mantener histórico de cambios de nombre

**¿Cómo gestionar fusiones de comercios?**

   - Mantener ambos registros activos
   - Usar referencias cruzadas en `miscellaneous_id`
   - Documentar la relación en atributos

**¿Cómo manejar diferentes zonas horarias?**

   - Almacenar todas las fechas en UTC
   - Incluir zona horaria en configuración del comercio
   - Convertir fechas en la capa de presentación 