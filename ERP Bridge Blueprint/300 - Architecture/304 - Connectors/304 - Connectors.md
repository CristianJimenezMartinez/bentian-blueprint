# Connectors — v1.0
> **Estado:** Borrador

---

# 304 - Connectors

> *Catálogo y especificación de conectores ERP.*

---

## 1. Propósito

Un **Connector** es un componente independiente encargado de comunicarse con un sistema externo y adaptar sus capacidades al modelo común de ERP Bridge.

- El **Connector** conoce cómo funciona el sistema externo.
- El **Core** conoce cómo funciona ERP Bridge y cómo orquestar la integración.

> Esta separación de responsabilidades es un pilar fundamental del diseño.

---

## 2. Principio Fundamental

```text
                    ERP BRIDGE CORE
                          │
                   Connector Interface
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ↓                 ↓                 ↓
    Factusol          WooCommerce        Shopify
    Connector          Connector         Connector
        │                 │                 │
        ↓                 ↓                 ↓
    Factusol          WooCommerce        Shopify
```

> **El Core no necesita saber cómo funciona internamente cada plataforma externa.**

---

## 3. Qué es un Connector

Un módulo independiente que implementa la comunicación con un sistema concreto:
- **ERPs:** Factusol, Sage, Odoo, SAP, etc.
- **Ecommerce:** WooCommerce, Shopify, PrestaShop, Magento, etc.
- **CRMs & Servicios:** HubSpot, Salesforce, Stripe, Holded, etc.

---

## 4. Responsabilidades de un Conector

1. **Conexión:** Establecer y mantener la comunicación con el sistema destino.
2. **Autenticación:** Gestionar API Keys, OAuth 2.0, Usuario/Contraseña, Tokens o conexiones locales.
3. **Lectura:** Obtener información (`getProducts()`, `getCustomers()`, `getOrders()`, `getStock()`).
4. **Escritura:** Enviar y mutar información (`createProduct()`, `updateProduct()`, `createOrder()`, `updateCustomer()`).
5. **Eventos:** Notificar al Core cuando el sistema lo soporte (`ORDER_CREATED`, `PRODUCT_UPDATED`, `STOCK_CHANGED`).
6. **Health Check:** Comprobar la disponibilidad y latencia del sistema externo.

---

## 5. El Connector NO Controla los Workflows

### ❌ Incorrecto (Acoplamiento directo):
```text
Factusol Connector ──> Enviar pedido a WooCommerce
```

### ✅ Correcto (Orquestación desacoplada vía Core):
```text
Factusol Connector    ──> Obtener pedido
          │
          ↓
  ERP Bridge Core     ──> Decide la regla de negocio y transformación
          │
          ↓
WooCommerce Connector ──> Crear pedido
```

> El conector proporciona **capacidades de acceso**; el Core decide **cómo y cuándo utilizarlas**.

---

## 6. Modelo Conceptual de Interfaz

Todos los conectores exponen sus capacidades mediante un contrato unificado:

```text
Connector
│
├── Metadata (nombre, versión, categoría, tipo)
├── Authentication (mecanismo de acceso)
├── Connection (apertura / cierre de sesión)
├── Capabilities (operaciones soportadas)
├── Read (lectura canónica)
├── Create (creación canónica)
├── Update (actualización canónica)
├── Delete (baja canónica)
├── Events (listeners / webhooks)
└── Health (test de estado)
```

---

## 7. Metadata

Cada conector expone metadatos descriptivos para el Core y Dashboard:

```text
Name:           Factusol
Version:        1.0.0
Category:       ERP
Type:           Local (Agent-assisted)
Authentication: Local Database / Access
Capabilities:   Products, Customers, Orders, Stock
```

---

## 8. Capability Model (Capacidades Específicas)

El Core detecta qué operaciones están disponibles en cada conector:

```text
FACTUSOL                     SHOPIFY
├── Products    ✓            ├── Products    ✓
├── Customers   ✓            ├── Customers   ✓
├── Orders      ✓            ├── Orders      ✓
├── Stock       ✓            ├── Stock       ✓
└── Webhooks    ✗            └── Webhooks    ✓
```

---

## 9. Abstracción de la Autenticación

El Core interactúa de manera transparente sin importar el esquema de seguridad subyacente:

```text
API Key ──────┐
OAuth 2.0 ────┼──> Connector Interface ──> ERP Bridge Core
Basic Auth ───┤
Local Agent ──┘
```

---

## 10. Adaptación al Modelo Común de Datos

El conector traduce los datos específicos del sistema hacia el modelo canónico de ERP Bridge:

```text
Factusol Product ──> Factusol Connector ──> ERP Bridge Product (Canónico)
                                                    │
                                                    ↓
WooCommerce Product <── WooCommerce Connector <─────┘
```

---

## 11. Transformación de Datos

- **Reglas Generales de Negocio:** Pertenecen al **Core**.
- **Particularidades del Formato del Sistema:** Pertenecen al **Connector**.

```text
Factusol ("Precio": 12.50) ──> ERP Bridge (price: 12.50) ──> WooCommerce (regular_price: "12.50")
```

---

## 12. Gestión de Errores Estructurados

El Connector debe devolver errores tipificados para que el Core tome decisiones inteligentes:

```json
{
  "code": "AUTH_EXPIRED",
  "message": "Token de autenticación caducado",
  "operation": "getProducts",
  "retryable": true
}
```

> El Core decidirá si debe **reintentar, pausar, alertar o registrar** en auditoría.

---

## 13. Idempotencia y Mapeo de Identificadores

Los conectores facilitan la correlación entre identificadores locales y remotos:

```text
Pedido ERP (ID: 12345) ──> ERP Bridge (Correlation Engine) ──> Ecommerce (External ID: ABC-987)
```

---

## 14. Mecanismos de Sincronización

El conector debe soportar uno o varios métodos de captura:
1. **Polling:** Consulta periódica de cambios en intervalos de tiempo.
2. **Webhooks:** Recepción de notificaciones push en tiempo real.
3. **Eventos Locales:** Detección de cambios a nivel de archivo/base de datos por el Local Agent.

---

## 15. Conector Oficial de Factusol (Conector #1)

Será el primer conector oficial del proyecto:
- **Especialidad:** Conocer en profundidad Factusol, Microsoft Access (ficheros `.mdb`/`.accdb`), consultas ODBC y estructura de tablas.
- **Frontera:** Solo se encarga de leer/escribir en Factusol; no asume ninguna lógica sobre el destino ni sobre la nube.

### Estrategia de Conectividad de Factusol
El Connector de Factusol adoptará una arquitectura de adaptadores desacoplados:
1. **API Oficial:** Factusol utilizará la API oficial del fabricante cuando sea suficiente para la operación requerida.
2. **Local Agent (ODBC / Access):** El Local Agent se utilizará cuando sea necesario acceder a recursos locales, bases de datos Access u operaciones que la API oficial no cubra adecuadamente.

*Nota técnica: Dado que la investigación técnica de la API oficial de Factusol todavía está en curso, no se prejuzga ni se afirma a priori qué operaciones específicas requerirán Local Agent y cuáles se resolverán vía API.*

---

## 16. Evolución del Proyecto Actual

Aprovechar el desarrollo existente refactorizándolo limpiamente:

```text
                  ERP Bridge
                       │
          ┌────────────┴────────────┐
          │                         │
     Core Engine            Factusol Connector
          │                         │
          ↓                         ↓
   Dashboard UI             Base de Datos Access
```

- La lógica genérica y reutilizable pasa al **Core**.
- La lógica específica de lectura de Access pasa a `connector-factusol`.

---

## 17. Estructura Conceptual del Repositorio

```text
ERP Bridge/
├── Core/
├── Agent/
├── Connectors/
│   ├── Factusol/
│   ├── WooCommerce/
│   ├── Shopify/
│   └── Sage/
├── SDK/
├── API/
└── Dashboard/
```

---

## 18. Independencia Modular

Cada conector debe poder:
- Desarrollarse y versionarse de forma aislada.
- Ejecutar su propia suite de pruebas unitarias.
- Actualizarse o desactivarse sin impactar al Core ni a otros conectores.

---

## 19. Versionado Semántico

```text
Factusol Connector ──> v1.0.0 ──> v1.1.0 ──> v2.0.0
```

Permite actualizar conectores sin romper flujos existentes en producción.

---

## 20. Estrategia de Testing por Conector

```text
Factusol Connector Tests
│
├── Connection Tests (Health check & Auth)
├── Product Tests (Read / Normalization)
├── Customer Tests (Deduplication / Read)
├── Order Tests (Payload validation)
├── Stock Tests (Diff checking)
└── Error Handling Tests (Timeout / Lock handling)
```

---

## 21. Documentación Estándar de Conector

Cada conector dispondrá de su ficha técnica:
- Sistema y versiones compatibles soportadas.
- Guía de instalación y configuración de credenciales.
- Entidades y operaciones soportadas.
- Limitaciones técnicas conocidas (ej. ausencia de webhooks).

---

## 22. Catálogo Oficial de Conectores (Entradas y Salidas)

ERP Bridge estructura su catálogo en conectores de Entrada (ERPs locales) y Salida (canales de venta B2C/B2B y marketplaces):

```text
CONECTORES DE ENTRADA (ERPs Locales)           CONECTORES DE SALIDA (Canales & Marketplaces)
┌────────────────────────────────────────┐     ┌────────────────────────────────────────┐
│ • Factusol (Access .accdb/.mdb)        │     │ • WooCommerce (REST API + Companion)   │
│ • SimplyGest (Access .mdb)             │     │ • PrestaShop (Web Service JSON/XML)    │
│ • ClassicGes 6 (Visual FoxPro .dbf)    │ ──> │ • Mirakl Universal (Leroy, PcComp, etc)│
│ • Visionwin (xBase / Harbour .dbf)     │     │ • Portal Privado B2B (Mayoristas)      │
│ • Sage 50 / Eurowin (MS SQL Server)    │     │ • Web Universal / CMS (HTTPS Bridge)   │
│                                        │     │ • Shopify (GraphQL Admin API)          │
└────────────────────────────────────────┘     └────────────────────────────────────────┘
```

---

## 23. Documentos Técnicos Específicos por Conector

Cada conector cuenta con su especificación de arquitectura detallada:
* [304.1 - SimplyGest Connector](304.1%20-%20SimplyGest%20Connector.md): Integración con base de datos Access `.mdb` reutilizando `AccessDriver`.
* [304.2 - PrestaShop Connector](304.2%20-%20PrestaShop%20Connector.md): Web Service dual JSON/XML, recálculo de `ps_stock_available` y micro-módulo acelerador.
* [304.3 - Mirakl Universal Connector](304.3%20-%20Mirakl%20Universal%20Connector.md): Conexión multi-marketplace para Leroy Merlin, PcComponentes y Carrefour.
* [304.4 - B2B Portal Connector](304.4%20-%20B2B%20Portal%20Connector.md): Portal web privado para clientes mayoristas con tarifas sincronizadas de Factusol.
* [304.5 - Universal Web & Bespoke CMS Connector](304.5%20-%20Universal%20Web%20%26%20Bespoke%20CMS%20Connector.md): Conexión universal para webs a medida, Angular/React, Plesk/cPanel y CMS sin frameworks mediante micro-puente HTTPS en puerto 443 (cero puertos en BD y SSL automático).

---

## 24. Reutilización Máxima de Motores de Conectividad

Para optimizar el tiempo de desarrollo y la estabilidad del código, los conectores comparten motores subyacentes:

```text
MOTOR ADODB / ACCESS (.MDB / .ACCDB) ──> Factusol Connector + SimplyGest Connector
MOTOR DBF / XBASE (.DBF / .CDX)     ──> ClassicGes 6 Connector + Visionwin Connector
MOTOR REST / WEBSERVICE             ──> WooCommerce Connector + PrestaShop Connector
MOTOR HTTPS BRIDGE (PHP COMPANION)  ──> Universal Web / Custom CMS / Plesk & cPanel Connector
MOTOR MIRAKL REST API               ──> Leroy Merlin + PcComponentes + Carrefour + MediaMarkt
```

---

## 25. Secuencia de Entrega por Fases

1. **MVP Base:** Factusol (Entrada) ↔ WooCommerce (Salida).
2. **Fase 1:** SimplyGest (Entrada) sobre `AccessDriver`.
3. **Fase 1.5:** Universal Web & Bespoke CMS Connector (Salida) sobre micro-puente HTTPS en puerto 443.
4. **Fase 2:** PrestaShop (Salida) para catálogos industriales y pyme tradicional.
5. **Fase 3:** Mirakl Universal (Salida) para marketplaces de alto volumen.
6. **Fase 4:** Portal Privado B2B (Salida) + Conectores DBF (ClassicGes 6 / Visionwin) + Shopify GraphQL.

---

## 26. Regla Final

> **Los conectores conocen los sistemas. El Core conoce la integración.**
