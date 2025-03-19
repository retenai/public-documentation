# Eventos de Producto

Los eventos de producto registran todas las interacciones de los usuarios con los productos en la plataforma, incluyendo visualizaciones, impresiones y otras interacciones relevantes.

## Eventos Disponibles

### `product_viewed`

Registra cuando un usuario visualiza un producto en detalle.

**Cuándo enviarlo:**
- Cuando un usuario accede a la página/vista detallada de un producto
- Cuando se muestra un modal/popup con detalles del producto
- Al expandir información detallada de un producto en una lista

**Estructura específica:**
```json
{
  "event_type": "product_viewed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "product": {
      "product_id": "string",
      "name": "string",
      "price": "float64",
      "category": "string",
      "brand": "string",
      "attributes": [{
        "key": "string",
        "value": "string"
      }]
    },
    "source": {
      "page": "string",      // home, category, search, recommendations
      "section": "string",   // featured, related, trending
      "position": "integer"  // posición en la lista/grid si aplica
    },
    "session": {
      "session_id": "string",
      "device_type": "string",
      "referrer": "string"
    }
  }
}
```

**Campos Requeridos:**
- `product`: Información completa del producto visualizado
- `source`: Origen de la visualización
- `session`: Información de la sesión

---

### `product_impressed`

Registra cuando un producto es mostrado al usuario en una lista o grid.

**Cuándo enviarlo:**
- Cuando un producto aparece en el viewport del usuario
- Al cargar una lista de productos
- En resultados de búsqueda o filtrado

**Estructura específica:**
```json
{
  "event_type": "product_impressed",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "products": [{
      "product_id": "string",
      "position": "integer",
      "list_id": "string"
    }],
    "context": {
      "page": "string",
      "section": "string",
      "filter_applied": [{
        "key": "string",
        "value": "string"
      }],
      "sort_by": "string"
    },
    "session": {
      "session_id": "string",
      "device_type": "string"
    }
  }
}
```

**Campos Requeridos:**
- `products`: Lista de productos impresionados
- `context`: Contexto donde se mostraron los productos

---

### `product_clicked`

Registra cuando un usuario hace click en un producto.

**Cuándo enviarlo:**
- Al hacer click en un producto en una lista
- Al interactuar con una card de producto
- Cuando se selecciona un producto en resultados de búsqueda

**Estructura específica:**
```json
{
  "event_type": "product_clicked",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "product": {
      "product_id": "string",
      "name": "string",
      "position": "integer",
      "price": "float64"
    },
    "source": {
      "page": "string",
      "section": "string",
      "list_id": "string"
    },
    "interaction": {
      "element": "string",    // image, title, price, button
      "destination": "string" // product_page, quick_view, add_to_cart
    }
  }
}
```

**Campos Requeridos:**
- `product`: Información básica del producto clickeado
- `source`: Origen del click
- `interaction`: Detalles de la interacción

---

### `product_favorited`

Registra cuando un usuario marca un producto como favorito.

**Cuándo enviarlo:**
- Al agregar un producto a favoritos/wishlist
- Al marcar un producto para ver más tarde
- Al guardar un producto en una lista personalizada

**Estructura específica:**
```json
{
  "event_type": "product_favorited",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "product": {
      "product_id": "string",
      "name": "string",
      "price": "float64"
    },
    "list_details": {
      "list_id": "string",
      "list_name": "string",  // favorites, wishlist, custom_list_name
      "list_type": "string"
    },
    "source": {
      "page": "string",
      "section": "string"
    }
  }
}
```

**Campos Requeridos:**
- `product`: Información del producto favoriteado
- `list_details`: Detalles de la lista donde se guardó

## Flujo de Eventos

El flujo típico de interacción con productos sigue este patrón:

1. `product_impressed` (múltiples productos)
2. `product_clicked` (un producto específico)
3. `product_viewed` (vista detallada)
4. Opcionalmente:
   - `product_favorited`
   - `product_add_to_cart`

## Validaciones Específicas

1. **Consistencia de Productos**
   - `product_id` debe ser válido y existir en el catálogo
   - Los precios deben corresponder al precio actual del producto

2. **Secuencia de Eventos**
   - Un `product_clicked` debería estar precedido por un `product_impressed`
   - Un `product_viewed` debería seguir a un `product_clicked` (con excepciones para acceso directo)

3. **Posiciones y Listas**
   - Las posiciones deben ser números positivos
   - Los `list_id` deben ser consistentes en eventos relacionados

## Ejemplos de Implementación

### JavaScript
```javascript
const sendProductEvent = async (eventType, productData) => {
  await retenClient.events.track({
    event_type: eventType,
    event_date: new Date().toISOString(),
    user_id: getCurrentUserId(),
    event_data: {
      ...productData,
      session: {
        session_id: getSessionId(),
        device_type: getDeviceType()
      }
    }
  });
};
```

### Python
```python
def send_product_event(event_type, product_data):
    reten_client.events.track(
        event_type=event_type,
        event_date=datetime.now().isoformat(),
        user_id=get_current_user_id(),
        event_data={
            **product_data,
            "session": {
                "session_id": get_session_id(),
                "device_type": get_device_type()
            }
        }
    )
``` 