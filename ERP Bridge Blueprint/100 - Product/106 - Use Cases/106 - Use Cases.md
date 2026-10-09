# Use Cases — v1.0
> **Estado:** Borrador

---

# 106 - Use Cases

> *Casos de uso principales y flujos de trabajo.*

---

## 1. Propósito

Este documento define los principales casos de uso de ERP Bridge.

Los casos de uso describen situaciones reales en las que una empresa utilizará la plataforma y servirán como referencia para definir las funcionalidades necesarias del producto.

Los casos de uso no definen todavía cómo se implementará técnicamente cada funcionalidad.

---

## 2. Caso de Uso Principal — Conectar un ERP con un Ecommerce

### Situación
Una empresa utiliza un ERP para gestionar productos, clientes, pedidos y stock. También dispone de una tienda online. Actualmente ambos sistemas funcionan de forma independiente.

### Objetivo
Conseguir que ambos sistemas intercambien información automáticamente.

### Flujo

```text
ERP
 │
 │ Productos / Stock
 ↓
ERP Bridge
 │
 ↓
Ecommerce
```

Y en sentido contrario:

```text
Ecommerce
 │
 │ Pedidos / Clientes
 ↓
ERP Bridge
 │
 ↓
ERP
```

### Resultado Esperado
La empresa deja de introducir manualmente pedidos, productos o stock entre ambos sistemas.

---

## 3. Caso de Uso — Conectar Factusol

### Situación
Una empresa utiliza Factusol instalado localmente y dispone de una base de datos Access. La empresa quiere conectar Factusol con un sistema externo.

### Problema
El sistema se encuentra dentro de la infraestructura local de la empresa y no necesariamente dispone de una API moderna para realizar todas las operaciones necesarias.

### Objetivo
Permitir que ERP Bridge pueda comunicarse con Factusol sin obligar a:
- Cambiar de ERP.
- Migrar la información.
- Subir la base de datos completa a Internet.

### Resultado Esperado
ERP Bridge puede acceder a la información necesaria desde el entorno local y transmitir únicamente la información necesaria hacia los sistemas conectados. *(Caso de uso fundamental del MVP).*

---

## 4. Caso de Uso — Sincronizar Productos

### Situación
Una empresa mantiene sus productos en el ERP y los vende a través de un ecommerce.

### Objetivo
Mantener la información de productos sincronizada.

```text
ERP
 │
 ├── SKU
 ├── Nombre
 ├── Precio
 ├── Descripción
 └── Stock
       ↓
  ERP Bridge
       ↓
  Ecommerce
```

### Resultado
Cuando cambia la información relevante del producto, ERP Bridge actualiza el ecommerce automáticamente.

---

## 5. Caso de Uso — Sincronizar Stock

### Situación
Una empresa vende productos tanto físicamente como online. El stock se gestiona principalmente desde el ERP.

### Objetivo
Evitar que el ecommerce muestre información de stock incorrecta.

```text
Venta física
     ↓
ERP
     ↓
ERP Bridge
     ↓
Ecommerce
     ↓
Nuevo stock
```

*(También podrá existir el flujo contrario cuando sea necesario).*

---

## 6. Caso de Uso — Sincronizar Pedidos

### Situación
Un cliente realiza un pedido en el ecommerce.

```text
Cliente
   ↓
Ecommerce
   ↓
ERP Bridge
   ↓
ERP
   ↓
Pedido registrado
```

### Resultado
El pedido aparece automáticamente en el sistema de gestión de la empresa sin intervención manual.

---

## 7. Caso de Uso — Sincronizar Clientes

### Situación
Un nuevo cliente realiza una compra online.

```text
Ecommerce
    ↓
ERP Bridge
    ↓
ERP
    ↓
Cliente creado/actualizado
```

ERP Bridge deberá evitar crear duplicados cuando el cliente ya exista.

---

## 8. Caso de Uso — Transformar Información

### Situación
Dos sistemas utilizan estructuras de datos diferentes.

```text
Sistema A
customer_name
customer_email
        ↓
    ERP Bridge
        ↓
Sistema B
cliente.nombre
cliente.email
```

### Objetivo
Permitir adaptar los datos de un sistema al modelo requerido por otro.

---

## 9. Caso de Uso — Automatizar un Proceso

ERP Bridge no debe limitarse a sincronizar datos. Debe poder ejecutar procesos completos:

```text
Nuevo pedido
     ↓
Crear pedido en ERP
     ↓
Actualizar stock
     ↓
Generar factura
     ↓
Enviar confirmación
```

La empresa configura el proceso y ERP Bridge lo ejecuta automáticamente.

---

## 10. Caso de Uso — Gestionar Errores

### Situación
ERP Bridge intenta enviar un pedido al ERP, pero el sistema está temporalmente desconectado.

### Comportamiento Esperado

```text
Intento → Error → Registrar error → Reintentar → Éxito
```

Si continúa fallando:

```text
Error → Reintentos → Error persistente → Notificación → Intervención humana
```

> **Regla:** La automatización no debe ocultar los errores.

---

## 11. Caso de Uso — Monitorizar una Integración

El usuario debe poder saber:
- Si una conexión está activa.
- Cuándo se ejecutó por última vez.
- Cuántas operaciones se realizaron.
- Cuántas fallaron.
- Qué errores existen.
- Qué operaciones están pendientes.

### Ejemplo Conceptual
```text
Factusol ─────── 🟢 Conectado
WooCommerce ──── 🟢 Conectado
Pedidos ──────── 🟢 1.248 sincronizados
Errores ──────── 🟡 2 pendientes
```

---

## 12. Caso de Uso — Instalar un Nuevo Conector

### Situación
Una empresa ya utiliza Factusol y WooCommerce, y decide añadir un nuevo canal de venta (por ejemplo PrestaShop o Web a medida mediante Micro-Puente HTTPS) o incorporar un nuevo sistema.

### Objetivo
El usuario o técnico puede activar y configurar el nuevo conector desde el Centro de Control Local sin tener que reprogramar la lógica de sincronización ni alterar el flujo de trabajo existente.

---

## 13. Caso de Uso — Conectar Sistemas Locales y Cloud

### Situación
El ERP está instalado localmente y el ecommerce está en Internet.

### Arquitectura Saliente y Zero-Knowledge
Cuando un sistema empresarial funciona en red local sin API accesible para la operación requerida (como Factusol sobre base de datos Access local), el Agente Local actúa como puente saliente seguro (*Outbound-only*).

La sincronización de catálogo, stock y pedidos se realiza de forma **directa y punto a punto** entre el Agente Local y el canal de venta (WooCommerce, PrestaShop o Micro-Puente HTTPS), sin que los datos de negocio (productos, precios, clientes, pedidos) pasen ni se almacenen jamás en la nube de Bentian.

```text
┌──────────────── EMPRESA (LOCAL) ───────────────┐
│                                                │
│  ERP (Factusol)                                │
│   │                                            │
│   ↓ (Lectura/Escritura Local OLEDB)            │
│ Agente Bentian (Edge Processing)               │
│                                                │
└───────────────┬────────────────────────────────┘
                │
                │ Conexión Directa HTTPS / TLS 1.3
                │ (Outbound-only, Cero Puertos Abiertos)
                │ [Zero-Knowledge: Cero Datos en Nube Bentian]
                ↓
           Canal Ecommerce
     (WooCommerce / PrestaShop / Web)
                
   [Telemetría y Licencia única]
   Agente ──(Heartbeat HTTPS)──> Bentian Cloud (Licencias / Stripe SSO)
```

### Objetivo
Permitir integrar sistemas locales con servicios externos sin exponer directamente el ERP a Internet y garantizando privacidad absoluta (Zero-Knowledge: los datos de negocio nunca tocan la nube de Bentian).

---

## 14. Caso de Uso — Utilizar IA *(Visión Futura)*

*(Este caso pertenece a la visión futura y no al núcleo del MVP).*

```text
Cliente → Agente IA → ERP Bridge → ERP / CRM / Ecommerce
```

### Ejemplos Conceptuales:
- *"¿Cuántos pedidos tenemos pendientes de enviar?"* -> La IA consulta mediante ERP Bridge y responde.
- *"Avísame cuando el stock del producto X sea inferior a 10 unidades."* -> La IA crea la automatización en ERP Bridge.

---

## 15. Caso de Uso — Crear un Nuevo Conector

### Situación
ERP Bridge ya tiene conectores para Factusol y WooCommerce. Queremos añadir Shopify.

### Objetivo
Un desarrollador debe poder crear `Shopify Connector` utilizando las capacidades existentes del Core, sin reconstruir autenticación, errores, logs, eventos, sincronización ni monitorización.

> **Resultado Esperado:** El coste y tiempo de crear un nuevo conector debe disminuir significativamente a medida que madura la plataforma.

---

## 16. Caso de Uso — Empresa sin Conocimientos Técnicos

### Experiencia Ideal para el Usuario
```text
¿Qué quieres conectar? ───> [ Factusol ]
                                 ↓
¿Con qué? ───────────────> [ WooCommerce ]
                                 ↓
¿Qué quieres sincronizar? ─> ☑ Productos  ☑ Stock  ☑ Clientes  ☑ Pedidos
                                 ↓
                           [ Conectar ]
                                 ↓
                           [ Activar ]
```

El usuario no necesita conocer APIs, bases de datos, webhooks, Docker ni programación.

---

## 17. Casos de Uso Fuera del MVP

Forman parte de la visión, pero no forman parte de la primera versión:
- ERP → CRM / Ecommerce → CRM
- ERP → Logística / ERP → Plataformas de pago
- IA → ERP / IA → CRM
- Automatizaciones complejas
- Marketplace de conectores
- Conectores desarrollados por terceros
- Integraciones entre múltiples empresas/sistemas

---

## 18. Priorización

| Caso de Uso | MVP | Futuro |
|---|:---:|:---:|
| **ERP ↔ Ecommerce** | ⭐⭐⭐⭐⭐ | |
| **Factusol** | ⭐⭐⭐⭐⭐ | |
| **Productos** | ⭐⭐⭐⭐⭐ | |
| **Stock** | ⭐⭐⭐⭐⭐ | |
| **Pedidos** | ⭐⭐⭐⭐⭐ | |
| **Clientes** | ⭐⭐⭐⭐ | |
| **Mapeo de datos** | ⭐⭐⭐⭐⭐ | |
| **Automatizaciones** | ⭐⭐⭐⭐ | |
| **Gestión de errores** | ⭐⭐⭐⭐⭐ | |
| **Monitorización** | ⭐⭐⭐⭐⭐ | |
| **Sistemas locales + cloud** | ⭐⭐⭐⭐⭐ | |
| **Nuevos conectores** | | ⭐⭐⭐⭐⭐ |
| **CRM** | | ⭐⭐⭐⭐ |
| **Logística** | | ⭐⭐⭐ |
| **IA** | | ⭐⭐⭐ |
| **Marketplace** | | ⭐⭐ |
| **Conectores de terceros** | | ⭐⭐ |

*(Nota: Durante el MVP solo se implementan Factusol y WooCommerce. Nuevos conectores como Sage o Shopify se priorizan para v1 tras la validación comercial).*
�n

| Caso de Uso | MVP | Futuro |
|---|:---:|:---:|
| **ERP ↔ Ecommerce** | ⭐⭐⭐⭐⭐ | |
| **Factusol** | ⭐⭐⭐⭐⭐ | |
| **Productos** | ⭐⭐⭐⭐⭐ | |
| **Stock** | ⭐⭐⭐⭐⭐ | |
| **Pedidos** | ⭐⭐⭐⭐⭐ | |
| **Clientes** | ⭐⭐⭐⭐ | |
| **Mapeo de datos** | ⭐⭐⭐⭐⭐ | |
| **Automatizaciones** | ⭐⭐⭐⭐ | |
| **Gestión de errores** | ⭐⭐⭐⭐⭐ | |
| **Monitorización** | ⭐⭐⭐⭐⭐ | |
| **Sistemas locales + cloud** | ⭐⭐⭐⭐⭐ | |
| **Nuevos conectores** | | ⭐⭐⭐⭐⭐ |
| **CRM** | | ⭐⭐⭐⭐ |
| **Logística** | | ⭐⭐⭐ |
| **IA** | | ⭐⭐⭐ |
| **Marketplace** | | ⭐⭐ |
| **Conectores de terceros** | | ⭐⭐ |

---

## 19. Definición de Éxito y Revelaciones de Arquitectura

> **ERP Bridge habrá demostrado su concepto cuando una empresa pueda conectar un ERP local con un ecommerce, configurar qué información desea intercambiar y mantener ambos sistemas sincronizados de forma automática, fiable y sin programación.**

### Métrica Clave de Plataforma:
> **El segundo conector debe poder construirse reutilizando el núcleo desarrollado para el primero.**
*(Si para el segundo conector tenemos que volver a construir medio sistema, habremos hecho un conector, no una plataforma).*

---

### 🧩 Revelación de Arquitectura Emergente

Los casos de uso comienzan a insinuar los componentes esenciales de la arquitectura:

```text
Conector ──> Conexión ──> Entidad ──> Evento ──> Transformación ──> Acción ──> Flujo ──> Resultado
```
