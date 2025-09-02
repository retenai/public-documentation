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
    
  // Información de marca
  "brand": "string",       // Identificador de la marca

  // Tags para búsqueda y filtrado
  "tags": ["string"],     // Lista de etiquetas para búsqueda y categorización adicional

  // Imágenes adicionales (opcional)
  "images": [{
    "url": "string",             // URL de la imagen
    "description": "string",     // Descripción de la imagen
    "type": "string",            // Tipo de imagen (producto, lifestyle, etc.)
    "position": "number",        // Orden de visualización
    "alt_text": "string"         // Texto alternativo para accesibilidad
  }],

  // Categorías adicionales (opcional)
  "categories": [{
    "category_id": "string",     // Referencia al ID de la categoría
    "category_name": "string",   // Nombre de la categoría
    "is_primary": "boolean",     // Indica si es la categoría principal del producto
    "attributes": [{            // Atributos específicos de la relación producto-categoría
      "key": "string",
      "value": "string",
      "type": "string"
    }]
  }],

  // Características físicas
  "physical": {
    "weight": {
      "value": "number",
      "unit": "string"
    },
    "dimensions": {
      "length": "number",
      "width": "number",
      "height": "number",
      "unit": "string"
    },
    "color": ["string"],
    "size": "string",
    "materials": ["string"]
  },

  // Composición
  "composition": {
    "is_pack": "boolean",
    "pack_details": {
      "units_per_pack": "number",
      "unit_type": "string",
      "unit_size": "number",
      "unit_measure": "string"
    },
    "is_combo": "boolean",
    "combo_details": {
      "combo_type": "string",
      "components": [{
        "sku": "string",
        "product_id": "string",
        "quantity": "number",
        "is_required": "boolean",
        "component_type": "string"
      }],
      "is_configurable": "boolean"
    },
    "display_unit": "string"
  },

  // Atributos personalizados
  "attributes": [{
    "key": "string",
    "value": "string",
    "type": "string"    // Tipo de valor (string, number, date, boolean)
  }],

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp"    // Última actualización (not null)
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

### Imágenes Adicionales (Opcional)

| Campo       | Tipo   | Descripción                                |
| ----------- | ------ | ------------------------------------------ |
| url         | string | URL de la imagen                           |
| description | string | Descripción de la imagen                   |
| type        | string | Tipo de imagen (producto, lifestyle, etc.) |
| position    | number | Orden de visualización                     |
| alt_text    | string | Texto alternativo para accesibilidad       |

**Nota:** Las imágenes principales (`main_image_url` y `thumbnail_url`) están en raíz. Este array es opcional para productos que requieran múltiples imágenes adicionales.

### Categorías Adicionales (Opcional)

| Campo         | Tipo    | Descripción                                             |
| ------------- | ------- | ------------------------------------------------------- |
| category_id   | string  | Referencia al ID de la categoría                        |
| category_name | string  | Nombre de la categoría                                  |
| is_primary    | boolean | Indica si es la categoría principal                     |
| attributes    | array   | Atributos específicos de la relación producto-categoría |

**Nota:** La categoría principal está en raíz (`category_id` y `category_name`). Este array es opcional para productos que requieran múltiples categorías.

### Tags

| Campo | Tipo     | Requerido | Descripción                                       |
| ----- | -------- | --------- | ------------------------------------------------- |
| tags  | string[] | No        | Lista de etiquetas para búsqueda y categorización |

### Composición

**Pack:**

- `is_pack`: Indica si es un pack
- `units_per_pack`: Cantidad de unidades por pack
- `unit_type`: Tipo de unidad
- `unit_size`: Tamaño de la unidad
- `unit_measure`: Unidad de medida

**Combo:**

- `is_combo`: Indica si es un combo
- `combo_type`: Tipo de combo
- `is_configurable`: Permite configuración
- `components`: Lista de productos que componen el combo

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

### Imágenes Adicionales (Opcional)

- Si se proporciona `images`, las URLs deben ser válidas y accesibles
- Las posiciones deben ser únicas
- Los tipos de imagen deben ser válidos

### Categoría Principal

- `category_id` y `category_name` son requeridos
- La categoría debe existir en el maestro de categorías

### Validaciones de Negocio

### Composición

- Si `is_pack` es true, `pack_details` debe estar completo
- Si `is_combo` es true, `combo_details` debe estar completo
- Componentes deben existir en el catálogo

### Categorías Adicionales (Opcional)

- Si se proporciona `categories`, las categorías deben existir en el maestro
- Los atributos de categoría deben ser válidos

### Otros

- Tags deben ser válidos
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
  "brand": "BRD_001",
  "tags": ["pintura", "latex", "interior", "premium"],
  "attributes": [{
    "key": "coverage",
    "value": "12",
    "type": "numeric"
  }, {
    "key": "finish",
    "value": "matte",
    "type": "string"
  }]
}
```

### Producto Pack

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
  "composition": {
    "is_pack": true,
    "pack_details": {
      "units_per_pack": 3,
      "unit_type": "galón",
      "unit_size": 1,
      "unit_measure": "galón"
    }
  },
  "brand": "BRD_002",
  "tags": ["pack", "pintura", "interior", "combo"],
  "attributes": [{
    "key": "color_family",
    "value": "neutrals",
    "type": "string"
  }, {
    "key": "recommended_use",
    "value": "living_spaces",
    "type": "string"
  }]
}
```
## 🔄 Integración

### **Método por Archivo**
Los productos se cargan en archivos CSV con las columnas correspondientes:

```csv
product_id,sku,display_name,short_name,slug,short_description,main_image_url,thumbnail_url,category_id,category_name,brand,created_at
PROD_001,SKU123,Pintura Látex Premium,Látex Premium,pintura-latex-premium,Pintura látex de alta calidad,https://example.com/images/main.jpg,https://example.com/images/thumb.jpg,CAT_001,Pinturas,BRD_001,2024-01-15T10:00:00Z
PROD_002,PACK456,Pack Pintura Interior,Pack Pintura,pack-pintura-interior,Pack de pinturas para interiores,https://example.com/images/pack-main.jpg,https://example.com/images/pack-thumb.jpg,CAT_002,Packs de Pintura,BRD_002,2024-01-15T11:00:00Z
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
    brand,
    created_at
FROM products 
WHERE updated_at > '2024-01-15T00:00:00Z'
ORDER BY created_at;
```
