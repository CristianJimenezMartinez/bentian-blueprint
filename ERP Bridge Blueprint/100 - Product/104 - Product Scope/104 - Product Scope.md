# Product Scope — v1.0
> **Estado:** Borrador

---

# 104 - Product Scope

> *Alcance del producto, módulos e inclusiones/exclusiones.*

---

## 1. Propósito

Este documento define el alcance funcional de ERP Bridge.

Su objetivo es establecer qué problemas resolverá el producto, qué capacidades formarán parte de él y qué funcionalidades quedan fuera de su alcance inicial.

El alcance podrá evolucionar mediante versiones posteriores, pero cualquier ampliación deberá evaluarse antes de incorporarse al producto.

> **Regla de oro:** Si algo no está dentro del alcance, no lo construimos durante el MVP aunque sea una idea excelente.

---

## 2. Producto Principal

ERP Bridge será una plataforma que permitirá:

> **Conectar sistemas empresariales y automatizar el intercambio de información y la ejecución de procesos entre ellos sin necesidad de desarrollar una integración desde cero para cada caso.**

---

## 3. Sistemas que ERP Bridge Podrá Conectar

El producto deberá estar diseñado para poder conectar diferentes categorías de sistemas:

- **ERP**: Factusol, Sage, Odoo, otros ERPs locales o cloud.
- **Ecommerce**: WooCommerce, Shopify, otros.
- **CRM**: HubSpot, Salesforce, CRMs propios, otros.
- **Servicios externos**: Plataformas de pago, servicios de logística, Email, WhatsApp, APIs externas.
- **IA**: Modelos de IA, Agentes, Servicios de generación, Sistemas internos basados en IA.
- **Sistemas propios**: Aplicaciones desarrolladas específicamente para una empresa.

---

## 4. Capacidades Principales

ERP Bridge deberá permitir, progresivamente:

- **Conectar**: Establecer conexiones entre diferentes sistemas.
- **Leer información**: Obtener información de un sistema conectado (Clientes, Productos, Pedidos, Stock, Facturas).
- **Escribir información**: Crear o modificar información en otro sistema.
- **Sincronizar**: Mantener información sincronizada entre sistemas.
- **Transformar**: Convertir datos de un formato a otro.
- **Automatizar**: Crear flujos que ejecuten procesos automáticamente.
- **Gestionar errores**: Detectar fallos y permitir recuperación mediante reintentos y mecanismos de control.
- **Monitorizar**: Permite al usuario conocer el estado de sus conexiones y automatizaciones.
- **Extender**: Permite añadir nuevos conectores sin modificar el núcleo de ERP Bridge.

---

## 5. Experiencia que Debe Proporcionar

El objetivo de ERP Bridge es que una empresa pueda realizar una integración siguiendo un proceso sencillo:

```text
1. Elegir sistema origen
          ↓
2. Elegir sistema destino
          ↓
3. Conectar cuentas
          ↓
4. Seleccionar información
          ↓
5. Configurar reglas
          ↓
6. Probar
          ↓
7. Activar
```

El usuario no debería necesitar conocer la implementación técnica que existe detrás.

---

## 6. Primer Caso de Uso

El primer caso de uso real será:

> **Conectar Factusol con un ecommerce.**

Esto es especialmente importante porque ERP Bridge no empieza desde cero:
- Ya existe un conector de Factusol.
- Existe experiencia real trabajando con su base de datos Access.

El objetivo será utilizar este caso para construir y validar el núcleo común de ERP Bridge.

---

## 7. MVP (Producto Mínimo Viable)

El MVP **NO** será *"conectar cualquier cosa con cualquier cosa"*. Eso sería demasiado grande.

El MVP deberá demostrar que el concepto funciona.

```text
ERP
 │
 │
 ▼
ERP Bridge
 │
 │
 ▼
Ecommerce
```

### El MVP incluirá:
- Un núcleo común (Core mínimo reutilizable).
- Primeros conectores del MVP comercial: **Factusol** y **WooCommerce**.
- Local Agent como mecanismo opcional de conectividad local (utilizado cuando sea necesario acceder a recursos locales, bases de datos Access u operaciones no cubiertas por la API oficial).
- Gestión de credenciales y autenticación.
- Mapeo de datos canónico.
- Motor de sincronización con reintentos e idempotencia.
- Reconciliación básica de datos.
- Flujos básicos de automatización.
- Gestión y visibilidad de errores.
- Dashboard administrativo básico.
- Logs y monitorización de conexiones.

---

## 8. Fuera del MVP

Aunque formen parte de la visión futura, **NO** serán objetivos del primer lanzamiento:

- ❌ **CRM completo**: ERP Bridge puede conectarse a CRMs, no pretende convertirse en un CRM.
- ❌ **Ecommerce propio**: ERP Bridge se conecta a ecommerce, no pretende competir con Shopify o WooCommerce.
- ❌ **ERP propio**: No construiremos un ERP.
- ❌ **Sistema contable**: No construiremos nuestro propio sistema contable.
- ❌ **Sistema logístico**: Podremos conectarnos con sistemas logísticos, no construiremos uno propio inicialmente.
- ❌ **IA autónoma avanzada**: La IA podrá formar parte de la plataforma, pero no será el núcleo del MVP.
- ❌ **Marketplace completo**: La arquitectura deberá permitirlo en el futuro, pero no necesitamos construir un marketplace complejo en la primera versión.
- ❌ **Cientos de conectores**: No necesitamos 100 conectores para demostrar que el producto funciona. Necesitamos los conectores adecuados.

---

## 9. Qué Sí Queremos Conseguir con el MVP

Al terminar el MVP deberíamos poder demostrar algo como:

1. Una empresa tiene un ERP que utiliza diariamente y un ecommerce.
2. Instala/configura ERP Bridge.
3. Conecta ambos sistemas.
4. Configura qué información quiere sincronizar.
5. Activa el flujo.
6. Los sistemas empiezan a comunicarse automáticamente.
7. Si ocurre un error, ERP Bridge lo detecta y lo muestra.

> **Si conseguimos esto de forma estable, sencilla y repetible, tenemos un producto.**

---

## 10. Principio de Expansión

Una vez validado el MVP, la expansión se realizará de forma progresiva:

```text
       MVP: Factusol → Ecommerce
                   ↓
           Nuevos conectores
                   ↓
                  CRM
                   ↓
                Logística
                   ↓
                 Pagos
                   ↓
                   IA
                   ↓
        Automatización avanzada
```

La expansión deberá producirse principalmente mediante nuevos conectores y capacidades reutilizables, no mediante proyectos completamente independientes.

---

## 11. Criterio para Añadir Funcionalidades

Antes de incorporar una nueva funcionalidad al producto se deberán responder las siguientes preguntas:

- ¿Resuelve un problema real?
- ¿Es necesario para el usuario?
- ¿Puede reutilizarse para otros conectores?
- ¿Aumenta el valor del núcleo?
- ¿Complica innecesariamente el producto?
- ¿Debe estar en el MVP o puede esperar?

> **Si no es necesaria para validar el producto, puede esperar.**

---

## 12. Definición de Éxito del MVP

El MVP será considerado exitoso cuando:

> **Una empresa pueda conectar sus sistemas y automatizar un proceso empresarial real sin necesidad de desarrollar una integración personalizada.**

### Métricas de éxito adicionales:
- La instalación sea reproducible.
- La configuración sea comprensible.
- Los errores sean visibles.
- Los datos no se pierdan.
- El sistema pueda mantenerse sin reconstruirlo para cada cliente.
- **Validación del Core:** El segundo conector (que se abordará tras validar comercialmente el primero con clientes de pago) deberá requerir significativamente menos esfuerzo gracias a la reutilización del Core.

*(Esta última métrica es **MUY importante** para demostrar que hemos construido una plataforma, no simplemente otro conector).*

---

## 13. Regla de Alcance

> **ERP Bridge no se construirá intentando resolver todos los problemas empresariales desde el principio.**
>
> **Primero demostrará que puede conectar y automatizar correctamente un caso real. Después crecerá sobre esa base.**
