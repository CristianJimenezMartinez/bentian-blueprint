# Coding Standards — v1.0
> **Estado:** Borrador

---

# 401 - Coding Standards

> *Estándares de desarrollo, guías de estilo TypeScript, patrones de arquitectura y reglas para desarrolladores e IA.*

---

## 1. Propósito

Definir las reglas de desarrollo que debe seguir todo el código de **ERP Bridge** para mantener una arquitectura consistente, mantenible, testeable y preparada para crecer de forma sostenible.

> **Alcance:** Estas directrices deben ser respetadas rigurosamente tanto por **desarrolladores humanos** como por **asistentes y agentes de inteligencia artificial** que operen sobre la base de código.

---

## 2. Principio Fundamental: Simplicidad y Claridad

> **El código debe ser simple, explícito y mantenible antes que sofisticado.**

ERP Bridge no implementa complejidad técnica que no responda a una necesidad real demostrada:
- ❌ Sin microservicios prematuros.
- ❌ Sin abstracciones innecesarias *"por si acaso"*.
- ❌ Sin frameworks adicionales sin justificación.
- ❌ Sin dependencias superfluas.

---

## 3. Stack Tecnológico Base

```text
Backend:    Node.js + TypeScript
Frontend:   Angular + TypeScript + HTML + SCSS
Database:   PostgreSQL (Multi-tenant)
Agent:      Node.js + TypeScript (Ejecutable local ligero en Windows)
```

---

## 4. TypeScript como Lenguaje Universal

Todo el código de aplicación debe escribirse en **TypeScript**:
- JavaScript queda reservado exclusivamente para scripts auxiliares de tooling o configuraciones específicas de empaquetado.

---

## 5. Modo Estricto Obligatorio (`strict: true`)

El archivo `tsconfig.json` debe mantener activado el modo estricto en todos los proyectos:
- `strict: true`
- `noImplicitAny: true`
- `strictNullChecks: true`

---

## 6. Prohibición de `any`

> **El uso de `any` está expresamente prohibido salvo casos extremos debidamente justificados.**

- Si se desconoce la estructura inicial de un dato externo, utilizar `unknown` y aplicar validación de tipos (*type narrowing* o librerías de esquema como Zod).

---

## 7. Interfaces y Contratos Explícitos

Toda frontera del sistema se define mediante interfaces tipadas:

```typescript
interface Connector {
  connect(config: ConnectionConfig): Promise<void>;
  disconnect(): Promise<void>;
  healthCheck(): Promise<HealthStatus>;
  getCapabilities(): ConnectorCapabilities;
}
```

---

## 8. Tipos de Dominio Fuertes

Los conceptos centrales de ERP Bridge disponen de sus propios tipos e interfaces tipadas:
- `Organization`, `User`, `Membership`
- `Connector`, `Connection`, `Agent`, `Credential`
- `Event`, `Flow`, `FlowStep`, `Action`
- `SyncJob`, `SyncExecution`, `Mapping`

*(Prohibido representar el dominio como objetos genéricos `Record<string, any>`)*.

---

## 9. Convenciones de Nomenclatura (Naming)

Usar nombres descriptivos, pronunciables y que revelen la intención de negocio:
- ✅ `connectionService`, `syncExecutionRunner`, `connectorRegistry`
- ❌ `data`, `manager`, `helper`, `thing`, `utils2`

---

## 10. Variables y Funciones: `camelCase`

```typescript
const connectionId = 'conn_8f71';
const syncExecution = await syncService.runJob(jobId);
```

---

## 11. Clases, Tipos y Enums: `PascalCase`

```typescript
class FactusolConnector implements Connector {}
enum SyncDirection { ONE_WAY, TWO_WAY }
type HealthStatus = 'HEALTHY' | 'DEGRADED' | 'DOWN';
```

---

## 12. Interfaces: Nombres Descriptivos sin Prefijos

- ✅ `interface Connector`, `interface ConnectionConfig`, `interface SyncResult`
- ❌ Evitar prefijos como `IConnector` o `IConnectionConfig`.

---

## 13. Convención de Archivos (`kebab-case.role.ts`)

```text
factusol.connector.ts
connection.service.ts
sync.engine.ts
connection.repository.ts
create-connection.dto.ts
```

---

## 14. Principio de Responsabilidad Única (SRP)

Cada módulo, clase o archivo debe tener una única razón para cambiar.
- Prohibido crear archivos gigantes que mezclen enrutamiento HTTP, acceso a base de datos, lógica de negocio y llamadas a APIs externas.

---

## 15. Aislamiento Estricto del Core

> **El Core nunca debe importar ni depender de un conector concreto.**

```text
Core Engine ──> Connector Interface (Contrato Canónico)
                         ▲
                         │ (Implementación aislada)
                 FactusolConnector
```

---

## 16. Aislamiento entre Conectores

Cada conector encapsula su propia lógica sin contaminar a los demás:
```text
connectors/
├── factusol/      (Específico de Access / ODBC / Factusol)
└── woocommerce/   (Específico de WooCommerce REST API)
```

---

## 17. Contrato Universal de Conectores

Todos los conectores implementan el contrato canónico:

```typescript
interface Connector {
  getMetadata(): ConnectorMetadata;
  connect(config: ConnectionConfig): Promise<void>;
  disconnect(): Promise<void>;
  healthCheck(): Promise<HealthStatus>;
  getCapabilities(): ConnectorCapabilities;
}
```

---

## 18. Modelo Basado en Capacidades (Capability Checking)

Antes de invocar una operación, el Core consulta si el conector la soporta:

```typescript
if (connector.getCapabilities().supportsWebhooks) {
  // Configurar webhook push
} else {
  // Configurar polling programado
}
```

---

## 19. Cero Abstracciones Prematuras (*No Fake Abstractions*)

No crear capas de abstracción o patrones de diseño por anticipación si no resuelven una necesidad actual del producto.

---

## 20. Capa de Servicios (`Services`)

Contienen exclusivamente la lógica de negocio y orquestación:
- `ConnectionService`, `SyncService`, `FlowService`, `AgentService`

---

## 21. Capa de Controladores (`Controllers`)

Responsables únicamente de recibir la petición HTTP, validar el DTO de entrada, invocar al servicio y devolver la respuesta:

```text
HTTP Request ──> Controller (Validar DTO) ──> Service (Lógica) ──> Controller (Format Response)
```

---

## 22. Capa de Repositorios (`Repositories`)

Encapsulan todas las consultas a PostgreSQL:
- Prohibido escribir sentencias SQL dispersas en servicios o controladores.

---

## 23. Objetos de Transferencia de Datos (`DTOs`)

Toda entrada y salida de API se tipa con DTOs explícitos:
- `CreateConnectionDto`, `UpdateConnectionDto`, `ConnectionResponseDto`

---

## 24. Validación Estricta de Entradas

Todo dato proveniente del exterior (API, webhooks, mensajes del Agent) se valida en el borde antes de ser procesado por la lógica de negocio.

---

## 25. Manejo Estructurado de Errores de Dominio

Prohibido lanzar errores genéricos sin contexto:

```typescript
// ❌ Incorrecto
throw new Error("Fallo de conexión");

// ✅ Correcto
throw new ConnectionError(
  ErrorCode.CONNECTION_TIMEOUT,
  "Tiempo de espera agotado al conectar con el servidor local",
  { connectionId, host, timeoutMs: 5000 }
);
```

---

## 26. Códigos de Error Estandarizados

```typescript
enum ErrorCode {
  CONNECTION_AUTH_FAILED = 'CONNECTION_AUTH_FAILED',
  CONNECTION_TIMEOUT = 'CONNECTION_TIMEOUT',
  CONNECTOR_UNSUPPORTED_OPERATION = 'CONNECTOR_UNSUPPORTED_OPERATION',
  SYNC_MAPPING_ERROR = 'SYNC_MAPPING_ERROR',
  SYNC_DESTINATION_ERROR = 'SYNC_DESTINATION_ERROR',
}
```

---

## 27. Logging Estructurado y Contextual

Toda traza debe responder a: *¿Qué ocurrió?, ¿Cuándo?, ¿Qué Organización?, ¿Qué Conexión?, ¿Qué Conector?, ¿Resultado?*

```typescript
logger.info("Sincronización de lote completada", {
  organizationId: org.id,
  connectionId: conn.id,
  jobId: job.id,
  processedCount: 150,
  durationMs: 1240,
});
```

---

## 28. Prohibición de Datos Sensibles en Logs

> **Filtros automáticos deben impedir el registro de contraseñas, API keys, tokens o secretos en los logs.**

---

## 29. Gestión de Configuración Externa

Prohibido hardcodear configuraciones sensibles en el código fuente:
- Uso de variables de entorno y Bóveda de Secretos cifrada.

---

## 30. Variables de Entorno Claras

```text
DATABASE_URL=postgresql://user:pass@localhost:5432/erpbridge
API_BASE_URL=https://api.erpbridge.io
LOG_LEVEL=info
```

---

## 31. Cifrado de Secretos

- Cifrado en reposo con **AES-256-GCM**.
- Secretos nunca commiteados en Git ni expuestos en APIs.

---

## 32. Código Asíncrono Limpio (`async / await`)

Uso consistente de `async / await` evitando anidamientos de `.then()` o callbacks complejos.

---

## 33. Acceso Centralizado a Base de Datos

Uso de un pool de conexiones administrado centralmente; prohibido abrir conexiones manuales directas a la base de datos de forma dispersa.

---

## 34. Migraciones de Base de Datos Obligatorias

Todo cambio en el esquema relacional de PostgreSQL se gestiona mediante migraciones versionadas y reproducibles.

---

## 35. Transacciones ACID

Las operaciones que requieran coherencia atómica se ejecutan dentro de transacciones de base de datos con rollback automático en caso de fallo.

---

## 36. Diseño Idempotente

Toda operación crítica (ingesta de webhooks, creación de pedidos, sincronizaciones) debe soportar reintentos sin duplicar registros.

---

## 37. Resiliencia ante Sistemas Externos

Todo conector asume que el sistema remoto puede fallar, demorarse o devolver errores:
- Manejo explícito de timeouts, rate limits y reconexiones.

---

## 38. Reintentos Controlados con Backoff

Reintentos automáticos para errores transitorios de red, con límite de intentos para evitar bucles infinitos.

---

## 39. Timeouts Obligatorios

Ninguna llamada HTTP, socket o consulta a base de datos externa puede quedar esperando indefinidamente sin un timeout configurado.

---

## 40. Independencia entre Conectores

Ningún conector puede importar ni asumir la existencia de otro conector.

---

## 41. Prohibición de Llamadas Directas Conector ↔ Conector

```text
❌ FactusolConnector ──> WooCommerceConnector
✅ FactusolConnector ──> Core / Sync Engine ──> WooCommerceConnector
```

---

## 42. Diseño RESTful de la API

Endpoints intuitivos basados en recursos y verbos HTTP estándar:
- `/api/v1/connections`
- `/api/v1/connectors`
- `/api/v1/agents`
- `/api/v1/flows`
- `/api/v1/sync-jobs`

---

## 43. Versionado de APIs

Toda API pública incluye prefijo de versión (`/api/v1/`). Cualquier cambio incompatible requiere una nueva versión de ruta (`/api/v2/`).

---

## 44. Compatibilidad Hacia Atrás (Backward Compatibility)

Las actualizaciones del SDK, API y contratos del Agent deben ser retrocompatibles para evitar romper agentes locales en producción.

---

## 45. Protocolo Versionado para el Agente

La comunicación Core ↔ Local Agent utiliza un protocolo formalmente versionado y seguro sobre WSS/HTTPS.

---

## 46. Aislamiento del Agente Local

El servidor en la nube no asume acceso directo a la máquina del cliente; toda interacción se delega al Agent local.

---

## 47. Seguridad por Defecto (Security by Default)

Toda petición debe autenticarse y autorizarse; nunca se confía ciegamente en una llamada por proceder de un socket interno.

---

## 48. Testabilidad Obligatoria

Todo código del Core, conectores, motor de eventos y sincronización debe ser 100% testeable de forma aislada mediante mocks.

---

## 49. Documentación Técnica Viva

Todo nuevo endpoint, interfaz o contrato público debe acompañarse de su respectiva documentación técnica actualizada.

---

## 50. Comentarios Útiles (El "Por qué", no el "Qué")

```typescript
// ❌ Inútil: Incrementa el contador
counter++;

// ✅ Valioso: Se incrementa solo tras confirmación de Factusol para evitar contar registros fallidos.
counter++;
```

---

## 51. Gestión de TODOs

Todo comentario `// TODO:` relevante debe correlacionarse con una tarea formal en el Roadmap o un ADR en `600 - Decisions`.

---

## 52. Control Estricto de Dependencias

Antes de instalar un paquete npm se evalúa su peso, mantenimiento, seguridad y licencia; prohibido añadir dependencias para utilidades triviales.

---

## 53. Registro de Decisiones Arquitectónicas (ADRs)

Cualquier cambio de stack o decisión de diseño de alto impacto debe registrarse formalmente en `600 - Decisions`.

---

## 54. Reglas Estrictas para Código Generado por IA

> **El código generado por IA está sujeto exactamente a los mismos estándares de calidad, seguridad y pruebas que el código humano.**

La IA tiene terminantemente prohibido:
- Alterar la arquitectura sin autorización explícita.
- Introducir dependencias no justificadas.
- Romper contratos de interfaces o eliminar tests para forzar el paso de un build.
- Duplicar lógica existente en lugar de reutilizarla.

---

## 55. Protocolo de Desarrollo con IA en 7 Pasos

```text
1. Comprender la arquitectura existente
2. Inspeccionar los archivos afectados
3. Identificar dependencias y contratos
4. Explicar el cambio propuesto
5. Implementar el cambio
6. Ejecutar tests unitarios
7. Verificar que el build compila limpiamente
```

---

## 56. Cambios Mínimos y Quirúrgicos (Minimal Changes)

Modificar únicamente el código estrictamente necesario para cumplir el objetivo; evitar refactorizaciones masivas no solicitadas.

---

## 57. Refactorizaciones Independientes

Las mejoras estructurales o refactors deben separarse en commits/PRs distintos del desarrollo de nuevas funcionalidades.

---

## 58. Convención de Commits Git (Conventional Commits)

```text
feat: add connector registry interface
fix: handle connection timeout in factusol agent
refactor: extract canonical product mapping helper
test: add unit tests for sync execution runner
docs: update architecture overview diagram
```

---

## 59. Estrategia de Ramas Git

Uso de ramas ordenadas (`main`, `develop`, `feature/*`, `fix/*`).

---

## 60. Revisiones de Código (Pull Requests)

Toda PR documenta: qué cambió, por qué, cómo probarlo y posibles riesgos.

---

## 61. Builds Reproducibles y Deterministas

La compilación y empaquetado del proyecto debe ser 100% reproducible a través de lockfiles y scripts de CI/CD.

---

## 62. Experiencia de Desarrollo Local (*Developer Experience*)

Un nuevo desarrollador debe poder clonar, instalar dependencias, configurar variables de entorno y arrancar el entorno local en minutos.

---

## 63. Seguridad en Producción

Ningún cambio crítico se despliega a producción sin haber sido validado en entornos de staging o mediante suites de testing automatizadas.

---

## 64. Prioridades de Ingeniería

```text
1. Correctitud (Correctness)
      ↓
2. Seguridad (Security)
      ↓
3. Mantenibilidad (Maintainability)
      ↓
4. Rendimiento (Performance)
```

---

## 65. Escalabilidad Progresiva

Diseñar la arquitectura pensando en el crecimiento, pero sin implementar infraestructura compleja antes de que exista demanda real.

---

## 66. Monolito Modular Primero (*Monolith First*)

La arquitectura inicial se despliega como un **monolito modular** limpio:
- `Core`, `API`, `Events`, `Sync` y `Connectors` comparten base de código modularizada, facilitando el desarrollo por un equipo pequeño sin el sobrecoste operacional de microservicios.

---

## 67. Fronteras Modulares Claras

Los módulos mantienen fronteras limpias a través de interfaces, permitiendo desacoplarlos en servicios independientes en el futuro si la escala lo justifica.

---

## 68. No al Overengineering

Prohibido introducir Kubernetes, Service Mesh, Kafka distribuido o Redis Cluster si PostgreSQL y colas locales resuelven la carga con holgura.

---

## 69. Definición de Hecho (*Definition of Done - DoD*)

Una tarea se considera completada únicamente cuando cumple:
- [x] Implementación completa y tipada estrictamente.
- [x] Validación y sanitización de datos de entrada.
- [x] Manejo estructurado de errores y logs contextuales.
- [x] Cobertura de tests unitarios/integración superada.
- [x] Documentación actualizada.
- [x] Compilación y build exitoso.

---

## 70. La Regla de Oro (*Golden Rule*)

> **ERP Bridge debe ser suficientemente sofisticado para resolver problemas empresariales reales, pero suficientemente simple para que una sola persona pueda entender, mantener y evolucionar el sistema.**

---

## 71. Regla Suprema para la Inteligencia Artificial

> **La IA es un acelerador de desarrollo, nunca una autoridad arquitectónica.**
> 
> Toda decisión técnica y de producto de alto nivel debe estar formalmente respaldada en el Blueprint y registrada en el bloque `600 - Decisions`.

---

## 72. Objetivo Final

> **El código de ERP Bridge debe permitir que Cristian entienda el sistema al 100%, pueda modificarlo con agilidad, probarlo con certeza, desplegarlo con seguridad y mantenerlo en solitario incluso cuando la plataforma crezca considerablemente.**
