# Pricing — v1.0
> **Estado:** Borrador

---

# 203 - Pricing

> *Estructura de precios y planes.*

---

## 1. Propósito

Definir la estrategia de precios de ERP Bridge para un producto de instalación local, mantenido por una sola persona, sin costes de infraestructura cloud obligatoria.

> **Principio:** El precio debe reflejar el valor entregado. El coste operativo para nosotros es prácticamente cero porque el producto corre en el equipo del cliente.

---

## 2. Modelo de Despliegue

ERP Bridge es un producto **local-first**:

```text
┌─────────── PC DEL CLIENTE ───────────┐
│                                       │
│  Factusol (.accdb)                    │
│     │                                 │
│  ERP Bridge (Core + Agent)            │
│     │                                 │
│     ├── Lee/escribe en Factusol       │
│     ├── Llama a WooCommerce API       │
│     └── Dashboard en localhost        │
│                                       │
└───────────────────────────────────────┘
```

- Los datos del cliente **nunca salen de su máquina**.
- No se requiere VPS, cloud ni infraestructura externa.
- WooCommerce se consulta vía polling (HTTPS saliente).
- El coste de infraestructura para nosotros es **€0**.

---

## 3. Contexto de Mercado

### Cuánto pagan las PYMEs españolas por software similar

| Competidor / Alternativa | Precio | Modelo |
|---|---|---|
| **Factusol Soporte Digital** | ~€240 / año (~€20/mes) | Licencia anual |
| **Factusol Soporte Telefónico** | ~€588 / año (~€49/mes) | Licencia anual |
| **Festeweb (FS2PS)** | €149 – €299 / año | Licencia anual |
| **QFACWIN Gestión Total** | €825 / año | Licencia anual |
| **Conecta HUB** | €49 – €150 / mes | SaaS mensual |
| **O2W / MusInWeb** | €35 – €120 / mes + setup | SaaS + setup |

### Observaciones clave

1. Las PYMEs españolas con Factusol ya pagan **€240-588/año** solo por soporte y actualizaciones.
2. Los conectores existentes (Festeweb) cobran **€149-299/año** por sincronización básica.
3. Las soluciones SaaS (Conecta HUB) cobran **€49-150/mes** — mucho más caro.
4. Ninguno ofrece la combinación de: dashboard moderno + logs + reintentos + instalación local sin cloud.

> **ERP Bridge ocupa un hueco real:** más moderno que Festeweb, más barato que Conecta HUB, y sin obligar a subir datos a la nube.

---

## 4. Filosofía de Pricing

1. **No hay coste cloud para nosotros.** El producto corre en local. Nuestro margen es prácticamente del 100%.
2. **Podemos ser más baratos que la competencia** sin sacrificar margen.
3. **Cobrar por valor, no por coste.** Si ahorramos 10-15 horas/mes de trabajo manual, €20-40/mes es una obviedad.
4. **Modelo de licencia anual** como estándar (es lo que conocen los usuarios de Factusol). Con opción mensual para quien lo prefiera.

---

## 5. Estructura de Planes: Plan Único Modular (Flat Rate)

Para evitar la complejidad de gestión y fricción de soporte que suponen los límites arbitrarios de productos (2.000 vs 10.000 artículos) en un software cuyo coste de servidor es **0 €**, ERP Bridge adopta un **modelo de Plan Único Todo Incluido**:

---

### Plan Base: Todo Incluido (1 Conexión ERP ↔ 1 Tienda Online)

| Característica | Detalle Incluido |
|---|---|
| **Precio Anual (Recomendado)** | **249 € / año + IVA** (~20,75 €/mes) · *Lanzamiento Early Bird: 199 € / año* |
| **Precio Mensual (Flexible)** | **29 € / mes + IVA** (sin permanencia) |
| **Puestos de Trabajo Incluidos** | **Hasta 3 puestos locales de trabajo** bajo la misma empresa (vinculación HWID y self-service unbind) |
| **Límite de Catálogo** | **PRODUCTOS ILIMITADOS** (sin cortes por número de referencias) |
| **Límite de Operaciones** | **PEDIDOS Y CLIENTES ILIMITADOS** |
| **Frecuencia de Sincronización** | Automática en tiempo real o programable (desde 1 minuto) |
| **Conexión Incluida** | 1 ERP (Factusol, SimplyGest, etc.) ↔ 1 Tienda Online (WooCommerce, PrestaShop) |
| **Dashboard y Logs** | Completo (logs en vivo, historial, reintentos automáticos, alertas) |
| **Mapeo Avanzado** | Tarifas B2B, stocks, atributos (tallas y colores) y Recargo de Equivalencia |
| **Soporte y Actualizaciones** | Email directo (24-48h laborables) + Actualizaciones continuas incluidas |

> **Posicionamiento:** Cero fricción mental ("No-Brainer"). Más moderno y transparente que Festeweb (que cobra soporte y penalizaciones), cuatro veces más económico que Conecta HUB (900 € - 2.000 €/año) y sin obligar a subir bases de datos a la nube.

---

### Multi-Canal: Regla de Oro (1 Tienda = 1 Licencia Base)

Para garantizar aislamiento de sincronización, evitar fraudes por subarriendo de agencias y dimensionar el soporte técnico con precisión:

> **Regla de Oro:** Cada Tienda Online conectada requiere **1 Licencia Base independiente**.

| Concepto | Precio | Descripción |
|---|---|---|
| **1ª Tienda Online (Plan Base)** | **199 € / año** (Promo) · 249 € | Conexión completa de 1 ERP ↔ 1 Tienda Online con catálogo y pedidos ilimitados |
| **2ª Tienda Online (Mismo CIF)** | **-20% Descuento Fidelización** (159 € / año) | Para empresas legítimas que operan canales B2B y B2C bajo el mismo CIF y ERP |

---

### Servicios de Implantación Asistida (Opcionales)

Para clientes o comercios que prefieren un servicio "llave en mano":

| Servicio | Precio | Descripción |
|---|---|---|
| **Puesta en Marcha / Setup Asistido** | **99 € + IVA** (Pago único) | Sesión remota de 45 min por AnyDesk: instalación del Agent, vinculación con el ERP y la tienda, prueba de un pedido en vivo y verificación de stock |
| **Implementación Completa & Mapeo VIP** | **249 € + IVA** (Pago único) | Instalación + configuración avanzada de matrices de tallas/colores y tarifas B2B personalizadas + soporte prioritario durante los primeros 30 días |
| **Desarrollo de Conector a Medida** | Bajo presupuesto | Conector específico para ERP local no soportado en el catálogo oficial |

---

## 6. Economía del Modelo

### Coste operativo por cliente

```text
Infraestructura cloud:     ~4,50 €/mes FIJO TOTAL (VPS Hetzner + Supabase Free + Cloudflare)
Almacenamiento de datos:   0 € (Arquitectura Zero-Knowledge: los datos de ERP quedan en local)
Soporte (1 persona):       Tiempo propio
Distribución:              0 € (Cloudflare R2 / releases directas sin coste de egress)
Stripe:                    1,4% + 0,25 € por transacción
───────────────────────────────────────────
Coste variable por cliente: ~3-5 € / año (comisión pasarela Stripe)
```

### Margen del Modelo

| Modalidad | Ingreso anual | Coste variable (Stripe) | Margen Neto |
|---|---|---|---|
| **Plan Base Anual** | 249 € | ~4 € | **~98,4%** |
| **Plan Base Mensual** | 348 € (29€ x 12) | ~8 € | **~97,7%** |
| **2ª Tienda Fidelización (-20%)** | 159 € | ~2,5 € | **~98,4%** |
| **Setup Asistido** | 99 € | ~1,6 € | **~98,4%** |

> **Sin costes de servidores en la nube para procesar datos, el margen es prácticamente del 100%.** Esto constituye la mayor ventaja competitiva frente a SaaS intermediarios.

### Escenarios de Ingresos con Plan Único

| Clientes | Estructura estimada | MRR Equivalente | ARR |
|---|---|---|---|
| **10** | 10 Licencias Base + 3 setups | ~275 € | ~3.300 € |
| **25** | 25 Licencias Base + 8 setups | ~720 € | ~8.600 € |
| **50** | 50 Licencias Base (con clientes multicanal) | ~1.450 € | ~17.400 € |
| **100** | 100 Licencias Base | ~2.900 € | ~34.800 € |

---

## 7. Decisiones de Pricing

### Sin cuota de alta

Las PYMEs españolas rechazan los costes de setup. **ERP Bridge no cobra cuota de alta.**

### Prueba gratuita de 14 días

Todos los planes incluyen **14 días de prueba completa** sin necesidad de tarjeta de crédito ni compromiso. Al expirar los 14 días, el cliente decide si activar la suscripción o desinstalar el agente.

### Descuento anual

El precio anual supone un **~20% de descuento** sobre el mensual. Incentiva la retención.

### Facturación

- Facturación mediante **Stripe** (ya integrado en el código existente).
- Facturas conformes con normativa española.
- IVA del 21% para clientes españoles.

---

## 8. Evolución del Pricing

### Fase MVP (Pre-lanzamiento)

```text
Beta 100% gratuita → 5-10 empresas → Feedback → Testimonios → Primer caso de éxito
```

No se cobra nada durante la beta. Se ofrece a cambio de feedback y permiso para usar como referencia.

### Fase Comercial (Post-MVP)

Activación de planes de pago comenzando por el Plan Base (199 €/año promo o 29 €/mes).

### Fase Expansión (Futuro)

- Conectores premium para otros ERPs.
- Servicios de implementación para empresas más grandes.
- Programa de partners/agencias con comisiones.

---

## 9. Principio Final

> **El producto corre en local. Nuestro coste es casi cero. Podemos ser más baratos que cualquier SaaS y aun así tener márgenes excelentes. El precio nunca debe ser la razón por la que una PYME deje de usar ERP Bridge.**
