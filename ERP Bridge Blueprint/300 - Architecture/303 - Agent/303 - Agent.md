# Agent — v1.0
> **Estado:** Borrador

---

# 303 - Agent

> *Arquitectura del Agente de integración local.*

---

## 1. Propósito

El ERP Bridge Agent es un componente instalado en el entorno local del cliente que permite a ERP Bridge comunicarse de forma segura con sistemas que no están expuestos directamente a Internet.

Su función principal es actuar como puente seguro entre los sistemas locales de la empresa y ERP Bridge.

El Agent no es un componente obligatorio para todas las integraciones de ERP Bridge. Constituye un adaptador de ejecución local especializado que se despliega únicamente cuando el Connector requiere acceder a sistemas cerrados, archivos locales, puertos locales o servicios sin presencia cloud.

---

## 2. Problema que Resuelve

Muchos programas empresariales funcionan dentro del ordenador o red local de la empresa:

```text
EMPRESA
│
├── Factusol
├── Base de datos Access
├── Servidor local
└── Red interna
```

Estos sistemas no deberían requerir:
- Abrir puertos al exterior en el router/firewall.
- Exponer bases de datos a Internet público.
- Migrar masivamente todos sus datos a la nube.
- Cambiar el software operativo de la empresa.

---

## 3. Arquitectura Conceptual

```text
┌────────────── EMPRESA ──────────────┐
│                                     │
│  ERP / Programa local               │
│          │                          │
│          ↓                          │
│    ERP Bridge Agent                 │
│          │                          │
└──────────┼──────────────────────────┘
           │ (Conexión Saliente Cifrada)
        Internet
           │
           ↓
     ERP Bridge Core
           │
           ↓
      Ecommerce / CRM
```

---

## 4. Instalación sin Fricción

El proceso de instalación debe ser inmediato para el usuario:

```text
Descargar Agent ──> Instalar ──> Login / Código de vinculación ──> Detección de entorno ──> Seleccionar ERP ──> Conectar
```

> **Principio:** El usuario no debe configurar manualmente servidores, puertos, certificados, túneles VPN ni reglas de firewall.

---

## 5. Comunicación Saliente (Outbound-only)

> **Principio de seguridad:** El Agent siempre establece **conexiones salientes** hacia ERP Bridge en lugar de requerir conexiones entrantes desde Internet hacia la empresa.

```text
Empresa (Agent) ────── Conexión Saliente (HTTPS / WebSocket) ──────> ERP Bridge Core
```

Esto elimina la necesidad de IPs públicas fijas, redirección de puertos (*port forwarding*) o exposición de la red interna.

---

## 6. Funciones Principales

El Agent es responsable de:
1. Conectarse y autenticarse contra el Core.
2. Recibir instrucciones firmadas y autorizadas.
3. Ejecutar operaciones locales de lectura/escritura.
4. Devolver resultados estructurados.
5. Gestionar errores y reintentos locales.
6. Mantener el estado de latido (*heartbeat*).

---

## 7. Ejemplo con Factusol

```text
ERP Bridge Cloud ──("Obtener productos")──> ERP Bridge Agent ──> Factusol / Access ──> Productos ──> Agent ──> ERP Bridge Cloud
```

> La nube nunca accede directamente a Microsoft Access; el Agent realiza la operación localmente y transmite únicamente el resultado normalizado.

---

## 8. Principio de Mínimo Acceso

El Agent opera con los permisos estrictamente necesarios:
- ❌ *"ERP Bridge tiene acceso total al ordenador."*
- ✅ *"ERP Bridge Agent tiene acceso únicamente a los ficheros/bases de datos y operaciones expresamente autorizadas por el administrador."*

---

## 9. Gestión Segura de Credenciales Locales

Las credenciales de base de datos o del software local (Access, SQL, claves ODBC):
- Se almacenan cifradas localmente en el almacén de seguridad del sistema operativo.
- **Nunca se exponen en logs, URLs, respuestas API ni interfaz gráfica.**

---

## 10. Centro de Control Local Embebido (Embedded Control Center)

El Agente incorpora un servidor HTTP local en loopback (`127.0.0.1:39281`) que sirve una interfaz SPA de control completa en tiempo real. Esta interfaz permite al usuario gestionar íntegramente la integración desde su puesto de trabajo:

```text
BentianAgent.exe (Windows Native GUI)
│
├── Módulo Factusol ERP
│   ├── Selector nativo de archivos de Windows (.accdb / .mdb)
│   ├── Auto-detección de empresas instaladas
│   ├── Selector de Tarifa de Venta (F_TAR)
│   ├── Selector de Almacén (F_ALM)
│   ├── Series de Pedidos y Facturación
│   └── Explorador en vivo de artículos (F_ART + F_STO)
│
├── Módulo WooCommerce
│   ├── URL de tienda, Consumer Key y Consumer Secret
│   ├── Test de conexión REST API en vivo con medición de latencia
│   └── Mapeo configurable de estados de pedidos (Pendiente/Procesando/Completado)
│
├── Motor de Sincronización y Reglas
│   ├── Vigilante de archivos Access en tiempo real (AccdbFileWatcher)
│   ├── Estabilización por debounce (5s por defecto para liberar bloqueos .ldb)
│   ├── Sincronización periódica programada
│   ├── Toggles de campos (Stock, Precios, Descripciones)
│   ├── Stock de seguridad (buffer) y filtro stock > 0
│   └── Disparo manual de sincronización total o parcial
│
├── Historial de Sincronización y DLQ
│   ├── Registro histórico persistente en disco (sync-history.json)
│   └── Cola de reintentos quirúrgicos para bloqueos temporales de Access
│
├── Logs y Diagnóstico
│   ├── Visor en vivo con filtrado por nivel (Info, Éxito, Aviso, Error) y búsqueda
│   └── Exportación de informe técnico (.txt) para soporte con 1 clic
│
└── Licenciamiento y HWID
    ├── Estado de licencia y días de gracia offline
    ├── Identificador único de máquina (HWID)
    └── Activación / cambio de clave de puesto (Seat Key EB-XXXXX)
```

---

## 11. Ejecución Nativa Zero-Console (PE Subsystem GUI)

Para garantizar una experiencia de usuario nativa de calidad comercial, el binario `BentianAgent.exe` se empaqueta y parchea eliminando cualquier consola de comandos:

- **Problema de origen:** Los runtimes tradicionales de Node.js o wrappers de consola ejecutan con el subsistema PE `IMAGE_SUBSYSTEM_WINDOWS_CUI` (valor `0x0003`), lo que provoca que Windows cree inmediatamente una ventana negra (`conhost.exe` / `cmd.exe`) visible para el usuario.
- **Solución técnica implementada:** Durante el proceso de compilación del binario en `builder/build-exe.js`, el PE Header de 64 bits (`IMAGE_OPTIONAL_HEADER64.Subsystem` en el offset `peOffset + 24 + 68`) es parcheado quirúrgicamente al valor `0x0002` (`IMAGE_SUBSYSTEM_WINDOWS_GUI`).
- **Garantía operativa:** Al hacer doble clic en el instalador o en el acceso directo del escritorio, Windows **nunca** crea ni muestra ninguna consola de terminal. El agente arranca de forma 100% invisible y abre automáticamente el Centro de Control en el navegador predeterminado del usuario.

---

## 12. Protocolo de Comunicación con el Core

- Comunicación mediante canales seguros (**HTTPS / WSS - Secure WebSockets**).
- Resistencia a micro-cortes y reconexión automática instantánea.

---

## 13. Gestión de Desconexiones de Red

```text
Agent Conectado ✓
      ↓
Pérdida de conexión a Internet
      ↓
Encolado de trabajo local pendiente (Queue)
      ↓
Recuperación de Internet
      ↓
Sincronización y vaciado de cola ordenado
```

---

## 14. Cola Local de Operaciones (Offline Queue)

```text
Operación ──> Local Queue ──> ¿Internet disponible?
                                ├── No ──> Conservar en cola local persistente
                                └── Sí ──> Despachar hacia la tienda online
```

---

## 15. Actualizaciones Controladas con Firmas Ed25519 y Rollback Atómico

- **Distribución de Manifiestos Firmados:** Cada actualización lanzada desde el Core incorpora un manifiesto con hash SHA-256 firmado asimétricamente con **Ed25519**. El Agente rechaza cualquier binario cuya firma no verifique contra la clave pública integrada.
- **Sustitución Atómica en Windows (Zero Downtime):** El ejecutable en caliente se renombra a `.old` para sortear el bloqueo de archivo del kernel de Windows y se reemplaza por el nuevo binario sin requerir desinstalador.
- **Auto-Rollback de Seguridad:** El agente supervisa su propio estado de salud durante los primeros 10 segundos post-arranque. Si el nuevo binario crashea o falla, un script independiente restaura automáticamente la versión anterior desde `.bak`.

---

## 16. Compatibilidad y Foco

La primera versión se enfoca exclusivamente en los entornos objetivo necesarios (Windows 10, Windows 11 y Windows Server para Factusol y Access), evitando dispersión en múltiples sistemas operativos el día uno.

---

## 17. Seguridad, Identificador de Hardware (HWID) y Ciclo de Vida

- **Cálculo de HWID Moderno (Windows 11 24H2 y Server 2025):** Para garantizar compatibilidad total tras la eliminación de `wmic.exe` por Microsoft, el Agente extrae el número de serie de la placa base y disco mediante PowerShell CIM nativo (`Get-CimInstance Win32_DiskDrive` y `Win32_BaseBoard`).
- **Almacenamiento Cifrado Local (SecureStore):** Las credenciales locales y tokens de sesión se guardan en `%APPDATA%/erp-bridge/license.enc` cifrados con **AES-256-GCM**, cuya clave deriva del HWID mediante PBKDF2.
- **Cero Datos de Negocio en la Nube (Zero-Knowledge):** El Agente procesa todo en local. Nunca envía catálogos completos ni bases de datos Access a la nube; solo se comunican deltas cifrados punto a punto con la tienda.
- **Período de Gracia Offline:** 7 días (168 horas) de tolerancia ante cortes de conexión.

---

## 18. Identidad del Agent

```text
Empresa / Tenant ──> Instalación Local ──> Agent ID Único
```

---

## 19. Multi-empresa y Multi-sistema

Un único Agent podrá eventualmente gestionar múltiples orígenes en una misma red (ej. Factusol + Base de datos externa), pero el MVP priorizará la máxima simplicidad (1 Agent = 1 instalación).

---

## 20. Monitorización en Tiempo Real

El Dashboard central reflejará el estado del Agent:

```text
Agent: Empresa ABC
Estado:          🟢 Online (Latido cada 30s)
Versión:         1.0.4
Última conexión: 18/08/2026 22:41
Conectores:
├── Factusol 🟢
└── Access   🟢
```

---

## 21. Política de Logs y Privacidad

- Logs técnicos locales detallados para diagnóstico.
- Envío al Core únicamente de metadatos de error (código de error, conector, operación), **sin transferir datos sensibles ni volcados de bases de datos**.

---

## 22. Política de Mínima Transferencia de Datos

> **El Agent solo transmite los datos estrictamente necesarios para la integración configurada.**

```text
Factusol (10.000 productos) ──> Agent (Detecta 5 productos modificados) ──> Core (Transmite 5 productos)
```

Esto reduce drásticamente ancho de banda, consumo de almacenamiento y riesgos de exposición.

---

## 23. El Agent como Extensión del Core

El Agent no es una plataforma duplicada, sino un ejecutor remoto especializado:

```text
ERP Bridge Core (Cerebro: Lógica, reglas, flujos, mapeos)
       │
       ↓ (Instrucciones canónicas)
ERP Bridge Agent (Brazos: Acceso local y ejecución física)
```

Para sistemas con APIs cloud estándar, el Core orquesta y ejecuta las llamadas directamente. Para sistemas locales o legacy, el Core delega la ejecución física en el Local Agent mediante instrucciones canónicas seguras.

---

## 24. Qué NO Debe Hacer el Agent

El Agent no debe convertirse en:
- Un ERP o base de datos centralizada.
- Un servidor web de propósito general.
- Un sistema de almacenamiento masivo en disco.

---

## 25. Alcance del MVP del Agent

```text
Agent MVP
│
├── Instalador ligero para Windows
├── Identidad y autenticación segura con Core
├── Conexión saliente WebSocket / HTTPS
├── Ejecución de consultas locales para Factusol
├── Logs estructurados locales
└── Heartbeat y reporte de estado básico
```

---

## 26. Caso Factusol (Objetivo Clave del MVP)

```text
┌────────────── CLIENTE ──────────────┐
│                                     │
│ Factusol (Access)                   │
│    │                                │
│    ↓                                │
│ ERP Bridge Agent                    │
│                                     │
└────┼────────────────────────────────┘
     │
     │ HTTPS / WebSocket (Saliente)
     ↓
ERP Bridge Core
     │
     ↓
WooCommerce / Shopify / CRM
```

---

## 27. Definición de Éxito

> **El Agent tendrá éxito cuando una empresa pueda instalarlo en su entorno local y conectar un sistema cerrado a Internet con ERP Bridge sin modificar su infraestructura de red de forma compleja.**

---

## 28. Regla Final

> **El Agent debe hacer posible integrar software antiguo y sistemas locales con tecnologías modernas sin obligar a la empresa a abandonar su infraestructura existente.**
