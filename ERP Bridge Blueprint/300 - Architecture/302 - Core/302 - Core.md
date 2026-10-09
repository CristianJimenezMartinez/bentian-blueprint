# Core — v1.0
> **Estado:** Borrador

---

# 302 - Core

> *Especificaciones del motor central (Core).*

---

## 1. Propósito

El Core es el núcleo independiente de ERP Bridge.

Proporciona las capacidades comunes necesarias para conectar sistemas, ejecutar integraciones y automatizar procesos sin contener lógica específica de un ERP, ecommerce, CRM o servicio concreto.

> **Objetivo fundamental:** Un nuevo conector debe poder reutilizar el Core sin tener que reconstruir la plataforma.

---

## 2. Qué es el Core

El Core proporciona las capacidades comunes, mientras que los conectores proporcionan la comunicación específica con cada sistema:

```text
                 ERP BRIDGE CORE
                       │
       ┌───────────────┼────────────────┐
       │               │                │
  Connections        Flows            Events
       │               │                │
       ├───────────────┼────────────────┤
       │               │                │
    Mapping      Transformation       Errors
       │               │                │
       └───────────────┼────────────────┘
                       │
                  Connectors
```

---

## 3. Qué NO es el Core

El Core **NO** debe contener:
- Código específico de Factusol.
- Código específico de WooCommerce.
- Código específico de Shopify.
- Consultas directas contra bases de datos específicas (ej. Microsoft Access).
- Lógica específica de un CRM.
- Credenciales fijas de un proveedor concreto.
- Reglas comerciales propietarias de un cliente único.

### ❌ Ejemplo Incorrecto:
```text
Core
 └── if (system == "Factusol") { ... }
```

### ✅ Ejemplo Correcto:
```text
Core
 └── Connector Interface (Abstracción)

Factusol Connector
 └── Lógica y consultas específicas de Factusol
```

---

## 4. Responsabilidades del Core

### 4.1 Connection Management
Gestionar las conexiones con sistemas externos:
- Crear, validar, activar, desactivar, actualizar y eliminar conexiones.
- Comprobar estado y salud de la conexión (*health checks*).

### 4.2 Connector Management
Gestionar los conectores instalados:
- Descubrimiento y registro de conectores disponibles.
- Control de versiones y compatibilidad.
- Inspección de capacidades expuestas por cada conector.
- Configuración y estado de activación.

### 4.3 Flow Engine
Ejecutar flujos y procesos definidos:
```text
Nuevo pedido ──> Transformar datos ──> Crear pedido en ERP ──> Actualizar stock
```

### 4.4 Event Engine
Gestionar y propagar eventos del sistema (`ORDER_CREATED`, `PRODUCT_UPDATED`, `STOCK_CHANGED`, `CUSTOMER_CREATED`), permitiendo disparar flujos asociados.

### 4.5 Mapping Engine
Gestionar la correspondencia entre esquemas de datos:
```text
Source: product_code ──> Mapping Engine ──> Target: sku
```

### 4.6 Transformation Engine
Modificar y transformar la estructura de datos:
- Modificación y normalización de formatos.
- Combinación y división de campos.
- Conversión de unidades de medida y monedas.
- Aplicación de reglas condicionales.

### 4.7 Validation Engine
Validar la integridad y consistencia de los datos antes de emitirlos al destino:
```text
Pedido ──> Validar [ Cliente ✓ | Productos ✓ | Precio ✓ | Dirección ✗ ] ──> Detener / Alerta
```

---

## 5. Execution Engine (Motor de Ejecución)

Mecanismo central de despacho y control de operaciones:

```text
Trigger ──> Flow ──> Action ──> Connector ──> Result
```

Controla en todo momento: estado, tiempo de ejecución (*latency*), errores, políticas de reintento, resultado y trazabilidad.

---

## 6. Retry Engine (Motor de Reintentos)

Mecanismo automático para la recuperación de errores transitorios:

```text
Request ──> Error ──> Retry #1 ──> Error ──> Retry #2 ──> Success
```

> **Regla:** Debe incorporar límites de reintentos (*backoff exponencial*) para evitar bucles infinitos.

---

## 7. Idempotency (Idempotencia)

Mecanismo para evitar la duplicación de datos o transacciones ante múltiples envíos:
- Si un ecommerce reenvía por error el mismo pedido, el Core detecta que ya fue procesado y evita crear duplicados en el ERP.

---

## 8. State Management (Gestión de Estados)

Control del ciclo de vida de los procesos y tareas:
- `Pending` ──> `Processing` ──> `Success`
- `Pending` ──> `Processing` ──> `Failed` ──> `Retry`

---

## 9. Logging Estructurado

Generación de trazas técnicas para diagnosticar cualquier operación:

```text
[10:32:15] ORDER_CREATED
Source:   WooCommerce
Target:   Factusol
Status:   SUCCESS
Duration: 1.24s
TraceId:  a8f9-4b12
```

---

## 10. Audit Trail (Auditoría)

Registro estructurado e inmutable de operaciones críticas:
- Cambios en configuraciones y credenciales.
- Activación/desactivación de flujos y conectores.
- Gestión de usuarios y permisos.

---

## 11. Security Layer

Gestión de la seguridad transversal:
- Autenticación y autorización basada en roles (RBAC).
- Bóveda de credenciales (*Secrets Vault*).
- Cifrado en tránsito (TLS) y en reposo (AES).
- Aislamiento estricto de clientes (*Tenant Isolation*).

---

## 12. Multi-tenancy

Garantía de aislamiento lógico total entre empresas en entornos cloud/compartidos:

```text
Tenant A ──> Datos A (Aislado)
Tenant B ──> Datos B (Aislado)
Tenant C ──> Datos C (Aislado)
```

*(En despliegues locales este concepto se adapta a la instancia local única).*

---

## 13. Common Data Model (Modelos Canónicos)

Definición de entidades estándar para desacoplar los sistemas entre sí:
- `Product`
- `Customer`
- `Order`
- `Invoice`
- `Stock`
- `Supplier`

---

## 14. Connector Interface (Contrato Común)

```text
Connector
├── Metadata (nombre, versión, icono)
├── Authentication (OAuth, API Key, credenciales)
├── Capabilities (lectura, escritura, eventos)
├── Read (lectura de entidades)
├── Create (creación de entidades)
├── Update (actualización de entidades)
├── Delete (eliminación / baja)
├── Events (escucha / webhooks / polling)
└── Health Check (test de conectividad)
```

---

## 15. Capability Model (Matriz de Capacidades)

El Core consulta dinámicamente qué operaciones soporta cada conector:

| Conector | Read Products | Update Products | Read Stock | Read Orders | Webhooks |
|---|:---:|:---:|:---:|:---:|:---:|
| **Factusol** | ✓ | ✓ | ✓ | ✓ | ✗ (Polling/Agent) |
| **Shopify** | ✓ | ✓ | ✓ | ✓ | ✓ (Webhooks) |

---

## 16. Connector Independence

El Core opera a nivel abstracto (`GetProducts()`, `SyncOrders()`). El detalle de cómo se obtienen los datos (consultando Access vía ODBC, llamando a una API REST GraphQL o leyendo un fichero) es responsabilidad exclusiva del conector.

---

## 17. Local / Cloud Execution

Capacidad de despachar operaciones entre entornos distribuidos:

```text
Cloud Core ──> Dispatch Request ──> Local Agent ──> Factusol (Access Local)
```

---

## 18. Extensibilidad

```text
                    Core
                     │
      ┌──────────────┼──────────────┐
      ↓              ↓              ↓
 Connectors      Workflows        Events
      ↓              ↓              ↓
     SDK        Marketplace         IA
```

---

## 19. Alcance del MVP vs Capacidades Futuras

### ✅ Incluido en el MVP del Core:
- Connection Management.
- Connector Interface estándar.
- Data Models básicos (Productos, Stock, Pedidos).
- Mapping & Transformation básica.
- Execution Engine & Error Handling.
- Retry Engine & Logging estructurado.
- Estado de sincronización y configuración.

### ⏳ Pospuesto para Versiones Posteriores:
- Motor complejo de workflows gráficos.
- Marketplace integrado.
- Asistentes de IA.
- Analytics avanzado de negocio.
- Event Bus distribuido y escalado masivo multi-región.

---

## 20. La Verdadera Prueba del Core

> **El Core será considerado correctamente diseñado cuando el segundo conector pueda construirse reutilizando la mayor parte posible de las capacidades desarrolladas para el primero.**

```text
Conector #1 (Factusol): Construimos Core + Connector Factusol
                     ↓
Conector #2 (Sage):     Reutilizamos Core al 100% ──> Solo desarrollamos la lógica de Sage
```

*(Si para Sage tuviéramos que rehacer autenticación, sincronización, logs, retries o mapping, el Core estaría mal diseñado).*

---

## 21. Evolución por Versiones

```text
V1 (MVP):
Core ──> Connections + Connector Interface + Canonical Data + Mapping + Execution + Retry + Logs

V2:
Core ──> Workflows Avanzados + Events Engine + Advanced Sync + Scheduling

V3:
Core ──> SDK Oficial + Marketplace + IA + Automatización Empresarial Avanzada
```

---

## 22. Regla Final

> **El Core debe ser pequeño, estable y reutilizable.**
>
> **La complejidad específica debe vivir en los conectores y componentes especializados, nunca dentro del núcleo.**
