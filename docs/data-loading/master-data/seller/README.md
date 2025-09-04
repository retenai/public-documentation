# :material-account-tie: Vendedores

Los vendedores son los usuarios del CPG (Consumer Packaged Goods) que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los usuarios, sus roles, permisos y relaciones con comercios.

## Estructura de Datos

```json
{
  // Identificadores
  "seller_id": "string",         // Identificador único interno (not null)
  "route_id": "string",          // Identificador de ruta o código de vendedor (not null)

  // Información básica
  "full_name": "string",         // Nombre completo (not null)
  "first_name": "string",        // Nombre (opcional)
  "last_name": "string",         // Apellido (opcional)
  "status": "string",            // Estado del vendedor (active, inactive, suspended, terminated)
  
  // Información de contacto directa
  "email": "string",             // Email del vendedor
  "phone": "string",             // Teléfono del vendedor
  "channel": "string",           // Canal de venta (salesman, callcenter, supervisor, activator)
  
  // Ubicaciones asignadas
  "location_ids": ["string"],    // Lista de IDs de ubicaciones asignadas al vendedor
  
  // Supervisores
  "supervisor_ids": ["string"],  // Lista de IDs de supervisores del vendedor
  
  // Fechas relevantes (opcional)
  "hire_date": "timestamp",      // Fecha de contratación
  "termination_date": "timestamp", // Fecha de término si aplica

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

| Campo     | Tipo   | Requerido | Descripción                                 |
| --------- | ------ | --------- | ------------------------------------------- |
| seller_id | string | Sí        | Identificador único interno en Reten        |
| route_id  | string | Sí        | Identificador de ruta o código del vendedor |

### Información Básica

| Campo      | Tipo   | Requerido | Descripción                  |
| ---------- | ------ | --------- | ---------------------------- |
| full_name  | string | Sí        | Nombre completo del vendedor |
| first_name | string | No        | Nombre del vendedor          |
| last_name  | string | No        | Apellido del vendedor        |
| status     | string | Sí        | Estado actual del vendedor   |

### Información de Contacto Directa

| Campo   | Tipo   | Requerido | Descripción                 |
| ------- | ------ | --------- | --------------------------- |
| email   | string | Sí        | Email del vendedor          |
| phone   | string | Sí        | Teléfono del vendedor       |
| channel | string | Sí        | Canal de venta del vendedor |

**Tipos de Canal:**
- **`salesman`**: Vendedor tradicional que vende productos
- **`callcenter`**: Vendedor por teléfono
- **`supervisor`**: Supervisor de vendedores
- **`activator`**: Activador que enrola usuarios nuevos

### Ubicaciones y Supervisión

| Campo          | Tipo     | Requerido | Descripción                           |
| -------------- | -------- | --------- | ------------------------------------- |
| location_ids   | [string] | No        | Lista de IDs de ubicaciones asignadas |
| supervisor_ids | [string] | No        | Lista de IDs de supervisores          |

### Fechas (Opcional)

| Campo            | Tipo      | Requerido | Descripción                |
| ---------------- | --------- | --------- | -------------------------- |
| hire_date        | timestamp | No        | Fecha de contratación      |
| termination_date | timestamp | No        | Fecha de término si aplica |

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

**Estados Válidos del Vendedor:**

- `active`: Vendedor activo
- `inactive`: Vendedor inactivo temporalmente
- `suspended`: Cuenta suspendida
- `terminated`: Relación terminada

## Atributos Personalizados

**Importante:** El campo `attributes` **NO es enviado por el cliente**. Reten lo construye automáticamente durante el proceso de carga de datos, extrayendo todas las columnas adicionales que vengan en la base de datos o archivo CSV y que no estén definidas en el modelo estándar de vendedores.

### **Cómo Funciona:**
1. **Cliente envía** datos con columnas adicionales (ej: `territory_code`, `performance_rating`, `training_completed`)
2. **Reten detecta** automáticamente las columnas no mapeadas al modelo
3. **Reten construye** el campo `attributes` con estas columnas adicionales
4. **Se almacena** como array de objetos con `key`, `value` y `type` inferido

### **Casos de Uso Comunes:**
- **Campos específicos del vendedor**: Información particular de cada CPG
- **Metadatos de integración**: Datos del sistema origen que no tienen equivalente en Reten
- **Atributos de negocio**: Campos específicos del canal de venta o empresa
- **Configuraciones personalizadas**: Parámetros únicos del vendedor

### **Formato del Campo (Construido por Reten):**
```json
"attributes": [
  {
    "key": "territory_code",
    "value": "TER_001",
    "type": "string"
  },
  {
    "key": "performance_rating",
    "value": "4.5",
    "type": "number"
  },
  {
    "key": "training_completed",
    "value": "true",
    "type": "boolean"
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

#### Identificadores

- `seller_id` debe ser único en todo el sistema
- `route_id` debe ser único dentro del mismo CPG
- El formato de `route_id` debe seguir el patrón definido por el CPG (ej: "R001", "V123", etc.)

#### Información Personal

- Nombres y apellidos no pueden estar vacíos
- El email principal debe ser corporativo
- El formato de teléfono debe ser válido

### Validaciones de Negocio

#### Información de Contacto

- El email debe tener formato válido
- El teléfono debe tener formato válido
- El canal debe ser uno de los valores permitidos: `salesman`, `callcenter`, `supervisor`, `activator`

#### Ubicaciones y Supervisión

- Los IDs de ubicaciones deben existir en el sistema
- Los IDs de supervisores deben existir y estar activos
- No pueden existir ciclos en la jerarquía de supervisión

#### Fechas (Opcional)

- Si se proporciona `hire_date`, debe ser una fecha válida
- Si se proporciona `termination_date`, debe ser posterior a `hire_date`
- Las fechas deben estar en formato ISO 8601

#### Atributos

- Las claves de atributos deben ser únicas por vendedor
- Los valores deben corresponder al tipo esperado
- El campo `attributes` es construido automáticamente por Reten

## Ejemplos de Uso

### Vendedor Básico

```json
{
  "seller_id": "SELLER_001",
  "route_id": "R001",
  "full_name": "Juan Pérez",
  "status": "active",
  "email": "juan.perez@cpg.com",
  "phone": "+56912345678",
  "channel": "salesman",
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

### Vendedor con Ubicaciones y Supervisores

```json
{
  "seller_id": "SELLER_002",
  "route_id": "V123",
  "full_name": "María González",
  "first_name": "María",
  "last_name": "González",
  "status": "active",
  "email": "maria.gonzalez@cpg.com",
  "phone": "+56987654321",
  "channel": "supervisor",
  "location_ids": ["LOC_001", "LOC_002", "LOC_003"],
  "supervisor_ids": ["SELLER_005"],
  "created_at": "2024-01-01T00:00:00Z",
  "updated_at": "2024-01-01T00:00:00Z",
  "_created_at": "2024-01-01T00:00:00Z",
  "_updated_at": "2024-01-01T00:00:00Z"
}
```

### Vendedor con Estructura Completa

```json
{
  "seller_id": "SELLER_003",
  "route_id": "R003",
  "full_name": "Carlos Rodríguez",
  "first_name": "Carlos",
  "last_name": "Rodríguez",
  "status": "active",
  "email": "carlos.rodriguez@cpg.com",
  "phone": "+56911223344",
  "channel": "callcenter",
  "location_ids": ["LOC_004"],
  "supervisor_ids": ["SELLER_002"],
  "hire_date": "2024-02-01T00:00:00Z",
  "attributes": [
    {
      "key": "specialty",
      "value": "new_business",
      "type": "string"
    },
    {
      "key": "certification_level",
      "value": "advanced",
      "type": "string"
    },
    {
      "key": "territory_code",
      "value": "TER_001",
      "type": "string"
    },
    {
      "key": "performance_rating",
      "value": "4.5",
      "type": "number"
    },
    {
      "key": "training_completed",
      "value": "true",
      "type": "boolean"
    }
  ],
  "created_at": "2024-02-01T00:00:00Z",
  "updated_at": "2024-02-01T00:00:00Z",
  "_created_at": "2024-02-01T00:00:00Z",
  "_updated_at": "2024-02-01T00:00:00Z"
}
```

### Activator (Enrolador de Clientes)

```json
{
  "seller_id": "SELLER_004",
  "route_id": "R004",
  "full_name": "Ana Martínez",
  "status": "active",
  "email": "ana.martinez@cpg.com",
  "phone": "+56999887766",
  "channel": "activator",
  "location_ids": ["LOC_005", "LOC_006"],
  "hire_date": "2024-03-01T00:00:00Z",
  "created_at": "2024-03-01T00:00:00Z",
  "updated_at": "2024-03-01T00:00:00Z",
  "_created_at": "2024-03-01T00:00:00Z",
  "_updated_at": "2024-03-01T00:00:00Z"
}
```

## 🔄 Integración

### **Método por Archivo**
Los vendedores se cargan en archivos CSV con las columnas correspondientes:

```csv
seller_id,route_id,full_name,status,email,phone,channel,attributes,created_at,updated_at,_created_at,_updated_at
SELLER_001,R001,Juan Pérez,active,juan.perez@cpg.com,+56912345678,salesman,"",2024-01-01T00:00:00Z,2024-01-01T00:00:00Z,2024-01-01T00:00:00Z,2024-01-01T00:00:00Z
SELLER_002,V123,María González,active,maria.gonzalez@cpg.com,+56987654321,supervisor,"",2024-01-01T00:00:00Z,2024-01-01T00:00:00Z,2024-01-01T00:00:00Z,2024-01-01T00:00:00Z
```

### **Método por Base de Datos**
Los vendedores se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT
    seller_id,
    route_id,
    full_name,
    status,
    email,
    phone,
    channel,
    attributes,
    created_at,
    updated_at,
    _created_at,
    _updated_at
FROM sellers
WHERE _updated_at > '2024-01-15T00:00:00Z'
ORDER BY _updated_at ASC;
```
