# :material-connection: Métodos de Conexión

Los métodos de conexión definen cómo los clientes pueden sincronizar datos con la plataforma Reten. Elige el método que mejor se adapte a tu infraestructura técnica.

{% from '/includes/cards.md' import section_overview %}

{% set connection_methods = [
    {
        'icon': 'material-folder',
        'title': 'Archivo',
        'link': 'file-based/README.md',
        'description': 'Carga de archivos CSV en bucket compartido para sincronización de datos'
    },
    {
        'icon': 'material-database',
        'title': 'Base de Datos',
        'link': 'database/README.md',
        'description': 'Consulta directa a base de datos expuesta por el cliente'
    }
] %}

{{ section_overview(connection_methods) }}

## Características Comunes

Ambos métodos de integración comparten:

- Sincronización automática de datos
- Validación de integridad y formato

## Convenciones Generales

- Formato de datos estándar (CSV para archivos, SQL para BD)
- Codificación UTF-8 para caracteres especiales
- Estructura de datos según la documentación de cada entidad
- IDs únicos para cada entidad sincronizada
