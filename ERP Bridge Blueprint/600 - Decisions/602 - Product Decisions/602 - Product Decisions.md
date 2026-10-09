# Product Decisions — v1.0
> **Estado:** Borrador

---

# 602 - Product Decisions

> *Registro de decisiones de producto, alcance, experiencia de usuario y priorización.*

---

## 1. Resumen / Definición
Registro de decisiones de producto y diseño funcional de ERP Bridge.

---

## 2. Decisiones de Producto Clave
- **PDR-001:** El software se adapta a la empresa, nunca la empresa a ERP Bridge.
- **PDR-002:** No sustituimos el ERP del cliente; conectamos y potenciamos el que ya utiliza.
- **PDR-003:** Experiencia Zero-Code para usuarios empresariales (configuración completa guiada en ~3 minutos).
- **PDR-004:** Separación de experiencias: Modo Simple (No-Code) para gerencia/administración vs Modo Avanzado para perfiles técnicos.
- **PDR-005:** Foco MVP estricto: Integración bidireccional de Factusol ↔ WooCommerce (validar con una empresa real antes de expandir).
- **PDR-006:** El Marketplace no es el producto inicial; es la fase de expansión futura del ecosistema.
- **PDR-007:** Paradigma First-Local: El software instalado localmente es el Centro de Control Completo de la integración.
- **PDR-008:** Modelo Comercial Flat-Rate: Plan Único Todo Incluido (249 €/año promo 199 €, 29 €/mes) con 3 puestos y catálogo ilimitado sin recargos por volumen.
- **PDR-009:** Licenciamiento Multi-Puesto y Autogestión de Flota (Self-Service Unbinding para sustitución de equipos sin tickets manuales).
- **PDR-010:** Programa de Partners Prescriptores con Comisión del 25% Trimestral y Liquidación Manual sin sobreingeniería.

---

## 3. PDR-007 Detallado: Paradigma First-Local para Usuarios de ERPs de Escritorio

### Contexto
El usuario tipo de Factusol en España es una pyme de suministros, fontanería, distribución o comercio minorista. El personal administrativo y de almacén utiliza un ordenador con Windows, no está familiarizado con arquitecturas cloud ni paneles de control remotos que requieran login con contraseña para ver si su propio ERP está sincronizando. Exigirles navegar a una web remota para configurar parámetros locales creaba frustración y desconfianza.

### Decisión
1. **La aplicación de escritorio (`BentianAgent.exe`) contiene TODO:**
   - Explorador de artículos y stock en vivo sin necesidad de entrar a Factusol.
   - Selector visual de bases de datos con explorador nativo de Windows (`OpenFileDialog`) y botón de auto-detección.
   - Test en vivo con cálculo de latencia tanto para Factusol (OLEDB) como para WooCommerce (REST API).
   - Selector directo de tarifas, almacenes y mapeos de estados de pedido.
   - Descarga de diagnósticos en 1 clic para soporte técnico sin intervención del usuario.
2. **El Dashboard Cloud es un portal de cliente para licencias y pagos:**
   - Pensado para el dueño o director de compras: gestionar suscripciones Stripe y claves de licencia para múltiples sedes.
3. **Privacidad Sagrada (Zero-Knowledge):** La nube de Bentian nunca recibe ni almacena productos, precios, pedidos ni clientes. Toda la sincronización viaja cifrada punto a punto entre la máquina del cliente y su tienda online.

### Consecuencias
- **Adopción inmediata:** El usuario abre el programa en su ordenador, ve su stock real inmediatamente y tiene el control total en local con retroalimentación instantánea.

---

## 4. PDR-008 Detallado: Modelo Comercial Flat-Rate y Cero Límites Artificiales

### Contexto
Los competidores tradicionales (Conecta HUB, Festeweb, etc.) imponen escalones de precio arbitrarios y castigan el crecimiento del cliente según el número de referencias (ej. 2.000 vs 10.000 SKUs) o pedidos mensuales. Dado que Bentian procesa todo en la máquina local del cliente y el coste de servidor en la nube es virtualmente nulo (~4,50 €/mes fijo), estos límites resultan artificiales y generan fricción comercial.

### Decisión
1. **Plan Base Todo Incluido:**
   - **Precio Anual:** 249 €/año (Tarifa promocional de lanzamiento: **199 €/año** + IVA).
   - **Precio Mensual:** **29 €/mes** + IVA sin permanencia.
2. **Sin Límites de Volumen:** Catálogo de productos, pedidos y clientes **100% ilimitados**.
3. **Puestos de Trabajo:** Incluye de serie **hasta 3 puestos locales de trabajo** bajo la misma empresa (ej. Mostrador, Almacén, Despacho).
4. **Prueba Completa de 14 Días:** Gratuita y sin solicitar tarjeta de crédito.

---

## 5. PDR-009 Detallado: Licenciamiento Multi-Puesto y Autogestión de Flota (Self-Service Unbinding)

### Contexto
Cuando un ordenador de tienda se avería (disco duro roto o sustitución de PC), el cliente no puede quedar bloqueado esperando a que el soporte técnico libere la licencia en base de datos. Tampoco es viable forzar al usuario a copiar y pegar claves alfanuméricas de 25 caracteres en cada máquina.

### Decisión
1. **Onboarding Asistido Agent-First:** El cliente activa cualquier equipo introduciendo su Email de compra y un PIN de 6 dígitos. El Agente consulta la nube y le permite elegir qué puesto asignar con 1 solo clic.
2. **Autogestión de Reemplazo ("Mudar PC"):** Si el ordenador antiguo murió, el nuevo equipo permite seleccionar el puesto ocupado y pulsar *"Reemplazar máquina inactiva"*, enviando un código de confirmación al email del titular para autorizar el relevo de HWID.
3. **Puente Web SSO 1-Clic:** El botón *"Mi Cuenta y Facturación"* en el Agente abre Microsoft Edge autenticado automáticamente en el Stripe Customer Portal mediante un token efímero de 60 segundos, sin pedir contraseñas.

---

## 6. PDR-010 Detallado: Programa de Partners Prescriptores y Liquidación Manual Trimestral

### Contexto
Las agencias de diseño web, consultores informáticos locales y mantenedores de Factusol son los mayores prescriptores de software de sincronización para PYMEs. Desarrollar una plataforma automatizada de payouts complejos (Stripe Connect) en la fase de validación añade una sobrecarga regulatoria y técnica innecesaria.

### Decisión
1. **Comisión Recurrente:** **25% trimestral** sobre las suscripciones activas referidas por el partner durante toda la vida del cliente.
2. **Operativa Pragmática para los Primeros 5-10 Partners:**
   - Asignación de código de partner en el checkout de Stripe.
   - Al cierre de cada trimestre natural, se envía al partner un informe de conciliación con sus licencias activas.
   - El partner emite su factura por el 25% devengado y se liquida mediante transferencia bancaria estándar en 5 días hábiles.
