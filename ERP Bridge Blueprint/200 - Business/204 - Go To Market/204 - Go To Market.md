# Go To Market — v1.0
> **Estado:** Borrador

---

# 204 - Go To Market

> *Estrategia de lanzamiento y comercialización.*

---

## 1. Propósito

Definir cómo ERP Bridge llegará a sus primeros clientes reales con un presupuesto mínimo o nulo, sin equipo comercial y sin inversión en publicidad.

La estrategia debe ser ejecutable por una sola persona y generar tracción orgánica antes de cualquier gasto.

> **Principio:** Primero demostrar valor real a personas reales. Después cobrar.

---

## 2. Situación de Partida

ERP Bridge parte de las siguientes condiciones:

- **Equipo:** 1 persona (fundador-desarrollador).
- **Presupuesto de marketing:** €0 inicial.
- **Producto:** MVP funcional (Factusol ↔ WooCommerce).
- **Mercado objetivo:** PYMEs españolas con Factusol y tienda online.
- **Ventaja competitiva:** Experiencia real con Factusol/Access, conector ya probado, dashboard moderno, arquitectura preparada para escalar.

> **Restricción fundamental:** No se puede invertir dinero en marketing antes de que el producto genere ingresos. Toda la estrategia debe basarse en acciones directas, contenido gratuito y relaciones.

---

## 3. Cliente Ideal (ICP — Ideal Customer Profile)

### 3.1 Perfil principal (MVP)

```text
Empresa española (PYME)
├── Usa Factusol como ERP de facturación/gestión
├── Tiene tienda online con WooCommerce
├── Gestiona 200 – 5.000 productos
├── Introduce pedidos manualmente del ecommerce al ERP
├── Actualiza stock manualmente o con scripts frágiles
├── 1 – 15 empleados
└── Factura entre €100.000 – €2.000.000 / año
```

### 3.2 Dolor principal

> *"Cada día tengo que copiar los pedidos de la web a Factusol a mano, y actualizar el stock uno por uno. Si me equivoco, vendo algo que no tengo."*

### 3.3 Dónde encontrarlo

| Canal | Cómo identificarlos |
|---|---|
| **Google** | Buscar: `site:.es "factusol" intext:"tienda online" OR "woocommerce"` |
| **BuiltWith / PublicWWW** | Filtrar tiendas españolas con WooCommerce activo |
| **LinkedIn** | Buscar: "eCommerce Manager" OR "Responsable eCommerce" + España |
| **Foros de Factusol** | Usuarios que preguntan sobre sincronización con tienda online |
| **WordPress España Slack** | Canal `#desarrollo` — preguntas sobre integración ERP |
| **Forea.org** | Foro de contabilidad donde se discuten integraciones de Factusol |

---

## 4. Estrategia de Lanzamiento en 4 Fases

### Fase 0 — Validación (Coste: €0)

**Objetivo:** Conseguir 5-10 empresas beta que usen ERP Bridge gratis a cambio de feedback.

**Duración estimada:** 2-4 semanas desde que el MVP funcione.

**Acciones:**

1. **Identificar 30 empresas candidatas** usando los canales del punto 3.3.
2. **Contacto directo personalizado** (LinkedIn o email, NUNCA spam masivo):

   > *"Hola [Nombre], he visto que gestionas la tienda [Tienda.es] con WooCommerce. Estamos desarrollando un conector nativo para sincronizar stock y pedidos con Factusol sin caídas ni duplicidades.*
   >
   > *Estamos seleccionando 5 empresas en España para darles acceso 100% gratuito y soporte personalizado a cambio de probarlo y darnos feedback honesto.*
   >
   > *¿Te encajaría probarlo esta semana?"*

3. **Onboarding "concierge"** — No dejar que configuren solos. Hacer videollamada de 20 minutos, instalar el Agent, configurar credenciales y ejecutar la primera sincronización en directo.
4. **Tras 2 semanas de uso estable**, solicitar:
   - Reseña escrita de 2 párrafos.
   - Permiso para usar su logo y nombre como caso de éxito.
   - Una métrica concreta (ej. *"Sincroniza 2.000 SKUs y ahorra 15 horas/mes"*).

**Resultado esperado:** 3-5 empresas usando ERP Bridge en producción con datos reales y testimonios publicables.

> **Regla:** Durante la Fase 0 no se cobra nada. El objetivo es aprendizaje y prueba social, no ingresos.

---

### Fase 1 — Primeros Clientes de Pago (Coste: €0 – €80/mes)

**Objetivo:** Convertir beta testers en clientes de pago y conseguir los primeros 10-20 clientes mediante el Plan Único.

**Duración estimada:** Meses 2-4 post-MVP.

**Acciones:**

1. **Activar el Plan Único Base (249 €/año o 29 €/mes + IVA)** para nuevos usuarios, sin límites de productos ni pedidos.
2. **Mantener a los beta testers con condiciones especiales** (descuento del 50% de por vida o plan gratuito extendido 6 meses) como agradecimiento y para retenerlos como casos de éxito.
3. **Publicar los primeros casos de éxito** en la landing de ERP Bridge.
4. **Contenido SEO de alta intención** (ver sección 5).
5. **Darse de alta como autónomo** con Tarifa Plana cuando se produzca la primera venta (ver sección 7).

---

### 4.5 Infraestructura Web y Dominio a Coste CERO (€0.00)

Para validar y comercializar ERP Bridge sin gastar dinero en dominios adicionales antes de generar ingresos:

#### Elección del Dominio Anfitrión: `bridge.cristianjm.com`
Tras evaluar `veltiatrust.com` (infraestructura eIDAS/legaltech) y `cristianjm.com` (web personal de Cristian Jiménez Martínez, Software Architect):
* **Descarte de `veltiatrust.com`:** Mezclar sincronización de Factusol con firmas cualificadas eIDAS genera confusión cognitiva en el comprador pyme y diluye el prestigio regulatorio de Veltia Trust.
* **Elección de `bridge.cristianjm.com`:** La PYME española valora tratar con un profesional técnico cualificado con nombre y apellidos. Aporta máxima confianza para instalar un `.exe` local en su servidor.
* **Branding:** La landing opera con la marca independiente **Bentian ERP Bridge**, respaldada en el pie por Cristian Jiménez Martínez.

#### Arquitectura Técnica de Despliegue (€0/mes):
1. **DNS & Certificados SSL:** Registro CNAME `bridge` en Cloudflare DNS apuntando al hosting estático con SSL automático gratuito.
2. **Landing Page:** Alojada en Cloudflare Pages o Vercel (Hobby Tier) consumiendo el HTML estático de `apps/api/public`.
3. **Descargas de Instaladores (.exe):** Los binarios de instalación (`Bentian-Setup.exe` de ~24 MB) se alojan en **GitHub Releases**, permitiendo descargas ilimitadas a máxima velocidad sin sobrecargar servidores.
4. **Backend API:** Expuesto a través de **Cloudflare Tunnel (`cloudflared`)** hacia el contenedor Docker local/VPS en `api.bridge.cristianjm.com` sin abrir puertos en el router ni contratar IPs fijas.
5. **Base de Datos:** PostgreSQL en Frankfurt mediante **Supabase Free Tier** (500 MB).
6. **Desacople Futuro:** Al alcanzar los primeros 5 clientes de pago, se adquiere `bentian.es` o `erpbridge.es` (~8€) y se aplica redirección `301 Moved Permanently` en Cloudflare sin enlaces rotos.

---

### Fase 2 — Tracción Orgánica (Coste: €0 – €50/mes)

**Objetivo:** Generar un flujo constante de leads orgánicos sin invertir en publicidad.

**Duración estimada:** Meses 4-12 post-MVP.

**Acciones:**

1. **SEO + Blog + YouTube** (detallado en sección 5).
2. **Partnerships con agencias web** (detallado en sección 6).
3. **Presencia en comunidades** (detallado en sección 5.3).
4. **Plugin gratuito en WordPress.org** como canal de adquisición (sección 5.4).

---

### Fase 3 — Escalado (Coste: variable)

**Objetivo:** Escalar la adquisición de clientes y expandir a otros ERPs.

**Duración estimada:** Año 2+.

**Acciones:**

1. Segundo conector (ej. Sage 50, Holded, a3ERP) para ampliar el mercado.
2. Programa de afiliados / partners con agencias.
3. Publicidad de pago en Google Ads (keywords de alta intención, bajo volumen).
4. Presencia en ferias sectoriales (eShow, Ecommerce Congress España).
5. Evolución a SL cuando los ingresos lo justifiquen (>€40.000 ARR).

---

## 5. Marketing con Presupuesto Cero

### 5.1 SEO — Artículos de alta intención

El SEO B2B funciona atacando búsquedas específicas de personas con un problema concreto, no keywords genéricas.

**Artículos prioritarios a crear:**

| Artículo | Keyword objetivo |
|---|---|
| *"Cómo sincronizar Factusol con WooCommerce automáticamente paso a paso"* | `sincronizar factusol woocommerce` |
| *"Solución: error de stock desactualizado entre Factusol y tienda online"* | `error stock factusol tienda online` |
| *"Conectar Factusol con WordPress sin programar"* | `conectar factusol wordpress` |
| *"Alternativa a FS2PS / Festeweb para WooCommerce"* | `alternativa festeweb woocommerce` |
| *"Cómo automatizar pedidos de WooCommerce en Factusol"* | `automatizar pedidos woocommerce factusol` |
| *"Factusol y ecommerce: guía completa de integración"* | `factusol ecommerce integración` |

**Formato:** Artículos de 1.500-2.500 palabras con capturas de pantalla, diagramas y CTA claro al final.

**Dónde publicar:** Blog propio de ERP Bridge (subdominio o sección de la web).

### 5.2 YouTube — El canal nº1 de descubrimiento B2B en España

Las PYMEs españolas buscan tutoriales en YouTube antes que en Google para aprender software.

**Vídeos prioritarios:**

1. *"Tutorial: Cómo conectar Factusol con WooCommerce en menos de 5 minutos"*
2. *"Demo: Sincronización automática de productos y stock Factusol ↔ WooCommerce"*
3. *"Cómo dejar de copiar pedidos a mano de tu tienda online a Factusol"*
4. *"ERP Bridge vs Festeweb vs Scripts manuales — Comparativa honesta"*

**Herramientas (gratuitas):** OBS Studio para grabar pantalla, DaVinci Resolve para editar, Canva para miniaturas.

### 5.3 Comunidades — Participación genuina

**No hacer spam.** Participar resolviendo problemas de otros usuarios y mencionar ERP Bridge solo cuando sea la solución natural.

| Comunidad | URL / Acceso | Estrategia |
|---|---|---|
| **WordPress España Slack** | `wp-es.slack.com` (registro vía es.wordpress.org) | Resolver dudas técnicas en `#desarrollo` |
| **Forea.org** | `forea.org` | Participar en hilos de Factusol y sincronización |
| **Foros Plan General Contable** | `foros.plangeneralcontable.com` | Hilos sobre Software DELSOL e integración ecommerce |
| **NocodeHackers** | `nocodehackers.es` | Comunidad de automatización española |
| **Telegram WordPress España** | Buscar en Telegram | Grupos activos de WordPress en español |
| **LinkedIn** | Publicaciones propias + interacción | Compartir aprendizajes del desarrollo y casos de uso |

### 5.4 Plugin gratuito en WordPress.org (Canal de adquisición)

Publicar un plugin gratuito y ligero en el repositorio oficial de WordPress:

> **"ERP Bridge — Factusol Connector for WooCommerce"**

**Funcionalidad gratuita del plugin:**
- Verificar conexión con Factusol / Agent.
- Mostrar estado de sincronización.
- Logs básicos de última sincronización.

**Upgrade a planes de pago para:**
- Sincronización automática programada.
- Sync bidireccional completo.
- Dashboard avanzado.
- Reintentos y alertas.

> **Ventaja:** WordPress.org tiene millones de visitas mensuales. Aparecer en búsquedas de "Factusol" dentro del repositorio de plugins es publicidad gratuita y cualificada.

---

## 6. Partnerships B2B — El Canal Multiplicador (Venta sin Puerta Fría)

En lugar de vender a PYMEs finales una por una con un CAC desproporcionado para un ticket de 249€/año, la estrategia de distribución se apoya en los **prescriptores que ya gestionan las tiendas y los servidores de los clientes**:

```text
                  ┌──────────────────────────────────────────────┐
                  │          BENEFICIOS PARA EL PARTNER          │
                  └──────────────────────────────────────────────┘
                                  │
         ┌────────────────────────┴────────────────────────┐
         ▼                                                 ▼
【 1. Margen Directo de Servicio 】            【 2. Ingresos Recurrentes SaaS 】
• El Partner cobra a su cliente:               • Comisión recurrente vitalicia:
  300€ - 800€ por "Setup y Mapeo"                20% - 30% del MRR de ERP Bridge
• 100% de este ingreso es para el Partner.     • Cobro mensual o anual garantizado.
• ERP Bridge se instala en < 20 min.           • Dashboard multitienda de partner.
```

---

### 6.1 Las 4 Categorías de Partners Clave en España

#### A) Agencias de Diseño Web y Desarrollo E-commerce (WooCommerce y PrestaShop)
* **El Dolor:** El cliente les contrata una tienda online pero exige que sincronice con su Factusol físico. Si la agencia presupuesta un desarrollo a medida (3.000€-5.000€), pierde el cliente; si intenta usar un script barato, asume horas de soporte no pagadas.
* **Propuesta:** Cierra el proyecto web hoy. ERP Bridge pone la pasarela probada. La agencia cobra entre 300 € y 800 € a su cliente por la puesta en marcha (100% de margen para ella) y recibe un **25% de comisión recurrente de por vida**.

#### B) Distribuidores Oficiales de Software DELSOL (Canal TeamPartner)
* **El Perfil:** Empresas de informática de proximidad y consultoras homologadas que implantan Factusol en su comarca.
* **Propuesta:** Permite al distribuidor ofrecer a sus clientes conexión con WooCommerce o PrestaShop sin depender de la rígida tienda de DELSOL y sin que sus técnicos tengan que aprender a programar APIs web. El distribuidor factura la configuración local y fideliza al cliente.

#### C) Agentes Digitalizadores del Kit Digital (Acelera Pyme)
* **El Dolor:** En las categorías de *Comercio Electrónico* (bono de 2.000€) y *Gestión de Procesos* (2.000€-6.000€), las bases reguladoras de Red.es **exigen obligatoriamente la integración del e-commerce con el ERP**. Un desarrollo a medida devoraría todo el margen del bono.
* **Propuesta:** ERP Bridge es la herramienta llave en mano para justificar técnicamente la sincronización ante Red.es maximizando el margen de la ayuda.

#### D) Empresas de Mantenimiento Informático y Soporte IT (MSPs)
* **El Perfil:** Administran la infraestructura de hardware, redes y copias de seguridad de las PYMEs.
* **Ventaja de ERP Bridge:** Al operar mediante un **Local Agent con conexión saliente segura TLS (sin abrir puertos entrantes ni requerir IP fija)**, el responsable de IT lo aprueba de inmediato porque no expone la red local a ataques de *ransomware*.

---

### 6.2 Cómo Reclutar a los Primeros 20-30 Partners

1. **Catálogo Oficial de Agentes Digitalizadores (Acelera Pyme):**
   * Filtrar en `acelerapyme.gob.es` por categoría "Comercio Electrónico" y provincia. Acceso público a miles de agencias con CIF, email y persona de contacto.
2. **Comunidades Técnicas E-commerce:**
   * Participación y networking en **WordCamps y Meetups de WordPress/PrestaShop** (Madrid, Barcelona, Valencia, Sevilla, Zaragoza).
3. **Asociaciones Territoriales TIC:**
   * Alianzas y webinars formativos a través de federaciones como **CONETIC**, **AETICAL** (Castilla y León), **AVETIC** (Comunidad Valenciana) y **GAIA** (País Vasco).
4. **Contenido Inbound Técnico para Desarrolladores:**
   * Artículos orientados a programadores de agencias: *"Cómo sincronizar Factusol con WooCommerce sin abrir puertos MySQL"* o *"Guía para justificar la integración ERP en el Kit Digital"*.

---

## 7. Estructura Legal Mínima

### 7.1 Antes de la primera venta (Fase 0)

```text
Coste legal: €0
Requisito: NINGUNO
```

Mientras el producto se ofrece gratuitamente a beta testers, no existe actividad económica lucrativa habitual. No se requiere registro fiscal ni en la Seguridad Social.

### 7.2 Primera venta (Fase 1)

Antes de cobrar el primer euro, registrarse como:

**Autónomo con Tarifa Plana:**

| Concepto | Coste |
|---|---|
| Certificado Digital FNMT | €0 (gratuito) |
| Alta en Hacienda (Modelo 036/037) | €0 |
| Epígrafe IAE | 845 — Explotación electrónica / 769.9 — Servicios informáticos |
| Alta en RETA (Seguridad Social) | €80/mes (Tarifa Plana, 12-24 meses) |
| **Cuota Cero (según CCAA)** | **€0/mes** (Madrid, Andalucía, Murcia, Galicia, Baleares, Canarias y otras reembolsan el 100%) |
| Cuenta bancaria business | €0 (Qonto, Revolut Business, Wise) |
| Stripe conectado al autónomo | €0 (comisión por transacción: 1,4% + €0,25) |

**Obligaciones trimestrales:**
- Modelo 303 (IVA): 21% a clientes españoles. Intra-UE B2B con NIF-IVA válido: 0% (inversión del sujeto pasivo).
- Modelo 130 (IRPF): 20% sobre beneficio neto trimestral.
- Modelo 349 (si hay ventas intra-UE).
- Facturación conforme a VeriFactu.

### 7.3 Escalado (>€40.000 beneficio neto/año)

Cuando el beneficio neto anual supere los €40.000:
- Constituir una **Sociedad Limitada (SL)** vía CIRCE/PAE (capital mínimo: €1 desde Ley Crea y Crece).
- Transferir la propiedad intelectual del software a la SL.
- Contratar gestoría dedicada (€100-200/mes).

### 7.4 Lo que NO funciona

| Opción | Por qué no |
|---|---|
| **No darse de alta** | Riesgo de multa de €3.750 – €12.000 + recargos del 20% |
| **Cooperativa de facturación** | No compatible con SaaS recurrente; bajo auditoría intensiva de la TGSS |
| **LLC americana / OÜ Estonia** | Si resides en España >183 días, la AEAT considera la sede de dirección efectiva en España (CFC rules). Doble contabilidad sin eliminar obligaciones españolas |

---

## 8. Métricas de Go To Market

### Fase 0 (Validación)

| Métrica | Objetivo |
|---|---|
| Empresas contactadas | 30 |
| Respuestas positivas | 10 |
| Beta testers activos | 5 |
| Testimonios obtenidos | 3 |

### Fase 1 (Primeros clientes)

| Métrica | Objetivo |
|---|---|
| Clientes de pago | 10-20 |
| MRR (Monthly Recurring Revenue) | €300 – €600 |
| Churn mensual | < 5% |
| NPS (Net Promoter Score) | > 40 |

### Fase 2 (Tracción)

| Métrica | Objetivo |
|---|---|
| Clientes de pago | 50-100 |
| MRR | €1.500 – €4.000 |
| Partnerships activos | 5-10 agencias |
| Artículos SEO publicados | 10-15 |
| Visitas orgánicas mensuales | 1.000+ |

---

## 9. Timeline Resumido

```text
            PRODUCTO                          COMERCIAL
               │                                  │
    ┌──────────┤                    ┌──────────────┤
    │          │                    │              │
 Milestone 1   │              Identificar 30      │
 (Sync manual) │              empresas target     │
    │          │                    │              │
    ▼          │                    ▼              │
 Milestone 2   │              Contactar y         │
 (Sync auto)   │              ofrecer beta        │
    │          │                    │              │
    ▼          │                    ▼              │
 MVP funcional ├──────────────► 5 beta testers    │
               │                activos            │
               │                    │              │
               │                    ▼              │
               │              Testimonios +        │
               │              caso de éxito        │
               │                    │              │
               │                    ▼              │
               │              Alta autónomo +      │
               │              Stripe               │
               │                    │              │
               │                    ▼              │
               │              Plan Starter         │
               │              activo (€29/mes)     │
               │                    │              │
               │                    ▼              │
               │              SEO + YouTube +      │
               │              Partnerships         │
```

---

## 10. Principio Final

> **ERP Bridge no necesita un presupuesto de marketing para empezar. Necesita resolver un problema real para 5 empresas reales. Todo lo demás vendrá después.**

> **El mejor marketing es un producto que funciona y un cliente que lo recomienda.**

---

## 11. Documentación Operativa Relacionada

- [207 - Tax, Autónomo & Pluriactivity Operations](file:///g:/Otros%20ordenadores/Mi%20PC/Bentian/ERP%20Bridge%20Blueprint/200%20-%20Business/207%20-%20Tax,%20Autonomo%20&%20Pluriactivity%20Operations/207%20-%20Tax,%20Autonomo%20&%20Pluriactivity%20Operations.md): Guía legal y fiscal para alta de autónomo bajo régimen de pluriactividad, IAE, RETA, Tarifa Plana de 80 € y pasarelas de pago.
- [208 - Launch Playbook & Regional Execution](file:///g:/Otros%20ordenadores/Mi%20PC/Bentian/ERP%20Bridge%20Blueprint/200%20-%20Business/208%20-%20Launch%20Playbook%20&%20Regional%20Execution/208%20-%20Launch%20Playbook%20&%20Regional%20Execution.md): Plan de prospección territorial (Comunidad Valenciana y Albacete), directorio de agencias e IT, guiones de contacto y agenda de 5 días adaptada a empleo por cuenta ajena.
