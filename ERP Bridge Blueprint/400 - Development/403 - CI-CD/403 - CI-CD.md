# CI/CD — v1.0
> **Estado:** Borrador

---

# 403 - CI-CD

> *Pipelines de integración y despliegue continuo, builds reproducibles y empaquetado seguro.*

---

## 1. Propósito

Definir el proceso automatizado mediante el cual ERP Bridge será validado, construido y desplegado.

El objetivo es que cada cambio importante pase automáticamente por una serie de comprobaciones antes de poder llegar a producción.

---

## 2. Principio fundamental

Ningún cambio debe llegar a producción simplemente porque "funciona en mi ordenador".

Todo cambio deberá pasar por un proceso reproducible de:

Code
```text
 ↓
```
Validation
```text
 ↓
```
Tests
```text
 ↓
```
Build
```text
 ↓
```
Deploy
```text
 ↓
```
Verification

---

## 3. Objetivos

El sistema CI/CD debe proporcionar:

Builds reproducibles.
Tests automáticos.
Validación de código.
Detección temprana de errores.
Despliegues controlados.
Posibilidad de rollback.
Trazabilidad de versiones.

---

## 4. CI vs CD

Continuous Integration

Comprueba automáticamente los cambios.

Git Push
```text
 ↓
```
Install
```text
 ↓
```
Lint
```text
 ↓
```
Type Check
```text
 ↓
```
Tests
```text
 ↓
```
Build
Continuous Delivery / Deployment

Gestiona la entrega del software:

Validated Build
```text
 ↓
```
Staging
```text
 ↓
```
Production

---

## 5. Git como fuente de verdad

El repositorio Git será la fuente principal del código.

No deberán existir modificaciones permanentes realizadas únicamente en servidores.

---

## 6. Branch principal

La rama principal será:

main

main deberá representar una versión estable del proyecto.

---

## 7. Feature Branches

Las funcionalidades importantes podrán desarrollarse en ramas:

feature/*

Ejemplos:

feature/connector-sage
feature/sync-engine
feature/dashboard-connections

---

## 8. Fix Branches

Los errores podrán desarrollarse en:

fix/*

Ejemplo:

fix/factusol-timeout

---

## 9. Commits

Los commits deberán ser pequeños y descriptivos.

Ejemplos:

feat: add connector registry
fix: handle factusol connection timeout
test: add sync idempotency tests
refactor: isolate connector contract
docs: update agent architecture

---

## 10. Pull Requests

Los cambios importantes deberán poder pasar por Pull Request.

Una Pull Request deberá incluir:

What changed
Why
Tests
Potential risks

---

## 11. CI Pipeline

Cada Pull Request deberá ejecutar como mínimo:

Checkout
```text
 ↓
```
Install dependencies
```text
 ↓
```
Lint
```text
 ↓
```
Type check
```text
 ↓
```
Unit tests
```text
 ↓
```
Integration tests
```text
 ↓
```
Build

---

## 12. Pipeline Failure

Si cualquiera de las comprobaciones críticas falla:

Pipeline
```text
   ↓
```
FAILED

el cambio no deberá considerarse válido.

---

## 13. Lint

El pipeline deberá ejecutar el linter del proyecto.

El objetivo será detectar:

errores,
código inconsistente,
imports incorrectos,
patrones problemáticos.

---

## 14. Type Check

El proyecto deberá comprobar los tipos TypeScript sin generar código cuando sea posible.

Conceptualmente:

tsc --noEmit

---

## 15. Tests

El pipeline deberá ejecutar automáticamente los tests definidos en:

402 - Testing

---

## 16. Build

El proyecto deberá compilarse automáticamente.

El build deberá realizarse desde un entorno limpio.

Esto evita depender de:

archivos temporales,
dependencias globales,
configuraciones locales,
modificaciones manuales.

---

## 17. Dependency Installation

Las dependencias deberán instalarse desde los archivos versionados del proyecto.

No depender de paquetes instalados globalmente.

---

## 18. Lockfile

El proyecto deberá utilizar un lockfile para mantener versiones reproducibles.

Ejemplo:

package-lock.json

o la alternativa elegida para el proyecto.

---

## 19. Node Version

La versión de Node.js deberá estar definida explícitamente.

Ejemplo conceptual:

Node 22 LTS

La versión definitiva se establecerá durante la implementación.

---

## 20. Environment Variables

Las variables de entorno necesarias deberán estar documentadas.

Ejemplo:

DATABASE_URL
API_URL
JWT_SECRET

Los valores reales no deberán almacenarse en Git.

---

## 21. Secrets

Los secretos del pipeline deberán utilizar el sistema de secrets del proveedor CI/CD.

Nunca:

SECRET=abc123

dentro del repositorio.

---

## 22. Development Environment

El entorno local será utilizado para:

Development
Debugging
Manual Testing

---

## 23. Testing Environment

Deberá existir un entorno separado para pruebas cuando el proyecto lo requiera.

Development
```text
    ↓
```
Testing

---

## 24. Staging

Antes de producción, las funcionalidades importantes podrán desplegarse en:

Staging

Staging deberá aproximarse a producción tanto como sea razonablemente posible.

---

## 25. Production

Producción deberá recibir únicamente builds validados.

main
```text
 ↓
```
CI
```text
 ↓
```
Build
```text
 ↓
```
Verification
```text
 ↓
```
Production

---

## 26. Deployment Strategy

Durante el MVP se utilizará una estrategia de despliegue sencilla.

No se introducirá infraestructura compleja sin necesidad.

---

## 27. Frontend Deployment

El Dashboard podrá desplegarse mediante una plataforma de hosting adecuada para aplicaciones Angular.

La tecnología concreta se decidirá en:

404 - Deployment

---

## 28. Backend Deployment

El backend Node.js deberá ejecutarse en infraestructura adecuada para procesos persistentes y APIs.

La plataforma concreta se definirá en:

404 - Deployment

---

## 29. Agent Distribution

El Agent será diferente al backend.

Debe poder distribuirse para instalación en los sistemas de los clientes.

El proceso deberá contemplar:

Build
```text
 ↓
```
Package
```text
 ↓
```
Version
```text
 ↓
```
Distribution
```text
 ↓
```
Installation

---

## 30. Agent Versioning

Cada Agent deberá tener una versión explícita.

Ejemplo:

1.0.0
1.1.0
1.1.1

---

## 31. Agent Compatibility

El servidor deberá poder conocer qué versión del Agent está conectada.

Esto permitirá gestionar compatibilidad futura.

---

## 32. Database Migrations

Las migrations deberán formar parte del proceso de despliegue.

Conceptualmente:

Deploy
```text
 ↓
```
Check Migration
```text
 ↓
```
Apply Migration
```text
 ↓
```
Start Application

Las migrations deberán ejecutarse de forma segura.

---

## 33. Migration Safety

Las migrations críticas deberán poder probarse previamente.

No se deberá ejecutar una migration destructiva en producción sin verificar previamente sus consecuencias.

---

## 34. Rollback

ERP Bridge deberá tener una estrategia para volver a una versión anterior cuando un despliegue provoque un problema.

Conceptualmente:

Version 1.4
```text
 ↓
```
Deploy 1.5
```text
 ↓
```
Problem
```text
 ↓
```
Rollback
```text
 ↓
```
Version 1.4

---

## 35. Database Rollback

No todas las migrations pueden revertirse automáticamente.

Las migrations destructivas deberán diseñarse con especial cuidado.

Siempre que sea posible:

Additive Change

antes que:

Destructive Change

---

## 36. Versioning

ERP Bridge deberá mantener versiones claras.

Se utilizará Semantic Versioning cuando sea apropiado:

MAJOR.MINOR.PATCH

Ejemplo:

1.4.2

---

## 37. Release

Una release deberá representar un estado conocido del sistema.

Debe poder identificarse:

Version
Commit
Build
Date

---

## 38. Release Notes

Los cambios importantes deberán quedar documentados.

Ejemplo:

ERP Bridge v0.5.0


Added:
- WooCommerce Connector


Changed:
- Sync Engine


Fixed:
- Factusol timeout

---

## 39. CI/CD for Connectors

La incorporación de un nuevo Connector deberá activar automáticamente:

Lint
```text
 ↓
```
Type Check
```text
 ↓
```
Connector Contract Tests
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
Build

---

## 40. CI/CD for Agent

El Agent deberá comprobar:

Build
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
Package

---

## 41. CI/CD for Frontend

El Dashboard deberá comprobar:

Install
```text
 ↓
```
Lint
```text
 ↓
```
Type Check
```text
 ↓
```
Unit Tests
```text
 ↓
```
Build

Los E2E críticos podrán ejecutarse posteriormente.

---

## 42. CI/CD for Database

Las migrations deberán comprobarse automáticamente cuando sea posible.

Fresh Database
```text
 ↓
```
Apply All Migrations
```text
 ↓
```
Run Tests

Esto garantiza que el proyecto pueda reconstruir correctamente la base de datos.

---

## 43. Build Artifacts

Los builds importantes deberán producir artefactos identificables.

Ejemplo:

Backend Build
Frontend Build
Agent Package

---

## 44. Artifact Traceability

Un artefacto deberá poder relacionarse con:

Git Commit
Version
Build

---

## 45. Automated Deployment

Cuando el sistema esté preparado, un merge aprobado en main podrá iniciar automáticamente el despliegue.

Conceptualmente:

Merge
```text
 ↓
```
CI
```text
 ↓
```
Build
```text
 ↓
```
Deploy
```text
 ↓
```
Health Check

---

## 46. Health Checks

Los servicios deberán proporcionar mecanismos para determinar si están funcionando correctamente.

Ejemplo:

GET /health

Respuesta conceptual:

{
  "status": "ok"
}

---

## 47. Deployment Verification

Después del despliegue se deberá comprobar:

Application running
Database available
API responding
Agent communication available
Critical services healthy

---

## 48. Failed Deployment

Si el despliegue falla:

Deployment
```text
 ↓
```
FAILED

deberá evitarse presentar la versión como correctamente desplegada.

---

## 49. Monitoring

El despliegue deberá poder complementarse con:

logs,
métricas,
health checks,
alertas.

La solución concreta se definirá posteriormente.

---

## 50. No Manual Production Changes

No se deberán realizar modificaciones manuales permanentes en producción como sustituto del proceso de despliegue.

Si se necesita una intervención manual excepcional, deberá documentarse.

---

## 51. Database Backups

La base de datos de producción deberá contar con backups adecuados.

El mecanismo concreto se definirá en:

404 - Deployment

---

## 52. Disaster Recovery

ERP Bridge deberá poder recuperarse de:

Database failure
Server failure
Deployment failure
Credential failure

La estrategia se irá desarrollando conforme aumente la criticidad del producto.

---

## 53. MVP CI/CD

Durante el MVP se priorizará:

Git
```text
 ↓
```
CI
```text
 ↓
```
Lint
```text
 ↓
```
Type Check
```text
 ↓
```
Tests
```text
 ↓
```
Build

No se implementará una infraestructura DevOps excesivamente compleja.

---

## 54. v1 CI/CD

En v1 se podrá evolucionar hacia:

Pull Request
```text
 ↓
```
CI
```text
 ↓
```
Tests
```text
 ↓
```
Build
```text
 ↓
```
Staging
```text
 ↓
```
Approval
```text
 ↓
```
Production

---

## 55. Future CI/CD

Cuando ERP Bridge alcance mayor escala podrá incorporar:

despliegues avanzados,
múltiples regiones,
canary releases,
blue/green deployments,
autoscaling,
observabilidad avanzada.

Estas funcionalidades no forman parte del MVP.

---

## 56. AI Development

Cuando Antigravity/Gemini modifique el proyecto deberá seguir:

Read Architecture
```text
 ↓
```
Read Coding Standards
```text
 ↓
```
Implement
```text
 ↓
```
Run CI-equivalent checks
```text
 ↓
```
Fix failures
```text
 ↓
```
Report result

---

## 57. AI Restrictions

La IA no deberá:

desactivar tests para conseguir un build verde,
eliminar validaciones,
modificar CI para ocultar errores,
cambiar Node sin justificación,
introducir dependencias críticas sin autorización,
modificar infraestructura de producción sin instrucciones explícitas.

---

## 58. Local CI

Antes de realizar un Pull Request, el desarrollador debería poder ejecutar localmente los principales checks:

Lint
Type Check
Tests
Build

Esto reduce ciclos innecesarios de CI.

---

## 59. Definition of Done

Un cambio estará preparado para desplegar cuando:

✓ Code committed
✓ Lint passed
✓ Type check passed
✓ Tests passed
✓ Build passed
✓ Security checks passed when applicable
✓ Migration verified when applicable
✓ Deployment verified

---

## 60. Golden Rule

El pipeline debe impedir que la velocidad de desarrollo se convierta en velocidad para romper el producto.

---

## 61. Objetivo final

ERP Bridge deberá evolucionar hacia un proceso donde:

Cristian / AI
```text
      ↓
```
      Git
```text
      ↓
```
     CI/CD
```text
      ↓
```
 Automated Validation
```text
      ↓
```
     Deploy
```text
      ↓
```
 Production

con la mínima intervención manual posible.

---
