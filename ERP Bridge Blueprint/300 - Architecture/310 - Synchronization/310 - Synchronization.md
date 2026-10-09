# Synchronization — v1.0
> **Estado:** Borrador

---

# 310 - Synchronization

> *Motor de sincronización, delta sync, resolución de conflictos y consistencia de datos.*

---

## 1. Propósito

El sistema de Synchronization permite mantener datos coherentes entre dos o más sistemas conectados mediante ERP Bridge.

Ejemplo:

Factusol
```text
   ↓
```
ERP Bridge
```text
   ↓
```
WooCommerce

---

## 2. Problema

Las empresas suelen tener información repartida:

ERP
```text
├── Productos
├── Stock
├── Clientes
└── Pedidos
```


Ecommerce
```text
├── Productos
├── Stock
├── Clientes
└── Pedidos
```


CRM
```text
├── Clientes
└── Actividades
```

El problema es mantenerlos sincronizados.

ERP Bridge actúa como intermediario.

---

## 3. Principio fundamental

ERP Bridge no debe obligar a una empresa a cambiar su sistema principal.

Si una empresa utiliza Factusol:

Factusol
```text
    ↓
```
ERP Bridge
```text
    ↓
```
Ecommerce

Factusol continúa siendo su ERP.

---

## 4. Dirección de sincronización

La sincronización puede ser:

Unidireccional
```text
ERP → Ecommerce
```

Ejemplo:

Productos
Stock
Inversa
```text
Ecommerce → ERP
```

Ejemplo:

Pedidos
Bidireccional
ERP ↔ Ecommerce

Ejemplo:

Clientes

---

## 5. Source of Truth

Cada entidad debe tener definido cuál es el sistema principal.

Ejemplo:

Productos
ERP = Source of Truth


Stock
ERP = Source of Truth


Pedidos
Ecommerce = Source of Truth


Clientes
ERP = Source of Truth

Esto evita conflictos.

---

## 6. Ejemplo completo

FACTUSOL
                │
        Product / Stock
```text
                │
                ↓
```
           ERP Bridge
```text
                │
                ↓
```
           WOOCOMMERCE

Y:

WOOCOMMERCE
     │
   Order
```text
     ↓
```
ERP Bridge
```text
     ↓
```
FACTUSOL

---

## 7. Sync Job

Una sincronización podrá representarse como un Job.

Sync Job
```text
├── Source
├── Destination
├── Entity
├── Direction
├── Frequency
├── Mapping
└── Status
```

Ejemplo:

```text
Factusol → WooCommerce
```
Entity: Products
Frequency: 5 min
Status: Active

---

## 8. Real-time

Cuando los sistemas soporten eventos:

Factusol
```text
   ↓
```
Event
```text
   ↓
```
ERP Bridge
```text
   ↓
```
WooCommerce

El cambio puede propagarse prácticamente inmediatamente.

No debemos prometer tiempo real cuando el sistema conectado no lo permite.

---

## 9. Polling

Cuando no existan eventos:

Every 5 minutes


ERP Bridge
```text
   ↓
```
Ask ERP:
"What changed?"

Entonces:

Changes
```text
 ↓
```
Process
```text
 ↓
```
Destination

---

## 10. Incremental Sync

No debemos descargar toda la información cada vez.

Incorrecto:

100.000 productos
```text
      ↓
```
Descargar todos
```text
      ↓
```
Comparar

Preferible:

Última sincronización
```text
       ↓
```
¿Qué cambió?
```text
       ↓
```
Solo esos registros

Siempre que el sistema de origen lo permita.

---

## 11. Full Sync

También deberá existir sincronización completa cuando sea necesario.

Por ejemplo:

First connection
```text
      ↓
```
Full Sync
```text
      ↓
```
100.000 products

Después:

Incremental Sync

---

## 12. Initial Sync

La primera conexión debe tener un proceso especial.

Connect
```text
  ↓
```
Discover data
```text
  ↓
```
Map
```text
  ↓
```
Preview
```text
  ↓
```
Confirm
```text
  ↓
```
Initial Sync

No debemos empezar a modificar datos sin confirmación del usuario cuando exista riesgo.

---

## 13. Mapping

La sincronización necesita mapear campos.

Ejemplo:

Factusol             WooCommerce


```text
Código       ─────→  SKU
Descripción  ─────→  Name
PVP          ─────→  Price
Existencias  ─────→  Stock
```

---

## 14. Automatic Mapping

ERP Bridge debería intentar realizar automáticamente el mapping.

Ejemplo:

Detecting fields...


```text
Código → SKU ✓
Descripción → Name ✓
PVP → Price ✓
Existencias → Stock ✓
```

El usuario solo deberá intervenir en excepciones.

---

## 15. Data Transformation

Los sistemas pueden representar los datos de manera diferente.

Ejemplo:

ERP:
12,50


Ecommerce:
"12.50"

ERP Bridge deberá transformar el dato.

---

## 16. Entity Identity

ERP Bridge debe saber cuándo dos registros representan el mismo objeto.

Ejemplo:

Factusol Product
ID: 1234


```text
        ↓
```


ERP Bridge


```text
        ↓
```


WooCommerce Product
ID: 5678

Debe guardar la relación:

1234 ↔ 5678

Esto evita duplicados.

---

## 17. External IDs

Cada sistema puede tener su propio identificador.

ERP Bridge deberá mantener relaciones entre IDs.

Conceptualmente:

ERP Bridge Mapping


Entity:
Product


ERP ID:
1234


WooCommerce ID:
5678

---

## 18. Conflict Resolution

Puede ocurrir:

ERP:
Stock = 20


Ecommerce:
Stock = 18

ERP Bridge necesita saber qué hacer.

Si el ERP es Source of Truth:

ERP = 20
```text
     ↓
```
Ecommerce = 20

---

## 19. Conflict Policies

Podremos definir políticas:

Source wins
Destination wins
Latest change wins
Manual resolution

No todas serán apropiadas para todos los datos.

---

## 20. Sync Errors

Si falla una operación:

Product #123
```text
      ↓
```
ERROR

No debería detener toda la sincronización.

Preferible:

100 products


97 ✓
2 ✓
1 ✗

Y después:

Retry failed item

---


---

## 21. Reconciliación Básica de Datos

Además de procesar eventos en tiempo real o cambios incrementales, ERP Bridge debe proporcionar un mecanismo de **reconciliación periódica** para garantizar que el estado en destino refleja con fidelidad la realidad del origen.

### Objetivo de la Reconciliación
Detectar divergencias silenciosas provocadas por:
* Modificaciones manuales realizadas directamente en WooCommerce o en Factusol sin pasar por ERP Bridge.
* Errores transitorios no recuperados.
* Caídas prolongadas de conexión durante periodos de alta actividad.

### Funcionamiento Básico:
1. **Auditoría de consistencia:** El sistema compara a intervalos regulares (o bajo demanda) identificadores clave y estados (ej. existencias de stock de productos activos o estado de pedidos recientes).
2. **Detección de divergencias:** Identifica registros desalineados sin alterar datos de forma destructiva.
3. **Registro y visibilidad:** Reporta las discrepancias en el Dashboard de forma comprensible.
4. **Acción correctiva:** Permite al usuario forzar la re-sincronización guiada aplicando la política de *Source of Truth*.

---

## 22. Retry

Las operaciones fallidas deberán poder reintentarse automáticamente.

Attempt 1
```text
   ↓
```
Failed
```text
   ↓
```
Retry
```text
   ↓
```
Success

Los límites y tiempos se definirán técnicamente.

---

## 23. Dead Letter

Si continúa fallando:

Failed
```text
 ↓
```
Retry
```text
 ↓
```
Retry
```text
 ↓
```
Dead Letter

El usuario podrá revisarlo.

---

## 24. Sync Status

Cada sincronización deberá tener estado.

ACTIVE
SYNCING
COMPLETED
FAILED
PAUSED

---

## 25. Sync History

El usuario podrá consultar:

18:30
Products
1.234 synchronized
3 failed


18:25
Stock
582 synchronized
0 failed

Esto genera trazabilidad.

---

## 26. Manual Sync

El usuario podrá forzar una sincronización.

[Sync now]

Esto será especialmente útil para soporte.

---

## 27. Pause

El usuario podrá detener temporalmente una sincronización.

Active
```text
   ↓
```
Pause

Sin eliminar la configuración.

---

## 28. Resume

Posteriormente:

Paused
```text
  ↓
```
Resume

El sistema continuará desde donde corresponda.

---

## 29. Scheduling

Podrán existir diferentes frecuencias:

Real-time
1 minute
5 minutes
15 minutes
30 minutes
1 hour
Daily
Manual

La disponibilidad dependerá del Connector y del plan.

---

## 30. Large Datasets

Esto es especialmente importante por Factusol + Access.

No debemos asumir que todas las bases de datos son pequeñas.

ERP Bridge deberá poder trabajar con:

10.000 registros
100.000 registros
1.000.000+ registros

cuando el sistema lo permita.

La arquitectura deberá favorecer:

procesamiento por lotes,
incremental sync,
paginación,
streaming cuando sea posible,
colas,
procesamiento asíncrono.

---

## 31. Factusol / Access

Para Factusol, el Agent tendrá una función especialmente importante.

Conceptualmente:

Factusol
```text
   ↓
```
Access
```text
   ↓
```
Agent
```text
   ↓
```
ERP Bridge

El Agent puede procesar la información localmente y enviar únicamente lo necesario.

Esto evita que el cliente tenga que subir su enorme .accdb a ERP Bridge.

---

## 32. No subir la base de datos

Uno de los principios del producto:

ERP Bridge no debe obligar al cliente a subir su base de datos completa a la nube para conectarse.

Especialmente importante para ERPs locales.

---

## 33. Sync Architecture

Conceptualmente:

                 ERP BRIDGE
                     │
                  Sync Core
```text
                     │
             ┌───────┴───────┐
             ↓               ↓
```
          Agent           Connectors
```text
             ↓               ↓
```
          Local ERP       Cloud Systems

---

## 34. Parallel Processing

Cuando sea seguro:

Products
```text
 ├── Batch 1
 ├── Batch 2
 ├── Batch 3
 └── Batch 4
```

Esto permite acelerar sincronizaciones grandes.

---

## 35. Rate Limits

Los sistemas externos pueden limitar las peticiones.

Ejemplo:

API:
100 requests/minute

ERP Bridge deberá respetar esos límites.

---

## 36. Backpressure

Si el destino no puede procesar datos suficientemente rápido:

Source
```text
 ↓
```
Queue
```text
 ↓
```
Destination

La cola evita que el sistema colapse.

---

## 37. Data Integrity

ERP Bridge debe priorizar la consistencia.

No queremos:

Pedido creado
Stock no actualizado
Cliente duplicado

sin que el sistema sea capaz de detectar y mostrar el estado.

---

## 38. Transactions

Cuando el sistema conectado lo permita, podremos utilizar transacciones.

Pero no debemos asumir que una operación distribuida entre dos sistemas puede ser una única transacción.

Por tanto, necesitaremos mecanismos de compensación cuando sea necesario.

---

## 39. Observability

Cada sincronización deberá poder rastrearse.

Sync #83921


Source:
Factusol


Destination:
WooCommerce


Entity:
Products


Started:
18:30


Completed:
18:32


Processed:
1.245


Failed:
3

---

## 40. Security

La sincronización deberá respetar:

permisos,
aislamiento por empresa,
cifrado,
credenciales,
auditoría.

---

## 41. AI

A largo plazo, la IA podrá ayudar a detectar problemas.

Ejemplo:

"El stock de 27 productos lleva 3 días sin actualizarse."

O:

"Estos 14 productos tienen conflictos entre el ERP y la tienda."

La IA podrá explicar el problema y sugerir una solución.

---

## 41.B Arquitectura de Sincronización de Alto Rendimiento (Hacerlo lo Mejor Posible)

Para garantizar que ERP Bridge opere de forma ultrarrápida y no sature hostings compartidos (SiteGround, Webempresa, Raiola) con límites estrictos de memoria PHP (256 MB) o timeout (30s):

### 1. Delta Sync en 3 Capas por Hashing (Local Agent SQLite)
El Local Agent almacena en una base de datos local SQLite el estado de cada producto dividido en 3 hashes criptográficos:
* `data_hash`: Hash determinista de nombre, descripción, categorías y atributos.
* `price_hash`: Hash de tarifa general y tarifas B2B.
* `stock_hash`: Hash de cantidad física y estado de disponibilidad.

```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ Si de 20.000 productos solo 35 cambian de stock o precio en el ERP:         │
│                                                                             │
│ • El Local Agent descarta los 19.965 productos intactos en local (< 100 ms).│
│ • Solo se envía 1 única petición HTTP a la tienda con los 35 modificados.   │
│ • Cero saturación de CPU, cero consumo inútil de ancho de banda y red.      │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 2. Estrategia Dual-Engine en WooCommerce
1. **Modo Universal (Zero-Touch por Defecto):**
   * Opera exclusivamente contra la REST API oficial (`/wp-json/wc/v3/`).
   * **Micro-lotes adaptativos:** 25 a 35 productos por petición (evita colapso de PHP-FPM).
   * **Concurrencia estricta = 1 worker** en hostings compartidos.
   * **Throttling Adaptativo (Leaky Bucket):** Si el TTFB del servidor supera los 1.500 ms, inserta pausas automáticas de 500-1.000 ms entre lotes.
   * **Exponential Backoff con Jitter:** Reintentos automáticos ante respuestas `429`, `503` o `504`.
2. **Modo Turbo (Companion Plugin Opcional):**
   * Si el cliente instala el micro-plugin gratuito `erp-bridge-worker.zip`, el conector conmuta a endpoints optimizados con bypass de hooks pesados (Yoast, WP Rocket).
   * Actualización directa sobre las tablas de búsqueda rápida de WooCommerce (`wp_wc_product_meta_lookup` y `wp_postmeta`) en MySQL: **500 stocks actualizados en 45 milisegundos**.

### 3. Blindaje de Imágenes (Deduplicación Estricta)
* Se calcula el hash MD5 de la imagen local en Factusol (`IMGART`).
* Si el hash no varía, **el array `images` se excluye del payload de actualización**. Esto impide que WordPress vuelva a descargar la imagen, la duplique en la biblioteca de medios y ejecute librerías GD/Imagick regenerando miniaturas innecesariamente.

### 4. Fiscalidad Española: Recargo de Equivalencia (R.E.)
* **Envíos ERP → Tienda:** Factusol almacena precios sin IVA. ERP Bridge calcula el PVP con IVA exacto y redondeo bancario a 2 decimales.
* **Descarga de Pedidos Tienda → Factusol:** Al detectar un cliente minorista con Recargo de Equivalencia (5,2%, 1,4%, 0,5%), ERP Bridge marca en Factusol el pedido con `REC_PCL = '1'`. Factusol calcula las bases imponibles oficiales de modo que **la factura contable en Factusol cuadre al céntimo exacto con el cobro de Redsys o Stripe**, evitando descuadres fiscales en el Modelo 303 de Hacienda.

### 5. Idempotencia y Prevención de Pedidos Duplicados
* Cada pedido se registra con la clave externa `REFPCL = 'WC-{order_id}'` (o `PS-{order_id}`).
* Antes de insertar, se verifica si el pedido ya existe en `F_PCL`.
* Inserción transaccional atómica: Cabecera (`F_PCL`) y Líneas (`F_LPC`) se insertan en una única transacción de base de datos con `ROLLBACK` automático ante cualquier anomalía.

## 42. MVP

La primera versión necesita:

Initial Sync
Incremental Sync
One-way Sync
Basic Two-way Sync
Mapping
External IDs
Retry
Sync Status
Sync History
Manual Sync
Pause / Resume
Basic Conflict Resolution

---

## 43. No construir inicialmente

Dejamos para fases posteriores:

sincronización distribuida extremadamente avanzada,
IA autónoma,
resolución automática compleja de conflictos,
streaming masivo,
data lake,
analytics avanzado.

---

## 44. Criterio de éxito

Una empresa debe poder conectar dos sistemas y mantener automáticamente sincronizadas las entidades que haya seleccionado, sin tener que desarrollar ni mantener scripts propios.

---

## 45. Regla fundamental

ERP Bridge no mueve datos simplemente de A a B. Mantiene relaciones, identidad, consistencia y trazabilidad entre sistemas.

---

## 46. Ejemplo completo

Este es uno de los casos que deberíamos conseguir:

                 FACTUSOL
                    │
              Product / Stock
                    │
                    ▼
                AGENT
                    │
                    ▼
               ERP BRIDGE
                    │
             Sync Engine
                    │
                    ▼
               WOOCOMMERCE

Y al revés:

WOOCOMMERCE
     │
New Order
```text
     ↓
```
ERP Bridge Event
```text
     ↓
```
Synchronization / Flow
```text
     ↓
```
Factusol
```text
     ↓
```
Pedido creado

El cliente simplemente verá:

🟢 Products synchronized
🟢 Stock synchronized
🟢 Orders synchronized

Y no tendrá que preocuparse de Access, APIs, polling, colas, mappings o reintentos.

---
