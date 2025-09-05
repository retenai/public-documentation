# :material-cog: Configuración de Canales

Esta documentación explica la información que debe proporcionar un cliente para habilitar un canal de comunicación con Reten. Estos son exactamente los campos que el cliente debe compartir con el equipo de Reten para configurar el canal.

!!! warning "Información Sensible"
    Esta documentación representa cómo Reten almacena y gestiona las credenciales de los clientes. **Las credenciales deben ser entregadas de manera segura previa coordinación con el equipo técnico de Reten**.

## Estructura de Datos

```json
{
  "channel_type": "push|email|whatsapp|sms",  // Tipo de canal
  "provider_name": "string",                  // Nombre identificador del proveedor
  "endpoint_url": "string",                   // URL del endpoint del cliente
  "auth_type": "bearer|api_key|basic_auth",   // Tipo de autenticación
  "auth_header_name": "string",               // Nombre del header de auth (opcional)
  "auth_token": "string",                     // Token de autenticación (opcional)
  "api_key": "string",                        // API key (opcional)
  "secret": "string",                         // Secret/token adicional (opcional)
  "token_url": "string"                       // URL para obtener token (opcional)
}
```

## Campos Detallados

| Campo            | Tipo   | Requerido | Descripción                                             |
| ---------------- | ------ | --------- | ------------------------------------------------------- |
| channel_type     | string | Sí        | Tipo de canal: push, email, whatsapp, sms               |
| provider_name    | string | Sí        | Nombre identificador del proveedor                      |
| endpoint_url     | string | Sí        | URL del endpoint del cliente                            |
| auth_type        | string | Sí        | Tipo de autenticación: bearer, api_key, basic_auth      |
| auth_header_name | string | No        | Nombre del header de autenticación                      |
| auth_token       | string | No        | Token de autenticación                                  |
| api_key          | string | No        | API key del proveedor                                   |
| secret           | string | No        | Secret o token adicional                                |
| token_url        | string | No        | URL para obtener token antes de las requests (opcional) |

## Validaciones

- `channel_type` debe ser uno de: push, email, whatsapp, sms
- `endpoint_url` debe ser una URL válida
- `auth_type` debe ser uno de: bearer, api_key, basic_auth
- Solo uno de `auth_token`, `api_key` o `secret` es requerido según el `auth_type`

## Flujo de Autenticación con Token URL

Cuando se proporciona un `token_url`, Reten seguirá este flujo de autenticación:

1. **Obtención de Token**: Primero hace una request al `token_url` con las credenciales proporcionadas
2. **Almacenamiento Temporal**: El token obtenido se almacena temporalmente para su uso
3. **Request al Endpoint**: Luego hace la request al `endpoint_url` usando el token obtenido
4. **Renovación Automática**: Si el token expira, se renueva automáticamente usando el `token_url`

### Ejemplo de Token URL
```json
{
  "channel_type": "whatsapp",
  "provider_name": "cliente_whatsapp_provider",
  "endpoint_url": "https://api.cliente.com/whatsapp/send",
  "auth_type": "bearer",
  "auth_header_name": "Authorization",
  "token_url": "https://api.cliente.com/oauth/token",
  "api_key": "client_id_here",
  "secret": "client_secret_here"
}
```

En este caso, Reten primero llamará al `token_url` con el `api_key` y `secret` para obtener un token de acceso, y luego usará ese token para hacer requests al `endpoint_url`.

## Ejemplos de Uso

### Configuración Push Notifications
```json
{
  "channel_type": "push",
  "provider_name": "cliente_push_provider",
  "endpoint_url": "https://api.cliente.com/notifications/push",
  "auth_type": "bearer",
  "auth_header_name": "Authorization",
  "auth_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9..."
}
```

### Configuración Email
```json
{
  "channel_type": "email",
  "provider_name": "cliente_email_provider",
  "endpoint_url": "https://api.cliente.com/emails/send",
  "auth_type": "api_key",
  "auth_header_name": "X-API-Key",
  "api_key": "sk_live_1234567890abcdef"
}
```

### Configuración con Token URL (OAuth Flow)
```json
{
  "channel_type": "whatsapp",
  "provider_name": "cliente_whatsapp_provider",
  "endpoint_url": "https://api.cliente.com/whatsapp/send",
  "auth_type": "bearer",
  "auth_header_name": "Authorization",
  "token_url": "https://api.cliente.com/oauth/token",
  "api_key": "client_id_here",
  "secret": "client_secret_here"
}
```

## Proceso de Configuración

1. **Cliente proporciona información**: Completa la estructura JSON con sus credenciales
2. **Reten configura internamente**: El equipo técnico configura el canal usando la API interna
3. **Pruebas y validación**: Se realizan pruebas para asegurar el funcionamiento
4. **Activación**: El canal queda habilitado para uso en producción

## Consideraciones Importantes

- **Seguridad**: Todas las credenciales son manejadas de forma segura por el equipo de Reten
- **Encriptación**: Los tokens y claves se almacenan encriptados
- **Validación**: Se validan todas las URLs y credenciales antes de la activación
- **Monitoreo**: Se establece monitoreo continuo del canal configurado
- **Entrega Segura**: Las credenciales deben coordinarse previamente con el equipo técnico para asegurar el método de entrega más apropiado
