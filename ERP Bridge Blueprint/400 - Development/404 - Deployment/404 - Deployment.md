# Deployment — v1.0
> **Estado:** Borrador

---

# 404 - Deployment

> *Infraestructura, topología de despliegue, entornos, hosting y distribución del agente local.*

---

## 1. Propósito

Definir dónde y cómo se ejecutan, despliegan y actualizan los diferentes componentes de ERP Bridge.

El sistema debe poder comenzar con una infraestructura sencilla y evolucionar progresivamente sin obligar a introducir complejidad innecesaria.

---

## 2. Principio fundamental

La infraestructura debe ser tan sencilla como sea posible, pero sin limitar la arquitectura del producto.

ERP Bridge debe poder ser desarrollado, desplegado y mantenido inicialmente por una sola persona.

---

## 3. Componentes

ERP Bridge estará compuesto conceptualmente por:

ERP Bridge
```text
│
├── Dashboard
├── API
├── Core
├── Agent
├── Connectors
├── Database
└── Infrastructure
```

---

## 4. Dashboard

El Dashboard será la interfaz web desde la que los usuarios gestionarán ERP Bridge.

Tecnología inicial:

Angular
TypeScript

Responsabilidades:

Login.
Organizations.
Connections.
Connectors.
Agents.
Flows.
Synchronization.
Events.
Logs.
Configuration.

---

## 5. API

La API será el punto de comunicación entre Dashboard, Agent y servicios externos.

Tecnología inicial:

Node.js
TypeScript

Responsabilidades:

Authentication
Authorization
Organizations
Connections
Agents
Flows
Events
Synchronization
API access

---

## 6. Core

El Core contiene la lógica principal de ERP Bridge.

No deberá depender directamente de:

Factusol
WooCommerce
Sage
Shopify

El Core trabajará mediante contratos y abstracciones comunes.

---

## 7. Connectors

Los Connectors implementarán las integraciones con sistemas externos.

Ejemplo:

Connectors
```text
│
├── Factusol
├── WooCommerce
├── Shopify
├── Sage
└── ...
```

Cada Connector deberá estar aislado.

---

## 8. Agent

El Agent es un componente instalado localmente en el ordenador o servidor del cliente.

Su función principal es permitir que ERP Bridge interactúe con sistemas que no están expuestos directamente a Internet.

Ejemplo:

Empresa
```text
│
├── Factusol
│     └── Access Database
│
└── ERP Bridge Agent
          │
          │ Secure Connection
          ↓
```
       ERP Bridge

---

## 9. Por qué existe el Agent

Muchos sistemas empresariales funcionan dentro de la red local.

Por ejemplo:

Internet
   X
   │
Firewall
   │
LAN
   │
Factusol

ERP Bridge no deberá obligar al cliente a:

abrir puertos,
exponer su base de datos,
subir su ERP a Internet.

El Agent actúa como puente.

---

## 10. Agent Architecture

El Agent deberá ser pequeño y especializado.

Agent
```text
│
├── Connection Manager
├── Connector Runtime
├── Local Communication
├── Authentication
├── Heartbeat
├── Job Execution
└── Update Manager
```

---

## 11. Agent Runtime

Inicialmente:

Node.js
TypeScript

El Agent deberá poder ejecutarse como proceso local.

Posteriormente podrá empaquetarse como aplicación instalable.

---

## 12. Local Installation

El objetivo será que una empresa pueda realizar:

Download
```text
 ↓
```
Install
```text
 ↓
```
Login / Pair
```text
 ↓
```
Select ERP
```text
 ↓
```
Connect

sin necesidad de conocimientos técnicos avanzados.

---

## 13. Agent Pairing

Un Agent deberá vincularse a una Organization de forma segura.

Conceptualmente:

Dashboard
```text
   ↓
```
Generate Pairing Code
```text
   ↓
```
Agent
```text
   ↓
```
Pair
```text
   ↓
```
Authenticated Agent

El mecanismo definitivo se definirá en Security.

---

## 14. Agent Identity

Cada Agent tendrá una identidad única.

Agent
```text
├── id
├── organization_id
├── version
├── status
└── last_seen_at
```

---

## 15. Agent Communication

El Agent deberá mantener una comunicación segura con ERP Bridge.

El protocolo definitivo se decidirá durante la implementación.

Las opciones podrán incluir:

HTTPS
WebSocket
WebSocket over TLS

No se implementarán múltiples protocolos inicialmente sin necesidad.

---

## 16. Heartbeat

El Agent deberá informar periódicamente de que continúa disponible.

Conceptualmente:

Agent
```text
 ↓
```
Heartbeat
```text
 ↓
```
ERP Bridge

Esto permitirá mostrar:

ONLINE
OFFLINE
UNKNOWN

en el Dashboard.

---

## 17. Offline Agent

Si el Agent pierde conexión a Internet:

Internet
   X
Agent

deberá detectar la pérdida de conexión.

Cuando Internet vuelva:

Internet
   ✓
Agent
```text
   ↓
```
Reconnect
```text
   ↓
```
ERP Bridge

---

## 18. Backend Hosting

El backend Node.js deberá ejecutarse en infraestructura que permita:

procesos persistentes (para `SyncScheduler` y `FlowEngine`),
HTTPS,
servicio estático de actualizaciones del agente (`/releases`),
acceso a PostgreSQL,
escalado futuro.

La arquitectura de despliegue seleccionada:
- **Contenedor Docker Persistente:** `apps/api` empaquetada como imagen Docker monolítica en fase MVP.
- **Entorno de Ejecución:** PaaS administrado (Railway / Render) para validación rápida, o servidor VPS de alto rendimiento y bajo coste (ej. Hetzner Cloud CX22 en Alemania/Finlandia, ~3,79 €/mes).
- **Protección Edge y Red:** Cloudflare Tunnel (`cloudflared`) o DNS Proxy por delante de la API, proporcionando terminación SSL automática gratuita, mitigación de ataques y ocultación de la IP pública del servidor backend.

---

## 19. Database Hosting

PostgreSQL deberá ejecutarse en un servicio gestionado siempre que resulte económicamente razonable.

Esto reduce la carga operacional de:

backups,
actualizaciones,
alta disponibilidad,
mantenimiento.

### Implementación Seleccionada: Supabase (PostgreSQL Gestionado)

Para cumplir la **Golden Rule (§72)** y garantizar que un Solo Founder pueda operar la infraestructura sin desatender el negocio:

1. **Plataforma:** **Supabase Cloud** en región europea (Frankfurt `eu-central-1`).
2. **Compatibilidad Estricta:** Supabase es PostgreSQL 15/16 estándar. El Core de ERP Bridge utiliza el driver oficial `pg` de Node.js, y las migraciones SQL (`001`, `002`, `003`) se ejecutan de forma nativa e idéntica.
3. **Conexión:** Se realiza mediante la variable de entorno `DATABASE_URL` apuntando al *Supavisor Pooler* (Session Mode en puerto `5432` o Transaction Mode en puerto `6543`), con flag SSL habilitado y soporte nativo para redes IPv4 e IPv6.
4. **Cero Mantenimiento Operativo:**
   - **Backups Automáticos Diarios:** Copias de seguridad en la nube con capacidad de restauración en 1 clic.
   - **Supabase Studio (Consola Web):** Interfaz gráfica web segura para consultar y auditar en caliente licencias (`licenses`), agentes activos (`agents`), correlación de IDs (`external_entity_mappings`) y ejecuciones (`sync_executions`) desde cualquier navegador o dispositivo móvil sin necesidad de herramientas de escritorio como pgAdmin.
5. **Conmutación Transparente:** La arquitectura permite alternar entre PostgreSQL local (Docker `bentian-postgres:5434`) en desarrollo y Supabase en producción simplemente alternando la variable `DATABASE_URL` en el archivo `.env`.

---

## 20. Frontend Hosting

El Dashboard Angular podrá desplegarse en un servicio de hosting estático/CDN.

La prioridad será:

Low Cost
+
High Availability
+
Easy Deployment

### Implementación Seleccionada: Cloudflare Pages

El Dashboard (`apps/dashboard`) se compila a ficheros estáticos HTML/JS/CSS (`ng build`) y se sirve en el subdominio unificado `bridge.cristianjm.com/dashboard/` o Cloudflare Pages:
- Distribución con certificado SSL automático en `bridge.cristianjm.com`.
- Desacoplamiento total entre el tráfico de navegación web y los recursos de cómputo del backend API.

---

## 21. API Domain

La API deberá utilizar un dominio independiente o subdominio.

Conceptualmente:

app.example.com
api.example.com

La estructura definitiva se definirá durante la implementación.

---

## 22. Dashboard Domain

El Dashboard utilizará un dominio o subdominio separado del API.

Ejemplo:

app.erpbridge.example

---

## 23. Agent Connection

El Agent no deberá necesitar una IP pública del cliente.

La conexión deberá iniciarse desde el entorno del cliente hacia ERP Bridge siempre que sea posible.

Esto simplifica:

firewalls,
NAT,
configuración de routers,
seguridad.

---

## 24. Factusol Deployment Model

Factusol representa un caso especialmente importante.

Arquitectura:

```text
┌─────────────────────────────┐
│        CLIENTE              │
│                             │
│  Factusol                   │
│      │                      │
│      ↓                      │
│  Access Database            │
│      ↑                      │
│      │                      │
│  ERP Bridge Agent           │
└──────┼──────────────────────┘
       │
       │ Secure outbound connection
       ↓
┌─────────────────────────────┐
│       ERP BRIDGE            │
│                             │
│ API                         │
│ Core                        │
│ Sync Engine                 │
│ PostgreSQL                  │
└─────────────────────────────┘
```

ERP Bridge no necesita alojar la base de datos Access del cliente.

---

## 25. Zero Database Upload

El modelo de ERP Bridge no obligará al cliente a subir su base de datos completa.

Especialmente en sistemas como Factusol:

Access
```text
   ↓
```
Agent
```text
   ↓
```
Required Data
```text
   ↓
```
ERP Bridge

El sistema deberá procesar únicamente la información necesaria.

---

## 26. Local Data Access

El Connector de Factusol deberá ejecutarse donde tenga acceso a:

.accdb

o al mecanismo local necesario para interactuar con Factusol.

El servidor central no deberá asumir acceso directo a la base de datos.

---

## 27. Deployment Environments

ERP Bridge tendrá conceptualmente:

Development
Testing
Staging
Production

Durante el MVP podrán simplificarse algunos entornos.

---

## 28. Development

Utilizado por Cristian y herramientas de IA.

Incluye:

Local Angular
Local Node
Local PostgreSQL
Local Agent

cuando sea necesario.

---

## 29. Testing

Entorno destinado a pruebas automatizadas y manuales.

Debe permitir ejecutar pruebas sin afectar a producción.

---

## 30. Staging

Representará una versión cercana a producción.

Se utilizará para validar cambios importantes antes del despliegue definitivo.

---

## 31. Production

Entorno utilizado por clientes reales.

Debe estar separado de:

Development
Testing

---

## 32. Configuration

Cada entorno deberá disponer de su propia configuración.

Ejemplo:

Development
DATABASE_URL=...


Testing
DATABASE_URL=...


Production
DATABASE_URL=...

Los valores reales nunca deberán almacenarse en Git.

---

## 33. Secrets

Los secretos de producción deberán almacenarse mediante mecanismos seguros proporcionados por la infraestructura.

Nunca deberán estar:

en Git
en código
en Dockerfile
en logs

---

## 34. Database Migrations

Las migrations se ejecutarán de forma controlada.

Proceso conceptual:

Deploy
```text
 ↓
```
Migration Check
```text
 ↓
```
Migration
```text
 ↓
```
Application Start

---

## 35. Database Backups

Producción deberá disponer de backups automáticos.

Los backups deberán permitir recuperar la información en caso de:

error humano,
fallo de infraestructura,
corrupción,
eliminación accidental.

---

## 36. Backup Testing

Un backup no se considerará suficiente simplemente porque exista.

Cuando el producto alcance suficiente criticidad deberán realizarse pruebas de restauración.

---

## 37. Logging

Los servicios deberán generar logs centralizados cuando sea necesario.

Los logs deberán permitir investigar:

Connection failures
Sync failures
Agent failures
API errors
Authentication errors

---

## 38. Monitoring

ERP Bridge deberá poder monitorizar:

API Health
Database Health
Agent Connectivity
Sync Jobs
Error Rate
Resource Usage

La infraestructura concreta se definirá posteriormente.

---

## 39. Health Checks

Cada servicio importante deberá disponer de un mecanismo de health check.

Ejemplo:

GET /health

---

## 40. Deployment Process

Proceso objetivo:

Developer / AI
```text
      ↓
```
Git
```text
      ↓
```
CI
```text
      ↓
```
Tests
```text
      ↓
```
Build
```text
      ↓
```
Staging
```text
      ↓
```
Verification
```text
      ↓
```
Production

---

## 41. Rollback

Todo despliegue importante deberá poder revertirse a una versión anterior.

El mecanismo dependerá de la infraestructura elegida.

---

## 42. Version Compatibility

El sistema deberá tener en cuenta la compatibilidad entre:

Dashboard
API
Agent
Connector
Database

Especialmente entre Agent y API.

---

## 43. Agent Updates

El Agent deberá poder actualizarse de forma controlada.

Objetivo futuro:

New Version Available
```text
        ↓
```
Download
```text
        ↓
```
Verify
```text
        ↓
```
Install
```text
        ↓
```
Restart
```text
        ↓
```
Connected

La actualización automática completa podrá introducirse después del MVP.

---

## 44. Connector Updates

Los Connectors deberán poder evolucionar independientemente cuando sea posible.

Ejemplo:

Factusol Connector
v1
v1.1
v2

La compatibilidad deberá gestionarse mediante versiones.

---

## 45. No Forced ERP Migration

El deployment de ERP Bridge nunca deberá requerir que el cliente cambie su ERP.

Ejemplo:

Cliente
```text
   ↓
```
Factusol actual
```text
   ↓
```
ERP Bridge Agent

---

## 46. No Public Database Exposure

ERP Bridge no deberá requerir como arquitectura normal:

Internet
```text
   ↓
```
Public Access Database

Esto sería contrario a los principios de seguridad del producto.

---

## 47. Infrastructure Simplicity

Durante el MVP se evitará introducir:

Kubernetes.
Service Mesh.
Microservicios distribuidos.
Clusters complejos.
Infraestructura multi-región.

No porque sean malas tecnologías, sino porque no son necesarias para el primer producto.

---

## 48. MVP Infrastructure

La infraestructura inicial deberá poder representarse aproximadamente como:

                 INTERNET
```text
                    │
          ┌─────────┴─────────┐
          │                   │
```
       Dashboard             API
       Angular             Node.js
```text
          │                   │
          └─────────┬─────────┘
                    │
```
                 PostgreSQL
                    │
             Secure Connection
                    │
                  Agent
                    │
                 Cliente
                    │
                 ERP/CRM

---

## 49. Local Development Architecture

Durante desarrollo:

Developer PC
```text
│
├── Angular
├── Node.js
├── PostgreSQL
└── Agent
```

La arquitectura deberá poder ejecutarse localmente sin depender constantemente de producción.

---

## 50. Containerization

Docker podrá utilizarse para facilitar entornos reproducibles.

Especialmente para:

PostgreSQL
Backend
Testing

Pero no será obligatorio introducir Docker en todos los componentes desde el primer día.

---

## 51. Infrastructure as Code

Cuando la infraestructura alcance suficiente complejidad podrá utilizarse Infrastructure as Code.

Durante el MVP no será necesario introducir herramientas avanzadas si aumentan innecesariamente la complejidad.

---

## 52. Cost Control

La infraestructura deberá diseñarse teniendo en cuenta que inicialmente ERP Bridge será gestionado por una sola persona.

Prioridad:

Low Fixed Cost
+
Predictable Billing
+
Easy Maintenance

---

## 53. Scaling

La arquitectura deberá poder evolucionar desde:

Small Deployment

hacia:

Multiple Instances
Multiple Workers
Multiple Agents
Multiple Regions

cuando exista una necesidad real.

---

## 54. Horizontal Scaling

El backend deberá evitar depender de estado almacenado exclusivamente en memoria cuando dicho estado necesite compartirse entre múltiples instancias.

---

## 55. Persistent State

El estado importante deberá almacenarse en sistemas persistentes apropiados.

Principalmente:

PostgreSQL

---

## 56. Queue / Background Jobs

Las tareas largas o asíncronas podrán utilizar workers o sistemas de cola cuando sea necesario.

Ejemplos:

Large Sync
Event Processing
Scheduled Jobs
AI Tasks

Durante el MVP se podrá utilizar una solución sencilla.

---

## 57. Scheduled Jobs

Las sincronizaciones programadas deberán ejecutarse mediante un mecanismo fiable.

Ejemplo:

Every 5 minutes
```text
       ↓
```
Sync Job
```text
       ↓
```
Execution

---

## 58. No Long HTTP Requests

Las operaciones largas no deberán depender de una única petición HTTP que permanezca abierta durante minutos.

Preferentemente:

POST /sync
```text
      ↓
```
Job Created
```text
      ↓
```
Background Processing
```text
      ↓
```
Status

---

## 59. Deployment Observability

Después de un despliegue deberán poder identificarse:

Version
Commit
Environment
Deployment time
Status

---

## 60. Production Access

El acceso administrativo a producción deberá estar restringido.

No se deberán compartir credenciales administrativas entre personas.

---

## 61. Security

Todo tráfico externo deberá utilizar:

HTTPS / TLS

El Agent deberá establecer conexiones seguras con ERP Bridge.

---

## 62. Firewall

La arquitectura deberá minimizar la necesidad de abrir puertos en las instalaciones del cliente.

Preferentemente:

```text
Agent → ERP Bridge
```

en lugar de:

```text
Internet → Agent
```

---

## 63. Client Network

ERP Bridge deberá funcionar en redes empresariales habituales con:

NAT.
Firewall.
Proxy cuando sea posible.
IP dinámica.

---

## 64. Deployment Documentation

Cada componente deberá disponer de instrucciones para:

Install
Configure
Run
Update
Troubleshoot
Uninstall

---

## 65. Agent Installation Experience

El objetivo final será:

1. Descargar Agent
2. Instalar
3. Vincular empresa
4. Seleccionar Connector
5. Configurar conexión
6. Comprobar conexión
7. Activar

La complejidad técnica deberá quedar oculta al usuario final.

---

## 66. Customer Support

El sistema deberá proporcionar suficiente información para diagnosticar problemas sin necesitar acceso remoto permanente al ordenador del cliente.

Ejemplos:

Agent version
Connection status
Last heartbeat
Last sync
Last error
Connector version

---

## 67. Diagnostics

El Agent deberá poder proporcionar información diagnóstica segura.

Nunca deberá incluir secretos.

---

## 68. Failure Recovery

Cuando sea posible, el sistema deberá recuperarse automáticamente de:

Temporary network failure
Temporary API failure
Agent restart
Server restart
Connector timeout

---

## 69. Disaster Recovery

La estrategia deberá proteger principalmente:

Organizations
Connections
Flows
Mappings
Events
Sync history
Configuration

---

## 70. MVP Deployment Priorities

Durante el MVP:

1. Simple deployment
2. Low cost
3. Security
4. Backups
5. Automated builds
6. Easy rollback
7. Monitoring basics

---

## 71. Future Deployment

En etapas posteriores podrán introducirse:

Horizontal scaling
Workers
Queues
Advanced monitoring
Multi-region
Automatic Agent updates
Advanced deployment strategies

---

## 72. Golden Rule

ERP Bridge debe poder desplegarse y mantenerse sin necesitar un equipo DevOps dedicado.

---

## 73. Objetivo final

La infraestructura deberá permitir:

                 ERP BRIDGE
```text
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
```
   Dashboard        API         PostgreSQL
```text
                                    │
                                    │
```
                              Secure Channel
```text
                                    │
                  ┌─────────────────┴────────────────┐
                  ↓                                  ↓
```
               Agent A                            Agent B
```text
                  ↓                                  ↓
```
              Empresa A                          Empresa B
```text
                  ↓                                  ↓
```
                 ERP                                CRM

permitiendo que miles de empresas puedan utilizar el sistema sin que la arquitectura tenga que ser rediseñada desde cero.

---
