# Transacciones

Las transacciones representan las compras realizadas por los clientes en el sistema. Esta entidad almacena toda la información relevante sobre las operaciones comerciales completadas.

## Estructura de Datos

```json
{
  // Identificadores
  "transaction_id": "string",    // Identificador único de la transacción (not null)
  "client_id": "string",         // ID del cliente que realizó la compra (not null)
  "seller_id": "string",         // ID del vendedor asociado (not null)

  // Información temporal
  "created_at": "timestamp",     // Fecha de creación (not null)
  "updated_at": "timestamp",     // Última actualización
  "_created_at": "timestamp",    // Fecha de creación en Reten
  "_updated_at": "timestamp",    // Última actualización en Reten
  "transaction_date": "timestamp", // Fecha de la transacción (not null)

  // Detalles de la transacción
  "items": [{
    "product_id": "string",      // ID del producto
    "item_id": "string",         // ID único del item
    "name": "string",            // Nombre del producto
    "category": "string",        // Categoría del producto
    "quantity": "number",        // Cantidad
    "unit_price": "number",      // Precio unitario
    "total_price": "number",     // Precio total
    "attributes": [{             // Atributos del item
      "key": "string",
      "value": "string"
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
  "status": "string",            // Estado de la transacción
  "coupon_code": "string",       // Código de cupón aplicado
  "payment_details": {
    "method": "string",          // Método de pago
    "amount": "number",          // Monto del pago
    "status": "string",          // Estado del pago
    "date": "timestamp"         // Fecha del pago
  },

  // Metadatos
  "notes": "string",             // Notas adicionales
  "tags": ["string"],           // Etiquetas para categorización
  "attributes": [{               // Atributos personalizados
    "key": "string",
    "value": "string"
  }]
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
  "client_id": "CLIENT_123",
  "seller_id": "SELLER_456",
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "_created_at": "2024-03-19T10:00:00Z",
  "_updated_at": "2024-03-19T10:00:00Z",
  "transaction_date": "2024-03-19T10:00:00Z",
  "items": [{
    "product_id": "PROD_789",
    "item_id": "ITEM_001",
    "name": "Producto A",
    "category": "Electrónicos",
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
  "status": "completed",
  "coupon_code": null,
  "payment_details": {
    "method": "credit_card",
    "amount": 238.00,
    "status": "paid",
    "date": "2024-03-19T10:00:00Z"
  },
  "notes": "",
  "tags": [],
  "attributes": []
}
```

### Ejemplos de Casos Específicos

#### Transacción con Descuento por Cupón

```json
{
  "transaction_id": "TRX_002",
  "client_id": "CLIENT_123",
  "seller_id": "SELLER_456",
  "transaction_date": "2024-03-19T11:00:00Z",
  "items": [{
    "product_id": "PROD_789",
    "name": "Producto A",
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
    "tax_amount": 15.20
  },
  "status": "completed",
  "coupon_code": "SUMMER20",
  "payment_details": {
    "method": "cash",
    "amount": 95.20,
    "status": "paid"
  }
}
```

#### Transacción con Múltiples Items

```json
{
  "transaction_id": "TRX_003",
  "client_id": "CLIENT_123",
  "seller_id": "SELLER_456",
  "transaction_date": "2024-03-19T12:00:00Z",
  "items": [
    {
      "product_id": "PROD_789",
      "name": "Producto A",
      "quantity": 2,
      "unit_price": 100.00,
      "total_price": 200.00
    },
    {
      "product_id": "PROD_790",
      "name": "Producto B",
      "quantity": 1,
      "unit_price": 50.00,
      "total_price": 50.00
    }
  ],
  "pricing": {
    "net_amount": 250.00,
    "final_amount": 297.50,
    "tax_amount": 47.50
  },
  "status": "completed"
}
```

#### Transacción Pendiente de Pago

```json
{
  "transaction_id": "TRX_004",
  "client_id": "CLIENT_123",
  "seller_id": "SELLER_456",
  "transaction_date": "2024-03-19T13:00:00Z",
  "items": [{
    "product_id": "PROD_789",
    "name": "Producto A",
    "quantity": 1,
    "unit_price": 100.00,
    "total_price": 100.00
  }],
  "pricing": {
    "net_amount": 100.00,
    "final_amount": 119.00,
    "tax_amount": 19.00
  },
  "status": "pending",
  "payment_details": {
    "method": "bank_transfer",
    "amount": 119.00,
    "status": "pending"
  }
}
```

## Notas de Implementación

### Gestión de Estados

1. **Transiciones de Estado**

   - Las transiciones deben seguir un flujo definido
   - Ciertos estados son terminales
   - Se debe mantener historial de cambios

2. **Validaciones de Estado**

   - Validar permisos para cambios de estado
   - Verificar condiciones necesarias
   - Mantener consistencia con otros sistemas

### Cálculos y Totales

1. **Cálculo de Descuentos**

   - Aplicar reglas de descuento en orden
   - Validar límites y restricciones
   - Considerar descuentos especiales

2. **Cálculo de Impuestos**

   - Aplicar tasas según región
   - Considerar exenciones
   - Manejar redondeos

## Integración con Otros Sistemas

### APIs

1. **Endpoints Principales**

   ```
   GET    /api/v1/transactions/{transaction_id}
   POST   /api/v1/transactions
   PUT    /api/v1/transactions/{transaction_id}
   DELETE /api/v1/transactions/{transaction_id}
   ```

2. **Endpoints de Relación**

   ```
   GET    /api/v1/clients/{client_id}/transactions
   GET    /api/v1/sellers/{seller_id}/transactions
   ```

### Webhooks

1. **Eventos Disponibles**

   - `transaction.created`
   - `transaction.updated`
   - `transaction.status_changed`
   - `transaction.payment_status_changed`

2. **Formato de Payload**

   ```json
   {
     "event": "transaction.status_changed",
     "timestamp": "2024-03-19T14:30:00Z",
     "data": {
       "transaction_id": "string",
       "old_status": "string",
       "new_status": "string"
     }
   }
   ```

## Preguntas Frecuentes

1. **¿Cómo manejar devoluciones?**

   - Crear una transacción de tipo devolución
   - Referenciar la transacción original
   - Actualizar inventarios y estados

2. **¿Cómo gestionar pagos parciales?**

   - Registrar cada pago individualmente
   - Mantener estado actualizado
   - Calcular saldo pendiente

3. **¿Cómo asegurar consistencia?**

   - Usar transacciones de base de datos
   - Implementar mecanismos de retry
   - Mantener logs de auditoría 