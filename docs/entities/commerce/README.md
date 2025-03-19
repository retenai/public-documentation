# Comercios

Los comercios son los establecimientos o negocios que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los clientes comerciales.

## Estructura de Datos

```json
{
  // Identificadores
  "user_id": "string",           // Identificador principal (not null)
  "external_id": "string",       // Identificador externo del cliente

  // Información básica
  "name": "string",              // Nombre del comercio
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",    // Fecha de creación en Reten
  "_updated_at": "timestamp",    // Fecha de última actualización en Reten
  "sign_up_date": "timestamp",   // Fecha de registro (not null)
  "set_up_date": "timestamp",    // Fecha de configuración inicial

  // Ubicación
  "country": "string",           // País del comercio

  // Información de contacto
  "contacts": [{
    "email": "string",
    "phone": "string",
    "type": "string"            // primary, secondary, billing, etc.
  }],

  // Información personal
  "personal_info": {
    "first_name": "string",
    "last_name": "string",
    "country": "string",
    "date_of_birth": "timestamp"
  },

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

| Campo   | Tipo   | Requerido | Descripción                          |
| ------- | ------ | --------- | ------------------------------------ |
| name    | string | Sí        | Nombre comercial del establecimiento |
| country | string | No        | País donde opera el comercio         |

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

Los contactos se manejan como un array para permitir múltiples puntos de contacto:

| Campo | Tipo   | Descripción                               |
| ----- | ------ | ----------------------------------------- |
| email | string | Dirección de correo electrónico           |
| phone | string | Número telefónico                         |
| type  | string | Tipo de contacto (primary, billing, etc.) |

**Tipos de Contacto Válidos:**
- `primary`: Contacto principal
- `billing`: Contacto de facturación
- `technical`: Contacto técnico
- `marketing`: Contacto de marketing
- `support`: Contacto de soporte

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

Los grupos permiten categorizar y segmentar comercios:

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

1. **Identificadores**
   - `user_id` debe ser único en todo el sistema
   - `external_id` debe ser único por cliente

2. **Fechas**
   - `created_at` no puede ser posterior a `updated_at`
   - `sign_up_date` no puede ser posterior a `set_up_date`
   - Todas las fechas deben ser válidas y en formato ISO 8601

3. **Contactos**
   - Debe existir al menos un contacto de tipo `primary`
   - Los emails deben ser únicos dentro del array de contactos
   - Los emails deben tener formato válido
   - Los teléfonos deben tener formato válido según el país

4. **Direcciones**
   - Debe existir al menos una dirección marcada como `is_default: true`
   - Las coordenadas geográficas deben ser válidas si se proporcionan
   - Los tipos de dirección deben ser valores predefinidos

### Validaciones de Negocio

1. **Configuración Inicial**
   - `set_up_date` solo se establece cuando se completan todos los campos requeridos
   - La configuración requiere al menos una dirección y un contacto válidos

2. **Grupos**
   - Los `group_type` deben corresponder a tipos predefinidos
   - Los atributos deben ser válidos para el tipo de grupo

3. **Atributos**
   - Las claves de atributos deben ser únicas por comercio
   - Los valores deben corresponder al tipo esperado

## Ejemplos de Uso

### Comercio Básico

```json
{
  "user_id": "COM_001",
  "external_id": "CLIENT_123",
  "name": "Minimarket Don José",
  "created_at": "2024-03-19T10:00:00Z",
  "sign_up_date": "2024-03-19T10:00:00Z",
  "country": "CL",
  "contacts": [{
    "type": "primary",
    "email": "jose@minimarket.cl",
    "phone": "+56912345678"
  }],
  "addresses": [{
    "type": "store",
    "name": "Tienda Principal",
    "street": "Av. Providencia",
    "number": "1234",
    "city": "Santiago",
    "region": "Metropolitana",
    "is_default": true
  }]
}
```

### Comercio con Múltiples Ubicaciones

```json
{
  "user_id": "COM_002",
  "name": "Supermercados El Sol",
  "addresses": [{
    "type": "store",
    "name": "Sucursal Centro",
    "street": "Estado",
    "number": "100",
    "city": "Santiago",
    "is_default": true
  }, {
    "type": "warehouse",
    "name": "Bodega Principal",
    "street": "Los Industriales",
    "number": "500",
    "city": "Santiago",
    "is_default": false
  }, {
    "type": "office",
    "name": "Oficinas Administrativas",
    "street": "Apoquindo",
    "number": "3000",
    "city": "Santiago",
    "is_default": false
  }]
}
```

### Comercio con Clasificaciones Complejas

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

1. **Actualización de Datos**
   - `_updated_at` se actualiza automáticamente
   - Los cambios en arrays deben mantener la integridad
   - Se debe mantener historial de cambios importantes

2. **Sincronización**
   - Los cambios deben propagarse a sistemas integrados
   - Se deben manejar conflictos de actualización
   - Implementar mecanismos de retry para fallos

### Consideraciones de Seguridad

1. **Datos Sensibles**
   - Información personal debe estar encriptada
   - Acceso controlado a datos financieros
   - Logs de auditoría para cambios sensibles

2. **Compliance**
   - Cumplimiento con regulaciones locales
   - Manejo de consentimientos y permisos
   - Retención de datos según normativa

### Optimización

1. **Indexación**
   - Índices en campos de búsqueda frecuente
   - Índices compuestos para queries comunes
   - Optimización de búsqueda por geolocalización

2. **Caché**
   - Cacheo de datos frecuentemente accedidos
   - Invalidación selectiva de caché
   - Estrategias de precarga

## Integración con Otros Sistemas

### APIs

1. **Endpoints Principales**
   ```
   GET    /api/v1/commerces/{user_id}
   POST   /api/v1/commerces
   PUT    /api/v1/commerces/{user_id}
   PATCH  /api/v1/commerces/{user_id}
   DELETE /api/v1/commerces/{user_id}
   ```

2. **Endpoints de Relación**
   ```
   GET    /api/v1/commerces/{user_id}/contacts
   GET    /api/v1/commerces/{user_id}/addresses
   GET    /api/v1/commerces/{user_id}/groups
   ```

### Webhooks

1. **Eventos Disponibles**
   - `commerce.created`
   - `commerce.updated`
   - `commerce.deleted`
   - `commerce.status_changed`

2. **Formato de Payload**
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

1. **¿Cómo manejar múltiples nombres comerciales?**
   - Usar el campo `name` para el nombre principal
   - Usar atributos para nombres alternativos
   - Mantener histórico de cambios de nombre

2. **¿Cómo gestionar fusiones de comercios?**
   - Mantener ambos registros activos
   - Usar referencias cruzadas en `miscellaneous_id`
   - Documentar la relación en atributos

3. **¿Cómo manejar diferentes zonas horarias?**
   - Almacenar todas las fechas en UTC
   - Incluir zona horaria en configuración del comercio
   - Convertir fechas en la capa de presentación 