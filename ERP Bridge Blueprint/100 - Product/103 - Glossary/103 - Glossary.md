# ERP Bridge Glossary — v1.0
> **Estado:** Borrador

---

# 103 - Glossary

> *Glosario de términos y definiciones clave.*

---

## Propósito

Este documento define la terminología oficial utilizada dentro de ERP Bridge.

Los términos definidos aquí deberán utilizarse de forma consistente en la documentación, arquitectura, código, interfaz y comunicación del producto.

Cuando exista una duda sobre el significado de un término, esta definición tendrá prioridad.

---

## A — Términos Generales

### Empresa
Organización que utiliza ERP Bridge para conectar y automatizar sus sistemas empresariales.

### Sistema
Cualquier software, plataforma, dispositivo o servicio que pueda participar en una integración con ERP Bridge.

*Ejemplos:*
- ERP
- CRM
- Ecommerce
- Sistema de almacén
- Plataforma de pagos
- API
- Servicio de IA

### ERP
Sistema de planificación y gestión empresarial utilizado por una empresa para gestionar operaciones como facturación, productos, clientes, proveedores, compras, ventas o inventario.

*Ejemplos:*
- Factusol
- Sage
- Odoo

### Integración
Conexión entre dos o más sistemas que permite intercambiar información o ejecutar acciones automáticamente.

*Ejemplo:*
```text
ERP → ERP Bridge → Ecommerce
```

---

## B — Arquitectura de ERP Bridge

### ERP Bridge
Plataforma de integración y automatización empresarial que permite conectar diferentes sistemas y ejecutar procesos entre ellos.

### Core / Núcleo
Parte central de ERP Bridge que proporciona las capacidades comunes de la plataforma.

El Core no debe contener lógica específica de un ERP o servicio concreto. Entre sus responsabilidades estarán:
- Gestión de conexiones.
- Ejecución de flujos.
- Gestión de eventos.
- Transformación de datos.
- Seguridad.
- Registro y auditoría.
- Gestión de errores.

### Conector
Módulo que permite a ERP Bridge comunicarse con un sistema concreto.

*Ejemplos:*
- Factusol Connector
- WooCommerce Connector
- Sage Connector
- Shopify Connector

El conector contiene la lógica específica necesaria para comunicarse con ese sistema.

### Adaptador
Componente que traduce las características específicas de un sistema al modelo común utilizado por ERP Bridge.

*Ejemplo:*
```text
Factusol
   ↓
Adaptador Factusol
   ↓
Modelo ERP Bridge
```

> **Nota:** Conector y adaptador no son necesariamente lo mismo. El conector representa la integración completa; el adaptador puede ser uno de sus componentes internos.

### Plugin
Módulo independiente que puede añadirse a ERP Bridge sin modificar el núcleo. Un conector podrá implementarse como plugin.

### Agente
Componente de ERP Bridge instalado en el entorno del cliente cuando sea necesario acceder a sistemas locales o recursos que no están disponibles directamente desde Internet.

*Ejemplo:*
```text
ERP local
    ↕
Agente ERP Bridge
    ↕
ERP Bridge
```

### Gateway
Punto de comunicación que permite intercambiar información entre un sistema y ERP Bridge.

Dependiendo de la arquitectura final, un gateway puede formar parte del agente, de un conector o de la infraestructura de ERP Bridge.

---

## C — Datos e Integración

### Fuente / Source
Sistema desde el que ERP Bridge obtiene información.

*Ejemplo:*
```text
Factusol → WooCommerce
   ↑
 Fuente
```

### Destino / Target
Sistema al que ERP Bridge envía información o sobre el que ejecuta una acción.

### Entidad
Objeto de información que puede ser gestionado por una integración.

*Ejemplos:*
- Cliente
- Producto
- Pedido
- Factura
- Stock
- Proveedor
- Cita

### Evento
Suceso que ocurre en un sistema y que puede provocar una acción en ERP Bridge.

*Ejemplo:*
```text
Nuevo pedido
      ↓
Evento
      ↓
ERP Bridge
```

### Acción
Operación que ERP Bridge ejecuta como consecuencia de un evento o de una regla.

*Ejemplos:*
- Crear cliente.
- Actualizar stock.
- Crear pedido.
- Enviar factura.
- Mandar un mensaje.

### Flujo / Flow
Secuencia de eventos, transformaciones, reglas y acciones que define un proceso automatizado.

*Ejemplo:*
```text
Nuevo pedido
     ↓
Transformar datos
     ↓
Crear pedido en ERP
     ↓
Actualizar stock
     ↓
Enviar confirmación
```

### Mapeo / Mapping
Reglas que indican cómo los datos de un sistema corresponden con los datos de otro.

*Ejemplo:*
```text
Factusol: customer.nombre
         ↓
WooCommerce: customer.first_name
```

### Transformación
Proceso mediante el cual ERP Bridge modifica los datos para adaptarlos al formato requerido por otro sistema.

### Sincronización
Proceso mediante el cual ERP Bridge mantiene información coherente entre diferentes sistemas. Puede ser:
- Unidireccional.
- Bidireccional.
- En tiempo real.
- Programada.
- Basada en eventos.

---

## D — Automatización

### Regla
Condición que determina qué debe ocurrir ante una determinada situación.

*Ejemplo:*
> Si el stock de un producto cambia en el ERP, actualizar el stock del ecommerce.

### Automatización
Proceso que ERP Bridge ejecuta sin intervención humana siguiendo una configuración previamente definida.

### Trigger
Condición o evento que inicia un flujo.

*Ejemplo:*
```text
TRIGGER: Nuevo pedido en WooCommerce
   ↓
ACTION: Crear pedido en Factusol
```

### IA
Capacidad de inteligencia artificial integrada en ERP Bridge para realizar tareas que requieren interpretación, generación, clasificación o toma de decisiones asistida.

La IA será una capacidad de la plataforma, no el propósito fundamental de ERP Bridge.

---

## E — Infraestructura y Seguridad

### Credencial
Información utilizada para autenticar ERP Bridge frente a un sistema externo (API Key, OAuth token, Usuario y contraseña, Certificado).

### Tenant
Espacio lógico independiente perteneciente a una empresa dentro de una instalación compartida de ERP Bridge. Cada tenant debe estar aislado de los demás.

### Instancia
Ejecución concreta de ERP Bridge o de uno de sus componentes.

### Log
Registro técnico de una operación realizada por el sistema.

### Auditoría
Registro estructurado de las operaciones relevantes realizadas por ERP Bridge, especialmente aquellas relacionadas con datos, usuarios, configuraciones y seguridad.

### Retry
Intento automático de repetir una operación que ha fallado.

### Idempotencia
Propiedad que permite ejecutar una misma operación varias veces sin producir efectos duplicados o inconsistentes (especialmente crítica en creación de pedidos, facturas o actualización de stock).

---

## F — Producto

### Marketplace
Catálogo desde el que los usuarios pueden descubrir, instalar y gestionar conectores y otros componentes compatibles con ERP Bridge.

### SDK
Conjunto de herramientas, librerías y especificaciones que permiten desarrollar conectores y extensiones para ERP Bridge.

### API
Interfaz que permite que sistemas externos se comuniquen programáticamente con ERP Bridge.

### Dashboard
Interfaz desde la que el usuario administra ERP Bridge (Conexiones, Conectores, Flujos, Eventos, Errores, Usuarios, Configuración, Métricas).

---

## G — Estados y Versiones

### Conector Oficial
Conector desarrollado, mantenido o validado oficialmente por el equipo de ERP Bridge.

### Conector de Terceros
Conector desarrollado por una persona o empresa externa al equipo principal de ERP Bridge.

### Versión
Identificador de una versión concreta de ERP Bridge o de uno de sus componentes. Los conectores deberán poder evolucionar independientemente del Core siempre que sea posible.

---

## Regla Importante del Glosario

> **Este glosario es un documento vivo.**
> 
> - Los términos nuevos deberán añadirse cuando aparezcan conceptos relevantes en la arquitectura o producto.
> - No se deberán utilizar términos diferentes para describir el mismo concepto sin una razón justificada.
