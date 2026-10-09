# ACUERDO DE ENCARGO DEL TRATAMIENTO DE DATOS (DPA)
## ARQUITECTURA "LOCAL-FIRST" Y RESPETO RADICAL DE LA PRIVACIDAD

> **Marco Normativo:** Conforme al Artículo 28 del Reglamento General de Protección de Datos (RGPD UE 2016/679) y los artículos 33 y concordantes de la Ley Orgánica 3/2018 (LOPDGDD).  
> **Ámbito de Aplicación:** Licencia de uso, soporte técnico y operación del software **Bentian** (ERP Bridge) instalado en la infraestructura local del cliente.

---

## REUNIDOS

De una parte, **D. CRISTIAN JIMÉNEZ MARTÍNEZ**, titular y comercializador del software **Bentian**, con NIF `[TU_NIF]` y domicilio a efectos de notificaciones en `[TU_DOMICILIO]`, España (en adelante, el **"PROVEEDOR"** o **"BENTIAN"**).

Y de otra parte, **`[RAZÓN_SOCIAL_CLIENTE_O_PARTNER]`**, con CIF `[CIF_CLIENTE]`, domicilio social en `[DOMICILIO_CLIENTE]` y representada por `[NOMBRE_REPRESENTANTE]`, con NIF `[NIF_REPRESENTANTE]` (en adelante, el **"RESPONSABLE DEL TRATAMIENTO"** o el **"CLIENTE"**).

Ambas partes acuerdan formalizar el presente Contrato de Encargo del Tratamiento vinculado a la prestación del servicio de software según las siguientes:

---

## CLÁUSULAS

### PRIMERA. — Naturaleza de la Arquitectura "Local-First" y No Exfiltración de Datos
1. **Inexistencia de Custodia Cloud de Bases de Datos Fiscales:** El CLIENTE reconoce y declara expresamente que ha contratado una solución de software con arquitectura **"Local-First"**. 
2. A diferencia de los modelos tradicionales de integración en la nube (iPaaS/SaaS centralizados), **el software Bentian (`BentianAgent.exe`) se ejecuta en modo local sobre el propio servidor, ordenador personal o máquina virtual del CLIENTE**, donde residen sus bases de datos ERP (Factusol / Access / ODBC).
3. **Flujo Directo y Cifrado:** Las operaciones de lectura de catálogos, sincronización de existencias de stock y descarga de pedidos se realizan de forma directa y bidireccional entre el agente local del CLIENTE y su propia plataforma de comercio electrónico (WooCommerce, PrestaShop, Shopify o API web) a través de conexiones cifradas punto a punto mediante protocolo **TLS 1.3 / HTTPS**.
4. **BENTIAN NO almacena, no centraliza, no replica, no indexa ni aloja en servidores propios ni de terceros ninguna copia de las bases de datos contables, fiscales, albaranes ni los datos personales de clientes finales o empleados del CLIENTE.**

---

### SEGUNDA. — Objeto y Datos Técnicos Tratados por Bentian
El presente encargo se limita exclusivamente al tratamiento accesorio e incidental necesario para garantizar la activación de la licencia, la estabilidad técnica del software y, en su caso, la prestación puntual de soporte técnico:

1. **Datos de Licenciamiento y Telemetría Técnica (Tratados por BENTIAN):**
   - Identificador único de instalación y huella de hardware ofuscada mediante hash criptográfico (HWID).
   - Clave de licencia alfanumérica y estado de suscripción (Stripe ID).
   - Dirección IP pública de conexión para verificación de sesión y prevención de abusos de licencia.
   - Registros de depuración técnica (*telemetry heartbeat*): versión del agente, estado del servicio (online/offline), recuento de lotes sincronizados y códigos de error HTTP de la plataforma e-commerce (sin contenido de datos personales).

2. **Datos Personales en Tránsito en la Infraestructura del Cliente (Bajo Control Exclusivo del CLIENTE):**
   - Datos identificativos y de contacto de compradores del e-commerce (nombre, apellidos, dirección de entrega, DNI/NIF para facturación, teléfono, correo electrónico).
   - Dichos datos son procesados exclusivamente en la memoria RAM y almacenamiento local del equipo del CLIENTE mediante llamadas a la API de WooCommerce/PrestaShop y sentencias SQL parametrizadas directas sobre Factusol, sin intervención de servidores intermediarios de BENTIAN.

---

### TERCERA. — Obligaciones del Proveedor (Encargado del Tratamiento)
En la medida en que la prestación de soporte técnico o la validación de licencias suponga algún tratamiento de datos personales, **BENTIAN** se compromete a:
1. Tratar los datos personales únicamente conforme a las instrucciones documentadas del CLIENTE.
2. Garantizar que las personas autorizadas para atender incidencias técnicas o soporte se hayan comprometido formalmente a respetar la confidencialidad técnica y deontológica.
3. No utilizar los datos para fines distintos a los estipulados en la prestación de la licencia del software.
4. Implementar las medidas técnicas y organizativas necesarias para garantizar un nivel de seguridad adecuado al riesgo (Cláusula Quinta).
5. Asistir al CLIENTE, en la medida de sus posibilidades técnicas, en dar respuesta a las solicitudes de ejercicio de derechos de los interesados (derechos de acceso, rectificación, supresión, limitación, portabilidad y oposición).
6. Suprimir o anonimizar los identificadores de telemetría una vez finalizada la relación contractual y el periodo de expiración de la licencia.
7. Poner a disposición del CLIENTE la información razonable necesaria para demostrar el cumplimiento del RGPD en relación con el software.

---

### CUARTA. — Subencargados del Tratamiento Autorizados
El CLIENTE autoriza de forma general la contratación de los siguientes subencargados técnicos de infraestructura auxiliar, seleccionados bajo estrictos estándares de seguridad y con garantías adecuadas de transferencia internacional:

| Subencargado | Servicio Prestado | Ubicación / Garantía |
| :--- | :--- | :--- |
| **Stripe Technology Europe, Ltd.** | Pasarela de cobro, gestión de suscripciones recurrentes y facturación telemática. | Dublín, Irlanda (Espacio Económico Europeo - EEE). |
| **Cloudflare Inc. / Vercel Inc.** | Red de entrega de contenido (CDN), protección anti-DDoS y servidor de validación de tokens de licencia (*Edge Functions*). | Cláusulas Contractuales Tipo (CCT) de la Comisión Europea / Marco de Privacidad de Datos UE-EE.UU. |

Cualquier incorporación o sustitución de subencargados será notificada a través del portal de actualizaciones o notas de versión del software con al menos quince (15) días de antelación.

---

### QUINTA. — Medidas Técnicas de Seguridad Implementadas
**BENTIAN** incorpora de fábrica en la arquitectura de `BentianAgent.exe` las siguientes salvaguardas de seguridad por diseño y por defecto (*Privacy by Design and by Default*):

1. **Cero Puertos Abiertos (Zero Open Ports):** El agente opera exclusivamente mediante conexiones de salida (*outbound*). No requiere abrir puertos en el router del cliente (como el puerto 3306 de MySQL o 1433 de SQL Server), eliminando por completo la superficie de ataque frente a ransomware o intrusiones de red perimetral.
2. **Cifrado en Tránsito:** Toda la transmisión de datos hacia la tienda e-commerce y hacia la API de licencias se ejecuta bajo túneles TLS 1.3 con certificados SSL/TLS con cipher suites modernos (AES-GCM / ChaCha20).
3. **Cifrado Local de Credenciales:** Las claves de API del comercio electrónico (Consumer Key / Consumer Secret de WooCommerce o Web Service Key de PrestaShop) se almacenan en el equipo local del cliente mediante cifrado AES-256 utilizando la API de protección de datos de Windows (DPAPI).
4. **Almacenamiento Local Efímero en SQLite:** Los índices de control de cambios (*Delta Sync Hashes*) se gestionan en una base de datos SQLite local en disco del cliente, que almacena únicamente hashes SHA-256 de las referencias y stocks, impidiendo la reconstitución inversa de datos fiscales por terceros.

---

### SEXTA. — Notificación de Brechas de Seguridad
En caso de que **BENTIAN** tuviera conocimiento confirmado de una brecha de seguridad que afecte a la infraestructura del servidor de licencias o a los componentes del software que comprometa datos del CLIENTE, lo notificará al CLIENTE por correo electrónico sin dilación indebida y, en cualquier caso, dentro de un plazo máximo de **cuarenta y ocho (48) horas**, detallando la naturaleza del incidente y las medidas correctivas adoptadas.

---

### SÉPTIMA. — Asistencia en Auditorías y Régimen de Responsabilidad
1. El CLIENTE, en su calidad de Responsable del Tratamiento, mantiene la obligación de recabar el consentimiento de sus propios clientes en su plataforma e-commerce y publicar su correspondiente Política de Privacidad conforme a la LOPDGDD.
2. El software **Bentian** se suministra con la consideración técnica de herramienta de procesamiento local. La responsabilidad de **BENTIAN** por reclamaciones relativas a la normativa de protección de datos se regirá por los límites pactados en los Términos de Licencia (EULA), quedando excluida cualquier sanción originada por la negligencia del CLIENTE en la configuración de sus servidores, falta de copias de seguridad de Factusol o custodia indebida de contraseñas locales.

---

### OCTAVA. — Legislación y Jurisdicción
El presente Contrato de Encargo del Tratamiento se regirá por la normativa española y de la Unión Europea en materia de protección de datos de carácter personal. Cualquier controversia será resuelta por los Juzgados y Tribunales competentes según el contrato principal de licencia de Bentian.

---

| Por BENTIAN (Encargado del Tratamiento) | Por el CLIENTE (Responsable del Tratamiento) |
| :--- | :--- |
| **Fdo.: D. Cristian Jiménez Martínez** | **Fdo.: `[NOMBRE_REPRESENTANTE]`** |
| NIF: `[TU_NIF]` | NIF/CIF: `[CIF_CLIENTE]` |
| Razón Social: Bentian Software | Empresa: `[RAZÓN_SOCIAL_CLIENTE]` |
| En Valencia/Albacete, a `[DÍA]` de `[MES]` de 2026. | En `[CIUDAD]`, a `[DÍA]` de `[MES]` de 2026. |
