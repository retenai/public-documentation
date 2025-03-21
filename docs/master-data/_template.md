# [Nombre de la Entidad]

[Descripción breve de la entidad y su propósito en el sistema Reten. Explicar qué representa y su importancia.]

## Estructura de Datos

```json
{
  // Identificadores
  "[entity]_id": "string",      // Identificador principal (not null)
  "external_id": "string",      // Identificador externo del cliente
  
  // Información básica
  "name": {
    "display_name": "string",   // Nombre para mostrar (not null)
    "short_name": "string",     // Nombre corto (opcional)
    // ... otros campos de nombre según necesidad
  },
  
  // Marcas temporales
  "created_at": "timestamp",    // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",    // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",   // Fecha de creación en Reten (not null)
  "_updated_at": "timestamp",   // Fecha de última actualización en Reten (not null)

  // Estado
  "status": "string",           // Estado actual de la entidad
  "is_active": "boolean",       // Indicador de estado activo

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"           // Tipo de valor (string, number, date, boolean)
  }]
}
```

## Campos Detallados

### Identificadores

| Campo       | Tipo   | Requerido | Descripción                             |
| ----------- | ------ | --------- | --------------------------------------- |
| [entity]_id | string | Sí        | Identificador único en Reten            |
| external_id | string | No        | Identificador en el sistema del cliente |

### Información Básica

[Tabla con los campos básicos específicos de la entidad]

### Estado

[Descripción de los estados posibles y su significado]

**Estados Válidos:**
- `active`: [Descripción]
- `inactive`: [Descripción]
- [Otros estados específicos]

## Validaciones

### Validaciones Generales

#### Identificadores
- `[entity]_id` debe ser único en todo el sistema
- `external_id` debe ser único por cliente

#### Fechas
- `created_at` no puede ser posterior a `updated_at`
- Todas las fechas deben ser válidas y en formato ISO 8601

#### Atributos
- Las claves de atributos deben ser únicas por entidad
- Los valores deben corresponder al tipo especificado

### Validaciones de Negocio

[Validaciones específicas del dominio de la entidad]

## Ejemplos de Uso

### Ejemplo Básico

```json
{
  "[entity]_id": "ENT_001",
  "external_id": "EXT_123",
  "name": {
    "display_name": "Ejemplo Básico",
    "short_name": "Ejemplo"
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

### Ejemplo Completo

```json
{
  "[entity]_id": "ENT_001",
  "external_id": "EXT_123",
  "name": {
    "display_name": "Ejemplo Completo",
    "short_name": "Ejemplo"
  },
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "_created_at": "2024-03-19T10:00:00Z",
  "_updated_at": "2024-03-19T10:00:00Z",
  "status": "active",
  "is_active": true,
  "attributes": [{
    "key": "color",
    "value": "blue",
    "type": "string"
  }, {
    "key": "weight",
    "value": "10.5",
    "type": "number"
  }, {
    "key": "expiry_date",
    "value": "2025-12-31",
    "type": "date"
  }]
}
```

## Notas de Implementación

### Gestión de Cambios

#### Actualización de Datos
- `_updated_at` se actualiza automáticamente
- Mantener historial de cambios importantes
- Manejar actualizaciones concurrentes

#### Sincronización
- Propagación de cambios a sistemas integrados
- Manejo de conflictos
- Mecanismos de retry

### Consideraciones de Seguridad

#### Protección de Datos
- Encriptación de datos sensibles
- Auditoría de accesos
- Protección contra ataques

#### Compliance
- GDPR y normativas locales
- Retención de datos
- Privacidad por diseño

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales
```
GET    /api/v1/[entities]/{[entity]_id}
POST   /api/v1/[entities]
PUT    /api/v1/[entities]/{[entity]_id}
PATCH  /api/v1/[entities]/{[entity]_id}
DELETE /api/v1/[entities]/{[entity]_id}
```

### Webhooks

#### Eventos Disponibles
- `[entity].created`
- `[entity].updated`
- `[entity].deleted`
- `[entity].status_changed`

#### Formato de Payload
```json
{
  "event": "[entity].updated",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "[entity]_id": "string",
    "changes": [{
      "field": "string",
      "old_value": "any",
      "new_value": "any"
    }]
  }
}
```

## Preguntas Frecuentes

**¿Cómo manejar cambios en los atributos personalizados?**
- Los atributos pueden agregarse o modificarse en cualquier momento
- El tipo de un atributo no debe cambiar una vez definido
- Se debe mantener un registro de los cambios en atributos

**¿Cómo gestionar relaciones con otras entidades?**
- Usar IDs para referencias
- Mantener integridad referencial
- Documentar dependencias

**¿Cómo manejar versionamiento?**
- Cambios mayores requieren nueva versión
- Mantener compatibilidad hacia atrás
- Documentar cambios en el esquema 