# Cupones

Los cupones representan los descuentos y promociones que pueden ser aplicados a las órdenes en la plataforma Reten. Esta entidad almacena toda la información relevante sobre las condiciones, restricciones y beneficios de cada cupón.

## Estructura de Datos

```json
{
  // Identificadores
  "coupon_id": "string",        // Identificador único del cupón (not null)
  "code": "string",             // Código del cupón para uso del cliente (not null)
  "external_id": "string",      // Identificador externo del cliente

  // Información básica
  "name": {
    "display_name": "string",    // Nombre para mostrar (not null)
    "description": "string"      // Descripción del cupón
  },

  // Beneficio del cupón
  "benefit": {
    "type": "string",           // Tipo de beneficio (percentage, fixed_amount, free_shipping) (not null)
    "discount_value": "number",  // Valor del descuento (not null)
    "minimum_purchase": "number" // Monto mínimo de compra requerido
  },

  // Estado y vigencia
  "is_active": "boolean",       // Estado activo del cupón (not null)
  "validity": {
    "start_date": "timestamp",  // Fecha de inicio de vigencia (not null)
    "end_date": "timestamp"     // Fecha de término de vigencia
  },

  // Restricciones de uso
  "usage_limits": {
    "max_uses": "number",       // Número máximo de usos totales
    "uses_per_user": "number"   // Número máximo de usos por usuario
  },

  // Reglas de aplicación
  "rules": {
    "category_rules": JSON,    // Reglas de categorías
    "time_rules": JSON,        // Reglas de horarios
    "location_rules": JSON,    // Reglas de ubicación
    "product_rules": JSON,     // Reglas de productos
    "combo_rules": JSON,       // Reglas de combos
    "user_rules": JSON,        // Reglas de usuarios
    "order_rules": JSON,       // Reglas de órdenes
    "custom_rules": JSON       // Reglas personalizadas
  },

  // Marcas temporales
  "created_at": "timestamp",    // Fecha de creación (not null)
  "updated_at": "timestamp",    // Última actualización (not null)
}
```

## Tipos de Beneficio

Los tipos de beneficio disponibles son:

- `percentage`: Descuento porcentual sobre el monto
- `fixed_amount`: Descuento de monto fijo
- `free_shipping`: Envío gratis
- `fixed_price`: Precio fijo especial

## Formato de Reglas

Cada tipo de regla se almacena como un campo de tipo `JSON` en la base de datos, lo que permite una estructura flexible y consultas eficientes:

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

### Validaciones Generales

#### Identificadores

- `coupon_id` debe ser único en todo el sistema
- `code` debe ser único y alfanumérico
- `code` debe ser fácil de recordar y usar

#### Vigencia

- `start_date` debe ser anterior a `end_date`
- Fechas deben estar en formato ISO 8601
- Zona horaria debe ser válida

#### Límites de Uso

- `max_uses` debe ser mayor a cero
- `uses_per_user` debe ser menor o igual a `max_uses`
- `minimum_purchase` debe ser mayor a cero

### Validaciones de Negocio

#### Beneficios

- Porcentajes deben estar entre 0 y 100
- Montos fijos deben ser positivos
- Moneda debe ser válida

#### Condiciones

- Métodos de pago deben existir
- Territorios deben ser válidos
- Horarios deben ser coherentes

## Ejemplos de Uso

### Cupón de Descuento Porcentual

```json
{
  "coupon_id": "COUPON_001",
  "code": "WELCOME20",
  "name": {
    "display_name": "20% de Descuento Bienvenida",
    "description": "Descuento de bienvenida para nuevos clientes"
  },
  "is_active": true,
  "validity": {
    "start_date": "2024-03-01T00:00:00Z",
    "end_date": "2024-12-31T23:59:59Z"
  },
  "usage_limits": {
    "max_uses": 1000,
    "uses_per_user": 1
  },
  "benefit": {
    "type": "percentage",
    "discount_value": 20,
    "minimum_purchase": 20000
  },
  "rules": {
    "category_rules": "new_customers"
  }
}
```

### Cupón de Envío Gratis

```json
{
  "coupon_id": "COUPON_002",
  "code": "FREESHIP",
  "name": {
    "display_name": "Envío Gratis en tu Primera Compra",
    "description": "Envío gratis en pedidos superiores a $30.000"
  },
  "benefit": {
    "type": "free_shipping",
    "minimum_purchase": 30000
  },
  "usage_limits": {
    "max_uses": 1000,
    "uses_per_user": 1
  },
  "rules": {
    "location_rules": "first_purchase"
  }
}
```

### Cupón para Productos Específicos

```json
{
  "coupon_id": "COUPON_003",
  "code": "PAINT25",
  "name": {
    "display_name": "25% en Pinturas Premium",
    "description": "Descuento exclusivo en línea de pinturas premium"
  },
  "benefit": {
    "type": "percentage",
    "discount_value": 25,
    "minimum_purchase": 25000
  },
  "rules": {
    "product_rules": "premium_paints"
  }
}
```

## Notas de Implementación

### Gestión de Redenciones

#### Validación en Tiempo Real

- Verificar vigencia
- Validar límites de uso
- Comprobar condiciones

#### Registro de Uso

- Actualizar contadores
- Registrar usuario y orden
- Mantener historial

### Optimización

#### Caché

- Cachear cupones activos
- Invalidar al actualizar
- Precalcular elegibilidad

#### Búsqueda

- Índices por código
- Búsqueda por condiciones
- Filtros de vigencia

## Integración con Otros Sistemas

### APIs

#### Endpoints Principales

```
GET    /api/v1/coupons/{coupon_id}
POST   /api/v1/coupons
PUT    /api/v1/coupons/{coupon_id}
PATCH  /api/v1/coupons/{coupon_id}
DELETE /api/v1/coupons/{coupon_id}
```

#### Endpoints de Validación

```
POST   /api/v1/coupons/validate
GET    /api/v1/coupons/{code}/check
POST   /api/v1/coupons/{code}/apply
```

### Webhooks

#### Eventos Disponibles

- `coupon.created`
- `coupon.updated`
- `coupon.status_changed`
- `coupon.redeemed`
- `coupon.depleted`

#### Formato de Payload

```json
{
  "event": "coupon.redeemed",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "coupon_id": "string",
    "code": "string",
    "order_id": "string",
    "user_id": "string",
    "discount_amount": "number"
  }
}
```

## Preguntas Frecuentes

**¿Cómo manejar cupones expirados?**

   - Mantener registro histórico
   - Notificar a usuarios afectados
   - Sugerir alternativas válidas

**¿Cómo gestionar conflictos entre cupones?**

   - Definir reglas de precedencia
   - Establecer compatibilidad
   - Mostrar mejor opción al usuario

**¿Cómo prevenir el abuso de cupones?**

   - Validar límites estrictamente
   - Implementar detección de fraude
   - Monitorear patrones de uso 