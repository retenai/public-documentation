# Entidades de Propiedades

Las entidades de propiedades representan objetos y sus características en el sistema Reten. Estas entidades son actualizables y mantienen el estado actual de los diferentes componentes del sistema.

## Estructura General

Todas las entidades de propiedades comparten características base:

1. **Sistema de Identificación Dual**
   - Identificador interno de Reten
   - Identificador externo del cliente

2. **Trazabilidad Temporal**
   - Fechas de creación y actualización en sistema cliente
   - Fechas de creación y actualización en Reten
   - Soporte para soft delete

3. **Extensibilidad**
   - Atributos personalizados
   - Metadatos específicos por tipo
   - Campos de auditoría

## Entidades Disponibles

1. [Comercios](./commerce.md)
   - Establecimientos comerciales
   - Información de contacto y ubicación
   - Grupos y clasificaciones

2. [Vendedores](./seller.md)
   - Usuarios del CPG
   - Roles y permisos
   - Información de acceso

3. [Productos](./product.md)
   - Catálogo de productos
   - Precios y categorías
   - Atributos específicos

4. [Cupones](./coupon.md)
   - Promociones y descuentos
   - Reglas y condiciones
   - Periodos de validez

5. [Suscripciones](./subscription.md)
   - Suscripciones a canales
   - Preferencias de comunicación
   - Estados y configuraciones

6. [Pricing](./pricing.md)
   - Políticas de precios
   - Reglas de precio
   - Configuraciones por producto

7. [Categorías](./category.md)
   - Jerarquía de productos
   - Atributos por nivel
   - Relaciones padre-hijo

## Convenciones

1. **Nomenclatura**
   - Campos en `snake_case`
   - IDs descriptivos y únicos
   - Prefijos según tipo de entidad

2. **Fechas y Timestamps**
   - Formato ISO 8601
   - Timezone UTC
   - Marcas temporales precisas

3. **Valores Nulos**
   - Campos requeridos marcados como `not null`
   - Valores por defecto cuando aplique
   - Validación de campos obligatorios

4. **Relaciones**
   - Referencias por ID
   - Integridad referencial
   - Consistencia en cascada

## Notas de Implementación

1. **Gestión de Cambios**
   - Todas las entidades mantienen historial
   - Cambios son auditables
   - Actualizaciones controladas

2. **Validaciones**
   - Reglas específicas por entidad
   - Validaciones cruzadas
   - Constraints de negocio

3. **Seguridad**
   - Control de acceso por entidad
   - Encriptación donde aplique
   - Auditoría de cambios

4. **Integración**
   - APIs RESTful
   - Webhooks para cambios
   - Sincronización bidireccional 