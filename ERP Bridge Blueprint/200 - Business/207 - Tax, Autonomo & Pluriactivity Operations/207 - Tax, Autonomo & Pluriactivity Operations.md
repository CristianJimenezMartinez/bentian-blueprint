# Tax, Autónomo & Pluriactivity Operations — v1.0
> **Estado:** Aprobado  
> **Autor:** Cristian Jiménez Martínez (Arquitecto de Software & Fundador)  
> **Marco Legal:** Real Decreto Legislativo 8/2015 (LGSS - Pluriactividad y RETA), Real Decreto-ley 13/2022 (Tarifa Plana Estatal), Ley 58/2003 (LGT), Ley 37/1992 (LIVA), Real Decreto 439/2007 (RIRPF), Orden HAC/1526/2024 (Supresión Modelo 037 y Asistente CensosWEB Modelo 036).

---

# 207 - Tax, Autónomo & Pluriactivity Operations

> *Guía operativa y fiscal integral para iniciar la actividad económica de Bentian / Sincriva compatibilizándola con un empleo por cuenta ajena (Pluriactividad), alta telemática en 24-48h, optimización de cotizaciones, modelos tributarios y pasarelas de cobro.*

---

## 1. Propósito y Modelo de Pluriactividad

Este documento establece la arquitectura fiscal, laboral y administrativa para operar **Bentian / Sincriva** de forma 100% legal en España mientras el fundador mantiene simultáneamente su contrato de trabajo por cuenta ajena en el Régimen General.

```text
                                RÉGIMEN DE PLURIACTIVIDAD
 ┌────────────────────────────────────────────────────────────────────────────────────────┐
 │                                CRISTIAN JIMÉNEZ MARTÍNEZ                               │
 └───────────────────────────┬────────────────────────────────┬───────────────────────────┘
                             │                                │
                             ▼                                ▼
              【 RÉGIMEN GENERAL (Cuenta Ajena) 】       【 RETA (Cuenta Propia / Bentian) 】
              • Jornada laboral contratada.              • Autónomo en estimación directa simplificada.
              • Retención IRPF en nómina mensual.        • Tarifa Plana reducida de 80 €/mes (Año 1).
              • Cotización contingencias en empresa.     • Epígrafes IAE 845 (SaaS) y 763 (Servicios).
              • Cobertura sanitaria y desempleo base.    • Emisión de facturas y cobros vía Stripe.
```

---

## 2. Requisitos Previos y Advertencias Críticas

### 2.1 Identidad Digital y Software
1. **Certificado Digital de la FNMT (Persona Física)** o **DNIe** activo con lector y claves operativas.
2. **AutoFirma (Gobierno de España):** Instalada y verificada en el navegador para la firma telemática de los formularios de la AEAT y la Sede de la Seguridad Social.
3. **Cl@ve Permanente / Cl@ve Móvil** como respaldo de autenticación rápida.

### 2.2 Cuenta Bancaria: Compatibilidad Obligatoria con la Seguridad Social
Para domiciliar la cuota de autónomos y liquidar impuestos, la cuenta bancaria debe tener **IBAN español (`ESxx`)** y pertenecer a una **Entidad Financiera Colaboradora de la Tesorería General de la Seguridad Social (TGSS)**.

| Entidad | Coste Mantenimiento | Compatibilidad AEAT (Impuestos) | Compatibilidad TGSS (Cuota RETA) | Veredicto |
| :--- | :--- | :--- | :--- | :--- |
| **BBVA Cuenta Online Autónomos** | 0 € / mes | SÍ (Colaboradora) | SÍ (Colaboradora) | ⭐ **Recomendada #1.** Cero comisiones, alta online en 10 minutos, soporte íntegro de recibos RETA. |
| **N26 Business (Plan Estándar)** | 0 € / mes | SÍ (Colaboradora) | SÍ (Colaboradora oficial TGSS) | ⭐ **Excelente alternativa Fintech.** Tarjeta virtual con cashback para gastos de software. |
| **Banco Sabadell Cuenta Online Autónomos** | 0 € / mes | SÍ (Colaboradora) | SÍ (Colaboradora) | Sólida alternativa tradicional sin comisiones. |
| **Revolut Business** | 0 € / mes | SÍ (Colaboradora AEAT) | ❌ **NO es colaboradora de la TGSS** | ⚠️ **PELIGRO:** La Seguridad Social **rechaza el IBAN de Revolut** al tramitar el alta del RETA. |

### 2.3 Exclusión de Ayudas para Desempleados (LABORA EAUTOE)
> [!WARNING]
> Las subvenciones autonómicas al inicio de actividad para autónomos (como el programa **EAUTOE de LABORA en la Comunidad Valenciana**, con importes de 5.000 € a 6.600 €) exigen expresamente que el solicitante esté en situación de **desempleo e inscrito como demandante de empleo no ocupado antes de la fecha de alta**.  
> **Al estar contratado por cuenta ajena, NO se puede solicitar esta ayuda.** Intentar solicitarla estando en activo constituye una causa de denegación automática y una infracción tributaria/laboral.

---

## 3. Tramitación Administrativa Paso a Paso (Cronograma 24-48h)

```text
 ┌─────────────────────────┐     ┌─────────────────────────┐     ┌─────────────────────────┐
 │ PASO 0: DÍA PREVIO      │     │ PASO 1: MAÑANA DÍA D    │     │ PASO 2: TARDE DÍA D     │
 │ • Certificado FNMT      │     │ • Hacienda (AEAT)       │     │ • Seg. Social (RETA)    │
 │ • AutoFirma instalada   │ ──► │ • Modelo 036 (CensosWEB)│ ──► │ • Portal Import@ss      │
 │ • Cuenta BBVA/N26 lista │     │ • IAE 845 + 763 (Dual)  │     │ • Tarifa Plana 80 €/mes │
 │ • IBAN para RETA        │     │ • Casillas ROI 582/584  │     │ • ¡Mismo día o previo!  │
 └─────────────────────────┘     └─────────────────────────┘     └─────────────────────────┘
```

### PASO 1: Alta Censal en la AEAT — Modelo 036
Desde el 3 de febrero de 2025 (Orden HAC/1526/2024), el antiguo Modelo 037 está oficialmente derogado. El alta se efectúa exclusivamente mediante el **Modelo 036** a través del asistente **Censos WEB** en la Sede Electrónica de la Agencia Tributaria.

#### A) Elección de Epígrafes IAE: Alta Dual (845 + 763)
Para operar legalmente un modelo de software compuesto por una suscripción SaaS/cloud y servicios técnicos de instalación local:

1. **Epígrafe 845 (Sección 1ª - Empresarial):**
   * *Descripción:* «Explotación electrónica por cuenta de terceros y servicios de tratamiento de datos».
   * *Uso en Bentian:* Cobro recurrente de licencias SaaS a través de Stripe, acceso a API de sincronización y telemetría.
   * *Retención IRPF:* **NO lleva retención en factura** (las actividades empresariales no retienen).
   * *Respaldo jurídico:* Consulta Vinculante de la DGT V0979-25.
2. **Epígrafe 763 (Sección 2ª - Profesional):**
   * *Descripción:* «Programadores y analistas de informática».
   * *Uso en Bentian:* Servicios de instalación asistida del Local Agent (addon de 99 €), configuración de mapeos de bases de datos, consultoría técnica y soporte especializado.
   * *Retención IRPF:* **Lleva 7% de retención** en facturas dirigidas a empresas y autónomos en España.

#### B) Régimen de IVA
* **Régimen General del IVA (21%):** Se marca en la página 5 del Modelo 036.
* Todas las ventas a clientes situados en territorio español devengan el 21% de IVA repercutido.

#### C) Alta en el ROI (Registro de Operadores Intracomunitarios - VIES)
* **Página 5 (Apartado IVA del Modelo 036):**
  * Marcar **Casilla 582** (Solicitud de inscripción en el ROI).
  * Indicar en la **Casilla 584** la fecha prevista para la primera operación (misma fecha de inicio de actividad).
* **Finalidad crítica:** Asigna un **NIF-IVA europeo (`ES` + DNI)** visible en la base de datos VIES de la UE.
* **Ahorro directo en stack tecnológico:** Al introducir este NIF-IVA en proveedores con sede comunitaria (**OpenAI Ireland Ltd, Vercel Inc, GitHub/Microsoft Ireland, Supabase, Cloudflare, AWS Europe**), las facturas se emiten con **0% de IVA por Inversión del Sujeto Pasivo** (art. 84.Uno.2º LIVA), eliminando la necesidad de adelantar el 21% de IVA extranjero.

---

### PASO 2: Alta en la Seguridad Social (RETA) vía Import@ss

> [!CAUTION]
> **REGLA TEMPORAL INQUEBRANTABLE:** El alta en el RETA debe tramitarse **el mismo día en que se inicia la actividad declarada en Hacienda (o con carácter previo hasta 60 días antes)**.  
> Si se declara inicio de actividad en Hacienda el día 15 y el trámite de Import@ss se firma el día 16, la Seguridad Social considera el alta **extemporánea**: se extingue de forma automática el derecho a la Tarifa Plana de 80 € y se cobran recargos inmediatos sobre la cuota general.

#### Tarifa Plana Estatal de 80 €/mes (RD-ley 13/2022)
* **Compatibilidad expresa con pluriactividad:** La normativa vigente permite acogerse a la cuota reducida de **80 €/mes durante los primeros 12 meses** con independencia de que el trabajador esté dado de alta en el Régimen General por cuenta ajena.
* **Coste real mensual:** ~86 €/mes (80 € de cuota reducida + 0,70% del Mecanismo de Equidad Intergeneracional - MEI).
* **Meses 13 al 24:** Posibilidad de prórroga de la tarifa reducida únicamente si los rendimientos netos computables de la actividad económica se sitúan por debajo del Salario Mínimo Interprofesional (SMI).

#### Devolución de Cuotas por Exceso de Cotización (Art. 313 LGSS)
* Si la suma de las cotizaciones por contingencias comunes realizadas en el Régimen General (empresa + trabajador) y las abonadas en el RETA supera el tope anual establecido por la Ley de Presupuestos Generales del Estado:
* La Tesorería General de la Seguridad Social **reintegra de oficio el 50% del exceso de cotizaciones ingresadas** mediante transferencia bancaria antes del 1 de mayo del ejercicio siguiente, sin necesidad de presentar solicitud formal.

---

## 4. Obligaciones Fiscales y Mecánica de Facturación

### 4.1 Retención de IRPF en Facturas Emitidas
* **Tipo reducido del 7%:** Aplicable durante el año de inicio de la actividad y los dos ejercicios fiscales inmediatos posteriores (artículo 95.1 del Reglamento del IRPF).
* **Aplicación práctica:**
  * **Facturas por servicios de integración (Epígrafe 763) a empresas/autónomos en España:** Llevan retención del **-7%** deducida de la base imponible.
  * **Leyenda legal obligatoria al pie de factura:**
    > *"Operación sujeta a retención del 7% de IRPF en virtud de lo establecido en el artículo 95.1 del Reglamento del IRPF (Real Decreto 439/2007) por encontrarse el emisor en el período de inicio de su actividad profesional."*
  * **Suscripciones SaaS (Epígrafe 845):** **0% de retención**. Las facturas emitidas por la cuota anual de 249 € o mensual de 29 € no admiten retención al constituir actividad empresarial.
  * **Clientes particulares (B2C) o clientes extranjeros (UE / Resto del mundo):** **0% de retención**.

### 4.2 Calendario Fiscal Trimestral y Anual

| Modelo | Objeto | Plazos de Presentación | Implicación para Bentian |
| :--- | :--- | :--- | :--- |
| **Modelo 303** | Autoliquidación de IVA | • T1: 1 - 20 abril<br>• T2: 1 - 20 julio<br>• T3: 1 - 20 octubre<br>• T4: 1 - 30 enero | Se liquida: `IVA Repercutido (21%) - IVA Soportado deducible`. Domiciliación bancaria disponible hasta el día 15 de cada mes de vencimiento (25 en T4). |
| **Modelo 130** | Pago Fraccionado de IRPF (20%) | • T1: 1 - 20 abril<br>• T2: 1 - 20 julio<br>• T3: 1 - 20 octubre<br>• T4: 1 - 30 enero | **OBLIGATORIO.** Al tener una actividad empresarial (Epígrafe 845) donde las suscripciones SaaS no llevan retención, se debe abonar trimestralmente el 20% del rendimiento neto acumulado (ingresos - gastos). |
| **Modelo 349** | Declaración Operaciones Intracomunitarias | Trimestral (1 - 20 de mes siguiente) | **Informativo (sin pago).** Obligatorio al registrar compras de software a proveedores intracomunitarios (OpenAI, GitHub, Supabase, Vercel) con NIF-IVA. |
| **Modelo 390** | Resumen Anual de IVA | 1 - 30 enero | Informativo. Recopila las operaciones de los cuatro trimestres del Modelo 303. |
| **Modelo 100** | Declaración de la Renta (IRPF) | Abril - Junio | Liquidación global de la renta del contribuyente. |

---

## 5. El Impacto de la Pluriactividad en la Declaración de la Renta (IRPF)

### 5.1 Suma en la Base Imponible General
En la Declaración de la Renta anual (Modelo 100), los ingresos obtenidos por el empleo por cuenta ajena y el beneficio neto de Bentian se suman íntegramente:

$$\text{Base Imponible General} = \text{Rendimientos del Trabajo (Nómina)} + \text{Rendimientos Netos de Actividades Económicas (Bentian)}$$

### 5.2 El "Efecto Salto de Tramo"
* La empresa empleadora retiene IRPF en la nómina calculando exclusivamente el salario que ella abona (ej. un tipo medio del 14% al 18%).
* Todo el rendimiento neto generado por Bentian **se liquida en los tramos marginales superiores del contribuyente** (típicamente entre el **30% y el 37%** de tipo marginal en la Comunidad Valenciana o Castilla-La Mancha).
* Como en las facturas de servicios solo se habrá retenido un 7% (y en las de SaaS un 0%), en la liquidación de la Renta de mayo/junio existirá una diferencia a pagar a Hacienda.

> [!IMPORTANT]
> **REGLA DE RESERVA FINANCIERA (LA "HUCHA FISCAL"):**  
> De cada ingreso neto que entre en la cuenta por ventas de Bentian, **se debe apartar de forma automática entre un 25% y un 30% en una cuenta o subcuenta de reserva para impuestos**. De este modo, los pagos fraccionados del Modelo 130 y la liquidación anual de la Renta estarán cubiertos en su totalidad sin afectar la liquidez personal.

---

## 6. Blindaje Laboral con la Empresa Empleadora

Para garantizar una compatibilidad jurídica intachable con el contrato de trabajo vigente:

1. **Pacto de No Competencia y Buena Fe Contractual (Art. 5 y 21 del Estatuto de los Trabajadores):**
   * Bentian no debe competir directamente con las líneas de negocio ni con los clientes específicos de la empresa donde se trabaja por cuenta ajena.
   * La actividad se circunscribe a conectividad técnica y software propio de integración de ERPs de escritorio con ecommerce.
2. **Separación Absoluta de Medios Técnicos:**
   * Queda terminantemente prohibido utilizar el ordenador portátil corporativo, licencias de software corporativas, correos de la empresa empleadora o la red Wi-Fi de las oficinas para tareas vinculadas a Bentian.
   * Todo el desarrollo, despliegue, soporte y prospección se ejecuta exclusivamente desde hardware personal e infraestructura propia.
3. **Segregación Horaria Total:**
   * La actividad comercial y técnica de Bentian se desarrolla estrictamente fuera de la jornada laboral contratada (franjas de mañana 07:30 - 08:30, descansos de mediodía 14:00 - 15:00 y tardes 18:30 - 20:30).

---

## 7. Infraestructura de Facturación y Cobros (Stripe Billing)

Para eliminar tareas manuales de cobro mientras se trabaja por cuenta ajena:

```text
                               FLUJO DE COBRO AUTOMATIZADO
 ┌──────────────────────┐     ┌──────────────────────┐     ┌──────────────────────┐
 │ Landing Page         │     │ Stripe Checkout      │     │ Stripe Billing       │
 │ bridge.cristianjm.com│ ──► │ • Tarjeta / SEPA     │ ──► │ • Factura PDF legal  │
 │ • Botón 'Comprar'    │     │ • SCA / 3D Secure    │     │ • Webhook activa     │
 │ • Plan 249 €/año     │     │ • Desglose 21% IVA   │     │   licencia en cloud  │
 └──────────────────────┘     └──────────────────────┘     └──────────────────────┘
```

1. **Stripe Billing:** Configurado con los datos fiscales de Cristian Jiménez Martínez (NIF personal, dirección fiscal y cuenta IBAN de BBVA/N26).
2. **Gestión Automática del IVA:** Configurar en Stripe Tax el tipo del 21% para compradores con dirección en España. Para compradores de la UE con VAT Number válido, Stripe aplica automáticamente la exención por inversión del sujeto pasivo.
3. **Portal del Cliente (Stripe Customer Portal):** Permite que agencias y clientes descarguen sus facturas, gestionen métodos de pago o renueven licencias de forma autónoma sin necesidad de intervención manual.
