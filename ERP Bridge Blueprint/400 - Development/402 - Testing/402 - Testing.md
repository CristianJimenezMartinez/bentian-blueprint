# Testing — v1.0
> **Estado:** Borrador

---

# 402 - Testing

> *Estrategia de aseguramiento de calidad, pirámide de pruebas, contratos de conectores y suites automatizadas.*

---

## 1. Propósito

Definir cómo se verifica que ERP Bridge funciona correctamente antes de considerar una funcionalidad terminada.

El sistema de testing deberá proteger especialmente:

Core.
Connectors.
Agent.
API.
Events.
Synchronization.
Authentication.
Data integrity.

---

## 2. Principio fundamental

Un cambio no está terminado porque compile. Está terminado cuando cumple su comportamiento esperado y puede demostrarse mediante pruebas.

---

## 3. Testing Pyramid

ERP Bridge utilizará diferentes niveles de pruebas:

                 E2E
                /   \
             Integration
            /          \
        Unit Tests

La mayor cantidad de tests estará en las capas inferiores.

---

## 4. Unit Tests

Los Unit Tests comprobarán unidades pequeñas de código de forma aislada.

Ejemplos:

MappingService
SyncEngine
EventParser
ConnectorRegistry
Validation
Transformations

Deben ser rápidos y ejecutarse frecuentemente.

---

## 5. Integration Tests

Los Integration Tests comprobarán la interacción entre componentes reales.

Ejemplos:

API
```text
 ↓
```
Service
```text
 ↓
```
Repository
```text
 ↓
```
PostgreSQL

También:

Sync Engine
```text
 ↓
```
Connector
```text
 ↓
```
Database

---

## 6. End-to-End Tests

Los E2E comprobarán flujos completos.

Ejemplo:

Login
```text
 ↓
```
Create Connection
```text
 ↓
```
Configure Sync
```text
 ↓
```
Start Sync
```text
 ↓
```
Process Data
```text
 ↓
```
Verify Result

---

## 7. Connector Tests

Cada Connector deberá disponer de sus propias pruebas.

Ejemplo:

connect()
disconnect()
healthCheck()
getCapabilities()
getProducts()
getOrders()
updateStock()

No se considerará terminado un Connector sin pruebas mínimas.

---

## 8. Connector Contract Tests

Además de tests específicos, todos los Connectors deberán cumplir un conjunto común de pruebas.

Conceptualmente:

Connector Contract
```text
       ↓
```
Factusol ✓
WooCommerce ✓
Sage ✓
Shopify ✓

Esto permitirá detectar rápidamente si un Connector rompe el contrato común.

---

## 9. Core Tests

El Core deberá tener una cobertura elevada porque todos los Connectors dependen de él.

Se probarán especialmente:

Connector Registry.
Connection Management.
Events.
Flows.
Actions.
Synchronization.
Error handling.

---

## 10. Agent Tests

El Agent deberá probar:

Installation
Connection
Authentication
Heartbeat
Health
Communication
Reconnection
Shutdown
Update

---

## 11. Agent Offline Tests

El Agent deberá ser probado bajo condiciones reales de red.

Ejemplos:

Internet lost
Internet restored
Server unavailable
Server restarted
Agent restarted

El Agent deberá poder recuperar la conexión cuando corresponda.

---

## 12. API Tests

Las APIs deberán probar:

Authentication.
Authorization.
Validation.
Responses.
Errors.
Rate limits cuando existan.
Resource ownership.

---

## 13. Security Tests

Las pruebas de seguridad deberán comprobar especialmente:

Organization A
```text
       ↓
```
NO ACCESS
```text
       ↓
```
Organization B

Nunca se deberá poder acceder a recursos de otra Organization manipulando IDs.

---

## 14. Authentication Tests

Se deberán probar:

Valid credentials
Invalid credentials
Expired credentials
Revoked credentials
Missing credentials

---

## 15. Authorization Tests

Cada recurso deberá comprobar los permisos necesarios.

Ejemplo:

VIEWER
```text
  ↓
```
Read connection ✓
Modify connection ✗
Delete connection ✗

---

## 16. Multi-Tenant Tests

Se deberán crear tests explícitos para aislamiento entre organizaciones.

Ejemplo:

Organization A
Connection A


Organization B
Connection B

El sistema deberá impedir:

```text
A → B
B → A
```

---

## 17. Synchronization Tests

La sincronización será una de las áreas con mayor cobertura.

Se deberán probar:

Create
Update
Delete
Duplicate
Retry
Failure
Partial failure
Timeout

---

## 18. Idempotency Tests

Ejemplo:

ORDER_CREATED

se procesa una vez.

Si llega el mismo evento otra vez:

ORDER_CREATED
ORDER_CREATED

el sistema deberá evitar crear dos pedidos cuando la operación deba ser idempotente.

---

## 19. Duplicate Tests

Se deberán probar duplicados procedentes de:

Webhooks.
Retries.
Reconexiones.
Jobs.
Eventos repetidos.

---

## 20. Mapping Tests

Los mappings deberán comprobar:

```text
Field A → Field B
Field C → Field D
```

y también:

campos inexistentes,
tipos incompatibles,
valores nulos,
transformaciones inválidas.

---

## 21. Data Integrity Tests

Después de una sincronización se deberá verificar que:

Source
```text
   ↓
```
Transformation
```text
   ↓
```
Destination

produce el resultado esperado.

No basta con comprobar que la operación devuelve HTTP 200.

---

## 22. Error Tests

Los errores deberán ser testeados deliberadamente.

Ejemplos:

ERP unavailable
API unavailable
Invalid credentials
Invalid data
Database unavailable
Timeout
Rate limit
Malformed response

---

## 23. Retry Tests

Los retries deberán comprobar:

Failure
```text
 ↓
```
Retry
```text
 ↓
```
Success

y:

Failure
```text
 ↓
```
Retry
```text
 ↓
```
Failure
```text
 ↓
```
Final failure

No deberá existir un retry infinito.

---

## 24. Timeout Tests

Todas las comunicaciones externas críticas deberán probar su comportamiento ante timeout.

Ejemplo:

Request
```text
 ↓
```
Timeout
```text
 ↓
```
Retry / Error

según corresponda.

---

## 25. Database Tests

Se deberán probar:

inserts,
updates,
deletes,
constraints,
relations,
transactions,
migrations.

---

## 26. Migration Tests

Antes de aplicar una migration se deberá comprobar:

Existing database
```text
       ↓
```
Migration
```text
       ↓
```
Expected schema

Las migrations no deberán destruir datos accidentalmente.

---

## 27. Transaction Tests

Cuando una operación utilice una transacción:

Operation A ✓
Operation B ✓
Operation C ✗

deberá comprobarse que el comportamiento final sea el esperado.

---

## 28. Event Tests

Se deberán probar:

Event created
Event received
Event processed
Event failed
Event retried
Event duplicated

---

## 29. Flow Tests

Un Flow deberá probar:

Trigger
```text
 ↓
```
Condition
```text
 ↓
```
Action
```text
 ↓
```
Result

También:

Trigger
```text
 ↓
```
Condition false
```text
 ↓
```
No action

---

## 30. Webhook Tests

Se deberán probar:

valid webhook,
invalid webhook,
missing signature,
invalid signature,
duplicate webhook,
malformed payload,
timeout.

---

## 31. External API Tests

No debemos depender constantemente de servicios externos reales para ejecutar todos los tests.

Se utilizarán mocks, stubs o servidores de prueba cuando sea apropiado.

---

## 32. Real Connector Tests

Los Connectors también deberán tener pruebas contra entornos reales o sandbox cuando sea necesario.

Especialmente antes de publicar una nueva versión.

---

## 33. Factusol Testing

Factusol merece especial atención debido a su arquitectura basada en Access.

Se deberán probar al menos:

Access database available
Access database unavailable
Database locked
Invalid path
Invalid schema
Large dataset
Concurrent access

---

## 34. Large Database Tests

ERP Bridge no deberá asumir que la base de datos de un cliente es pequeña.

Las pruebas deberán incluir datasets suficientemente grandes para detectar:

consumo excesivo de memoria,
queries lentas,
timeouts,
bloqueos,
procesamiento excesivo.

---

## 35. Performance Tests

No todas las funcionalidades necesitan benchmarks.

Pero deberán existir pruebas de rendimiento para operaciones críticas:

Synchronization
Large datasets
Event processing
Agent communication
API throughput

---

## 36. Load Tests

Cuando el sistema lo necesite se realizarán pruebas con:

Multiple organizations
Multiple connections
Concurrent sync jobs
Large event volumes

No se realizará load testing avanzado antes de tener un producto funcional.

---

## 37. Frontend Tests

El Dashboard deberá probar especialmente:

authentication,
navigation,
forms,
connection creation,
flow creation,
synchronization status,
error states.

---

## 38. UI E2E

Los flujos críticos deberán probarse mediante E2E.

Ejemplo:

Login
```text
 ↓
```
Connections
```text
 ↓
```
Add Connector
```text
 ↓
```
Configure
```text
 ↓
```
Save
```text
 ↓
```
Connected

---

## 39. Regression Tests

Cuando se encuentre un bug importante:

Bug
```text
 ↓
```
Fix
```text
 ↓
```
Regression Test

El test deberá permanecer en el proyecto para evitar que el error reaparezca.

---

## 40. Test Naming

Los tests deberán describir comportamiento.

Preferible:

should_create_connection_when_credentials_are_valid

en lugar de:

testConnection1

---

## 41. Test Independence

Los tests deberán ser independientes siempre que sea posible.

Un test no deberá depender del resultado de otro.

---

## 42. Test Data

Los datos de prueba deberán ser controlados y reproducibles.

No utilizar información real de clientes en tests.

---

## 43. Test Environment

ERP Bridge deberá poder disponer de un entorno específico de testing.

Conceptualmente:

Development
Testing
Staging
Production

---

## 44. Production Data

Nunca se deberán ejecutar tests destructivos contra producción.

---

## 45. Test Secrets

Los tests deberán utilizar credenciales específicas de testing.

Nunca:

Production API Key

dentro del repositorio.

---

## 46. Coverage

La cobertura será una métrica útil, pero no será el único criterio de calidad.

Un 90% de coverage con tests inútiles no significa que el sistema esté bien probado.

La prioridad será:

Cobertura significativa del comportamiento crítico.

---

## 47. Critical Paths

Las siguientes áreas tendrán prioridad:

Authentication
Authorization
Connections
Connectors
Synchronization
Events
Data integrity
Agent communication

---

## 48. Definition of Test Completion

Una funcionalidad crítica deberá tener, cuando corresponda:

✓ Unit tests
✓ Integration tests
✓ Error tests
✓ Security tests
✓ E2E test

No todas las funcionalidades requerirán los cinco niveles.

---

## 49. CI Testing

Los tests deberán ejecutarse automáticamente en CI.

Conceptualmente:

Git Push
```text
   ↓
```
Build
```text
   ↓
```
Lint
```text
   ↓
```
Unit Tests
```text
   ↓
```
Integration Tests
```text
   ↓
```
Result

---

## 50. Pull Request Protection

Los cambios importantes no deberían fusionarse si los tests críticos fallan.

---

## 51. Failed Tests

Un test que falla deberá considerarse un problema que debe resolverse.

No se permitirá:

eliminar el test,
desactivarlo,
ignorarlo,
cambiar la expectativa simplemente para hacerlo pasar,

sin una razón técnica válida documentada.

---

## 52. Flaky Tests

Los tests inestables deberán identificarse y corregirse.

Un test que falla aleatoriamente reduce la confianza en todo el sistema.

---

## 53. AI Generated Tests

Los tests generados por IA deberán revisarse igual que el código.

La IA no deberá crear tests artificiales cuyo único objetivo sea aumentar el porcentaje de coverage.

---

## 54. AI Development Loop

Cada cambio generado con IA deberá seguir:

Understand
```text
    ↓
```
Implement
```text
    ↓
```
Test
```text
    ↓
```
Run
```text
    ↓
```
Inspect failures
```text
    ↓
```
Fix
```text
    ↓
```
Run again

---

## 55. No "Test Passing" Without Verification

Un build verde no significa automáticamente que la funcionalidad sea correcta.

Cuando sea posible, deberá verificarse también el comportamiento real.

---

## 56. Manual Verification

Las funcionalidades importantes podrán requerir pruebas manuales.

Ejemplo:

Install Agent
Connect Factusol
Configure WooCommerce
Start Sync
Verify Product
Verify Stock
Create Order
Verify Order

---

## 57. MVP Testing Priority

Durante el MVP la prioridad será:

1. Core
2. Factusol Connector
3. Agent
4. WooCommerce Connector
5. Synchronization
6. Authentication
7. Dashboard critical flows

---

## 58. Testing Philosophy

Probar primero aquello que puede causar pérdida de datos, duplicación de operaciones, exposición de información o interrupción de una empresa.

---

## 59. Golden Rule

ERP Bridge debe fallar de forma visible, controlada y recuperable. Nunca silenciosamente.

---

## 60. Definition of Done

Una funcionalidad crítica de ERP Bridge se considerará terminada cuando:

✓ Código implementado
✓ Tests relevantes creados
✓ Tests ejecutados
✓ Build correcto
✓ Errores controlados
✓ Seguridad verificada
✓ Comportamiento validado
✓ Documentación actualizada

---

## 61. Objetivo final

El sistema de testing debe permitir que podamos desarrollar rápidamente sin miedo a romper lo que ya funciona.

La velocidad de desarrollo de ERP Bridge dependerá de:

Good Architecture
        +
Automated Tests
        +
CI/CD
```text
        ↓
```
Safe Fast Development

---
