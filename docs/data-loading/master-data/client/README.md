# :material-store: Clientes

Los clientes son los establecimientos, negocios o personas que interactúan con la plataforma Reten. Esta entidad almacena toda la información relevante sobre los clientes.

## Estructura de Datos

```json
{
  // Identificadores
  "client_id": "string",           // Identificador principal (not null)

  // Información básica
  "name": "string",              // Nombre del cliente
  
  // Información de contacto directa
  "email": "string",             // Email principal del cliente
  "phone": "string",             // Teléfono principal del cliente
  
  // Ubicación
  "country": "string",           // País del cliente
  
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
  "created_at": "timestamp",     // Fecha de creación en sistema cliente (not null)
  "updated_at": "timestamp",     // Fecha de última actualización en sistema cliente

  // Información de contacto estructurada (opcional)
  "contacts": [{
    "email": "string",           // Correo electrónico
    "phone": "string",           // Número telefónico
    "type": "string",            // primary, secondary, billing, etc.
    "personal_info": {           // Información personal del contacto
      "first_name": "string",    // Nombre
      "last_name": "string",     // Apellido
      "country": "string",       // País de residencia
      "date_of_birth": "timestamp", // Fecha de nacimiento
      "role": "string"           // Rol en la organización (CEO, gerente, dueño, etc.)
    }
  }],

  // Direcciones adicionales (opcional)
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
    "value": "string",
    "type": "string"    // Tipo de valor (string, number, date, boolean)
  }],

  // Grupos y clasificaciones
  "groups": [{
    "group_type": "string",
    "attributes": [{
      "key": "string",
      "value": "string",
      "type": "string"    // Tipo de valor (string, number, date, boolean)
    }]
  }],

  // IDs adicionales
  "miscellaneous_id": [{
    "platform": "string",      // Plataforma relacionada
    "misc_id": "string"       // ID en esa plataforma
  }]
}
```

## Campos Detallados

### Identificadores

| Campo     | Tipo   | Requerido | Descripción                  |
| --------- | ------ | --------- | ---------------------------- |
| client_id | string | Sí        | Identificador único en Reten |

### Información Básica

| Campo   | Tipo   | Requerido | Descripción                 |
| ------- | ------ | --------- | --------------------------- |
| name    | string | Sí        | Nombre del cliente          |
| country | string | No        | País donde opera el cliente |

### Información de Contacto Directa

| Campo | Tipo   | Requerido | Descripción                    |
| ----- | ------ | --------- | ------------------------------ |
| email | string | Sí        | Email principal del cliente    |
| phone | string | Sí        | Teléfono principal del cliente |

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
| created_at | timestamp | Sí        | Fecha de creación en sistema cliente    |
| updated_at | timestamp | No        | Última actualización en sistema cliente |

### Contactos (Opcional)

Los contactos se manejan como un array opcional para permitir múltiples personas relacionadas con el cliente. Normalmente se usa cuando el cliente es un comercio o empresa con varias personas como punto de contacto. Para clientes que son personas naturales, este campo puede omitirse ya que la información principal está en los campos `email` y `phone` de raíz.

| Campo         | Tipo   | Descripción                               |
| ------------- | ------ | ----------------------------------------- |
| email         | string | Dirección de correo electrónico           |
| phone         | string | Número telefónico                         |
| type          | string | Tipo de contacto (primary, billing, etc.) |
| personal_info | object | Información personal del contacto         |

**Información Personal del Contacto:**

| Campo         | Tipo      | Descripción            |
| ------------- | --------- | ---------------------- |
| first_name    | string    | Nombre del contacto    |
| last_name     | string    | Apellido del contacto  |
| country       | string    | País de residencia     |
| date_of_birth | timestamp | Fecha de nacimiento    |
| role          | string    | Rol en la organización |

**Tipos de Contacto Válidos:**

- `primary`: Contacto principal (requerido)
- `billing`: Contacto de facturación
- `technical`: Contacto técnico
- `marketing`: Contacto de marketing
- `support`: Contacto de soporte

**Roles Comunes:**

- `owner`: Dueño o propietario
- `ceo`: Director ejecutivo
- `manager`: Gerente
- `administrator`: Administrador
- `employee`: Empleado
- `representative`: Representante legal

### Direcciones Adicionales (Opcional)

Las direcciones adicionales se manejan como un array opcional para permitir múltiples ubicaciones. Normalmente se usa para negocios con varias sucursales o direcciones de atención, oficina y facturación separadas. Para clientes simples, la información principal está en los campos de dirección de raíz.

| Campo       | Tipo    | Descripción                         |
| ----------- | ------- | ----------------------------------- |
| type        | string  | Tipo de dirección                   |
| name        | string  | Nombre identificativo               |
| street      | string  | Nombre de la calle                  |
| number      | string  | Número exterior                     |
| dept_number | string  | Número interior/departamento        |
| commune     | string  | Comuna o municipio                  |
| city        | string  | Ciudad                              |
| region      | string  | Región o estado                     |
| lat         | string  | Latitud                             |
| long        | string  | Longitud                            |
| is_default  | boolean | Indica si es la dirección principal |

**Tipos de Dirección Válidos:**

- `billing`: Dirección de facturación
- `shipping`: Dirección de envío
- `warehouse`: Almacén o bodega
- `office`: Oficina
- `store`: Tienda física

### Grupos y Clasificaciones

Los grupos permiten categorizar y segmentar clientes:

```json
{
  "groups": [{
    "group_type": "channel",
    "attributes": [{
      "key": "category",
      "value": "retail"
    }, {
      "key": "sub_category",
      "value": "convenience_store"
    }]
  }, {
    "group_type": "segment",
    "attributes": [{
      "key": "size",
      "value": "medium"
    }, {
      "key": "potential",
      "value": "high"
    }]
  }]
}
```

**Tipos de Grupo Comunes:**

- `channel`: Canal de ventas
- `segment`: Segmentación de mercado
- `territory`: División territorial
- `portfolio`: Portafolio de productos

## Validaciones

### Validaciones Generales

### Identificadores

- `client_id` debe ser único en todo el sistema

### Fechas

- `created_at` no puede ser posterior a `updated_at`
- `signup_at` no puede ser posterior a `setup_at`
- Todas las fechas deben ser válidas y en formato ISO 8601

### Información de Contacto

- `email` y `phone` son campos requeridos en raíz
- Los emails deben tener formato válido
- Los teléfonos deben tener formato válido según el país

### Contactos (Opcional)

- Si se proporciona `contacts`, debe existir al menos un contacto de tipo `primary`
- Los emails deben ser únicos dentro del array de contactos
- Los emails deben tener formato válido
- Los teléfonos deben tener formato válido según el país

### Dirección Principal

- Al menos uno de los campos de dirección debe estar presente (`address` o combinación de campos desglosados)
- Si se usan campos desglosados, `address_city` es recomendado para ubicación básica

### Direcciones Adicionales (Opcional)

- Si se proporciona `addresses`, debe existir al menos una dirección marcada como `is_default: true`
- Las coordenadas geográficas deben ser válidas si se proporcionan
- Los tipos de dirección deben ser valores predefinidos

### Validaciones de Negocio

### Configuración Inicial

- `setup_at` solo se establece cuando se completan todos los campos requeridos
- La configuración requiere al menos `email`, `phone` y un campo de dirección válido
- Los campos `contacts` y `addresses` son opcionales y no afectan la configuración inicial

### Grupos

- Los `group_type` deben corresponder a tipos predefinidos
- Los atributos deben ser válidos para el tipo de grupo

### Atributos

- Las claves de atributos deben ser únicas por cliente
- Los valores deben corresponder al tipo esperado

## Ejemplos de Uso

### Cliente Persona Natural

```json
{
  "client_id": "COM_001",
  "name": "José Pérez",
  "email": "jose.perez@email.com",
  "phone": "+56912345678",
  "country": "CL",
  "address": "Los Alerces 123, Santiago, Región Metropolitana",
  "signup_at": "2024-03-19T10:00:00Z",
  "created_at": "2024-03-19T10:00:00Z"
}
```

### Cliente Empresa con Múltiples Contactos

```json
{
  "client_id": "COM_002",
  "name": "Supermercados El Sol",
  "email": "gerencia@elsol.cl",
  "phone": "+56922334455",
  "country": "CL",
  "address_street": "Av. Providencia",
  "address_number": "1234",
  "address_commune": "Providencia",
  "address_city": "Santiago",
  "address_region": "Metropolitana",
  "signup_at": "2024-01-15T09:00:00Z",
  "setup_at": "2024-01-20T16:00:00Z",
  "created_at": "2024-01-15T09:00:00Z",
  "contacts": [{
    "type": "primary",
    "email": "gerencia@elsol.cl",
    "phone": "+56922334455",
    "personal_info": {
      "first_name": "María",
      "last_name": "González",
      "country": "CL",
      "role": "ceo"
    }
  }, {
    "type": "billing",
    "email": "finanzas@elsol.cl",
    "phone": "+56922334456",
    "personal_info": {
      "first_name": "Juan",
      "last_name": "Silva",
      "country": "CL",
      "role": "administrator"
    }
  }, {
    "type": "technical",
    "email": "sistemas@elsol.cl",
    "phone": "+56922334457",
    "personal_info": {
      "first_name": "Pedro",
      "last_name": "Rojas",
      "country": "CL",
      "role": "manager"
    }
  }],
  "addresses": [{
    "type": "warehouse",
    "name": "Bodega Central",
    "street": "Camino Industrial",
    "number": "567",
    "city": "Maipú",
    "region": "Metropolitana",
    "is_default": false
  }]
}
```

### Cliente con Clasificaciones Complejas

```json
{
  "client_id": "COM_003",
  "name": "Distribuidora Mayor",
  "groups": [{
    "group_type": "channel",
    "attributes": [{
      "key": "category",
      "value": "wholesale"
    }]
  }, {
    "group_type": "segment",
    "attributes": [{
      "key": "size",
      "value": "large"
    }, {
      "key": "credit_score",
      "value": "A+"
    }]
  }, {
    "group_type": "territory",
    "attributes": [{
      "key": "zone",
      "value": "north"
    }, {
      "key": "manager",
      "value": "MANAGER_001"
    }]
  }]
}
```

## 🔄 Integración

### **Método por Archivo**
Los clientes se cargan en archivos CSV con las columnas correspondientes:

```csv
client_id,name,email,phone,country,address,signup_at,setup_at,created_at
CLI_001,Restaurante La Pasta,contacto@lapasta.cl,+56911223344,CL,"Los Alerces 123, Santiago, Región Metropolitana",2024-03-19T10:00:00Z,,2024-03-19T10:00:00Z
COM_002,Supermercados El Sol,gerencia@elsol.cl,+56922334455,CL,Av. Providencia 1234,2024-01-15T09:00:00Z,2024-01-20T16:00:00Z,2024-01-15T09:00:00Z
```

### **Método por Base de Datos**
Los clientes se consultan desde una tabla con la estructura correspondiente:

```sql
SELECT 
    client_id,
    name,
    email,
    phone,
    country,
    address,
    signup_at,
    setup_at,
    created_at
FROM clients 
WHERE updated_at > '2024-01-15T00:00:00Z'
ORDER BY created_at;
```
