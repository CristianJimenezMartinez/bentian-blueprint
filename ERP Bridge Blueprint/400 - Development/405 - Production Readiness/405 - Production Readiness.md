# Production Readiness — v1.0
> **Estado:** Aprobado

---

# 405 - Production Readiness

> *Matriz de riesgos en producción, mitigaciones operativas para Solo Founder y lista de verificación antes del despliegue comercial.*

---

## 1. Propósito

Definir los criterios de calidad, mitigación de riesgos operativos y preparación técnica necesarios para llevar ERP Bridge a clientes reales minimizando el soporte manual y garantizando cero fricción.

---

## 2. Principio Rector

Un Solo Founder no puede sostener un producto que requiera soporte técnico manual recurrente.
Todo componente que interactúe con el entorno del cliente debe ser:
- Auto-recuperable ante caídas de red o reinicios.
- Resiliente ante bloqueos de archivos en red local (`.laccdb`).
- Capaz de auto-actualizarse en segundo plano sin intervención humana.
- Capaz de operar temporalmente sin conexión a Internet (gracia offline).

---

## 3. Matriz de Riesgos y Mitigaciones en Producción

### R1. URL del Servidor API en Entornos de Cliente
- **Riesgo:** Si el binario cliente recurre a valores por defecto locales (`localhost:3000`), el agente no puede comunicarse con la nube.
- **Mitigación:** El instalador Inno Setup deposita un `agent-config.json` inicial con la URL de producción (`https://bridge.cristianjm.com`). Además, el ejecutable ofrece comandos directos (`BentianAgent.exe set-api <URL>`) y soporte de variables de entorno (`CORE_API_URL`).

### R2. Ubicación de Factusol en Servidores o Redes Locales
- **Riesgo:** Factusol instalado en unidades de red (`Z:\`, `\\SERVIDOR\Datos\FS`) que escapan al escaneo de la unidad C: estándar.
- **Mitigación:**
  1. Escaneo ampliado incluyendo subdirectorios `/FS/` y unidades montadas.
  2. Comando CLI `BentianAgent.exe set-db [RUTA]` que, si se ejecuta sin argumentos, abre un cuadro de diálogo gráfico nativo de Windows (`OpenFileDialog`) para que el usuario seleccione el archivo `.accdb` visualmente.
  3. Selección automática del ejercicio contable más reciente (año mayor) si existen múltiples ficheros de la misma empresa.

### R3. Compatibilidad de Arquitectura OLEDB (32-bit vs 64-bit)
- **Riesgo:** Factusol es típicamente de 32 bits, mientras que el sistema operativo Windows del cliente es de 64 bits. Si el host de scripting intenta abrir el controlador OLEDB con la arquitectura incorrecta, Windows arroja error de proveedor no registrado.
- **Mitigación:** El ejecutor de scripts OLEDB (`adodb.js`) detecta la presencia del motor ACE de 32 bits y ejecuta el proceso a través de `C:\Windows\SysWOW64\cscript.exe` con fallback dinámico a `System32\cscript.exe`.

### R4. Bloqueos de Archivo por Concurrencia en Factusol
- **Riesgo:** Bloqueos temporales del archivo Access cuando varios usuarios introducen albaranes o facturas simultáneamente.
- **Mitigación:** Motor de reintentos con *exponential backoff* y fluctuación aleatoria (*jitter*), evitando saturar el disco y registrando fallos definitivos en la Dead-Letter Queue para reintento automático posterior.

### R5. Seguridad y Autenticación del Servidor API
- **Riesgo:** Exposición pública de endpoints administrativos (`/licenses`, `/organizations`, `/agents`) sin control de acceso.
- **Mitigación:** Autenticación obligatoria mediante JWT para acceso al Dashboard administrativo vía `/api/v1/auth/login` con contraseñas hasheadas (`bcrypt`/`scrypt`), manteniendo los endpoints públicos debidamente asegurados (webhook de Stripe con firma criptográfica y agentes locales con tokens HWID y firmas Ed25519).

---

## 4. Checklist Técnico Pre-Venta

- [x] Conectores de Factusol y WooCommerce validados con datos reales (7 flujos E2E).
- [x] Empaquetador nativo Windows (`BentianAgent.exe`) e instalador Inno Setup (`Bentian-Setup.exe`).
- [x] Licenciamiento ligado a webhooks de Stripe con almacenamiento cifrado DPAPI y gracia de 7 días.
- [x] Sistema de auto-actualización autónomo en Windows con firma Ed25519 y swap atómico `.old`.
- [x] Soporte de base de datos cloud gestionada en Supabase con SSL y pooler IPv4/IPv6.
- [x] Configuración de URL pública predeterminada en el instalador y comando `set-api`.
- [x] Selector interactivo y comando `set-db` para rutas manuales o de red de Factusol.
- [x] Selector dinámico de 32/64 bits para el driver OLEDB.
- [x] Autenticación de administración en la API con endpoint `/api/v1/auth/login`.
