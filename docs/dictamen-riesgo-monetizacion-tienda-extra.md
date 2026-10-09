# DICTAMEN DE RIESGO DE NEGOCIO, MONETIZACIÓN Y ANTI-FRAUDE
## Auditoría del Add-on 'Tienda Extra' (99€) en Bentian ERP Bridge

**Fecha:** 29 de Septiembre de 2026  
**Área:** Estrategia de Negocio, Monetización y Prevención de Fraude  
**Estado:** Dictamen Ejecutivo Oficial  
**Destinatario:** Dirección General y Arquitectura de Bentian ERP Bridge  

---

## 1. RESUMEN EJECUTIVO

El presente dictamen evalúa el impacto financiero, operativo, legal y comercial de comercializar el **"Add-on Tienda Extra" (99 €/año o 12 €/mes)** en la pasarela pública de Stripe de Bentian ERP Bridge.

La conclusión pericial es concluyente: **el Add-on de "Tienda Extra" en venta abierta y desasistida constituye una vulnerabilidad estructural de monetización.** Introduce una vía directa para el arbitraje comercial ("subarrendamiento pirata"), canibaliza entre el **40% y el 48% de la facturación legítima**, destruye los incentivos del Programa Oficial de Partners y expone a Bentian a pasivos legales y sobrecargas de soporte técnico no remunerado ("clientes fantasma").

Se recomienda su **retirada inmediata del catálogo público de Stripe** y la implantación de la **Regla de Oro: "1 Tienda Conectada = 1 Licencia Base"**, con salvaguardas comerciales controladas para empresas legítimas con el mismo CIF.

---

## 2. ESCENARIOS DE ABUSO, ARBITRAJE Y CANIBALIZACIÓN FINANCIERA

### 2.1 El Mecanismo del Arbitraje ("Host Agency" o Revendedor Parásito)
En el sector del software B2B en España (agencias de marketing digital, instaladores de software DELSOL y micropymes informáticas), existe una práctica recurrente de optimización agresiva de costes:
1. Una agencia o técnico freelance adquiere **1 Licencia Base** (199 €/año promo o 249 €/año estándar) a su propio nombre o razón social.
2. Cada vez que capta un cliente con Factusol y WooCommerce/PrestaShop, no le hace comprar una licencia independiente, sino que adquiere un **Add-on de Tienda Extra por 99 €/año** (o 12 €/mes).
3. Conecta el software al equipo o servidor de sus clientes (o mapea múltiples bases de datos/tiendas bajo una única cuenta de Bentian).
4. La agencia factura al cliente final su propia cuota de mantenimiento mensual o anual (ej. entre 350 € y 800 €/año por "módulo de sincronización Factusol").

### 2.2 Impacto Económico Directo: Fuga de Ingresos y Canibalización
Comparemos los ingresos de Bentian en una muestra representativa de 5 clientes (1 cliente original + 4 clientes captados):

| Métrica | Venta Legítima (5 Licencias Base) | Abuso con Tienda Extra (1 Base + 4 Add-ons) | Fuga Neta / Pérdida | % Canibalización |
|---|---|---|---|---|
| **Precio Promo (199 € / 99 €)** | **995 € / año** (5 × 199 €) | **595 € / año** (199 € + 4×99 €) | **-400 € / año** | **-40,2%** |
| **Precio Oficial (249 € / 99 €)** | **1.245 € / año** (5 × 249 €) | **645 € / año** (249 € + 4×99 €) | **-600 € / año** | **-48,2%** |
| **Escala 25 Tiendas** | 4.975 € – 6.225 € / año | 2.575 € – 2.625 € / año | **-2.400 € a -3.600 € / año** | **~48%** |
| **Escala 100 Tiendas** | 19.900 € – 24.900 € / año | 10.000 € – 10.099 € / año | **-9.900 € a -14.800 € / año** | **~50%** |

> **Conclusión Financiera:** Por cada 5 tiendas conectadas vía arbitraje de add-on, Bentian regala casi la mitad de su facturación recurrente. A medida que el parque instalado crece, la empresa asume el coste de soporte y desarrollo de 100 clientes cobrando solo el equivalente a 50.

### 2.3 Incentivo Perverso frente al Programa de Partners Oficial
El Blueprint de Bentian establece un **Programa de Partners B2B con un 25% de comisión recurrente** para premiar a agencias y distribuidores DELSOL:

* **Vía Oficial (Partner Honesto):**
  - El Partner vende a su cliente 1 Licencia Base (199 €).
  - Bentian ingresa 199 €.
  - Bentian abona al Partner su comisión del 25% = **49,75 € / año**.
  - **Margen retenido por Bentian:** **149,25 € / año**.
  - El cliente final figura en el censo oficial, con contrato, CIF y factura a su nombre.

* **Vía Tramposa (Arbitraje de Tienda Extra a 99 €):**
  - El Partner no da de alta al cliente en Bentian. Compra un Add-on por 99 €.
  - El Partner le cobra a su cliente 199 € (o 300 €) en su factura mensual.
  - **Margen que se embolsa el Partner:** **100 € a 201 € / año** (frente a 49,75 € oficiales).
  - **Ingreso para Bentian:** apenas **99 €**.

> **Efecto Tóxico:** El Add-on público crea un incentivo perverso donde hacer trampa es **más del doble de rentable para el revendedor** que seguir el canal oficial de afiliación ($100\text{ € vs }49,75\text{ €}$). Esto pudre el canal de partners antes de consolidarse.

### 2.4 Riesgo Legal, Contractual y Soporte Fantasma ("Shadow Customers")
1. **Inexistencia de Relación Jurídica (Incertidumbre Contractual):**
   - El cliente final que sufre la avería no ha firmado los Términos de Servicio (ToS) ni el Acuerdo de Nivel de Servicio (SLA) de Bentian. Para Bentian, esa empresa "no existe"; solo existe la agencia intermediaria.
2. **Cumplimiento RGPD y Encargo de Tratamiento:**
   - La sincronización procesa pedidos con datos de carácter personal (nombres de compradores online, direcciones postales, teléfonos, DNI para facturación). Si la relación con el responsable del tratamiento (el titular del e-commerce) no está formalizada contractualmente con Bentian como subencargado/proveedor tecnológico, se vulnera el Art. 28 del RGPD.
3. **Sobrecarga Asimétrica de Soporte Técnico:**
   - Si una tienda externa falla en plena campaña de rebajas o Black Friday, la agencia traslada la histeria a Bentian mediante tickets urgentes. Bentian termina prestando soporte a 5 tiendas críticas por el precio ridículo de 99€, atendido por los recursos de una empresa unipersonal o de equipo reducido.
4. **Riesgo Reputacional Incontrolable:**
   - Si la agencia incurre en impago, cambia de proveedor o configura incorrectamente los almacenes de Factusol, las tiendas online de terceros dejarán de sincronizar. El cliente final culpará públicamente a "Bentian" de haber roto su negocio online, sin que Bentian supiera de la existencia de dicho cliente ni haya recibido un euro directo de él.

---

## 3. DIFERENCIACIÓN: EMPRESA LEGÍTIMA VS PICARESCA COMERCIAL

### 3.1 El Caso de Uso Legítimo de 2 Tiendas
Existe un caso técnico y empresarial donde conectar más de una tienda es completamente legítimo:
* **Mismo CIF / Misma Personalidad Jurídica:** Una misma sociedad mercantil (ej. "Suministros Industriales Martínez S.L.", CIF B-XXXXXXXX).
* **Mismo Almacén / Misma Base de Datos Factusol:** Los productos, existencias y series de facturación residen en el mismo archivo `.accdb` en el mismo equipo o red local.
* **Separación de Canales de Venta:**
  - Tienda 1: `b2c.martinez.es` (PVP minorista con IVA para particulares).
  - Tienda 2: `mayoristas.martinez.es` (Área privada B2B con tarifas profesionales y recargo de equivalencia).

En este escenario legítimo, los datos no pertenecen a empresas diferentes, no hay subarriendo comercial y el responsable legal es exactamente el mismo titular de la licencia.

### 3.2 Por qué el Add-on Público es un Coladero Inadmisible
* **Carencia de Validación Semántica:** Cualquier pasarela Stripe Checkout pública permite comprar un Add-on introduciendo un email cualquiera. Stripe no valida CIFs, no comprueba si los dominios web coinciden ni verifica la arquitectura de bases de datos.
* **Asimetría de Vigilancia:** En un software local-first con telemetría mínima (priorizando la privacidad y cero coste de servidor), auditar si dos URLs de WooCommerce pertenecen a la misma sociedad mercantil requeriría inspecciones manuales continuas o desarrollo de bots de verificación registral (cif, whois, aviso legal web).
* **Vulnerabilidad Innecesaria:** Vender un conector B2B a 99€ en abierto es abrir una puerta trasera auto-servicio para que cualquier agencia externalice su coste técnico a costa del margen de Bentian.

---

## 4. PROPUESTA DE RESOLUCIÓN COMERCIAL Y REGLAS DE BLINDAJE

### 4.1 Acción Inmediata: Retirada de Stripe Público
* **ELIMINAR YA** los productos `addon_extra_store_annual` (99 €) y `addon_extra_store_monthly` (12 €) del catálogo público de la web, del endpoint `GET /billing/plans` y de la pasarela de Stripe.
* La web pública solo debe mostrar:
  1. **Plan Base Todo Incluido (Anual)** – 199 € promo / 249 € estándar.
  2. **Plan Base Todo Incluido (Mensual)** – 29 € / mes.
  3. **Servicios de Implantación / Setup Asistido** (99 € / 249 € one-off).

---

### 4.2 Matriz de Opciones para la Gestión de Empresas Legítimas Multitienda

Evaluamos las tres vías posibles para dar respuesta a clientes con 2 tiendas bajo el mismo CIF:

| Criterio | Opción A: "1 Tienda = 1 Licencia Base" (RECOMENDADA) | Opción B: Add-on Tienda Extra Solo en Backoffice con Validación CIF | Opción C: Plan Corporativo Dual (349 €/año) |
|---|---|---|---|
| **Mecánica** | Cada tienda web requiere 1 Licencia Base completa. | Solo accesible desde soporte o portal privado tras aportar CIF y factura idéntica. | Plan empaquetado para 2 tiendas en una sola suscripción. |
| **Precio Cliente** | 199 € + 199 € = **398 € / año** (o 159 € la 2ª con descuento familiar del 20%). | 199 € + 99 € = **298 € / año**. | **349 € / año** (ahorro frente a 398 €). |
| **Ingreso para Bentian** | **Máximo** (358 € – 398 €). | **Medio** (298 €). | **Alto** (349 €). |
| **Fricción Técnica/Desarrollo** | **Cero**. Utiliza la infraestructura actual de licencias sin modificar el core. | **Alta**. Requiere verificación manual o lógica de validación de dominios/CIF en DB. | **Baja**. Nuevo plan en Stripe asignado manualmente. |
| **Riesgo de Fuga / Fraude** | **0%**. Imposible arbitrar porque cada conexión paga precio de licencia completa. | **Medio**. Si la agencia aporta facturas trucadas o dominios pantalla. | **Bajo**. Al costar 349€ el arbitraje de 2 tiendas ya no es rentable para agencias. |
| **Percepción del Cliente Legítimo** | Muy favorable. 398€/año por dos tiendas sigue siendo infinitamente más barato que Conecta HUB (1.500€/año). | Exclusiva pero burocrática. | Profesional y transparente. |

---

### 4.3 Recomendación Definitiva y Dictamen del Analista

#### 1. Adopción de la "Regla de Oro": 1 Tienda Online = 1 Licencia Base
En software de sincronización de datos de misión crítica, la unidad económica generadora de valor (y de coste de soporte) es la **Tienda Online conectada**, no el ordenador donde corre el ejecutable.
* Una empresa con 2 tiendas genera el doble de llamadas a la API de WooCommerce, el doble de riesgo de rotura por actualizaciones de plugins de WordPress y el doble de demanda de soporte.
* Pagar **199 € / año por tienda** (menos de 17 €/mes) es un precio extraordinariamente competitivo que ningún cliente legítimo rechazará. Competidores directos como QFACWIN o Conecta HUB cobran entre 800 € y 2.000 € por tienda adicional.

#### 2. Protocolo de Descuento por "Mismo CIF" (Cierre Comercial Controlado)
Para satisfacer comercialmente a clientes legítimos (B2B + B2C) sin abrir brechas en la web:
* Si un cliente acredita documentalmente que ambas tiendas pertenecen al mismo CIF y comparten la misma base de datos de Factusol:
  - Se le ofrece un **cupón de descuento de fidelización del 20% en su 2ª Licencia Base** (ej. 199 € la 1ª + 159 € la 2ª = **358 € / año** en total).
  - Ambas tiendas quedan dadas de alta como licencias independientes en base de datos, con claves criptográficas separadas y métricas aisladas.
  - Si una tienda se desvincula o se vende, la gestión de licencias permanece limpia y sin deudas técnicas.

#### 3. Blindaje Contractual en los Términos de Servicio (Anti-Subarriendo)
Incluir en los Términos y Condiciones Generales de Uso de Bentian la siguiente cláusula mandataria:
> *"Queda expresamente prohibido el subarriendo, cesión, explotación comercial compartida o uso de una misma licencia o instancia de Bentian ERP Bridge para conectar dominios, tiendas o almacenes de terceras personas físicas o jurídicas distintas a la entidad titular del contrato. La detección de actividad multientidad sin autorización previa facultará a Bentian para la resolución inmediata del contrato y revocación de licencias sin derecho a reembolso."*

#### 4. Canal de Partners Blindado
Comunicar a las agencias y empresas informáticas asociadas que:
* Para cada uno de sus clientes deben tramitar una **Licencia Base con el CIF y datos del cliente final**.
* La agencia percibirá su **25% de comisión recurrente neta oficial** por cada licencia vendida, garantizando un flujo de ingresos recurrente y legal, respaldado por el soporte directo de Bentian.

---

## 5. PLAN DE ACCIÓN INMEDIATO

1. **Retirar Add-on de la Web:** Eliminar `addon_extra_store_annual` y `addon_extra_store_monthly` de `apps/api/src/routes/billing.router.ts` y de los formularios de precios de la web pública.
2. **Actualizar el Blueprint de Negocio:** Modificar el documento `203 - Pricing.md` para consagrar el principio de *"1 Licencia Base por cada Tienda Online conectada"*.
3. **Implantar Cupones Privados de Stripe:** Configurar en Stripe el cupón `MISMO-CIF-20` aplicable exclusivamente por el administrador a segundas licencias verificadas.
4. **Actualizar ToS:** Incorporar la cláusula anti-subarriendo en los términos legales de la plataforma.

---
*Dictamen emitido por el Analista de Riesgo de Negocio, Monetización y Anti-Fraude de Bentian ERP Bridge.*
