# Eventos de Usuario

Los eventos de usuario registran todas las interacciones relacionadas con la gestión de cuentas, autenticación y autorización en la plataforma.

## Eventos Disponibles

### `account_requested`

Registra cuando un usuario solicita la creación de una cuenta.

**Cuándo enviarlo:**
- Al completar un formulario de registro
- Al solicitar una invitación
- Al iniciar proceso de creación de cuenta

**Estructura específica:**
```json
{
  "event_type": "account_requested",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "account": {
      "email": "string",
      "name": "string",
      "type": "string",      // personal, business
      "role": "string"       // buyer, seller, admin
    },
    "business_info": {       // opcional, para cuentas business
      "company_name": "string",
      "tax_id": "string",
      "industry": "string"
    },
    "source": {
      "channel": "string",   // web, app, sales_rep
      "campaign": "string",  // opcional
      "referrer": "string"   // opcional
    }
  }
}
```

**Campos Requeridos:**
- `account`: Información básica de la cuenta solicitada
- `source`: Origen de la solicitud

---

### `account_created`

Registra la creación exitosa de una cuenta.

**Cuándo enviarlo:**
- Cuando se completa el proceso de creación de cuenta
- Al confirmar el email de verificación
- Al activar una cuenta pre-registrada

**Estructura específica:**
```json
{
  "event_type": "account_created",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "account": {
      "account_id": "string",
      "email": "string",
      "name": "string",
      "type": "string",
      "role": "string",
      "status": "string"     // active, pending_verification
    },
    "preferences": {
      "language": "string",
      "currency": "string",
      "notifications": [{
        "channel": "string",
        "enabled": "boolean"
      }]
    },
    "metadata": {
      "creation_method": "string",  // self_service, admin_created, invitation
      "verification_status": "string"
    }
  }
}
```

**Campos Requeridos:**
- `account`: Información completa de la cuenta creada
- `metadata`: Metadatos del proceso de creación

---

### `customer_authorized`

Registra la autorización de un cliente para operar en la plataforma.

**Cuándo enviarlo:**
- Al aprobar una cuenta de cliente
- Al asignar permisos específicos
- Al activar funcionalidades para un cliente

**Estructura específica:**
```json
{
  "event_type": "customer_authorized",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "customer": {
      "customer_id": "string",
      "account_id": "string",
      "type": "string"
    },
    "authorization": {
      "level": "string",     // full, restricted, trial
      "channels": ["string"],
      "features": ["string"],
      "valid_until": "timestamp"  // opcional
    },
    "approver": {
      "user_id": "string",
      "role": "string"
    }
  }
}
```

**Campos Requeridos:**
- `customer`: Información del cliente autorizado
- `authorization`: Detalles de la autorización
- `approver`: Información de quien autorizó

---

### `account_login`

Registra los inicios de sesión en la plataforma.

**Cuándo enviarlo:**
- Al iniciar sesión exitosamente
- Al renovar una sesión
- Al usar SSO para acceder

**Estructura específica:**
```json
{
  "event_type": "account_login",
  "event_date": "2024-03-19T14:30:00Z",
  "user_id": "string",
  
  "event_data": {
    "session": {
      "session_id": "string",
      "login_method": "string",  // password, sso, token
      "device_type": "string",
      "ip_address": "string"
    },
    "security": {
      "mfa_used": "boolean",
      "mfa_type": "string",      // opcional
      "location": {              // opcional
        "country": "string",
        "city": "string"
      }
    }
  }
}
```

**Campos Requeridos:**
- `session`: Información de la sesión iniciada
- `security`: Detalles de seguridad del login

## Flujo de Eventos

El flujo típico de gestión de usuarios sigue este orden:

1. `account_requested`
2. `account_created`
3. `customer_authorized` (si requiere aprobación)
4. `account_login` (múltiples ocurrencias)

## Validaciones Específicas

1. **Seguridad**
   - Los emails deben ser únicos
   - Los `account_id` deben ser únicos
   - Las contraseñas no deben incluirse en los eventos

2. **Consistencia**
   - Los `user_id` deben ser consistentes entre eventos relacionados
   - Los roles y permisos deben ser válidos según la configuración

3. **Temporalidad**
   - `account_created` debe seguir a `account_requested`
   - `customer_authorized` debe seguir a `account_created`

## Ejemplos de Implementación

### JavaScript
```javascript
const sendUserEvent = async (eventType, userData) => {
  await retenClient.events.track({
    event_type: eventType,
    event_date: new Date().toISOString(),
    user_id: getCurrentUserId(),
    event_data: {
      ...userData,
      session: {
        session_id: getSessionId(),
        device_type: getDeviceType()
      }
    }
  });
};
```

### Python
```python
def send_user_event(event_type, user_data):
    reten_client.events.track(
        event_type=event_type,
        event_date=datetime.now().isoformat(),
        user_id=get_current_user_id(),
        event_data={
            **user_data,
            "session": {
                "session_id": get_session_id(),
                "device_type": get_device_type()
            }
        }
    )
``` 