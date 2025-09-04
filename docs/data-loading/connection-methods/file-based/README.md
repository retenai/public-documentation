# :material-folder: Integración por Archivo

La integración por archivo permite sincronizar datos con Reten mediante la carga de archivos CSV en un bucket compartido.

## Descripción General

Este método de integración funciona mediante:

1. **Preparación de archivos**: El cliente genera archivos CSV con los datos a sincronizar
2. **Carga al bucket**: Los archivos se suben al bucket proporcionado por Reten
3. **Procesamiento automático**: Reten detecta nuevos archivos y los procesa automáticamente

## Estructura de Archivos

### Formato de Archivo
- **Formato**: CSV
- **Codificación**: UTF-8
- **Separador**: Punto y coma (;)
- **Primera fila**: Encabezados de columnas

### Nomenclatura
```
{entity_type}_{timestamp}.csv
```

**Formato de timestamp**: `YYYYMMDD_HHMMSS` (año, mes, día, hora, minuto, segundo)

**Ejemplos**:
- `users_20241201_143022.csv`
- `products_20241201_143025.csv`
- `assignments_20241201_143030.csv`
- `routes_20241201_143032.csv`

> 💡 **Beneficios del timestamp**: Facilita la trazabilidad, ordenamiento cronológico y auditoría de archivos sincronizados.

## Configuración del Bucket

Reten proporciona un bucket compartido para cada cliente:

- **Bucket**: `reten-integration-{client_id}`
- **Acceso**: Credenciales proporcionadas por Reten
- **Permisos**: Escritura para el cliente, lectura para Reten

### Estructura de Carpetas
```
reten-integration-{client_id}/
├── users/
├── products/
├── sellers/
├── transactions/
├── assignments/
└── routes/
```

## Estructura de Datos

Los archivos CSV deben contener las columnas especificadas en la documentación de cada entidad:

### Datos Maestros
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

## Proceso de Sincronización

### 1. Preparación de Datos
Generar archivo CSV con los datos según la estructura de cada entidad.

### 2. Carga del Archivo
- Subir archivo CSV al bucket en la carpeta correspondiente
- El archivo será procesado automáticamente por Reten

## Seguridad

- **Acceso restringido**: Solo el cliente y Reten tienen acceso al bucket
- **Credenciales únicas**: Cada cliente recibe credenciales específicas
- **Logs de auditoría**: Se registran todas las operaciones de archivo

## Próximos Pasos

Para implementar este método de integración:

1. **Solicitar credenciales** del bucket al equipo de Reten
2. **Generar archivos CSV** según la estructura de cada entidad
3. **Probar con datos de ejemplo** antes de la implementación completa
