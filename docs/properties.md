# Entidades de Propiedades

Las entidades de propiedades representan objetos y sus características en el sistema Reten. Estas entidades son actualizables y mantienen el estado actual de los diferentes componentes del sistema.

## Comercios (Clientes del CPG)

Representa los establecimientos comerciales que interactúan con la plataforma.

### Estructura de Datos

```json
{
  // Identificadores
  "user_id": "string",           // Identificador principal (not null)
  "external_id": "string",       // Identificador externo del cliente

  // Información básica
  "name": "string",              // Nombre del comercio
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente
  "_created_at": "timestamp",    // Fecha de creación en Reten
  "_updated_at": "timestamp",    // Fecha de última actualización en Reten
  "sign_up_date": "timestamp",   // Fecha de registro (not null)
  "set_up_date": "timestamp",    // Fecha de configuración inicial

  // Ubicación
  "country": "string",           // País del comercio

  // Información de contacto
  "contacts": [{
    "email": "string",
    "phone": "string",
    "type": "string"            // primary, secondary, billing, etc.
  }],

  // Información personal
  "personal_info": {
    "first_name": "string",
    "last_name": "string",
    "country": "string",
    "date_of_birth": "timestamp"
  },

  // Direcciones
  "addresses": [{
    "type": "string",           // shipping, billing, warehouse, etc.
    "name": "string",           // Nombre de la dirección
    "street": "string",         // Calle
    "commune": "string",        // Comuna
    "city": "string",          // Ciudad
    "region": "string",        // Región
    "number": "string",        // Número
    "dept_number": "string",   // Número de departamento/oficina
    "lat": "string",          // Latitud
    "long": "string",         // Longitud
    "is_default": "boolean"   // Indica si es la dirección principal
  }],

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string"
  }],

  // Grupos y clasificaciones
  "groups": [{
    "group_type": "string",
    "attributes": [{
      "key": "string",
      "value": "string"
    }]
  }],

  // IDs adicionales
  "miscellaneous_id": [{
    "platform": "string",      // Plataforma relacionada
    "misc_id": "string"       // ID en esa plataforma
  }]
}
```

### Campos Requeridos

1. **Identificación**
   - `user_id`: Identificador único del comercio
   - `external_id`: Identificador en el sistema del cliente

2. **Fechas Obligatorias**
   - `created_at`: Fecha de creación original
   - `sign_up_date`: Fecha de registro en la plataforma

### Campos Opcionales pero Recomendados

1. **Información de Contacto**
   - Al menos un contacto en el array `contacts` con:
     - `email`: Email principal de contacto
     - `phone`: Teléfono de contacto
   - `type`: "primary"

2. **Dirección Principal**
   - Al menos una dirección en el array `addresses` con:
     - `type`: Tipo de dirección
     - `street`: Calle
     - `city`: Ciudad
     - `region`: Región/Estado/Provincia
     - `is_default`: true

### Ejemplos de Uso

#### Múltiples Direcciones
```json
{
  "addresses": [{
    "type": "billing",
    "name": "Oficina Central",
    "street": "Av. Principal",
    "city": "Santiago",
    "is_default": true
  }, {
    "type": "shipping",
    "name": "Bodega Norte",
    "street": "Calle Industrial",
    "city": "Santiago",
    "is_default": false
  }]
}
```

#### Múltiples Contactos
```json
{
  "contacts": [{
    "type": "primary",
    "email": "principal@comercio.com",
    "phone": "+56912345678"
  }, {
    "type": "billing",
    "email": "facturacion@comercio.com",
    "phone": "+56987654321"
  }]
}
```

### Grupos y Clasificaciones

Los comercios pueden pertenecer a múltiples grupos, cada uno con sus propios atributos:

```json
{
  "groups": [{
    "group_type": "channel",     // Ejemplo: canal de ventas
    "attributes": [{
      "key": "category",
      "value": "retail"
    }]
  }, {
    "group_type": "segment",     // Ejemplo: segmentación
    "attributes": [{
      "key": "size",
      "value": "medium"
    }]
  }]
}
```

### Atributos Personalizados

Los atributos permiten extender la información del comercio con datos específicos:

```json
{
  "attributes": [{
    "key": "payment_terms",
    "value": "30_days"
  }, {
    "key": "credit_score",
    "value": "A+"
  }]
}
```

### IDs Adicionales

Permite mantener referencias a identificadores en otras plataformas:

```json
{
  "miscellaneous_id": [{
    "platform": "erp_system",
    "misc_id": "ERP123"
  }, {
    "platform": "legacy_system",
    "misc_id": "OLD456"
  }]
}
```

### Notas de Implementación

1. **Gestión de Fechas**
   - Todas las fechas deben estar en formato ISO 8601
   - `sign_up_date` y `created_at` son obligatorios
   - `set_up_date` se establece cuando el comercio completa su configuración inicial

2. **Gestión de Direcciones y Contactos**
   - Debe existir al menos una dirección marcada como `is_default: true`
   - Debe existir al menos un contacto con `type: "primary"`
   - Los tipos de dirección y contacto deben ser valores predefinidos

3. **Validaciones**
   - Emails deben ser únicos dentro del array de contactos
   - Las coordenadas geográficas deben ser válidas si se proporcionan
   - Los `group_type` deben corresponder a tipos predefinidos

4. **Actualización de Datos**
   - `_updated_at` se actualiza automáticamente
   - Los cambios en arrays (addresses, contacts) deben mantener la integridad de los datos

5. **Consideraciones de Privacidad**
   - La información personal debe manejarse según regulaciones locales
   - Algunos campos pueden requerir encriptación

## Vendedores (Usuarios del CPG)

Representa los usuarios que tienen acceso a la plataforma:

```json
{
  // Identificadores
  "id": "string",              // Identificador único (not null)
  "external_id": "string",     // Identificador externo
  "data_object_id": "string",  // ID del objeto de datos (not null)

  // Información básica
  "name": {
    "first_name": "string",    // Nombre (not null)
    "last_name": "string",     // Apellido (not null)
    "display_name": "string"   // Nombre para mostrar
  },
  "email": "string",           // Email principal (not null)
  "phone": "string",           // Teléfono de contacto

  // Permisos y accesos
  "role": "string",            // Rol principal (not null)
  "permissions": ["string"],   // Lista de permisos
  "status": "string",          // Estado del usuario (active, inactive, blocked)

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp",   // Última actualización (not null)
  "_created_at": "timestamp",  // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp",  // Última actualización en BigQuery (not null)
  "deleted_at": "timestamp"    // Fecha de eliminación (soft delete)
}
```

## Productos

Catálogo de productos disponibles:

```json
{
  // Identificadores
  "product_id": "string",      // Identificador único del producto (not null)
  "data_object_id": "string",  // ID del objeto de datos (not null)

  // Identificadores de producto
  "identifiers": {
    "gtin": "string",         // Código GTIN/EAN/UPC
    "sku": "string",          // SKU interno
    "mpn": "string"          // Número de parte del fabricante
  },

  // Información básica
  "name": {
    "display_name": "string", // Nombre para mostrar (not null)
    "short_name": "string",   // Nombre corto para listados
    "slug": "string",        // URL amigable (not null)
    "short_description": "string", // Descripción corta
    "description": "string"  // Descripción completa
  },

  // Categorización
  "categorization": {
    "category_id": "string",  // ID de la categoría principal
    "categories": ["string"], // IDs de todas las categorías
    "brand": "string",       // Marca del producto
    "tags": ["string"]       // Tags para búsqueda
  },

  // Características físicas
  "physical": {
    "weight": "number",      // Peso en gramos
    "dimensions": {
      "length": "number",    // Largo en cm
      "width": "number",     // Ancho en cm
      "height": "number"     // Alto en cm
    },
    "unit": "string"        // Unidad de venta (unit, kg, lt, etc)
  },

  // Composición
  "composition": {
    "materials": ["string"], // Materiales principales
    "ingredients": ["string"], // Ingredientes si aplica
    "attributes": [{         // Atributos de composición
      "key": "string",
      "value": "string"
    }]
  },

  // Atributos comerciales
  "commercial": {
    "price": "number",      // Precio base
    "currency": "string",   // Moneda
    "tax_rate": "number",   // Tasa de impuesto
    "status": "string"      // Estado (active, inactive, discontinued)
  },

  // Atributos flexibles
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"
  }],

  // Marcas temporales
  "created_at": "timestamp", // Fecha de creación (not null)
  "updated_at": "timestamp", // Última actualización (not null)
  "_created_at": "timestamp", // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp" // Última actualización en BigQuery (not null)
}
```

## Cupones

Promociones y descuentos:

```json
{
  // Identificadores
  "coupon_id": "string",       // Identificador único del cupón (not null)
  "code": "string",            // Código del cupón para uso del cliente (not null)
  "data_object_id": "string",  // ID del objeto de datos (not null)

  // Información básica
  "name": {
    "display_name": "string",  // Nombre para mostrar (not null)
    "description": "string"    // Descripción del cupón
  },

  // Beneficio del cupón
  "benefit": {
    "type": "string",          // Tipo de beneficio (percentage, fixed_amount, free_shipping) (not null)
    "discount_value": "number", // Valor del descuento (not null)
    "minimum_purchase": "number" // Monto mínimo de compra requerido
  },

  // Estado y vigencia
  "is_active": "boolean",      // Estado activo del cupón (not null)
  "validity": {
    "start_date": "timestamp", // Fecha de inicio de vigencia (not null)
    "end_date": "timestamp"    // Fecha de término de vigencia
  },

  // Restricciones de uso
  "usage_limits": {
    "max_uses": "number",      // Número máximo de usos totales
    "uses_per_user": "number"  // Número máximo de usos por usuario
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
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp",   // Última actualización (not null)
  "_created_at": "timestamp",  // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp"   // Última actualización en BigQuery (not null)
}
```

## Suscripción Canales

Configuración de suscripciones a canales de comunicación:

```json
{
  // Identificadores
  "subscription_id": "string", // Identificador único de la suscripción (not null)
  "user_id": "string",        // Identificador del usuario (not null)
  "data_object_id": "string", // ID del objeto de datos (not null)

  // Información del canal
  "channel": {
    "type": "string",         // Tipo de canal (email, sms, push, whatsapp) (not null)
    "identifier": "string",   // Identificador del canal (email, teléfono, token) (not null)
    "name": "string",         // Nombre descriptivo del canal
    "status": "string"        // Estado del canal (active, inactive, pending, blocked)
  },

  // Consentimientos y preferencias
  "preferences": {
    "marketing": "boolean",    // Comunicaciones de marketing
    "notifications": "boolean", // Notificaciones del sistema
    "promotions": "boolean",   // Ofertas y promociones
    "news": "boolean",         // Noticias y actualizaciones
    "orders": "boolean",       // Actualizaciones de órdenes
    "support": "boolean"       // Comunicaciones de soporte
  },

  // Configuración de frecuencia
  "frequency": {
    "type": "string",         // Tipo de frecuencia (immediate, daily, weekly, custom)
    "time_zone": "string",    // Zona horaria del usuario
    "quiet_hours": {          // Horas de no disturbar
      "start": "string",      // Hora de inicio (HH:mm)
      "end": "string"         // Hora de fin (HH:mm)
    },
    "custom_schedule": JSON   // Configuración personalizada de horarios
  },

  // Metadatos de verificación
  "verification": {
    "is_verified": "boolean", // Canal verificado
    "verified_at": "timestamp", // Fecha de verificación
    "verification_method": "string", // Método de verificación usado
    "last_verification_attempt": "timestamp" // Último intento de verificación
  },

  // Marcas temporales
  "created_at": "timestamp",  // Fecha de creación (not null)
  "updated_at": "timestamp",  // Última actualización (not null)
  "_created_at": "timestamp", // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp"  // Última actualización en BigQuery (not null)
}
```

## Categorías

Clasificación jerárquica de productos:

```json
{
  // Identificadores
  "category_id": "string",     // Identificador único de la categoría (not null)
  "parent_id": "string",       // ID de la categoría padre (null si es raíz)
  "data_object_id": "string",  // ID del objeto de datos (not null)

  // Información básica
  "name": {
    "display_name": "string",  // Nombre para mostrar (not null)
    "slug": "string",         // URL amigable (not null)
    "description": "string"   // Descripción de la categoría
  },

  // Metadatos
  "metadata": {
    "level": "number",        // Nivel en la jerarquía (0 para raíz)
    "path": ["string"],       // Array de IDs desde la raíz hasta esta categoría
    "is_leaf": "boolean",     // Indica si es una categoría final
    "has_products": "boolean", // Indica si tiene productos asociados
    "position": "number",     // Posición para ordenamiento manual
    "tags": ["string"]        // Tags para búsqueda alternativa
  },

  // SEO
  "seo": {
    "title": "string",        // Título para SEO
    "description": "string",  // Descripción para SEO
    "keywords": ["string"],   // Palabras clave
    "canonical_url": "string" // URL canónica
  },

  // Marcas temporales
  "created_at": "timestamp",  // Fecha de creación (not null)
  "updated_at": "timestamp",  // Última actualización (not null)
  "_created_at": "timestamp", // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp"  // Última actualización en BigQuery (not null)
}
```

### Notas Importantes

1. Todas las entidades de propiedades mantienen un historial de cambios
2. Los cambios son auditables a través de las marcas temporales
3. Las actualizaciones son permitidas y esperadas
4. Cada entidad puede tener campos adicionales específicos según su naturaleza 