# [Nombre de la Entidad]

## Descripción

[Descripción detallada de la entidad y su propósito]

## Estructura de Datos

```json
{
  // Identificadores
  "entity_id": "string",        // Identificador único interno (not null)

  // Información básica
  "name": {
    "display_name": "string",   // Nombre para mostrar (not null)
    "short_name": "string",     // Nombre corto (opcional)
    // ... otros campos de nombre según necesidad
  },

  // [Otros campos específicos de la entidad]
  // ...

  // Atributos personalizados
  "attributes": [{
    "key": "string",           // Nombre del atributo
    "value": "string",         // Valor del atributo
    "type": "string"          // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp"    // Fecha de última actualización en sistema cliente
}
```

## Reglas de Validación

- `entity_id`: Identificador único, no puede ser nulo
- `name.display_name`: Requerido, máximo 100 caracteres
- `name.slug`: Requerido, solo letras minúsculas, números y guiones
- `name.description`: Opcional, máximo 500 caracteres
- `attributes`: Array opcional de atributos personalizados
- `created_at`: Requerido, formato ISO 8601
- `updated_at`: Opcional, formato ISO 8601

## Ejemplos

### Ejemplo Básico

```json
{
  "entity_id": "ent_01H9RKX7NGT9PX1PJRW1MT2K5W",
  "name": {
    "display_name": "Ejemplo Básico",
    "slug": "ejemplo-basico",
    "description": "Un ejemplo básico de la entidad"
  },
  "attributes": [{
    "key": "color",
    "value": "blue",
    "type": "string"
  }],
  "created_at": "2023-08-01T14:30:00Z",
  "updated_at": "2023-08-01T14:30:00Z"
}
```

### Ejemplo Completo

[Agregar un ejemplo más completo con todos los campos posibles]

## Notas Adicionales

[Cualquier información adicional relevante sobre la entidad]

## Campos Detallados

### Identificadores

| Campo     | Tipo   | Requerido | Descripción                  |
| --------- | ------ | --------- | ---------------------------- |
| entity_id | string | Sí        | Identificador único en Reten |

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
- `entity_id` debe ser único en todo el sistema

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
  "entity_id": "EX_001",
  "name": {
    "display_name": "Ejemplo Básico",
    "slug": "ejemplo-basico",
    "description": "Un ejemplo básico de la entidad"
  },
  "status": "active",
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplo con Atributos

```json
{
  "entity_id": "EX_002",
  "name": {
    "display_name": "Ejemplo con Atributos",
    "slug": "ejemplo-con-atributos",
    "description": "Un ejemplo con atributos personalizados"
  },
  "status": "active",
  "attributes": [{
    "key": "color",
    "value": "blue"
  }],
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

## Notas de Implementación

### Gestión de Estado

#### Actualizaciones

- Los estados se validan antes de actualizar
- Se mantiene historial de cambios
- Se notifican cambios relevantes

### Gestión de Cambios

#### Actualización de Datos
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
GET    /api/v1/[entities]/{entity_id}
POST   /api/v1/[entities]
PUT    /api/v1/[entities]/{entity_id}
PATCH  /api/v1/[entities]/{entity_id}
DELETE /api/v1/[entities]/{entity_id}
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
    "entity_id": "string",
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