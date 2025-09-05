# :material-store: Usuarios

Los usuarios son los establecimientos, negocios o personas que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los usuarios.

## Estructura de Datos

```json
{
  // Identificadores
  "user_id": "string",           // Identificador principal (not null)
  "miscellaneous_id": "string",  // ID externo adicional del usuario (opcional)

  // Información básica
  "name": "string",              // Nombre del usuario

  // Clasificación y categorización
  "channel": "string",           // Canal del usuario (ej: "ferretero", "retail", "mayorista", "b2b", "b2c")
  "subchannel": "string",        // Subcanal específico (ej: "construccion", "industrial", "tienda", "online")
  "category": "string",          // Nivel de usuario (ej: "oro", "plata", "bronce")
  "subcategory": "string",       // Prioridad (ej: "prioritario", "normal", "bajo")

  // Información de contacto directa
  "email": "string",             // Email principal del usuario
  "phone": "string",             // Teléfono principal del usuario
  "push_token": "string",        // Identificador único para notificaciones push (Firebase token, etc.)
  
  // Ubicación
  "country": "string",           // País del usuario
  
  // Dirección principal (opcional)
  "address": "string",           // Dirección completa como string
  
  // Dirección desglosada (opcional)
  "address_street": "string",    // Calle
  "address_number": "string",    // Número
  "address_commune": "string",   // Comuna
  "address_city": "string",      // Ciudad
  "address_region": "string",    // Región
  "address_dept": "string",      // Departamento/oficina
  
  // Fechas relevantes
  "signup_at": "timestamp",      // Fecha de registro (not null)
  "setup_at": "timestamp",       // Fecha de configuración final
  
  // Marcas temporales
  "created_at": "timestamp",     // Fecha de creación en sistema usuario (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema usuario

  // Atributos personalizados - Cualquier columna no definida en el modelo se almacenará aquí
  "attributes": [
    {
      "key": "string",        // Nombre del atributo personalizado
      "value": "string",      // Valor del atributo
      "type": "string"        // Tipo de dato: string, number, date, boolean
    }
  ],

  // Marcas temporales de sincronización con Reten
  "_created_at": "timestamp",           // Fecha de creación del registro en Método de Conexión con Reten
  "_updated_at": "timestamp"            // Fecha de última actualización del registro en Método de Conexión con Reten
}
```

## Campos Detallados

### Identificadores

| Campo            | Tipo   | Requerido | Descripción                      |
| ---------------- | ------ | --------- | -------------------------------- |
| user_id          | string | Sí        | Identificador único en Reten     |
| miscellaneous_id | string | No        | ID externo adicional del usuario |

### Información Básica

| Campo   | Tipo   | Requerido | Descripción                 |
| ------- | ------ | --------- | --------------------------- |
| name    | string | Sí        | Nombre del usuario          |
| country | string | No        | País donde opera el usuario |

### Clasificación y Categorización

| Campo       | Tipo   | Requerido | Descripción                                                                |
| ----------- | ------ | --------- | -------------------------------------------------------------------------- |
| channel     | string | No        | Canal del usuario (ej: "ferretero", "retail", "mayorista", "b2b", "b2c")   |
| subchannel  | string | No        | Subcanal específico (ej: "construccion", "industrial", "tienda", "online") |
| category    | string | No        | Categoría de usuario (ej: "oro", "plata", "bronce")                        |
| subcategory | string | No        | Subcategoría de usuario (ej: "prioritario", "normal", "bajo")              |

### Información de Contacto Directa

| Campo      | Tipo   | Requerido | Descripción                                                         |
| ---------- | ------ | --------- | ------------------------------------------------------------------- |
| email      | string | Sí        | Email principal del usuario                                         |
| phone      | string | Sí        | Teléfono principal del usuario                                      |
| push_token | string | No        | Identificador único para notificaciones push (Firebase token, etc.) |

### Dirección Principal

| Campo           | Tipo   | Requerido | Descripción                    |
| --------------- | ------ | --------- | ------------------------------ |
| address         | string | No        | Dirección completa como string |
| address_street  | string | No        | Calle                          |
| address_number  | string | No        | Número                         |
| address_commune | string | No        | Comuna                         |
| address_city    | string | No        | Ciudad                         |
| address_region  | string | No        | Región                         |
| address_dept    | string | No        | Departamento/oficina           |

### Fechas Relevantes

| Campo     | Tipo      | Requerido | Descripción                  |
| --------- | --------- | --------- | ---------------------------- |
| signup_at | timestamp | Sí        | Fecha de registro inicial    |
| setup_at  | timestamp | No        | Fecha de configuración final |

### Marcas Temporales

| Campo      | Tipo      | Requerido | Descripción                             |
| ---------- | --------- | --------- | --------------------------------------- |
| created_at | timestamp | Sí        | Fecha de creación en sistema usuario    |
| updated_at | timestamp | No        | Última actualización en sistema usuario |

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el usuario**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan en la base de datos o archivo CSV y que no estén definidas en el modelo estándar de usuarios.

### **Cómo Funciona:**
1. **Usuario envía** datos con columnas adicionales (ej: `segment_id`, `credit_score`, `business_type`)
2. **Reten detecta** automáticamente las columnas no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas columnas adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos del usuario**: Información particular de cada negocio
- **Metadatos de integración**: Datos del sistema origen que no tienen equivalente en Reten
- **Atributos de negocio**: Campos específicos de la industria o empresa
- **Configuraciones personalizadas**: Parámetros únicos del usuario

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "segment_id",
    "value": "SEG_001",
    "type": "string"
  },
  {
    "key": "credit_score",
    "value": "850",
    "type": "number"
  },
  {
    "key": "business_type",
    "value": "retail",
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
- **Procesamiento automático** sin intervención del usuario

## Validaciones

### Validaciones Generales

### Identificadores

- `user_id` debe ser único en todo el sistema

### Fechas

- `created_at` no puede ser posterior a `updated_at`
- `signup_at` no puede ser posterior a `setup_at`
- Todas las fechas deben ser válidas y en formato ISO 8601

### Información de Contacto

- `email` y `phone` son campos requeridos en raíz
- Los emails deben tener formato válido
- Los teléfonos deben tener formato válido según el país

### Dirección Principal

- Al menos uno de los campos de dirección debe estar presente (`address` o combinación de campos desglosados)
- Si se usan campos desglosados, `address_city` es recomendado para ubicación básica

### Clasificación y Categorización

- Los campos `channel`, `subchannel`, `category` y `subcategory` son opcionales pero recomendados para análisis
- Si se proporciona `subchannel`, se recomienda tener también `channel` para consistencia
- Si se proporciona `subcategory`, se recomienda tener también `category` para jerarquía lógica
- Los valores deben ser descriptivos y consistentes para análisis posterior

### Validaciones de Negocio

### Configuración Inicial

- `setup_at` solo se establece cuando se completan todos los campos requeridos
- La configuración requiere al menos `email`, `phone` y un campo de dirección válido

### Atributos

- Las claves de atributos deben ser únicas por usuario
- Los valores deben corresponder al tipo esperado
- El campo `attributes` es construido automáticamente por Reten

## Ejemplos de Uso

### Usuario Persona Natural

```json
{
  "user_id": "CLI_001",
  "miscellaneous_id": "EXT_001",
  "name": "José Pérez",
  "channel": "b2c",
  "subchannel": "online",
  "category": "oro",
  "subcategory": "prioritario",
  "email": "jose.perez@email.com",
  "phone": "+56912345678",
  "push_token": "fcm_token_abc123",
  "country": "CL",
  "address": "Los Alerces 123, Santiago, Región Metropolitana",
  "signup_at": "2024-03-19T10:00:00Z",
  "created_at": "2024-03-19T10:00:00Z",
  "updated_at": "2024-03-19T10:00:00Z",
  "attributes": [
    {
      "key": "customer_type",
      "value": "individual",
      "type": "string"
    }
  ],
  "_created_at": "2024-03-19T10:00:00Z",
  "_updated_at": "2024-03-19T10:00:00Z"
}
```

### Usuario Empresa

```json
{
  "user_id": "CLI_002",
  "miscellaneous_id": "ERP_456",
  "name": "Supermercados El Sol",
  "channel": "retail",
  "subchannel": "tienda",
  "category": "plata",
  "subcategory": "normal",
  "email": "gerencia@elsol.cl",
  "phone": "+56922334455",
  "push_token": "fcm_business_xyz789",
  "country": "CL",
  "address_street": "Av. Providencia",
  "address_number": "1234",
  "address_commune": "Providencia",
  "address_city": "Santiago",
  "address_region": "Metropolitana",
  "signup_at": "2024-01-15T09:00:00Z",
  "setup_at": "2024-01-20T16:00:00Z",
  "created_at": "2024-01-15T09:00:00Z",
  "updated_at": "2024-01-20T16:00:00Z",
  "attributes": [
    {
      "key": "business_type",
      "value": "retail",
      "type": "string"
    },
    {
      "key": "segment_id",
      "value": "SEG_001",
      "type": "string"
    },
    {
      "key": "credit_score",
      "value": "850",
      "type": "number"
    }
  ],
  "_created_at": "2024-01-15T09:00:00Z",
  "_updated_at": "2024-01-20T16:00:00Z"
}
```

### Usuario con Atributos Personalizados

```json
{
  "user_id": "CLI_003",
  "miscellaneous_id": "CRM_789",
  "name": "Distribuidora Mayor",
  "channel": "mayorista",
  "subchannel": "industrial",
  "category": "oro",
  "subcategory": "prioritario",
  "email": "contacto@distribuidora.cl",
  "phone": "+56933445566",
  "push_token": "fcm_enterprise_def456",
  "country": "CL",
  "address": "Camino Industrial 567, Maipú, Región Metropolitana",
  "signup_at": "2024-02-10T08:00:00Z",
  "setup_at": "2024-02-15T14:00:00Z",
  "created_at": "2024-02-10T08:00:00Z",
  "updated_at": "2024-02-15T14:00:00Z",
  "attributes": [
    {
      "key": "zone",
      "value": "north",
      "type": "string"
    },
    {
      "key": "manager_id",
      "value": "MANAGER_001",
      "type": "string"
    },
    {
      "key": "loyalty_level",
      "value": "gold",
      "type": "string"
    },
    {
      "key": "last_purchase_date",
      "value": "2024-03-15",
      "type": "date"
    },
    {
      "key": "is_active",
      "value": "true",
      "type": "boolean"
    }
  ],
  "_created_at": "2024-02-10T08:00:00Z",
  "_updated_at": "2024-02-15T14:00:00Z"
}
```

## 🔄 Integración

### **Método por Archivo**
Los usuarios se cargan en archivos CSV con las columnas correspondientes:

```csv
user_id,miscellaneous_id,name,channel,subchannel,category,subcategory,email,phone,push_token,country,address,signup_at,setup_at,created_at,updated_at,attributes,_created_at,_updated_at
CLI_001,EXT_001,Restaurante La Pasta,b2c,online,oro,prioritario,contacto@lapasta.cl,+56911223344,fcm_restaurant_abc123,CL,"Los Alerces 123, Santiago, Región Metropolitana",2024-03-19T10:00:00Z,,2024-03-19T10:00:00Z,2024-03-19T10:00:00Z,"",2024-03-19T10:00:00Z,2024-03-19T10:00:00Z
CLI_002,ERP_456,Supermercados El Sol,retail,tienda,plata,normal,gerencia@elsol.cl,+56922334455,fcm_business_xyz789,CL,Av. Providencia 1234,2024-01-15T09:00:00Z,2024-01-20T16:00:00Z,2024-01-15T09:00:00Z,2024-01-20T16:00:00Z,"",2024-01-15T09:00:00Z,2024-01-20T16:00:00Z
```

### **Método por Base de Datos**
Los usuarios se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT
    user_id,
    miscellaneous_id,
    name,
    channel,
    subchannel,
    category,
    subcategory,
    email,
    phone,
    push_token,
    country,
    address,
    signup_at,
    setup_at,
    created_at,
    updated_at,
    attributes,
    _created_at,
    _updated_at
FROM users
WHERE _updated_at > '2024-01-15T00:00:00Z'
ORDER BY _updated_at ASC;
```
