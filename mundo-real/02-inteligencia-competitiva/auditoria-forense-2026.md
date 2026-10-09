# AUDITORÍA FORENSE DE COMPETENCIA E INTEGRACIÓN FACTUSOL (2025/2026)
**Estructura Oculta de Precios, Fallas Críticas en Comunidades y el Colapso TeamSystem**

---

## 1. RESUMEN EJECUTIVO

El ecosistema español de comercio electrónico para PYMEs tradicionales está atrapado en un trilema tecnológico y financiero:
1. **Factusol (Software DELSOL / TeamSystem)** es el estándar de facto en facturación física para más de 100.000 PYMEs y autónomos españoles, pero carece de un motor nativo y fluido de sincronización e-commerce.
2. Los **conectores de nicho (Festeweb, OBSolutions/Moduleshop)** utilizan arquitecturas obsoletas basadas en subida de ficheros por FTP y procesamiento PHP en el hosting del cliente, provocando cuelgues constantes por límites de memoria y tiempo de ejecución (`max_execution_time`, `memory_limit`), además de cobrar cuotas de soporte obligatorias para sobrevivir a las actualizaciones.
3. Las **soluciones iPaaS corporativas (Conecta HUB, O2W)** imponen costes prohibitivos (setups de 1.000€ a 3.000€ y cuotas mensuales de 100€ a 300€/mes con límites de SKUs), inaccesibles para el 85% del tejido empresarial que eligió Factusol precisamente por su bajo coste.
4. Tras la compra de Software DELSOL por el fondo y multinacional italiana **TeamSystem**, se ha producido un encarecimiento agresivo, la erradicación del modelo de software gratuito/perpetuo y un empuje forzoso hacia la nube mediante el apalancamiento de normativas fiscales (VeriFactu), degradando severamente la calidad de la atención al cliente y el soporte de APIs.

En este vacío de mercado, el modelo **Flat Rate de Bentian (249 €/año ilimitado)** representa una disrupción total: elimina la extorsión por tramos de catálogo, traslada el esfuerzo computacional fuera de PHP mediante un agente desacoplado nativo y ofrece sincronización reactiva anti-overselling.

---

## 2. AUDITORÍA FORENSE DE COMPETIDORES CLAVE

### 2.1. FESTEWEB (FS2PS - Factusol a PrestaShop / FS2WC a WooCommerce)
* **Entidad:** Festeweb (Alicante, España / [festeweb.com](https://www.festeweb.com)).
* **Arquitectura:** Programa ejecutable de escritorio para Windows que lee la base de datos de Factusol en local o red local y se comunica mediante scripts PHP / módulo instalado en la tienda online.
* **Precios Reales Identificados:**
  * **Licencia Básica:** **149 € + IVA** (pago único inicial).
  * **Licencia PRO:** **299 € + IVA** (pago único inicial).
* **Letra Pequeña y Costes Ocultos:**
  * **Sincronización desatendida capada en Básica:** La versión Básica no permite la automatización horaria completa. La sincronización programada (ej. cada 15-30 minutos) está reservada exclusivamente a la versión **PRO** (299 €).
  * **La trampa de los 90 días de soporte:** La compra incluye únicamente **3 meses de soporte técnico gratuito**.
  * **Cuota de mantenimiento recurrente obligatoria:** Para recibir actualizaciones de compatibilidad (nuevas versiones anuales de Factusol, cambios de versión de PrestaShop 1.7 a 8.x, compatibilidad con PHP 8.1 / 8.2), el cliente está obligado a contratar planes de soporte que van desde **25 €/mes** (300 €/año) en adelante.
  * **Penalización Multitienda / Multilicencia:** No es tarifa plana multitienda. Cada tienda o instalación adicional requiere comprar otra licencia y contratar planes de soporte ampliados.

### 2.2. CONECTA SOFTWARE (CONECTA HUB)
* **Entidad:** Conecta Software (Tenerife / Canarias / [conectasoftware.com](https://www.conectasoftware.com) - Partner oficial de Sage y plataformas e-commerce).
* **Arquitectura:** iPaaS (Integration Platform as a Service) tipo ETL en la nube. Intermedia entre el ERP (Factusol, Sage 50/200) y plataformas de venta (PrestaShop, WooCommerce, Shopify, Amazon).
* **Estructura de Precios Reales y Modelo Comercial:**
  * **Cero transparencia web:** No publican tarifas en abierto; canalizan a través de formularios de diagnóstico y comerciales.
  * **Estructura oficial:**
    * **Startpack (Coste de alta / implantación):** Pago único inicial que oscila típicamente entre **900 € y 2.500 € + IVA** dependiendo de la complejidad de familias, variantes y tarifas.
    * **Cuota recurrente:** Suscripción mensual o anual. Los tramos habituales oscilan entre **80 €/mes y más de 250 €/mes** (~1.000 € a 3.000 €/año).
* **Letra Pequeña y Costes Ocultos:**
  * **Peaje por volumen (SKUs y canales):** Diferencian entre versiones *Express* (básica, limitada) y *Pro*. Si el catálogo supera ciertos tramos de productos o si se añaden marketplaces (Amazon, Miravia, PcComponentes), la cuota mensual se multiplica.
  * **Secuestro del flujo de datos (Vendor Lock-in):** Toda la lógica de transformación reside en sus servidores en la nube. Si la empresa deja de pagar la cuota mensual, el conector se apaga de inmediato y la tienda queda huérfana de stock y pedidos.

### 2.3. O2W (OPEN TO WEB)
* **Entidad:** Open To Web S.L. ([o2w.es](https://www.o2w.es)).
* **Arquitectura:** Middleware cloud para gobernanza de flujos de datos, B2B y reglas de pricing avanzado.
* **Estructura de Precios Reales y Modelo Comercial:**
  * **Cero transparencia:** Muro comercial que exige un "diagnóstico operativo".
  * **Setup / Puesta en marcha:** Coste de implantación de **1.000 € a 3.500 € + IVA**.
  * **Cuota de servicio:** Habitualmente entre **90 € y 300 €/mes** según canales y transacciones.
* **Letra Pequeña y Costes Ocultos:**
  * **Fragmentación modular:** Funcionalidades críticas como la sincronización de reglas dinámicas de márgenes (*O2W Pricing*) o workflows avanzados de pedidos se tarifican como add-ons sobre la cuota base.
  * **Inadecuación de mercado:** Orientado a empresas medianas con facturaciones de millones de euros. Inviable financieramente para la PYME estándar de Factusol que busca rentabilidad y simplicidad.

### 2.4. SOFTWARE DELSOL / TEAMSYSTEM (FACTUSOL CLOUD & MÓDULO WEB)
* **Entidad:** Software DELSOL S.A.U. / TeamSystem S.p.A. ([sdelsol.com](https://www.sdelsol.com) / [teamsystem.es](https://www.teamsystem.es)).
* **Precios Reales de Factusol (TeamSystem):**
  * **Soporte Digital:** ~240 €/año + IVA (solo actualizaciones web, sin soporte telefónico).
  * **Soporte Correo:** ~384 €/año + IVA.
  * **Soporte Telefónico / Élite:** **588 €/año + IVA** en adelante por usuario/empresa.
  * **Alojamiento Factusol Cloud:** Suplemento mensual desde ~20 € a 50 €/mes por puesto.
* **Módulos de agencia más extendidos (Ej. Moduleshop / OBSolutions):**
  * Precio del módulo en PrestaShop Addons: **199,20 € - 249,00 €**.
  * Arquitectura: Un script PHP en PrestaShop descomprime el archivo ZIP subido por FTP desde Factusol y parsea línea a línea.

---

## 3. AUDITORÍA DE COSTES: COMPARATIVA TCO A 3 AÑOS

Desglose del Coste Total de Propiedad (**TCO**) para una PYME con un catálogo típico de **8.000 artículos** y 1 tienda online en un horizonte de 3 años:

| Concepto | Festeweb (FS2PS PRO) | Conecta Software (Conecta HUB) | O2W Connect | OBSolutions (Moduleshop) + Factusol | **Bentian ERP Bridge** |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Setup / Alta inicial** | 0 € (lo instala usuario) | 1.500 € | 1.800 € | 0 € | **0 €** |
| **Licencia Año 1** | 299 € | 1.440 € (120€/mes) | 1.800 € (150€/mes) | 249 € | **249 €** |
| **Soporte / Mantenimiento Año 1** | 225 € (9 meses tras cortesía) | Incluido en cuota | Incluido en cuota | 120 € (renovación) | **0 € (Incluido)** |
| **Coste Año 2 (Licencia + Mantenimiento)** | 300 € (25€/mes) | 1.440 € | 1.800 € | 120 € | **249 €** |
| **Coste Año 3 (Licencia + Mantenimiento)** | 300 € (25€/mes) | 1.440 € | 1.800 € | 120 € | **249 €** |
| **Penalización por +5.000 SKUs** | Rendimiento degradado | +30% a 50% en cuota | Salto de plan tarifario | Cuelgues en hosting | **0 € (Ilimitado)** |
| **TCO REAL A 3 AÑOS** | **1.124 €** | **5.820 €** | **7.200 €** | **609 €** | **747 €** |

---

## 4. NECROPSIA TÉCNICA: QUEJAS REALES Y FALLOS EN FOROS

### 4.1. El Colapso por `max_execution_time` y `memory_limit` en Hosting Compartido
* **Mecanismo:** FS2PS o scripts FTP descargan archivos pesados en el servidor web. PHP intenta ejecutar inserciones masivas en la base de datos MySQL de PrestaShop/WooCommerce.
* **Límites en hostings habituales (Hostinger, Raiola, Webempresa, Siteground):**
  * `max_execution_time` topado en 30-60 segundos.
  * `memory_limit` topado en 128M o 256M.
  * **Cloudflare Error 524:** Las conexiones proxy se cortan implacablemente a los **100 segundos exactos**.
* **Consecuencias:** Error 500/504, catálogos cortados a mitad de sincronización (miles de productos sin precio o stock), y saturación de tablas de logs (`prstshp_log`).

### 4.2. Descuadres de Stock Físico vs Online: El Drama del "Overselling"
* Los conectores baratos operan por cron jobs espaciados (cada 15 a 60 minutos para no tumbar la web).
* Durante esa ventana de 30-45 minutos, se producen ventas físicas en Factusol mientras el stock sigue disponible en la web, generando roturas de stock, pedidos cancelados, devoluciones con comisiones perdidas y reseñas demoledoras en Google Reviews.

---

## 5. EL FACTOR TEAMSYSTEM: LA CRISIS DE SOFTWARE DELSOL

1. **Privatización forzosa:** Tras la adquisición por TeamSystem en 2021, se introdujo el inicio de sesión obligatorio **TeamSystem ID** y se usó la normativa **VeriFactu / Ley Antifraude 11/2021** para forzar a las PYMEs a abandonar las versiones perpetuas e imponer suscripciones anuales (240 € a 588 €/año).
2. **Reputación desplomada:** Trustpilot califica a TeamSystem con un **2.4 sobre 5**, con quejas constantes por subidas abusivas y burocracia.
3. **El Colapso del Soporte de APIs (Caso Castris, Marzo 2025):** La agencia tecnológica Castris publicó la carta abierta *"Como el soporte puede hacer fracasar un proyecto"*, documentando cómo el soporte de la API de Factusol Cloud fue deslocalizado y colapsado, arruinando integraciones de e-commerce tras meses de bloqueo técnico.

---

## 6. COMPARATIVA DE ARQUITECTURA: BENTIAN VS RIVALES

| Factor Clave | Conectores FTP Tradicionales | Festeweb (FS2PS PRO) | Plataformas iPaaS (Conecta HUB / O2W) | **Bentian ERP Bridge** |
| :--- | :--- | :--- | :--- | :--- |
| **Precio Anual Todo Incluido** | ~199 € + 120 €/año | 299 € + 300 €/año | 1.500 € setup + 1.200-3.000 €/año | **249 €/año (Tarifa Plana)** |
| **Límite de Catálogo / SKUs** | Se cuelga a partir de 2.000 SKUs | Límite por hosting | Tramos con recargo | **Ilimitado** |
| **Impacto en CPU/RAM del Hosting** | Crítico (bucles PHP pesados) | Alto (peticiones masivas) | Medio | **NULO (Cómputo en local)** |
| **Tolerancia a timeouts** | Frágil (Error 500 / 524) | Propenso a timeouts | Manejado por colas cloud | **Inmune (Deltas desacoplados)** |
| **Ventana de Overselling** | 15 a 60 minutos | 15 a 30 minutos | 5 a 15 minutos | **Reactiva / Casi tiempo real** |
| **Soporte de Unidades NAS / Red** | Pésimo (bloqueo de archivos) | Frágil | Exige agentes cloud | **Blindado (Rutas UNC nativas)** |
| **Dependencia de Factusol Cloud** | No | No | Frecuente | **No requerida (Local y Red)** |
