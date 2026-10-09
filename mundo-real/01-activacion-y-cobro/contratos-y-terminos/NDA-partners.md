# ACUERDO DE CONFIDENCIALIDAD Y NO COMPETENCIA (NDA) PARA PARTNERS Y DISTRIBUIDORES

> **Régimen Jurídico:** Conforme a la Ley 1/2019, de 20 de febrero, de Secretos Empresariales, el Real Decreto Legislativo 1/1996 (Ley de Propiedad Intelectual) y los artículos 1.255 y concordantes del Código Civil español.  
> **Finalidad:** Presentación técnica, evaluación comercial y distribución del software **Bentian** (ERP Bridge) protegiendo el secreto empresarial, código, esquemas y bases de clientes.

---

## REUNIDOS

De una parte, **D. CRISTIAN JIMÉNEZ MARTÍNEZ**, mayor de edad, con NIF `[TU_NIF]`, actuando en su propio nombre y representación en calidad de titular legítimo, autor y propietario industrial y comercial de la solución tecnológica **Bentian** (en adelante, la **"PARTE REVELADORA"** o **"BENTIAN"**).

Y de otra parte, **D./Dña. `[NOMBRE_DEL_REPRESENTANTE]`**, mayor de edad, con NIF `[NIF_REPRESENTANTE]`, en nombre y representación de la mercantil `[RAZÓN_SOCIAL_PARTNER]` (o como profesional autónomo), con NIF/CIF `[CIF_PARTNER]`, domicilio social en `[DOMICILIO_COMPLETO_PARTNER]` e inscrita en el Registro Mercantil de `[CIUDAD_REGISTRO]` al Tomo `[TOMO]`, Folio `[FOLIO]`, Hoja `[HOJA]` (en adelante, la **"PARTE RECEPTORA"** o el **"PARTNER"**).

Ambas partes se reconocen mutuamente capacidad jurídica suficiente para suscribir el presente Acuerdo y, al efecto:

---

## EXPONEN

I. Que **BENTIAN** ha desarrollado y es titular exclusivo de la arquitectura de software, especificaciones técnicas, conectores de enlace, modelos de datos, esquemas de sincronización diferencial (Delta Sync) y código fuente y objeto relativos al producto **Bentian** (solución local-first de integración y pasarela entre sistemas de gestión ERP de escritorio como Factusol y plataformas e-commerce/APIs web).

II. Que el **PARTNER** es una empresa o profesional del sector de desarrollo web, consultoría de comercio electrónico, mantenimiento informático de sistemas o distribución de software empresarial interesada en conocer la arquitectura técnica, modelo operativo y condiciones comerciales de **Bentian** con el fin de evaluar una alianza estratégica, integración técnica o suscripción de acuerdos de distribución/reventa comercial a su cartera de clientes.

III. Que para dicha finalidad es estrictamente necesario que **BENTIAN** revele a la **PARTE RECEPTORA** determinada información de carácter técnico, metodológico, comercial y estratégico que constituye Secreto Empresarial bajo el amparo de la Ley 1/2019.

Por todo ello, las partes acuerdan formalizar el presente Acuerdo al tenor de las siguientes:

---

## CLÁUSULAS

### PRIMERA. — Definición de Información Confidencial y Secreto Empresarial
A los efectos del presente Acuerdo, tendrá la consideración de **"Información Confidencial"** y **"Secreto Empresarial"** (conforme a los artículos 1 y 2 de la Ley 1/2019) toda información técnica, operativa, financiera o comercial comunicada por **BENTIAN** al **PARTNER**, por cualquier medio (verbal, escrito, digital, acceso a repositorios, demostraciones telemáticas, código binario o archivos de configuración), incluyendo a título enunciativo y no limitativo:
1. El código fuente, bytecode, código binario y scripts empaquetados del *Local Agent* (`BentianAgent.exe`), módulos conectores y SDK.
2. La arquitectura de conexión local-first con bases de datos `.accdb` (Access/ODBC), esquemas de mapeo de tablas (`F_ART`, `F_STO`, `F_LCL`, `F_PCL`), queries SQL optimizadas y rutinas de evasión de bloqueos concurrentes.
3. El algoritmo y diseño de sincronización diferencial mediante hashing en SQLite local (`Delta Sync`), control de cambios y gestión de colas resilientes.
4. Estrategias de precios, márgenes de comisión para distribuidores, costes internos, listas de prospectos y métricas de rendimiento.
5. Los materiales formativos, manuales técnicos internos y documentación de integración no publicada abiertamente.

No tendrá la consideración de Información Confidencial aquella que: (a) fuera de dominio público con anterioridad a su divulgación sin infracción del presente Acuerdo; (b) estuviera legalmente en posesión de la Parte Receptora antes de su recepción acreditándolo por escrito; o (c) deba ser revelada por imperativo legal o resolución judicial firme, notificando previamente a **BENTIAN** para que ejerza las acciones de defensa pertinentes.

---

### SEGUNDA. — Obligación Estricta de Confidencialidad y Custodia
La **PARTE RECEPTORA** se compromete formalmente a:
1. Mantener la más estricta confidencialidad sobre toda la Información Confidencial recibida, aplicando para su custodia el mismo nivel de diligencia que aplicaría a sus propios secretos industriales y, en todo caso, no inferior a una diligencia razonable y rigurosa.
2. Utilizar dicha información única y exclusivamente para evaluar y ejecutar la relación de colaboración o distribución comercial de **Bentian**.
3. No revelar, transferir, publicar ni poner a disposición de terceros (incluyendo subcontratistas, agencias asociadas o clientes) total o parcialmente la Información Confidencial sin el consentimiento previo y por escrito de **BENTIAN**.
4. Restringir el acceso a la Información Confidencial exclusivamente a aquellos de sus empleados, socios o colaboradores que precisen conocerla directamente para los fines autorizados, advirtiéndoles expresamente del carácter confidencial y vinculándoles a compromisos equivalentes al presente.

---

### TERCERA. — Prohibición Expresa de Ingeniería Inversa y Clonación
La **PARTE RECEPTORA** se compromete de manera irrevocable a:
1. **NO descompilar, desensamblar, desofuscar, realizar ingeniería inversa** ni intentar acceder al código fuente, lógica subyacente o algoritmos propietarios del ejecutable del *Local Agent*, librerías o conectores suministrados por **BENTIAN**.
2. **NO interceptar ni clonar** las llamadas de red, protocolos de autenticación o esquemas de licencias entre el *Local Agent* y los servidores de control o plataformas e-commerce.
3. **NO extraer los esquemas de mapeo de bases de datos** de Factusol o cualquier otro ERP provisto por **BENTIAN** para volcarlos en un desarrollo propio, plugin alternativo o solución de terceros.

---

### CUARTA. — No Competencia y No Elusión (Pacto de Exclusividad Temporal)
Durante la vigencia del presente Acuerdo y por un periodo adicional de **veinticuatro (24) meses** tras su finalización o resolución:
1. **No Competencia:** La **PARTE RECEPTORA** se abstendrá de desarrollar, comercializar, colaborar directa o indirectamente en el desarrollo o promocionar una solución tecnológica de software que replique funcionalmente o compita directamente con la pasarela de integración local-first Factusol-Ecommerce de **Bentian**, basada en el conocimiento adquirido a través de la Información Confidencial revelada.
2. **No Elusión:** El **PARTNER** no contactará, negociará ni contratará de forma directa o interpuesta con clientes, proveedores o distribuidores presentados o compartidos por **BENTIAN** eludiendo la participación o el cobro de licencias de **Bentian**.

---

### QUINTA. — Propiedad Intelectual e Industrial
Toda la Información Confidencial, código, marcas, logotipos, documentación técnica y derechos de explotación comercial son y seguirán siendo propiedad exclusiva de **D. CRISTIAN JIMÉNEZ MARTÍNEZ** (**BENTIAN**). La entrega de información, accesos demo o licencias de evaluación no concede al **PARTNER** derecho de explotación, copia ni cesión sobre tales derechos, salvo los expresamente estipulados en el eventual Contrato de Distribución que pudiera suscribirse con posterioridad.

---

### SEXTA. — Cláusula Penal Disuasoria y Reparación de Daños
En caso de incumplimiento culpable o doloso de cualquiera de las obligaciones de confidencialidad, no ingeniería inversa o no competencia estipuladas en el presente Acuerdo por parte de la **PARTE RECEPTORA**, esta quedará automáticamente obligada a abonar a **BENTIAN**, en concepto de cláusula penal (artículo 1.152 del Código Civil), la cantidad a tanto alzado de **TREINTA MIL EUROS (30.000,00 €)**, sin perjuicio de:
1. La reclamación judicial por los daños y perjuicios reales, daño emergente y lucro cesante acreditado que pudieran exceder de dicha suma.
2. La adopción inmediata de medidas cautelares (incluyendo el cese inmediato de actividades comerciales, retirada de software competidor y secuestro judicial de soportes informáticos) al amparo de los artículos 12 y siguientes de la Ley 1/2019 de Secretos Empresariales.

---

### SÉPTIMA. — Devolución o Destrucción de la Información
En cualquier momento, a requerimiento de **BENTIAN** o tras la finalización de las conversaciones exploratorias sin formalización de contrato de distribución, el **PARTNER** deberá, en un plazo máximo de cinco (5) días hábiles:
1. Devolver o destruir de forma segura y permanente todos los documentos, instaladores, tokens demo, archivos y copias que contengan Información Confidencial.
2. Desinstalar de cualquier equipo de laboratorio o servidor cualquier copia en ejecución de `BentianAgent.exe`.
3. Certificar por escrito emitido por su representante legal la efectiva destrucción de dichos soportes.

---

### OCTAVA. — Duración
Las obligaciones de confidencialidad y secreto industrial pactadas en este documento permanecerán en vigor durante un plazo de **cinco (5) años** a contar desde la fecha de su firma, o con carácter indefinido respecto a aquella información que legalmente mantenga su estatus de Secreto Empresarial o derecho de autor protegido. El pacto de no competencia y no elusión mantendrá la vigencia expresa de **veinticuatro (24) meses** estipulada en la Cláusula Cuarta.

---

### NOVENA. — Ley Aplicable y Fuero Jurisdiccional
El presente Acuerdo se regirá e interpretará con arreglo a la legislación común española.

Para la resolución de cuantas controversias, divergencias o litigios pudieran suscitarse respecto a la validez, interpretación, cumplimiento o resolución del presente contrato, las partes, con renuncia expresa a cualquier otro fuero que pudiera corresponderles, se someten expresamente a la jurisdicción de los **Juzgados y Tribunales de la ciudad de `[VALENCIA / ALBACETE]` (España)**.

---

Y en prueba de conformidad y para su debido cumplimiento, las partes firman el presente documento por duplicado ejemplar y a un solo efecto, telemáticamente mediante firma electrónica cualificada o en la fecha y lugar indicados.

| Por BENTIAN (Parte Reveladora) | Por el PARTNER (Parte Receptora) |
| :--- | :--- |
| **Fdo.: D. Cristian Jiménez Martínez** | **Fdo.: `[NOMBRE_DEL_REPRESENTANTE]`** |
| Titular y Desarrollador de Bentian | Cargo: `[CARGO / REPRESENTANTE LEGAL]` |
| NIF: `[TU_NIF]` | NIF/CIF: `[CIF_PARTNER]` |
| En Valencia/Albacete, a `[DÍA]` de `[MES]` de 2026. | En `[CIUDAD]`, a `[DÍA]` de `[MES]` de 2026. |
