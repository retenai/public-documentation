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
{entity_type}.csv
```

**Ejemplos**:
- `users.csv`
- `products.csv`
- `assignments.csv`
- `routes.csv`

#### Particionado de Archivos Grandes
Para archivos muy grandes que requieran particionado, usar índices numéricos:

```
{entity_type}_{index}.csv
```

**Ejemplos de archivos particionados**:
- `users_1.csv`
- `users_2.csv`
- `users_3.csv`
- `products_1.csv`
- `products_2.csv`

> 💡 **Procesamiento**: Reten procesará automáticamente todos los archivos de la misma entidad que se encuentren en la carpeta del día correspondiente.

> 💡 **Organización temporal**: La fecha se maneja a través de la estructura de carpetas, permitiendo un archivo único por entidad por día que se reemplaza en caso de actualizaciones.

## Configuración del Bucket

Reten proporciona un bucket compartido para cada cliente:

- **Bucket**: `reten-{client_id}`
- **Acceso**: Credenciales proporcionadas por Reten
- **Permisos**: Escritura para el cliente, lectura para Reten

### Estructura de Carpetas
```
reten-client/
├── users/
│   ├── 2024-02-20/
│   ├── 2024-02-21/
│   └── 2024-02-22/
├── products/
│   ├── 2024-02-20/
│   ├── 2024-02-21/
│   └── 2024-02-22/
├── sellers/
│   ├── 2024-02-20/
│   ├── 2024-02-21/
│   └── 2024-02-22/
├── transactions/
│   ├── 2024-02-20/
│   ├── 2024-02-21/
│   └── 2024-02-22/
├── assignments/
│   ├── 2024-02-20/
│   ├── 2024-02-21/
│   └── 2024-02-22/
└── routes/
    ├── 2024-02-20/
    ├── 2024-02-21/
    └── 2024-02-22/
```

#### Organización por Fecha
Los archivos deben organizarse en subcarpetas por fecha de generación:

- **Formato de carpeta**: `YYYY-MM-DD` (año-mes-día)
- **Ubicación**: Dentro de cada carpeta de entidad
- **Un archivo por entidad por día**: Solo debe existir un archivo por tipo de entidad en cada carpeta diaria
- **Reemplazo automático**: Si se actualiza durante el mismo día, el archivo se reemplaza
- **Archivos particionados**: Si es necesario particionar, todos los archivos de la misma entidad se procesan juntos

**Ejemplos de estructura completa**:

*Archivo único:*
```
reten-client/
└── users/
    └── 2024-02-22/
        └── users.csv
```

*Archivos particionados:*
```
reten-client/
└── users/
    └── 2024-02-22/
        ├── users_1.csv
        ├── users_2.csv
        └── users_3.csv
```

> 💡 **Ventajas**: Simplifica la gestión de archivos, evita duplicados y garantiza que siempre se procese la versión más reciente de los datos del día.

> ⚡ **Archivos grandes**: Para datasets grandes, el particionado permite manejar volúmenes de datos sin límites de tamaño por archivo individual.

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
- Crear carpeta con formato de fecha (`YYYY-MM-DD`) dentro de la carpeta de entidad correspondiente
- Subir archivo(s) CSV al bucket según el caso:
  - **Archivo único**: `{entity_type}/{YYYY-MM-DD}/{entity_type}.csv`
  - **Archivos particionados**: `{entity_type}/{YYYY-MM-DD}/{entity_type}_{index}.csv`
- Si ya existe un archivo del mismo tipo en la carpeta del día, se reemplazará automáticamente
- Reten procesará automáticamente todos los archivos de la misma entidad en la carpeta del día

**Ejemplos de rutas completas**:

*Archivo único:*
```
reten-client/users/2024-02-22/users.csv
```

*Archivos particionados:*
```
reten-client/users/2024-02-22/users_1.csv
reten-client/users/2024-02-22/users_2.csv
reten-client/users/2024-02-22/users_3.csv
```

> ⚠️ **Importante**: Para archivos particionados, todos los archivos de la misma entidad en la carpeta del día se procesan como un conjunto completo.

> 💡 **Recomendación**: Usar particionado solo cuando sea realmente necesario por tamaño del dataset. Para la mayoría de casos, un archivo único es suficiente.

## Seguridad

- **Acceso restringido**: Solo el cliente y Reten tienen acceso al bucket
- **Credenciales únicas**: Cada cliente recibe credenciales específicas
- **Logs de auditoría**: Se registran todas las operaciones de archivo

## Próximos Pasos

Para implementar este método de integración:

1. **Solicitar credenciales** del bucket al equipo de Reten
2. **Generar archivos CSV** según la estructura de cada entidad
3. **Probar con datos de ejemplo** antes de la implementación completa
