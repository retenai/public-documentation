# :material-package: Productos

Los productos representan los artículos comercializados a través de la plataforma Reten. Esta entidad almacena toda la información relevante sobre los productos, sus características, categorización y relaciones comerciales.

## Estructura de Datos

```json
{
  // Identificadores
  "product_id": "string",        // Identificador único del producto (not null)
  "sku": "string",               // Stock Keeping Unit, código único de producto para gestión de inventario (not null)
  "gtin": "string",              // Global Trade Item Number (código de barras) - opcional

  // Información básica
  "display_name": "string",      // Nombre comercial del producto (not null)
  "short_name": "string",        // Nombre corto para listados
  "slug": "string",              // Versión URL-friendly del nombre
  "short_description": "string", // Descripción corta del producto
  "description": "string",       // Descripción detallada del producto

  // Imágenes principales
  "main_image_url": "string",    // URL de la imagen principal
  "thumbnail_url": "string",     // URL de la miniatura

  // Categoría principal
  "category_id": "string",       // ID de la categoría principal
  "category_name": "string",     // Nombre de la categoría principal

  // Subcategoría (opcional)
  "subcategory_id": "string",    // ID de la subcategoría
  "subcategory_name": "string",  // Nombre de la subcategoría

  // Información de marca
  "brand": "string",       // Identificador de la marca

  // Tags para búsqueda y filtrado
  "tags": "string",        // Etiquetas separadas por comas (tag1,tag2,tag3)

  // Atributos personalizados - Cualquier columna no definida en el modelo se almacenará aquí
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // Marcas temporales del sistema origen
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de actualización en sistema cliente

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",    // Fecha de creación del registro en Método de Conexión con Reten
  "_updated_at": "timestamp"     // Fecha de última actualización del registro en Método de Conexión con Reten
}
```

## Campos Detallados

### Identificadores

| Campo      | Tipo   | Requerido | Descripción                                                 |
| ---------- | ------ | --------- | ----------------------------------------------------------- |
| product_id | string | Sí        | Identificador único del producto                            |
| sku        | string | Sí        | Stock Keeping Unit, código único para gestión de inventario |
| gtin       | string | No        | Global Trade Item Number (código de barras)                 |

### Información Básica

| Campo             | Tipo   | Requerido | Descripción                                |
| ----------------- | ------ | --------- | ------------------------------------------ |
| display_name      | string | Sí        | Nombre comercial del producto              |
| short_name        | string | No        | Nombre corto para listados                 |
| slug              | string | No        | Versión URL-friendly del nombre            |
| short_description | string | No        | Descripción corta para listados y previews |
| description       | string | No        | Descripción detallada del producto         |

### Imágenes Principales

| Campo          | Tipo   | Requerido | Descripción                |
| -------------- | ------ | --------- | -------------------------- |
| main_image_url | string | Sí        | URL de la imagen principal |
| thumbnail_url  | string | Sí        | URL de la miniatura        |

### Categoría Principal

| Campo         | Tipo   | Requerido | Descripción                      |
| ------------- | ------ | --------- | -------------------------------- |
| category_id   | string | Sí        | ID de la categoría principal     |
| category_name | string | Sí        | Nombre de la categoría principal |

### Subcategoría

| Campo            | Tipo   | Requerido | Descripción               |
| ---------------- | ------ | --------- | ------------------------- |
| subcategory_id   | string | No        | ID de la subcategoría     |
| subcategory_name | string | No        | Nombre de la subcategoría |

### Tags

| Campo | Tipo   | Requerido | Descripción                                    |
| ----- | ------ | --------- | ---------------------------------------------- |
| tags  | string | No        | Etiquetas separadas por comas (tag1,tag2,tag3) |

### Marcas Temporales

| Campo      | Tipo      | Requerido | Descripción                             |
| ---------- | --------- | --------- | --------------------------------------- |
| created_at | timestamp | Sí        | Fecha de creación en sistema cliente    |
| updated_at | timestamp | No        | Última actualización en sistema cliente |

### Marcas Temporales de Sincronización

| Campo       | Tipo      | Requerido | Descripción                                                                |
| ----------- | --------- | --------- | -------------------------------------------------------------------------- |
| _created_at | timestamp | Sí        | Fecha de creación del registro en Método de Conexión con Reten             |
| _updated_at | timestamp | Sí        | Fecha de última actualización del registro en Método de Conexión con Reten |

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el cliente**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan en la base de datos o archivo CSV y que no estén definidas en el modelo estándar de productos.

### **Cómo Funciona:**
1. **Cliente envía** datos con columnas adicionales (ej: `supplier_id`, `warranty_period`, `custom_field`)
2. **Reten detecta** automáticamente las columnas no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas columnas adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos del producto**: Información particular de cada catálogo
- **Metadatos de integración**: Datos del sistema origen que no tienen equivalente en Reten
- **Atributos de negocio**: Campos específicos de la industria o empresa
- **Configuraciones personalizadas**: Parámetros únicos del producto

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "supplier_id",
    "value": "SUP_001",
    "type": "string"
  },
  {
    "key": "warranty_period",
    "value": "24",
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

## Validaciones

### Validaciones Generales

### Identificadores

- `product_id` debe ser único en todo el sistema
- `sku` debe ser único por cliente
- `gtin` debe ser válido según estándar GS1

### Información Básica

- `display_name` no puede estar vacío
- `slug` debe ser único y URL-friendly
- Descripciones no deben exceder límites de caracteres

### Imágenes Principales

- `main_image_url` y `thumbnail_url` son requeridos
- URLs deben ser válidas y accesibles

### Categoría Principal

- `category_id` y `category_name` son requeridos
- La categoría debe existir en el maestro de categorías

### Subcategoría

- Si se proporciona `subcategory_id`, debe existir en el maestro de subcategorías
- Si se proporciona `subcategory_name`, debe corresponder con `subcategory_id`

### Validaciones de Negocio

### Atributos

- Las claves de atributos deben ser únicas por producto
- Los valores deben corresponder al tipo esperado
- El campo `attributes` es construido automáticamente por Reten

### Otros

- Tags deben ser válidos (formato separado por comas)
- Marca debe existir en el maestro

## Ejemplos de Uso

### Producto Simple

```json
{
  "product_id": "PROD_001",
  "sku": "SKU123",
  "display_name": "Pintura Látex Premium",
  "short_name": "Látex Premium",
  "slug": "pintura-latex-premium",
  "short_description": "Pintura látex de alta calidad",
  "main_image_url": "https://example.com/images/main.jpg",
  "thumbnail_url": "https://example.com/images/thumb.jpg",
  "category_id": "CAT_001",
  "category_name": "Pinturas",
  "subcategory_id": "SUB_001",
  "subcategory_name": "Látex",
  "brand": "BRD_001",
  "tags": "pintura,latex,interior,premium",
  "attributes": [
    {
      "key": "coverage",
      "value": "12",
      "type": "number"
    },
    {
      "key": "finish",
      "value": "matte",
      "type": "string"
    }
  ],
  "created_at": "2024-01-15T10:00:00Z",
  "updated_at": "2024-01-15T10:00:00Z",
  "_created_at": "2024-01-15T10:00:00Z",
  "_updated_at": "2024-01-15T10:00:00Z"
}
```

### Producto con Atributos Personalizados

```json
{
  "product_id": "PROD_002",
  "sku": "PACK456",
  "display_name": "Pack Pintura Interior",
  "short_name": "Pack Pintura",
  "slug": "pack-pintura-interior",
  "short_description": "Pack de pinturas para interiores",
  "main_image_url": "https://example.com/images/pack-main.jpg",
  "thumbnail_url": "https://example.com/images/pack-thumb.jpg",
  "category_id": "CAT_002",
  "category_name": "Packs de Pintura",
  "subcategory_id": "SUB_002",
  "subcategory_name": "Packs Especiales",
  "brand": "BRD_002",
  "tags": "pack,pintura,interior,combo",
  "attributes": [
    {
      "key": "supplier_id",
      "value": "SUP_001",
      "type": "string"
    },
    {
      "key": "warranty_period",
      "value": "24",
      "type": "number"
    },
    {
      "key": "color_family",
      "value": "neutrals",
      "type": "string"
    },
    {
      "key": "recommended_use",
      "value": "living_spaces",
      "type": "string"
    },
    {
      "key": "is_discontinued",
      "value": "false",
      "type": "boolean"
    }
  ],
  "created_at": "2024-01-15T11:00:00Z",
  "updated_at": "2024-01-15T11:00:00Z",
  "_created_at": "2024-01-15T11:00:00Z",
  "_updated_at": "2024-01-15T11:00:00Z"
}
```
## 🔄 Integración

### **Método por Archivo**
Los productos se cargan en archivos CSV con las columnas correspondientes:

```csv
product_id,sku,display_name,short_name,slug,short_description,main_image_url,thumbnail_url,category_id,category_name,subcategory_id,subcategory_name,brand,tags,attributes,created_at,updated_at,_created_at,_updated_at
PROD_001,SKU123,Pintura Látex Premium,Látex Premium,pintura-latex-premium,Pintura látex de alta calidad,https://example.com/images/main.jpg,https://example.com/images/thumb.jpg,CAT_001,Pinturas,SUB_001,Látex,BRD_001,"pintura,latex,interior,premium","",2024-01-15T10:00:00Z,2024-01-15T10:00:00Z,2024-01-15T10:00:00Z,2024-01-15T10:00:00Z
PROD_002,PACK456,Pack Pintura Interior,Pack Pintura,pack-pintura-interior,Pack de pinturas para interiores,https://example.com/images/pack-main.jpg,https://example.com/images/pack-thumb.jpg,CAT_002,Packs de Pintura,SUB_002,Packs Especiales,BRD_002,"pack,pintura,interior,combo","",2024-01-15T11:00:00Z,2024-01-15T11:00:00Z,2024-01-15T11:00:00Z,2024-01-15T11:00:00Z
```

### **Método por Base de Datos**
Los productos se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT
    product_id,
    sku,
    display_name,
    short_name,
    slug,
    short_description,
    main_image_url,
    thumbnail_url,
    category_id,
    category_name,
    subcategory_id,
    subcategory_name,
    brand,
    tags,
    attributes,
    created_at,
    updated_at,
    _created_at,
    _updated_at
FROM products
WHERE _updated_at > '2024-01-15T00:00:00Z'
ORDER BY _updated_at ASC;
```
