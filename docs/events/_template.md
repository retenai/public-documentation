# [Nombre de la Categoría] de Eventos

Esta sección documenta los eventos relacionados con [descripción breve de la categoría].

## Descripción General

[Descripción detallada de la categoría, su propósito y cuándo se generan estos eventos]

## Eventos Disponibles

### [Nombre del Evento 1]

**Tipo de Evento:** `event_type_1`

**Descripción:**  
Descripción detallada del evento y cuándo se genera.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "event_type_1",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    // Campos específicos del evento
    "campo1": "tipo_dato",
    "campo2": "tipo_dato"
  },
  "metadata": {
    // Metadatos opcionales
    "browser": "string",
    "device": "string",
    "location": "string"
  }
}
```

**Campos Específicos:**
| Campo  | Tipo   | Requerido | Descripción           |
| ------ | ------ | --------- | --------------------- |
| campo1 | string | Sí        | Descripción del campo |
| campo2 | number | No        | Descripción del campo |

**Ejemplo:**
```json
{
  "event_id": "evt_123456789",
  "event_type": "event_type_1",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "web",
  "data": {
    "campo1": "valor1",
    "campo2": "valor2"
  },
  "metadata": {
    "browser": "Chrome",
    "device": "desktop",
    "location": "CL"
  }
}
```

## Consideraciones Técnicas

- [Consideración técnica 1]
- [Consideración técnica 2]
- [Límites de rate, si aplican]
- [Recomendaciones de implementación]

## Casos de Uso Comunes

### Caso de Uso 1
Descripción del caso de uso y cómo implementarlo.

### Caso de Uso 2
Descripción del caso de uso y cómo implementarlo.

## Preguntas Frecuentes

### ¿Pregunta frecuente 1?
Respuesta a la pregunta frecuente 1.

### ¿Pregunta frecuente 2?
Respuesta a la pregunta frecuente 2.

## Referencias

- [Link a documentación relacionada 1]
- [Link a documentación relacionada 2]
- [Link a ejemplos de implementación] 