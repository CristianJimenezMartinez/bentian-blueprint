# Architecture Decisions — v1.0
> **Estado:** Borrador

---

# 601 - Architecture Decisions (ADR)

> *Registro de decisiones arquitectónicas fundamentales (Architectural Decision Records).*

---

## 1. Resumen / Definición
Registro cronológico y justificado de las decisiones de arquitectura adoptadas para ERP Bridge.

---

## 2. Decisiones Clave Registradas
- **ADR-001:** Modelo Estrella (*Star Architecture*) con Modelo Canónico central vs Integraciones Punto a Punto (Mesh).
- **ADR-002:** Conexión Saliente Exclusiva (*Outbound-only*) del Agente Local mediante WebSockets seguros (WSS) / HTTPS sin apertura de puertos.
- **ADR-003:** Desacoplamiento total: El Core no contiene lógica específica de conectores (`if (factusol)` prohibido en el Core).
- **ADR-004:** Procesamiento en local (*Edge Processing*) para Factusol/Access: no se transfiere la base de datos completa a la nube, solo los deltas.
- **ADR-005:** Bus de eventos asíncrono con tolerancia a fallos, desduplicación, reintentos con backoff y Dead Letter Queue (DLQ).
- **ADR-006:** Separación Estricta: Centro de Control Local Autónomo (BentianAgent.exe) vs Portal Cloud SaaS de Facturación y Licencias Multi-Puesto.
- **ADR-007:** Prohibición de Conexiones Remotas a Bases de Datos (Puerto 3306) en favor de Micro-Puentes HTTPS (Puerto 443).
- **ADR-008:** Arquitectura Híbrida Tri-Pilar (Agent-First Onboarding, Puente SSO 1-Clic a Stripe y Portal Cloud Passwordless).
- **ADR-009:** Extracción de Identificador de Hardware (HWID) mediante PowerShell CIM Nativo para Windows 11 24H2 sin dependencias de `wmic`.
- **ADR-010:** Actualización Silenciosa y Atómica del Agente con Firmas Digitales Ed25519 y Auto-Rollback NTFS.
- **ADR-011:** Licenciamiento Offline con Período de Gracia de 7 Días (168 horas) y Almacenamiento Cifrado AES-256-GCM.

---

## 3. ADR-006 Detallado: Centro de Control Local vs Cloud SaaS

### Contexto
El usuario final de Factusol trabaja en ordenadores Windows on-premise. Forzar la configuración de Factusol, rutas `.accdb`, tarifas y credenciales de WooCommerce a través de un panel cloud central introducía una dependencia innecesaria de conectividad a Internet, mayor superficie de ataque para credenciales de tienda y una experiencia fragmentada donde la aplicación local parecía "pobre" al ser un mero visor de estado.

### Decisión
1. **Centro de Control Local (`BentianAgent.exe`):** Se convierte en el centro de control completo de la integración. Gestiona localmente la conexión a Factusol, visualización en vivo de artículos (`F_ART`), tarifas (`F_TAR`), almacenes (`F_ALM`), credenciales y test de conexión con WooCommerce, vigilante de archivos Access en tiempo real (`AccdbFileWatcher`), reglas de sincronización y stock, historial de ejecuciones, DLQ y exportación de informes técnicos de diagnóstico.
2. **Dashboard Cloud SaaS (`bridge.cristianjm.com/dashboard`):** Se simplifica para centrarse exclusivamente en la administración de la cuenta comercial del cliente:
   - Facturación y cobros automáticos mediante Stripe.
   - Gestión multi-clave de licencias (`EB-XXXXX`), cupos por puesto (*seats*), asignación de alias descriptivos a cada puesto físico y revocación / desvinculación de hardware (HWID) para transferir licencias a nuevos equipos.
   - Monitorización agregada de flota (estado *online/offline* y versión de cada agente del cliente).
3. **Seguridad y Criptografía:** La lógica de generación y activación de licencias se ejecuta de forma hermética en el backend central contra PostgreSQL/Supabase. Los agentes locales consumen únicamente los endpoints criptográficos `/activate` y `/validate`.

### Consecuencias
- **Positivas:** Máxima autonomía y resiliencia offline del cliente. Ninguna credencial de Factusol ni contraseñas de red viajan a la nube. Experiencia de escritorio rica y completa para el administrativo o responsable de almacén.
- **Negativas:** La interfaz local requiere mantener una suite visual completa servida en loopback local. Mitigado mediante el micro-servidor HTTP embebido en el binario sin dependencias externas.

---

## 4. ADR-007 Detallado: Prohibición de Conexiones Remotas a Bases de Datos (Puerto 3306) en favor de Micro-Puentes HTTPS (Puerto 443)

### Contexto
Muchas PYMEs cuentan con desarrollos propios, SPAs en Angular/React o webs sobre WordPress/PHP sin WooCommerce alojadas en servidores gestionados (Plesk / cPanel). En el pasado, herramientas antiguas (como WebLinker de SimplyGest) intentaban conectar el PC local directamente contra el puerto **3306 (MySQL/MariaDB)** del hosting remoto a través de Internet.

Esta práctica genera fallos sistemáticos:
1. **Bloqueo de Puertos:** El 98% de los hostings españoles (Raiola, Dinahosting, Arsys, Ionos, OVH) bloquean el puerto 3306 por defecto.
2. **IPs Dinámicas:** Las PYMEs operan con IPs dinámicas o CG-NAT en sus routers de fibra, haciendo imposible mantener listas blancas de IP. Abrir el puerto a `0.0.0.0/0` expone la base de datos a ataques de fuerza bruta que saturan `max_connections` y tumban la web de producción.
3. **Fallo Crítico de SSL:** Plesk y cPanel gestionan Let's Encrypt para Nginx/Apache (puerto 443), pero no para MariaDB en el puerto 3306. Forzar certificados en MariaDB provoca que cada 60-90 días el servicio crashee al renovar (`Permission Denied` entre `root:root` 0600 y `mysql:mysql`) o arroje errores de CA (`DEPTH_ZERO_SELF_SIGNED_CERT`) en Windows.
4. **Infierno de Mantenimiento:** El desarrollador se convierte en DBA no remunerado, gestionando migraciones manuales, bloqueos de tablas y discrepancias de codificación (`utf8mb4` vs `Windows-1252`).

### Decisión
1. **Prohibición Absoluta del Puerto 3306:** Se prohíbe explícitamente cualquier conexión directa desde el Agente Local a bases de datos remotas por el puerto 3306. El servicio MariaDB/MySQL en el hosting del cliente debe permanecer escuchando únicamente en local (`127.0.0.1` / socket Unix interno).
2. **Adopción del Patrón "Micro-Puente HTTPS" (Puerto 443):** Para canales sin APIs preconstruidas (Webs a medida, Angular, WordPress custom, Plesk/cPanel), el Agente se comunica exclusivamente mediante llamadas salientes HTTPS autenticadas contra un micro-script PHP autocontenido (`erp-bridge-endpoint.php` < 8 KB) alojado en el propio servidor web del cliente.
3. **Protocolo y Seguridad:** Todas las peticiones viajan bajo TLS 1.3 con certificados Let's Encrypt nativos del servidor web, firmadas criptográficamente con HMAC-SHA256, cabecera de timestamp contra ataques de repetición y tokens Bearer.
4. **Auto-Migración Idempotente:** El micro-script gestiona sus propias tablas (`eb_products`, `eb_stock`, `eb_orders`) y auto-migra el esquema en tiempo de ejecución sin intervención manual de DBAs.

### Consecuencias
- **Positivas:** Cero puertos abiertos en el router de la PYME y cero puertos en el hosting. Certificados SSL 100% automáticos sin mantenimiento. Inmunidad frente a ataques de fuerza bruta al motor SQL. Compatibilidad universal con cualquier web (Angular, WordPress, PHP) y coste 0 € en infraestructura central.
- **Negativas:** Requiere depositar un archivo PHP en el hosting del cliente (proceso de 30 segundos vía FTP o Plesk File Manager). Mitigado proporcionando el script listo para usar y empaquetado en el SDK.

---

## 5. ADR-008 Detallado: Arquitectura Híbrida Tri-Pilar (Agent-First, SSO 1-Clic y Respaldo Passwordless)

### Contexto
Obligar al usuario de una PYME a entrar a un portal web externo, crear contraseñas, copiar claves alfanuméricas de 25 caracteres (`EB-XXXXX`) y pegarlas en múltiples máquinas genera alta tasa de abandono y fricción. Por otro lado, restringir la gestión exclusivamente al PC local deja al cliente vulnerable ante averías físicas (disco quemado) o impide a directores y asesores contables acceder a facturas de Stripe desde el móvil o Mac.

### Decisión
1. **Pilar 1 (Agent-First Onboarding):** El cliente activa cualquier equipo local introduciendo únicamente su **Email de compra** y un **PIN de 6 dígitos**. El Agente consulta la nube de Bentian, detecta automáticamente la suscripción y muestra en pantalla la lista de puestos disponibles (hasta 3 por empresa). El usuario asigna el equipo con un solo clic. Si un equipo se averió, el nuevo PC ofrece *"Reemplazar máquina inactiva"* con confirmación por email.
2. **Pilar 2 (Puente SSO de 1 Clic sin Contraseñas):** El menú del Agente y el icono de System Tray incorporan el acceso directo `[ 💳 Mi Cuenta y Facturas ↗ ]`. El Agente genera un ticket efímero firmado (HMAC-SHA256, validez de 60 segundos) y abre el navegador por defecto del sistema directamente en el **Stripe Customer Portal**. El cliente descarga facturas y gestiona tarjetas sin recordar contraseñas.
3. **Pilar 3 (Portal Web Passwordless de Emergencia):** Acceso al panel web cloud para consultas remotas mediante **Magic Link** enviado al correo del titular de la cuenta.
4. **Principio Rector: Zero-Knowledge:** Los servidores de Bentian **NUNCA almacenan datos de negocio, artículos, precios ni pedidos de clientes**. Toda la sincronización se procesa en el borde local (*Edge Processing*) directamente entre el Agente y la tienda online.

---

## 6. ADR-009 Detallado: Identificador de Hardware (HWID) mediante PowerShell CIM Nativo

### Contexto
Microsoft ha deprecado y eliminado definitivamente `wmic.exe` a partir de Windows 11 24H2 y Windows Server 2025. Los scripts de extracción de HWID basados en `wmic` fallan catastróficamente en las instalaciones modernas de Windows.

### Decisión
El cálculo del HWID en `HWIDManager` migra obligatoriamente a comandos nativos de PowerShell CIM:
`powershell.exe -NoProfile -NonInteractive -Command "try { (Get-CimInstance Win32_DiskDrive | Select-Object -ExpandProperty SerialNumber -First 1).Trim() } catch { (Get-CimInstance Win32_BaseBoard | Select-Object -ExpandProperty SerialNumber -First 1).Trim() }"`
Se combina con `MachineGuid` del registro de Windows y dirección MAC de la interfaz de red principal, generando un hash SHA-256 único e inmutable.

---

## 7. ADR-010 Detallado: Auto-Actualización Atómica del Agente con Firmas Ed25519

### Contexto
Actualizar un ejecutable en Windows mientras está en ejecución es bloqueado por el kernel (`ERROR_SHARING_VIOLATION`). Además, la distribución de binarios sin verificación criptográfica expone a los clientes a ataques de sustitución maliciosa.

### Decisión
1. **Firma Asimétrica Ed25519:** Cada manifiesto de versión en `/api/v1/updates/check` incluye hash SHA-256 y firma criptográfica digital Ed25519. El Agente verifica la firma contra la clave pública integrada antes de escribir en disco.
2. **Reemplazo Atómico NTFS:** El ejecutable actual se renombra a `.old` (permitido por Windows) y el nuevo binario ocupa su lugar. Un proceso secundario lanza el nuevo binario.
3. **Supervisión y Rollback Automático:** El nuevo ejecutable debe reportar salud en los primeros 10 segundos. Si crashea o no arranca, un script recupera automáticamente la copia previa desde `.bak`.

---

## 8. ADR-011 Detallado: Licenciamiento Offline con Período de Gracia de 7 Días (168h)

### Contexto
Cortes temporales de fibra óptica o problemas en el router de la PYME no deben paralizar la facturación ni la sincronización en la tienda del cliente.

### Decisión
1. La validación exitosa de licencia emite un License Token firmado válido por **7 días (168 horas)**.
2. Si el Agente no puede conectar con la API de validación diaria, transiciona automáticamente al estado `GRACE_PERIOD` y continúa sincronizando localmente sin interrupciones durante 168 horas continuas.
3. El token se almacena en `%APPDATA%/erp-bridge/license.enc` cifrado con **AES-256-GCM** derivado del HWID. Si se copia a otro ordenador, no puede desencriptarse.

