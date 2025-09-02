# 📊 Eventos

Los eventos representan acciones o actividades que ocurren en el sistema del cliente y que se cargan a Reten para análisis y procesamiento. Permite cargar eventos históricos o en lote desde el sistema del cliente hacia Reten. Se enfoca en la carga masiva de datos para sincronización y análisis retrospectivo.

## Estructura de Datos

```json
{
  // Identificadores
  "event_id": "string",          // Identificador único del evento (not null)
  "user_id": "string",           // Identificador del usuario que generó el evento (not null)
  "event_date": "datetime",      // Fecha y hora cuando ocurrió el evento (not null)
  
  // Datos del evento
  "event_data": "object",        // Objeto JSON con los datos específicos del evento (not null)
  
  // Atributos personalizados
  "attributes": [{               // Lista de atributos personalizados (opcional)
    "key": "string",             // Clave del atributo
    "value": "string",           // Valor del atributo
    "type": "string"             // Tipo de valor (string, number, date, boolean)
  }],
  
  // Información adicional
  "source": "string",            // Sistema o plataforma de origen (opcional)
  "metadata": "object"           // Información adicional del evento (opcional)
}
```

## Campos Detallados

### Identificadores

| Campo      | Tipo     | Requerido | Descripción                                    |
| ---------- | -------- | --------- | ---------------------------------------------- |
| event_id   | string   | Sí        | Identificador único del evento                 |
| user_id    | string   | Sí        | Identificador del usuario que generó el evento |
| event_date | datetime | Sí        | Fecha y hora cuando ocurrió el evento          |

### Datos del Evento

| Campo      | Tipo   | Requerido | Descripción                                      |
| ---------- | ------ | --------- | ------------------------------------------------ |
| event_data | object | Sí        | Objeto JSON con los datos específicos del evento |

### Atributos Personalizados

| Campo      | Tipo  | Requerido | Descripción                                             |
| ---------- | ----- | --------- | ------------------------------------------------------- |
| attributes | array | No        | Lista de atributos personalizados con key, value y type |

### Información Adicional

| Campo    | Tipo   | Requerido | Descripción                      |
| -------- | ------ | --------- | -------------------------------- |
| source   | string | No        | Sistema o plataforma de origen   |
| metadata | object | No        | Información adicional del evento |

## 🔍 Tipos de Eventos Comunes

### **Eventos de Comercio Electrónico**
- `purchase`: Compra completada
- `cart_added`: Producto agregado al carrito
- `cart_abandoned`: Carrito abandonado
- `product_viewed`: Producto visualizado
- `wishlist_added`: Producto agregado a favoritos

### **Eventos de Usuario**
- `user_login`: Usuario inició sesión
- `user_logout`: Usuario cerró sesión
- `user_registered`: Usuario se registró
- `profile_updated`: Perfil actualizado

### **Eventos de Marketing**
- `email_opened`: Email abierto
- `email_clicked`: Link de email clickeado
- `campaign_viewed`: Campaña visualizada
- `coupon_applied`: Cupón aplicado

### **Eventos de Negocio**
- `order_status_changed`: Estado de orden cambiado
- `inventory_updated`: Inventario actualizado
- `price_changed`: Precio modificado
- `stock_low`: Stock bajo

## Validaciones

### Campos Requeridos
- **`event_id`**: Debe ser único y no nulo
- **`user_id`**: Debe ser un identificador válido de usuario
- **`event_date`**: Debe ser una fecha/hora válida en formato ISO 8601
- **`event_data`**: Debe ser un objeto JSON válido

### Campos Opcionales
- **`source`**: Si se proporciona, debe ser una cadena válida
- **`metadata`**: Si se proporciona, debe ser un objeto JSON válido

### Reglas de Negocio
- El `event_id` debe ser único dentro del conjunto de eventos del cliente
- El `event_date` no puede ser futuro
- El `user_id` debe corresponder a un usuario válido en el sistema
- El `event_data` debe contener al menos un campo
- Los `attributes` deben tener `key` y `value` válidos si se proporcionan

## Ejemplos de Uso

### **Ejemplo 1: Evento de Compra Simple**
```json
{
  "event_id": "purchase_001",
  "user_id": "USER_123",
  "event_date": "2024-01-15T14:30:00Z",
  "event_data": {
    "order_id": "ORD-12345",
    "amount": 150.00,
    "currency": "USD",
    "products": ["PROD-001", "PROD-002"]
  },
  "attributes": [{
    "key": "channel",
    "value": "web",
    "type": "string"
  }, {
    "key": "priority",
    "value": "normal",
    "type": "string"
  }],
  "source": "web"
}
```

### **Ejemplo 2: Evento de Carrito Abandonado**
```json
{
  "event_id": "cart_abandoned_001",
  "user_id": "USER_456",
  "event_date": "2024-01-15T15:45:00Z",
  "event_data": {
    "cart_id": "CART-789",
    "total_items": 3,
    "total_value": 89.99,
    "abandonment_reason": "shipping_cost"
  },
  "attributes": [{
    "key": "priority",
    "value": "high",
    "type": "string"
  }, {
    "key": "campaign_tag",
    "value": "recovery_campaign",
    "type": "string"
  }],
  "source": "mobile"
}
```

### **Ejemplo 3: Evento de Usuario**
```json
{
  "event_id": "user_login_001",
  "user_id": "USER_789",
  "event_date": "2024-01-15T16:20:00Z",
  "event_data": {
    "login_method": "email",
    "ip_address": "192.168.1.100",
    "user_agent": "Mozilla/5.0..."
  },
  "attributes": [{
    "key": "login_method",
    "value": "email",
    "type": "string"
  }, {
    "key": "session_duration",
    "value": "0",
    "type": "number"
  }],
  "source": "web"
}
```

## Integración

### **Método por Archivo**
Los eventos se cargan en archivos CSV con las columnas correspondientes:

```csv
event_id,user_id,event_date,event_data,source
purchase_001,USER_123,2024-01-15T14:30:00Z,"{""order_id"":""ORD-12345"",""amount"":150.00}","web"
cart_abandoned_001,USER_456,2024-01-15T15:45:00Z,"{""cart_id"":""CART-789"",""total_value"":89.99}","mobile"
```

### **Método por Base de Datos**
Los eventos se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT 
    event_id,
    user_id,
    event_date,
    event_data,
    source
FROM events 
WHERE updated_at > '2024-01-15T00:00:00Z'
ORDER BY event_date;
```
