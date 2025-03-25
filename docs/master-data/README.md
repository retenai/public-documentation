# Datos Maestros

Los datos maestros representan la información fundamental y de referencia en el sistema Reten. Estas entidades constituyen el núcleo de datos que es compartido y reutilizado a través de los diferentes procesos y operaciones del negocio, asegurando consistencia y uniformidad en toda la plataforma.

## Características Comunes

Todas las entidades de datos maestros comparten:

- Identificación mediante ID de cliente
- Trazabilidad temporal (fechas de creación/actualización)
- Extensibilidad mediante atributos personalizados

Para asegurar la consistencia en la documentación y estructura de las entidades, se proporciona una [plantilla](./_template.md) que debe seguirse al crear nuevas entidades master-data.

## Catálogo de Entidades

### [Clientes](./client/README.md)

Personas naturales o empresas que interactúan con la plataforma, según el modelo de negocio (B2B o B2C).

- Información de contacto y ubicación
- Grupos y clasificaciones
- Datos de facturación y entrega


### [Productos](./product/README.md)

Catálogo de productos disponibles para comercialización.

- Información básica y precios
- Categorías y atributos
- Configuraciones específicas


### [Categorías](./category/README.md)

Estructura jerárquica para la organización de productos.

- Jerarquía y relaciones
- Atributos por nivel
- Metadatos de clasificación


### [Vendedores](./seller/README.md)

Representantes comerciales que gestionan las relaciones con los clientes.

- Información personal y de contacto
- Estructura organizacional y jerarquía
- Rutas y territorios asignados


### [Cupones](./coupon/README.md)

Mecanismos de promoción y descuento.

- Reglas y condiciones
- Periodos de validez
- Restricciones de uso


### [Transacciones](./transactions/README.md)

Registro de operaciones comerciales realizadas.

- Detalles de la compra
- Información de pago
- Estado y seguimiento


### [Suscripciones](./subscription/README.md)

Configuraciones de comunicación y notificaciones.

- Preferencias por canal
- Estados y configuraciones
- Gestión de consentimientos


## Convenciones Generales

- Campos en `snake_case`
- Fechas en formato ISO 8601 (UTC)
- IDs descriptivos y únicos por entidad
- Campos requeridos marcados como `not null`
- Referencias mediante IDs para mantener integridad referencial


Para más detalles sobre cada entidad, consulta su documentación específica en los enlaces proporcionados.