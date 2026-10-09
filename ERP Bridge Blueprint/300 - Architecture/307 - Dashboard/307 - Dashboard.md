# Dashboard — v1.0
> **Estado:** Borrador

---

# 307 - Dashboard

> *Interfaz visual, panel de control, asistentes de configuración y monitorización.*

---

## 1. Propósito y Separación Arquitectónica

ERP Bridge implementa una separación limpia y estricta entre dos paneles complementarios:

### A. Dashboard Cloud SaaS (`https://bridge.cristianjm.com/dashboard/`)
El panel web central se enfoca en el titular de la cuenta, la administración de licencias multi-puesto y la facturación:
- **Gestión Multi-Clave de Puestos (Multi-Seat):** Visualización de claves de activación (`EB-XXXXX`), asignación de alias (ej. "Tienda Centro", "Almacén"), estado del hardware vinculado (HWID), y revocación/desvinculación para transferir licencias a nuevos ordenadores.
- **Facturación y Suscripciones:** Planes mensual/anual mediante Stripe Checkout y Customer Portal.
- **Supervisión de Flota (Fleet Status):** Semáforo de estado (Online / Offline / En pausa) y versión instalada de cada agente del cliente.
- **Seguridad en Backend:** Toda la lógica de emisión criptográfica de licencias, validación y cupos se procesa de forma hermética en el backend central contra PostgreSQL/Supabase.

### B. Centro de Control Local (`BentianAgent.exe` en `127.0.0.1:39281`)
Toda la configuración técnica y de negocio de la integración reside y se ejecuta en local:
- Selección y pruebas de la base de datos de Factusol (`.accdb`), tarifas, almacenes y series.
- Explorador interactivo de artículos en tiempo real.
- Credenciales y test de WooCommerce REST API.
- Reglas de stock, vigilante de archivos Access y cola de reintentos.
- Registro de eventos (logs) y diagnóstico descargable.

---

## 2. Principio fundamental

ERP Bridge debe ser técnicamente potente por dentro y transparente por fuera. El usuario nunca tiene que configurar parámetros de su red local en la nube; el programa local tiene todo lo necesario para operar de forma autónoma.

---

## 3. Experiencia objetivo

La experiencia principal será:

Entrar
```text
  ↓
```
Añadir sistema
```text
  ↓
```
Conectar
```text
  ↓
```
Elegir qué sincronizar
```text
  ↓
```
Activar
```text
  ↓
```
Funcionando

Objetivo:

Una integración básica debería poder configurarse en aproximadamente 3 minutos cuando los sistemas y credenciales están preparados.

No prometemos que absolutamente cualquier integración pueda hacerse en 3 minutos.

---

## 4. Dashboard principal

La pantalla inicial deberá mostrar el estado general.

Conceptualmente:

ERP Bridge
────────────────────────────────


Buenos días, Empresa X


```text
┌────────────┐ ┌────────────┐ ┌────────────┐
│ Conexiones │ │ Flujos     │ │ Errores    │
│     4      │ │     7      │ │     1      │
└────────────┘ └────────────┘ └────────────┘
```


Conexiones


🟢 Factusol
🟢 WooCommerce
🟢 CRM
🔴 Email


Actividad reciente
──────────────────────────────
Pedido #1234 sincronizado ✓
Producto #782 actualizado ✓
Cliente #543 creado ✓

---

## 5. Navegación

La navegación inicial deberá ser sencilla.

Dashboard
Connections
Flows
Activity
Agents
Connectors
Settings

No necesitamos 30 apartados.

---

## 6. Connections

Aquí el usuario verá todos los sistemas conectados.

Connections


```text
┌──────────────────────────────────┐
│ 🟢 Factusol                      │
│ ERP                              │
│ Connected                        │
└──────────────────────────────────┘
```


```text
┌──────────────────────────────────┐
│ 🟢 WooCommerce                   │
│ Ecommerce                        │
│ Connected                        │
└──────────────────────────────────┘
```


+ Add connection

---

## 7. Añadir conexión

Este debe ser uno de los flujos más importantes de toda la aplicación.

+ Add connection
```text
       ↓
```
What do you want to connect?
```text
       ↓
```
ERP
CRM
Ecommerce
Payments
Other

Después:

Choose your system


Factusol
Sage
Odoo
WooCommerce
Shopify
...

---

## 8. Separación de Responsabilidades y Arquitectura Zero-Knowledge

Conforme a **ADR-006** y **PDR-007**, el sistema separa estrictamente las funciones locales y cloud para garantizar que **Bentian Cloud nunca acceda ni almacene datos de negocio del cliente**:

1. **Centro de Control Local (`BentianAgent.exe` en `127.0.0.1:39281`):**
   * Aloja el 100% de la configuración operativa y técnica:
     * Detección y selección de la base de datos de Factusol (`.accdb`).
     * Credenciales privadas de WooCommerce (Consumer Key / Secret) cifradas con AES-256-GCM.
     * Mapeo de tarifas, almacenes, series y reglas de sincronización.
     * Visor de logs en vivo y cola de reintentos offline.
   * **Privacidad Absoluta:** Ningún archivo de Factusol ni credencial de tienda se transmite jamás a los servidores de Bentian.

2. **Dashboard Cloud SaaS (`bridge.cristianjm.com`):**
   * Aloja exclusivamente la gestión de cuenta, facturación y flota:
     * Consulta de estado de suscripción y facturas fiscales mediante Stripe Customer Portal.
     * Inventario de puestos contratados y activos (vinculados por HWID).
     * **Autoservicio de Hardware (Mudar PC):** Botón de desvinculación para liberar un puesto averiado.
     * Monitorización de latidos (*heartbeat*) agregados de los agentes.

---

## 9. Asistente de Configuración Local (Agent Onboarding Wizard)

El asistente de puesta en marcha no se ejecuta en la web, sino directamente dentro del **Centro de Control Local del Agente**:

```text
Asistente Local de Configuración (127.0.0.1:39281)
●──────●──────○──────○
1. Activación      2. ERP Local        3. Tienda Online     4. Sincronización
(Email + PIN)      (Auto-detectar DB)   (WooCommerce Keys)   (Activar Motor)
```

Esto reduce a cero la carga cognitiva del usuario y garantiza que sus credenciales comerciales nunca salgan de su red local.

---

## 10. Connection Test

Antes de activar una integración:

Testing connection...


✓ Agent connected
✓ Database detected
✓ Authentication successful
✓ Products available
✓ Orders available


Connection ready

---

## 11. Flows

El usuario podrá crear automatizaciones.

Ejemplo:

When:


New order in WooCommerce


```text
        ↓
```


Do:


Create order in Factusol

La interfaz debe evitar código.

---

## 12. Visual Workflow

A largo plazo:

[WooCommerce]


New Order
```text
     │
     ↓
```
[Transform]
```text
     │
     ↓
```
[Factusol]


Create Order

El usuario podrá configurar las acciones mediante interfaz visual.

---

## 13. Mapping

Uno de los puntos más importantes.

Ejemplo:

WooCommerce             Factusol


```text
SKU       ───────────→  Código
Name      ───────────→  Descripción
Price     ───────────→  Precio
Stock     ───────────→  Existencias
```

ERP Bridge debería intentar hacer automáticamente el mapping cuando sea posible.

---

## 14. Automatic Mapping

Objetivo:

Detecting fields...


```text
✓ SKU → Código
✓ Name → Descripción
✓ Price → Precio
✓ Stock → Existencias
```


95% automatically mapped

El usuario solo revisaría excepciones.

Esto es una pieza importante de la promesa de facilidad.

---

## 15. Sync Configuration

El usuario deberá elegir qué quiere sincronizar.

Ejemplo:

Synchronize


☑ Products
☑ Stock
☑ Orders
☑ Customers
☐ Suppliers

Y la dirección:

Products


```text
ERP → Ecommerce
```


Stock


```text
ERP → Ecommerce
```


Orders


```text
Ecommerce → ERP
```

---

## 16. Sync Frequency

Dependiendo de las capacidades del conector:

Real-time
Every 1 minute
Every 5 minutes
Every 15 minutes
Every hour
Manual

Si el sistema soporta webhooks/eventos, podremos ofrecer tiempo real.

Si no, utilizaremos polling.

---

## 17. Activity

El usuario deberá poder ver qué está ocurriendo.

Activity


✓ Order #1024 synced
✓ Product #554 updated
✓ Stock #883 synchronized
✗ Customer #92 failed

---

## 18. Error details

Cuando algo falle:

Customer #92


Synchronization failed


Reason:
Customer email is required.


[View details]
[Retry]

Nunca deberíamos mostrar un stack trace técnico como experiencia principal.

---

## 19. Retry

El usuario podrá reintentar operaciones fallidas.

Synchronization failed


[Retry]

El Core gestionará la lógica técnica de reintentos.

---

## 20. Agents

El Dashboard deberá mostrar los Agents instalados.

Agents


🟢 Oficina Madrid
Version 1.0.3
Connected


🟢 Almacén
Version 1.0.3
Connected


🔴 Oficina Murcia
Version 1.0.2
Offline

---

## 21. Experiencia de Instalación y Activación Tri-Pilar

En lugar de obligar al usuario a emparejar códigos alfanuméricos arbitrarios, la experiencia de activación sigue el modelo **Agent-First**:

```text
Experiencia de Usuario Tri-Pilar:

1. Compra en Stripe ──> Recibe PIN de 6 dígitos (en pantalla de éxito y email)
2. Descarga e instala BentianAgent.exe en el equipo local
3. Introduce su Email y PIN en el Centro de Control Local
4. El Agente detecta su suscripción y muestra sus puestos:
   ┌────────────────────────────────────────────────────────┐
   │ Ferretería García S.L. — Plan Base (3 Puestos)        │
   │                                                        │
   │ [x] Puesto 1: Mostrador (Ocupado por PC-VENTAS)        │
   │ [ ] Puesto 2: Disponible para este ordenador           │
   │ [ ] Puesto 3: Disponible                               │
   │                                                        │
   │      [ VINCULAR ESTE ORDENADOR CON 1 CLIC ]            │
   └────────────────────────────────────────────────────────┘
5. ¡Listo! El HWID queda registrado y el motor empieza a sincronizar.

Puente SSO de 1-Clic:
Desde el menú del Agente o la bandeja del sistema, el botón "Mi Cuenta y Facturas"
abre Microsoft Edge autenticado automáticamente en el Stripe Customer Portal
mediante un token temporal de 60 segundos. Cero contraseñas que recordar.

---

## 22. Connectors

El Dashboard podrá mostrar los conectores disponibles.

Connector Marketplace


ERP
```text
├── Factusol
├── Sage
├── Odoo
```


Ecommerce
```text
├── WooCommerce
├── Shopify
```


CRM
```text
├── HubSpot
├── Salesforce
```

Inicialmente será simplemente un catálogo.

Más adelante puede convertirse en Marketplace.

---

## 23. Connector page

Cada conector deberá mostrar:

Factusol


ERP


✓ Products
✓ Customers
✓ Orders
✓ Stock


Requires:
ERP Bridge Agent


[Connect]

El usuario debe saber qué puede hacer antes de instalarlo.

---

## 24. Settings

Configuraciones generales:

Settings


Organization
Users
Security
API
Billing
Notifications
Agents

La estructura definitiva dependerá del modelo comercial.

---

## 25. API

Los usuarios técnicos podrán gestionar sus API keys.

Developer


API Keys


Production
••••••••••••••


[Create API Key]
[Revoke]

La sección técnica debe estar separada de la experiencia principal.

---

## 26. Roles

El Dashboard deberá poder soportar diferentes usuarios.

Ejemplo:

Owner
Admin
Operator
Viewer
Developer

No todos necesitarán las mismas capacidades.

---

## 27. Onboarding

La primera experiencia será crítica.

Al entrar por primera vez:

Welcome to ERP Bridge


What do you want to connect?


[ERP]
[Online Store]
[CRM]
[Other]

Después:

What do you want to do?


○ Synchronize products
○ Synchronize stock
○ Synchronize orders
○ Synchronize customers

ERP Bridge debería construir la integración a partir de estas respuestas.

---

## 28. Zero-Code

El Dashboard debe perseguir una experiencia no-code para usuarios empresariales.

El usuario selecciona:

WHEN
New Order


```text
↓
```


DO
Create Order


```text
↓
```


IN
Factusol

No escribe:

JavaScript
SQL
REST
JSON

---

## 29. Advanced Mode

Los usuarios técnicos sí podrán acceder a opciones avanzadas.

Simple Mode
```text
        ↓
```
Advanced Mode

Advanced Mode podría permitir:

mappings manuales,
filtros,
transformaciones,
expresiones,
API,
logs detallados.

Esto evita limitar a los desarrolladores sin complicar al usuario normal.

---

## 30. AI Assistant

A largo plazo, el Dashboard podrá incorporar IA.

Ejemplo:

"Quiero que los pedidos de mi tienda entren automáticamente en Factusol y que el stock se actualice cada 5 minutos."

ERP Bridge podría traducirlo a:

Trigger:
WooCommerce Order Created


Action:
Create Factusol Order


Sync:
```text
Factusol → WooCommerce
```


Interval:
5 minutes

El usuario simplemente revisaría:

¿Quieres activar esta automatización?


[Activate]

Esta parte es visión futura, no MVP.

---

## 31. Principio de transparencia

Aunque el sistema sea sencillo, el usuario siempre deberá poder averiguar:

qué está conectado,
qué se sincroniza,
cuándo se sincronizó,
qué falló,
qué datos se están moviendo,
quién modificó la configuración.

La simplicidad no debe convertirse en falta de control.

---

## 32. Mobile

El Dashboard inicialmente estará optimizado para escritorio.

La versión móvil deberá permitir principalmente:

consultar estado,
recibir alertas,
revisar errores,
consultar actividad.

No es necesario diseñar toda la administración para móvil en la primera versión.

---

## 33. MVP

El Dashboard inicial deberá contener:

Dashboard
Connections
Connection Wizard
Flows básicos
Activity
Errors
Agents
Connectors
Settings

No necesitamos inicialmente:

Marketplace completo.
IA.
Analytics avanzado.
Developer Portal.
Billing avanzado.
Constructor visual extremadamente complejo.

---

## 34. Criterio de éxito

Una empresa sin conocimientos técnicos debería poder conectar dos sistemas compatibles y activar una sincronización básica sin necesitar asistencia de un desarrollador.

---

## 35. Regla fundamental

La complejidad de ERP Bridge debe quedar detrás de la interfaz.

El usuario debe ver:

Conectar
```text
→ Configurar
→ Activar
→ Supervisar
```

Mientras por debajo ocurre:

Agent
```text
→ Connector
→ Authentication
→ Mapping
→ Transformation
→ Core
→ Events
→ Sync
→ Retry
→ Logging
```

---
