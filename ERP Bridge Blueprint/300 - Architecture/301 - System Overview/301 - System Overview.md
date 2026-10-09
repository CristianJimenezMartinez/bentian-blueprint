# System Overview — v1.0
> **Estado:** Borrador

---

# 301 - System Overview

> *Visión general de la arquitectura conceptual del sistema.*

---

## 1. Propósito

Definir la arquitectura conceptual de ERP Bridge y los principales componentes que forman la plataforma.

La arquitectura deberá permitir conectar sistemas locales y cloud, incorporar nuevos conectores sin modificar el núcleo y evolucionar desde un primer caso de uso hacia una plataforma de integración empresarial.

---

## 2. Principios Arquitectónicos

La arquitectura deberá cumplir los siguientes principios:

- **Core independiente de los conectores.**
- **Conectores modulares e intercambiables.**
- **Soporte para entornos local, cloud e híbrido.**
- **Los sistemas del cliente no deben tener que modificarse.**
- **Los datos deben permanecer bajo el control del cliente siempre que sea posible.**
- **Seguridad desde el diseño (Security by Design).**
- **Tolerancia a errores y recuperación segura.**
- **Observabilidad total.**
- **Escalabilidad progresiva.**
- **Mantenibilidad:** La arquitectura debe poder mantenerse por un equipo pequeño.

---

## 3. Arquitectura Conceptual

```text
                         ERP BRIDGE
                              │
             ┌────────────────┼────────────────┐
             │                │                │
          Dashboard          API             Core
             │                │                │
             └────────────────┼────────────────┘
                              │
                  ┌───────────┴───────────┐
                  │                       │
             Local Agent              Cloud
                  │                       │
             ┌────┴────┐             ┌────┴────┐
             │         │             │         │
          Factusol    Otros       Ecommerce   CRM
          /Access     ERPs        /APIs       /APIs
                  │                       │
                  └───────────┬───────────┘
                              │
                         Connectors
```

> *Esta representación es conceptual, no una arquitectura técnica definitiva.*

---

## 4. ERP Bridge Core

El Core es el componente central, responsable de proporcionar capacidades comunes que no pertenecen a ningún sistema concreto:

### Responsabilidades Potenciales:
- Gestión de conexiones.
- Gestión de conectores.
- Ejecución de flujos.
- Eventos.
- Transformación de datos.
- Reglas.
- Sincronización.
- Gestión de errores y reintentos.
- Logs y auditoría.
- Seguridad y configuración.

> **Regla fundamental:** El Core **NO** debe contener lógica específica de Factusol, Shopify, WooCommerce, Sage, etc.

---

## 5. Connectors

Los conectores encapsulan la lógica específica de cada sistema:

```text
                ERP Bridge Core
                       │
 ┌─────────────┬───────┴───────┬─────────────┐
 │             │               │             │
Factusol      Sage        WooCommerce     Shopify
Connector   Connector      Connector     Connector
```

Cada conector deberá proporcionar una interfaz común al Core.

### Interfaz Conceptual de Conector:
```text
Connector
├── connect()
├── authenticate()
├── read()
├── create()
├── update()
├── delete()
└── test()
```

*(Los métodos definitivos se decidirán al diseñar el SDK/Core).*

### Estrategias de Conectividad por Connector
Un Connector puede implementar internamente diferentes mecanismos de adaptación según la topología del sistema:

```text
API CLOUD
Cliente → API Cloud → ERP Bridge

RECURSO LOCAL
Cliente → Local Agent → ERP Bridge
```

1. **API Cloud Adapter:** Comunicación directa vía API (REST, SOAP o API oficial del fabricante) para servicios en la nube o endpoints accesibles.
2. **Local Agent Adapter:** Ejecución delegada a través del Local Agent cuando el Connector necesita acceder a recursos locales, software legacy o bases de datos on-premise (Access, ODBC, SQL Server local).

Bajo ningún concepto la infraestructura cloud de ERP Bridge se conecta directamente a una base de datos local del cliente sin intermediación. Todo acceso a recursos locales se realiza a través del Local Agent para garantizar:
* Conexiones salientes exclusivas (*outbound-only*).
* Ausencia de apertura de puertos entrantes o reglas NAT en el firewall del cliente.
* No exposición de bases de datos locales a Internet.
* Transferencia mínima de datos (solo deltas).
* Procesamiento local en el extremo (*edge processing*).

El Core interactúa únicamente a través de la interfaz canónica del Connector, permaneciendo agnóstico al mecanismo de transporte físico subyacente.

---

## 6. Local Agent (Centro de Control Local First-Local)

El Local Agent (`BentianAgent.exe`) es un ejecutable nativo de Windows (compilado en subsistema GUI `0x0002` sin consola negra/CMD) que reside en el equipo del cliente donde se encuentra Factusol.

Bajo la filosofía **First-Local**, el Agente no es un mero visor pasivo ni un bridge ciego, sino el **CENTRO DE CONTROL COMPLETO DE LA INTEGRACIÓN**:

- **Autonomía Operativa 100% Local:** Toda la configuración del negocio reside en el archivo local `agent-config.json` cifrado y protegido.
- **Configuración de Factusol ERP:** Selección de base de datos (`.accdb`), detección automática de empresas instaladas, selector de tarifa de venta (`F_TAR`), selector de almacén (`F_ALM`), series de pedidos y facturación, y explorador interactivo de artículos en vivo.
- **Configuración de WooCommerce:** Endpoint de la tienda, credenciales REST API (`ck_...`, `cs_...`), test de conexión con cálculo de latencia y mapeo de estados de pedidos.
- **Reglas de Sincronización y Motor Reactivo:** Vigilante de archivos en tiempo real (`AccdbFileWatcher`), estabilización por *debounce*, intervalos periódicos, sincronización selectiva (stock, precios, descripciones) y *buffer* de stock de seguridad.
- **Historial, DLQ y Diagnóstico:** Historial de ejecuciones, cola de reintentos quirúrgicos por bloqueos de Access y generador de informes de diagnóstico técnico para soporte.
- **Interfaz Embebida:** Micro-servidor HTTP local en loopback (`127.0.0.1:39281`) que sirve una SPA moderna de alto rendimiento sin requerir NodeJS instalado en la máquina del usuario.

```text
┌──────────────────────── EQUIPO LOCAL (EMPRESA) ────────────────────────┐
│                                                                        │
│   Factusol ERP (.accdb) <──── (OLEDB / ADODB) ─────┐                   │
│                                                     │                   │
│   BentianAgent.exe [SUBSYSTEM_GUI] ─────────────────┤                   │
│   ├── Servidor GUI Local (127.0.0.1:39281)          │                   │
│   ├── Centro de Control: Factusol + WooCommerce + Sync + Logs          │
│   ├── Vigilante Reactivo (AccdbFileWatcher)         │                   │
│   └── Almacén Seguro Local (DPAPI / AES-256-GCM)   │                   │
│                                                     │                   │
│   WooCommerce REST API <── (HTTPS Outbound) ────────┘                   │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │
                    (Heartbeat & Licencia Outbound)
                                   │
                                   ↓
┌────────────────────── CLOUD SAAS (CENTRAL) ────────────────────────────┐
│                                                                        │
│   Bentian Cloud API + Dashboard (https://bridge.cristianjm.com)        │
│   ├── Facturación y Suscripciones (Stripe)                             │
│   ├── Gestión Multi-Clave de Puestos (Multi-Seat EB-XXXXX)             │
│   ├── Desvinculación y Transferencia de HWID entre equipos             │
│   └── Telemetría de Flota (Estado Online / Offline de agentes)         │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 7. Cloud (Gestión Central de Claves y Flota)

El componente Cloud se enfoca exclusivamente en la administración de la cuenta del cliente y el licenciamiento multi-puesto:
- **Gestión Multi-Licencia:** Un cliente corporativo puede adquirir 1, 3 o 10 licencias para diferentes tiendas o puestos. El dashboard Cloud le permite generar nuevas claves (`EB-XXXXX`), asignarles alias descriptivos (ej. "Almacén Central", "Caja Tienda 1"), revocar claves o desvincular un HWID para transferir la licencia a un PC nuevo.
- **Facturación Stripe:** Gestión de suscripciones recurrentes, actualización de tarjetas de crédito y descarga de facturas fiscales.
- **Monitorización de Flota (Fleet Overview):** Vista agregada del estado operativo de los puestos del cliente (En línea / Desconectado / Versión instalada / Último latido).
- **Auto-Actualizador Central:** Distribución firmada de actualizaciones seguras mediante manifiestos Ed25519.

---

## 8. Local Deployment (Despliegue Local)

ERP Bridge podrá funcionar en un entorno completamente local cuando el caso de uso lo requiera:

```text
┌─────────────── EMPRESA ───────────────┐
│                                       │
│ ERP                                   │
│  │                                    │
│  ↓                                    │
│ ERP Bridge                            │
│  │                                    │
│  ↓                                    │
│ Base de datos / sistemas internos     │
│                                       │
└───────────────────────────────────────┘
```

---

## 9. Cloud Deployment (Despliegue Cloud)

Para arquitecturas donde todos los sistemas disponen de APIs accesibles:

```text
Usuario ──> Dashboard ──> ERP Bridge Cloud ──> APIs / SaaS / Ecommerce
```

---

## 10. Hybrid Deployment (Despliegue Híbrido)

El modelo que combina lo mejor de ambos mundos para sistemas que no pueden exponerse a Internet:

```text
             EMPRESA
                │
        ┌───────┴───────┐
        │               │
    ERP local       Sistemas cloud
        │               │
        ↓               ↓
   Local Agent      Internet/API
        │               │
        └───────┬───────┘
                ↓
           ERP Bridge
```

---

## 11. Data Flow (Flujo de Datos)

El flujo conceptual de procesamiento de datos:

```text
Source ──> Connector ──> Normalize ──> Transform ──> Validate ──> Core ──> Connector ──> Target
```

### Ejemplo Concreto:
```text
Factusol
   ↓
Factusol Connector
   ↓
Modelo común (Normalización)
   ↓
Transformación de datos
   ↓
WooCommerce Connector
   ↓
WooCommerce
```

---

## 12. Modelo Común de Datos (Common Data Model)

Uno de los pilares arquitectónicos esenciales: evitar la necesidad de crear integraciones punto a punto entre todos los sistemas.

### ❌ Integración en Malla (Punto a Punto No Escalable):
```text
Factusol ↔ WooCommerce | Factusol ↔ Shopify | Factusol ↔ Sage | Factusol ↔ Odoo
```

### ✅ Integración en Estrella (Modelo Común Escalable):
```text
Factusol ──┐
Sage     ──┼──> Modelo Común ERP Bridge ──┼──> WooCommerce
Odoo     ──┘                              └──> Shopify
```

```text
Factusol Product ──┐
                   ├──> Product (Modelo Común) <── Shopify Product
WooCommerce Product ──┘
```

---

## 13. Event-Driven Architecture

Capacidad para reaccionar a eventos en tiempo real:

```text
ORDER_CREATED (Evento) ──> ERP Bridge ──> Workflow ──> CREATE_ORDER (Acción) ──> ERP
```

---

## 14. Synchronization Engine (Motor de Sincronización)

Mecanismo común del Core para gestionar:
- Qué se sincroniza y en qué dirección.
- Frecuencia y disparadores temporales.
- Detección de cambios (*delta / diff*).
- Control de elementos procesados.
- Registro de fallos y reintentos automáticos.

---

## 15. Security Layer (Capa de Seguridad Transversal)

```text
Authentication ──> Authorization ──> Credentials Vault ──> Encryption ──> Audit
```

- Gestión segura de credenciales externas (tokens, claves API, contraseñas).
- Aislamiento entre clientes (*tenants*).
- Cifrado en tránsito y en reposo.

---

## 16. Observability (Observabilidad y Trazabilidad)

```text
Connection ──> Event ──> Execution ──> Result ──> Log
```

El usuario debe poder auditar e investigar cualquier fallo de forma gráfica y comprensible sin necesidad de inspeccionar código fuente.

---

## 17. Extensibilidad

```text
                    Core
                     │
      ┌──────────────┼──────────────┐
      ↓              ↓              ↓
 Connectors        Agents          SDK
      │
 ┌────┼────┬────┬────┐
 ↓    ↓    ↓    ↓    ↓
ERP  CRM  Shop  Pay  IA
```

El crecimiento de la plataforma se produce mediante extensiones y conectores, manteniendo el Core intacto.

---

## 18. Evolución Arquitectónica por Etapas

```text
Etapa 1: Core + Factusol + Ecommerce (MVP)
   ↓
Etapa 2: Core + Múltiples conectores + Agent + Dashboard
   ↓
Etapa 3: Core + Event Engine + Workflow Engine + API + SDK
   ↓
Etapa 4: Core + Marketplace + Terceros + IA + Automatización Avanzada
```

> **Regla de progresión:** No construiremos las etapas 3 y 4 antes de validar en producción las anteriores.

---

## 19. Restricción Arquitectónica Fundamental

> **Ningún conector debe convertirse en una dependencia estructural del Core.**
>
> Factusol es nuestro primer conector, pero **ERP Bridge NO es un producto exclusivo para Factusol**. Factusol es el caso de uso real para validar la arquitectura.

---

## 20. Criterios de Selección Tecnológica

La tecnología se elegirá para servir a la arquitectura según:
1. Rendimiento y consumo de recursos.
2. Velocidad de desarrollo y mantenimiento.
3. Ecosistema de librerías.
4. Compatibilidad nativa con sistemas locales (Windows, Access, ODBC, etc.).
5. Facilidad de distribución e instalación.
6. Seguridad y robustez.
7. Coste operativo bajo.
8. **Viabilidad para ser mantenido por un equipo pequeño / una sola persona.**

---

## 21. Arquitectura Objetivo

```text
                         ERP BRIDGE
                             │
       ┌─────────────────────┼─────────────────────┐
       │                     │                     │
   Dashboard                 API                  Core
                                                    │
                         ┌──────────────────────────┤
                         │                          │
                  Workflow Engine             Event Engine
                         │                          │
                         └────────────┬─────────────┘
                                      │
                              Common Data Model
                                      │
                         ┌────────────┴────────────┐
                         │                         │
                    Connectors                   Agent
                         │                         │
              ┌──────────┼──────────┐             │
              │          │          │             │
           Factusol    Ecommerce   CRM         ERP local
              │          │          │
              └──────────┴──────────┘
                         │
                       IA
```

*(Representa la arquitectura final objetivo, no el alcance del MVP).*

---

## 22. Regla de Arquitectura

> **Construir primero la mínima arquitectura capaz de demostrar el producto y añadir complejidad únicamente cuando un caso de uso real la justifique.**
