# Security — v1.0
> **Estado:** Borrador

---

# 308 - Security

> *Principios de seguridad, Zero Trust, modelo de permisos, cifrado y aislamiento multi-tenant.*

---

## 1. Propósito

Definir los principios y mecanismos de seguridad necesarios para proteger ERP Bridge, sus usuarios, sus conexiones, los sistemas de los clientes y los datos que atraviesan la plataforma.

La seguridad deberá estar presente en:

Usuario
```text
   ↓
```
Dashboard
```text
   ↓
```
API
```text
   ↓
```
Core
```text
   ↓
```
Agent
```text
   ↓
```
Connector
```text
   ↓
```
Sistema del cliente

---

## 2. Principio fundamental

ERP Bridge debe tener el mínimo acceso necesario para realizar cada operación.

No queremos:

ERP Bridge
```text
   ↓
```
Acceso completo a toda la empresa

Queremos:

ERP Bridge
```text
   ↓
```
Permisos específicos
```text
   ↓
```
Operaciones autorizadas

---

## 3. Zero Trust

La arquitectura seguirá un principio de Zero Trust.

Ningún componente deberá considerarse automáticamente confiable.

Usuario
```text
   ↓
```
¿Quién eres?
```text
   ↓
```
¿Qué puedes hacer?
```text
   ↓
```
¿Desde dónde?
```text
   ↓
```
¿Sobre qué recurso?
```text
   ↓
```
Autorizar

Esto se aplicará también entre:

Dashboard.
API.
Core.
Agent.
Connectors.

---

## 4. Identidad

Cada elemento importante deberá tener una identidad.

Usuario
User ID
Organización
Organization ID
Agent
Agent ID
Connection
Connection ID
API Key
Key ID

Esto permite controlar y auditar cada operación.

---

## 5. Authentication

ERP Bridge implementa una arquitectura de seguridad moderna **Passwordless y Zero-Knowledge**:

1. **Cero Almacenamiento de Datos de Clientes en la Nube (Zero-Knowledge):**
   * Los servidores de Bentian **NUNCA almacenan, procesan ni copian** catálogos de artículos, precios, pedidos, clientes ni datos contables de Factusol.
   * La sincronización se realiza en el extremo local (*Edge Processing*) directamente entre el PC del cliente y su tienda online (WooCommerce / PrestaShop) vía HTTPS.
   * Las credenciales de la tienda (Consumer Secret) se guardan cifradas únicamente en el equipo local mediante AES-256-GCM.

2. **Autenticación Tri-Pilar sin Contraseñas (Passwordless):**
   * **Pilar 1 (Agent-First):** Activación directa en la aplicación de escritorio mediante Email de compra + PIN de un solo uso (OTP).
   * **Pilar 2 (SSO de 1-Clic):** Acceso seguro a la facturación y gestión de suscripción en Stripe Customer Portal mediante tickets efímeros firmados (HMAC-SHA256, TTL 60s) lanzados desde el Agente.
   * **Pilar 3 (Portal de Respaldo):** Acceso al panel web de flota mediante Magic Links temporales enviados al correo del titular. Cero contraseñas que almacenar o que puedan filtrarse.

---

## 6. Authorization

Autenticarse no significa tener permiso para hacerlo todo.

Ejemplo:

Usuario
```text
   ↓
```
Autenticado ✓
```text
   ↓
```
¿Puede eliminar conexión?
```text
   ↓
```
No ✗

Los permisos deberán evaluarse en cada operación sensible.

---

## 7. Roles

Inicialmente podremos utilizar roles como:

Owner
Admin
Operator
Viewer
Developer

Ejemplo:

Owner

Acceso completo.

Admin

Gestiona configuración y usuarios.

Operator

Gestiona operaciones e integraciones.

Viewer

Solo lectura.

Developer

Acceso técnico a API y herramientas.

Los permisos exactos se definirán durante la implementación.

---

## 8. Multi-tenancy

ERP Bridge deberá aislar completamente las empresas.

Ejemplo:

Organization A
```text
├── Users
├── Connections
├── Agents
└── Data
```


Organization B
```text
├── Users
├── Connections
├── Agents
└── Data
```

Nunca:

Organization A
```text
        ↓
```
Datos de B ✗

---

## 9. Credentials

ERP Bridge tendrá que manejar credenciales de terceros.

Ejemplos:

API Keys.
Tokens OAuth.
Contraseñas.
Credenciales de bases de datos.
Certificados.

Estas credenciales son información altamente sensible.

---

## 10. Secrets

Las credenciales nunca deberán almacenarse:

en código,
en Git,
en logs,
en URLs,
en texto plano innecesariamente.

Conceptualmente:

Connector
```text
    ↓
```
Secrets Manager
```text
    ↓
```
Encrypted Credential

---

## 11. Encryption

Los datos sensibles deberán cifrarse:

En tránsito
Client
```text
 ↓ HTTPS/TLS
```
ERP Bridge
En reposo

Credenciales y otros secretos deberán almacenarse cifrados.

---

## 12. Agent Security

El Agent es especialmente sensible porque está instalado dentro de la empresa.

Por tanto:

Un Agent comprometido no debe permitir automáticamente controlar todo el ERP Bridge.

Debe existir:

identidad única,
autenticación,
autorización,
revocación,
actualización segura.

---

## 13. Vinculación Segura del Agente (Agent-First Pairing)

La instalación y vinculación no requiere generar códigos manuales en la nube, sino que opera directamente desde el Agente:

```text
Flujo de Vinculación Criptográfica:

1. Usuario introduce: [Email de compra] + [PIN temporal de 6 dígitos]
2. El Agente calcula el HWID local mediante PowerShell CIM nativo:
   Get-CimInstance Win32_DiskDrive + Win32_BaseBoard + MachineGuid
3. Petición saliente HTTPS al Core (POST /api/v1/licenses/activate):
   - Valida PIN y cupo de puestos disponibles (máx. 3 puestos por empresa)
   - Asocia el HWID en la base de datos de licencias
   - Emite un License Token firmado válido por 7 días (168 horas)
4. El token se almacena cifrado en el equipo local con AES-256-GCM
   (clave derivada del HWID mediante PBKDF2). Imposible de clonar a otro PC.
```

Una vez vinculado, el PIN se invalida y el equipo queda autorizado de forma autónoma.

---

## 14. Agent Revocation

Si un ordenador se pierde o se sospecha que está comprometido:

Dashboard
```text
   ↓
```
Agents
```text
   ↓
```
Office PC
```text
   ↓
```
Revoke

El Agent deberá dejar de poder comunicarse con ERP Bridge.

---

## 15. Network Security

El diseño deberá evitar exponer innecesariamente servicios locales.

Preferencia:

Agent
```text
   │
   │ outbound connection
   ↓
```
ERP Bridge

En lugar de:

Internet
```text
   │
   ↓
```
Puerto abierto
```text
   ↓
```
ERP local

Esto simplifica el despliegue y reduce superficie de ataque.

---

## 16. Connector Permissions

Un Connector no deberá tener acceso ilimitado.

Ejemplo:

Factusol Connector


✓ Read Products
✓ Read Stock
✓ Create Orders


✗ Read Users
✗ Access other Connections
✗ Access billing

Esto permite aplicar least privilege.

---

## 17. API Security

La API deberá incluir:

autenticación,
autorización,
rate limiting,
validación de entradas,
protección contra abuso,
logs,
request IDs.

Las operaciones críticas deberán tener controles adicionales.

---

## 18. API Keys

Las API Keys deberán:

ser únicas,
poder revocarse,
poder rotarse,
tener permisos,
tener alcance limitado.

Nunca se deberá mostrar una clave completa después de su creación si el modelo de seguridad no lo permite.

---

## 19. Webhooks

Los webhooks también necesitan seguridad.

ERP Bridge deberá poder verificar que un webhook procede realmente del proveedor esperado.

Dependiendo del sistema:

firmas HMAC,
tokens,
autenticación,
IP allowlisting cuando sea apropiado.

---

## 20. Input Validation

Todo dato externo deberá considerarse potencialmente malicioso.

API
```text
 ↓
```
Validate
```text
 ↓
```
Sanitize
```text
 ↓
```
Process

Esto incluye:

parámetros,
JSON,
archivos,
IDs,
expresiones,
configuraciones.

---

## 21. SQL / Database Security

Los conectores que interactúen con bases de datos deberán utilizar mecanismos seguros.

Especialmente el conector de Factusol/Access.

No se deberán construir consultas mediante concatenación insegura de datos.

---

## 22. Logs

Los logs deberán ser útiles sin convertirse en una fuga de información.

Incorrecto:

password=123456
apiKey=sk_abc...

Correcto:

Authentication failed
Connection: conn_123
Request: req_456

---

## 23. Audit Log

ERP Bridge deberá registrar operaciones importantes.

Ejemplo:

18/08/2026 22:45


User:
admin@empresa.com


Action:
Connection modified


Connection:
Factusol


Result:
Success

Esto permitirá investigar incidentes.

---

## 24. Arquitectura Zero-Knowledge y Minimización Radical

Los servidores centrales de Bentian **NUNCA almacenan datos de negocio de los clientes**.

Principio Rector:
- Procesamiento en el Extremo (*Edge Processing*): los datos de artículos, precios, inventario, clientes y pedidos viajan directamente y de forma exclusiva punto a punto entre la máquina del cliente y su tienda online.
- La infraestructura cloud de Bentian únicamente procesa metadatos técnicos mínimos e indispensables: validación de licencias, comprobación criptográfica de versiones y latidos (*heartbeat*) de salud operativa sin cargas útiles de negocio.

---

## 25. Procesamiento Local en el Extremo (Edge Processing)

Los datos de negocio permanecen permanentemente bajo el control exclusivo y soberano de la empresa.

Flujo de Datos Directo Punto a Punto:
```text
Factusol ERP (.accdb)
    ↓  (Lectura/Escritura local OLEDB)
BentianAgent.exe (Edge Processing)
    ↓  (HTTPS cifrado directo TLS 1.3)
Canal Web / Tienda (WooCommerce / PrestaShop / Web)
```

Los servidores cloud de Bentian **NUNCA actúan como intermediarios ni almacenan copia alguna** de los catálogos, pedidos, clientes o bases de datos de las empresas integradas.

---

## 26. GDPR / RGPD por Diseño

Al no almacenar datos de carácter personal de los clientes finales de las tiendas en la nube de Bentian (ya que los pedidos y NIFs viajan directamente del ecommerce al Factusol local sin pasar por los servidores de Bentian):
- Se reduce a cero el riesgo de fuga masiva de datos en la nube.
- Bentian no actúa como custodio ni encargado de almacenamiento de datos de compras, clientes ni facturación.
- Máximo cumplimiento de los principios de minimización de datos (*Data Minimization*), privacidad desde el diseño (*Privacy by Design*) y limitación del plazo de conservación del RGPD.

---

## 27. Política de Backups y Ausencia de Custodia de Datos

Bentian **NO almacena, no custodia ni realiza copias de seguridad de las bases de datos de los clientes**.

Política de Separación:
- **Base de Datos del ERP (Factusol):** Es propiedad y responsabilidad exclusiva del cliente y sus políticas de copia de seguridad interna (copias locales, NAS o backups de Windows).
- **Configuración Operativa Local:** Reside localmente en `%APPDATA%/erp-bridge/agent-config.json` cifrada con AES-256-GCM.
- **Servidores Bentian Cloud:** Únicamente respaldan los registros de suscripciones de Stripe, claves de licencia (`EB-XXXXX`), hashes de HWID y cuentas de usuario administrativo.


---

## 28. Disaster Recovery

ERP Bridge deberá tener mecanismos para recuperarse de:

fallo de servidor,
pérdida de base de datos,
fallo de despliegue,
corrupción de datos,
caída de servicios externos.

El nivel de disponibilidad dependerá del plan contratado.

---

## 29. Updates

El Core, Agent y Connectors deberán poder actualizarse de forma controlada.

Especialmente el Agent:

Nueva versión
```text
     ↓
```
Verificación
```text
     ↓
```
Actualización
```text
     ↓
```
Health Check
```text
     ↓
```
Activo

---

## 30. Dependency Security

Las dependencias del software deberán controlarse.

Deberemos contemplar:

actualizaciones,
vulnerabilidades conocidas,
dependencias abandonadas,
lockfiles,
análisis automatizado.

Esto será parte del proceso CI/CD.

---

## 31. Supply Chain Security

A medida que ERP Bridge crezca y admita conectores de terceros, aparecerá un riesgo adicional.

Un Connector externo podría ser malicioso.

Por tanto:

Third-party Connector
```text
        ↓
```
Validation
```text
        ↓
```
Permissions
```text
        ↓
```
Sandbox / Isolation
```text
        ↓
```
ERP Bridge

La ejecución de conectores de terceros deberá tratarse con especial cuidado.

---

## 32. Security Boundaries

La arquitectura deberá establecer límites claros:

```text
┌───────────────┐
│   Dashboard   │
└───────┬───────┘
        │
```
      API
```text
        │
┌───────▼───────┐
│     Core      │
└───────┬───────┘
        │
 ┌──────┴───────┐
 ↓              ↓
```
Agent       Connectors
```text
 ↓              ↓
```
Local        External
Systems      Systems

Cada frontera debe autenticar y autorizar las comunicaciones.

---

## 33. Security Monitoring

A largo plazo ERP Bridge deberá detectar comportamientos anómalos.

Ejemplos:

muchas peticiones fallidas,
Agent desconocido,
API Key utilizada desde un contexto extraño,
aumento anormal de operaciones,
múltiples intentos de autenticación.

Esto puede evolucionar posteriormente hacia detección automática.

---

## 34. Incident Response

Deberá existir un procedimiento para:

Detect
```text
  ↓
```
Contain
```text
  ↓
```
Investigate
```text
  ↓
```
Recover
```text
  ↓
```
Notify
```text
  ↓
```
Learn

No necesitamos construir toda la infraestructura de seguridad empresarial desde el primer día, pero el diseño deberá permitirla.

---

## 35. MVP

La primera versión deberá incluir como mínimo:

Authentication
Authorization
HTTPS/TLS
Secure Credentials
Agent Identity
Agent Revocation
API Key Security
Input Validation
Basic Audit Logs
Tenant Isolation
Secure Database Access

---

## 36. No construir todavía

No necesitamos inicialmente:

SSO empresarial.
SOC 2.
ISO 27001.
Zero Trust Enterprise completo.
SIEM propio.
HSM.
Advanced Threat Detection.
Sandbox complejo para terceros.

Estos podrán aparecer cuando el mercado lo justifique.

---

## 37. Criterio de éxito

Una empresa debe poder conectar sus sistemas con ERP Bridge sin tener que exponer innecesariamente su infraestructura interna ni entregar a ERP Bridge más permisos de los estrictamente necesarios.

---

## 38. Regla fundamental

ERP Bridge debe tratar cada conexión como una frontera de seguridad.

No debemos asumir:

"Está conectado, por lo tanto es de confianza."

Debemos asumir:

"Está conectado, por lo tanto debemos autenticarlo, autorizarlo y monitorizarlo."

---

## 39. Blindaje Técnico Anti-Copia e Ingeniería Inversa del Agente Local

Un contrato o registro de marca no impiden por sí solos que un tercero malintencionado desensamble el binario local del agente (`BentianAgent.exe`), extraiga las consultas OLEDB a Access (`F_ART`, `F_CLI`, `F_PCL`) o intente replicar la lógica de sincronización. Por ello, se implementa una arquitectura de defensa en profundidad anti-ingeniería inversa:

```text
 CÓDIGO FUENTE (TS)         OFUSCACIÓN AST               COMPILACIÓN V8               BINARIO NATIVO
 ┌─────────────────┐       ┌──────────────────────┐     ┌──────────────────────┐     ┌─────────────────┐
 │ • TypeScript    │ ───►  │ • javascript-        │ ──►│ • Compilación con    │ ──► │ • BentianAgent  │
 │   modular       │       │   obfuscator         │     │   BYTENODE           │     │   .exe final    │
 │ • Zero plaintext│       │ • String encryption  │     │ • Bytecode binario   │     │ • Imposible ver │
 │   SQL queries   │       │ • Control Flow Flat  │     │   V8 (.jsc)          │     │   código JS     │
 └─────────────────┘       └──────────────────────┘     └──────────────────────┘     └─────────────────┘
```

### 39.1. Auditoría de Vulnerabilidades y Contramedidas
1. **Eliminación de Sourcemaps y Minificación Obligatoria:**
   - En `builder/build-bundle.js`, `sourcemap: false` y `minify: true` por defecto en entornos de producción, eliminando comentarios y nombres internos de variables.
2. **Ofuscación AST de Grado Industrial:**
   - Integración de ofuscación de transformaciones AST sobre el bundle de producción.
   - **Cifrado de cadenas (String Encryption):** Todas las consultas SQL a Factusol, claves de registro y nombres de tablas se cifran en el binario y se descifran dinámicamente en memoria durante la ejecución.
   - **Aplanamiento del Flujo de Control (Control Flow Flattening):** La lógica secuencial de sincronización se descompone en máquinas de estados con saltos no lineales que frustran la descompilación legible.
3. **Compilación a Bytecode V8 (Bytenode):**
   - Node SEA (Single Executable Applications) almacena el código JS como recurso inyectado. La compilación a bytecode binario nativo V8 (`.jsc`) sustituye el texto plano por instrucciones de bajo nivel de la máquina virtual V8, impidiendo la lectura directa con herramientas de strings o editores hexadecimales.
4. **Modernización de HWID (Compatibilidad Windows 11 24H2):**
   - Debido a la eliminación definitiva de `wmic.exe` a partir de Windows 11 24H2 y Windows Server 2025, el cálculo del HWID en `HWIDManager` migra obligatoriamente a consultas CIM nativas de PowerShell (`Get-CimInstance Win32_DiskDrive` y `Win32_BaseBoard`).
5. **Licenciamiento Criptográfico Asimétrico (Ed25519):**
   - El backend en la nube firma los tokens de licencia con una clave privada Ed25519.
   - El ejecutable local embebe la clave pública y verifica matemáticamente la firma del token sin posibilidad de alteración local de estados.
   - Período de gracia offline de **168 horas (7 días)**: si la red local del cliente se interrumpe, la sincronización entre el ERP y la tienda online continúa operativa sin bloqueos de servicio.
6. **Marcado Canario (Watermarking Esteganográfico):**
   - Inyección en compilación de un identificador criptográfico único por cliente (`Client_ID`). Si un binario es distribuido a terceros o publicado en repositorios no autorizados, permite la trazabilidad forense matemática del infractor para la exigencia de la cláusula penal contractual.

---
