# Product Principles — v1.0
> **Estado:** Borrador

---

# 102 - Product Principles

> *Principios de diseño y desarrollo del producto.*

---

## Propósito

Este documento define los principios fundamentales que deben guiar las decisiones de producto, arquitectura, desarrollo y negocio de ERP Bridge.

Cuando exista una duda entre varias opciones, estos principios deberán utilizarse como criterio para tomar la decisión.

---

## Principios Fundamentales

### Principio 1 — El software se adapta a la empresa

ERP Bridge debe adaptarse a la forma de trabajar de la empresa, nunca obligar a la empresa a cambiar sus sistemas para utilizar ERP Bridge.

La empresa debe poder conservar sus herramientas actuales siempre que sea posible.

---

### Principio 2 — No obligar a cambiar de ERP

ERP Bridge no pretende sustituir el ERP del cliente.

Debe poder trabajar con sistemas existentes, incluidos sistemas tradicionales o antiguos.

El cliente no compra un ERP nuevo. Compra la capacidad de conectar el que ya tiene.

---

### Principio 3 — Conectar sin programar

Las integraciones habituales deben poder configurarse sin conocimientos de programación.

El usuario debe poder seleccionar:

```text
Origen
   ↓
Destino
   ↓
Datos
   ↓
Reglas
   ↓
Activar
```

La programación debe quedar reservada para el desarrollo de nuevos conectores o casos realmente excepcionales.

---

### Principio 4 — Un núcleo, muchos conectores

El núcleo de ERP Bridge nunca debe depender de un sistema concreto.

Factusol, Sage, Odoo, WooCommerce, Shopify, HubSpot, etc. deben ser módulos independientes.

El objetivo es que añadir un nuevo conector no implique reconstruir la plataforma.

```text
                 ERP Bridge Core
                       │
       ┌───────────────┼───────────────┐
       │               │               │
   Factusol          Sage          WooCommerce
       │               │               │
       └───────────────┼───────────────┘
                       │
                 Otros sistemas
```

---

### Principio 5 — Los conectores son intercambiables

Un conector debe poder:

- instalarse,
- actualizarse,
- desactivarse,
- sustituirse,
- versionarse,

sin romper el núcleo ni otros conectores.

Esto permitirá que ERP Bridge crezca mediante un ecosistema de conectores.

---

### Principio 6 — Los datos pertenecen al cliente (Zero-Knowledge)

Bentian nunca debe apropiarse de los datos de la empresa ni almacenarlos en la nube.

**Arquitectura Zero-Knowledge:** La infraestructura cloud de Bentian **NUNCA ingesta, copia ni almacena información de negocio** (productos, precios, pedidos, clientes ni bases de datos de Factusol). Toda la sincronización se procesa localmente en el extremo (*Edge Processing*) y viaja de forma directa y exclusiva entre la máquina del cliente y su tienda online.

---

### Principio 7 — La nube no debe ser una obligación

ERP Bridge debe poder trabajar con sistemas locales.

Esto es especialmente importante para ERPs tradicionales que funcionan en ordenadores o servidores de la empresa.

La arquitectura debe permitir que exista un componente local capaz de comunicarse de forma segura con la plataforma.

---

### Principio 8 — Seguridad desde el diseño

La seguridad no será una característica que añadiremos al final.

Debe estar presente desde el diseño inicial:

- Autenticación.
- Autorización.
- Cifrado.
- Gestión segura de credenciales.
- Control de acceso.
- Auditoría.
- Aislamiento entre clientes.
- Actualizaciones seguras.

---

### Principio 9 — Fallar de forma segura

Una integración puede fallar.

Internet puede caerse.

Un ERP puede estar apagado.

Una API puede dejar de responder.

Un cliente puede modificar un dato.

ERP Bridge debe detectar estos problemas y recuperarse sin provocar pérdida o corrupción de información.

Un error de conexión nunca debe convertirse en un error empresarial.

---

### Principio 10 — Todo debe ser observable

ERP Bridge debe permitir saber:

- qué ocurrió,
- cuándo ocurrió,
- qué sistema participó,
- qué datos se procesaron,
- si hubo errores,
- qué se hizo para recuperarse.

El usuario no debería encontrarse con un simple:

> *"Error de sincronización."*

Debe poder entender qué ocurrió.

---

### Principio 11 — Automatización antes que intervención humana

Si un proceso puede ejecutarse de forma segura y fiable automáticamente, no debería requerir intervención humana.

La intervención humana debe reservarse para excepciones, decisiones o situaciones que realmente lo necesiten.

---

### Principio 12 — IA como capacidad, no como excusa

ERP Bridge podrá utilizar IA cuando aporte valor real.

La IA puede utilizarse para:

- interpretar información,
- clasificar datos,
- detectar anomalías,
- generar respuestas,
- automatizar decisiones,
- asistir al usuario.

Pero:

- No utilizaremos IA simplemente porque sea una tendencia.
- Si una regla determinista es mejor, utilizaremos una regla determinista.

---

### Principio 13 — Diseñado para una sola persona

Este es especialmente importante para ti.

La arquitectura debe permitir que una empresa pequeña pueda mantener ERP Bridge sin necesitar un ejército de desarrolladores.

Cada nuevo conector debe reutilizar el máximo posible del trabajo existente.

La automatización del desarrollo, las pruebas, documentación y despliegue debe ser una prioridad.

---

### Principio 14 — La complejidad debe estar detrás del producto

El usuario debe ver:

> **Conectar Factusol con WooCommerce**

No:

> *OAuth + Webhooks + Event Bus + Mapping Engine + Queue + Retry Policy + Access Driver...*

Toda esa complejidad debe existir debajo de una interfaz sencilla.

---

### Principio 15 — Diseñar para crecer, no para sobreingeniería

ERP Bridge debe poder crecer, pero no vamos a construir infraestructura para problemas que todavía no existen.

Primero resolver el problema real. Después escalar la solución cuando los datos demuestren que es necesario.

Esto es importante para evitar que el proyecto vuelva a convertirse en una arquitectura gigantesca que nunca llega a producción.

---

## Tabla Resumen de los 15 Principios

| # | Principio | Idea |
|---|---|---|
| 01 | Adaptación | El software se adapta a la empresa |
| 02 | Independencia del ERP | No sustituimos el ERP |
| 03 | No-code | Conectar sin programar |
| 04 | Núcleo común | Un core, muchos conectores |
| 05 | Modularidad | Conectores independientes |
| 06 | Propiedad de datos | Los datos pertenecen al cliente |
| 07 | Local + nube | No obligar a llevar todo a la nube |
| 08 | Seguridad | Security by design |
| 09 | Resiliencia | Fallar de forma segura |
| 10 | Observabilidad | Saber siempre qué ocurrió |
| 11 | Automatización | Automatizar antes que intervenir |
| 12 | IA responsable | IA cuando aporta valor |
| 13 | Una persona | Mantenerlo viable para un equipo pequeño |
| 14 | Simplicidad | Complejidad oculta |
| 15 | Evolución | Escalar cuando sea necesario |
