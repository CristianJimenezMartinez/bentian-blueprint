# Master Roadmap — v1.0
> **Estado:** Borrador

---

# 500 - Roadmap

> *Estrategia de construcción progresiva, fases de desarrollo, hitos y visión a 10 años.*

---

## 1. Objetivo

Construir ERP Bridge progresivamente desde el conector de Factusol existente hasta una plataforma de integración y automatización capaz de conectar ERPs, ecommerce, CRM, servicios digitales y sistemas empresariales.

---

## 2. Punto de partida

ERP Bridge no comienza desde cero.

Ya existe:

Factusol
```text
    ↓
```
Access / ACCDB
```text
    ↓
```
Node.js + TypeScript
```text
    ↓
```
PostgreSQL
```text
    ↓
```
Angular
```text
    ↓
```
Ecommerce

Este proyecto será utilizado como laboratorio y primera fuente de tecnología reutilizable.

---

## 3. Principio de construcción

No se debe desarrollar:

Todo ERP Bridge
```text
       ↓
```
Esperar
```text
       ↓
```
Buscar clientes

Se debe desarrollar:

Problema real
```text
      ↓
```
Producto mínimo
```text
      ↓
```
Empresa real
```text
      ↓
```
Feedback
```text
      ↓
```
Mejora
```text
      ↓
```
Nuevo problema

---

## 4. Fase 0 — Blueprint

Estado

Completado conceptualmente.

Documentación:

001 Vision
002 Product Principles
003 Glossary
004 Product Scope
005 Personas
006 Use Cases
007 Business Model
008 Licensing
009 Architecture
010 Core
011 Agent
012 Connectors
013 SDK
014 API
015 Dashboard
016 Security
017 Events
018 Synchronization
019 Marketplace
020 Roadmap

Objetivo:

Tener una definición coherente antes de construir.

---

## 5. Fase 1 — Core Foundation

Primero debemos extraer el núcleo reutilizable.

ERP Bridge
└── Core

El Core debe encargarse de conceptos comunes:

Connections.
Organizations.
Credentials.
Events.
Actions.
Flows.
Jobs.
Logs.
Errors.

No debe contener lógica específica de Factusol.

---

## 6. Regla de oro del Core

Esto:

if factusol
   ...

no debería aparecer continuamente dentro del Core.

La lógica específica deberá vivir en:

Connectors

Así:

Core
  ↑
Connector
  ↑
Factusol

---

## 7. Fase 2 — Connector Architecture

Definir una interfaz común para conectores.

Conceptualmente:

Connector
```text
├── connect()
├── disconnect()
├── healthCheck()
├── capabilities()
├── execute()
└── events()
```

La interfaz exacta se decidirá durante la implementación.

---

## 8. Fase 3 — Factusol Connector

Tu conector actual de Factusol será el primer Connector oficial.

ERP Bridge
```text
     │
     ↓
```
Factusol Connector
```text
     │
     ↓
```
Agent
```text
     │
     ↓
```
Access / ACCDB

No debemos tirar lo que ya tienes.

Debemos extraer y reorganizar lo reutilizable.

---

## 9. Objetivo Factusol

El Connector deberá poder trabajar inicialmente con:

Products
Stock
Customers
Orders
Invoices

Pero no necesariamente todo desde el primer día.

---

## 10. Fase 4 — Agent MVP

Construir el Agent local.

Su responsabilidad:

Local ERP
```text
   ↓
```
Agent
```text
   ↓
```
Secure connection
```text
   ↓
```
ERP Bridge

El Agent será especialmente importante para:

Factusol.
Access.
ERPs locales.
Bases de datos locales.
Sistemas sin API pública.

---

## 11. Fase 5 — First Connection

Debemos conseguir una experiencia extremadamente sencilla:

Install Agent
```text
      ↓
```
Login
```text
      ↓
```
Connect
```text
      ↓
```
Factusol detected
```text
      ↓
```
Connected ✓

Este será uno de los principales diferenciadores.

---

## 12. Fase 6 — Dashboard MVP

Crear una interfaz mínima.

No necesitamos las "150 páginas".

Necesitamos unas pocas pantallas extremadamente buenas.

Inicialmente:

Dashboard
Connections
Agents
Sync
Activity
Settings

---

## 13. Fase 7 — First Real Synchronization

Primer objetivo comercial:

Factusol
   ↕
WooCommerce

Con:

Products
Stock
Orders

No intentaremos soportar 50 entidades.

---

## 14. Fase 8 — First Complete Use Case

Tenemos que conseguir este flujo:

Factusol
```text
   ↓
```
Products
```text
   ↓
```
WooCommerce


Factusol
```text
   ↓
```
Stock
```text
   ↓
```
WooCommerce


WooCommerce
```text
   ↓
```
Order
```text
   ↓
```
Factusol

Si esto funciona correctamente, ya tenemos algo que enseñar.

---

## 15. Fase 9 — Events

Añadimos el sistema de Events.

Ejemplo:

ORDER_CREATED
```text
      ↓
```
Flow
```text
      ↓
```
CREATE_ORDER

Aquí empieza a aparecer la automatización.

---

## 16. Fase 10 — Flows

Construir el primer sistema visual de automatización.

Por ejemplo:

WHEN
Order Created


THEN
Create Order in Factusol

Después:

WHEN
Stock Changed


THEN
Update WooCommerce Stock

---

## 17. Fase 11 — Second Connector

No debemos construir diez conectores.

Construiremos uno más.

Por ejemplo:

WooCommerce

o el sistema que resulte más estratégico según las primeras pruebas.

La razón es comprobar que nuestra arquitectura realmente permite:

Connector A
Connector B

sin modificar el Core.

---

## 18. Fase 12 — Connector Test

Esta será una prueba fundamental.

Debemos poder añadir:

Connector C

sin tener que reescribir:

Core
Agent
Dashboard
Events
Synchronization

Si necesitamos hacerlo, la arquitectura todavía no está suficientemente desacoplada.

---

## 19. Fase 13 — SDK

Cuando tengamos dos conectores funcionando, podremos extraer el SDK.

¿Por qué no antes?

Porque primero necesitamos saber qué tienen realmente en común los conectores.

El SDK deberá permitir:

Developer
```text
   ↓
```
ERP Bridge SDK
```text
   ↓
```
New Connector

---

## 20. Fase 14 — Public Connector Model

Crear una estructura estándar:

connector-factusol
connector-woocommerce
connector-sage
connector-shopify

Cada uno separado.

Esto encaja exactamente con la idea que comentabas anteriormente:

Cada Connector debe ser un proyecto/módulo independiente.

---

## 21. Fase 15 — First External Tester

Aquí cambia todo.

Debemos conseguir:

una empresa real que no seas tú.

No necesitamos 100 clientes.

Necesitamos:

1 empresa
1 ERP
1 ecommerce
1 problema real

Y observar:

instalación,
conexión,
errores,
sincronización,
soporte,
experiencia de usuario.

---

## 22. Fase 16 — Product Hardening

Con feedback real:

corregir errores,
mejorar UX,
mejorar logs,
mejorar recuperación,
mejorar seguridad,
mejorar instalación.

Aquí no añadimos funcionalidades porque sí.

El cliente decide qué duele.

---

## 23. Fase 17 — Commercial MVP

El primer producto comercial podría ser:

Conecta tu ERP local con tu ecommerce sin cambiar de ERP y sin subir tu base de datos.

Por ejemplo:

Factusol
    ↕
ERP Bridge
    ↕
WooCommerce

---

## 24. Fase 18 — Pricing

No debemos fijar definitivamente los precios antes de tener experiencia real.

Primero:

¿Funciona?
```text
↓
```
¿Lo necesitan?
```text
↓
```
¿Lo utilizan?
```text
↓
```
¿Pagan?

Después podremos optimizar pricing.

---

## 25. Fase 19 — More Connectors

Cuando el núcleo esté validado:

Factusol
WooCommerce
```text
       ↓
```
Sage
Shopify
PrestaShop
HubSpot
Salesforce
...

Pero cada nuevo Connector deberá justificar su desarrollo.

---

## 26. Fase 20 — Marketplace

Solo cuando exista suficiente demanda:

ERP Bridge
```text
     ↓
```
SDK
```text
     ↓
```
Developers
```text
     ↓
```
Third-party Connectors
```text
     ↓
```
Marketplace

No antes.

---

## 27. Fase 21 — AI

La IA no debe ser el punto de partida.

Primero:

Connect
Synchronize
Automate

Después:

Understand
Recommend
Detect
Optimize
Execute

La IA podrá convertirse en una capa superior del sistema.

---

## 28. Fase 22 — AI Agent

Visión futura:

Empresa
```text
   ↓
```
ERP Bridge AI
```text
   ↓
```
Analiza sistemas
```text
   ↓
```
Detecta problemas
```text
   ↓
```
Propone automatizaciones
```text
   ↓
```
Ejecuta acciones

Ejemplo:

"He detectado que vuestro stock de Factusol se actualiza manualmente en WooCommerce. ¿Quieres que cree una sincronización automática?"

Esto encaja directamente con tu visión de:

conectar todo en 3 minutos.

---

## 29. Fase 23 — Enterprise

Cuando exista tracción:

SSO.
Advanced RBAC.
Advanced audit.
SLA.
High availability.
Advanced monitoring.
Compliance.
Enterprise support.

---

## 30. Fase 24 — Ecosystem

Visión:

              ERP BRIDGE
```text
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
```
    Connectors    AI        Automation
```text
       ↓           ↓           ↓
       └───────────┼───────────┘
                   ↓
```
              Marketplace
```text
                   ↓
```
               Empresas

---

## 31. Lo que NO vamos a hacer

Durante el MVP NO vamos a construir:

100 conectores.
Marketplace completo.
IA autónoma.
Aplicación móvil.
Sistema empresarial gigantesco.
Kubernetes porque sí.
Microservicios por moda.
Arquitectura distribuida innecesaria.
150 pantallas.
Todas las funciones imaginables.

---

## 32. Orden real de construcción

Este es probablemente el apartado más importante del documento:

1. Core
```text
      ↓
```
2. Connector Architecture
```text
      ↓
```
3. Factusol Connector
```text
      ↓
```
4. Agent
```text
      ↓
```
5. Dashboard
```text
      ↓
```
6. WooCommerce Connector
```text
      ↓
```
7. Synchronization
```text
      ↓
```
8. Events
```text
      ↓
```
9. Flows
```text
      ↓
```
10. Second/Third Connector
```text
      ↓
```
11. SDK
```text
      ↓
```
12. External Tester
```text
      ↓
```
13. Commercial MVP
```text
      ↓
```
14. More Connectors
```text
      ↓
```
15. Marketplace
```text
      ↓
```
16. AI
```text
      ↓
```
17. Enterprise

---

## 32.B Formalización de las 5 Fases Quirúrgicas de Ejecución

Para evitar la dispersión técnica y asegurar la validación comercial en cada entrega, ERP Bridge se ejecuta siguiendo 5 fases secuenciales estrictas:

```text
┌──────────────────────────────────────────────────────────────────────────────────┐
│ FASE 0: Blindaje de WooCommerce (Delta Sync + R.E.) + Lanzamiento a coste 0 €   │
│         en bridge.cristianjm.com                                                 │
│ • Motor Delta Sync en SQLite (detección por 3 hashes: data, price, stock).       │
│ • Micro-lotes de 25 items y throttling adaptativo para hostings compartidos.     │
│ • Mapeo de Recargo de Equivalencia (R.E. 5,2%, 1,4%, 0,5%) a REQPCL en Factusol. │
│ • Blindaje y deduplicación de imágenes por omisión en updates y hash MD5.        │
│ • Transacciones atómicas nativas (BeginTrans/CommitTrans) en AccessDriver.       │
│ • Landing page en bridge.cristianjm.com conectada a Stripe a coste 0,00 €.       │
│ • Publicación del Plan Único Base a 249 €/año (199 € lanzamiento) o 29 €/mes.    │
│ • Prospección de las primeras 10 agencias web de WooCommerce (Canal B2B).        │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ FASE 1: Conector SimplyGest (Quick Win de entrada sobre AccessDriver)            │
│ • Desarrollo de packages/connectors/simplygest.                                  │
│ • Reutilización del 100% de AccessDriver sobre Datos.mdb (Microsoft Jet/ACE).    │
│ • Mapeo de ARTICULOS, CLIENTES y transacciones atómicas en PEDIDOS_VENTA.        │
│ • Resuelve el fallo histórico de WebLinker (puerto MySQL 3306 bloqueado).        │
│ • Apertura inmediata a más de 35.000 empresas activas en España.                 │
│ • Validación comercial: alcanzar los primeros 3 a 5 clientes de pago (€).        │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ FASE 2: Conector PrestaShop (Salida industrial y catálogos masivos)              │
│ • Desarrollo de packages/connectors/prestashop.                                  │
│ • Web Service nativo con motor dual: lectura en JSON y escritura en XML.         │
│ • Gestión optimizada de la tabla relacional ps_stock_available y combinaciones.   │
│ • Micro-módulo opcional erpbridge_sync para actualización SQL directa masiva.    │
│ • Acceso al 40% del e-commerce industrial (ferretería, recambios, calzado).      │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ FASE 3: Conector Universal Mirakl (Leroy Merlin, PcComponentes, Carrefour)        │
│ • Desarrollo de packages/connectors/mirakl.                                      │
│ • Un solo conector conecta simultáneamente con Leroy Merlin, PcComponentes,     │
│   Carrefour España, MediaMarkt y Decathlon.                                      │
│ • Sincronización de ofertas, precios y stock en tiempo real vía POST /api/offers.│
│ • Descarga bidireccional de pedidos y subida de tracking de expedición.          │
└────────────────────────────────────────┬─────────────────────────────────────────┘
                                         │
                                         ▼
┌──────────────────────────────────────────────────────────────────────────────────┐
│ FASE 4: Portal B2B Mayorista + Conectores DBF (ClassicGes 6 / Visionwin)         │
│         + Shopify GraphQL                                                        │
│ • Módulo Portal Privado B2B: catálogo cerrado con tarifas personalizadas de     │
│   Factusol y descarga de facturas/albaranes PDF para distribuidores mayoristas.   │
│ • Desarrollo de DbfDriver para desbloquear ClassicGes 6 y Visionwin (>100.000).  │
│ • Desarrollo de packages/connectors/shopify mediante GraphQL Admin API.          │
└──────────────────────────────────────────────────────────────────────────────────┘
```

### Detalle de Ejecución de las 5 Fases:

1. **Fase 0: Blindaje de WooCommerce (Delta Sync + R.E.) + Lanzamiento a coste 0 € en bridge.cristianjm.com:**
   - Blindaje de peticiones batch en micro-lotes de 25 productos y throttling adaptativo (>1.500 ms) para evitar caídas en WordPress/WooCommerce.
   - Recargo de Equivalencia (R.E.) mapeado bidireccionalmente a la columna `REQPCL` de Factusol para cumplir con el Modelo 303 de la AEAT.
   - Deduplicación y omisión de imágenes en updates para evitar consumo de disco y sobrecarga de CPU en WordPress.
   - Transacciones atómicas seguras en Access OLEDB (`BeginTrans`, `CommitTrans`, `RollbackTrans`).
   - Publicación y cobro de suscripciones mediante Stripe en `bridge.cristianjm.com` a coste 0 €.

2. **Fase 1: Conector SimplyGest (Quick Win de entrada sobre AccessDriver):**
   - Implementación de `@erp-bridge/connector-simplygest` sobre `Datos.mdb` reutilizando el motor `AccessDriver`.
   - Soporte nativo de `ARTICULOS`, `CLIENTES` y transacciones en `PEDIDOS_VENTA` y `LINEAS_PEDIDOS`.
   - Superación de las limitaciones de WebLinker sin necesidad de abrir el puerto 3306.

3. **Fase 2: Conector PrestaShop (Salida industrial y catálogos masivos):**
   - Integración con el Web Service oficial de PrestaShop 1.7 / 8 mediante motor híbrido JSON (lectura) y XML (escritura).
   - Sincronización directa de stock sobre `ps_stock_available` y matrices de variantes de producto.

4. **Fase 3: Conector Universal Mirakl (Leroy Merlin, PcComponentes, Carrefour):**
   - Conexión unificada con los principales marketplaces del mercado español que operan bajo Mirakl.
   - Sincronización continua de inventario y precios, con gestión bidireccional de pedidos y expediciones con seguimiento.

5. **Fase 4: Portal B2B Mayorista + Conectores DBF (ClassicGes 6 / Visionwin) + Shopify GraphQL:**
   - Portal web privado B2B para mayoristas con tarifas diferenciadas y descarga de albaranes/facturas.
   - Driver `DbfDriver` nativo para bases de datos xBase (`.dbf`) de ClassicGes 6 y Visionwin.
   - Conector oficial para Shopify basado en GraphQL Admin API.

---

## 33. Definición del MVP

El MVP de ERP Bridge será considerado válido cuando una empresa pueda:

1. Crear una cuenta
```text
        ↓
```
2. Instalar Agent
```text
        ↓
```
3. Conectar su ERP
```text
        ↓
```
4. Conectar su ecommerce
```text
        ↓
```
5. Seleccionar qué sincronizar
```text
        ↓
```
6. Configurar el mapping necesario
```text
        ↓
```
7. Activar la sincronización
```text
        ↓
```
8. Ver el estado
```text
        ↓
```
9. Detectar errores
```text
        ↓
```
10. Recuperarse de ellos

Sin programar.

---

## 34. La métrica más importante

No mediremos el éxito únicamente por:

Número de funcionalidades

La métrica principal será:

Tiempo desde "quiero conectar mi ERP" hasta "la conexión está funcionando".

Objetivo futuro:

< 3 minutos

Pero no vamos a fingir que serán 3 minutos desde el primer día.

Primero tenemos que conseguir que funcione de forma fiable.

---

## 35. La visión a 10 años

ERP Bridge deberá evolucionar desde:

CONNECTION

hacia:

INTEGRATION

después:

AUTOMATION

después:

INTELLIGENCE

y finalmente:

BUSINESS INFRASTRUCTURE

La progresión conceptual:

Conectar
```text
   ↓
```
Sincronizar
```text
   ↓
```
Automatizar
```text
   ↓
```
Entender
```text
   ↓
```
Optimizar
```text
   ↓
```
Ejecutar

---

## 36. Criterio final

ERP Bridge será el núcleo de integración que permita a una empresa conectar sus sistemas existentes, sincronizar sus datos y automatizar sus operaciones sin sustituir su infraestructura actual ni necesitar desarrolladores para cada integración.

---

## 37. Regla definitiva

Construir primero aquello que una empresa pueda utilizar. Después aquello que una empresa pueda pagar. Y solamente después aquello que pueda escalar a un ecosistema.

---
