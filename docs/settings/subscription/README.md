# Suscripciones

Las suscripciones representan la configuración de comunicación y notificaciones para los diferentes canales del sistema. Cada suscripción define las preferencias y consentimientos de comunicación para un usuario específico.

## Estructura de Datos

```json
{
  // Identificadores
  "subscription_id": "string",  // Identificador único de la suscripción (not null)
  "user_id": "string",         // Identificador del usuario (not null)

  // Información del canal
  "channel": {
    "type": "string",          // Tipo de canal (email, sms, push, whatsapp) (not null)
    "identifier": "string",    // Identificador del canal (email, teléfono, token) (not null)
    "name": "string",          // Nombre descriptivo del canal
    "status": "string"         // Estado del canal (active, inactive, pending, blocked)
  },

  // Consentimientos y preferencias
  "preferences": {
    "marketing": "boolean",     // Comunicaciones de marketing
    "notifications": "boolean", // Notificaciones del sistema
    "promotions": "boolean",    // Ofertas y promociones
    "news": "boolean",         // Noticias y actualizaciones
    "orders": "boolean",       // Actualizaciones de órdenes
    "support": "boolean"       // Comunicaciones de soporte
  },

  // Configuración de disponibilidad
  "availability": {
    "time_zone": "string",     // Zona horaria del usuario
    "working_hours": [{        // Intervalos de disponibilidad para contacto
      "start": "string",       // Hora de inicio (HH:mm)
      "end": "string",         // Hora de fin (HH:mm)
      "days": ["string"]       // Días de la semana aplicables (MON, TUE, WED, THU, FRI, SAT, SUN)
    }]
  },

  // Metadatos de verificación
  "verification": {
    "is_verified": "boolean",  // Canal verificado
    "verified_at": "timestamp", // Fecha de verificación
    "verification_method": "string", // Método de verificación usado
    "last_verification_attempt": "timestamp" // Último intento de verificación
  },

  // Marcas temporales
  "created_at": "timestamp",   // Fecha de creación (not null)
  "updated_at": "timestamp",   // Última actualización (not null)
  "_created_at": "timestamp",  // Fecha de creación en Reten (not null)
  "_updated_at": "timestamp"   // Última actualización en Reten (not null)
}
```

## Tipos de Canal

- `email`: Comunicaciones por correo electrónico
- `sms`: Mensajes de texto
- `push`: Notificaciones push
- `whatsapp`: Mensajes por WhatsApp

## Estados de Canal

- `active`: Canal activo y verificado
- `inactive`: Canal desactivado por el usuario
- `pending`: Pendiente de verificación
- `blocked`: Canal bloqueado por el sistema

## Tipos de Frecuencia

- `immediate`: Envío inmediato
- `daily`: Resumen diario
- `weekly`: Resumen semanal
- `custom`: Configuración personalizada

## Validaciones

1. **Canal**
   - Tipo de canal debe ser válido
   - Identificador debe corresponder al tipo de canal
   - Estado debe ser válido

2. **Preferencias**
   - Al menos una preferencia debe estar activa
   - Valores booleanos válidos

3. **Frecuencia**
   - Zona horaria válida
   - Horas de working_hours en formato válido
   - Configuración custom_schedule válida según tipo

4. **Verificación**
   - Fechas de verificación válidas
   - Método de verificación conocido

## Ejemplos

### Suscripción de Email
```json
{
  "subscription_id": "SUB_001",
  "user_id": "USER_123",
  "channel": {
    "type": "email",
    "identifier": "usuario@ejemplo.com",
    "status": "active"
  },
  "preferences": {
    "marketing": true,
    "notifications": true,
    "orders": true
  },
  "availability": {
    "time_zone": "America/Santiago"
  }
}
```

### Suscripción de WhatsApp
```json
{
  "subscription_id": "SUB_002",
  "user_id": "USER_123",
  "channel": {
    "type": "whatsapp",
    "identifier": "+56912345678",
    "status": "pending"
  },
  "preferences": {
    "orders": true,
    "support": true
  },
  "availability": {
    "time_zone": "America/Santiago",
    "working_hours": [{
      "start": "22:00",
      "end": "08:00",
      "days": ["MON", "TUE", "WED", "THU", "FRI"]
    }]
  }
}
```

## Notas de Implementación

1. **Verificación de Canales**
   - Email: Enlace de verificación
   - SMS/WhatsApp: Código de verificación
   - Push: Token del dispositivo

2. **Gestión de Preferencias**
   - Cambios requieren confirmación
   - Historial de cambios auditable
   - Respeto de regulaciones locales

3. **Control de Frecuencia**
   - Respetar zona horaria del usuario
   - Consolidación de mensajes según frecuencia
   - Manejo de excepciones en working_hours 