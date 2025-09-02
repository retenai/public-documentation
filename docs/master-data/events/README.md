# 📊 Eventos

Los eventos representan acciones o actividades que ocurren en el sistema del cliente y que se cargan a Reten para análisis y procesamiento.

## 🎯 Propósito

Esta entidad permite cargar eventos históricos o en lote desde el sistema del cliente hacia Reten. Es diferente de la funcionalidad de eventos en tiempo real, ya que se enfoca en la carga masiva de datos para sincronización y análisis retrospectivo.

## 📋 Estructura de Datos

```json
{
  "id": "string",
  "type": "string",
  "timestamp": "datetime",
  "event_data": "object",
  "source": "string",
  "metadata": "object"
}
```

## 🏗️ Campos Detallados

### **Campos Requeridos**

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| `id` | string | Identificador único del evento | `"evt_001"` |
| `type` | string | Tipo o categoría del evento | `"purchase"` |
| `timestamp` | datetime | Fecha y hora cuando ocurrió el evento | `"2024-01-15T14:30:00Z"` |
| `event_data` | object | Objeto JSON con los datos específicos del evento | `{"amount": 150.00, "currency": "USD"}` |

### **Campos Opcionales**

| Campo | Tipo | Descripción | Ejemplo |
|-------|------|-------------|---------|
| `source` | string | Sistema o plataforma de origen | `"web"`, `"mobile"`, `"pos"` |
| `metadata` | object | Información adicional del evento | `{"priority": "high", "tags": ["urgent"]}` |

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

## ✅ Validaciones

### **Campos Requeridos**
- `id`: Debe ser único y no nulo
- `type`: Debe ser una cadena válida
- `timestamp`: Debe ser una fecha/hora válida en formato ISO 8601
- `event_data`: Debe ser un objeto JSON válido

### **Campos Opcionales**
- `source`: Si se proporciona, debe ser una cadena válida
- `metadata`: Si se proporciona, debe ser un objeto JSON válido

### **Reglas de Negocio**
- El `id` debe ser único dentro del conjunto de eventos del cliente
- El `timestamp` no puede ser futuro
- El `event_data` debe contener al menos un campo
- El `type` debe ser descriptivo y consistente

## 📝 Ejemplos de Uso

### **Ejemplo 1: Evento de Compra Simple**
```json
{
  "id": "purchase_001",
  "type": "purchase",
  "timestamp": "2024-01-15T14:30:00Z",
  "event_data": {
    "order_id": "ORD-12345",
    "amount": 150.00,
    "currency": "USD",
    "products": ["PROD-001", "PROD-002"]
  },
  "source": "web"
}
```

### **Ejemplo 2: Evento de Carrito Abandonado**
```json
{
  "id": "cart_abandoned_001",
  "type": "cart_abandoned",
  "timestamp": "2024-01-15T15:45:00Z",
  "event_data": {
    "cart_id": "CART-789",
    "total_items": 3,
    "total_value": 89.99,
    "abandonment_reason": "shipping_cost"
  },
  "source": "mobile",
  "metadata": {
    "priority": "high",
    "tags": ["recovery_campaign"]
  }
}
```

### **Ejemplo 3: Evento de Usuario**
```json
{
  "id": "user_login_001",
  "type": "user_login",
  "timestamp": "2024-01-15T16:20:00Z",
  "event_data": {
    "user_id": "USER-456",
    "login_method": "email",
    "ip_address": "192.168.1.100",
    "user_agent": "Mozilla/5.0..."
  },
  "source": "web"
}
```

## 🔄 Integración

### **Método por Archivo**
Los eventos se cargan en archivos CSV con las columnas correspondientes:

```csv
id,type,timestamp,event_data,source,metadata
purchase_001,purchase,2024-01-15T14:30:00Z,"{""order_id"":""ORD-12345"",""amount"":150.00}","web","{""priority"":""normal""}"
cart_abandoned_001,cart_abandoned,2024-01-15T15:45:00Z,"{""cart_id"":""CART-789"",""total_value"":89.99}","mobile","{""priority"":""high""}"
```

### **Método por Base de Datos**
Los eventos se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT 
    id,
    type,
    timestamp,
    event_data,
    source,
    metadata
FROM events 
WHERE updated_at > '2024-01-15T00:00:00Z'
ORDER BY timestamp;
```

## 💡 Mejores Prácticas

### **Diseño de Eventos**
- **Nombres descriptivos**: Usa tipos de eventos claros y específicos
- **Estructura consistente**: Mantén la misma estructura para eventos similares
- **Datos esenciales**: Incluye solo la información necesaria en `event_data`
- **Metadatos útiles**: Usa `metadata` para información de procesamiento

### **Gestión de Datos**
- **IDs únicos**: Asegúrate de que cada evento tenga un ID único
- **Timestamps precisos**: Usa timestamps con zona horaria cuando sea posible
- **Validación**: Valida la estructura de `event_data` antes de enviar
- **Limpieza**: Elimina campos innecesarios o duplicados

### **Rendimiento**
- **Lotes apropiados**: Envía eventos en lotes de tamaño razonable
- **Compresión**: Considera comprimir archivos CSV grandes
- **Frecuencia**: Establece una frecuencia de sincronización apropiada
- **Monitoreo**: Revisa logs y métricas de carga regularmente

## 🔗 Relaciones

Los eventos pueden relacionarse con otras entidades del sistema:

- **Clientes**: Eventos de usuario vinculados a clientes específicos
- **Productos**: Eventos de visualización o interacción con productos
- **Transacciones**: Eventos de compra vinculados a transacciones
- **Cupones**: Eventos de aplicación o uso de cupones

## 📊 Casos de Uso

### **Análisis de Comportamiento**
- Seguimiento del recorrido del cliente
- Análisis de conversión por tipo de evento
- Identificación de puntos de abandono

### **Optimización de Negocio**
- Mejora de la experiencia del usuario
- Optimización de campañas de marketing
- Ajuste de estrategias de precios

### **Reportes y Métricas**
- Dashboards de actividad en tiempo real
- Análisis de tendencias históricas
- KPIs de rendimiento del negocio
