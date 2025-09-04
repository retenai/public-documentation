# :material-cart: Transacciones

Las transacciones representan las compras realizadas por los usuarios en el sistema. Esta entidad se divide en dos estructuras relacionadas para optimizar el procesamiento y almacenamiento de datos.

## Entidades Relacionadas

{% from '/includes/cards.md' import section_overview %}

{% set transactions_entities = [
    {
        'icon': 'material-cart',
        'title': 'Transacciones',
        'link': '#Estructura de Datos - Transaction',
        'description': 'Contiene la información general de la transacción'
    },
    {
        'icon': 'material-package-variant',
        'title': 'Items de Transacción',
        'link': 'transactions/README.md',
        'description': 'Contiene los detalles de productos/items asociados a cada transacción'
    }
] %}

{{ section_overview(transactions_entities) }}

<div style="text-align: center;">

```mermaid
erDiagram
    direction LR

    %% Estilos personalizados para las entidades - bordes redondeados y solo relleno
    classDef transactionClass fill:#e1f5fe,stroke:none,color:#000,rx:8,ry:8
    classDef itemsClass fill:#f3e5f5,stroke:none,color:#000,rx:8,ry:8

    %% Entidades simplificadas
    Transacción {
    }

    Item {
    }

    %% Relación con etiqueta descriptiva
    Transacción ||--o{ Item : "contiene (1:N)"

    %% Aplicar estilos
    class Transacción transactionClass
    class Item itemsClass
```

</div>

Una transacción puede tener múltiples items. Indicar los items de una transacción es opcional.

## Estructura de Datos

### :material-cart: Transacción

```json
{
  // Identificadores (requeridos)
  "transaction_id": "string",           // Identificador único interno (not null)
  "order_id": "string",                 // Identificador de la orden asociada (not null)
  "user_id": "string",                  // Identificador del usuario (not null)

  // Información básica
  "transaction_type": "string",         // Tipo: order, invoice, credit_note, debit_note
  "status": "string",                   // Estado: completed, cancelled, pending, rejected
  "currency": "string",                 // Moneda: CLP, USD, EUR, etc

  // Fechas relevantes
  "order_date": "timestamp",            // Fecha de creación de la orden (not null)
  "transaction_date": "timestamp",      // Fecha de la transacción (not null)
  "approval_date": "timestamp",         // Fecha de aprobación (opcional)
  "rejection_date": "timestamp",        // Fecha de rechazo (opcional)
  "cancellation_date": "timestamp",     // Fecha de cancelación (opcional)

  // Origen y atribución (aplanada)
  "origin_channel": "string",           // Canal: web, app, marketplace, pos, call_center, whatsapp
  "origin_platform": "string",          // Plataforma: website, android_app, ios_app, mercadolibre, genesys
  "attribution_type": "string",         // Tipo: seller, agent, self_service
  "attribution_entity_id": "string",    // ID de la entidad atribuida (seller_id, agent_id)

  // Precios y totales (aplanados)
  "pricing_net_amount": "number",       // Monto neto base (not null - requerido)
  "pricing_discount_amount": "number",  // Monto total de descuentos aplicados (opcional)
  "pricing_tax_amount": "number",       // Monto de impuestos calculados (opcional)
  "pricing_shipping_amount": "number",  // Costo de envío (opcional)
  "pricing_final_amount": "number",     // Monto final total (opcional)

  // Direcciones (aplanadas)
  "shipping_name": "string",            // Nombre del destinatario para envío
  "shipping_street": "string",          // Calle de envío
  "shipping_number": "string",          // Número de envío
  "shipping_dept_number": "string",     // Número de departamento de envío
  "shipping_city": "string",            // Ciudad de envío
  "shipping_commune": "string",         // Comuna de envío
  "shipping_region": "string",          // Región de envío
  "shipping_lat": "string",             // Latitud de envío
  "shipping_long": "string",            // Longitud de envío

  "billing_name": "string",             // Nombre para facturación
  "billing_street": "string",           // Calle de facturación
  "billing_number": "string",           // Número de facturación
  "billing_dept_number": "string",      // Número de departamento de facturación
  "billing_city": "string",             // Ciudad de facturación
  "billing_commune": "string",          // Comuna de facturación
  "billing_region": "string",           // Región de facturación
  "billing_lat": "string",              // Latitud de facturación
  "billing_long": "string",             // Longitud de facturación

  // Cupones (aplanados)
  "coupon_id": "string",                // ID del cupón aplicado
  "coupon_code": "string",              // Código del cupón aplicado
  "coupon_discount_value": "number",    // Valor del descuento del cupón
  "coupon_type": "string",              // Tipo de cupón: percentage, fixed

  // Metadatos
  "notes": "string",                    // Notas adicionales
  "tags": "string",                     // Etiquetas separadas por comas (tag1,tag2,tag3)

  // Información de pago
  "payment_method": "string",           // Método: credit_card, debit_card, cash, transfer, bank_transfer
  "payment_provider": "string",         // Proveedor: transbank, mercadopago, paypal, etc
  "payment_reference": "string",        // Referencia o número de autorización
  "payment_installments": "number",     // Número de cuotas (opcional)
  "payment_status": "string",           // Estado del pago: paid, pending, failed, refunded
  "payment_date": "timestamp",          // Fecha del pago
  "payment_card_type": "string",        // Tipo de tarjeta: credit, debit
  "payment_card_brand": "string",       // Marca: visa, mastercard, amex, etc
  "payment_card_last_digits": "string", // Últimos 4 dígitos
  "payment_card_holder_name": "string", // Nombre del titular


  // Atributos personalizados - Cualquier columna no definida en el modelo se almacenará aquí
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // Marcas temporales del sistema origen
  "created_at": "timestamp",            // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",            // Fecha de actualización en sistema cliente

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",           // Fecha de creación del registro en Método de Conexión con Reten
  "_updated_at": "timestamp"            // Fecha de última actualización del registro en Método de Conexión con Reten
}
```

### :material-package-variant: Item

```json
{
  // Identificadores
  "transaction_id": "string",       // ID de la transacción padre (not null)
  "product_id": "string",           // ID del producto (not null)
  "item_id": "string",              // ID único del item dentro de la transacción (not null)

  // Información del producto
  "display_name": "string",         // Nombre del producto para mostrar
  "slug": "string",                 // Slug/URL del producto
  "category_id": "string",          // ID de la categoría del producto
  "category_name": "string",        // Nombre de la categoría del producto
  "sku": "string",                  // SKU del producto
  "gtin": "string",                 // GTIN/EAN del producto
  "brand": "string",                // Marca del producto

  // Cantidad y precios
  "quantity": "number",             // Cantidad del producto (not null)
  "pricing_net_amount": "number",   // Monto neto base del item (not null - requerido)
  "pricing_discount_amount": "number", // Monto de descuento del item (opcional)
  "pricing_tax_amount": "number",   // Monto de impuestos del item (opcional)
  "pricing_shipping_amount": "number", // Costo de envío del item (opcional)
  "pricing_final_amount": "number", // Monto final del item (opcional)

  // Cupones aplicados al item
  "coupon_id": "string",            // ID del cupón aplicado al item
  "coupon_code": "string",          // Código del cupón aplicado al item
  "coupon_discount_value": "number", // Valor del descuento del cupón
  "coupon_type": "string",          // Tipo de cupón: percentage, fixed

  // Atributos personalizados - Cualquier columna no definida en el modelo se almacenará aquí
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // Marcas temporales del sistema origen
  "created_at": "timestamp",            // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",            // Fecha de actualización en sistema cliente

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",           // Fecha de creación del registro en Método de Conexión con Reten
  "_updated_at": "timestamp"            // Fecha de última actualización del registro en Método de Conexión con Reten
}
```

## Campos de Pricing

### :material-cart: Transacción

Los campos de pricing siguen un orden lógico que refleja cómo se calcula el precio final de una transacción:

| Campo                     | Requerido | Descripción                                               | Ejemplo  |
| ------------------------- | --------- | --------------------------------------------------------- | -------- |
| `pricing_net_amount`      | ✅ Sí      | Monto base neto antes de descuentos, impuestos o envío    | `100.00` |
| `pricing_discount_amount` | ❌ No      | Total de descuentos aplicados al monto neto               | `20.00`  |
| `pricing_tax_amount`      | ❌ No      | Impuestos calculados sobre el monto neto menos descuentos | `16.00`  |
| `pricing_shipping_amount` | ❌ No      | Costo de envío de la transacción completa                 | `10.00`  |
| `pricing_final_amount`    | ❌ No      | Monto total final que paga el cliente                     | `106.00` |

#### Flujo de Cálculo Lógico:
1. **Monto Neto Base** (`pricing_net_amount`): Es el precio base de los productos/servicios
2. **Aplicar Descuentos** (`pricing_discount_amount`): Se restan del monto neto
3. **Calcular Impuestos** (`pricing_tax_amount`): Se calculan sobre (neto - descuentos)
4. **Agregar Envío** (`pricing_shipping_amount`): Costo adicional de entrega
5. **Monto Final** (`pricing_final_amount`): Suma total que incluye todos los componentes

#### Ejemplo de Cálculo:
```
Monto Neto: $100.00
- Descuento: $20.00
= Subtotal: $80.00
+ Impuestos (20%): $16.00
+ Envío: $10.00
= Total Final: $106.00
```

### :material-package-variant: Items de Transacción

Los campos de pricing para items individuales siguen la misma lógica pero aplicados a cada producto:

| Campo                     | Requerido | Descripción                                                | Ejemplo |
| ------------------------- | --------- | ---------------------------------------------------------- | ------- |
| `pricing_net_amount`      | ✅ Sí      | Precio base neto del producto individual                   | `50.00` |
| `pricing_discount_amount` | ❌ No      | Descuento aplicado específicamente a este item             | `10.00` |
| `pricing_tax_amount`      | ❌ No      | Impuestos del producto (calculados sobre neto - descuento) | `8.00`  |
| `pricing_shipping_amount` | ❌ No      | Costo de envío proporcional para este item                 | `2.00`  |
| `pricing_final_amount`    | ❌ No      | Precio final del item individual                           | `50.00` |

#### Consideraciones para Items:
- Los montos de items **NO se suman automáticamente** para obtener los totales de transacción
- El total de transacción se calcula por separado y puede incluir descuentos globales
- Los impuestos de items pueden calcularse de forma individual o agregada
- El envío puede distribuirse entre items o calcularse a nivel de transacción

### Reglas Generales de Pricing

#### Consistencia de Datos:
- Si se informa `pricing_final_amount`, debe ser consistente con los demás campos
- Los descuentos siempre deben ser valores positivos
- Los montos pueden ser cero pero nunca negativos (excepto descuentos)

#### Campos Opcionales:
- **Enviar todos los campos que conozcas**: Mayor información = mejor análisis
- **Mínimo requerido**: Solo `pricing_net_amount`
- **Campos complementarios**: Ayudan a entender mejor la estructura de precios

#### Manejo de Casos Especiales:
- **Productos gratis**: `pricing_net_amount: 0`
- **Descuentos 100%**: `pricing_discount_amount = pricing_net_amount`
- **Sin impuestos**: `pricing_tax_amount: 0` o no enviar
- **Envío gratuito**: `pricing_shipping_amount: 0` o no enviar

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

### Tipo de Transacción

- `order`: Orden de compra
- `invoice`: Factura
- `credit_note`: Nota de crédito
- `debit_note`: Nota de débito

### Método de Pago

- `credit_card`: Tarjeta de crédito
- `debit_card`: Tarjeta de débito
- `cash`: Efectivo
- `transfer`: Transferencia bancaria
- `bank_transfer`: Transferencia bancaria
- `check`: Cheque

## Validaciones

### Identificadores

- `transaction_id` debe ser único en todo el sistema
- `user_id` debe corresponder a un usuario existente
- `attribution_entity_id` debe existir si `attribution_type` no es `self_service`

### Origen

- `origin_channel` debe ser uno de: `web`, `app`, `marketplace`, `pos`, `call_center`, `whatsapp`
- `origin_platform` debe ser consistente con el canal seleccionado

### Atribución

- `attribution_type` debe ser uno de: `seller`, `agent`, `self_service`
- `attribution_entity_id` debe estar presente si `attribution_type` no es `self_service`

### Fechas

- `created_at` no puede ser posterior a `updated_at`
- `order_date` debe ser anterior o igual a `transaction_date`
- `_created_at` y `_updated_at` son timestamps de sincronización con Reten

### Items (Transaction Items)

- Los items son opcionales (cliente puede no informar items)
- Si se informan items, `product_id` debe existir en el catálogo
- `quantity` debe ser positivo
- `pricing_net_amount` es requerido para cada item

### Precios

- `pricing_net_amount` es el campo obligatorio básico
- Si se especifican otros campos de precio, deben ser consistentes
- Los descuentos deben ser positivos cuando se apliquen

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el cliente**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan en la base de datos o archivo CSV y que no estén definidas en el modelo estándar de transacciones.

### **Cómo Funciona:**
1. **Cliente envía** datos con columnas adicionales (ej: `campaign_id`, `priority_level`, `custom_field`)
2. **Reten detecta** automáticamente las columnas no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas columnas adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos del cliente**: Información particular de cada negocio
- **Metadatos de integración**: Datos del sistema origen que no tienen equivalente en Reten
- **Atributos de negocio**: Campos específicos de la industria o empresa
- **Configuraciones personalizadas**: Parámetros únicos del cliente

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "campaign_id",
    "value": "SUMMER2024",
    "type": "string"
  },
  {
    "key": "priority_level",
    "value": "5",
    "type": "number"
  },
  {
    "key": "custom_field",
    "value": "valor_personalizado",
    "type": "string"
  }
]
```

### **Tipos de Datos Soportados:**
- `string`: Texto libre
- `number`: Números enteros o decimales
- `date`: Fechas en formato ISO 8601
- `boolean`: Valores true/false

### **Ventajas:**
- **Flexibilidad total** para adaptarse a cualquier modelo de datos
- **Extensibilidad** sin modificar el esquema principal
- **Compatibilidad** con sistemas legacy o personalizados
- **Escalabilidad** para futuras necesidades del negocio
- **Procesamiento automático** sin intervención del cliente

## Ejemplos

### Ejemplo Base (Transacción Completa) - Transaction

```json
{
  "transaction_id": "TRX_001",
  "order_id": "ORDER_001",
  "user_id": "CLIENT_123",

  "transaction_type": "order",
  "status": "completed",
  "currency": "CLP",

  "payment_method": "credit_card",
  "payment_provider": "transbank",
  "payment_reference": "1234567890",
  "payment_installments": 1,
  "payment_status": "paid",
  "payment_date": "2024-03-19T10:00:00Z",
  "payment_card_type": "credit",
  "payment_card_brand": "visa",
  "payment_card_last_digits": "1234",
  "payment_card_holder_name": "Juan Pérez",

  "order_date": "2024-03-19T09:45:00Z",
  "transaction_date": "2024-03-19T10:00:00Z",
  "approval_date": "2024-03-19T10:00:00Z",

  "origin_channel": "web",
  "origin_platform": "website",
  "attribution_type": "seller",
  "attribution_entity_id": "SELLER_456",

  "pricing_net_amount": 200.00,
  "pricing_discount_amount": 0,
  "pricing_tax_amount": 38.00,
  "pricing_shipping_amount": 0,
  "pricing_final_amount": 238.00,

  "shipping_name": "Juan Pérez",
  "shipping_street": "Av. Principal",
  "shipping_number": "123",
  "shipping_city": "Santiago",
  "shipping_commune": "Las Condes",
  "shipping_region": "Metropolitana",
  "shipping_lat": "-33.4513",
  "shipping_long": "-70.5947",

  "billing_name": "Juan Pérez",
  "billing_street": "Av. Principal",
  "billing_number": "123",
  "billing_city": "Santiago",
  "billing_commune": "Las Condes",
  "billing_region": "Metropolitana",
  "billing_lat": "-33.4513",
  "billing_long": "-70.5947",

  "notes": "",
  "tags": "",
  "attributes": [
    {
      "key": "custom_attribute",
      "value": "value1",
      "type": "string"
    }
  ],

  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "_created_at": "2024-03-19T10:00:00Z",
  "_updated_at": "2024-03-19T10:00:00Z"
}
```

### Ejemplo - Transaction Items

```json
{
  "transaction_id": "TRX_001",
  "product_id": "PROD_789",
  "item_id": "ITEM_001",

  "display_name": "Producto A",
  "slug": "producto-a",
  "category_id": "CAT_001",
  "category_name": "Electrónicos",
  "sku": "SKU123",
  "brand": "Marca A",

  "quantity": 2,
  "pricing_net_amount": 200.00,
  "pricing_discount_amount": 0,
  "pricing_tax_amount": 0,
  "pricing_shipping_amount": 0,
  "pricing_final_amount": 200.00,

  "attributes": [
    {
      "key": "custom_item_attribute",
      "value": "value1",
      "type": "string"
    }
  ]
}
```

### Ejemplos de Casos Específicos

#### Transacción con Descuento por Cupón - Transaction

```json
{
  "transaction_id": "TRX_002",
  "order_id": "ORDER_002",
  "user_id": "CLIENT_123",

  "transaction_type": "order",
  "status": "completed",
  "currency": "CLP",

  "payment_method": "cash",
  "payment_provider": "store",
  "payment_reference": "STORE_001",
  "payment_status": "paid",
  "payment_date": "2024-03-19T11:00:00Z",

  "order_date": "2024-03-19T10:45:00Z",
  "transaction_date": "2024-03-19T11:00:00Z",
  "approval_date": "2024-03-19T11:00:00Z",

  "origin_channel": "pos",
  "origin_platform": "store",
  "attribution_type": "seller",
  "attribution_entity_id": "SELLER_789",

  "pricing_net_amount": 100.00,
  "pricing_discount_amount": 20.00,
  "pricing_tax_amount": 15.20,
  "pricing_shipping_amount": 0,
  "pricing_final_amount": 95.20,

  "coupon_id": "COUPON_001",
  "coupon_code": "SUMMER20",
  "coupon_discount_value": 20.00,
  "coupon_type": "percentage",

  "attributes": [
    {
      "key": "custom_transaction_attribute",
      "value": "value1",
      "type": "string"
    }
  ],

  "created_at": "2024-03-19T11:00:00Z",
  "updated_at": "2024-03-19T11:00:00Z",
  "_created_at": "2024-03-19T11:00:00Z",
  "_updated_at": "2024-03-19T11:00:00Z"
}
```

#### Transacción con Descuento por Cupón - Transaction Items

```json
{
  "transaction_id": "TRX_002",
  "product_id": "PROD_789",
  "item_id": "ITEM_002",

  "display_name": "Producto A",
  "slug": "producto-a",
  "category_id": "CAT_001",
  "category_name": "Electrónicos",
  "sku": "SKU123",
  "brand": "Marca A",

  "quantity": 1,
  "pricing_net_amount": 100.00,
  "pricing_discount_amount": 20.00,
  "pricing_tax_amount": 12.16,
  "pricing_shipping_amount": 0,
  "pricing_final_amount": 80.00,

  "coupon_id": "COUPON_001",
  "coupon_code": "SUMMER20",
  "coupon_discount_value": 20.00,
  "coupon_type": "percentage",

  "attributes": [
    {
      "key": "custom_item_attribute",
      "value": "value1",
      "type": "string"
    }
  ]
}
```



## 🔄 Integración

### **Método por Archivo**

#### Archivo de Transacciones
Las transacciones se cargan en archivos CSV con todas las columnas:

```csv
transaction_id,order_id,user_id,transaction_type,status,currency,payment_method,payment_provider,payment_reference,payment_installments,payment_status,payment_date,payment_card_type,payment_card_brand,payment_card_last_digits,payment_card_holder_name,order_date,transaction_date,approval_date,rejection_date,cancellation_date,origin_channel,origin_platform,attribution_type,attribution_entity_id,pricing_net_amount,pricing_discount_amount,pricing_tax_amount,pricing_shipping_amount,pricing_final_amount,shipping_name,shipping_street,shipping_number,shipping_dept_number,shipping_city,shipping_commune,shipping_region,shipping_lat,shipping_long,billing_name,billing_street,billing_number,billing_dept_number,billing_city,billing_commune,billing_region,billing_lat,billing_long,coupon_id,coupon_code,coupon_discount_value,coupon_type,notes,tags,attributes,created_at,updated_at,_created_at,_updated_at
TRX_001,ORDER_001,CLIENT_123,order,completed,CLP,credit_card,transbank,1234567890,1,paid,2024-03-19T10:00:00Z,credit,visa,1234,Juan Pérez,2024-03-19T09:45:00Z,2024-03-19T10:00:00Z,2024-03-19T10:00:00Z,,,,web,website,seller,SELLER_456,200.00,0,38.00,0,238.00,Juan Pérez,Av. Principal,123,,Santiago,Las Condes,Metropolitana,-33.4513,-70.5947,Juan Pérez,Av. Principal,123,,Santiago,Las Condes,Metropolitana,-33.4513,-70.5947,,,,,,,,,2024-03-19T10:00:00Z,2024-03-19T10:00:00Z,2024-03-19T10:00:00Z,2024-03-19T10:00:00Z
TRX_002,ORDER_002,CLIENT_124,order,pending,CLP,bank_transfer,banco_chile,TRANSFER_001,,pending,2024-03-19T11:00:00Z,,,,,2024-03-19T10:45:00Z,2024-03-19T11:00:00Z,,,,,,,,100.00,20.00,15.20,0,95.20,,,,,,,,,,,,COUPON_001,SUMMER20,20.00,percentage,,,2024-03-19T11:00:00Z,2024-03-19T11:00:00Z,2024-03-19T11:00:00Z,2024-03-19T11:00:00Z
```

#### Archivo de Transaction Items
Los items se cargan en un archivo separado:

```csv
transaction_id,product_id,item_id,display_name,slug,category_id,category_name,sku,gtin,brand,quantity,pricing_net_amount,pricing_discount_amount,pricing_tax_amount,pricing_shipping_amount,pricing_final_amount,coupon_id,coupon_code,coupon_discount_value,coupon_type,attributes
TRX_001,PROD_789,ITEM_001,Producto A,producto-a,CAT_001,Electrónicos,SKU123,,Marca A,2,200.00,0,0,0,200.00,,,,
TRX_002,PROD_790,ITEM_002,Producto B,producto-b,CAT_002,Hogar,SKU456,,Marca B,1,100.00,20.00,12.16,0,80.00,COUPON_001,SUMMER20,20.00,percentage,
```

### **Método por Base de Datos**

#### Tabla de Transacciones
```sql
CREATE TABLE transactions (
    transaction_id VARCHAR(255) PRIMARY KEY,
    order_id VARCHAR(255) NOT NULL,
    user_id VARCHAR(255) NOT NULL,
    transaction_type VARCHAR(50),
    status VARCHAR(50) NOT NULL,
    currency VARCHAR(10) NOT NULL,
    payment_method VARCHAR(50),
    payment_provider VARCHAR(100),
    payment_reference VARCHAR(255),
    payment_installments INTEGER,
    payment_status VARCHAR(50),
    payment_date TIMESTAMP,
    payment_card_type VARCHAR(50),
    payment_card_brand VARCHAR(50),
    payment_card_last_digits VARCHAR(10),
    payment_card_holder_name VARCHAR(255),
    order_date TIMESTAMP NOT NULL,
    transaction_date TIMESTAMP NOT NULL,
    approval_date TIMESTAMP,
    rejection_date TIMESTAMP,
    cancellation_date TIMESTAMP,
    origin_channel VARCHAR(50),
    origin_platform VARCHAR(100),
    attribution_type VARCHAR(50),
    attribution_entity_id VARCHAR(255),
    pricing_net_amount DECIMAL(15,2) NOT NULL,
    pricing_discount_amount DECIMAL(15,2),
    pricing_tax_amount DECIMAL(15,2),
    pricing_shipping_amount DECIMAL(15,2),
    pricing_final_amount DECIMAL(15,2),
    shipping_name VARCHAR(255),
    shipping_street VARCHAR(255),
    shipping_number VARCHAR(50),
    shipping_dept_number VARCHAR(50),
    shipping_city VARCHAR(100),
    shipping_commune VARCHAR(100),
    shipping_region VARCHAR(100),
    shipping_lat VARCHAR(20),
    shipping_long VARCHAR(20),
    billing_name VARCHAR(255),
    billing_street VARCHAR(255),
    billing_number VARCHAR(50),
    billing_dept_number VARCHAR(50),
    billing_city VARCHAR(100),
    billing_commune VARCHAR(100),
    billing_region VARCHAR(100),
    billing_lat VARCHAR(20),
    billing_long VARCHAR(20),
    coupon_id VARCHAR(255),
    coupon_code VARCHAR(100),
    coupon_discount_value DECIMAL(15,2),
    coupon_type VARCHAR(50),
    notes TEXT,
    tags VARCHAR(1000),
    attributes JSON,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP,
    _created_at TIMESTAMP,
    _updated_at TIMESTAMP,
    INDEX idx_updated_at (updated_at),
    INDEX idx_user_id (user_id),
    INDEX idx_transaction_date (transaction_date)
);
```

#### Tabla de Transaction Items
```sql
CREATE TABLE transaction_items (
    transaction_id VARCHAR(255) NOT NULL,
    product_id VARCHAR(255) NOT NULL,
    item_id VARCHAR(255) NOT NULL,
    display_name VARCHAR(255),
    slug VARCHAR(255),
    category_id VARCHAR(255),
    category_name VARCHAR(255),
    sku VARCHAR(100),
    gtin VARCHAR(100),
    brand VARCHAR(100),
    quantity INTEGER NOT NULL,
    pricing_net_amount DECIMAL(15,2) NOT NULL,
    pricing_discount_amount DECIMAL(15,2),
    pricing_tax_amount DECIMAL(15,2),
    pricing_shipping_amount DECIMAL(15,2),
    pricing_final_amount DECIMAL(15,2),
    coupon_id VARCHAR(255),
    coupon_code VARCHAR(100),
    coupon_discount_value DECIMAL(15,2),
    coupon_type VARCHAR(50),
    attributes JSON,
    PRIMARY KEY (transaction_id, item_id),
    FOREIGN KEY (transaction_id) REFERENCES transactions(transaction_id),
    INDEX idx_product_id (product_id),
    INDEX idx_category_id (category_id)
);
```

#### Consulta de Sincronización
```sql
-- Consultar transacciones modificadas desde la última sincronización
SELECT * FROM transactions
WHERE _updated_at > :last_sync_timestamp
ORDER BY _updated_at ASC;

-- Consultar items de transacciones modificadas
SELECT ti.* FROM transaction_items ti
JOIN transactions t ON ti.transaction_id = t.transaction_id
WHERE t._updated_at > :last_sync_timestamp
ORDER BY t._updated_at ASC, ti.item_id ASC;
```
