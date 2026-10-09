# Checklist Operativo: Alta de Autónomo en 48 Horas (Régimen de Pluriactividad)
**Ámbito:** España (Comunidad Valenciana / Albacete - Castilla-La Mancha)  
**Marco Legal Vigente:** 2026 (Orden HAC/1526/2024 de supresión del 037 y RD-ley 13/2022 de cotización por ingresos reales)  
**Situación del Titular:** Asalariado por cuenta ajena (Régimen General) + Inicio de actividad por cuenta propia (RETA).

---

## 1. Cronograma Estricto de Ejecución (Orden Inalterable)

```text
 ┌────────────────────────────────────────────────────────────────────────┐
 │ PASO 0 (Previo): Identidad Digital y Cuenta Bancaria Colaboradora       │
 ├────────────────────────────────────────────────────────────────────────┤
 │ PASO 1 (Día D - Mañana): Alta Censal AEAT (Modelo 036 - Censos WEB)     │
 │ ├── Epígrafes IAE: 845 (SaaS) y 763 (Servicios de programación)        │
 │ └── Solicitud ROI (VIES): Casillas 582 y 584                           │
 ├────────────────────────────────────────────────────────────────────────┤
 │ PASO 2 (Día D - Tarde / Mismo Día): Alta en Seguridad Social (RETA)    │
 │ ├── Portal Import@ss con Certificado Digital                           │
 │ ├── Solicitud Tarifa Plana estatal de 80 €/mes                         │
 │ └── ¡MISMA FECHA O PREVIA a la declarada en Hacienda!                  │
 ├────────────────────────────────────────────────────────────────────────┤
 │ PASO 3 (Semana 1): Protección Registral (14,97 €) y Pasarela Stripe     │
 └────────────────────────────────────────────────────────────────────────┘
```

---

## 2. Fase 0: Preparación de Documentos y Cuentas (Día Previo)

- [ ] **Certificado Digital FNMT de Persona Física:** Verificar que está instalado en el navegador y no caducado.
- [ ] **AutoFirma (Gobierno de España):** Descargar, instalar y comprobar funcionamiento en la sede de la AEAT.
- [ ] **Apertura de Cuenta Bancaria Colaboradora de la TGSS:**
  - [x] **BBVA Cuenta Online Autónomos** o **N26 Business** (ambas colaboradoras oficiales con IBAN `ESxx`).
  - [ ] ⚠️ *Evitar Revolut Business para el alta en la Seguridad Social (la TGSS rechaza su domiciliación).*
- [ ] **Confirmación de Pluriactividad:**
  - [ ] Verificar que no existe cláusula de exclusividad expresa con compensación económica en el contrato laboral por cuenta ajena.
  - [ ] Asumir que **NO se pueden pedir subvenciones para desempleados (como LABORA EAUTOE)**.

---

## 3. Fase 1: Alta en la Agencia Tributaria (Modelo 036)

Acceder a la Sede Electrónica de la AEAT > **Censos, NIF y domicilio fiscal** > **Modelo 036 (Censos WEB)**.

### Casillas y Secciones Obligatorias:
- [ ] **Página 1 — Causas de presentación:**
  - Marcar **Casilla 111:** Alta en el censo de empresarios, profesionales y retenedores.
  - Consignar fecha de inicio de la actividad (Día D).
- [ ] **Página 4 — Actividades Económicas e IAE:**
  - **Alta Actividad 1 (SaaS y Licencias):**
    - Epígrafe: **845** (Sección 1ª - Empresarial).
    - Tipo: *Explotación electrónica por terceros y servicios de tratamiento de datos*.
    - Motivo: Justifica los cobros recurrentes de licencias y suscripciones de software sin retención de IRPF.
  - **Alta Actividad 2 (Servicios Técnicos e Integración):**
    - Epígrafe: **763** (Sección 2ª - Profesional).
    - Tipo: *Programadores y analistas de informática*.
    - Motivo: Permite facturar servicios de configuración, setup asistido (99€) y consultoría aplicando la retención reducida del 7% a empresas españolas.
  - Domicilio de la actividad: Marcar el domicilio habitual si no se dispone de local comercial abierto al público.
- [ ] **Página 5 — Impuesto sobre el Valor Añadido (IVA):**
  - Marcar **Casilla 500:** Régimen General del IVA (tipo general del 21%).
  - Indicar la fecha de inicio de operaciones en la Casilla 501.
- [ ] **Página 5 — Registro de Operadores Intracomunitarios (ROI / VIES):**
  - Marcar **Casilla 582:** Solicitar inscripción en el ROI.
  - Marcar **Casilla 584:** Fecha prevista para la primera operación comunitaria (misma fecha Día D).
  - *Resultado:* Permite comprar OpenAI, Supabase, Vercel y GitHub al **0% de IVA por Inversión del Sujeto Pasivo**.
- [ ] **Firma y Envío:** Firmar telemáticamente con AutoFirma y descargar el resguardo PDF oficial con CSV (Código Seguro de Verificación).

---

## 4. Fase 2: Alta en la Seguridad Social (RETA) vía Import@ss

Acceder a **Import@ss (Portal de la Tesorería General de la Seguridad Social)** > **Altas, bajas y modificaciones** > **Alta en trabajo autónomo**.

> [!CAUTION]
> **REGLA DE VIDA O MUERTE FINANCIERA:** El alta en el RETA debe firmarse **el mismo día declarado en Hacienda o con carácter previo (hasta 60 días antes)**.  
> Si Hacienda tiene fecha 15 de septiembre y en la Seguridad Social se firma el 16 de septiembre, el alta se considera fuera de plazo, **se pierde automáticamente la Tarifa Plana de 80 €/mes** y se genera una deuda inmediata de cuota completa (~300 €).

- [ ] **Fecha de inicio de actividad:** Poner **exactamente la misma fecha** consignada en la casilla de Hacienda.
- [ ] **Código de Actividad (CNAE):** Seleccionar **6201** (*Actividades de programación informática*) o **6311** (*Proceso de datos, hosting y actividades relacionadas*).
- [ ] **Régimen de Cotización:** Seleccionar acogimiento a la **Tarifa Plana Reducida (Artículo 38 ter de la LGSS)**.
  - Cuota mensual: **80 €/mes** (+ MEI = ~86 €/mes totales durante los primeros 12 meses).
  - Es 100% compatible con estar contratado en el Régimen General por cuenta ajena.
- [ ] **Datos Bancarios:** Introducir el IBAN de la cuenta de autónomo abierta en el Paso 0 para la domiciliación bancaria de la cuota (se cargará a mes vencido, el último día hábil de cada mes).
- [ ] **Firma del alta:** Firmar y descargar la Resolución de Alta en el Régimen Especial de Trabajadores Autónomos (RETA).

---

## 5. Fase 3: La Hucha Fiscal y las Obligaciones Trimestrales

Al operar en pluriactividad, los rendimientos del trabajo (nómina) y los rendimientos de actividades económicas (Bentian) se suman en la **Base Imponible General del IRPF**.

- [ ] **Regla de la Reserva Financiera ("Hucha Fiscal"):**
  - De cada pago que entre por Stripe o transferencia, **apartar de inmediato entre el 25% y el 30% en una cuenta o subcuenta separada**.
  - Este dinero cubrirá sin sobresaltos:
    1. El **Modelo 130** trimestral (pago a cuenta del 20% del rendimiento neto de Bentian).
    2. El ajuste marginal en la **Declaración de la Renta anual (Modelo 100)** en mayo/junio.
- [ ] **Calendario de Presentaciones Tributarias:**
  - **1 al 20 de Abril:** Modelo 303 (IVA T1) + Modelo 130 (IRPF T1) + Modelo 349 (Intracomunitario).
  - **1 al 20 de Julio:** Modelo 303 (IVA T2) + Modelo 130 (IRPF T2) + Modelo 349 (Intracomunitario).
  - **1 al 20 de Octubre:** Modelo 303 (IVA T3) + Modelo 130 (IRPF T3) + Modelo 349 (Intracomunitario).
  - **1 al 30 de Enero:** Modelo 303 (IVA T4) + Modelo 130 (IRPF T4) + Modelo 390 (Resumen Anual IVA) + Modelo 349.
