# :material-[icono]: [Nombre de la Categoría]

Esta sección documenta los eventos relacionados con [descripción breve de la categoría].

## Descripción General

[Descripción detallada de la categoría]. Estos eventos son fundamentales para:

- [Punto clave 1]
- [Punto clave 2]
- [Punto clave 3]
- [Punto clave 4]

## Eventos Disponibles

La siguiente tabla muestra un resumen de todos los eventos disponibles en esta sección:

| Evento                           | Tipo           | Descripción       | Trigger              |
| -------------------------------- | -------------- | ----------------- | -------------------- |
| [Nombre del Evento 1](#anchor-1) | `event_type_1` | Descripción corta | Condición de disparo |
| [Nombre del Evento 2](#anchor-2) | `event_type_2` | Descripción corta | Condición de disparo |

---

### [Nombre del Evento 1]

!!! info "`event_type_1`"
    **Trigger**  
    Se dispara cuando [condición específica]

    **Descripción**  
    [Descripción detallada del evento]

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "event_type_1",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "campo1": "tipo_dato",
    "campo2": "tipo_dato"
  },
  "metadata": {
    "meta1": "tipo_dato",
    "meta2": "tipo_dato"
  }
}
```

**Campos Específicos:**

| Campo    | Tipo   | Requerido | Descripción           |
| -------- | ------ | --------- | --------------------- |
| `campo1` | string | Sí        | Descripción del campo |
| `campo2` | number | No        | Descripción del campo |

**Ejemplo de Uso:**
```json
{
  "event_type": "event_type_1",
  "data": {
    "campo1": "valor1",
    "campo2": 123
  }
}
```

---

### [Nombre del Evento 2]

!!! note "`event_type_2`"
    **Trigger**  
    Se dispara cuando [condición específica]

    **Descripción**  
    [Descripción detallada del evento]

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "event_type_2",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "campo1": "tipo_dato",
    "campo2": "tipo_dato"
  },
  "metadata": {
    "meta1": "tipo_dato",
    "meta2": "tipo_dato"
  }
}
```

**Campos Específicos:**

| Campo    | Tipo   | Requerido | Descripción           |
| -------- | ------ | --------- | --------------------- |
| `campo1` | string | Sí        | Descripción del campo |
| `campo2` | number | No        | Descripción del campo |

**Ejemplo de Uso:**
```json
{
  "event_type": "event_type_2",
  "data": {
    "campo1": "valor1",
    "campo2": 123
  }
}
```

---

## Consideraciones Técnicas

- [Consideración técnica 1]
- [Consideración técnica 2]
- [Consideración técnica 3]
- [Consideración técnica 4]

## Casos de Uso Comunes

### [Caso de Uso 1]

Utiliza los eventos `event_type_1` y `event_type_2` para:

1. [Objetivo 1]
2. [Objetivo 2]
3. [Objetivo 3]

### [Caso de Uso 2]

Utiliza el evento `event_type_1` para:

1. [Objetivo 1]
2. [Objetivo 2]
3. [Objetivo 3]

## Preguntas Frecuentes

### [Pregunta 1]?
[Respuesta detallada a la pregunta 1]

### [Pregunta 2]?
[Respuesta detallada a la pregunta 2]

### [Pregunta 3]?
[Respuesta detallada a la pregunta 3]

## Referencias

- [Link a documentación relacionada 1](../path/to/doc1.md)
- [Link a documentación relacionada 2](../path/to/doc2.md)
- [Link a documentación relacionada 3](../path/to/doc3.md) 