# :material-api: Consumo desde API REST de Reten

!!! warning "🚧 Work in Progress"
    Esta sección está en desarrollo activo. La documentación de API REST Reten se está construyendo y puede estar incompleta o sujeta a cambios.


### **Características Principales**

- **🔐 Autenticación**: OAuth 2.0 con JWT tokens
- **📡 Protocolo**: HTTP/HTTPS con RESTful endpoints
- **📊 Formato**: JSON para request/response
- **⚡ Performance**: Rate limiting configurado por cliente
- **🛡️ Seguridad**: TLS 1.3, validación de tokens, CORS configurado

### **Base URL**

```
https://api.reten.com/v1
```

### **Autenticación**

```http
Authorization: Bearer <your_jwt_token>
Content-Type: application/json
```

## 📋 Endpoints Disponibles

### **Tareas (Tasks)**

#### **Listar Tareas**
```http
GET /tasks
```

**Parámetros de Query:**
- `status`: Estado de la tarea (pending, in_progress, completed, cancelled)
- `assigned_to`: ID del usuario asignado
- `priority`: Prioridad (low, medium, high, urgent)
- `due_date_from`: Fecha límite desde (ISO 8601)
- `due_date_to`: Fecha límite hasta (ISO 8601)
- `page`: Número de página (default: 1)
- `limit`: Elementos por página (default: 20, max: 100)

**Ejemplo de Respuesta:**
```json
{
  "data": [
    {
      "id": "task_001",
      "title": "Visita a Cliente ABC",
      "description": "Reunión de seguimiento mensual",
      "status": "pending",
      "priority": "high",
      "assigned_to": "user_123",
      "due_date": "2024-01-20T14:00:00Z",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "pages": 8
  }
}
```

#### **Obtener Tarea Específica**
```http
GET /tasks/{task_id}
```

#### **Actualizar Estado de Tarea**
```http
PATCH /tasks/{task_id}
```

**Body:**
```json
{
  "status": "in_progress",
  "notes": "Iniciando visita al cliente"
}
```

### **Webhooks para Tareas**

#### **Configurar Webhook**
```http
POST /webhooks
```

**Body:**
```json
{
  "url": "https://tu-sistema.com/webhooks/tasks",
  "events": ["task.created", "task.updated", "task.completed"],
  "secret": "tu_secret_para_validar_webhooks"
}
```

#### **Estructura del Webhook**
```json
{
  "event": "task.created",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "id": "task_001",
    "title": "Nueva Tarea",
    "status": "pending",
    "assigned_to": "user_123"
  }
}
```

## 🔄 Métodos de Consumo

### **1. Polling (Consulta Periódica)**

**Descripción**: Consultas programadas a intervalos regulares para obtener datos actualizados.

**Ventajas:**
- ✅ Implementación simple
- ✅ Control total sobre la frecuencia
- ✅ Fácil de debuggear

**Desventajas:**
- ❌ Puede generar consultas innecesarias
- ❌ Latencia en la obtención de datos
- ❌ Mayor consumo de recursos

**Implementación Recomendada:**
```python
import requests
import time
from datetime import datetime, timedelta

def poll_tasks():
    headers = {
        'Authorization': f'Bearer {API_TOKEN}',
        'Content-Type': 'application/json'
    }
    
    # Consultar tareas pendientes
    response = requests.get(
        f'{API_BASE_URL}/tasks?status=pending',
        headers=headers
    )
    
    if response.status_code == 200:
        tasks = response.json()['data']
        process_tasks(tasks)
    
    # Esperar antes de la siguiente consulta
    time.sleep(300)  # 5 minutos
```

### **2. Webhooks (Notificaciones en Tiempo Real)**

**Descripción**: Reten envía notificaciones automáticas cuando ocurren eventos relevantes.

**Ventajas:**
- ✅ Notificaciones inmediatas
- ✅ Menor latencia
- ✅ Más eficiente en recursos

**Desventajas:**
- ❌ Requiere endpoint público
- ❌ Manejo de fallos más complejo
- ❌ Configuración inicial más elaborada

**Implementación Recomendada:**
```python
from flask import Flask, request, jsonify
import hmac
import hashlib

app = Flask(__name__)

@app.route('/webhooks/tasks', methods=['POST'])
def handle_task_webhook():
    # Validar firma del webhook
    signature = request.headers.get('X-Reten-Signature')
    if not verify_signature(request.data, signature):
        return jsonify({'error': 'Invalid signature'}), 401
    
    # Procesar el evento
    event_data = request.json
    event_type = event_data['event']
    
    if event_type == 'task.created':
        handle_task_created(event_data['data'])
    elif event_type == 'task.updated':
        handle_task_updated(event_data['data'])
    
    return jsonify({'status': 'success'}), 200

def verify_signature(payload, signature):
    expected = hmac.new(
        WEBHOOK_SECRET.encode(),
        payload,
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(f'sha256={expected}', signature)
```

### **3. Híbrido (Recomendado)**

**Descripción**: Combinar webhooks para eventos críticos con polling para sincronización periódica.

**Implementación:**
- **Webhooks**: Para eventos importantes (tarea creada, completada, cancelada)
- **Polling**: Para sincronización diaria y verificación de integridad
- **Fallback**: Polling como respaldo si fallan los webhooks

## ⚙️ Configuración de la Conexión

### **1. Obtención de Credenciales**

```bash
# Solicitar credenciales al equipo de Reten
curl -X POST https://api.reten.com/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "client_name": "Tu Sistema",
    "redirect_uri": "https://tu-sistema.com/callback",
    "scopes": ["tasks:read", "tasks:write"]
  }'
```

### **2. Configuración de Tokens**

```python
# Configuración de autenticación
API_CONFIG = {
    'base_url': 'https://api.reten.com/v1',
    'client_id': 'tu_client_id',
    'client_secret': 'tu_client_secret',
    'redirect_uri': 'https://tu-sistema.com/callback'
}

# Flujo de autenticación OAuth 2.0
def get_access_token():
    auth_url = f"{API_CONFIG['base_url']}/auth/authorize"
    params = {
        'client_id': API_CONFIG['client_id'],
        'redirect_uri': API_CONFIG['redirect_uri'],
        'scope': 'tasks:read tasks:write',
        'response_type': 'code'
    }
    # Redirigir usuario a auth_url para autorización
```

### **3. Configuración de Webhooks**

```python
# Configurar webhook para tareas
def setup_task_webhook():
    webhook_data = {
        'url': 'https://tu-sistema.com/webhooks/tasks',
        'events': ['task.created', 'task.updated', 'task.completed'],
        'secret': generate_webhook_secret()
    }
    
    response = requests.post(
        f'{API_BASE_URL}/webhooks',
        headers={'Authorization': f'Bearer {ACCESS_TOKEN}'},
        json=webhook_data
    )
    
    if response.status_code == 201:
        webhook_id = response.json()['id']
        print(f'Webhook configurado con ID: {webhook_id}')
```

## 🚀 Implementación Paso a Paso

### **Paso 1: Configuración Inicial**
1. Solicitar credenciales de API al equipo de Reten
2. Configurar variables de entorno con las credenciales
3. Implementar flujo de autenticación OAuth 2.0

### **Paso 2: Implementar Consumo Básico**
1. Crear función para listar tareas
2. Implementar paginación para grandes volúmenes
3. Agregar filtros por estado y prioridad

### **Paso 3: Configurar Webhooks**
1. Crear endpoint público para recibir webhooks
2. Implementar validación de firma
3. Configurar webhook en Reten

### **Paso 4: Implementar Sincronización**
1. Crear sistema de polling como respaldo
2. Implementar lógica de deduplicación
3. Agregar logging y monitoreo

### **Paso 5: Optimización y Monitoreo**
1. Configurar alertas para fallos de API
2. Implementar retry logic con backoff exponencial
3. Monitorear métricas de consumo y latencia

## 📊 Métricas y Monitoreo

### **KPIs Recomendados**
- **Tasa de Éxito**: Porcentaje de requests exitosos
- **Latencia**: Tiempo de respuesta promedio
- **Throughput**: Requests por minuto
- **Error Rate**: Porcentaje de errores por tipo

### **Alertas Configurables**
- **API Down**: Más de 5 errores consecutivos
- **High Latency**: Respuesta > 2 segundos
- **Rate Limit**: Aproximación al límite de requests
- **Webhook Failures**: Fallos en webhooks > 10%

## 🔧 Herramientas de Desarrollo

### **SDKs Oficiales**
- **Python**: `reten-python-sdk`
- **JavaScript**: `reten-js-sdk`
- **Java**: `reten-java-sdk`
- **C#**: `reten-dotnet-sdk`

### **Postman Collection**
```json
{
  "info": {
    "name": "Reten API - Consumo de Datos",
    "description": "Colección para consumir datos desde Reten"
  },
  "item": [
    {
      "name": "Obtener Tareas",
      "request": {
        "method": "GET",
        "url": "{{base_url}}/tasks",
        "header": [
          {
            "key": "Authorization",
            "value": "Bearer {{access_token}}"
          }
        ]
      }
    }
  ]
}
```

### **Ejemplos de Código**
- **Python**: Scripts de ejemplo para cada endpoint
- **JavaScript**: Implementaciones con Node.js
- **cURL**: Comandos de línea para testing
- **Postman**: Colección completa importable

## 🚨 Solución de Problemas

### **Errores Comunes**

#### **401 Unauthorized**
- Verificar que el token no haya expirado
- Confirmar que el scope incluya el recurso solicitado
- Validar formato del header Authorization

#### **429 Too Many Requests**
- Implementar backoff exponencial
- Reducir frecuencia de polling
- Optimizar consultas para obtener más datos por request

#### **500 Internal Server Error**
- Revisar logs de Reten
- Contactar al equipo de soporte
- Implementar retry logic para errores temporales

### **Debugging**
```python
import logging

# Configurar logging detallado
logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger(__name__)

def debug_api_call(url, headers, params=None):
    logger.debug(f"API Call: {url}")
    logger.debug(f"Headers: {headers}")
    if params:
        logger.debug(f"Params: {params}")
    
    response = requests.get(url, headers=headers, params=params)
    
    logger.debug(f"Response Status: {response.status_code}")
    logger.debug(f"Response Headers: {response.headers}")
    logger.debug(f"Response Body: {response.text}")
    
    return response
```

## 📚 Recursos Adicionales

### **Documentación de Referencia**
- [Guía de Autenticación](../README.md#autenticación)
- [Códigos de Error](../README.md#códigos-de-error)
- [Rate Limiting](../README.md#rate-limiting)
- [Mejores Prácticas](../README.md#mejores-prácticas)

### **Soporte Técnico**
- **Email**: api-support@reten.com
- **Slack**: #reten-api-support
- **Documentación**: https://docs.reten.com/api
- **Status Page**: https://status.reten.com

### **Comunidad**
- **GitHub**: https://github.com/reten/api-examples
- **Stack Overflow**: Tag `reten-api`
- **Discord**: Canal de desarrolladores
