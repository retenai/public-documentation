# [Nombre de la Configuración]

[Descripción breve de la configuración y su propósito en el sistema]

## Estructura de Datos

```json
{
  // Identificadores
  "config_id": "string",        // Identificador único de la configuración (not null)
  "external_id": "string",      // Identificador externo del cliente (opcional)

  // Información básica
  "name": {
    "display_name": "string",   // Nombre para mostrar (not null)
    "short_name": "string"      // Nombre corto (opcional)
  },
  "description": "string",      // Descripción detallada
  "status": "string",           // Estado actual

  // Configuración específica
  "settings": {
    // Campos específicos de la configuración
  },

  // Validaciones y reglas
  "rules": [{
    "type": "string",          // Tipo de regla
    "condition": "string",     // Condición a evaluar
    "action": "string"         // Acción a tomar
  }],

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp",   // Última actualización (not null)
}
```

## Campos Detallados

### Identificadores

| Campo       | Tipo   | Requerido | Descripción                             |
| ----------- | ------ | --------- | --------------------------------------- |
| config_id   | string | Sí        | Identificador único en Reten            |
| external_id | string | No        | Identificador en el sistema del cliente |

[Continuar con otros grupos de campos...]

## Validaciones

### Validaciones Generales

#### Identificadores
- Lista de validaciones para identificadores

#### [Otros grupos de validación]
- Lista de validaciones específicas

### Validaciones de Negocio

- Lista de reglas de negocio aplicables
- Restricciones específicas del dominio
- Casos especiales a considerar

## Ejemplos

### [Caso de Uso 1]

```json
{
  // Ejemplo de configuración para el caso de uso 1
}
```

### [Caso de Uso 2]

```json
{
  // Ejemplo de configuración para el caso de uso 2
}
```

## Notas de Implementación

### [Aspecto Técnico 1]

- Consideraciones técnicas
- Mejores prácticas
- Casos de borde

### [Aspecto Técnico 2]

- Detalles de implementación
- Optimizaciones recomendadas
- Advertencias importantes

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales
```
GET    /api/v1/[recurso]
POST   /api/v1/[recurso]
PUT    /api/v1/[recurso]/{id}
DELETE /api/v1/[recurso]/{id}
```

### Webhooks

#### Eventos Disponibles
- Lista de eventos que disparan webhooks
- Formato de payload por evento
- Manejo de errores

## Preguntas Frecuentes

**[Pregunta 1]**
- Respuesta detallada
- Ejemplos si aplica
- Referencias a documentación adicional

**[Pregunta 2]**
- Respuesta detallada
- Casos de uso comunes
- Soluciones recomendadas 