# BATTLECARD DE DEMOLICIÓN DE OBJECIONES
## GUÍA TÁCTICA PARA REUNIONES CON AGENCIAS, TÉCNICOS IT Y CLIENTES

> **Contexto:** Cristian Jiménez Martínez dispone de ventanas de reunión de 20 minutos (18:30 a 20:30).  
> **Objetivo:** Desmontar en 30 segundos cualquier objeción técnica o comercial, proyectar autoridad de arquitecto de software y cerrar la demo o el acuerdo de partner en la primera llamada.

---

## 1. OBJECIONES DE AGENCIAS DE DISEÑO WEB Y E-COMMERCE

---

### Objeción 1: *"Ya usamos Festeweb (FS2PS) para los clientes que nos piden Factusol."*

* **Diagnóstico del Dolor Oculto:** El director técnico o el programador de la agencia sabe que Festeweb les da problemas continuos, pero les da pereza cambiar si no ven una ventaja radical inmediata.
* **Respuesta Demoledora (Paso a Paso):**
  > *"Totalmente comprensible, Festeweb es el veterano. Pero déjame hacerte una pregunta técnica directa: **¿Cuántas veces se os ha quedado colgada la sincronización con un error 500 de `max_execution_time` de PHP cuando el cliente mete más de 3.000 productos o en campañas como Black Friday?**"*
  > 
  > *(Pausa. Esperar a que asienta o sonría).*
  > 
  > *"Ese es el problema del script PHP tradicional en la raíz del hosting: satura la memoria de Apache y bloquea las tablas de MySQL mientras tus clientes intentan comprar.  
  > **Bentian cambia las reglas:** no toca el PHP del hosting. Funciona con un motor local en SQLite que calcula hashes de cambios en milisegundos. Si el cliente tiene 10.000 productos y solo han cambiado 2 precios, solo enviamos un paquete JSON con 2 productos por la API REST oficial. Cero carga para el servidor web, cero cuelgues y catálogo ilimitado sin que os cobren más si el cliente supera las 5.000 referencias."*

---

### Objeción 2: *"Nosotros preferimos programar nuestros propios scripts a medida en Python/Node."*

* **Diagnóstico del Dolor Oculto:** El programador jefe cree que hacer un script de sincronización es tarea de 3 tardes y quiere justificar horas de desarrollo.
* **Respuesta Demoledora:**
  > *"Sé exactamente lo que se siente porque soy arquitecto de software y yo mismo construí los primeros conectores a medida. El problema no es programar la primera versión en 3 días; el problema es el año que viene:  
  > 1. Cuando WooCommerce actualice su base de datos a HPOS (High-Performance Order Storage) y cambie las tablas de pedidos.  
  > 2. Cuando Factusol actualice su esquema de tablas anual.  
  > 3. Cuando el cliente te llame un sábado por la tarde enfadado porque un cliente compró online un producto que acababa de venderse en el mostrador físico y el stock no se actualizó.  
  > 
  > **¿De verdad queréis que vuestros mejores programadores sénior pasen horas no facturables manteniendo scripts de Access y ODBC en lugar de vender tiendas online nuevas de 3.000 €?**  
  > Con Bentian os quitáis ese marrón de encima para siempre, le cobráis al cliente entre 300 € y 600 € de configuración que os quedáis al 100%, y encima os ingresamos un 25% de comisión recurrente todos los años sin mover un dedo."*

---

### Objeción 3: *"El cliente tiene un hosting barato compartido y tememos que tumbe la web."*

* **Diagnóstico del Dolor Oculto:** Miedo real al soporte y quejas del cliente si la tienda va lenta.
* **Respuesta Demoledora:**
  > *"Justamente para eso está diseñado Bentian. Los conectores antiguos hacen 'barridos masivos' leyendo toda la base de datos MySQL.  
  > **Bentian opera en 3 capas de Delta Sync local:**  
  > - Capa 1: El stock rápido se actualiza cada 2-5 minutos atacando únicamente los IDs modificados en lotes de 20 en 20 con pausas adaptativas.  
  > - Capa 2: Los precios se revisan en segundo plano.  
  > - Capa 3: El catálogo completo solo se toca si se da de alta un producto nuevo.  
  > Vuestro hosting en SiteGround, Webempresa o Raiola no notará absolutamente nada. La web volará igual que antes."*

---

## 2. OBJECIONES DE EMPRESAS DE MANTENIMIENTO IT Y REDES

---

### Objeción 4: *"No permitimos abrir puertos en el router del cliente ni exponer bases de datos a internet por riesgo de virus o ransomware."*

* **Diagnóstico del Dolor Oculto:** El responsable de sistemas teme que una intrusión infecte la red local de la empresa y le culpen a él.
* **Respuesta Demoledora:**
  > *"Coincido al 100% contigo y yo tampoco permitiría jamás abrir el puerto 3306 de MySQL ni el puerto de escritorio remoto hacia fuera.  
  > **Bentian no abre ni un solo puerto entrante en el firewall del cliente (Zero Open Ports).**  
  > El agente es un servicio de Windows que funciona igual que un navegador web: solo realiza peticiones de salida (*outbound HTTPS*) cifradas con TLS 1.3 hacia el dominio web de la tienda. Para vuestro firewall perimetral, el agente es exactamente igual que un empleado navegando por internet. Cero superficie de ataque, cero exposición de IPs locales y cero riesgo de intrusión externa."*

---

### Objeción 5: *"¿Y si el conector corrompe la base de datos de Factusol (`.accdb`) al meter pedidos online?"*

* **Diagnóstico del Dolor Oculto:** Pánico a tener que restaurar un backup de Factusol a mitad de jornada con los contables gritando.
* **Respuesta Demoledora:**
  > *"Esa es la diferencia entre un script amateur y un sistema de grado industrial:  
  > 1. **Lectura Aislada:** Las lecturas de artículos, precios y stock se realizan en modo consulta de solo lectura mediante ADODB Snapshot no bloqueante. Los administrativos pueden estar facturando en Factusol en red local que el agente no bloquea los registros ni arroja errores de `.laccdb`.  
  > 2. **Escritura Atómica de Pedidos:** La inserción de pedidos en las tablas `F_PCL` (cabeceras) y `F_LCL` (líneas) se realiza bajo transacciones atómicas (`executeTransaction`). Si por cualquier razón la red parpadea o falta un campo, se ejecuta un rollback inmediato antes de consolidar el registro. No queda jamás un pedido a medias ni registros huérfanos."*

---

### Objeción 6: *"Software DELSOL ya les ofrece migrar a Factusol Cloud oficial."*

* **Diagnóstico del Dolor Oculto:** Creen que la solución oficial de la marca siempre es mejor.
* **Respuesta Demoledora:**
  > *"Factusol Cloud es una gran opción si el cliente quiere gastarse entre 1.500 € y 2.500 € al año en suscripciones mensuales por usuario y está dispuesto a soportar la pesadilla de migrar toda su empresa a la nube.  
  > Pero la inmensa mayoría de las PYMEs tradicionales con las que trabajáis tienen su Factusol de escritorio local pagado y amortizado, les funciona a la velocidad del rayo en su red local y no quieren depender de la conexión a internet para emitir un albarán en el mostrador.  
  > **Bentian les permite conservar su Factusol local de siempre, con su velocidad de siempre, conectándolo a la tienda online por solo 249 € al año tarifa plana para toda la empresa.**"*

---

## 3. OBJECIONES DEL DUEÑO DE LA PYME O GERENTE

---

### Objeción 7: *"249 € al año me parece muy barato comparado con lo que me pedían otros... ¿dónde está la trampa?"*

* **Diagnóstico del Dolor Oculto:** Desconfianza española clásica ante un precio accesible ("lo barato sale caro").
* **Respuesta Demoledora:**
  > *"La diferencia no está en la calidad, sino en la arquitectura tecnológica.  
  > Otras empresas te cobran 1.500 € al año porque montan servidores gigantescos en la nube donde tienen que subir tu base de datos y procesarla allí, pagando miles de euros al mes a Amazon o Google.  
  > **Bentian utiliza tecnología 'Local-First':** el programa corre directamente en tu propio ordenador. Utiliza la potencia del procesador de tu servidor para hacer la sincronización, por lo que a mí no me cuesta servidores en la nube. Ese ahorro de costes de infraestructura es el que te traslado directamente a ti en el precio.  
  > Además, no hay trampa ni cartón: **tienes 14 días de prueba real sin compromiso** para ver cómo sincroniza tus productos y pedidos antes de pagar un solo euro."*

---

### Objeción 8: *"Tengo productos con variantes de tallas y colores (calzado / confección)."*

* **Diagnóstico del Dolor Oculto:** Sectores de moda y calzado (muy presentes en Elche y Almansa) con matrices complejas de atributos.
* **Respuesta Honesta y Estratégica:**
  > *"Actualmente Bentian v1.0 está optimizado al 100% para producto simple y catálogo por SKU directo (suministros, ferretería, recambios, papelería, bricolaje, alimentación, joyería, cosmética).  
  > El módulo de matrices complejas de tallas y colores (`F_TCO`) de Factusol está en fase de beta controlada para su lanzamiento en la versión 1.1 este trimestre.  
  > Si tu catálogo maneja códigos de referencia únicos por variante o producto individual, podemos sincronizarlo hoy mismo. Si requieres matriz pura multidimensional en WooCommerce, te anotamos en nuestro programa de acceso preferente beta para darte soporte prioritario en cuanto esté liberado el módulo."*

---

## 4. MATRIZ DE CIERRE RÁPIDO EN 3 MINUTOS

| Perfil del Interlocutor | Gancho Principal | Próximo Paso Inmediato (Llamada a la Acción) |
| :--- | :--- | :--- |
| **Agencia Web / E-commerce** | *"Os quitamos el marrón del soporte de Factusol + 25% recurrente anual para vosotros + cobráis vuestro setup íntegro."* | *"Os envío el instalador demo y el acuerdo de partner hoy mismo para que lo probéis en vuestro entorno de test."* |
| **Empresa Mantenimiento IT** | *"Cero puertos abiertos en el firewall, cero riesgos de seguridad, 15 minutos de instalación por máquina."* | *"Hacemos una sesión de 10 minutos por Teams donde veis el monitor de red y comprobáis que no abre puertos."* |
| **Dueño de Tienda / PYME** | *"Mantén tu Factusol de escritorio de toda la vida, olvídate de meter stock a mano y ahorra miles de euros en software."* | *"Te dejo activada la prueba de 14 días en tu ordenador hoy mismo y ves cómo suben tus productos a la tienda."* |
