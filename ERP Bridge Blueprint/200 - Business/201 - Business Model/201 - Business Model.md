# Business Model — v1.0
> **Estado:** Borrador

---

# 201 - Business Model

> *Modelo de negocio y estrategia de monetización.*

---

## 1. Propósito

Definir cómo ERP Bridge puede generar ingresos de forma sostenible sin comprometer sus principios de producto: facilidad de implementación, independencia del ERP, despliegue local/cloud/híbrido y ausencia de dependencia de desarrolladores para el usuario final.

---

## 2. Principio Económico Fundamental

ERP Bridge no debe cobrar al cliente simplemente por almacenar sus datos en nuestra infraestructura.

Esto es especialmente importante para empresas que utilizan ERPs locales como Factusol.

El valor que proporcionamos es:
- Conectividad.
- Automatización.
- Conectores.
- Actualizaciones.
- Seguridad.
- Mantenimiento.
- Soporte.
- Infraestructura cuando sea necesaria.

---

## 3. Modelo Principal

ERP Bridge podrá utilizar un modelo híbrido de ingresos, combinando:

### A. Software
El cliente paga por utilizar ERP Bridge mediante:
- Licencia.
- Suscripción.
- Pago anual.
- Modelo híbrido.

*(El modelo definitivo dependerá del tipo de instalación).*

### B. Conectores
Algunos conectores podrán ser:
- Gratuitos.
- Incluidos en el plan.
- De pago.
- Premium.

Esto permitirá que el valor del producto pueda crecer mediante el ecosistema de conectores.

### C. Servicios de Implementación
Para empresas que necesiten ayuda:
- Instalación.
- Configuración.
- Migraciones puntuales.
- Integraciones especiales.
- Formación.

*(Estos servicios podrán cobrarse independientemente del software).*

### D. Soporte y Mantenimiento
Los clientes podrán contratar servicios adicionales:
- Soporte prioritario.
- Mantenimiento.
- Monitorización.
- Actualizaciones gestionadas.
- Asistencia técnica.

---

## 4. Modelo Local

Cuando ERP Bridge se instale localmente:

```text
Cliente
   ↓
Compra/licencia ERP Bridge
   ↓
Instalación local
   ↓
Uso
```

El cliente no necesariamente necesita contratar infraestructura cloud para utilizar el núcleo local.

Podrá existir una cuota opcional por:
- Actualizaciones.
- Soporte.
- Servicios cloud.
- Monitorización.
- Gestión remota.
- Funciones premium.

---

## 5. Modelo Cloud

Cuando el cliente utilice componentes cloud:

```text
Cliente
   ↓
ERP Bridge Cloud
   ↓
Suscripción
```

La suscripción puede cubrir:
- Infraestructura.
- APIs.
- Dashboard.
- Automatizaciones cloud.
- Monitorización.
- Eventos.
- Servicios adicionales.

---

## 6. Modelo Híbrido

Este probablemente será uno de los modelos más interesantes y diferenciales:

```text
                 ERP BRIDGE
                     │
           ┌─────────┴─────────┐
           │                   │
        LOCAL                 CLOUD
           │                   │
        Factusol          Dashboard/API
        Access             Automatización
           │                   │
           └─────────┬─────────┘
                     │
                Ecommerce
```

El cliente puede mantener los datos sensibles o sistemas antiguos localmente mientras utiliza determinados servicios cloud, adaptando el coste a las necesidades reales.

---

## 7. Modelo Gratuito

ERP Bridge podrá ofrecer determinados componentes gratuitamente:
- Core básico.
- Conectores iniciales.
- Herramientas de desarrollo.
- SDK.
- Documentación.

> **Objetivo:** Reducir la barrera de entrada y aumentar la adopción de ERP Bridge. ("Gratis" no significa que todo el producto deba serlo).

---

## 8. Estrategia de Adopción

```text
Gratis ──> Primer conector ──> Primera automatización ──> Uso real ──> Necesidad de más capacidades ──> Plan de pago
```

> **Prioridad inicial:** Conseguir **usuarios + instalaciones + conectores + casos reales**, antes que maximizar ingresos inmediatos.

---

## 9. Ecosistema de Conectores (Marketplace Futuro)

```text
ERP Bridge Marketplace
│
├── ERPs
├── Ecommerce
├── CRM
├── Logística
├── Pagos
├── IA
└── Otros
```

### Tipos de Conectores:
- **Oficiales:** Desarrollados y mantenidos por ERP Bridge.
- **Terceros:** Desarrollados por empresas o desarrolladores externos.

### Vías de Monetización:
- Venta de conectores.
- Suscripciones.
- Comisión por ventas en marketplace.
- Servicios profesionales.

*(Este modelo no forma parte del MVP).*

---

## 10. Modelo para Desarrolladores

Los desarrolladores podrán utilizar ERP Bridge para crear integraciones para sus propios clientes.

A largo plazo podrán disponer de:
- SDK gratuito.
- Documentación y Sandbox.
- Marketplace y publicación de conectores.
- Licencias comerciales.

Esto permitirá que el crecimiento de ERP Bridge no dependa exclusivamente del equipo central.

---

## 11. Modelo para Agencias e Integradores

Una agencia podrá utilizar ERP Bridge como plataforma común para implementar integraciones en múltiples clientes:

```text
Agencia
   │
   ├── Cliente A
   ├── Cliente B
   ├── Cliente C
   └── Cliente D
```

La agencia podrá pagar por capacidades adicionales o utilizar un modelo profesional, convirtiéndose en un canal de distribución clave.

---

## 12. Fuentes Potenciales de Ingresos

| Fuente | MVP | Futuro |
|---|:---:|:---:|
| **Licencia local** | ⭐⭐⭐ | |
| **Suscripción cloud** | ⭐⭐⭐ | |
| **Conectores premium** | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Implementación** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Soporte** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Mantenimiento** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Marketplace** | | ⭐⭐⭐⭐⭐ |
| **Comisión de conectores** | | ⭐⭐⭐⭐ |
| **SDK / Herramientas profesionales** | | ⭐⭐⭐ |
| **Servicios cloud** | ⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 13. Qué NO Queremos Hacer

ERP Bridge no debe depender exclusivamente de:

- ❌ **Cobrar grandes proyectos personalizados:** Evitar convertir cada cliente en un proyecto a medida diferente.
- ❌ **Cobrar únicamente por almacenamiento:** El cliente no debe estar obligado a subir su ERP completo.
- ❌ **Crear un SaaS obligatorio:** El producto debe poder funcionar también localmente.
- ❌ **Crear una consultora alrededor del producto:** Los servicios pueden existir, pero ERP Bridge debe ser un producto de software escalable.

---

## 14. Objetivo Económico

El objetivo es transformar el modelo de negocio:

### ⚠️ De (Modelo Consultora):
```text
1 cliente ──> 1 integración personalizada ──> Mucho trabajo no escalable
```

### ✅ A (Modelo Producto Escalable):
```text
ERP Bridge ──> 1 conector ──> 100 empresas
     ↓
ERP Bridge ──> 100 conectores ──> Miles de empresas
```

El crecimiento debe producirse principalmente mediante **reutilización de software**, no mediante el aumento proporcional de horas de trabajo.

---

## 15. Métrica Económica Fundamental

> **Coste marginal de incorporar un nuevo cliente.**
>
> - Si incorporar el cliente número 100 requiere prácticamente el mismo trabajo que el número 10, estamos construyendo un **producto escalable**.
> - Si cada cliente requiere programación manual, estamos construyendo una **consultora**.

---

## 16. Modelo Comercial Consolidado (Lanzamiento y Validación)

### Plan Base Flat-Rate (Todo Incluido y Cero Fricción):
La propuesta de valor se basa en eliminar límites artificiales y respetar estrictamente la **privacidad total del cliente**:

* **Plan Base Anual:** 249 €/año (Promoción de Lanzamiento Early-Bird: **199 €/año** + IVA).
* **Plan Base Mensual:** **29 €/mes** + IVA (sin compromiso de permanencia).
* **Puestos Incluidos:** **Hasta 3 puestos locales de trabajo** (Mostrador, Almacén, Administración) bajo la misma licencia empresarial con vinculación criptográfica por hardware (HWID).
* **Alcance de Sincronización:** Catálogo y pedidos **ilimitados** (sin recargos por volumen de SKUs ni transacciones).
* **Add-ons y Servicios:**
  * **Conexión Multi-Tienda (Mismo CIF):** Cada tienda online requiere 1 Licencia Base independiente (con un 20% de descuento por fidelización en la 2ª tienda bajo el mismo CIF/empresa).
  * **Puesta en Marcha Asistida (Setup One-Off):** 99 € (sesión remota guiada de 45 min por AnyDesk con un ingeniero).
* **Prueba Gratuita:** 14 días de prueba completa sin necesidad de introducir tarjeta de crédito.

### Principio Sagrado: Cero Almacenamiento de Datos de Negocio (Zero-Knowledge Architecture)
Bentian **NUNCA ingesta, copia, almacena ni aloja datos de negocio del cliente** (productos, precios, clientes, facturas ni bases de datos de Factusol) en sus servidores cloud.
* Toda la sincronización se procesa **en local (Edge Processing)** en la máquina del cliente y se transmite de forma directa y cifrada a su tienda online mediante HTTPS.
* La nube de Bentian actúa exclusivamente como verificador de licencias/puestos (Stripe) y distribuidor de actualizaciones firmadas (Ed25519). Cero datos de negocio en servidores externos.

---

## 17. Principio Final

> **El modelo de negocio debe crecer alrededor del producto, no obligar al producto a funcionar como una máquina de facturación.**
>
> **Prioridad:** Producto ──> Usuarios ──> Validación ──> Adopción ──> Monetización.
