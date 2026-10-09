# Marketplace — v1.0
> **Estado:** Borrador

---

# 205 - Marketplace

> *Ecosistema de extensiones, catálogo de conectores, desarrolladores externos y revenue share.*

---

## 1. Propósito

El Marketplace será el ecosistema donde los usuarios podrán descubrir, instalar y gestionar conectores y extensiones compatibles con ERP Bridge.

La visión es que ERP Bridge no dependa únicamente de los conectores que desarrolle directamente el equipo de ERP Bridge.

---

## 2. Visión

Inicialmente:

Cristian
```text
   ↓
```
Construye Connector
```text
   ↓
```
ERP Bridge

A largo plazo:

                  ERP BRIDGE
                      │
                 MARKETPLACE
```text
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
```
   Oficial        Developer A    Developer B
```text
       ↓              ↓              ↓
```
   Factusol        ERP X          CRM Y

Esto permite que el catálogo pueda crecer mucho más rápido.

---

## 3. Qué habrá en el Marketplace

Inicialmente principalmente:

Connectors.
Integraciones.
Extensiones.

Más adelante:

Templates.
Workflows.
Transformaciones.
Agentes.
Herramientas.
Servicios profesionales.

---

## 4. Connector Listing

Cada conector tendrá una página.

Ejemplo:

FACTUSOL
ERP Connector


★★★★★


Official ERP Bridge Connector


Capabilities:
✓ Products
✓ Stock
✓ Customers
✓ Orders


Requires:
ERP Bridge Agent


[Install]

---

## 5. Información del Connector

Cada publicación deberá indicar:

Name
Description
Category
Developer
Version
Compatibility
Capabilities
Requirements
Documentation
Pricing
Support

Esto evita que el usuario instale algo sin saber qué está adquiriendo.

---

## 6. Official Connectors

ERP Bridge podrá marcar determinados conectores como oficiales.

Ejemplo:

✓ Official


ERP Bridge
Factusol Connector

Esto permite diferenciar:

Official
Community
Verified
Unverified

Los criterios exactos se definirán posteriormente.

---

## 7. Community Connectors

Un desarrollador externo podrá crear:

ERP X Connector

Utilizando:

ERP Bridge SDK

Y publicarlo.

---

## 8. Developer

El desarrollador tendrá un espacio propio.

Conceptualmente:

Developer Portal


My Connectors


```text
├── ERP X
├── CRM Y
└── Logistics Z
```

Podrá gestionar:

versiones,
publicaciones,
documentación,
estadísticas,
incidencias.

---

## 9. Publishing

El proceso podría ser:

Develop
```text
   ↓
```
Test
```text
   ↓
```
Build
```text
   ↓
```
Submit
```text
   ↓
```
Review
```text
   ↓
```
Publish

ERP Bridge deberá validar que el Connector cumple determinados requisitos.

---

## 10. Verification

Los conectores de terceros deberán poder pasar verificaciones.

Por ejemplo:

✓ Manifest valid
✓ SDK compatible
✓ Security checks
✓ Tests passed
✓ Documentation

Esto será especialmente importante si el conector puede acceder a datos empresariales.

---

## 11. Permissions

Un Connector deberá declarar qué permisos necesita.

Ejemplo:

Requires:


✓ Read Products
✓ Read Orders
✓ Write Orders


✗ Read Billing
✗ Manage Users

El usuario podrá ver esto antes de instalarlo.

---

## 12. Installation

La experiencia debería ser:

Connector
```text
   ↓
```
[Install]
```text
   ↓
```
Permissions
```text
   ↓
```
Confirm
```text
   ↓
```
Installed
```text
   ↓
```
[Connect]

No queremos que instalar un conector implique configuración manual compleja.

---

## 13. Updates

Los conectores tendrán versiones.

Installed:
v1.2.0


Available:
v1.3.0


[Update]

Las actualizaciones deberán respetar compatibilidad.

---

## 14. Rollback

Cuando sea técnicamente posible:

v1.3.0
```text
   ↓
```
Problem
```text
   ↓
```
Rollback
```text
   ↓
```
v1.2.0

Esto será importante para operaciones empresariales.

---

## 15. Ratings

Los usuarios podrán valorar conectores.

Ejemplo:

★★★★★ 4.8


127 reviews

Pero las valoraciones no deben ser el único indicador de calidad.

---

## 16. Reviews

Los usuarios podrán dejar comentarios sobre:

estabilidad,
facilidad de instalación,
soporte,
compatibilidad.

Esto permitirá detectar problemas recurrentes.

---

## 17. Categories

El Marketplace podrá organizarse por categorías.

ERP
CRM
Ecommerce
Payments
Accounting
Logistics
Marketing
Communication
AI
Other

---

## 18. Search

Los usuarios podrán buscar:

"Factusol"
"Sage"
"Shopify"
"CRM"
"Stock"
"Pedidos"

Y filtrar por:

categoría,
oficialidad,
compatibilidad,
precio,
valoración.

---

## 19. Pricing

Los conectores podrán ser:

Free
€0
Paid
€X/month
Usage-based
€X / operations
Included

Algunos conectores podrían formar parte de determinados planes de ERP Bridge.

---

## 20. Revenue Sharing

Si terceros venden conectores, ERP Bridge podrá quedarse con una parte.

Conceptualmente:

Customer
```text
   ↓
```
€100
```text
   ↓
```
Marketplace
```text
   ├── Developer → €80
   └── ERP Bridge → €20
```

El porcentaje real se definirá cuando exista el Marketplace.

---

## 21. Free Connectors

La estrategia no debe ser monetizar absolutamente todo.

Algunos conectores gratuitos pueden aumentar la adopción de ERP Bridge.

Ejemplo:

WooCommerce
Free


Shopify
Free


Factusol
Free

Mientras conectores especializados podrían ser de pago.

---

## 22. Developer Incentives

El Marketplace puede convertirse en una forma de atraer desarrolladores.

Ejemplo:

"Construye una integración una vez y gana dinero cada vez que una empresa la utiliza."

Eso puede crear un efecto de red.

---

## 23. Connector Quality

ERP Bridge deberá establecer estándares.

Por ejemplo:

Connector Quality


✓ Tests
✓ Error handling
✓ Security
✓ Documentation
✓ Version compatibility
✓ Health checks

---

## 24. Certification

A largo plazo podremos crear niveles:

Community
Verified
Certified
Official

Esto permite distinguir diferentes niveles de confianza.

---

## 25. Security

El Marketplace es uno de los puntos más delicados.

Nunca deberíamos permitir:

Upload Connector
```text
      ↓
```
Execute blindly

Especialmente porque un Connector puede interactuar con sistemas empresariales.

Deberemos establecer:

revisión,
permisos,
aislamiento,
firma/verificación,
control de versiones.

La implementación concreta se definirá técnicamente cuando llegue el momento.

---

## 26. Marketplace y SDK

La relación será:

SDK
```text
 ↓
```
Developer
```text
 ↓
```
Connector
```text
 ↓
```
Marketplace
```text
 ↓
```
Customer
```text
 ↓
```
ERP Bridge

Por tanto, los documentos:

012 Connectors
013 SDK
019 Marketplace

forman una cadena.

---

## 27. Marketplace y API

Los conectores podrán utilizar la API/SDK según corresponda.

Marketplace
```text
      ↓
```
Connector
```text
      ↓
```
SDK
```text
      ↓
```
ERP Bridge Core
```text
      ↓
```
API / Agent

---

## 28. Templates

A largo plazo el Marketplace no tiene por qué vender únicamente conectores.

También podremos ofrecer plantillas.

Ejemplo:

```text
WooCommerce → Factusol
```


Template:
Orders + Stock + Products

El usuario podría instalar:

[Install Template]

y tener la automatización prácticamente preparada.

---

## 29. Workflow Marketplace

También podremos compartir automatizaciones.

Ejemplo:

New WooCommerce Order
```text
       ↓
```
Create Factusol Order
```text
       ↓
```
Update Stock
```text
       ↓
```
Notify CRM

Un usuario podría instalarla sin construirla desde cero.

---

## 30. Marketplace como ecosistema

La visión final:

                   ERP BRIDGE
                       │
                 MARKETPLACE
```text
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
```
   Connectors       Templates        Workflows
```text
       │               │                │
       └───────────────┼────────────────┘
                       ↓
```
                    Empresa

---

## 31. MVP

Aquí quiero ser muy claro:

NO necesitamos Marketplace en el MVP.

El MVP solo necesita una estructura interna que permita que algún día exista.

Inicialmente:

Connectors
```text
   ↓
```
Official
```text
   ↓
```
Install

Nada más.

---

## 32. Primera fase

Cuando tengamos:

Core funcional.
Agent funcional.
Factusol Connector.
WooCommerce Connector.
SDK básico.
Dashboard.

podremos empezar a pensar en publicar oficialmente los primeros conectores.

---

## 33. Segunda fase

Cuando exista demanda:

Developer Portal
```text
      ↓
```
Third-party Connectors
```text
      ↓
```
Verification
```text
      ↓
```
Marketplace

---

## 34. Tercera fase

Cuando haya suficiente ecosistema:

Marketplace
```text
   ↓
```
Payments
```text
   ↓
```
Revenue Sharing
```text
   ↓
```
Developers
```text
   ↓
```
More Connectors
```text
   ↓
```
More Customers
```text
   ↓
```
More Developers

Esto puede crear un network effect.

---

## 35. Criterio de éxito

El Marketplace tendrá éxito cuando ERP Bridge pueda incorporar nuevos sistemas más rápido de lo que el equipo interno puede desarrollarlos por sí mismo.

---

## 36. Regla fundamental

El Marketplace no es el producto inicial. Es el mecanismo que permitirá que el producto crezca más allá de nuestro propio equipo.

---
