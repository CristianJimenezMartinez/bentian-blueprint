# Bentian ERP Bridge — Ecosistema y Arquitectura Central

> **Espacio de Trabajo Central y Repositorio de Arquitectura, Blueprints y Reglas de Proyecto.**  
> Desarrollado por **Cristian Jiménez Martínez** (<cristianjimeneztrabajo@gmail.com>).

---

## 🗺️ Mapa de Repositorios del Ecosistema

El proyecto Bentian está modularizado en repositorios especializados e interconectados:

| Repositorio | Descripción | Enlace en GitHub |
| :--- | :--- | :--- |
| **`erp-bridge`** | **Monorepo Principal:** Agente de escritorio Windows, API Cloud (Hetzner), conectores OLEDB (Factusol, WooCommerce, PrestaShop, SimplyGest) y Web App. | [github.com/CristianJimenezMartinez/erp-bridge](https://github.com/CristianJimenezMartinez/erp-bridge) |
| **`cristianjm`** | **Sitio Web Corporativo & Portfolio:** Landing pages de autoridad, servicios de rescate ERP/Verifactu y demos interactivas. | [github.com/CristianJimenezMartinez/cristianjm](https://github.com/CristianJimenezMartinez/cristianjm) |
| **`bentian-status`** | **Página de Estado & Catálogo:** Telemetría en vivo, estado de la red y catálogo de servicios firmado criptográficamente. | [github.com/CristianJimenezMartinez/bentian-status](https://github.com/CristianJimenezMartinez/bentian-status) |
| **`Bentian_Conector_Factusol_Node_TS`** | **Conector Factusol Autónomo:** Implementación TypeScript independiente para consultas directas y extracción de esquemas XSD. | [github.com/CristianJimenezMartinez/Bentian_Conector_Factusol_Node_TS](https://github.com/CristianJimenezMartinez/Bentian_Conector_Factusol_Node_TS) |
| **`bentian-blueprint`** | **Plan Director, Estrategia & Reglas:** Blueprints de diseño, playbooks comerciales de mercado, reglas de agentes y habilidades. | [github.com/CristianJimenezMartinez/bentian-blueprint](https://github.com/CristianJimenezMartinez/bentian-blueprint) |

---

## 📁 Estructura de este Repositorio

* **`ERP Bridge Blueprint/`**: Especificaciones de producto, arquitectura detallada, modelos de datos y registro de decisiones de ingeniería (ADRs).
* **`mundo-real/`**: Estrategia de comercialización (GTM), inteligencia competitiva, playbooks de prospección y acuerdos de homologación con distribuidores.
* **`docs/`**: Dictámenes técnicos y regulatorios, análisis de riesgos y catálogos de contingencia.
* **`.agent/` y `.agents/`**: Habilidades operativas del agente (`bentian-security-architecture`, `kling-cli`).
* **`AGENTS.md` / `GEMINI.md`**: Reglas mandatarias inmutables de desarrollo (persistencia en AppData, anti-wiping NAS, módulos congelados y Quality Gate).

---

## 🛡️ Reglas Fundamentales de Blindaje

1. **Persistencia Segura:** Configuración exclusiva en `%APPDATA%\Bentian Agent\agent-config.json`.
2. **Anti-Wiping de Red:** Preservación indestructible de rutas UNC y NAS ante caídas de red.
3. **Calidad Continua:** Quality Gate automatizado con 8 puertas de validación criptográfica y modular.
4. **Identidad de Commits:** Todos los cambios se firman bajo la identidad oficial `CristianJimenezMartinez <cristianjimeneztrabajo@gmail.com>`.
