# Data Model — v1.0
> **Estado:** Borrador

---

# 311 - Data Model

> *Modelo de datos central, entidades de plataforma, relaciones y aislamiento multi-tenant.*

---

## 1. Propósito

Definir las entidades fundamentales de **ERP Bridge**, sus relaciones, atributos y responsabilidades.

El modelo de datos permite a ERP Bridge gestionar múltiples empresas, usuarios, agentes locales, conexiones, conectores, eventos, automatizaciones y procesos de sincronización, garantizando en todo momento **aislamiento estricto de datos y trazabilidad total**.

---

## 2. Principios del Modelo de Datos

1. **Multi-Tenancy:** Todo recurso de una empresa pertenece obligatoriamente a una `Organization`.
2. **Identidad Unívoca:** Cada entidad dispone de un identificador interno inmutable propio de ERP Bridge.
3. **Trazabilidad Integral:** Cada acción se correlaciona con su usuario, organización, conexión, agente, evento y ejecución.
4. **Seguridad & Cifrado:** Las credenciales y secretos nunca se almacenan en texto plano.
5. **Desacoplamiento Canónico:** El Core no depende de estructuras particulares de ningún ERP o Ecommerce.
6. **Extensibilidad:** Capacidad de añadir nuevos conectores sin alterar el esquema relacional central.

---

## 3. Visión General de Entidades (Entity Overview)

```text
ERP Bridge Platform Entities
│
├── Tenancy & Users:    Organization, User, Membership, APIKey, AuditLog
├── Infrastructure:     Connector, Connection, Agent, Credential, Webhook
├── Automation:         Event, Flow, FlowStep, Action
└── Synchronization:    SyncJob, SyncExecution, Mapping, ExternalEntityMapping
```

---

## 4. Organization (Espacio de Trabajo / Tenant)

Representa una empresa o espacio de trabajo independiente dentro de ERP Bridge:

```typescript
interface Organization {
  id: string;             // UUID interno (ej. "org_8f71aa9b")
  name: string;           // Nombre de la empresa (ej. "Empresa ABC, S.L.")
  slug: string;           // Identificador URL amigable (ej. "empresa-abc")
  status: OrganizationStatus; // ACTIVE | SUSPENDED | CANCELLED
  plan: string;           // Plan comercial contratado
  created_at: Date;
  updated_at: Date;
}
```

```text
Organization A (Aislamiento Estricto)
├── Users & Memberships
├── Local Agents
├── Connections
├── Flows
└── Sync Jobs
```

---

## 5. User (Usuario)

Representa a una persona física que interactúa con la plataforma:

```typescript
interface User {
  id: string;             // UUID interno
  email: string;          // Correo único de acceso
  name: string;           // Nombre completo
  status: UserStatus;     // ACTIVE | INVITED | DISABLED
  created_at: Date;
  updated_at: Date;
}
```

> Un `User` puede pertenecer a una o varias organizaciones simultáneamente.

---

## 6. Membership (Pertenencia y RBAC)

Vincula a un `User` con una `Organization` asignándole un rol operativo:

```typescript
interface Membership {
  id: string;
  user_id: string;        // Referencia a User
  organization_id: string;// Referencia a Organization
  role: Role;             // OWNER | ADMIN | OPERATOR | VIEWER | DEVELOPER
  status: MembershipStatus;
  created_at: Date;
}
```

---

## 7. Roles de Plataforma

- **OWNER:** Control total de la organización, facturación y propiedad de datos.
- **ADMIN:** Configuración de conexiones, flujos, agentes e invitación de miembros.
- **OPERATOR:** Gestión operativa del día a día, reintentos de sincronización y supervisión.
- **VIEWER:** Solo lectura de métricas, estados y registros.
- **DEVELOPER:** Gestión técnica de API Keys, webhooks y herramientas de desarrollo.

---

## 8. Connector (Definición de Integración)

Representa el módulo o adaptador para un software específico (definición abstracta):

```typescript
interface Connector {
  id: string;             // ej. "conn_type_factusol"
  name: string;           // "Factusol"
  slug: string;           // "factusol"
  type: ConnectorType;    // LOCAL_DATABASE | CLOUD_REST | HYBRID
  version: string;        // "1.0.4"
  status: ConnectorStatus;// OFFICIAL | VERIFIED | COMMUNITY | DEPRECATED
  capabilities: string[]; // ["products", "stock", "customers", "orders"]
  metadata: Record<string, any>; // Iconos, enlaces a docs, autor
}
```

---

## 9. Connection (Instancia de Conexión)

Representa la configuración e instancia concreta que una empresa conecta con un software:

```typescript
interface Connection {
  id: string;             // UUID (ej. "conn_8812a")
  organization_id: string;// Tenant propietario
  connector_id: string;   // Tipo de conector (ej. Factusol)
  name: string;           // Nombre legible (ej. "Factusol Oficina Central")
  status: ConnectionStatus; // CONNECTED | ERROR | DISCONNECTED | PAUSED
  agent_id?: string;      // ID del Local Agent si es un sistema on-premise
  credential_id: string;  // Referencia a la bóveda cifrada
  configuration: Record<string, any>; // Parámetros específicos (rutas, endpoints)
  created_at: Date;
  updated_at: Date;
}
```

---

## 10. Diferencia Fundamental: Connector vs Connection

```text
Connector (Catálogo) ──> "Cómo conectarse a Factusol" (Código / SDK)
       │
       ├── Connection A ──> "Factusol Empresa ABC, S.L." (Instancia de Empresa A)
       └── Connection B ──> "Factusol Almacén Madrid"    (Instancia de Empresa B)
```

---

## 11. Agent (Agente Local)

Representa una instalación física o virtual del agente en el entorno local del cliente:

```typescript
interface Agent {
  id: string;             // UUID único
  organization_id: string;// Tenant
  name: string;           // ej. "Servidor Central Madrid"
  status: AgentStatus;    // ONLINE | OFFLINE | PENDING_PAIRING | REVOKED
  version: string;        // Versión binaria instalada
  last_seen_at: Date;     // Último latido (heartbeat)
  created_at: Date;
  updated_at: Date;
}
```

---

## 12. Relación de Agentes con Conexiones

Una organización puede disponer de múltiples agentes locales para acceder a sedes distribuidas:

```text
Organization
├── Agent Madrid ──> Connection Factusol Tienda Central
└── Agent Murcia ──> Connection Factusol Almacén Secundario
```

---

## 13. Credential (Bóveda Cifrada de Secretos)

Almacén seguro para tokens OAuth, API Keys, contraseñas y certificados:

```typescript
interface Credential {
  id: string;
  organization_id: string;
  type: CredentialType;   // API_KEY | OAUTH2_TOKEN | BASIC_AUTH | ODBC_SECRET
  encrypted_data: string; // Payload cifrado con AES-256-GCM
  status: CredentialStatus; // VALID | EXPIRED | REVOKED
  created_at: Date;
  updated_at: Date;
}
```

---

## 14. Event (Registro de Eventos)

Hecho ocurrido en la plataforma o recibido de sistemas conectados:

```typescript
interface Event {
  id: string;             // UUID interno (ej. "evt_7712a")
  organization_id: string;// Tenant
  type: string;           // "ORDER_CREATED" | "STOCK_CHANGED" | "SYNC_FAILED"
  source: string;         // "woocommerce" | "factusol" | "system"
  connection_id?: string; // Conexión origen
  agent_id?: string;      // Agente origen
  payload: Record<string, any>; // Datos del evento
  occurred_at: Date;      // Momento del suceso en el origen
  received_at: Date;      // Momento de ingesta en ERP Bridge
  status: EventStatus;    // RECEIVED | PROCESSING | COMPLETED | FAILED | DLQ
}
```

---

## 15. Identidad y Desduplicación de Eventos

```typescript
interface EventTrace {
  internal_id: string;    // "evt_123"
  external_event_id?: string; // ej. "shopify_webhook_9981"
  source_system: string;  // "shopify"
  idempotency_key?: string;
}
```

---

## 16. Flow (Flujo de Automatización)

Define la regla de automatización (*Trigger ──> Acciones*):

```typescript
interface Flow {
  id: string;
  organization_id: string;
  name: string;           // "Sincronizar Pedidos Web a Factusol"
  status: FlowStatus;     // ACTIVE | PAUSED | DRAFT
  trigger_type: string;   // "EVENT" | "SCHEDULE" | "WEBHOOK"
  trigger_config: Record<string, any>;
  created_at: Date;
  updated_at: Date;
}
```

---

## 17. Flow Step (Paso del Flujo)

Cada nodo o acción individual dentro de un `Flow`:

```typescript
interface FlowStep {
  id: string;
  flow_id: string;
  type: FlowStepType;     // ACTION | FILTER | TRANSFORMATION | CONDITION
  position: number;       // Orden secuencial de ejecución (1, 2, 3...)
  configuration: Record<string, any>;
  conditions?: Record<string, any>; // Filtros condicionales (ej. total > 500€)
}
```

---

## 18. Action (Acción Ejecutable)

Catálogo de comandos que ERP Bridge o sus conectores pueden ejecutar:
- `CREATE_ORDER`
- `UPDATE_PRODUCT`
- `UPDATE_STOCK`
- `CREATE_CUSTOMER`
- `SEND_NOTIFICATION`

---

## 19. Sync Job (Definición de Sincronización)

Configuración periódica o continua de sincronización de entidades:

```typescript
interface SyncJob {
  id: string;
  organization_id: string;
  source_connection_id: string;      // ej. Factusol
  destination_connection_id: string; // ej. WooCommerce
  entity: string;                    // "products" | "stock" | "orders" | "customers"
  direction: SyncDirection;          // ONE_WAY_SOURCE_TO_DEST | ONE_WAY_DEST_TO_SOURCE | TWO_WAY
  schedule: string;                  // Cron expression o "REAL_TIME"
  status: SyncJobStatus;             // ACTIVE | PAUSED | ERROR
  configuration: Record<string, any>;
  created_at: Date;
  updated_at: Date;
}
```

---

## 20. Sync Execution (Histórico de Ejecución)

Registro detallado de cada ciclo de sincronización ejecutado:

```typescript
interface SyncExecution {
  id: string;
  sync_job_id: string;
  status: ExecutionStatus; // SUCCESS | FAILED | PARTIAL_SUCCESS | RUNNING
  started_at: Date;
  completed_at?: Date;
  processed_count: number; // Artículos evaluados
  success_count: number;   // Artículos transferidos con éxito
  failed_count: number;    // Errores aislados
  metadata?: Record<string, any>; // Mensajes y detalles de diagnóstico
}
```

---

## 21. Mapping (Mapeo de Campos de Esquema)

Reglas de traducción entre campos origen y destino para un `SyncJob`:

```typescript
interface Mapping {
  id: string;
  sync_job_id: string;
  source_field: string;       // ej. "Código"
  destination_field: string;  // ej. "SKU"
  transformation?: string;    // "uppercase" | "trim" | "currency_eur"
  configuration?: Record<string, any>;
}
```

---

## 22. External Entity Mapping (Correlación de Registros)

Almacena la correspondencia de identificadores entre sistemas para evitar duplicados:

```typescript
interface ExternalEntityMapping {
  id: string;
  organization_id: string;
  entity_type: string;        // "product" | "order" | "customer"
  source_connection_id: string;
  source_identifier: string;  // ej. "1234" en Factusol
  target_connection_id: string;
  target_identifier: string;  // ej. "5678" en WooCommerce
  last_synced_at: Date;
}
```

---

## 23. Webhook (Endpoint de Ingesta)

```typescript
interface Webhook {
  id: string;
  organization_id: string;
  connection_id: string;
  endpoint_path: string;      // ej. "/api/v1/webhooks/wh_98a7b"
  status: WebhookStatus;      // ACTIVE | DISABLED
  secret: string;             // Secret HMAC para verificación de firma
  created_at: Date;
}
```

---

## 24. API Key (Consumo Externo)

```typescript
interface APIKey {
  id: string;
  organization_id: string;
  name: string;               // "Integración CRM Producción"
  key_hash: string;           // Hash seguro de la clave (nunca texto plano)
  key_prefix: string;         // ej. "eb_live_8f71..."
  scopes: string[];           // ["read:products", "write:orders"]
  last_used_at?: Date;
  expires_at?: Date;
  status: APIKeyStatus;       // ACTIVE | REVOKED
  created_at: Date;
}
```

---

## 25. Audit Log (Registro de Auditoría)

```typescript
interface AuditLog {
  id: string;
  organization_id: string;
  user_id?: string;           // Usuario responsable o "SYSTEM"
  action: string;             // "CONNECTION_CREATED", "AGENT_REVOKED"
  resource_type: string;      // "connection" | "flow" | "agent"
  resource_id: string;
  timestamp: Date;
  result: AuditResult;        // SUCCESS | DENIED | FAILED
  metadata: Record<string, any>; // IP, User Agent, parámetros
}
```

---

## 26. Diagrama de Relaciones Principales

```text
User ──(1:N)──> Membership ──(N:1)──> Organization
                                            │
           ┌─────────────────┬──────────────┼─────────────────┬─────────────────┐
           ↓                 ↓              ↓                 ↓                 ↓
        Agents          Connections     Credentials         Flows           Sync Jobs
           │                 │                                │                 │
           │                 ↓                                ↓                 ↓
           └───────────> (Asociación)                     Flow Steps         Mappings
                                                              │                 │
                                                              ↓                 ↓
                                                           Actions       Sync Executions
```

---

## 27. Arquitectura de Conexiones

```text
Organization
     │
     ├──────────────────────────┐
     ↓                          ↓
Connection A (Factusol)    Connection B (WooCommerce)
     │                          │
     └────────────┬─────────────┘
                  ↓
               Sync Job
```

---

## 28. Arquitectura de Eventos

```text
Connection ──> Event ──> Event Bus ──> Flow ──> Action ──> Connection Destino
```

---

## 29. Arquitectura de Sincronización

```text
Source Connection ──> Sync Job ──> Field Mapping ──> Sync Execution ──> Target Connection
```

---

## 30. Aislamiento Multi-Tenant Estricto

> **Clave foránea universal:** Toda tabla/colección con datos de cliente incluye obligatoriamente `organization_id`.
> 
> Las consultas SQL en el Core inyectan automáticamente el filtro `WHERE organization_id = $tenantId` para imposibilitar fugas cruzadas entre empresas.

---

## 31. Identificadores Internos Inmutables (UUID v4 / Prefijos)

ERP Bridge mantiene su propio esquema de IDs independiente del software externo:
- `org_...` para Organizaciones.
- `conn_...` para Conexiones.
- `agent_...` para Agentes.
- `evt_...` para Eventos.
- `job_...` para Sync Jobs.

---

## 32. Eliminación Lógica (Soft Delete)

Para mantener la integridad histórica y registros de auditoría, las entidades clave utilizan borrado lógico (`status = 'DELETED'` y timestamp `deleted_at`) en lugar de destrucción física inmediata en base de datos.

---

## 33. Marcas de Tiempo Estándar

Todas las entidades registran:
- `created_at`: Fecha y hora de alta en UTC (ISO 8601).
- `updated_at`: Última modificación.
- `occurred_at` / `received_at`: Específicos para eventos.

---

## 34. Configuraciones Flexibles en JSONB

Los parámetros específicos de cada software se encapsulan en campos `configuration` tipo JSONB relacional (ej. `database_path` para Factusol vs `store_url` para WooCommerce), evitando alterar las columnas de la tabla `Connection`.

---

## 35. Propiedad y Residencia de Datos (Zero-Knowledge)

```text
Datos de Plataforma Bentian Cloud          Datos de Negocio del Cliente
(Se gestionan en Bentian Cloud)            (Residen 100% en ERP / Ecommerce)
├── Organizations & Subscriptions/Stripe   ├── Catálogo Completo de Productos y Precios
├── Licencias Multi-Puesto (HWID & Seats)  ├── Base de Datos de Clientes y NIFs
├── Registro de Agentes Activos            ├── Ficheros Contables y Facturas
└── Historial de Activaciones y Versiones  └── Pedidos y Direcciones de Envío
```

Las credenciales de tienda y rutas de Factusol residen localmente en el PC del cliente (`agent-config.json` cifrado con AES-256-GCM derivado del HWID) y **nunca se envían a la nube de Bentian**.

---

## 36. Arquitectura Zero-Knowledge y Cero Almacenamiento Central

Bentian **NO ingesta, copia, almacena ni procesa datos de negocio del cliente en su infraestructura cloud**.

Toda la sincronización de catálogo, stock y pedidos se procesa en el borde (*Edge Processing*) directamente entre el Agente Local (`BentianAgent.exe`) y el canal de venta online. La nube de Bentian actúa exclusivamente como proveedor de validación criptográfica de licencias, portal de facturación de Stripe (SSO) y repositorio de binarios de actualización.

---

## 37. Entidades para Fases Futuras

Reservadas para evolución comercial:
- `MarketplaceApp` & `MarketplaceReview`
- `Subscription` & `UsageRecord`
- `AIAgent` & `AITask`
- `WorkflowTemplate`
- `Team`

---

## 38. Entidades Prioritarias del MVP

```text
MVP Core Entities
├── Organization
├── User
├── Membership
├── Connector
├── Connection
├── Agent
├── Credential
├── Event
├── Flow & FlowStep
├── Action
├── SyncJob & SyncExecution
├── Mapping & ExternalEntityMapping
└── AuditLog
```

---

## 39. Criterio de Éxito

> **El modelo de datos debe permitir registrar un nuevo conector y crear conexiones para 1.000 clientes sin modificar ninguna de las tablas o entidades nucleares del Core.**

---

## 40. Regla Fundamental

> **ERP Bridge modela conexiones y relaciones entre sistemas; no se convierte en una copia redundante de cada ERP que conecta.**
