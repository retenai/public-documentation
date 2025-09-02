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

  // === REGLAS COMPLEJAS (OPCIONALES) ===
  "rules": {
    "category_rules": "JSON",      // Reglas de categorías como JSON estructurado
    "time_rules": "JSON",          // Reglas de horarios como JSON estructurado
    "location_rules": "JSON",      // Reglas de ubicación como JSON estructurado
    "product_rules": "JSON",       // Reglas de productos como JSON estructurado
    "user_rules": "JSON",          // Reglas de usuarios como JSON estructurado
    "order_rules": "JSON",         // Reglas de órdenes como JSON estructurado
    "custom_rules": "JSON"         // Reglas personalizadas como JSON estructurado
  }
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
  "updated_at": "2024-01-01T00:00:00Z"
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
  "uses_per_user": 1
}
```

### 🔴 Implementación con Reglas Complejas

Para cupones empresariales con reglas sofisticadas y múltiples restricciones.

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
  "rules": {
    "category_rules": {
      "included_categories": ["ELECTRONICS", "HOME"],
      "excluded_categories": ["FOOD"]
    },
    "time_rules": {
      "days": ["MONDAY", "TUESDAY", "WEDNESDAY"],
      "start_time": "09:00",
      "end_time": "18:00"
    },
    "location_rules": {
      "territories": ["SANTIAGO", "VALPARAISO"],
      "excluded_territories": ["PUNTA_ARENAS"]
    },
    "user_rules": {
      "customer_types": ["PREMIUM", "VIP"],
      "min_purchase_history": 100000
    }
  }
}
```

## Tipos de Beneficio

Los tipos de beneficio disponibles son:

- `percentage`: Descuento porcentual sobre el monto
- `fixed_amount`: Descuento de monto fijo
- `free_shipping`: Envío gratis
- `fixed_price`: Precio fijo especial

## Estructura de Reglas

Las reglas se implementan como objetos JSON estructurados para mantener consistencia y permitir validaciones robustas.

### Ejemplo de Reglas JSON

```json
{
  "category_rules": {
    "included_categories": ["ELECTRONICS", "HOME"],
    "excluded_categories": ["FOOD"],
    "min_category_value": 10000
  },
  "time_rules": {
    "days": ["MONDAY", "TUESDAY", "WEDNESDAY"],
    "start_time": "09:00",
    "end_time": "18:00"
  },
  "location_rules": {
    "territories": ["SANTIAGO", "VALPARAISO"],
    "excluded_territories": ["PUNTA_ARENAS"]
  }
}
```

## Formato de Reglas

Cada tipo de regla se almacena como un campo de tipo `string` o `JSON` en la base de datos, lo que permite una estructura flexible y consultas eficientes:

### Reglas de Categoría

```json
{
  "included_categories": ["category_1", "category_2"],
  "excluded_categories": ["category_3"]
}
```

### Reglas de Tiempo

```json
{
  "days": ["MONDAY", "TUESDAY"],
  "start_time": "09:00",
  "end_time": "18:00"
}
```

### Reglas de Ubicación

```json
{
  "territories": ["SANTIAGO", "VALPARAISO"],
  "excluded_territories": ["PUNTA_ARENAS"]
}
```

### Reglas de Producto

```json
{
  "included_products": ["SKU1", "SKU2"],
  "excluded_products": ["SKU3"],
  "min_quantity": 2
}
```

### Reglas de Combo

```json
{
  "required_combinations": [{
    "products": ["SKU1", "SKU2"],
    "quantity": 1
  }]
}
```

### Reglas de Usuario

```json
{
  "customer_types": ["NEW", "PREMIUM"],
  "excluded_users": ["USER1", "USER2"]
}
```

### Reglas de Orden

```json
{
  "min_items": 2,
  "payment_methods": ["CREDIT_CARD", "DEBIT_CARD"]
}
```

### Ejemplo de Uso en SQL

```sql
-- Crear tabla con campos JSON
CREATE TABLE cupones (
  id STRING,
  code STRING,
  rules STRUCT<
    category_rules JSON,
    time_rules JSON,
    location_rules JSON,
    product_rules JSON,
    combo_rules JSON,
    user_rules JSON,
    order_rules JSON,
    custom_rules JSON
  >
);

-- Insertar un cupón con reglas
INSERT INTO cupones (id, code, rules)
VALUES (
  'COUPON1',
  'WELCOME20',
  STRUCT(
    JSON '{"included_categories": ["ELECTRONICS"]}',
    JSON '{"days": ["MONDAY", "TUESDAY"]}',
    NULL,
    NULL,
    NULL,
    NULL,
    NULL,
    NULL
  )
);

-- Consultar cupones por categoría
SELECT id, code
FROM cupones
WHERE JSON_VALUE(rules.category_rules, '$.included_categories[0]') = 'ELECTRONICS';

-- Actualizar reglas de tiempo
UPDATE cupones
SET rules.time_rules = JSON '{"days": ["MONDAY", "TUESDAY", "WEDNESDAY"]}'
WHERE id = 'COUPON1';
```

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

### Validaciones de Reglas JSON

#### Estructura General
- Las reglas JSON deben tener estructura válida
- Los campos requeridos dentro de cada regla deben estar presentes
- Los valores de las reglas deben ser del tipo correcto

#### Validaciones Específicas por Tipo de Regla

##### Reglas de Categoría
- `included_categories` y `excluded_categories` deben ser arrays de strings
- No puede haber categorías en ambos arrays simultáneamente

##### Reglas de Tiempo
- `days` debe contener días válidos de la semana
- `start_time` y `end_time` deben estar en formato HH:MM
- `start_time` debe ser anterior a `end_time`

##### Reglas de Ubicación
- `territories` y `excluded_territories` deben ser arrays de strings
- No puede haber territorios en ambos arrays simultáneamente

##### Reglas de Usuario
- `customer_types` debe ser un array de strings válidos
- `min_purchase_history` debe ser un número positivo

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
  "rules": {
    "category_rules": "new_customers"
  }
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
  "rules": {
    "location_rules": "first_purchase"
  }
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
  "rules": {
    "product_rules": {
      "included_products": ["PAINT_PREMIUM_001", "PAINT_PREMIUM_002"],
      "min_quantity": 1
    },
    "category_rules": {
      "included_categories": ["PAINT", "DECORATION"]
    }
  }
}
```
