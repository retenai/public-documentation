# :material-cart: Transacciones

Las transacciones representan las compras realizadas por los clientes en el sistema. Esta entidad almacena toda la información relevante sobre las operaciones comerciales completadas.

## Estructura de Datos

```json
{
  // Identificadores
  "transaction_id": "string",    // Identificador único interno (not null)
  "order_id": "string",          // Identificador de la orden asociada (not null)
  "client_id": "string",         // Identificador del cliente (not null)

  // Información básica
  "status": "string",            // Estado de la transacción (pending, approved, rejected, cancelled)
  "amount": {
    "value": "number",           // Monto de la transacción (not null)
    "currency": "string"         // Moneda de la transacción (not null)
  },

  // Detalles de pagos
  "payments": [{
    "method": "string",          // Método de pago (credit_card, debit_card, cash, transfer)
    "provider": "string",        // Proveedor del método de pago
    "reference": "string",       // Referencia o número de autorización
    "installments": "number",    // Número de cuotas si aplica
    "amount": "number",          // Monto del pago
    "status": "string",          // Estado del pago (paid, pending, failed)
    "date": "timestamp",         // Fecha del pago
    "card": {
      "type": "string",          // Tipo de tarjeta (credit, debit)
      "brand": "string",         // Marca de la tarjeta
      "last_digits": "string",   // Últimos 4 dígitos
      "holder_name": "string"    // Nombre del titular
    }
  }],

  // Fechas relevantes
  "dates": {
    "order_date": "timestamp",       // Fecha de creación de la orden por el cliente (not null)
    "transaction_date": "timestamp",  // Fecha de la transacción (not null)
    "approval_date": "timestamp",     // Fecha de aprobación
    "rejection_date": "timestamp",    // Fecha de rechazo
    "cancellation_date": "timestamp"  // Fecha de cancelación
  },

  // Origen y atribución
  "origin": {
    "channel": "string",         // Canal de venta (ej: web, app, marketplace, pos)
    "platform": "string",        // Plataforma específica (ej: website, android_app, ios_app, mercadolibre)
    "attributes": [{             // Atributos adicionales del origen
      "key": "string",
      "value": "string",
      "type": "string"    // Tipo de valor (string, number, date, boolean)
    }]
  },
  "attribution": {
    "type": "string",            // Tipo de atribución (seller, agent, self_service)
    "entity_id": "string",       // ID de la entidad a la que se atribuye (seller_id, agent_id, etc)
    "attributes": [{             // Atributos adicionales de la atribución
      "key": "string",
      "value": "string",
      "type": "string"    // Tipo de valor (string, number, date, boolean)
    }]
  },

  // Detalles de la transacción
  "items": [{
    "product_id": "string",      // ID del producto
    "item_id": "string",         // ID único del item
    "display_name": "string",    // Nombre del producto
    "slug": "string",            // Slug del producto
    "category_id": "string",     // ID de la categoría del producto
    "category_name": "string",   // Nombre de la categoría del producto
    "sku": "string",             // SKU del producto (opcional)
    "gtin": "string",            // GTIN del producto (opcional)
    "brand": "string",           // Marca del producto (opcional)
    "quantity": "number",        // Cantidad
    "unit_price": "number",      // Precio unitario
    "total_price": "number",     // Precio total
    "attributes": [{             // Atributos del item
      "key": "string",
      "value": "string",
      "type": "string"    // Tipo de valor (string, number, date, boolean)
    }],
    "discount": {
      "coupon_id": "string",     // ID del cupón aplicado
      "amount": "number",        // Monto del descuento
      "type": "string"           // Tipo de descuento (percentage, fixed)
    }
  }],

  // Precios y totales
  "pricing": {
    "net_amount": "number",      // Monto neto
    "final_amount": "number",    // Monto final
    "discount_amount": "number", // Monto total de descuentos
    "shipping_amount": "number", // Costo de envío
    "tax_amount": "number"      // Monto de impuestos
  },

  // Direcciones
  "shipping_address": {
    "name": "string",           // Nombre del destinatario
    "street": "string",         // Calle
    "number": "string",         // Número
    "dept_number": "string",    // Número de departamento
    "city": "string",           // Ciudad
    "commune": "string",        // Comuna
    "region": "string",         // Región
    "lat": "string",           // Latitud
    "long": "string"           // Longitud
  },
  "billing_address": {
    "name": "string",           // Nombre para facturación
    "street": "string",         // Calle
    "number": "string",         // Número
    "dept_number": "string",    // Número de departamento
    "city": "string",           // Ciudad
    "commune": "string",        // Comuna
    "region": "string",         // Región
    "lat": "string",           // Latitud
    "long": "string"           // Longitud
  },

  // Estado y pago
  "coupon_code": "string",       // Código de cupón aplicado

  // Metadatos
  "notes": "string",             // Notas adicionales
  "tags": ["string"],           // Etiquetas para categorización

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"    // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp"      // Fecha de última actualización en sistema cliente
}
```

## Estados Válidos

### Estado de Transacción

- `completed`: Transacción completada
- `cancelled`: Transacción cancelada
- `pending`: Pendiente de confirmación
- `rejected`: Transacción rechazada

### Estado de Pago

- `paid`: Pago completado
- `pending`: Pago pendiente
- `failed`: Pago fallido
- `refunded`: Pago reembolsado

## Validaciones

### Identificadores

- `transaction_id` debe ser único en todo el sistema
- `client_id` debe corresponder a un cliente existente
- `seller_id` debe corresponder a un vendedor existente

### Origen

- `channel` debe ser uno de los valores permitidos: `web`, `app`, `marketplace`, `pos`, `call_center`, `whatsapp`
- `platform` debe ser consistente con el canal seleccionado

### Atribución

- `type` debe ser uno de: `seller`, `agent`, `self_service`
- `entity_id` debe estar presente y ser válido si `type` no es `self_service`

### Fechas

- `created_at` no puede ser posterior a `updated_at`
- `transaction_date` debe ser una fecha válida

### Items

- Debe existir al menos un item
- Los productos deben existir en el catálogo
- Cantidades y precios deben ser positivos
- El total debe corresponder a la suma de items

### Totales

- Los totales deben ser consistentes con los items
- Los descuentos deben ser válidos según las reglas de negocio
- Los impuestos deben calcularse correctamente

## Ejemplos

### Ejemplo Base (Transacción Completa)

```json
{
  "transaction_id": "TRX_001",
  "order_id": "ORDER_001",
  "client_id": "CLIENT_123",

  "status": "completed",
  "amount": {
    "value": 238.00,
    "currency": "CLP"
  },

  "payments": [{
    "method": "credit_card",
    "provider": "transbank",
    "reference": "1234567890",
    "installments": 1,
    "amount": 238.00,
    "status": "paid",
    "date": "2024-03-19T10:00:00Z",
    "card": {
      "type": "credit",
      "brand": "visa",
      "last_digits": "1234",
      "holder_name": "Juan Pérez"
    }
  }],

  "dates": {
    "order_date": "2024-03-19T09:45:00Z",     // Cliente crea la orden
    "transaction_date": "2024-03-19T10:00:00Z", // Se procesa el pago
    "approval_date": "2024-03-19T10:00:00Z"
  },

  "origin": {
    "channel": "web",
    "platform": "website",
    "attributes": []
  },
  "attribution": {
    "type": "seller",
    "entity_id": "SELLER_456",
    "attributes": [
      {
        "key": "conversion_type",
        "value": "assisted"
      }
    ]
  },

  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_001",
    "display_name": "Producto A",
    "slug": "producto-a",
    "category_id": "CAT_001",
    "category_name": "Electrónicos",
    "sku": "SKU123",
    "brand": "Marca A",
    "quantity": 2,
    "unit_price": 100.00,
    "total_price": 200.00,
    "attributes": [],
    "discount": null
  }],

  "pricing": {
    "net_amount": 200.00,
    "final_amount": 238.00,
    "discount_amount": 0,
    "shipping_amount": 0,
    "tax_amount": 38.00
  },

  "shipping_address": {
    "name": "Juan Pérez",
    "street": "Av. Principal",
    "number": "123",
    "dept_number": null,
    "city": "Santiago",
    "commune": "Las Condes",
    "region": "Metropolitana",
    "lat": "-33.4513",
    "long": "-70.5947"
  },
  "billing_address": {
    "name": "Juan Pérez",
    "street": "Av. Principal",
    "number": "123",
    "dept_number": null,
    "city": "Santiago",
    "commune": "Las Condes",
    "region": "Metropolitana",
    "lat": "-33.4513",
    "long": "-70.5947"
  },

  "coupon_code": null,
  "notes": "",
  "tags": [],
  "attributes": [],

  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplos de Casos Específicos

#### Transacción con Descuento por Cupón

```json
{
  "transaction_id": "TRX_002",
  "order_id": "ORDER_002",
  "client_id": "CLIENT_123",

  "status": "completed",
  "amount": {
    "value": 95.20,
    "currency": "CLP"
  },

  "payments": [{
    "method": "cash",
    "provider": "store",
    "reference": "STORE_001",
    "amount": 95.20,
    "status": "paid",
    "date": "2024-03-19T11:00:00Z"
  }],

  "dates": {
    "order_date": "2024-03-19T10:45:00Z",
    "transaction_date": "2024-03-19T11:00:00Z",
    "approval_date": "2024-03-19T11:00:00Z"
  },

  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_002",
    "display_name": "Producto A",
    "slug": "producto-a",
    "category_id": "CAT_001",
    "category_name": "Electrónicos",
    "sku": "SKU123",
    "brand": "Marca A",
    "quantity": 1,
    "unit_price": 100.00,
    "total_price": 100.00,
    "discount": {
      "coupon_id": "COUPON_001",
      "amount": 20.00,
      "type": "percentage"
    }
  }],

  "pricing": {
    "net_amount": 100.00,
    "final_amount": 95.20,
    "discount_amount": 20.00,
    "shipping_amount": 0,
    "tax_amount": 15.20
  },

  "coupon_code": "SUMMER20",
  "attributes": [],

  "created_at": "2024-03-19T11:00:00Z",
  "updated_at": "2024-03-19T11:00:00Z"
}
```

#### Transacción con Múltiples Pagos

```json
{
  "transaction_id": "TRX_003",
  "order_id": "ORDER_003",
  "client_id": "CLIENT_123",

  "status": "completed",
  "amount": {
    "value": 297.50,
    "currency": "CLP"
  },

  "payments": [
    {
      "method": "credit_card",
      "provider": "transbank",
      "reference": "1234567891",
      "installments": 1,
      "amount": 200.00,
      "status": "paid",
      "date": "2024-03-19T12:00:00Z",
      "card": {
        "type": "credit",
        "brand": "visa",
        "last_digits": "1234",
        "holder_name": "Juan Pérez"
      }
    },
    {
      "method": "debit_card",
      "provider": "transbank",
      "reference": "1234567892",
      "amount": 97.50,
      "status": "paid",
      "date": "2024-03-19T12:00:00Z",
      "card": {
        "type": "debit",
        "brand": "maestro",
        "last_digits": "5678",
        "holder_name": "Juan Pérez"
      }
    }
  ],

  "dates": {
    "order_date": "2024-03-19T11:45:00Z",
    "transaction_date": "2024-03-19T12:00:00Z",
    "approval_date": "2024-03-19T12:00:00Z"
  },

  "items": [
    {
      "product_id": "PROD_789",
      "item_id": "ITEM_003",
      "display_name": "Producto A",
      "slug": "producto-a",
      "category_id": "CAT_001",
      "category_name": "Electrónicos",
      "sku": "SKU123",
      "brand": "Marca A",
      "quantity": 2,
      "unit_price": 100.00,
      "total_price": 200.00
    },
    {
      "product_id": "PROD_790",
      "item_id": "ITEM_004",
      "display_name": "Producto B",
      "slug": "producto-b",
      "category_id": "CAT_002",
      "category_name": "Hogar",
      "sku": "SKU456",
      "brand": "Marca B",
      "quantity": 1,
      "unit_price": 50.00,
      "total_price": 50.00
    }
  ],

  "pricing": {
    "net_amount": 250.00,
    "final_amount": 297.50,
    "discount_amount": 0,
    "shipping_amount": 0,
    "tax_amount": 47.50
  },

  "attributes": [],

  "created_at": "2024-03-19T12:00:00Z",
  "updated_at": "2024-03-19T12:00:00Z"
}
```

#### Transacción Pendiente de Pago

```json
{
  "transaction_id": "TRX_004",
  "order_id": "ORDER_004",
  "client_id": "CLIENT_123",

  "status": "pending",
  "amount": {
    "value": 119.00,
    "currency": "CLP"
  },

  "payments": [{
    "method": "bank_transfer",
    "provider": "banco_chile",
    "reference": "TRANSFER_001",
    "amount": 119.00,
    "status": "pending",
    "date": "2024-03-19T13:00:00Z"
  }],

  "dates": {
    "order_date": "2024-03-19T12:45:00Z",
    "transaction_date": "2024-03-19T13:00:00Z"
  },

  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_005",
    "display_name": "Producto A",
    "slug": "producto-a",
    "category_id": "CAT_001",
    "category_name": "Electrónicos",
    "sku": "SKU123",
    "brand": "Marca A",
    "quantity": 1,
    "unit_price": 100.00,
    "total_price": 100.00
  }],

  "pricing": {
    "net_amount": 100.00,
    "final_amount": 119.00,
    "discount_amount": 0,
    "shipping_amount": 0,
    "tax_amount": 19.00
  },

  "attributes": [],

  "created_at": "2024-03-19T13:00:00Z",
  "updated_at": "2024-03-19T13:00:00Z"
}
```

#### Transacción de Autoservicio Web

```json
{
  "transaction_id": "TRX_005",
  "order_id": "ORDER_005",
  "client_id": "CLIENT_123",

  "status": "completed",
  "amount": {
    "value": 119.00,
    "currency": "CLP"
  },

  "payments": [{
    "method": "credit_card",
    "provider": "transbank",
    "reference": "1234567893",
    "installments": 1,
    "amount": 119.00,
    "status": "paid",
    "date": "2024-03-19T14:00:00Z",
    "card": {
      "type": "credit",
      "brand": "mastercard",
      "last_digits": "9012",
      "holder_name": "Juan Pérez"
    }
  }],

  "dates": {
    "order_date": "2024-03-19T13:45:00Z",
    "transaction_date": "2024-03-19T14:00:00Z",
    "approval_date": "2024-03-19T14:00:00Z"
  },

  "origin": {
    "channel": "web",
    "platform": "website",
    "attributes": []
  },
  "attribution": {
    "type": "self_service",
    "entity_id": null,
    "attributes": []
  },

  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_006",
    "display_name": "Producto A",
    "slug": "producto-a",
    "category_id": "CAT_001",
    "category_name": "Electrónicos",
    "sku": "SKU123",
    "brand": "Marca A",
    "quantity": 1,
    "unit_price": 100.00,
    "total_price": 100.00
  }],

  "pricing": {
    "net_amount": 100.00,
    "final_amount": 119.00,
    "discount_amount": 0,
    "shipping_amount": 0,
    "tax_amount": 19.00
  },

  "attributes": [],

  "created_at": "2024-03-19T14:00:00Z",
  "updated_at": "2024-03-19T14:00:00Z"
}
```

#### Transacción por Call Center

```json
{
  "transaction_id": "TRX_006",
  "order_id": "ORDER_006",
  "client_id": "CLIENT_123",

  "status": "completed",
  "amount": {
    "value": 119.00,
    "currency": "CLP"
  },

  "payments": [{
    "method": "credit_card",
    "provider": "transbank",
    "reference": "1234567894",
    "installments": 1,
    "amount": 119.00,
    "status": "paid",
    "date": "2024-03-19T15:00:00Z",
    "card": {
      "type": "credit",
      "brand": "visa",
      "last_digits": "3456",
      "holder_name": "Juan Pérez"
    }
  }],

  "dates": {
    "order_date": "2024-03-19T14:45:00Z",
    "transaction_date": "2024-03-19T15:00:00Z",
    "approval_date": "2024-03-19T15:00:00Z"
  },

  "origin": {
    "channel": "call_center",
    "platform": "genesys",
    "attributes": [
      {
        "key": "call_id",
        "value": "CALL_123"
      }
    ]
  },
  "attribution": {
    "type": "agent",
    "entity_id": "AGENT_789",
    "attributes": [
      {
        "key": "shift",
        "value": "morning"
      }
    ]
  },

  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_007",
    "display_name": "Producto A",
    "slug": "producto-a",
    "category_id": "CAT_001",
    "category_name": "Electrónicos",
    "sku": "SKU123",
    "brand": "Marca A",
    "quantity": 1,
    "unit_price": 100.00,
    "total_price": 100.00
  }],

  "pricing": {
    "net_amount": 100.00,
    "final_amount": 119.00,
    "discount_amount": 0,
    "shipping_amount": 0,
    "tax_amount": 19.00
  },

  "attributes": [],

  "created_at": "2024-03-19T15:00:00Z",
  "updated_at": "2024-03-19T15:00:00Z"
}
```

## 🔄 Integración

### **Método por Archivo**
Las transacciones se cargan en archivos CSV con las columnas correspondientes:

```csv
transaction_id,order_id,client_id,status,amount_value,amount_currency,created_at
TRX_001,ORDER_001,CLIENT_123,completed,238.00,CLP,2024-03-19T10:00:00Z
TRX_002,ORDER_002,CLIENT_124,pending,150.00,CLP,2024-03-19T11:00:00Z
```

### **Método por Base de Datos**
Las transacciones se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT 
    transaction_id,
    order_id,
    client_id,
    status,
    amount_value,
    amount_currency,
    created_at
FROM transactions 
WHERE updated_at > '2024-01-15T00:00:00Z'
ORDER BY created_at;
```
