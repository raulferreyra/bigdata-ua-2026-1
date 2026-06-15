# GUÍA DE TRABAJO — SEMANA 1
## Big Data (DD283) | Universidad Autónoma del Perú

**Nombre(s)**: Jamphier paipay Angeles 
**Grupo de proyecto**: _______________________________________________  
**Fecha de entrega**: Antes de la sesión de la Semana 2  
**Modalidad**: Individual  
**Puntaje**: 20 puntos (2 puntos por pregunta)

---

> **Instrucciones**: Responde cada pregunta con tus propias palabras. No copies y pegues definiciones de internet — el objetivo es que construyas TU comprensión del tema. Se valorará la conexión con ejemplos reales de tu entorno laboral.

---

## PARTE 1: CONCEPTOS FUNDAMENTALES DE BIG DATA (10 preguntas)

### Pregunta 1
Define Big Data con tus propias palabras. ¿Cuál es la diferencia fundamental entre Big Data y una base de datos tradicional como SQL Server o MySQL que probablemente usas en tu empresa?

_Respuesta_:  
Big Data es un conjunto de tecnologías que permiten almacenar y analizar enormes volúmenes de datos que se generan muy rápido y en distintos formatos, algo que una computadora o base de datos tradicional no puede manejar eficientemente.

La diferencia principal es que las bases de datos tradicionales guardan la información en un único servidor y usan estructuras fijas (tablas), mientras que Big Data distribuye los datos entre varias computadoras, facilita el crecimiento agregando más equipos y permite almacenar información sin estructurar, como imágenes, textos y audios.

---

### Pregunta 2
Explica las **5 V's del Big Data** con un ejemplo de tu propia empresa o de una empresa peruana que conozcas. Completa la siguiente tabla:

| V | Definición con tus palabras | Ejemplo de tu empresa/empresa conocida |
|---|---------------------------|---------------------------------------|
| **Volumen** | Es la cantidad inmensa de datos acumulados que llenan los discos duros. | El archivo histórico de todas las recetas médicas e historias clínicas de los pacientes de un hospital a lo largo de los años. |
| **Velocidad** | Es la rapidez con la que entran y se procesan los datos en el momento. | Las alertas por mensajes de texto automáticos que envía el sistema hospitalario para recordar citas médicas por segundo. |
| **Variedad** | Son los diferentes tipos de archivos que se reciben (no solo tablas). | Guardar a la vez tablas de pacientes, archivos de texto con las notas de los doctores y documentos XML para firmas digitales. |
| **Veracidad** | Es asegurarse de que los datos no tengan errores o estén incompletos. | Revisar que los códigos de las enfermedades estén bien escritos antes de mandar el reporte final al Ministerio de Salud. |
| **Valor** | Es la utilidad real que tiene la información para mejorar el negocio o servicio. | Analizar las horas con más colas en el hospital para poner más médicos a esa hora y atender más rápido a la gente. |

---

### Pregunta 3
¿Por qué una empresa como BCP (Banco de Crédito del Perú) NO podría usar solo una base de datos Oracle tradicional para procesar todos sus datos de transacciones en tiempo real? Menciona al menos 3 razones técnicas y 1 razón de negocio.

_Respuesta_:  
**Razones Técnicas:**
1. **Saturación por clientes usando la app (Bloqueos):** Al haber millones de personas usando Yape o la banca móvil al mismo tiempo, el sistema tradicional tendría que congelar cuentas por milisegundos para actualizar los saldos, lo que haría que la aplicación se ponga sumamente lenta o se caiga.
2. **Costo de servidores:** Hacer que un solo servidor crezca para aguantar los clics de millones de peruanos es físicamente imposible o costaría millones de dólares.
3. **Datos desordenados:** Las transacciones modernas guardan la ubicación GPS, datos del celular y fotos de reconocimiento facial. Una base de datos tradicional se vuelve muy lenta si intenta procesar esto al instante.

**Razón de Negocio:**
Si el sistema del banco se vuelve lento o se cae por no usar tecnologías preparadas para procesar datos masivos, los clientes se frustran y se van a usar aplicaciones de otros bancos o billeteras digitales de la competencia.

---

### Pregunta 4
Clasifica los siguientes tipos de datos como **Estructurado**, **Semi-estructurado** o **No estructurado**. Justifica tu respuesta:

| Dato | Clasificación | Justificación |
|------|-------------|--------------|
| Un archivo Excel con ventas mensuales | **Estructurado** | Tiene un orden fijo de filas y columnas bien definidas. |
| Un tweet sobre el precio del dólar | **No estructurado** | Es texto libre, cada persona escribe como quiere, usa emojis y no tiene un tamaño fijo. |
| Una foto del ticket de compra en Metro | **No estructurado** | Es una imagen. Una computadora no puede leer el texto de la foto directamente a menos que use un programa especial. |
| Un archivo JSON de la API de SUNAT | **Semi-estructurado** | No tiene filas ni columnas, pero usa etiquetas organizadas que hacen fácil separar los datos. |
| Un audio de una llamada al call center de Claro | **No estructurado** | Es una grabación de voz con ruido y pausas, no tiene un formato de texto directo. |
| Un archivo CSV de exportaciones del BCRP | **Estructurado** | Los datos están organizados en filas separados por comas de forma idéntica. |
| Un video de seguridad de un supermercado | **No estructurado** | Es un archivo multimedia pesado que registra imágenes en movimiento, sin un orden de tablas. |
| Un log de errores de un servidor web | **Semi-estructurado** | Aunque es texto, siempre se escribe bajo el mismo patrón (Fecha, Hora, Error, Dirección IP). |

---

### Pregunta 5
¿Qué es un **clúster** en el contexto de Big Data? ¿Cuál es la diferencia entre un sistema de **memoria compartida** y un sistema de **memoria distribuida**? Usa un diagrama o esquema para explicarlo.

_Respuesta_:  
Un clúster es un grupo de varias computadoras normales que se conectan entre sí mediante una red para trabajar juntas, simulando ser una sola computadora gigante y muy potente.

**Diferencia de memoria:**
- **Memoria Compartida:** Imagina a varios procesadores en una sola computadora usando el mismo bloque de memoria RAM. Si hay mucho trabajo, se estorban entre ellos.
- **Memoria Distribuida:** Cada computadora del grupo tiene su propio procesador y su propia memoria RAM. No se estorban porque se reparten los pedazos de información por la red.

*(Ver diagrama generado con  draw.io al final del documento)*

![Diagrama de Memoria Compartida vs Distribuida](Capturas_GuiaTrabajo/pregunta5.drawio.png)

---

### Pregunta 6
Investiga y responde: ¿Qué empresa latinoamericana (puede ser peruana) ha implementado Big Data de manera exitosa? Describe:
- El problema que tenían
- La solución Big Data que implementaron
- Los resultados que obtuvieron

_Fuente consultada_: https://www.linkedin.com/posts/melissa-z%C3%A1rate-mor%C3%A1n-a573b8147_big-data-en-la-sunat-un-caso-peruano-de-share-7395102499415953408-seP5/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFRnsFgBGuZ7VKmJVriUgUY0RXgdosr5pS0

_Respuesta_:  
La institución peruana elegida es la . **SUNAT (Superintendencia Nacional de Aduanas y de Administración Tributaria)**.

- **Problema:** Durante muchos años, procesar los millones de comprobantes de pago electrónicos de todo el país le tomaba meses a la SUNAT. Esta limitación en su infraestructura heredada hacía que la fiscalización fuera muy lenta y poco precisa, lo que abría grandes espacios para la evasión de impuestos y dificultaba la detección a tiempo de irregularidades.
- **Solución:** La SUNAT migró sus sistemas a una arquitectura moderna y escalable basada en la nube. Implementaron herramientas clave de Big Data como Azure Data Factory (para mover los datos), un Data Lake (para almacenar la información masiva en bruto), Hive (para procesar consultas) y modelos predictivos de Machine Learning (Inteligencia Artificial).

- **Resultados:** Hoy en día, la SUNAT puede procesar esos enormes volúmenes de comprobantes en cuestión de horas y ya no en meses. Gracias a la analítica de Big Data, logran cruzar información en tiempo real con otras entidades (como SBS, RENIEC y SUNARP) para detectar automáticamente patrones de riesgo, como facturas falsas, proveedores ficticios o desbalances patrimoniales, logrando que las auditorías sean mucho más rápidas, transparentes y efectivas.

---

### Pregunta 7
Explica la diferencia entre **Data Lake** y **Data Warehouse**. ¿En qué situación usarías cada uno? Da un ejemplo de negocio para cada caso.

| Característica | Data Lake | Data Warehouse |
|--|----------|---------------|
| **Definición** | Es como un gran almacén donde guardas todos tus datos en bruto, tal cual vienen, sin ordenar. | Es como una biblioteca donde los datos ya están limpios, ordenados y listos para leer. |
| **Tipo de datos** | Guarda de todo: tablas, archivos de texto, audios, logs y JSON. | Solo guarda datos bien ordenados en tablas (filas y columnas). |
| **Cuándo usarlo** | Cuando quieres guardar mucha información rápido y no sabes todavía para qué la usarás. | Cuando necesitas hacer reportes fijos de la empresa con números exactos y limpios. |
| **Ejemplo de negocio** | Guardar todas las grabaciones de los clientes que se quejan para analizarlas en el futuro con inteligencia artificial. | Hacer el reporte oficial de las ventas del mes para pagar los impuestos a la SUNAT. |
| **Herramienta** | Amazon S3, Hadoop. | Google BigQuery, SQL Server Analysis Services. |

---

### Pregunta 8
¿Qué son los **requisitos de un sistema Big Data**? Identifica y explica los 5 requisitos principales que debe cumplir una arquitectura Big Data robusta. Para cada uno, menciona qué pasa si ese requisito NO se cumple.

_Respuesta_:  
Los requisitos son las características obligatorias que debe tener el sistema para que funcione bien, no se caiga y sea útil para los jefes de la empresa.

1. **Escalabilidad (Poder crecer):** Poder meter más computadoras baratas al grupo cuando la información aumente.
   - *Si no se cumple:* El sistema se llenará, se pondrá lento y dejará de funcionar cuando la empresa crezca.
2. **Tolerancia a fallos (Que no se caiga):** Si una computadora del grupo se malogra, otra debe tener una copia de los datos para seguir trabajando.
   - *Si no se cumple:* El sistema se detendría por completo y perderías información importante a mitad del día.
3. **Velocidad de respuesta:** Poder procesar los datos rápido para tomar decisiones en el momento útil.
   - *Si no se cumple:* Las respuestas llegarán tarde; por ejemplo, te avisará de una estafa horas después de que ya le robaron al cliente.
4. **Flexibilidad (Aceptar formatos):** Poder recibir datos ordenados y desordenados (audios, textos) sin problemas.
   - *Si no se cumple:* El sistema solo aceptaría tablas simples e ignoraría los correos o archivos de texto de la empresa.
5. **Seguridad (Privacidad):** Cuidar los datos con contraseñas y permisos para cumplir las leyes de privacidad.
   - *Si no se cumple:* Hackers podrían robar información de los clientes y el Estado multaría a la empresa por descuidada.

---

### Pregunta 9
La empresa en la que trabajas actualmente, ¿tiene algún problema de datos que podría resolverse con Big Data? Describe:
- El problema o necesidad
- Qué tipo de datos implicaría (V's del Big Data)
- Una propuesta inicial de solución (aunque sea básica)

_Respuesta_:  
En los hospitales públicos, un problema común es la lentitud al juntar la información médica con los reportes de las farmacias y los seguros (como el SIS). Los doctores escriben los síntomas en texto libre, las recetas van por otro sistema y los seguros usan archivos en formatos planos. 

Cuando el área de estadística intenta juntar todo esto al final del mes para hacer reportes o cobrar los reembolsos al seguro, los servidores principales se saturan y se ponen lentos, perjudicando la atención médica de los pacientes en vivo.

Esto implica **Volumen** (años de historias clínicas), **Variedad** (texto de doctores, tablas SQL, archivos del seguro) y **Veracidad** (corregir códigos médicos mal anotados).

La solución inicial sería crear un almacén separado (Data Lake) donde se haga una copia automática de todos estos archivos todos los días. Así, los encargados de estadística pueden cruzar la información y hacer sus reportes pesados en este almacén sin tocar ni volver lenta la computadora que usan los doctores para atender en los consultorios.

---

### Pregunta 10
**Análisis crítico**: Lee el siguiente caso y responde las preguntas:
> "Una empresa de telecomunicaciones en Perú tiene 8 millones de clientes. Cada cliente genera en promedio 500 registros de datos al día (llamadas, SMS, datos móviles, pagos). La empresa quiere predecir qué clientes cancelarán su contrato en los próximos 30 días para ofrecerles retención proactiva."

**a) ¿Cuántos registros se generan por día? ¿Por año?**
- **Al día:** 8 millones de clientes × 500 registros = **4,000 millones de registros diarios**.
- **Al año:** 4,000 millones × 365 días = **1.46 billones de registros anuales**.

**b) ¿Qué tipo de datos están involucrados?**
Datos mixtos: Estructurados (fechas de pago), Semi-estructurados (logs de conexión a internet de los celulares) y No estructurados (las llamadas grabadas de los clientes que se quejan).

**c) ¿Cuáles de las 5 V's son más relevantes en este caso?**
**Volumen** (por los miles de millones de datos), **Velocidad** (hay que procesar rápido antes de que el cliente se cambie de compañía) y **Valor** (la utilidad de convencer al cliente de quedarse para no perder dinero).

**d) ¿Qué tecnologías Big Data necesitarían para resolver este problema?**
Un almacén en la nube para guardar los archivos crudos (como Amazon S3), un motor para procesar datos en grupo a gran velocidad (como Apache Spark) y un panel interactivo para que los analistas vean los resultados (como Power BI).

**e) ¿Qué impacto ético podría tener esta solución? (pista: privacidad de datos)**
Revisar detalladamente las llamadas de los clientes, las páginas web por donde navegan en su celular y su ubicación GPS puede violar su privacidad según la **Ley de Protección de Datos Personales en el Perú**. La empresa debe pedir permiso obligatorio al cliente para usar sus datos y evitar usar esa información para discriminar a los usuarios.

---

## PARTE 2: REFLEXIÓN Y CONEXIÓN CON TU PROYECTO (2 preguntas adicionales)

### Pregunta 11 — Tu Proyecto
Describe brevemente el proyecto Big Data que tu grupo ha elegido:

_Respuesta_:  
- **Nombre del Proyecto:** Sistema de Predicción para el Abastecimiento de Medicamentos en Hospitales.
- **Sector:** Salud Pública.
- **Problema que resuelve:** Evitar que los hospitales se queden sin medicinas importantes o que estas se venzan en los almacenes por estar guardadas sin usarse. El sistema cruzará las enfermedades que tienen los pacientes con el stock de las farmacias para calcular de forma automática cuántas medicinas se necesitarán comprar el próximo mes.
- **V's presentes:** **Variedad** (tablas de stock y texto libre de recetas médicas), **Veracidad** (limpiar recetas con errores de escritura) y **Valor** (asegurar que ningún paciente se quede sin sus medicinas).

---

### Pregunta 12 — Arquitectura inicial
Dibuja una arquitectura inicial **muy básica** de cómo crees que debería funcionar tu proyecto. Incluye: fuentes de datos, almacenamiento, procesamiento y visualización.

_Link o descripción de tu diagrama_:  
*(Ver diagrama generado con  draw.io al final del documento)*

![Diagrama de Arquitectura del Proyecto](Capturas_GuiaTrabajo/Pregunta12.drawio.png)



---

## CRITERIOS DE EVALUACIÓN

| Criterio | Puntos |
|---------|--------|
| Responde todas las preguntas (no deja en blanco) | 4 |
| Usa sus propias palabras, no copia de internet | 4 |
| Da ejemplos reales de su entorno laboral | 4 |
| Las definiciones son técnicamente correctas | 4 |
| Respuestas de reflexión (P9, P11, P12) muestran pensamiento propio | 4 |
| **TOTAL** | **20** |

---

> **Recuerda**: La nota EC (10% del total) se basa en tu dominio conceptual. Esta guía de trabajo es el mejor preparativo. Si puedes responder estas 12 preguntas con seguridad, el examen EC no debería sorprenderte.

---

*Entrega: Subir al repositorio de GitHub Classroom o al foro de la plataforma virtual antes de la Semana 2*