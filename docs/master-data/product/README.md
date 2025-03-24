# Productos

Los productos representan los artículos comercializados a través de la plataforma Reten. Esta entidad almacena toda la información relevante sobre los productos, sus características, categorización y relaciones comerciales.

## Estructura de Datos

```json
{
  // Identificadores
  "product_id": "string",        // Identificador único del producto (not null)
  "product_identifiers": {
    "sku": "string",            // Stock Keeping Unit, código único de producto para gestión de inventario (not null)
    "gtin": "string"            // Global Trade Item Number (código de barras)
  },

  // Información básica
  "name": {
    "display_name": "string",    // Nombre comercial del producto (not null)
    "short_name": "string",      // Nombre corto para listados
    "slug": "string",           // Versión URL-friendly del nombre
    "short_description": "string", // Descripción corta del producto
    "description": "string"      // Descripción detallada del producto
  },

  // Imágenes
  "images": {
    "main_image_url": "string",
    "thumbnail_url": "string",
    "additional_images": [{
      "url": "string",
      "description": "string",
      "type": "string",
      "position": "number",
      "alt_text": "string"
    }]
  },

  // Categorización
  "categories": [{
    "category_id": "string",     // Referencia al ID de la categoría
    "is_primary": "boolean",     // Indica si es la categoría principal del producto
    "attributes": [{            // Atributos específicos de la relación producto-categoría
      "key": "string",
      "value": "string",
      "type": "string"
    }]
  }],

  // Información de marca
  "brand": "string",       // Identificador de la marca

  // Tags para búsqueda y filtrado
  "tags": ["string"],     // Lista de etiquetas para búsqueda y categorización adicional

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

### Imágenes

| Campo             | Tipo   | Descripción                       |
| ----------------- | ------ | --------------------------------- |
| main_image_url    | string | URL de la imagen principal        |
| thumbnail_url     | string | URL de la miniatura               |
| additional_images | array  | Colección de imágenes adicionales |

**Estructura de Imágenes Adicionales:**

- `url`: URL de la imagen
- `description`: Descripción de la imagen
- `type`: Tipo de imagen (producto, lifestyle, etc.)
- `position`: Orden de visualización
- `alt_text`: Texto alternativo para accesibilidad

### Categorización

| Campo       | Tipo    | Descripción                                             |
| ----------- | ------- | ------------------------------------------------------- |
| category_id | string  | Referencia al ID de la categoría                        |
| is_primary  | boolean | Indica si es la categoría principal                     |
| attributes  | array   | Atributos específicos de la relación producto-categoría |

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

- `name` no puede estar vacío
- `slug` debe ser único y URL-friendly
- Descripciones no deben exceder límites de caracteres

### Imágenes

- URLs deben ser válidas y accesibles
- Posiciones deben ser únicas
- Tipos de imagen deben ser válidos

### Validaciones de Negocio

### Composición

- Si `is_pack` es true, `pack_details` debe estar completo
- Si `is_combo` es true, `combo_details` debe estar completo
- Componentes deben existir en el catálogo

### Categorización

- Categorías deben existir en el maestro
- Tags deben ser válidos
- Marca debe existir en el maestro

## Ejemplos de Uso

### Producto Simple

```json
{
  "product_id": "PROD_001",
  "product_identifiers": {
    "sku": "SKU123",
    "gtin": null
  },
  "name": {
    "display_name": "Pintura Látex Premium",
    "short_name": "Látex Premium",
    "slug": "pintura-latex-premium",
    "short_description": "Pintura látex de alta calidad"
  },
  "images": {
    "main_image_url": "https://example.com/images/main.jpg",
    "thumbnail_url": "https://example.com/images/thumb.jpg"
  },
  "categories": [{
    "category_id": "CAT_001",
    "is_primary": true,
    "attributes": [{
      "key": "coverage",
      "value": "12",
      "type": "numeric"
    }, {
      "key": "finish",
      "value": "matte",
      "type": "string"
    }]
  }],
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
  "product_identifiers": {
    "sku": "PACK456",
    "gtin": null
  },
  "name": {
    "display_name": "Pack Pintura Interior",
    "short_name": "Pack Pintura",
    "slug": "pack-pintura-interior",
    "short_description": "Pack de pinturas para interiores"
  },
  "composition": {
    "is_pack": true,
    "pack_details": {
      "units_per_pack": 3,
      "unit_type": "galón",
      "unit_size": 1,
      "unit_measure": "galón"
    }
  },
  "categories": [{
    "category_id": "CAT_002",
    "is_primary": true,
    "attributes": [{
      "key": "color_family",
      "value": "neutrals",
      "type": "string"
    }, {
      "key": "recommended_use",
      "value": "living_spaces",
      "type": "string"
    }]
  }],
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