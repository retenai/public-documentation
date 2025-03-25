# :material-account: Cuenta

Esta sección documenta los eventos relacionados con la gestión de usuarios y sus preferencias.

## Descripción General

Los eventos de cuenta y perfil capturan todas las interacciones relacionadas con la gestión de usuarios, sus datos personales y preferencias. Estos eventos son fundamentales para:

- Mantener un registro de la actividad de los usuarios
- Gestionar el ciclo de vida de las cuentas
- Asegurar el cumplimiento de normativas de privacidad
- Personalizar la experiencia del usuario
- Analizar el comportamiento y preferencias de usuarios
- Mejorar la seguridad y prevención de fraudes

## Eventos Disponibles

### Registro de Usuario

**Tipo de Evento:** `user_registered`

**Descripción:**  
Registra la creación de una nueva cuenta de usuario en la plataforma.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "user_registered",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "email": "string",
    "registration_method": "email|google|facebook|apple",
    "user_type": "customer|business",
    "profile_data": {
      "first_name": "string",
      "last_name": "string",
      "phone_number": "string",
      "language": "string",
      "country": "string"
    },
    "marketing_preferences": {
      "email_notifications": "boolean",
      "sms_notifications": "boolean",
      "push_notifications": "boolean"
    }
  },
  "metadata": {
    "registration_platform": "web|mobile|pos",
    "utm_source": "string",
    "utm_medium": "string",
    "utm_campaign": "string"
  }
}
```

**Campos Específicos:**

| Campo                 | Tipo   | Requerido | Descripción                       |
| --------------------- | ------ | --------- | --------------------------------- |
| user_id               | string | Sí        | Identificador único del usuario   |
| email                 | string | Sí        | Correo electrónico del usuario    |
| registration_method   | string | Sí        | Método utilizado para el registro |
| user_type             | string | Sí        | Tipo de usuario registrado        |
| profile_data          | object | Sí        | Datos básicos del perfil          |
| marketing_preferences | object | No        | Preferencias de comunicación      |

### Inicio de Sesión

**Tipo de Evento:** `user_login`

**Descripción:**  
Registra cuando un usuario inicia sesión en la plataforma.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "user_login",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "login_method": "email|google|facebook|apple",
    "session_id": "string",
    "device_info": {
      "type": "mobile|desktop|tablet",
      "os": "string",
      "browser": "string",
      "app_version": "string"
    },
    "location": {
      "ip": "string",
      "country": "string",
      "city": "string"
    }
  },
  "metadata": {
    "is_suspicious": "boolean",
    "requires_2fa": "boolean",
    "last_login": "string"
  }
}
```

**Campos Específicos:**

| Campo        | Tipo   | Requerido | Descripción                    |
| ------------ | ------ | --------- | ------------------------------ |
| login_method | string | Sí        | Método utilizado para el login |
| session_id   | string | Sí        | ID de la sesión creada         |
| device_info  | object | Sí        | Información del dispositivo    |
| location     | object | No        | Información de ubicación       |

### Actualización de Perfil

**Tipo de Evento:** `profile_updated`

**Descripción:**  
Registra cuando un usuario actualiza su información de perfil.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "profile_updated",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "updated_fields": ["string"],
    "previous_values": {},
    "new_values": {},
    "update_type": "user|system",
    "verification_status": {
      "email": "verified|pending|none",
      "phone": "verified|pending|none"
    }
  },
  "metadata": {
    "requires_verification": "boolean",
    "update_reason": "string"
  }
}
```

**Campos Específicos:**

| Campo               | Tipo   | Requerido | Descripción                      |
| ------------------- | ------ | --------- | -------------------------------- |
| updated_fields      | array  | Sí        | Lista de campos actualizados     |
| previous_values     | object | No        | Valores anteriores de los campos |
| new_values          | object | Sí        | Nuevos valores de los campos     |
| update_type         | string | Sí        | Origen de la actualización       |
| verification_status | object | No        | Estado de verificación           |

### Gestión de Direcciones

**Tipo de Evento:** `address_updated`

**Descripción:**  
Registra cuando un usuario agrega, modifica o elimina una dirección.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "address_updated",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "address_id": "string",
    "action": "add|update|delete",
    "address_type": "shipping|billing|both",
    "address_data": {
      "street": "string",
      "number": "string",
      "unit": "string",
      "city": "string",
      "state": "string",
      "country": "string",
      "postal_code": "string",
      "is_default": "boolean"
    },
    "validation_status": "valid|invalid|pending"
  },
  "metadata": {
    "geocoding_data": {
      "latitude": "number",
      "longitude": "number"
    }
  }
}
```

**Campos Específicos:**

| Campo             | Tipo   | Requerido | Descripción                   |
| ----------------- | ------ | --------- | ----------------------------- |
| address_id        | string | Sí        | Identificador de la dirección |
| action            | string | Sí        | Tipo de acción realizada      |
| address_type      | string | Sí        | Tipo de dirección             |
| address_data      | object | Sí        | Datos de la dirección         |
| validation_status | string | No        | Estado de validación          |

### Actualización de Preferencias

**Tipo de Evento:** `preferences_updated`

**Descripción:**  
Registra cuando un usuario actualiza sus preferencias de comunicación o configuración.

**Estructura del Evento:**
```json
{
  "event_id": "string",
  "event_type": "preferences_updated",
  "timestamp": "2024-03-21T10:30:00Z",
  "source": "string",
  "data": {
    "user_id": "string",
    "preference_type": "notifications|privacy|language|currency",
    "previous_settings": {},
    "new_settings": {},
    "communication_preferences": {
      "email_marketing": "boolean",
      "order_updates": "boolean",
      "promotional_sms": "boolean",
      "newsletter": "boolean"
    },
    "privacy_settings": {
      "data_sharing": "boolean",
      "profile_visibility": "public|private|contacts"
    }
  },
  "metadata": {
    "update_source": "user_settings|onboarding|support",
    "effective_date": "string"
  }
}
```

**Campos Específicos:**

| Campo                     | Tipo   | Requerido | Descripción                     |
| ------------------------- | ------ | --------- | ------------------------------- |
| preference_type           | string | Sí        | Tipo de preferencia actualizada |
| previous_settings         | object | No        | Configuración anterior          |
| new_settings              | object | Sí        | Nueva configuración             |
| communication_preferences | object | No        | Preferencias de comunicación    |
| privacy_settings          | object | No        | Configuración de privacidad     |

## Consideraciones Técnicas

- Los eventos deben cumplir con regulaciones de privacidad (GDPR, CCPA)
- La información sensible debe estar encriptada
- Se debe mantener un historial de cambios para auditoría
- Los eventos de autenticación deben incluir información de seguridad
- Se debe implementar rate limiting para eventos de autenticación
- Las actualizaciones de perfil deben validar datos antes del evento

## Casos de Uso Comunes

### Análisis de Registro y Retención

Utiliza los eventos `user_registered` y `user_login` para:

1. Analizar tasas de conversión de registro
2. Identificar patrones de abandono
3. Optimizar el proceso de onboarding

### Gestión de Preferencias

Utiliza el evento `preferences_updated` para:

1. Personalizar la experiencia del usuario
2. Mejorar las estrategias de comunicación
3. Cumplir con preferencias de privacidad

### Validación de Direcciones

Utiliza el evento `address_updated` para:

1. Mejorar la precisión de entregas
2. Optimizar costos de envío
3. Prevenir fraudes

## Preguntas Frecuentes

### ¿Cómo manejar datos sensibles en los eventos?
Encriptar datos sensibles y nunca incluir información como contraseñas.

### ¿Qué hacer si falla la verificación de email/teléfono?
Implementar un sistema de reintentos y notificaciones alternativas.

### ¿Cómo gestionar el consentimiento de usuarios?
Mantener registros detallados de consentimientos y permitir su revocación.

## Referencias

- [Documentación de la API de Eventos](../integration/events_api.md)
- [Guía de Privacidad y Seguridad](../integration/privacy_security.md)
- [Mejores Prácticas de Gestión de Usuarios](../integration/user_management_best_practices.md)
