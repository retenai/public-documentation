# :material-database: Integración por Base de Datos

La integración por base de datos permite a Reten consultar directamente los datos del cliente en tiempo real.

## Descripción General

Este método de integración funciona mediante:

1. **Exposición de BD**: El cliente expone una base de datos con los datos a sincronizar
2. **Conexión segura**: Reten se conecta usando credenciales seguras
3. **Consultas automáticas**: Los datos se consultan automáticamente según la configuración

## Requisitos de Infraestructura

### Base de Datos
- **Tipo**: PostgreSQL, MySQL, SQL Server, Oracle
- **Acceso**: Desde internet con IPs de Reten permitidas

### Seguridad
- **Conexión**: SSL/TLS recomendado
- **Autenticación**: Usuario y contraseña únicos para Reten
- **Permisos**: Solo lectura en tablas específicas

## Estructura de Datos

El cliente debe crear tablas con la estructura especificada en la documentación de cada entidad:

- **Usuarios**: Ver estructura en [Datos Maestros > Usuarios](../../master-data/user/README.md)
- **Productos**: Ver estructura en [Datos Maestros > Productos](../../master-data/product/README.md)
- **Vendedores**: Ver estructura en [Datos Maestros > Vendedores](../../master-data/seller/README.md)
- **Transacciones**: Ver estructura en [Datos Maestros > Transacciones](../../master-data/transactions/README.md)
- **Eventos**: Ver estructura en [Datos Maestros > Eventos](../../master-data/events/README.md)

> 💡 **Ver todas las entidades de datos maestros**: [Datos Maestros](../../master-data/README.md)

### Configuraciones
- **Asignaciones**: Ver estructura en [Configuraciones > Asignaciones](../../settings/assignments/README.md)
- **Rutas**: Ver estructura en [Configuraciones > Rutas](../../settings/routes/README.md)

> 💡 **Ver todas las configuraciones disponibles**: [Configuraciones](../../settings/README.md)

### Campos de Auditoría
Todas las tablas deben incluir los campos de timestamp especificados en cada entidad:

- `created_at`: Timestamp de creación
- `updated_at`: Timestamp de última modificación

### Ejemplo: Tabla de Usuarios

#### CREATE TABLE
```sql
CREATE TABLE users (
    user_id VARCHAR(255) PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    phone VARCHAR(50),
    address TEXT,
    city VARCHAR(100),
    country VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

#### Consulta de Sincronización
```sql
-- Consultar solo usuarios modificados desde la última sincronización
SELECT * FROM users 
WHERE updated_at > :last_sync_timestamp
ORDER BY updated_at ASC;
```

## Configuración de Conexión

### Parámetros de Conexión
```json
{
  "host": "db.cliente.com",
  "port": 5432,
  "database": "reten_integration",
  "username": "reten_user",
  "password": "secure_password"
}
```

## Proceso de Sincronización

### 1. Configuración Inicial
- Crear tablas según la estructura de cada entidad
- Configurar permisos de acceso para Reten
- Proporcionar credenciales de conexión

### 2. Sincronización Automática
- Reten se conecta automáticamente a la base de datos
- Los datos se sincronizan según la configuración establecida
- No se requiere intervención manual del cliente

## Seguridad

- **Acceso restringido**: Solo Reten puede acceder a las tablas de integración
- **Credenciales únicas**: Usuario específico para Reten con permisos limitados
- **Logs de auditoría**: Se registran todas las consultas realizadas

## Próximos Pasos

Para implementar este método de integración:

1. **Preparar infraestructura** de base de datos
2. **Crear tablas** según la estructura de cada entidad
3. **Configurar permisos** de acceso para Reten
4. **Probar conectividad** desde IPs de Reten
5. **Realizar pruebas** con datos de ejemplo
