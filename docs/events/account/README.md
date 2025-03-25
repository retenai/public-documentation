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

| Evento                | Descripción                                    | Trigger                                 |
| --------------------- | ---------------------------------------------- | --------------------------------------- |
| `user_registered`     | Registra la creación de una nueva cuenta       | Usuario completa el proceso de registro |
| `user_login`          | Registra el inicio de sesión en la plataforma  | Usuario accede a su cuenta              |
| `profile_updated`     | Registra actualizaciones al perfil del usuario | Usuario modifica sus datos personales   |
| `address_updated`     | Registra cambios en las direcciones            | Usuario gestiona sus direcciones        |
| `preferences_updated` | Registra cambios en preferencias               | Usuario actualiza sus configuraciones   |

### Registro de Usuario

!!! info "Trigger"
    Este evento se dispara cuando un usuario completa exitosamente el proceso de registro en la plataforma.

!!! example "Descripción"
    Registra la creación de una nueva cuenta de usuario, capturando información esencial del perfil y preferencias iniciales.

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

| Campo                   | Tipo   | Requerido | Descripción                       |
| ----------------------- | ------ | --------- | --------------------------------- |
| `user_id`               | string | Sí        | Identificador único del usuario   |
| `email`                 | string | Sí        | Correo electrónico del usuario    |
| `registration_method`   | string | Sí        | Método utilizado para el registro |
| `user_type`             | string | Sí        | Tipo de usuario registrado        |
| `profile_data`          | object | Sí        | Datos básicos del perfil          |
| `marketing_preferences` | object | No        | Preferencias de comunicación      |

---

### Inicio de Sesión

!!! info "Trigger"
    Este evento se dispara cuando un usuario accede exitosamente a su cuenta en la plataforma.

!!! example "Descripción"
    Registra cuando un usuario inicia sesión, incluyendo información sobre el método de acceso, dispositivo y ubicación.

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

| Campo          | Tipo   | Requerido | Descripción                    |
| -------------- | ------ | --------- | ------------------------------ |
| `login_method` | string | Sí        | Método utilizado para el login |
| `session_id`   | string | Sí        | ID de la sesión creada         |
| `device_info`  | object | Sí        | Información del dispositivo    |
| `location`     | object | No        | Información de ubicación       |

---

### Actualización de Perfil

!!! info "Trigger"
    Este evento se dispara cuando un usuario modifica cualquier información de su perfil.

!!! example "Descripción"
    Registra cuando un usuario actualiza su información personal, manteniendo un historial de cambios y estados de verificación.

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

| Campo                 | Tipo   | Requerido | Descripción                      |
| --------------------- | ------ | --------- | -------------------------------- |
| `updated_fields`      | array  | Sí        | Lista de campos actualizados     |
| `previous_values`     | object | No        | Valores anteriores de los campos |
| `new_values`          | object | Sí        | Nuevos valores de los campos     |
| `update_type`         | string | Sí        | Origen de la actualización       |
| `verification_status` | object | No        | Estado de verificación           |

---

### Gestión de Direcciones

!!! info "Trigger"
    Este evento se dispara cuando un usuario agrega, modifica o elimina una dirección en su perfil.

!!! example "Descripción"
    Registra cuando un usuario gestiona sus direcciones, incluyendo validación y datos de geolocalización.

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

| Campo               | Tipo   | Requerido | Descripción                   |
| ------------------- | ------ | --------- | ----------------------------- |
| `address_id`        | string | Sí        | Identificador de la dirección |
| `action`            | string | Sí        | Tipo de acción realizada      |
| `address_type`      | string | Sí        | Tipo de dirección             |
| `address_data`      | object | Sí        | Datos de la dirección         |
| `validation_status` | string | No        | Estado de validación          |

---

### Actualización de Preferencias

!!! info "Trigger"
    Este evento se dispara cuando un usuario modifica sus preferencias de comunicación o configuración de la cuenta.

!!! example "Descripción"
    Registra cuando un usuario actualiza sus preferencias, incluyendo configuraciones de privacidad y comunicación.

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

| Campo                       | Tipo   | Requerido | Descripción                     |
| --------------------------- | ------ | --------- | ------------------------------- |
| `preference_type`           | string | Sí        | Tipo de preferencia actualizada |
| `previous_settings`         | object | No        | Configuración anterior          |
| `new_settings`              | object | Sí        | Nueva configuración             |
| `communication_preferences` | object | No        | Preferencias de comunicación    |
| `privacy_settings`          | object | No        | Configuración de privacidad     |

## Consideraciones Técnicas

- Los eventos deben manejar datos sensibles según normativas de privacidad
- Se debe implementar validación de datos en tiempo real
- Los cambios en información crítica deben requerir verificación
- Se recomienda mantener un historial de cambios para auditoría
- Los eventos deben ser consistentes entre diferentes plataformas
- Se debe implementar rate limiting para prevenir abusos

## Casos de Uso Comunes

### Análisis de Registro y Retención

Utiliza los eventos `user_registered` y `user_login` para:

1. Medir tasas de conversión en el registro
2. Analizar patrones de retención de usuarios
3. Identificar fuentes de tráfico efectivas

### Optimización de Perfiles

Utiliza el evento `profile_updated` para:

1. Identificar campos comúnmente actualizados
2. Mejorar el proceso de onboarding
3. Personalizar la experiencia del usuario

### Gestión de Comunicaciones

Utiliza el evento `preferences_updated` para:

1. Optimizar estrategias de comunicación
2. Respetar preferencias de privacidad
3. Mejorar la relevancia del contenido

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
