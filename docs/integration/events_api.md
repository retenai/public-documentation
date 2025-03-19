# API de Eventos

La API de eventos de Reten permite la integración con el sistema de eventos a través de diferentes métodos.

## Métodos de Integración

### 1. Webhooks

Configuración de endpoints para recibir eventos en tiempo real:

```json
POST https://api.example.com/webhook
Content-Type: application/json
X-Reten-Signature: sha256=...

{
  "event_id": "evt_123",
  "event_type": "order.created",
  "timestamp": "2024-03-19T12:00:00Z",
  "data": {
    "order_id": "ord_456",
    "status": "pending"
  }
}
```

### 2. API REST

Endpoints para consulta y gestión de eventos:

```bash
# Listar eventos
GET /api/v1/events

# Obtener evento específico
GET /api/v1/events/{event_id}

# Crear evento
POST /api/v1/events

# Actualizar evento
PUT /api/v1/events/{event_id}
```

### 3. Streaming

Conexión mediante WebSocket para eventos en tiempo real:

```javascript
const ws = new WebSocket('wss://api.example.com/events/stream');
ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Nuevo evento:', data);
};
```

## Autenticación

Todas las peticiones deben incluir un token de autenticación:

```bash
Authorization: Bearer your-api-key
```

## Rate Limits

- Webhooks: 100 eventos/segundo
- API REST: 1000 peticiones/minuto
- Streaming: Sin límite

## Manejo de Errores

```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Has excedido el límite de peticiones",
    "status": 429
  }
}
```

## Ejemplos de Implementación

### Node.js
```javascript
const axios = require('axios');

async function getEvents() {
  try {
    const response = await axios.get('https://api.example.com/events', {
      headers: { 'Authorization': 'Bearer your-api-key' }
    });
    return response.data;
  } catch (error) {
    console.error('Error:', error.response.data);
  }
}
```

### Python
```python
import requests

def get_events():
    try:
        response = requests.get(
            'https://api.example.com/events',
            headers={'Authorization': 'Bearer your-api-key'}
        )
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f'Error: {e}')
```

## Mejores Prácticas

1. **Manejo de Reintentos**
   - Implementar backoff exponencial
   - Definir número máximo de reintentos
   - Almacenar eventos fallidos

2. **Procesamiento de Eventos**
   - Procesar eventos de forma asíncrona
   - Validar firma de webhooks
   - Mantener idempotencia

3. **Monitoreo**
   - Registrar latencia de eventos
   - Monitorear tasa de errores
   - Alertas por umbrales 