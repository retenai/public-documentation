# :material-bell: Notificaciones Push

Esta documentación explica cómo configurar e implementar **Notificaciones Push** automáticas con Reten. Las notificaciones se envían a aplicaciones móviles y web a través de tu servicio de push existente (FCM, APNs, etc.).

Para la configuración general del canal, consulta [Configuración de Canales](../channel-provider-configuration/README.md). Aquí nos enfocamos en los aspectos específicos de push notifications.

## :material-api: Integración Técnica

### Configuración del Canal Push

Para configurar notificaciones push, sigue la estructura general descrita en [Configuración de Canales](../channel-provider-configuration/README.md) con los siguientes valores específicos:

```json
{
  "channel_type": "push",
  "provider_name": "tu_cliente_push",
  "endpoint_url": "https://api.tucliente.com/notifications/push",
  "auth_type": "bearer",
  "auth_header_name": "Authorization",
  "auth_token": "tu_token_fcm_o_apns"
}
```

!!! tip "Campos específicos para Push"
    - **channel_type**: Siempre `"push"`
    - **endpoint_url**: URL de tu API de notificaciones push
    - **auth_token**: Token de FCM, APNs, o tu servicio de push
    - **token_url**: Opcional, si usas OAuth para obtener tokens

### Payload Estándar

Reten enviará el siguiente payload estandarizado a tu endpoint. Este formato está diseñado para ser fácilmente adaptable a diferentes proveedores de push:

```json
{
  "title": "¡Pedido Confirmado! 🎉",
  "message": "Tu pedido #12345 ha sido confirmado",
  "action_url": "https://tucliente.com/orders/12345",
  "push_tokens": ["fcm_token_abc123", "apns_token_xyz789"],
  "metadata": {
    "campaign_id": "order_notifications",
    "trigger_type": "order_completed",
    "order_id": "12345"
  }
}
```

### Adaptación a Proveedores

Este payload se puede adaptar fácilmente a los formatos específicos de cada proveedor:

#### Firebase Cloud Messaging (FCM)
```json
{
  "registration_ids": ["fcm_token_abc123", "fcm_token_xyz789"],
  "notification": {
    "title": "¡Pedido Confirmado! 🎉",
    "body": "Tu pedido #12345 ha sido confirmado"
  },
  "data": {
    "action_url": "https://tucliente.com/orders/12345",
    "campaign_id": "order_notifications",
    "trigger_type": "order_completed",
    "order_id": "12345"
  }
}
```


### Requisitos Previos

Antes de configurar las notificaciones push, asegúrate de tener:

- Servicio de Push Notifications para móviles: Firebase Cloud Messaging (FCM)
- Servicio de Push Notifications para web: Service Workers registrados y soporte para Push API en navegadores modernos
- **Credenciales de API**: Claves y tokens necesarios para enviar notificaciones
- **Base de Datos de Tokens**: Almacén de tokens de dispositivo/navegador de tus usuarios
- **Consentimiento de Usuarios**: Sistema para gestionar opt-in/opt-out de notificaciones
- **Service Worker** (para web): Implementación para manejar las notificaciones push en el navegador

### Configuración en Reten

Una vez que proporciones la configuración siguiendo la estructura de [Configuración de Canales](../channel-provider-configuration/README.md), el proceso continúa así:

!!! note "1. Reten configura el canal"
    - Creamos la configuración del canal push en nuestro sistema
    - Probamos la conectividad con tu endpoint
    - Validamos el envío de notificaciones de prueba

!!! note "2. Tú preparas tus datos"
    - Incluye el campo `push_token` en tu carga de datos de usuarios
    - El `push_token` debe corresponder al token de FCM/APNs de cada usuario
    - Mantén actualizados los tokens de push de tus usuarios

!!! note "3. Reten implementa triggers"
    - Configuramos qué eventos gatillarán notificaciones push
    - Establecemos reglas de segmentación
    - Creamos plantillas de mensajes personalizadas

### Triggers y Reglas

Reten puede enviar notificaciones push basándose en diferentes tipos de triggers:

#### Triggers de Eventos
- **Compra Completada**: Notificar confirmación de pedido
- **Producto Agotado**: Alertar sobre productos sin stock
- **Promoción Disponible**: Informar ofertas especiales
- **Recordatorio de Carrito**: Recordar productos abandonados

#### Triggers de Reglas de Negocio
- **Cumpleaños del Usuario**: Felicitaciones personalizadas
- **Inactividad**: Recordatorios para reactivar engagement
- **Recompensas**: Notificar puntos o beneficios acumulados
- **Actualizaciones de Estado**: Cambios en pedidos o entregas

### Ejemplo de Implementación

Cuando Reten detecta un trigger (ej: pedido completado), envía el siguiente payload a tu endpoint:

```json
{
  "title": "¡Pedido Confirmado! 🎉",
  "message": "Tu pedido #12345 ha sido confirmado",
  "action_url": "https://cliente.com/orders/12345",
  "push_tokens": ["fcm_token_abc123"],
  "metadata": {
    "campaign_id": "order_notifications",
    "trigger_type": "order_completed",
    "order_id": "12345"
  }
}
```

**Tu endpoint debe:**

1. Recibir este payload
2. **Usar directamente los push_tokens**: Los tokens ya vienen listos del campo `push_token` de tus usuarios
3. **Adaptar al formato del proveedor**: Transformar el payload según el proveedor (FCM)
4. Enviar la notificación usando tu servicio de push
5. Retornar una respuesta de éxito/error

### Implementación Simplificada

```javascript
// En tu endpoint:
app.post('/notifications/push', (req, res) => {
  const { push_tokens, title, message, action_url } = req.body;

  // Los tokens ya vienen listos, no necesitas mapear
  const tokens = push_tokens; // ["fcm_token_abc123", "fcm_token_xyz789"]

  // Enviar a FCM con todos los tokens en batch
  const fcmPayload = {
    registration_ids: tokens,
    notification: { title, body: message },
    data: { action_url }
  };

  await sendToFCM(fcmPayload);
});
```

!!! tip "Identificadores Compuestos"
    Si necesitas el `user_id` junto con el token, puedes usar identificadores compuestos en el `push_token`:
    - `"CLI_001:fcm_token_abc123"` (user_id:token)
    - `"CLI_001|fcm_token_xyz789"` (user_id|token)

## :material-chart-line: Monitoreo y Métricas

Reten proporciona métricas detalladas sobre el rendimiento de tus notificaciones push:

- **Tasa de Entrega**: Porcentaje de notificaciones entregadas exitosamente
- **Tasa de Apertura**: Porcentaje de usuarios que interactúan con la notificación
- **Tasa de Conversión**: Porcentaje que completa la acción deseada
- **Opt-out Rate**: Porcentaje de usuarios que desactiva notificaciones

## :material-shield-check: Consideraciones de Privacidad

- **Consentimiento Explícito**: Solo enviamos a usuarios que han dado consentimiento (tanto en app móvil como en navegador web)
- **Categorización**: Permite diferentes tipos de notificaciones con opt-in independiente
- **Frecuencia**: Límites configurables para evitar spam en ambos canales
- **Transparencia**: Los usuarios pueden ver el historial de notificaciones enviadas
- **Soporte Multi-plataforma**: Manejo adecuado de tokens para diferentes dispositivos y navegadores

## :material-help-circle: Soporte

Si necesitas ayuda con la configuración de notificaciones push o tienes preguntas específicas sobre la integración, contacta al equipo de soporte de Reten.
