# Entidades

Las entidades representan los objetos principales del sistema Reten. Cada entidad tiene propiedades que definen sus características y comportamientos, manteniendo el estado actual de los diferentes componentes del sistema.

## Características Comunes

Todas las entidades comparten:
- Identificación dual (ID interno Reten y ID externo cliente)
- Trazabilidad temporal (fechas de creación/actualización)
- Extensibilidad mediante atributos personalizados
- Campos de auditoría y metadatos

## Catálogo de Entidades

### [Clientes](./client/README.md)
Establecimientos comerciales que interactúan con la plataforma.
- Información de contacto y ubicación
- Grupos y clasificaciones
- Datos de facturación y entrega

### [Vendedores](./seller/README.md)
Usuarios del CPG que gestionan las relaciones comerciales.
- Roles y permisos
- Información de acceso
- Asignaciones y territorios

### [Productos](./product/README.md)
Catálogo de productos disponibles para comercialización.
- Información básica y precios
- Categorías y atributos
- Configuraciones específicas

### [Cupones](./coupon/README.md)
Mecanismos de promoción y descuento.
- Reglas y condiciones
- Periodos de validez
- Restricciones de uso

### [Suscripciones](./subscription/README.md)
Configuraciones de comunicación y notificaciones.
- Preferencias por canal
- Estados y configuraciones
- Gestión de consentimientos

### [Categorías](./category/README.md)
Estructura jerárquica para la organización de productos.
- Jerarquía y relaciones
- Atributos por nivel
- Metadatos de clasificación

## Convenciones Generales

- Campos en `snake_case`
- Fechas en formato ISO 8601 (UTC)
- IDs descriptivos y únicos por entidad
- Campos requeridos marcados como `not null`
- Referencias mediante IDs para mantener integridad referencial

Para más detalles sobre cada entidad, consulta su documentación específica en los enlaces proporcionados. 