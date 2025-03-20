# Categorías

Las categorías permiten organizar y clasificar productos de manera jerárquica en la plataforma Reten. Esta entidad maneja la estructura de navegación, filtrado y agrupación de productos.

## Estructura de Datos

```json
{
  // Identificadores
  "category_id": "string",      // Identificador único de la categoría (not null)
  "parent_id": "string",        // ID de la categoría padre (null si es raíz)
  "data_object_id": "string",   // ID del objeto de datos (not null)

  // Información básica
  "name": {
    "display_name": "string",   // Nombre para mostrar (not null)
    "slug": "string",          // URL amigable (not null)
    "description": "string"    // Descripción de la categoría
  },

  // Metadatos
  "metadata": {
    "level": "number",         // Nivel en la jerarquía (0 para raíz)
    "path": ["string"],        // Array de IDs desde la raíz hasta esta categoría
    "is_leaf": "boolean",      // Indica si es una categoría final
    "has_products": "boolean", // Indica si tiene productos asociados
    "position": "number",      // Posición para ordenamiento manual
    "tags": ["string"]         // Tags para búsqueda alternativa
  },

  // SEO
  "seo": {
    "title": "string",         // Título para SEO
    "description": "string",   // Descripción para SEO
    "keywords": ["string"],    // Palabras clave
    "canonical_url": "string"  // URL canónica
  },

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp",   // Última actualización (not null)
  "_created_at": "timestamp",  // Fecha de creación en BigQuery (not null)
  "_updated_at": "timestamp"   // Última actualización en BigQuery (not null)
}
```

## Campos Detallados

### Identificadores

| Campo          | Tipo   | Requerido | Descripción              |
| -------------- | ------ | --------- | ------------------------ |
| category_id    | string | Sí        | Identificador único      |
| parent_id      | string | No        | ID de la categoría padre |
| data_object_id | string | Sí        | ID del objeto de datos   |

### Nombre

| Campo        | Tipo   | Requerido | Descripción                 |
| ------------ | ------ | --------- | --------------------------- |
| display_name | string | Sí        | Nombre para mostrar         |
| slug         | string | Sí        | URL amigable                |
| description  | string | No        | Descripción de la categoría |

### Metadatos

| Campo        | Tipo    | Requerido | Descripción                    |
| ------------ | ------- | --------- | ------------------------------ |
| level        | number  | Sí        | Nivel en la jerarquía          |
| path         | array   | Sí        | Ruta desde la raíz             |
| is_leaf      | boolean | Sí        | Es categoría final             |
| has_products | boolean | Sí        | Tiene productos                |
| position     | number  | No        | Posición de ordenamiento       |
| tags         | array   | No        | Tags para búsqueda alternativa |

## Validaciones

### Validaciones Generales

### Identificadores

- `category_id` debe ser único
- `parent_id` debe existir si no es categoría raíz
- `slug` debe ser único dentro del mismo nivel

### Jerarquía

- No se permiten ciclos en la estructura
- Máximo 5 niveles de profundidad
- Una categoría no puede ser su propio ancestro

### Metadatos

- `level` debe ser consistente con la posición en la jerarquía
- `path` debe incluir todos los ancestros en orden
- `position` debe ser único dentro del mismo nivel y padre

### Validaciones de Negocio

### Visibilidad

- Categorías padres visibles si tienen hijos visibles
- Categorías con productos deben ser visibles
- Categorías ocultas no aparecen en navegación

### SEO

- URLs amigables sin caracteres especiales
- Títulos y descripciones con longitud adecuada
- Keywords relevantes y no duplicados

### Tags

- Sin caracteres especiales
- Longitud máxima de 50 caracteres por tag
- Máximo 20 tags por categoría
- No duplicados dentro de la misma categoría

## Ejemplos

### Categoría Raíz

```json
{
  "category_id": "CAT_001",
  "parent_id": null,
  "name": {
    "display_name": "Pinturas",
    "slug": "pinturas",
    "description": "Todo tipo de pinturas y recubrimientos"
  },
  "metadata": {
    "level": 0,
    "path": ["CAT_001"],
    "is_leaf": false,
    "has_products": false,
    "position": 1,
    "tags": ["pintura", "recubrimiento", "acabados", "decoración"]
  },
  "seo": {
    "title": "Pinturas y Recubrimientos | Reten",
    "description": "Encuentra todo tipo de pinturas y recubrimientos para tus proyectos",
    "keywords": ["pinturas", "recubrimientos", "pintura interior", "pintura exterior"]
  }
}
```

### Categoría Hija

```json
{
  "category_id": "CAT_002",
  "parent_id": "CAT_001",
  "name": {
    "display_name": "Pinturas Látex",
    "slug": "pinturas-latex",
    "description": "Pinturas látex para interiores y exteriores"
  },
  "metadata": {
    "level": 1,
    "path": ["CAT_001", "CAT_002"],
    "is_leaf": true,
    "has_products": true,
    "position": 1,
    "tags": ["latex", "pintura agua", "lavable", "mate", "satinado"]
  },
  "seo": {
    "title": "Pinturas Látex Interior y Exterior | Reten",
    "description": "Amplia selección de pinturas látex para interiores y exteriores",
    "keywords": ["pintura látex", "látex interior", "látex exterior"]
  }
}
```

## Notas de Implementación

### Gestión de Jerarquía

### Actualización de Metadatos

- Recalcular `level` y `path` al mover categorías
- Actualizar `is_leaf` al agregar/eliminar hijos
- Mantener consistencia de `has_products`

### Ordenamiento

- Por `position` dentro del mismo nivel
- Por `display_name` si no hay posición
- Considerar ordenamiento personalizado por comercio

### Optimización

### Caché

- Árbol de categorías completo
- Rutas de navegación frecuentes
- Conteos de productos

### Consultas

- Índices por `parent_id` y `path`
- Materializar rutas completas
- Precalcular breadcrumbs

## Integración con Otros Sistemas

### APIs

### Endpoints Principales

```
GET    /api/v1/categories
GET    /api/v1/categories/{category_id}
POST   /api/v1/categories
PUT    /api/v1/categories/{category_id}
DELETE /api/v1/categories/{category_id}
```

### Endpoints de Jerarquía

```
GET    /api/v1/categories/{category_id}/children
GET    /api/v1/categories/{category_id}/ancestors
POST   /api/v1/categories/{category_id}/move
```

### Webhooks

### Eventos Disponibles

- `category.created`
- `category.updated`
- `category.deleted`
- `category.moved`

### Formato de Payload

```json
{
  "event": "category.moved",
  "timestamp": "2024-03-19T14:30:00Z",
  "data": {
    "category_id": "string",
    "old_parent_id": "string",
    "new_parent_id": "string",
    "old_path": ["string"],
    "new_path": ["string"]
  }
}
```

## Preguntas Frecuentes

1. **¿Cómo manejar la eliminación de categorías?**
   - Verificar productos asociados
   - Opción de mover productos
   - Mantener historial para URLs

2. **¿Cómo gestionar URLs amigables?**
   - Generación automática desde nombre
   - Validación de unicidad
   - Redirecciones 301 para cambios

3. **¿Cómo optimizar la navegación?**
   - Precarga de rutas comunes
   - Caché de árbol de categorías
   - Paginación de productos 