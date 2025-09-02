# 📊 Carga de Datos

Esta sección contiene toda la información necesaria para cargar datos desde tu sistema hacia Reten. La carga de datos es el primer paso fundamental para implementar Reten en tu negocio.

!!! tip "Flujo de Datos"
    - **🔄 Carga de Datos**: Tu sistema envía información a Reten
    - **📤 Consumo de Datos**: Reten envía información procesada a tu sistema

## 🔌 Métodos de Conexión

Reten ofrece dos métodos principales para cargar datos:

{% from '/includes/cards.md' import feature_card, section_cards, feature_highlights, highlight_item, section_overview %}

{% set connection_methods = [
    {
        'icon': 'material-folder',
        'title': 'Archivo',
        'link': '../connection-methods/file-based/README.md',
        'description': 'Carga periódica de archivos CSV en bucket compartido para sincronización de datos.'
    },
    {
        'icon': 'material-database',
        'title': 'Base de Datos',
        'link': '../connection-methods/database/README.md',
        'description': 'Consulta directa a base de datos expuesta por el cliente para acceso en tiempo real.'
    }
] %}

{{ section_overview(connection_methods) }}

## 📋 Tipos de Datos Disponibles

### 🗂️ Datos Maestros

Los datos maestros representan la información fundamental de tu negocio:

{% set master_data = [
    {
        'icon': 'material-account-group',
        'title': 'Clientes',
        'link': '../master-data/client/README.md',
        'description': 'Información de clientes, contactos y direcciones.'
    },
    {
        'icon': 'material-package-variant',
        'title': 'Productos',
        'link': '../master-data/product/README.md',
        'description': 'Catálogo de productos, precios y características.'
    },
    {
        'icon': 'material-account-tie',
        'title': 'Vendedores',
        'link': '../master-data/seller/README.md',
        'description': 'Equipo de ventas y fuerza comercial.'
    },
    {
        'icon': 'material-ticket-percent',
        'title': 'Cupones',
        'link': '../master-data/coupon/README.md',
        'description': 'Descuentos, promociones y reglas comerciales.'
    },
    {
        'icon': 'material-cart',
        'title': 'Transacciones',
        'link': '../master-data/transactions/README.md',
        'description': 'Historial de compras y ventas.'
    },
    {
        'icon': 'material-chart-timeline-variant',
        'title': 'Eventos',
        'link': '../master-data/events/README.md',
        'description': 'Acciones y actividades del sistema para análisis retrospectivo.'
    }
] %}

{{ section_overview(master_data) }}

### ⚙️ Configuraciones

Las configuraciones definen cómo funciona tu negocio en Reten:

{% set settings = [
    {
        'icon': 'material-account-multiple-check',
        'title': 'Asignaciones',
        'link': '../settings/assignments/README.md',
        'description': 'Relaciones entre vendedores y clientes.'
    },
    {
        'icon': 'material-map-marker-path',
        'title': 'Rutas',
        'link': '../settings/routes/README.md',
        'description': 'Planificación de visitas y territorios.'
    }
] %}

{{ section_overview(settings) }}

## 🚀 Flujo de Implementación

Para implementar la carga de datos en Reten, sigue estos pasos:

!!! note "1. Elige tu Método de Conexión"
    - **Archivo**: Ideal para sincronizaciones periódicas y sistemas legacy
    - **Base de Datos**: Ideal para datos en tiempo real y sistemas modernos

!!! note "2. Prepara tus Datos Maestros"
    - Define tus [Productos](../master-data/product/README.md)
    - Registra tus [Clientes](../master-data/client/README.md)
    - Informa tus [Vendedores](../master-data/seller/README.md) en caso de tener fuerza de venta

!!! note "3. Configura tu Sistema"
    - Define las [Asignaciones](../settings/assignments/README.md) entre vendedores y clientes
    - Configura las [Rutas](../settings/routes/README.md) de visita

!!! note "4. Implementa la Sincronización"
    - Configura la frecuencia de sincronización según tu método elegido
    - Monitorea los logs y confirmaciones de carga
    - Verifica que los datos se procesen correctamente

## 💡 Recomendaciones

- **Empieza simple**: Comienza con los datos maestros esenciales (clientes y productos)
- **Valida datos**: Asegúrate de que tus datos cumplan con las validaciones especificadas
- **Prueba primero**: Usa un ambiente de desarrollo antes de pasar a producción
- **Monitorea**: Revisa regularmente los logs de sincronización

## 🔗 Próximos Pasos

Una vez que hayas implementado la carga de datos, podrás:

- Configurar [Tareas](../tasks/README.md) para gestionar actividades
- Monitorear [Eventos](../events/README.md) del sistema
- Integrar con herramientas de comunicación (funcionalidad futura)
