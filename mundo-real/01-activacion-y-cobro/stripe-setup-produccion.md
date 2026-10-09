# Guía de Configuración de Stripe en Producción
**Proyecto:** Bentian ERP Bridge  
**Titular:** Cristian Jiménez Martínez (Autónomo en régimen de pluriactividad)  
**Dominio:** `bridge.cristianjm.com`  
**Moneda base:** EUR (€)  
**Fecha de actualización:** Septiembre 2026  

---

## 1. Configuración de la Cuenta de Stripe

### 1.1 Registro y Datos Fiscales
1. Crear cuenta en [Stripe.com](https://stripe.com/es) con el correo profesional (`contacto@cristianjm.com` o el email personal de gestión).
2. **Tipo de entidad:** Seleccionar **Autónomo / Empresario Individual** (*Individual / Sole Proprietorship*).
3. **Datos del titular:**
   * **Nombre legal completo:** Cristian Jiménez Martínez.
   * **NIF / NIE:** NIF personal del titular.
   * **Fecha de nacimiento y domicilio fiscal:** Dirección declarada en el Modelo 036 de la AEAT.
4. **Datos de la empresa / Producto:**
   * **Nombre comercial:** `Bentian ERP Bridge`.
   * **Descripción del negocio:** *«Desarrollo y comercialización de software de integración y sincronización de datos entre sistemas de gestión empresarial (ERP) y plataformas de comercio electrónico»*.
   * **Sitio web:** `https://bridge.cristianjm.com`.
   * **Descriptor en el extracto bancario (Statement Descriptor):** `BENTIAN SOFTWARE` (máximo 22 caracteres, claro para que el cliente no solicite retrocesos).
   * **Descriptor corto:** `BENTIAN`.
   * **Teléfono de soporte:** Teléfono móvil del titular.
5. **Cuenta bancaria de cobro (Payouts):**
   * Introducir el IBAN español (`ESxx`) de la cuenta de autónomo (BBVA Cuenta Online Autónomos o N26 Business).
   * **Frecuencia de transferencias (Payout Schedule):** Configurar en **Automático diario** o **Semanal (los viernes)**.

---

## 2. Creación del Catálogo de Productos y Precios

En el panel de Stripe, acceder a **Catálogo de productos** (`/products`) y crear los dos planes oficiales:

```text
                                CATÁLOGO DE PRODUCTOS
 ┌───────────────────────────────────────┬───────────────────────────────────────┐
 │ PRODUCTO 1: BENTIAN PLAN ANUAL       │ PRODUCTO 2: BENTIAN PLAN MENSUAL      │
 ├───────────────────────────────────────┼───────────────────────────────────────┤
 │ • Nombre: Bentian ERP Bridge - Anual  │ • Nombre: Bentian ERP Bridge - Mensual│
 │ • ID Producto: `prod_bentian_anual`   │ • ID Producto: `prod_bentian_mensual` │
 │ • Precio: 249,00 €                    │ • Precio: 29,00 €                     │
 │ • Intervalo: Cada 1 año (Yearly)      │ • Intervalo: Cada 1 mes (Monthly)     │
 │ • IVA: No incluido (se suma en checkout)│ • IVA: No incluido (se suma en checkout)│
 └───────────────────────────────────────┴───────────────────────────────────────┘
```

### Configuración del Plan Anual (Recomendado / Foco de Venta):
* **Nombre:** `Bentian ERP Bridge — Suscripción Anual`
* **Descripción:** *«Licencia de uso anual para 1 instalación de Bentian Local Agent. Sincronización bidireccional continua Factusol ↔ WooCommerce/PrestaShop, catálogo ilimitado, pedidos ilimitados y soporte técnico prioritario.»*
* **Modelo de precios:** Tarifa plana (*Flat fee*).
* **Importe:** `249.00 EUR`.
* **Facturación recurrente:** Intervalo = `Año`.
* **Comportamiento fiscal:** No incluye impuestos (*Tax exclusive*). El 21% de IVA se calculará dinámicamente según la ubicación del comprador.

### Configuración del Plan Mensual:
* **Nombre:** `Bentian ERP Bridge — Suscripción Mensual`
* **Importe:** `29.00 EUR`.
* **Facturación recurrente:** Intervalo = `Mes`.
* **Comportamiento fiscal:** No incluye impuestos (*Tax exclusive*).

---

## 3. Configuración de Impuestos Automáticos (Stripe Tax)

Para cumplir estrictamente con la normativa fiscal española y comunitaria sin calcular impuestos a mano:

1. Activar **Stripe Tax** en el panel de configuración (`/settings/tax`).
2. **Dirección de origen fiscal:** Establecer la dirección de actividad económica en España (Comunidad Valenciana o Albacete).
3. **Categoría fiscal predeterminada (Tax Code):**
   * Seleccionar: **Software como servicio (SaaS) — Software as a Service - business** (Código de Stripe: `txcd_10202000`).
4. **Reglas fiscales aplicadas automáticamente por Stripe:**
   * **Clientes en España (Península y Baleares):** Aplica automáticamente el **21% de IVA**.
   * **Clientes en Canarias / Ceuta / Melilla:** Aplica exención de IVA por territorio no sujeto (IGIC / IPSI).
   * **Empresas de la Unión Europea (B2B intracomunitario):** Si introducen un *VAT Number* válido registrado en VIES, Stripe aplica automáticamente **0% de IVA (Inversión del Sujeto Pasivo / Reverse Charge)**.
   * **Clientes fuera de la UE (EE.UU., Reino Unido, Latinoamérica):** Aplica **0% de IVA (Exportación de servicios no sujeta a IVA español)**.

---

## 4. Activación del Portal de Clientes (Stripe Customer Portal)

El portal permite que las agencias y clientes gestionen sus datos de forma 100% autónoma, ahorrándote horas de atención al cliente:

1. Ir a **Configuración > Portal de clientes** (`/settings/billing/portal`).
2. Habilitar las siguientes funciones para los clientes:
   * [x] **Actualizar métodos de pago:** Permitir cambiar o renovar tarjetas caducadas.
   * [x] **Actualizar datos de facturación:** Modificar NIF, razón social o dirección fiscal.
   * [x] **Ver y descargar historial de facturas en PDF con validez fiscal.**
   * [x] **Cancelar suscripciones:** Con efecto al término del período de facturación pagado.
3. **Personalización de marca:**
   * Subir logotipo de Bentian (cuadrado, fondo transparente, 512x512 px).
   * Color de marca: Azul tecnológico (`#0F172A` o `#2563EB`).
   * Enlace de retorno: `https://bridge.cristianjm.com`.

---

## 5. Integración con la Landing Page (Enlaces de Pago - Payment Links)

Sin necesidad de programar un backend complejo en el día 1, Stripe permite generar **Payment Links directos**:

1. En el menú **Enlaces de pago** (`/payment_links`), crear dos enlaces:
   * **Enlace 1 (Plan Anual):** `https://buy.stripe.com/live_bentian_anual_249`
   * **Enlace 2 (Plan Mensual):** `https://buy.stripe.com/live_bentian_mensual_29`
2. **Opciones obligatorias a marcar en el enlace de pago:**
   * [x] **Recopilar dirección fiscal del cliente:** Obligatorio para la factura.
   * [x] **Recopilar NIF / NIE / CIF del cliente (Tax ID):** Añadir el campo de ID fiscal como obligatorio.
   * [x] **Permitir códigos promocionales:** Permite crear cupones de descuento para partners (ej. cupón del 25% para que la agencia compre la licencia a 186,75 € directamente).
   * [x] **URL de redirección tras el pago:** Redirigir a:
     `https://bridge.cristianjm.com/descargas/gracias?session_id={CHECKOUT_SESSION_ID}`
     (donde se descarga automáticamente el instalador `BentianAgentSetup.exe` y se muestra el token de licencia).

---

## 6. Automatización de Facturas Legales (Stripe Invoicing)

1. En **Configuración > Facturación** (`/settings/invoicing`):
   * **Serie y numeración:** Fijar prefijo `BEN-2026-` con numeración correlativa automática (ej. `BEN-2026-0001`).
   * **Pie de factura legal obligatorio:**
     > *«Cristian Jiménez Martínez — NIF: [Tu NIF] — Domicilio: [Tu Dirección Fiscal]. Operación sujeta a la Ley 37/1992 del IVA. Régimen General.»*
   * **Envío automático de factura:** Marcar para que Stripe envíe por correo electrónico el PDF de la factura legal en el mismo segundo en que se confirma el cobro.
