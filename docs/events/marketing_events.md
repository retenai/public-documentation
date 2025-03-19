# Eventos de Marketing

Los eventos de marketing registran todas las interacciones relacionadas con actividades promocionales, comunicaciones y campañas de marketing.

## Eventos Disponibles

### `channel_subscription`

Registra la suscripción o modificación de preferencias en canales de comunicación.

**Cuándo enviarlo:**
- Al suscribirse a un canal de comunicación
- Al modificar preferencias de comunicación
- Al optar por recibir notificaciones específicas

**Estructura específica:**
```json
{
  "event_type": "channel_subscription",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "channel": {
      "type": "string",         // email, sms, whatsapp, push
      "status": "string",       // subscribed, unsubscribed, pending
      "preferences": {
        "frequency": "string",  // daily, weekly, monthly
        "categories": ["string"],
        "time_window": {
          "start_hour": "integer",
          "end_hour": "integer",
          "timezone": "string"
        }
      }
    },
    "consent": {
      "given": "boolean",
      "timestamp": "timestamp",
      "source": "string",      // form, api, import
      "terms_version": "string"
    }
  }
}
```

**Campos Requeridos:**
- `channel`: Información del canal y preferencias
- `consent`: Detalles del consentimiento otorgado

---

### `campaign_interaction`

Registra interacciones con campañas de marketing.

**Cuándo enviarlo:**
- Al abrir un email de campaña
- Al hacer click en un link de campaña
- Al interactuar con una notificación push

**Estructura específica:**
```json
{
  "event_type": "campaign_interaction",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "campaign": {
      "campaign_id": "string",
      "name": "string",
      "type": "string",        // promotional, transactional, lifecycle
      "channel": "string"      // email, sms, push, whatsapp
    },
    "interaction": {
      "type": "string",        // open, click, view, dismiss
      "element": "string",     // button, link, image
      "timestamp": "timestamp",
      "metadata": {
        "url": "string",       // opcional
        "position": "string",  // opcional
        "device": "string"     // opcional
      }
    },
    "context": {
      "session_id": "string",
      "device_type": "string",
      "platform": "string"
    }
  }
}
```

**Campos Requeridos:**
- `campaign`: Información de la campaña
- `interaction`: Detalles de la interacción
- `context`: Contexto de la interacción

---

### `survey_response`

Registra respuestas a encuestas y formularios de feedback.

**Cuándo enviarlo:**
- Al completar una encuesta
- Al enviar feedback
- Al responder un cuestionario

**Estructura específica:**
```json
{
  "event_type": "survey_response",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "survey": {
      "survey_id": "string",
      "type": "string",       // nps, csat, custom
      "context": "string"     // post_purchase, service, product
    },
    "responses": [{
      "question_id": "string",
      "type": "string",       // rating, text, choice
      "value": "string",
      "metadata": {
        "scale": "string",    // opcional
        "category": "string"  // opcional
      }
    }],
    "completion": {
      "duration": "integer",  // segundos
      "platform": "string",
      "submitted": "boolean"
    }
  }
}
```

**Campos Requeridos:**
- `survey`: Información de la encuesta
- `responses`: Lista de respuestas
- `completion`: Detalles de completitud

---

### `promotional_view`

Registra visualizaciones de contenido promocional.

**Cuándo enviarlo:**
- Al mostrar un banner promocional
- Al desplegar una oferta especial
- Al presentar contenido personalizado

**Estructura específica:**
```json
{
  "event_type": "promotional_view",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "promotion": {
      "promotion_id": "string",
      "type": "string",        // banner, popup, sidebar
      "name": "string",
      "position": "string"     // home_top, category_sidebar
    },
    "content": {
      "title": "string",
      "description": "string",
      "creative_id": "string",
      "offer": {
        "type": "string",      // discount, bundle, free_shipping
        "value": "string"
      }
    },
    "exposure": {
      "duration": "integer",   // segundos
      "viewport": "boolean",   // si estuvo visible
      "interaction": "boolean" // si hubo interacción
    }
  }
}
```

**Campos Requeridos:**
- `promotion`: Información de la promoción
- `content`: Contenido mostrado
- `exposure`: Detalles de la exposición

## Flujo de Eventos

Los eventos de marketing pueden seguir varios flujos:

1. Suscripción a Comunicaciones:
   ```
   channel_subscription -> campaign_interaction -> survey_response
   ```

2. Campañas Promocionales:
   ```
   promotional_view -> campaign_interaction -> conversion_event
   ```

## Validaciones Específicas

1. **Consentimiento y Privacidad**
   - Verificar consentimiento activo antes de enviar comunicaciones
   - Respetar preferencias de frecuencia y horarios
   - Mantener registro de opt-ins y opt-outs

2. **Consistencia de Campañas**
   - `campaign_id` debe ser válido y activo
   - Las interacciones deben corresponder al tipo de campaña
   - Los timestamps deben ser consistentes con el ciclo de vida de la campaña

3. **Calidad de Datos**
   - Las respuestas de encuestas deben estar dentro de rangos válidos
   - Los IDs de promociones deben existir
   - Las URLs en interacciones deben ser válidas

## Ejemplos de Implementación

### JavaScript
```javascript
const sendMarketingEvent = async (eventType, marketingData) => {
  await retenClient.events.track({
    event_type: eventType,
    event_date: new Date().toISOString(),
    user_id: getCurrentUserId(),
    event_data: {
      ...marketingData,
      context: {
        session_id: getSessionId(),
        platform: getPlatformInfo(),
        campaign_source: getCampaignSource()
      }
    }
  });
};
```

### Python
```python
def send_marketing_event(event_type, marketing_data):
    reten_client.events.track(
        event_type=event_type,
        event_date=datetime.now().isoformat(),
        user_id=get_current_user_id(),
        event_data={
            **marketing_data,
            "context": {
                "session_id": get_session_id(),
                "platform": get_platform_info(),
                "campaign_source": get_campaign_source()
            }
        }
    )
``` 