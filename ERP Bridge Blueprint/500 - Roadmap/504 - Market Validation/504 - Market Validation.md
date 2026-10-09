# 504 - Market Validation

## Estado

**Tipo:** Documento estratégico
**Estado:** Validación inicial
**Objetivo:** Determinar el mercado de entrada de ERP Bridge y las hipótesis que deben validarse antes de ampliar el producto.

---

# 1. Objetivo

ERP Bridge tiene una visión horizontal: convertirse en una capa de conectividad empresarial capaz de conectar ERPs, CRMs, ecommerce, logística, bases de datos, APIs y otros sistemas.

Sin embargo, el producto no debe intentar entrar en el mercado como una plataforma universal desde el primer día.

La estrategia inicial será:

> **Visión horizontal + entrada vertical.**

El primer mercado de entrada será la integración de software empresarial local o difícil de integrar con canales digitales, comenzando por:

```text
Factusol
    ↓
ERP Bridge
    ↓
WooCommerce
```

El objetivo de esta fase no es construir toda la plataforma.

El objetivo es demostrar que ERP Bridge puede resolver un problema empresarial real de forma repetible y rentable.

---

# 2. Hipótesis de mercado

La hipótesis principal es:

> Las empresas que utilizan software empresarial local o difícil de integrar necesitan conectar sus sistemas existentes con aplicaciones modernas sin sustituir su ERP ni realizar proyectos de desarrollo complejos.

ERP Bridge pretende resolver esta necesidad mediante una capa de conectividad que abstraiga las particularidades de cada sistema.

---

# 3. Problema inicial

Una empresa puede utilizar:

* ERP local.
* Ecommerce.
* CRM.
* Software de gestión.
* Sistemas de logística.
* Bases de datos.
* Aplicaciones externas.

Estos sistemas pueden funcionar correctamente de forma individual pero no comunicarse adecuadamente entre ellos.

Esto provoca:

* Introducción manual de datos.
* Duplicación de información.
* Errores de stock.
* Pedidos que no llegan correctamente.
* Falta de trazabilidad.
* Procesos administrativos repetitivos.
* Dependencia de desarrolladores.
* Costes de integración.

---

# 4. Cliente inicial

El cliente inicial de ERP Bridge será una PYME española que:

1. Utiliza Factusol.
2. Tiene o quiere tener un ecommerce.
3. Necesita sincronizar productos, stock, pedidos o clientes.
4. No quiere cambiar de ERP.
5. No quiere desarrollar una integración desde cero.
6. Prefiere una solución instalada y configurada que simplemente funcione.

No se intentará vender inicialmente a todo tipo de empresas.

---

# 5. Producto inicial

El primer producto comercial será:

> **Factusol ↔ WooCommerce mediante ERP Bridge.**

El sistema deberá permitir, como mínimo:

* Sincronización de productos.
* Sincronización de stock.
* Sincronización de pedidos.
* Sincronización de clientes cuando sea necesaria.
* Registro de operaciones.
* Detección de errores.
* Reintentos.
* Identificación de duplicados.
* Estado de sincronización.
* Reconciliación básica.

---

# 6. Principio arquitectónico importante

El Local Agent NO será obligatorio por definición.

ERP Bridge debe seleccionar el mecanismo de conexión adecuado según las capacidades del sistema externo.

Las posibles estrategias son:

```text
API
 │
 ├── API REST
 ├── API SOAP
 └── API específica del fabricante

LOCAL AGENT
 │
 ├── Bases de datos locales
 ├── Software legacy
 ├── ODBC
 └── Recursos locales

DATABASE
 │
 ├── Access
 ├── SQL Server
 ├── PostgreSQL
 └── Otros sistemas compatibles
```

La decisión de utilizar uno u otro mecanismo pertenece al Connector.

El Core debe permanecer independiente de esta decisión.

---

# 7. Factusol

Factusol es el primer Connector de referencia.

Debe investigarse exhaustivamente:

* API disponible.
* Permisos.
* Entidades disponibles.
* Operaciones de lectura.
* Operaciones de escritura.
* Limitaciones.
* Versiones.
* Acceso a base de datos.
* Access.
* ODBC.
* Límites.
* Autenticación.
* Requisitos del fabricante.
* Cambios entre versiones.

El Connector de Factusol debe utilizar la API oficial cuando ésta cubra adecuadamente una operación.

El Local Agent o acceso local se utilizará cuando sea técnicamente necesario.

---

# 8. Posicionamiento

ERP Bridge NO se posicionará inicialmente como:

> "El nuevo Zapier."

Tampoco:

> "Una plataforma universal de automatización."

Ni:

> "Un simple conector Factusol."

El posicionamiento inicial será:

> **Conecta tu ERP con tus canales digitales sin cambiar de software.**

Propuesta de valor:

> **Conecta tu ERP con ecommerce y aplicaciones modernas sin sustituir tu software actual ni desarrollar una integración desde cero.**

---

# 9. Diferenciación

La diferenciación inicial no será simplemente conectar sistemas.

Será proporcionar una integración operable y mantenible.

ERP Bridge debe proporcionar:

* Conectividad.
* Sincronización.
* Idempotencia.
* Reintentos.
* Logs.
* Diagnóstico.
* Reconciliación.
* Estado de las conexiones.
* Trazabilidad.
* Control de errores.
* Compatibilidad con software local.
* Abstracción mediante Connectors.

La propuesta es:

> **No solamente mover datos. Mantener la integración funcionando.**

---

# 10. Competencia

ERP Bridge reconoce tres grandes grupos de competidores.

## 10.1 Connectors específicos

Ejemplos:

* Factusol ↔ WooCommerce.
* Factusol ↔ PrestaShop.
* ERP ↔ ecommerce.

Estos competidores pueden tener una implementación muy sencilla y precios bajos.

ERP Bridge no debe competir únicamente por precio.

---

## 10.2 Plataformas de automatización

Ejemplos:

* Zapier.
* Make.
* n8n.
* Pipedream.

Estas plataformas proporcionan gran cantidad de integraciones y automatizaciones.

ERP Bridge no debe intentar replicar su amplitud inicialmente.

---

## 10.3 Plataformas iPaaS empresariales

Ejemplos:

* Workato.
* Boomi.
* Celigo.
* Otros.

Estas plataformas tienen una orientación empresarial y una complejidad y precio superiores.

ERP Bridge buscará inicialmente un segmento más pequeño:

> PYMEs que necesitan integrar software empresarial concreto sin contratar un proyecto de integración.

---

# 11. Ventaja competitiva potencial

Las ventajas que deben investigarse y validarse son:

### 11.1 Conocimiento de software empresarial español

Especialización en sistemas como:

* Factusol.
* Sage.
* Holded.
* Otros ERPs utilizados por PYMEs españolas.

### 11.2 Compatibilidad con software local

Capacidad de conectar sistemas que no fueron diseñados originalmente como servicios cloud.

### 11.3 Simplicidad

El cliente no debería necesitar conocimientos técnicos.

### 11.4 Operabilidad

El sistema debe permitir detectar y resolver problemas de sincronización.

### 11.5 Arquitectura modular

Un nuevo Connector no debe requerir modificar el Core.

---

# 12. MVP comercial

El MVP comercial será deliberadamente pequeño.

Debe soportar:

```text
Factusol
    ↓
ERP Bridge Core
    ↓
WooCommerce
```

No se construirán inicialmente:

* Marketplace.
* Constructor visual avanzado.
* Decenas de Connectors.
* IA avanzada.
* Automatizaciones complejas.
* Aplicación móvil.
* Multi-región.
* Arquitectura de microservicios.
* Kubernetes.
* Infraestructura distribuida.

---

# 13. MVP técnico

El MVP técnico debe demostrar:

### Core

* Connector Contract.
* Canonical Models.
* Connections.
* Mapping.
* Sync Engine.
* Idempotency.
* Error Handling.
* Logging.

### Connectors

* Factusol.
* WooCommerce.

### Operación

* Estado de conexión.
* Estado de sincronización.
* Historial.
* Reintentos.
* Reconciliación básica.

### Agent

Solo cuando una integración lo requiera.

---

# 14. Hipótesis que deben validarse

Antes de ampliar el producto deben validarse estas hipótesis:

### H1

Las empresas que utilizan Factusol tienen problemas reales de integración con ecommerce.

### H2

Están dispuestas a pagar por resolverlos.

### H3

Prefieren mantener su ERP actual antes que migrarlo.

### H4

Una solución de instalación/configuración sencilla resulta más atractiva que contratar desarrollo personalizado.

### H5

El problema se repite entre diferentes clientes.

### H6

Una misma arquitectura puede resolver múltiples instalaciones de Factusol sin modificaciones importantes.

### H7

El Core desarrollado para Factusol puede reutilizarse para otros Connectors.

### H8

El soporte necesario por cliente puede mantenerse suficientemente bajo para que el producto sea viable para un solo desarrollador.

---

# 15. Métricas de validación

Durante la fase inicial se buscará:

* 10 conversaciones con empresas potenciales.
* 5 demostraciones.
* 3 pilotos.
* Al menos 1 piloto pagado.
* Preferiblemente 3-5 clientes dispuestos a pagar.

No se considerará validado el producto simplemente porque alguien diga que "parece interesante".

La señal de validación principal será:

> **Una empresa entrega acceso y paga para utilizarlo.**

---

# 16. Precio experimental

El precio inicial no se considera definitivo.

Se probarán diferentes modelos.

### Modelo A

Setup + mensualidad.

### Modelo B

Pago único + mantenimiento.

### Modelo C

Mensualidad con instalación incluida.

### Modelo D

Precio por conexión.

La estructura comercial consolidada para validación es:

```text
Plan Base Anual:
249 €/año (199 €/año promo) · 3 puestos incluidos

Plan Base Mensual:
29 €/mes · 3 puestos incluidos

Setup Asistido (Opcional):
99 € (sesión remota de 45 min)
```

Estos precios eliminan la fricción de entrada en PYMEs y garantizan alta recurrencia sin almacenar datos de clientes en la nube (Zero-Knowledge).

---

# 17. Estrategia de entrada

La estrategia inicial será:

```text
Factusol
   ↓
WooCommerce
   ↓
Primeros clientes
   ↓
Repetibilidad
   ↓
Mejorar Core
   ↓
Segundo Connector
   ↓
Tercer Connector
   ↓
Plataforma
```

No se desarrollarán Connectors basándose únicamente en una lista de ideas.

El siguiente Connector debe seleccionarse utilizando:

* Demanda.
* Número de clientes potenciales.
* Dificultad técnica.
* Reutilización del Core.
* Precio potencial.
* Competencia.
* Coste de mantenimiento.

---

# 18. Estrategia de crecimiento

La visión de largo plazo sigue siendo:

```text
ERP
CRM
Ecommerce
Logística
Contabilidad
APIs
Bases de datos
IA
       ↓
 ERP BRIDGE
       ↓
Automatización empresarial
```

Pero esta visión no debe condicionar la implementación del MVP.

Cada nueva capacidad deberá justificarse mediante demanda real o una necesidad arquitectónica demostrada.

---

# 19. Criterio de expansión

ERP Bridge podrá ampliar su mercado cuando:

1. El Connector de Factusol sea estable.
2. Existan clientes reales.
3. El proceso de instalación sea repetible.
4. El soporte sea razonablemente bajo.
5. La arquitectura haya demostrado reutilización.
6. El Core no necesite cambios específicos para cada cliente.
7. Exista demanda de otro sistema.

---

# 20. Regla estratégica

> **No construir una plataforma para descubrir después si existe un mercado.**

ERP Bridge debe utilizar el primer Connector como mecanismo de validación comercial y técnica.

La plataforma crecerá a partir de problemas repetidos.

---

# 21. Decisión de producto

Estado actual:

**GO WITH CHANGES**

Cambios principales:

1. Entrada vertical antes de expansión horizontal.
2. Factusol + WooCommerce como primer mercado.
3. Core mínimo reutilizable.
4. Agent opcional según el sistema.
5. Prioridad absoluta a operabilidad y diagnóstico.
6. Validación comercial antes de construir múltiples Connectors.
7. No competir frontalmente con Zapier, Make o n8n.
8. No construir Marketplace hasta que exista una base real de Connectors y clientes.

---

# 22. Objetivo de los próximos 90 días

El objetivo no es:

> "Terminar ERP Bridge."

El objetivo es:

> **Demostrar que una integración productizada puede venderse y repetirse.**

Resultado ideal:

```text
Factusol
    ↓
ERP Bridge
    ↓
WooCommerce

       +

Clientes reales

       +

Ingresos recurrentes

       +

Core reutilizable
```

Si estas condiciones se cumplen, se justifica continuar hacia v1.

Si no se cumplen, deberá revisarse el posicionamiento, mercado o producto antes de invertir más recursos.

---

# 23. Fuente de verdad

Este documento complementa:

* 101 - Vision.
* 104 - Product Scope.
* 106 - Use Cases.
* 201 - Business Model.
* 204 - Go To Market.
* 206 - Legal, Trademarks & IP Protection.
* 207 - Tax, Autónomo & Pluriactivity Operations.
* 208 - Launch Playbook & Regional Execution.
* 301 - System Overview.
* 303 - Agent.
* 304 - Connectors.
* 310 - Synchronization.
* 501 - MVP.
* 502 - v1.

Si este documento contradice alguna decisión anterior, deberá revisarse el documento original correspondiente.

No deben modificarse decisiones arquitectónicas automáticamente.

Las contradicciones deben identificarse y resolverse explícitamente.

---

# 24. Principio final

> **Primero demostrar que funciona para una empresa. Después demostrar que funciona para diez. Después construir la plataforma para cien.**
