# :material-connection: Métodos de Conexión

Los métodos de conexión definen cómo los clientes pueden sincronizar datos con la plataforma Reten. Elige el método que mejor se adapte a tu infraestructura técnica.

{% from '/includes/cards.md' import feature_card, section_cards %}

{% call section_cards() %}
{{ feature_card(
    'material-folder',
    'Archivo',
    'Carga de archivos CSV en bucket compartido para sincronización de datos',
    'file-based/README.md'
) }}

{{ feature_card(
    'material-database',
    'Base de Datos',
    'Consulta directa a base de datos expuesta por el cliente',
    'database/README.md'
) }}
{% endcall %}

## Características Comunes

Ambos métodos de integración comparten:

- Sincronización automática de datos
- Validación de integridad y formato

## Convenciones Generales

- Formato de datos estándar (CSV para archivos, SQL para BD)
- Codificación UTF-8 para caracteres especiales
- Estructura de datos según la documentación de cada entidad
- IDs únicos para cada entidad sincronizada
