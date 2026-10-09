# Personas — v1.0
> **Estado:** Borrador

---

# 105 - Personas

> *Perfiles de usuario, roles y necesidades.*

---

## 1. Propósito

Este documento define los principales perfiles de usuario relacionados con ERP Bridge y sus necesidades.

Las personas servirán como referencia para diseñar el producto, la experiencia de usuario, las funcionalidades y el modelo comercial.

---

## 2. Persona Principal — Propietario / Gerente de PYME

### Perfil
Persona responsable de una pequeña o mediana empresa que utiliza uno o varios sistemas de gestión y necesita automatizar procesos.

Normalmente:
- No es programador.
- Tiene conocimientos básicos de tecnología.
- Conoce perfectamente su negocio.
- Quiere reducir trabajo manual.
- No quiere cambiar el ERP que ya utiliza.
- No quiere contratar un proyecto tecnológico complejo.

### Problemas
- Tiene información repartida entre varios programas.
- Sus empleados introducen datos manualmente.
- Tiene procesos repetitivos.
- Los sistemas no están conectados.
- Las integraciones existentes son caras.
- Depende de terceros para modificaciones.

### Necesidad Principal
Conectar sus sistemas y automatizar procesos sin tener que convertirse en experto en tecnología.

> **Expectativa de ERP Bridge:** *"Quiero conectar mis programas y olvidarme."*

---

## 3. Persona Secundaria — Responsable de Administración / Operaciones

### Perfil
Persona que utiliza diariamente el ERP y otros sistemas de la empresa (Facturación, Pedidos, Clientes, Productos, Stock, Compras, Administración).

### Problemas
Tiene que copiar información manualmente entre sistemas.

```text
Pedido online
     ↓
Copiar manualmente
     ↓
ERP
     ↓
Actualizar stock
     ↓
Enviar información
```

Esto consume tiempo y genera errores humanos constantes.

### Necesidad Principal
Que la información fluya automáticamente entre los sistemas.

> **Expectativa de ERP Bridge:** *"Quiero dejar de hacer esta tarea manual todos los días."*

---

## 4. Persona Técnica — Administrador / Técnico Informático

### Perfil
Persona responsable de la infraestructura tecnológica de la empresa o proveedor externo (Redes, Servidores, Sistemas operativos, Bases de datos, APIs).

### Problemas
- Integraciones difíciles de mantener.
- Software antiguo o local.
- Sistemas sin API.
- Problemas de conectividad.
- Dependencia de proveedores.

### Necesidad Principal
Una plataforma fiable que pueda instalar, configurar y mantener sin desarrollar cada integración desde cero.

> **Expectativa de ERP Bridge:** *"Quiero instalarlo una vez y que funcione de forma fiable."*

---

## 5. Persona Técnica Avanzada — Desarrollador / Integrador

### Perfil
Desarrollador o empresa tecnológica que necesita construir integraciones para sus clientes. Esta persona no necesariamente será el cliente inicial, pero puede convertirse en una pieza importante del ecosistema.

### Necesidades
SDK, API, Documentación, Herramientas de desarrollo, Creación de conectores, Testing, Versionado, Marketplace.

> **Expectativa de ERP Bridge:** *"Quiero poder crear un conector una vez y reutilizarlo para muchos clientes."*

---

## 6. Persona Futura — Fabricante de Software

### Perfil
Empresa que desarrolla un ERP, CRM u otro software empresarial. Puede querer ofrecer integración con ERP Bridge para que sus clientes puedan conectarse con otros sistemas.

### Necesidad
Permitir que su software forme parte de un ecosistema de integraciones sin tener que desarrollar todas las conexiones por su cuenta.

---

## 7. Persona Futura — Agencia / Integrador

### Perfil
Empresa que implementa software y automatizaciones para múltiples clientes.

### Necesidad
Implementar integraciones para múltiples clientes utilizando una plataforma común en lugar de desarrollar cada proyecto desde cero.

---

## 8. Prioridad de las Personas

| Persona | Prioridad MVP | Función |
|---|:---:|---|
| **Gerente / Propietario** | ⭐⭐⭐⭐⭐ | Decisión de compra |
| **Administración / Operaciones** | ⭐⭐⭐⭐⭐ | Uso diario |
| **Técnico informático** | ⭐⭐⭐⭐ | Instalación / Mantenimiento |
| **Desarrollador** | ⭐⭐ | Extensiones |
| **Fabricante de software** | ⭐ | Ecosistema futuro |
| **Agencia / Integrador** | ⭐ | Ecosistema futuro |

---

## 9. Decisión de Producto

ERP Bridge se diseñará inicialmente para que una PYME pueda utilizarlo **sin necesidad de conocimientos de programación**.

La existencia de herramientas técnicas, APIs y SDKs permitirá posteriormente que desarrolladores, integradores y fabricantes amplíen el ecosistema.

Existen dos experiencias claramente separadas en el sistema:

### 💼 La Experiencia del Empresario
```text
Conectar → Configurar → Activar → Funciona
```

### 🛠️ La Experiencia del Desarrollador
```text
SDK → Crear conector → Probar → Publicar → Reutilizar
```

> **No debemos mezclarlas.** El empresario no debería tener que pensar como desarrollador.

---

## 10. Regla Fundamental

> **ERP Bridge debe ocultar la complejidad técnica al usuario empresarial sin limitar las capacidades del usuario técnico.**
