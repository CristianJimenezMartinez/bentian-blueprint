# Technical Decisions — v1.0
> **Estado:** Borrador

---

# 603 - Technical Decisions

> *Registro de decisiones técnicas, selección de stack, librerías y estándares de ingeniería.*

---

## 1. Resumen / Definición
Registro de decisiones de implementación técnica y selección de tecnologías.

---

## 2. Decisiones Técnicas Clave
- **TDR-001:** Stack de backend: Node.js con TypeScript estricto para garantizar tipado canónico y rendimiento asíncrono I/O.
- **TDR-002:** Base de datos relacional PostgreSQL con aislamiento de esquemas/tablas para soporte Multi-Tenant nativo.
- **TDR-003:** Comunicación segura entre Agent y Core: WebSockets seguros (WSS) con fallback a polling HTTPS.
- **TDR-004:** Cifrado de credenciales en reposo mediante AES-256-GCM y en tránsito mediante TLS 1.3.
- **TDR-005:** Manejo de bases de datos Access/Factusol mediante consultas SQL parametrizadas para prevenir inyecciones y bloqueos de archivos.
- **TDR-006:** Idempotencia en APIs y webhooks mediante cabecera `Idempotency-Key` y almacén de correlación de eventos.
- **TDR-007:** Adopción de **Supabase (PostgreSQL Gestionado)** en región UE (Frankfurt) como motor de persistencia en producción para cumplir la Golden Rule (404 §19 y §72). Aporta backups automáticos diarios con recuperación en 1 clic, consola web Supabase Studio para inspección operativa de tablas, alta disponibilidad y conexión estándar mediante Supavisor Pooler con TLS/SSL y compatibilidad IPv4/IPv6, manteniendo conmutación transparente con PostgreSQL local (Docker) para desarrollo mediante la variable `DATABASE_URL`.
- **TDR-008:** Topología de Despliegue de la API y Edge Proxy: El backend Node.js (`apps/api`) se empaqueta como contenedor Docker persistente en servidor VPS (Hetzner Cloud) o PaaS (Railway), protegido en el edge por Cloudflare (Cloudflare Tunnel / Proxy) para terminación TLS gratuita, mitigación DDoS y ocultación de la IP de origen.
- **TDR-009:** Despliegue de Frontend en Edge CDN: El Dashboard Angular (`apps/dashboard`) se compila a estático (`ng build`) y se sirve globalmente vía Cloudflare Pages (CDN gratuito), desacoplando totalmente el tráfico de navegación del cómputo del backend.
- **TDR-010:** Parcheo Binario PE Subsystem a GUI (`0x0002`) y Servidor Loopback HTTP Local: Para eliminar por completo el parpadeo y la ventana de consola negra (`conhost.exe` / `cmd.exe`) al arrancar el agente en Windows, el pipeline de empaquetado (`builder/build-exe.js`) modifica directamente el campo `IMAGE_OPTIONAL_HEADER64.Subsystem` en la cabecera PE del binario x64 inyectado con Node SEA, configurando el valor `0x0002` (`IMAGE_SUBSYSTEM_WINDOWS_GUI`). El agente arranca de forma 100% nativa sin ventana de comandos, levantando un micro-servidor HTTP loopback (`127.0.0.1:39281`) que sirve el Centro de Control local y los endpoints REST locales (`/api/local/*`) para la configuración completa de Factusol, WooCommerce, reglas de sincronización, explorador de artículos en vivo, logs y diagnóstico.

---

## 3. Notas y Acuerdos
- Implementación de Zero-Console PE Subsystem verificada en compilación SEA con Inno Setup installer y arranque dual (interactivo y background).
