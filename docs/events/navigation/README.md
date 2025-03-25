# :material-navigation: Navegación

Esta sección documenta los eventos relacionados con la exploración y descubrimiento de productos en la plataforma.

## Descripción General

Los eventos de navegación y descubrimiento capturan las interacciones de los usuarios mientras exploran la plataforma. Estos eventos son fundamentales para:
- Entender el comportamiento de navegación de los usuarios
- Analizar patrones de búsqueda y exploración
- Identificar productos y categorías de mayor interés
- Optimizar la experiencia de usuario y el diseño de la plataforma
- Mejorar la relevancia de resultados de búsqueda

## Eventos Disponibles

### Visualización de Página

**Tipo de Evento:** `page_view`

**Descripción:**  
Registra cuando un usuario visualiza una página específica de la plataforma. Este evento se genera cada vez que se carga una nueva página o se realiza una navegación mediante SPA (Single Page Application).

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "page_view",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "page_url": "string",
    "page_title": "string",
    "page_type": "string",
    "referrer": "string",
    "session_id": "string"
  },
  "metadata": {
    "browser": "string",
    "device": "string",
    "location": "string",
    "viewport_size": "string"
  }
}
```

**Campos Específicos:**
| Campo      | Tipo   | Requerido | Descripción                                            |
| ---------- | ------ | --------- | ------------------------------------------------------ |
| page_url   | string | Sí        | URL completa de la página visualizada                  |
| page_title | string | Sí        | Título de la página                                    |
| page_type  | string | Sí        | Tipo de página (home, category, product, search, etc.) |
| referrer   | string | No        | URL de la página anterior                              |
| session_id | string | Sí        | Identificador único de la sesión del usuario           |

### Búsqueda de Productos

**Tipo de Evento:** `product_search`

**Descripción:**  
Captura cuando un usuario realiza una búsqueda de productos. Incluye tanto búsquedas por texto como aplicación de filtros.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "product_search",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "search_query": "string",
    "filters": {
      "category": ["string"],
      "price_range": {
        "min": "number",
        "max": "number"
      },
      "attributes": {}
    },
    "results_count": "number",
    "page_number": "number",
    "sort_by": "string"
  },
  "metadata": {
    "search_type": "text|filter|category",
    "session_id": "string"
  }
}
```

**Campos Específicos:**
| Campo         | Tipo   | Requerido | Descripción                                  |
| ------------- | ------ | --------- | -------------------------------------------- |
| search_query  | string | No        | Término de búsqueda ingresado por el usuario |
| filters       | object | No        | Filtros aplicados a la búsqueda              |
| results_count | number | Sí        | Cantidad de resultados encontrados           |
| page_number   | number | Sí        | Número de página de resultados               |
| sort_by       | string | No        | Criterio de ordenamiento aplicado            |

### Visualización de Producto

**Tipo de Evento:** `product_view`

**Descripción:**  
Registra cuando un usuario visualiza el detalle de un producto específico.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "product_view",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "product_id": "string",
    "product_name": "string",
    "category_id": "string",
    "price": "number",
    "currency": "string",
    "view_duration": "number",
    "source_page": "search|category|recommendation|direct"
  },
  "metadata": {
    "session_id": "string",
    "is_returning_view": "boolean"
  }
}
```

**Campos Específicos:**
| Campo         | Tipo   | Requerido | Descripción                               |
| ------------- | ------ | --------- | ----------------------------------------- |
| product_id    | string | Sí        | Identificador único del producto          |
| product_name  | string | Sí        | Nombre del producto                       |
| category_id   | string | Sí        | Identificador de la categoría principal   |
| price         | number | Sí        | Precio actual del producto                |
| currency      | string | Sí        | Moneda del precio                         |
| view_duration | number | No        | Tiempo de visualización en segundos       |
| source_page   | string | Sí        | Página desde donde se accedió al producto |

### Interacción con Banner

**Tipo de Evento:** `banner_interaction`

**Descripción:**  
Captura las interacciones de los usuarios con banners promocionales o informativos.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "banner_interaction",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "banner_id": "string",
    "banner_name": "string",
    "banner_type": "promotion|info|category",
    "interaction_type": "view|click",
    "position": "string",
    "campaign_id": "string"
  },
  "metadata": {
    "session_id": "string",
    "page_type": "string"
  }
}
```

**Campos Específicos:**
| Campo            | Tipo   | Requerido | Descripción                       |
| ---------------- | ------ | --------- | --------------------------------- |
| banner_id        | string | Sí        | Identificador único del banner    |
| banner_name      | string | Sí        | Nombre descriptivo del banner     |
| banner_type      | string | Sí        | Tipo de banner                    |
| interaction_type | string | Sí        | Tipo de interacción con el banner |
| position         | string | Sí        | Posición del banner en la página  |
| campaign_id      | string | No        | ID de la campaña asociada         |

## Consideraciones Técnicas

- Los eventos deben enviarse de forma asíncrona para no afectar el rendimiento de la página
- Se recomienda implementar un sistema de cola local para manejar pérdidas de conexión
- La frecuencia máxima de envío es de 100 eventos por segundo por usuario
- Los campos timestamp deben estar en formato ISO 8601 con zona horaria UTC
- Los eventos de page_view deben manejarse correctamente en aplicaciones SPA
- Se debe respetar la privacidad del usuario según las regulaciones locales

## Casos de Uso Comunes

### Análisis de Embudo de Conversión
Utiliza los eventos `page_view` y `product_view` para:
1. Identificar páginas con mayor tasa de abandono
2. Analizar la efectividad de la navegación
3. Optimizar el flujo de conversión

### Optimización de Búsqueda
Utiliza el evento `product_search` para:
1. Mejorar el algoritmo de búsqueda
2. Identificar términos de búsqueda sin resultados
3. Optimizar los filtros más utilizados

### Efectividad de Campañas
Utiliza el evento `banner_interaction` para:
1. Medir el rendimiento de banners promocionales
2. Optimizar el posicionamiento de banners
3. Evaluar la efectividad de diferentes tipos de contenido

## Preguntas Frecuentes

### ¿Cómo manejar eventos en modo offline?
Se recomienda almacenar los eventos en localStorage y enviarlos cuando se recupere la conexión.

### ¿Cómo evitar duplicados en eventos de page_view en SPAs?
Implementar un debounce y verificar cambios reales en la ruta de la aplicación.

### ¿Qué hacer si se excede el límite de eventos por segundo?
Implementar un sistema de cola que agrupe y envíe los eventos respetando los límites establecidos.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Implementación de Analytics](../integration/analytics.md)
- [Mejores Prácticas de Tracking](../integration/tracking_best_practices.md)
