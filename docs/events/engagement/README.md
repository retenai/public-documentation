# :material-heart: Engagement

Esta sección documenta los eventos relacionados con la interacción y fidelización de usuarios.

## Descripción General

Los eventos de engagement capturan las interacciones de los usuarios que demuestran su nivel de compromiso e interés con la plataforma y sus productos. Estos eventos son fundamentales para:

- Medir el nivel de participación de los usuarios
- Identificar productos y contenido de mayor interés
- Mejorar la experiencia de usuario basada en feedback
- Fortalecer la relación con los clientes
- Optimizar estrategias de retención
- Personalizar recomendaciones y contenido

## Eventos Disponibles

La siguiente tabla muestra un resumen de todos los eventos disponibles en esta sección:

| Evento                                              | Tipo                  | Descripción                         | Trigger                  |
| --------------------------------------------------- | --------------------- | ----------------------------------- | ------------------------ |
| [Lista de Deseos](#lista-de-deseos)                 | `wishlist_updated`    | Actualización de lista de deseos    | Agregar/remover producto |
| [Reseña de Producto](#resena-de-producto)           | `product_review`      | Gestión de reseñas                  | Crear/modificar reseña   |
| [Compartir Producto](#compartir-producto)           | `product_shared`      | Compartir en redes sociales         | Compartir producto       |
| [Interacción con Soporte](#interaccion-con-soporte) | `support_interaction` | Interacción con servicio al cliente | Contacto con soporte     |
| [Respuesta a Encuesta](#respuesta-a-encuesta)       | `survey_response`     | Respuesta a encuestas               | Completar encuesta       |

---

### Lista de Deseos

!!! info "`wishlist_updated`"
    **Trigger**  
    Se dispara cuando un usuario agrega o remueve productos de su lista de deseos

    **Descripción**  
    Registra cuando un usuario agrega o remueve productos de su lista de deseos.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "wishlist_updated",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "action": "add|remove",
    "product_id": "string",
    "product_name": "string",
    "category_id": "string",
    "price": "number",
    "currency": "string",
    "source_page": "product|search|recommendation",
    "wishlist_id": "string",
    "wishlist_name": "string"
  },
  "metadata": {
    "total_items_in_wishlist": "number",
    "notification_preferences": {
      "price_drop": "boolean",
      "back_in_stock": "boolean"
    }
  }
}
```

**Campos Específicos:**

| Campo                      | Tipo   | Requerido | Descripción                      |
| -------------------------- | ------ | --------- | -------------------------------- |
| `action`                   | string | Sí        | Tipo de acción realizada         |
| `product_id`               | string | Sí        | ID del producto afectado         |
| `wishlist_id`              | string | Sí        | ID de la lista de deseos         |
| `wishlist_name`            | string | No        | Nombre personalizado de la lista |
| `notification_preferences` | object | No        | Preferencias de notificación     |

**Ejemplo de Uso:**
```json
{
  "event_type": "wishlist_updated",
  "data": {
    "user_id": "USR_123",
    "action": "add",
    "product_id": "PROD_456",
    "product_name": "Zapatos Deportivos Nike",
    "wishlist_id": "WL_789",
    "notification_preferences": {
      "price_drop": true,
      "back_in_stock": true
    }
  }
}
```

---

### Reseña de Producto

!!! note "`product_review`"
    **Trigger**  
    Se dispara cuando un usuario crea, modifica o elimina una reseña

    **Descripción**  
    Registra cuando un usuario crea, modifica o elimina una reseña de producto.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "product_review",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "product_id": "string",
    "review_id": "string",
    "action": "create|update|delete",
    "rating": "number",
    "review_text": "string",
    "purchase_verified": "boolean",
    "media": [
      {
        "type": "image|video",
        "url": "string"
      }
    ],
    "attributes": {
      "quality": "number",
      "value": "number",
      "durability": "number"
    }
  },
  "metadata": {
    "order_id": "string",
    "days_since_purchase": "number",
    "review_source": "email_prompt|product_page|order_followup"
  }
}
```

**Campos Específicos:**

| Campo               | Tipo    | Requerido | Descripción                      |
| ------------------- | ------- | --------- | -------------------------------- |
| `review_id`         | string  | Sí        | Identificador único de la reseña |
| `rating`            | number  | Sí        | Calificación del producto (1-5)  |
| `review_text`       | string  | No        | Texto de la reseña               |
| `purchase_verified` | boolean | Sí        | Si la compra está verificada     |
| `media`             | array   | No        | Archivos multimedia adjuntos     |

**Ejemplo de Uso:**
```json
{
  "event_type": "product_review",
  "data": {
    "user_id": "USR_123",
    "product_id": "PROD_456",
    "review_id": "REV_789",
    "action": "create",
    "rating": 5,
    "review_text": "Excelente producto, muy satisfecho con la compra",
    "purchase_verified": true
  }
}
```

---

### Compartir Producto

!!! info "`product_shared`"
    **Trigger**  
    Se dispara cuando un usuario comparte un producto en redes sociales o por otros medios

    **Descripción**  
    Registra cuando un usuario comparte un producto en redes sociales o por otros medios.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "product_shared",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "product_id": "string",
    "share_method": "whatsapp|facebook|twitter|email|copy_link",
    "share_destination": "string",
    "custom_message": "string",
    "share_url": "string"
  },
  "metadata": {
    "platform": "web|mobile|app",
    "social_campaign": "string",
    "share_context": "product_page|cart|wishlist"
  }
}
```

**Campos Específicos:**

| Campo               | Tipo   | Requerido | Descripción                      |
| ------------------- | ------ | --------- | -------------------------------- |
| `share_method`      | string | Sí        | Método utilizado para compartir  |
| `share_destination` | string | No        | Destino específico del compartir |
| `custom_message`    | string | No        | Mensaje personalizado agregado   |
| `share_url`         | string | Sí        | URL compartida                   |

**Ejemplo de Uso:**
```json
{
  "event_type": "product_shared",
  "data": {
    "user_id": "USR_123",
    "product_id": "PROD_456",
    "share_method": "whatsapp",
    "share_url": "https://example.com/products/PROD_456",
    "share_context": "product_page"
  }
}
```

---

### Interacción con Soporte

!!! note "`support_interaction`"
    **Trigger**  
    Se dispara cuando un usuario interactúa con el sistema de soporte al cliente

    **Descripción**  
    Registra las interacciones de los usuarios con el sistema de soporte al cliente.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "support_interaction",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "interaction_type": "chat|ticket|call|email",
    "topic": "string",
    "category": "order|product|account|technical|other",
    "priority": "low|medium|high|urgent",
    "status": "opened|in_progress|resolved|closed",
    "satisfaction_rating": "number",
    "resolution_time": "number"
  },
  "metadata": {
    "agent_id": "string",
    "channel": "web|mobile|phone|email",
    "related_order_id": "string"
  }
}
```

**Campos Específicos:**

| Campo                 | Tipo   | Requerido | Descripción                     |
| --------------------- | ------ | --------- | ------------------------------- |
| `interaction_type`    | string | Sí        | Tipo de interacción con soporte |
| `topic`               | string | Sí        | Tema principal de la consulta   |
| `category`            | string | Sí        | Categoría de la interacción     |
| `priority`            | string | Sí        | Nivel de prioridad asignado     |
| `satisfaction_rating` | number | No        | Calificación de satisfacción    |

**Ejemplo de Uso:**
```json
{
  "event_type": "support_interaction",
  "data": {
    "user_id": "USR_123",
    "interaction_type": "chat",
    "topic": "Estado de orden",
    "category": "order",
    "priority": "medium",
    "status": "opened"
  }
}
```

---

### Respuesta a Encuesta

!!! info "`survey_response`"
    **Trigger**  
    Se dispara cuando un usuario completa una encuesta o formulario de feedback

    **Descripción**  
    Registra cuando un usuario completa una encuesta o formulario de feedback.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "survey_response",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "survey_id": "string",
    "survey_type": "nps|csat|product_feedback|general",
    "responses": [
      {
        "question_id": "string",
        "question_type": "rating|multiple_choice|text|scale",
        "response": "string|number|array",
        "additional_comment": "string"
      }
    ],
    "completion_time": "number",
    "survey_version": "string"
  },
  "metadata": {
    "trigger_event": "string",
    "survey_context": "post_purchase|post_support|periodic|targeted",
    "incentive_offered": "boolean"
  }
}
```

**Campos Específicos:**

| Campo             | Tipo   | Requerido | Descripción                       |
| ----------------- | ------ | --------- | --------------------------------- |
| `survey_id`       | string | Sí        | Identificador de la encuesta      |
| `survey_type`     | string | Sí        | Tipo de encuesta                  |
| `responses`       | array  | Sí        | Respuestas proporcionadas         |
| `completion_time` | number | No        | Tiempo de completitud en segundos |

**Ejemplo de Uso:**
```json
{
  "event_type": "survey_response",
  "data": {
    "user_id": "USR_123",
    "survey_id": "SUR_456",
    "survey_type": "nps",
    "responses": [
      {
        "question_id": "Q1",
        "question_type": "rating",
        "response": 9,
        "additional_comment": "Excelente servicio"
      }
    ],
    "completion_time": 120
  }
}
```

---

## Consideraciones Técnicas

- Los eventos deben respetar las preferencias de privacidad del usuario
- Las reseñas deben pasar por un proceso de moderación antes de ser publicadas
- Los contenidos generados por usuarios deben ser validados y sanitizados
- Se debe implementar rate limiting para prevenir spam
- Las encuestas no deben interferir excesivamente con la experiencia del usuario
- Los datos de engagement deben alimentar los sistemas de recomendación

## Casos de Uso Comunes

### Análisis de Satisfacción

Utiliza los eventos `support_interaction` y `survey_response` para:

1. Medir el nivel de satisfacción general
2. Identificar áreas de mejora
3. Optimizar procesos de atención

### Gestión de Contenido

Utiliza el evento `product_review` para:

1. Moderar contenido generado por usuarios
2. Destacar reseñas relevantes
3. Identificar productos con problemas

### Optimización de Engagement

Utiliza los eventos `wishlist_updated` y `product_shared` para:

1. Analizar tendencias de productos
2. Mejorar recomendaciones
3. Optimizar campañas sociales

## Preguntas Frecuentes

### ¿Cómo manejar contenido inapropiado en reseñas?
Implementar un sistema de moderación automática y manual antes de la publicación.

### ¿Qué hacer si un usuario solicita eliminar su feedback?
Proporcionar un proceso claro para la eliminación de contenido manteniendo la integridad de los datos agregados.

### ¿Cómo optimizar la tasa de respuesta en encuestas?
Implementar incentivos y elegir momentos apropiados para solicitar feedback.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Moderación de Contenido](../integration/content_moderation.md)
- [Mejores Prácticas de Engagement](../integration/engagement_best_practices.md)
