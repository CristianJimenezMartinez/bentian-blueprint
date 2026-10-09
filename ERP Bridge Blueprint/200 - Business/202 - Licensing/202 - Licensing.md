# Licensing — v1.0
> **Estado:** Borrador

---

# 202 - Licensing

> *Esquema de licencias y términos comerciales.*

---

## 1. Propósito

Definir el modelo de licenciamiento de ERP Bridge, incluyendo el Core, los conectores, el SDK, las extensiones y los componentes desarrollados por terceros.

El modelo deberá favorecer la adopción del producto sin impedir su sostenibilidad comercial.

---

## 2. Principio General

ERP Bridge debe ser fácil de probar, fácil de adoptar y difícil de convertir en un producto cerrado para el usuario.

El objetivo es permitir que empresas y desarrolladores puedan experimentar con ERP Bridge sin una barrera inicial elevada.

---

## 3. Separación Modular del Producto

ERP Bridge se dividirá conceptualmente en diferentes componentes independientes:

```text
ERP Bridge
│
├── Core
├── Connectors
├── Agent
├── SDK
├── API
├── Dashboard
└── Marketplace
```

Cada componente podrá tener un modelo de licencia diferente cuando sea necesario.

---

## 4. Core (Núcleo)

El Core será el componente central de ERP Bridge, responsable de las capacidades comunes de la plataforma:
- Gestión de conexiones.
- Flujos.
- Eventos.
- Transformación.
- Errores.
- Logs.
- Seguridad.
- Ejecución.

### Hipótesis Inicial:
El Core deberá ser accesible para facilitar la adopción y permitir que ERP Bridge pueda crecer como ecosistema. 

> **Decisión:** La licencia exacta (*open source, source-available o propietaria*) no se decide todavía para evitar comprometer prematuramente el modelo de negocio.

---

## 5. Conectores

Los conectores deberán admitir diferentes esquemas:

- **Conectores Oficiales:** Gratuitos, incluidos en determinados planes o premium.
- **Conectores de Terceros:** Desarrollados por desarrolladores independientes, agencias, empresas de software o fabricantes de ERP, quienes establecerán sus condiciones comerciales dentro del marketplace.

---

## 6. Agent

El Agent es especialmente relevante para instalaciones locales:

```text
Empresa
│
├── Factusol
│
└── ERP Bridge Agent
          │
          ↓
       Internet
          │
          ↓
    ERP Bridge
```

El Agent deberá poder distribuirse de forma sencilla y ligera. Su licencia estará vinculada al modelo de ERP Bridge contratado por el cliente.

---

## 7. SDK (Software Development Kit)

El objetivo del SDK es permitir la creación de nuevos conectores:

```text
Nuevo conector ──> SDK ──> ERP Bridge
```

> **Intención:** Facilitar que terceros amplíen ERP Bridge sin requerir acceso directo al código interno del Core.

---

## 8. API

La API permitirá la interacción programática de aplicaciones externas con ERP Bridge. Su disponibilidad podrá escalonarse por niveles (*API básica, API profesional, API empresarial*).

---

## 9. Marketplace

Catálogo central para distribuir conectores y extensiones (*oficiales y de terceros, plantillas, automatizaciones*).

```text
Cliente ──> Marketplace ──> Conector ──> Desarrollador + ERP Bridge (Revenue Share)
```

---

## 10. Conectores Propietarios de Terceros

Un desarrollador podrá crear y distribuir comercialmente un conector propietario para ERP Bridge sin verse forzado a liberarlo como software libre (ej. un conector para un ERP privativo).

---

## 11. Código de Cliente (Extensiones Privadas)

Una empresa podrá desarrollar extensiones internas y personalizadas sobre ERP Bridge para resolver necesidades propias sin obligación de publicarlas:

```text
Empresa A ──> ERP Bridge ──> Conector interno personalizado
```

---

## 12. Propiedad Intelectual

Cada parte del ecosistema mantendrá claramente delimitada su propiedad intelectual:

- **ERP Bridge:** Core, componentes oficiales, conectores oficiales, SDK oficial e infraestructura oficial.
- **Terceros:** Propiedad sobre sus propios conectores, extensiones e integraciones propietarias.

---

## 13. Principio de Interoperabilidad (Anti-Vendor Lock-in)

ERP Bridge no utilizará el licenciamiento para bloquear a una empresa o impedir que abandone la plataforma. El cliente mantendrá control total sobre:
- Sus datos.
- Sus configuraciones.
- Sus sistemas.

> **La plataforma debe ganar por utilidad, nunca por bloqueo forzado.**

---

## 14. "Gratis" no significa "Sin Modelo de Negocio"

Es fundamental diferenciar claramente los conceptos:

- **Gratis para utilizar:** Uso de software sin pago monetario.
- **Open Source:** Código inspeccionable, modificable y redistribuible bajo términos legales específicos.
- **Freeware:** Uso gratuito con código fuente propietario/cerrado.
- **Freemium:** Capa base gratuita y módulos/capacidades avanzadas de pago.

---

## 15. Hipótesis Inicial de Licenciamiento

```text
                    ERP Bridge
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
      Core            Conectores          SDK
       │                 │                 │
  Accesible          Free/Premium       Gratuito/
                                       Profesional
       │
       └─────────────────┬─────────────────┘
                         │
                    Marketplace
                         │
                   Terceros
```

*(Se evaluará más adelante si el Core adopta licencias tipo MIT, Apache 2.0, AGPL, BSL, Source-available o Propietaria).*

---

## 16. Objetivo Estratégico del Licenciamiento

1. **Adopción:** Que probar y poner en marcha ERP Bridge sea inmediato y sin fricción.
2. **Protección:** Que el Core no pueda ser explotado por un competidor sin aportar al ecosistema.
3. **Ecosistema:** Incentivar activamente a terceros para construir conectores.
4. **Monetización:** Habilitar vías de ingresos sólidas (*conectores, cloud, soporte, marketplace, licencias corporativas*).

---

## 17. Decisión de Licencia — Recomendación

Tras analizar el modelo de negocio, la estrategia comercial y los precedentes de mercado, la recomendación es:

### Licencia por Componente

| Componente | Licencia Recomendada | Justificación |
|---|---|---|
| **Core** | **BSL 1.1** (Business Source License) | Código fuente visible desde el día 1. Conversión automática a Apache 2.0 tras 3 años. Protege contra competidores que copien el Core sin aportar. Usado por CockroachDB, Sentry, MariaDB. |
| **SDK** | **MIT** | Máxima adopción. Los desarrolladores necesitan cero fricción legal para crear conectores. |
| **Conectores Oficiales** | **BSL 1.1** o **Propietaria** según el plan | Los conectores gratuitos pueden ser BSL. Los premium, propietarios. |
| **Agent** | **Propietaria** (binario distribuible) | Se distribuye como ejecutable. El código del Agent está vinculado al plan contratado. |
| **Dashboard** | **Propietaria** | Interfaz web servida desde la infraestructura de ERP Bridge. |
| **Conectores de Terceros** | **A elección del desarrollador** | Cada desarrollador decide la licencia de sus conectores. |

### Por qué BSL 1.1 para el Core

La BSL (Business Source License) cumple los 4 objetivos del licenciamiento:

1. **Adopción:** El código fuente es visible e inspeccionable desde el primer día. Cualquier empresa puede leerlo, auditarlo y verificar la seguridad.
2. **Protección:** Ningún competidor puede tomar el Core y ofrecer un servicio comercial competidor sin autorización.
3. **Ecosistema:** Los desarrolladores pueden estudiar el Core para construir conectores mejor integrados.
4. **Monetización:** Las vías de ingresos (suscripciones, conectores premium, servicios) quedan protegidas.
5. **Confianza a largo plazo:** La conversión automática a Apache 2.0 tras 3 años garantiza que el código nunca quedará atrapado si el proyecto se abandona.

### Parámetros BSL recomendados

- **Change Date:** 3 años desde la publicación de cada versión.
- **Change License:** Apache License 2.0.
- **Additional Use Grant:** Uso permitido para desarrollo, testing, uso personal y contribuciones al ecosistema de ERP Bridge.

> **Esta decisión podrá revisarse antes del lanzamiento público, pero constituye la hipótesis de trabajo recomendada.**

---

## 18. Modelo Multi-Clave de Puestos (Multi-Seat Keys) y Vinculación HWID

Para clientes corporativos o empresas con múltiples puestos de trabajo físicos (ej. 2 almacenes, 3 tiendas o múltiples ordenadores de facturación con Factusol), ERP Bridge implementa un modelo de **Multi-Seat Licencing**:

```text
Suscripción Corporativa Stripe (1 Cuenta Cliente)
│
├── Cupo de Puestos: 3 Puestos (max_license_seats = 3)
│
├── Clave Puesto 1: EB-MAD01-XXXXX (Alias: "Almacén Central Madrid") ──> HWID vinculada: 5c1f... [ONLINE]
├── Clave Puesto 2: EB-BCN02-XXXXX (Alias: "Tienda Barcelona")       ──> HWID vinculada: a89d... [ONLINE]
└── Clave Puesto 3: EB-VAL03-XXXXX (Alias: "Portátil Gerencia")      ──> HWID vinculada: 3e7c... [OFFLINE]
```

### Principios del Modelo Multi-Seat:
1. **Generación Criptográfica Centralizada:** Todas las claves de puesto siguen el formato estándar `EB-[A-Z0-9]{5}-[A-Z0-9]{5}-[A-Z0-9]{5}-[A-Z0-9]{5}` y se generan herméticamente en el backend (`apps/api`) validando el cupo permitido por la suscripción en Stripe.
2. **Vinculación por Hardware ID (HWID):** Al activar una clave en el equipo local, el agente genera un hash criptográfico SHA-256 de las características físicas de la máquina (BIOS UUID, CPU serial, Motherboard, MAC address). La primera activación vincula permanentemente la clave a ese equipo físico.
3. **Desvinculación y Transferencia (Self-Service Unbinding):** Si un cliente renueva su ordenador o formatea su disco duro, puede entrar al Dashboard Cloud (`https://bridge.cristianjm.com/dashboard/`) y pulsar "Desvincular Equipo (Unbind HWID)". Esto libera la clave para que pueda ser activada en la nueva máquina sin necesidad de soporte técnico manual.
4. **Tolerancia Offline (Offline Grace Period):** Si la empresa sufre un corte de fibra o caída de conexión a Internet, el agente local almacena un token firmado con un período de gracia de 168 horas (7 días) garantizando que Factusol y las sincronizaciones locales no se detengan repentinamente.

---

## 19. Regla Final

> **El licenciamiento debe facilitar que ERP Bridge se convierta en un estándar de integración sin destruir la capacidad de mantener un negocio sostenible alrededor del producto.**
