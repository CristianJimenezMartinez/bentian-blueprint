# Events — v1.0
> **Estado:** Borrador

---

# 309 - Events

> *Arquitectura de eventos, bus asíncrono, ingestión, ciclo de vida y Dead Letter Queue.*

---

## 1. Propósito

El sistema de Events permite detectar, transportar, procesar y distribuir acontecimientos que ocurren dentro de ERP Bridge o en los sistemas conectados.

La idea fundamental es:

ALGO OCURRE
```text
     ↓
```
EVENT
```text
     ↓
```
ERP BRIDGE
```text
     ↓
```
ACCIÓN

---

## 2. Ejemplo real

Una tienda recibe un pedido.

WooCommerce
```text
     │
     │ Order Created
     ↓
```
ERP Bridge
```text
     │
     ├──→ Crear pedido en Factusol
     ├──→ Actualizar stock
     └──→ Notificar CRM
```

El usuario no necesita programar todo esto.

Lo configura como un flujo.

---

## 3. Qué es un Event

Un Event representa que algo ha sucedido.

Ejemplos:

ORDER_CREATED
PRODUCT_UPDATED
CUSTOMER_CREATED
STOCK_CHANGED
INVOICE_CREATED
CONNECTION_LOST
SYNC_FAILED

Un evento no dice necesariamente qué hacer.

Simplemente informa:

"Esto ha ocurrido."

---

## 4. Event ≠ Action

Esta distinción es fundamental.

Event
ORDER_CREATED

Significa:

Se ha creado un pedido.

Action
CREATE_ORDER

Significa:

Crea un pedido.

Entonces:

EVENT
ORDER_CREATED
```text
      ↓
```
FLOW
```text
      ↓
```
ACTION
CREATE_ORDER

---

## 5. Fuentes de eventos

Los eventos pueden proceder de diferentes lugares.

Sistemas externos
Shopify
WooCommerce
CRM
ERP
Agent
ERP local
```text
 ↓
```
Agent
```text
 ↓
```
Event
Core
ERP Bridge
```text
 ↓
```
Internal Event

---

## 6. Eventos externos

Ejemplo:

Shopify
```text
   ↓
```
Webhook
```text
   ↓
```
ERP Bridge
```text
   ↓
```
SHOPIFY_ORDER_CREATED

El Connector transforma el evento externo al modelo común.

---

## 7. Normalización

Cada plataforma puede llamar a lo mismo de forma diferente.

Shopify:

orders/create

WooCommerce:

order.created

ERP Bridge:

ORDER_CREATED

Esto es muy importante.

El Core trabaja con eventos comunes, no con cientos de nombres diferentes.

---

## 8. Event Payload

Cada evento tendrá información asociada.

Conceptualmente:

Event
```text
├── id
├── type
├── timestamp
├── source
├── connection
└── data
```

Ejemplo:

type:
ORDER_CREATED


source:
woocommerce


connection:
conn_123


data:
{
  orderId: "1234",
  customerId: "5678"
}

La estructura definitiva se definirá durante la implementación.

---

## 9. Event ID

Cada evento deberá tener un identificador único.

Esto permitirá:

rastrearlo,
evitar duplicados,
relacionarlo con ejecuciones,
auditarlo.

---

## 10. Event Timestamp

Todos los eventos deberán registrar cuándo ocurrieron.

timestamp:
2026-08-18T22:30:00Z

Esto será importante para sincronización y debugging.

---

## 11. Event Source

El sistema deberá conocer de dónde procede.

Ejemplo:

source:
factusol


connection:
conn_abc123

---

## 12. Event Types

Inicialmente podemos definir categorías.

Business Events
ORDER_CREATED
ORDER_UPDATED
CUSTOMER_CREATED
PRODUCT_UPDATED
STOCK_CHANGED
INVOICE_CREATED
Connection Events
CONNECTION_CREATED
CONNECTION_LOST
CONNECTION_RESTORED
System Events
SYNC_STARTED
SYNC_COMPLETED
SYNC_FAILED

---

## 13. Event Bus

El Core deberá disponer de un mecanismo para distribuir eventos internamente.

Conceptualmente:

                EVENT BUS
```text
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
```
      Flow A     Flow B     Flow C

Un mismo evento puede activar varios flujos.

---

## 14. Ejemplo

Evento:

ORDER_CREATED

Puede activar:

             ORDER_CREATED
```text
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
```
 Create ERP     Update CRM   Send Email

Esto es lo que convierte a ERP Bridge en una plataforma de automatización.

---

## 15. Event Filtering

No todos los eventos deben activar todos los workflows.

Podremos aplicar filtros.

Ejemplo:

ORDER_CREATED


IF:
total > 500€


THEN:
Notify manager

Otro:

ORDER_CREATED


IF:
country = Spain


THEN:
Create ERP Order

---

## 16. Event Routing

El Core deberá determinar qué flujo recibe cada evento.

Event
```text
 ↓
```
Router
```text
 ↓
```
Matching Flows
```text
 ↓
```
Executions

---

## 17. Event Deduplication

Este punto es crítico.

Un webhook puede llegar dos veces.

ORDER_CREATED
```text
       ↓
```
Webhook #1
```text
       ↓
```
Webhook #2

No queremos:

Crear pedido
Crear pedido

Queremos:

Evento procesado ✓
```text
Evento duplicado → ignorar
```

---

## 18. Idempotency

Los eventos deberán poder procesarse de forma segura incluso si se reciben más de una vez.

Esto estará relacionado con el sistema de idempotencia definido anteriormente.

---

## 19. Event Queue

Cuando haya mucho tráfico:

Events
```text
  ↓
```
Queue
```text
  ↓
```
Workers
```text
  ↓
```
Processing

Esto permite absorber picos.

Ejemplo:

Una tienda recibe 500 pedidos en pocos minutos.

ERP Bridge no debería colapsar intentando procesarlos todos simultáneamente.

---

## 20. Retry

Si una acción falla:

Event
```text
 ↓
```
Action
```text
 ↓
```
ERROR
```text
 ↓
```
Retry

Podremos utilizar estrategias como:

1º intento
```text
↓
```
30 segundos
```text
↓
```
2º intento
```text
↓
```
2 minutos
```text
↓
```
3º intento
```text
↓
```
...

Los valores exactos se definirán técnicamente.

---

## 21. Dead Letter Queue

Si un evento sigue fallando:

Event
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
Retry
```text
 ↓
```
FAILED
```text
 ↓
```
Dead Letter Queue

Esto permite que el resto del sistema continúe funcionando.

El usuario podrá revisar posteriormente el problema.

---

## 22. Event Status

Podremos controlar estados:

RECEIVED
PROCESSING
COMPLETED
FAILED
RETRYING
DEAD_LETTER

---

## 23. Event Traceability

El usuario deberá poder seguir un evento completo.

Ejemplo:

Order #1234


WooCommerce
```text
   ↓
```
ORDER_CREATED
```text
   ↓
```
Flow #17
```text
   ↓
```
Factusol
```text
   ↓
```
CREATE_ORDER
```text
   ↓
```
SUCCESS

Esto será tremendamente útil para soporte.

---

## 24. Event → Flow → Action

Esta será una de las abstracciones principales de ERP Bridge:

EVENT
```text
  ↓
```
FLOW
```text
  ↓
```
ACTION

Por ejemplo:

STOCK_CHANGED
```text
      ↓
```
Update stock
```text
      ↓
```
WooCommerce

---

## 25. Múltiples acciones

Un flujo puede ejecutar varias acciones.

ORDER_CREATED
```text
      ↓
```
Create ERP Order
```text
      ↓
```
Update Stock
```text
      ↓
```
Create CRM Customer
```text
      ↓
```
Send Notification

---

## 26. Orden de ejecución

Algunas acciones podrán ejecutarse secuencialmente.

A
```text
 ↓
```
B
```text
 ↓
```
C

O en paralelo cuando sea seguro:

      A
```text
      │
      ↓
     ┌┴┐
     ↓ ↓
```
     B C

El motor deberá determinar cuándo es posible.

---

## 27. Event Replay

A largo plazo podremos permitir volver a procesar eventos.

Ejemplo:

Evento #12345


[Replay]

Esto puede ser extremadamente útil después de corregir un error de configuración.

---

## 28. Event History

Dependiendo del plan y de las necesidades de almacenamiento, podremos mantener historial de eventos.

Esto permitirá:

debugging,
auditoría,
estadísticas,
replay.

---

## 29. Seguridad

Los eventos pueden contener información empresarial.

Por tanto:

deberán respetar permisos,
no deberán exponer secretos,
deberán aislarse por organización,
deberán registrarse de forma segura.

---

## 30. IA y Events

Aquí aparece otra posibilidad muy potente.

Un evento podría alimentar un agente de IA.

Ejemplo:

CUSTOMER_CREATED
```text
       ↓
```
AI Agent
```text
       ↓
```
Analizar cliente
```text
       ↓
```
Generar segmento
```text
       ↓
```
Actualizar CRM

O:

ORDER_CREATED
```text
       ↓
```
AI
```text
       ↓
```
Detectar anomalía
```text
       ↓
```
Alertar

Esto pertenece a la visión futura del producto, no al MVP.

---

## 31. Events internos

ERP Bridge también podrá generar eventos propios.

Ejemplo:

SYNC_FAILED

Entonces:

SYNC_FAILED
```text
     ↓
```
Notify Administrator

Así el propio sistema puede automatizar su operación.

---

## 32. MVP

Inicialmente necesitamos:

Event Model
Event Bus
Basic Event Types
Event Routing
Deduplication
Retry
Event Logging
Basic Webhooks

No necesitamos inicialmente:

Event Replay avanzado.
Streaming distribuido complejo.
IA.
Event Analytics avanzado.

---

## 33. Criterio de éxito

Cualquier evento relevante producido por un sistema conectado debe poder convertirse en el disparador de una automatización sin que el usuario tenga que programarla.

---

## 34. Regla fundamental

Los eventos describen lo que ha ocurrido; los workflows determinan qué debe ocurrir después.

Esta separación es esencial.

---

## 35. Ejemplo completo de ERP Bridge

Aquí empieza a verse el producto:

             WOOCommerce
                  │
             New Order
                  │
                  ▼
              Connector
                  │
                  ▼
                EVENT
            ORDER_CREATED
                  │
                  ▼
              Event Bus
                  │
                  ▼
                FLOW
```text
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
```
   Factusol      CRM       Email
       │
   Create Order
       │
       ▼
      ERP

Eso es mucho más potente que un simple "conector de Factusol".

---
