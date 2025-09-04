# :material-ticket-percent: Cupones

Los cupones representan los descuentos y promociones que pueden ser aplicados a las órdenes en la plataforma Reten. Esta entidad almacena toda la información relevante sobre las condiciones, restricciones y beneficios de cada cupón.

## Estructura de Datos

La estructura de cupones está diseñada para ser simple y flexible, con campos básicos requeridos y campos adicionales opcionales según las necesidades del negocio.

### Estructura Simplificada

```json
{
  // === CAMPOS BÁSICOS (REQUERIDOS) ===
  "coupon_id": "string",           // Identificador único del cupón
  "code": "string",                // Código del cupón para uso del cliente
  "name": "string",                // Nombre para mostrar
  "type": "string",                // Tipo de beneficio (percentage, fixed_amount, free_shipping)
  "discount_value": "number",      // Valor del descuento
  "is_active": "boolean",          // Estado activo del cupón
  "start_date": "timestamp",       // Fecha de inicio de vigencia
  "end_date": "timestamp",         // Fecha de término de vigencia
  "created_at": "timestamp",       // Fecha de creación
  "updated_at": "timestamp",       // Última actualización

  // === CAMPOS OPCIONALES (SEGÚN NECESIDAD) ===
  "description": "string",         // Descripción del cupón
  "minimum_purchase": "number",    // Monto mínimo de compra requerido
  "max_uses": "number",            // Número máximo de usos totales
  "uses_per_user": "number",       // Número máximo de usos por usuario

  // === ATRIBUTOS PERSONALIZADOS ===
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // === MARCAS TEMPORALES DE SINCRONIZACIÓN ===
  "_created_at": "timestamp",    // Fecha de creación del registro en Método de Conexión con Reten
  "_updated_at": "timestamp"     // Fecha de última actualización del registro en Método de Conexión con Reten
}
```

## Ejemplos de Implementación Flexible

### 🟢 Implementación Básica (Solo Campos Requeridos)

Cupones básicos y funcionalidad esencial.

```json
{
  "coupon_id": "caf3b866-1179-48e9-8d87-8cf16e908a43",
  "code": "WELCOME20",
  "name": "20% de Descuento Bienvenida",
  "type": "percentage",
  "discount_value": 20,
  "is_active": true,
  "start_date": "2024-01-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

### 🟡 Implementación con Campos Opcionales

Agregando campos adicionales según las necesidades del negocio.

```json
{
  "coupon_id": "COUPON_002",
  "code": "FREESHIP",
  "name": "Envío Gratis",
  "type": "free_shipping",
  "discount_value": 0,
  "is_active": true,
  "start_date": "2024-01-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "description": "Envío gratis en pedidos superiores a $30.000",
  "minimum_purchase": 30000,
  "max_uses": 1000,
  "uses_per_user": 1,
  "attributes": [
    {
      "key": "campaign_id",
      "value": "FREESHIP2024",
      "type": "string"
    },
    {
      "key": "target_audience",
      "value": "all_customers",
      "type": "string"
    },
    {
      "key": "redemption_channel",
      "value": "online_only",
      "type": "string"
    }
  ],
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

### 🔴 Implementación con Atributos Personalizados Avanzados

Para cupones empresariales con atributos personalizados extensos y metadata rica.

```json
{
  "coupon_id": "COUPON_003",
  "code": "PREMIUM25",
  "name": "25% Premium",
  "type": "percentage",
  "discount_value": 25,
  "is_active": true,
  "start_date": "2024-01-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "description": "Descuento exclusivo para clientes premium",
  "minimum_purchase": 50000,
  "max_uses": 500,
  "uses_per_user": 2,
  "attributes": [
    {
      "key": "campaign_id",
      "value": "PREMIUM_Q1_2024",
      "type": "string"
    },
    {
      "key": "target_audience",
      "value": "premium_customers",
      "type": "string"
    },
    {
      "key": "customer_segments",
      "value": "VIP,GOLD",
      "type": "string"
    },
    {
      "key": "redemption_channel",
      "value": "all_channels",
      "type": "string"
    },
    {
      "key": "marketing_budget",
      "value": "50000",
      "type": "number"
    },
    {
      "key": "expected_roi",
      "value": "3.5",
      "type": "number"
    },
    {
      "key": "is_seasonal",
      "value": "false",
      "type": "boolean"
    },
    {
      "key": "campaign_start_date",
      "value": "2024-01-15",
      "type": "date"
    },
    {
      "key": "campaign_end_date",
      "value": "2024-03-31",
      "type": "date"
    }
  ],
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

## Tipos de Beneficio

Los tipos de beneficio disponibles son:

- `percentage`: Descuento porcentual sobre el monto
- `fixed_amount`: Descuento de monto fijo
- `free_shipping`: Envío gratis
- `fixed_price`: Precio fijo especial

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el cliente**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan en la base de datos o archivo CSV y que no estén definidas en el modelo estándar de cupones.

### **Cómo Funciona:**
1. **Cliente envía** datos con columnas adicionales (ej: `campaign_id`, `target_audience`, `redemption_channel`)
2. **Reten detecta** automáticamente las columnas no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas columnas adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos de campaña**: Información particular de cada promoción
- **Metadatos de integración**: Datos del sistema origen que no tienen equivalente en Reten
- **Atributos de negocio**: Campos específicos de la estrategia de marketing
- **Configuraciones personalizadas**: Parámetros únicos de cada cupón

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "campaign_id",
    "value": "SUMMER2024",
    "type": "string"
  },
  {
    "key": "target_audience",
    "value": "new_customers",
    "type": "string"
  },
  {
    "key": "redemption_channel",
    "value": "online_only",
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

## Campos Detallados

### Identificadores

| Campo     | Tipo   | Requerido | Descripción                     |
| --------- | ------ | --------- | ------------------------------- |
| coupon_id | string | Sí        | Identificador único del cupón   |
| code      | string | Sí        | Código para redención del cupón |

### Información Básica

| Campo        | Tipo   | Requerido | Descripción           |
| ------------ | ------ | --------- | --------------------- |
| display_name | string | Sí        | Nombre para mostrar   |
| description  | string | No        | Descripción del cupón |

**Estados Válidos del Cupón:**

- `active`: Cupón activo y disponible
- `inactive`: Cupón temporalmente desactivado
- `expired`: Cupón expirado
- `depleted`: Cupón sin usos disponibles

### Beneficios

| Campo            | Tipo   | Descripción                      |
| ---------------- | ------ | -------------------------------- |
| type             | string | Tipo de beneficio                |
| discount_value   | number | Valor del descuento              |
| minimum_purchase | number | Monto mínimo de compra requerido |

### Atributos Personalizados

| Campo      | Tipo  | Requerido | Descripción                       |
| ---------- | ----- | --------- | --------------------------------- |
| attributes | array | No        | Lista de atributos personalizados |

### Marcas Temporales de Sincronización

| Campo       | Tipo      | Requerido | Descripción                                                                |
| ----------- | --------- | --------- | -------------------------------------------------------------------------- |
| _created_at | timestamp | Sí        | Fecha de creación del registro en Método de Conexión con Reten             |
| _updated_at | timestamp | Sí        | Fecha de última actualización del registro en Método de Conexión con Reten |

**Tipos de Beneficio:**

- `percentage`: Descuento porcentual
- `fixed_amount`: Descuento de monto fijo
- `free_shipping`: Envío gratis
- `fixed_price`: Precio fijo especial

## Validaciones

### Validaciones de Campos Requeridos

#### Identificadores
- `coupon_id` debe ser único en todo el sistema
- `code` debe ser único y alfanumérico
- `code` debe ser fácil de recordar y usar

#### Beneficios
- `type` debe ser uno de los valores válidos: `percentage`, `fixed_amount`, `free_shipping`, `fixed_price`
- `discount_value` debe ser un número válido
- Para `free_shipping`, `discount_value` debe ser 0

#### Vigencia
- `start_date` debe ser anterior a `end_date`
- Fechas deben estar en formato ISO 8601
- Zona horaria debe ser válida

#### Estado
- `is_active` debe ser un valor booleano válido

### Validaciones de Campos Opcionales

#### Límites de Uso
- `max_uses` debe ser mayor a cero si está presente
- `uses_per_user` debe ser mayor a cero si está presente
- `uses_per_user` debe ser menor o igual a `max_uses` si ambos están presentes

#### Compra Mínima
- `minimum_purchase` debe ser mayor a cero si está presente

### Validaciones de Atributos Personalizados

#### Estructura General
- Las claves de atributos deben ser únicas por cupón
- Los valores deben corresponder al tipo esperado
- El campo `attributes` es construido automáticamente por Reten

#### Tipos de Datos
- `string`: Texto libre sin restricciones de longitud
- `number`: Números enteros o decimales válidos
- `date`: Fechas en formato ISO 8601 válido
- `boolean`: Valores true/false únicamente

## Compatibilidad con Implementaciones Existentes

Esta estructura simplificada es compatible con implementaciones existentes como NIU MX, que utiliza solo los campos básicos requeridos:
- `coupon_id`, `code`, `name`, `type`, `discount_value`
- `start_date`, `end_date`, `is_active`
- `created_at`, `updated_at`

Los campos adicionales son completamente opcionales y se pueden implementar según las necesidades del negocio, sin afectar la funcionalidad básica.

## Ejemplos de Uso

### Cupón de Descuento Porcentual (Nivel Intermedio)

```json
{
  "coupon_id": "COUPON_001",
  "code": "WELCOME20",
  "name": "20% de Descuento Bienvenida",
  "description": "Descuento de bienvenida para nuevos clientes",
  "type": "percentage",
  "discount_value": 20,
  "minimum_purchase": 20000,
  "is_active": true,
  "start_date": "2024-03-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "max_uses": 1000,
  "uses_per_user": 1,
  "created_at": "2024-03-01T00:00:00Z",
  "updated_at": "2024-03-01T00:00:00Z",
  "attributes": [
    {
      "key": "target_audience",
      "value": "new_customers",
      "type": "string"
    }
  ],
  "_created_at": "2024-03-01T00:00:00Z",
  "_updated_at": "2024-03-01T00:00:00Z"
}
```

### Cupón de Envío Gratis (Nivel Intermedio)

```json
{
  "coupon_id": "COUPON_002",
  "code": "FREESHIP",
  "name": "Envío Gratis en tu Primera Compra",
  "description": "Envío gratis en pedidos superiores a $30.000",
  "type": "free_shipping",
  "discount_value": 0,
  "minimum_purchase": 30000,
  "is_active": true,
  "start_date": "2024-01-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "max_uses": 1000,
  "uses_per_user": 1,
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "attributes": [
    {
      "key": "redemption_type",
      "value": "first_purchase",
      "type": "string"
    }
  ],
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

### Cupón para Productos Específicos (Nivel Avanzado)

```json
{
  "coupon_id": "COUPON_003",
  "code": "PAINT25",
  "name": "25% en Pinturas Premium",
  "description": "Descuento exclusivo en línea de pinturas premium",
  "type": "percentage",
  "discount_value": 25,
  "minimum_purchase": 25000,
  "is_active": true,
  "start_date": "2024-01-01T00:00:00Z",
  "end_date": "2024-12-31T23:59:59Z",
  "max_uses": 500,
  "uses_per_user": 2,
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "attributes": [
    {
      "key": "included_products",
      "value": "PAINT_PREMIUM_001,PAINT_PREMIUM_002",
      "type": "string"
    },
    {
      "key": "min_quantity",
      "value": "1",
      "type": "number"
    },
    {
      "key": "included_categories",
      "value": "PAINT,DECORATION",
      "type": "string"
    },
    {
      "key": "product_line",
      "value": "premium_paint",
      "type": "string"
    }
  ],
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

## 🔄 Integración

### **Método por Archivo**
Los cupones se cargan en archivos CSV con las columnas correspondientes:

```csv
coupon_id,code,display_name,description,type,discount_value,is_active,start_date,end_date,minimum_purchase,max_uses,uses_per_user,attributes,created_at,updated_at,_created_at,_updated_at
COUPON1,WELCOME20,Descuento de Bienvenida,20% de descuento en primera compra,percentage,20,true,2024-01-01T00:00:00Z,2024-12-31T23:59:59Z,,1000,1,"",2024-01-15T10:00:00Z,2024-01-15T10:00:00Z,2024-01-15T10:00:00Z,2024-01-15T10:00:00Z
COUPON2,SUMMER15,Descuento de Verano,15% de descuento en productos de verano,percentage,15,true,2024-06-01T00:00:00Z,2024-08-31T23:59:59Z,50000,500,2,"",2024-01-15T11:00:00Z,2024-01-15T11:00:00Z,2024-01-15T11:00:00Z,2024-01-15T11:00:00Z
```

### **Método por Base de Datos**
Los cupones se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT
    coupon_id,
    code,
    display_name,
    description,
    type,
    discount_value,
    is_active,
    start_date,
    end_date,
    minimum_purchase,
    max_uses,
    uses_per_user,
    attributes,
    created_at,
    updated_at,
    _created_at,
    _updated_at
FROM coupons
WHERE _updated_at > '2024-01-15T00:00:00Z'
ORDER BY _updated_at ASC;
```
