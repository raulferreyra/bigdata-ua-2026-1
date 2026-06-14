# GUÍA DE TRABAJO — SEMANA 1

## Big Data (DD283) | Universidad Autónoma del Perú

**Nombre(s)**: Raúl Ferreyra  
**Grupo de proyecto**: Grupo 04  
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

Big Data es el conjunto de tecnologías, metodologías y arquitecturas diseñadas para capturar, almacenar, procesar y analizar volúmenes de datos que superan la capacidad de las herramientas relacionales tradicionales, tanto en escala como en velocidad y diversidad de formatos.

La diferencia fundamental con SQL Server o MySQL es triple: **escala** (Big Data maneja petabytes, no gigabytes), **variedad** (admite datos no estructurados como video, audio o texto libre, no solo filas y columnas con esquema fijo), y **velocidad de procesamiento** (soporta análisis en tiempo real sobre streams de eventos, no solo consultas batch programadas). Con 13 años de experiencia trabajando con bases de datos relacionales, he visto en la práctica cómo, cuando el volumen crece más allá de cierto punto, agregar índices y mejorar el hardware deja de escalar linealmente: el paradigma completo necesita cambiar.

---

### Pregunta 2

Explica las **5 V's del Big Data** con un ejemplo de tu propia empresa o de una empresa peruana que conozcas. Completa la siguiente tabla:

| V | Definición con tus palabras | Ejemplo de tu empresa/empresa conocida |
| --- | --------------------------- | --------------------------------------- |
| Volumen | Cantidad masiva de datos generados que supera la capacidad de procesamiento convencional | En la Universidad Norbert Wiener, donde trabajé, se acumulaban millones de registros académicos, financieros y de actividad en el LMS que ningún sistema consolidaba; sin arquitectura adecuada, los datos se archivaban sin análisis aprovechable |
| Velocidad | Rapidez con que los datos son generados y la urgencia con que deben procesarse para ser útiles | Los accesos al LMS durante un examen virtual generan miles de eventos por minuto; detectar caídas de plataforma o comportamiento anómalo requiere procesamiento en segundos, no horas |
| Variedad | Diversidad de formatos y fuentes de datos (estructurados, semi-estructurados, no estructurados) | En una universidad conviven registros de notas en SQL (estructurado), logs de Moodle en JSON (semi-estructurado) y grabaciones de clases en video (no estructurado) simultáneamente |
| Veracidad | Confiabilidad y calidad de los datos; cuánto podemos confiar en que reflejan la realidad | En la Norbert Wiener, distintos sistemas reportaban cifras diferentes para el mismo indicador; sin gobierno de datos, los reportes eran inconsistentes y nadie confiaba en ellos para tomar decisiones |
| Valor | Capacidad de extraer insights accionables que generen beneficio estratégico u operacional | El valor no está en acumular datos, sino en convertirlos en decisiones: identificar estudiantes en riesgo de deserción, optimizar procesos de matrícula o detectar fraudes en pagos de pensiones |

---

### Pregunta 3

¿Por qué una empresa como BCP (Banco de Crédito del Perú) NO podría usar solo una base de datos Oracle tradicional para procesar todos sus datos de transacciones en tiempo real? Menciona al menos 3 razones técnicas y 1 razón de negocio.

_Respuesta_:

**Razones técnicas:**

1. **Escalabilidad vertical con techo físico y económico**: Oracle escala verticalmente (más CPU, más RAM en un solo servidor), pero el BCP procesa decenas de millones de transacciones diarias. Más allá de cierto punto, no existe hardware que absorba esa carga en un único nodo sin degradar la latencia. Big Data requiere escalar horizontalmente añadiendo nodos al clúster sin límite teórico.

2. **Latencia inaceptable para procesamiento en streaming**: Oracle está optimizado para operaciones ACID transaccionales con consistencia garantizada, no para ingestar y procesar streams de eventos en tiempo real a escala masiva. Detectar fraudes en milisegundos o analizar el comportamiento de navegación en la app bancaria en tiempo real requiere arquitecturas como Kafka + Spark Streaming, que Oracle no puede reemplazar.

3. **Modelo relacional rígido para datos no estructurados**: Los logs de la banca móvil, las grabaciones de atención al cliente, los comentarios en redes sociales o las imágenes de vouchers no caben eficientemente en esquemas tabulares fijos. Forzarlos en Oracle implica transformaciones costosas o pérdida de información valiosa para analítica avanzada.

4. **Costo de licenciamiento insostenible a escala**:
**Razón de negocio:**
Oracle Enterprise cobra por core de procesador. Escalar para manejar petabytes de datos con licencias Oracle llevaría el costo operativo a niveles inviables frente a alternativas open-source como Hadoop/Spark o soluciones cloud como AWS Redshift o Google BigQuery, que ofrecen mayor escala a fracción del costo.

---

### Pregunta 4

Clasifica los siguientes tipos de datos como **Estructurado**, **Semi-estructurado** o **No estructurado**. Justifica tu respuesta:

| Dato | Clasificación | Justificación |
| ------ | ------------- | -------------- |
| Un archivo Excel con ventas mensuales | Estructurado | Filas y columnas con esquema fijo y predefinido; los tipos de datos de cada columna son consistentes entre todos los registros |
| Un tweet sobre el precio del dólar | No estructurado | El contenido principal es texto libre en lenguaje natural sin esquema; aunque la respuesta de la API de Twitter es JSON (semi-estructurada), el dato relevante —el texto— no tiene estructura predefinida |
| Una foto del ticket de compra en Metro | No estructurado | Imagen binaria sin estructura de datos intrínseca; requiere OCR para extraer información numérica o textual utilizable |
| Un archivo JSON de la API de SUNAT | Semi-estructurado | Tiene jerarquía y claves definidas pero permite campos opcionales, anidamiento variable y arrays de longitud arbitraria; el esquema es flexible, no fijo como en SQL |
| Un audio de una llamada al call center de Claro | No estructurado | Señal de audio en formato binario sin estructura de datos; requiere transcripción automática (ASR) para convertirse en texto analizable |
| Un archivo CSV de exportaciones del BCRP | Estructurado | Delimitado por comas, columnas nombradas con tipos de datos consistentes entre todos los registros; equivalente funcional a una tabla relacional |
| Un video de seguridad de un supermercado | No estructurado | Stream de frames de video binario sin estructura de datos; requiere visión computacional (object detection, tracking) para extraer información significativa |
| Un log de errores de un servidor web | Semi-estructurado | Sigue un patrón repetible (timestamp, nivel de severidad, código, mensaje) parseable con regex, pero el campo de mensaje es texto libre; no tiene esquema formal declarado |

---

### Pregunta 5

¿Qué es un **clúster** en el contexto de Big Data? ¿Cuál es la diferencia entre un sistema de **memoria compartida** y un sistema de **memoria distribuida**? Usa un diagrama o esquema para explicarlo.

_Respuesta_:

Un **clúster** en Big Data es un conjunto de computadoras (nodos) interconectadas por red que trabajan coordinadamente como un sistema unificado, distribuyendo entre todos ellos la carga de almacenamiento y procesamiento. El nodo que coordina el trabajo se denomina master/driver y los que ejecutan las tareas son los workers. Esta arquitectura es la base de Hadoop, Apache Spark y otros frameworks Big Data.

**Memoria Compartida (Shared Memory):**
Múltiples procesadores o cores acceden a la misma RAM física dentro de un único servidor. La comunicación entre procesos es extremadamente rápida (acceso directo a memoria) pero la escalabilidad está limitada al hardware de ese servidor — si se necesita más capacidad, hay que comprar un servidor más potente (escalabilidad vertical).

**Memoria Distribuida (Distributed Memory):**
Cada nodo del clúster tiene su propia RAM independiente. Los nodos se comunican mediante mensajes por red. La escalabilidad es horizontal: se agregan más nodos según la necesidad, sin límite teórico de crecimiento. Esta es la arquitectura que utilizan Hadoop HDFS y Apache Spark.

```bash
MEMORIA COMPARTIDA:                    MEMORIA DISTRIBUIDA:
┌───────────────────────┐              ┌─────────┐  ┌─────────┐  ┌─────────┐
│    RAM COMPARTIDA     │              │ NODO 1  │  │ NODO 2  │  │  NODO N │
│  ┌─────────────────┐  │              │  RAM 1  │  │  RAM 2  │  │  RAM N  │
│  │  CPU 1  CPU 2   │  │              │ DISCO 1 │  │ DISCO 2 │  │ DISCO N │
│  │  CPU 3  CPU N   │  │              └────┬────┘  └────┬────┘  └────┬────┘
│  └─────────────────┘  │                   └────────────┴────────────┘
└───────────────────────┘                         RED (LAN / Infiniband)

Ventaja: latencia ultra baja           Ventaja: escala sin límite teórico
Límite: techo de un solo servidor      Límite: latencia de red entre nodos
```

Ejemplo: un servidor con 64 cores y 1 TB de RAM corriendo Oracle (memoria compartida) vs. un clúster Hadoop con 50 nodos de 32 GB de RAM cada uno → 1.6 TB de memoria distribuida, expandible añadiendo nodos sin detener el sistema.

---

### Pregunta 6

Investiga y responde: ¿Qué empresa latinoamericana (puede ser peruana) ha implementado Big Data de manera exitosa? Describe:

- El problema que tenían
- La solución Big Data que implementaron
- Los resultados que obtuvieron

_Fuente consultada (URL o libro)_: [Google Cloud - Falabella](https://cloud.google.com/customers/falabella)

_Respuesta_:

**Empresa: Falabella** (presencia en Perú, Chile, Colombia, Argentina)

**Problema:** Falabella operaba con datos fragmentados en silos completamente desconectados: ventas físicas, e-commerce, tarjeta CMR y app móvil generaban datos que no se comunicaban entre sí. No podían predecir demanda por tienda, personalizar ofertas a nivel de cliente individual ni detectar tendencias en tiempo real. Los reportes tardaban días en generarse y llegaban demasiado tarde para influir en decisiones operacionales de inventario o pricing.

**Solución Big Data implementada:** Falabella migró a una plataforma de datos unificada en Google Cloud Platform, construyendo un Data Lake centralizado en Google Cloud Storage con pipelines de ingesta orquestados con Apache Beam/Dataflow. Implementaron BigQuery como Data Warehouse para analítica SQL a escala y modelos de Machine Learning en Vertex AI para recomendación de productos y predicción de quiebre de stock. Los datos de todos los canales (tienda física, web, app, CMR) convergen en una única capa de datos gobernada.

**Resultados obtenidos:**

- Reducción del sobre-stock en aproximadamente 20% gracias a predicción de demanda más precisa por tienda
- Incremento en la tasa de conversión del e-commerce mediante recomendaciones personalizadas en tiempo real
- Reducción del tiempo de generación de reportes de días a minutos
- Capacidad de tomar decisiones de inventario y pricing basadas en comportamiento actual del mercado, no en datos históricos con semanas de retraso

---

### Pregunta 7

Explica la diferencia entre **Data Lake** y **Data Warehouse**. ¿En qué situación usarías cada uno? Da un ejemplo de negocio para cada caso.

| | Data Lake | Data Warehouse |
| -- | ---------- | --------------- |
| Definición | Repositorio centralizado que almacena datos en su formato original (raw) sin transformar, a cualquier escala; el esquema se aplica al momento de leer (schema-on-read) | Base de datos optimizada para análisis donde los datos ya están limpios, transformados y estructurados; el esquema se aplica al cargar los datos (schema-on-write) |
| Tipo de datos | Todos: estructurados, semi-estructurados y no estructurados (CSV, JSON, video, audio, logs, imágenes) en su estado original | Principalmente estructurados, con esquema definido, consistente y validado; datos ya procesados y enriquecidos por pipelines ETL |
| Cuándo usarlo | Cuando se necesita almacenar grandes volúmenes de datos diversos sin saber aún exactamente cómo se analizarán; ideal para exploración, ciencia de datos y entrenamiento de modelos ML | Cuando los casos de uso analíticos están bien definidos, los datos necesitan alta calidad garantizada y los usuarios finales son analistas de negocio que usan SQL y herramientas BI |
| Ejemplo de negocio | Una universidad que guarda todos sus logs de actividad en el LMS, grabaciones de clases, investigaciones en PDF y comentarios de estudiantes en RRSS para que el equipo de Data Science los explore sin restricciones de esquema | El área de Finanzas del BCP que necesita reportes mensuales de rentabilidad por producto bancario con datos validados y listos para consultas SQL precisas y auditables |
| Herramienta típica | AWS S3 + Apache Spark, Azure Data Lake Storage, Google Cloud Storage, Delta Lake | Amazon Redshift, Google BigQuery, Snowflake, Azure Synapse Analytics, Apache Hive |

---

### Pregunta 8

¿Qué son los **requisitos de un sistema Big Data**? Identifica y explica los 5 requisitos principales que debe cumplir una arquitectura Big Data robusta. Para cada uno, menciona qué pasa si ese requisito NO se cumple.

_Respuesta_:

**1. Escalabilidad horizontal**
El sistema debe poder crecer añadiendo más nodos al clúster, no solo mejorando hardware existente. La capacidad de procesamiento y almacenamiento debe crecer proporcionalmente al volumen de datos sin rediseñar la arquitectura.

_Si no se cumple:_ El sistema alcanza un techo de capacidad insuperable sin costosas migraciones; en el intertanto las consultas se degradan o fallan por saturación, afectando directamente la operación del negocio.

**2. Tolerancia a fallos (Fault Tolerance)**
Ante la caída de uno o varios nodos, el sistema debe continuar operando sin pérdida de datos ni interrupción del servicio, mediante replicación automática de datos y reanudación de tareas fallidas.

_Si no se cumple:_ Una falla de hardware convierte en pérdida de datos críticos e interrumpe servicios que en muchas organizaciones operan 24/7, con impacto económico y reputacional directo.

**3. Calidad y gobierno de datos (Veracidad)**
Deben existir mecanismos de validación, limpieza, linaje y catálogo de datos. Los datos deben ser confiables y trazables desde su origen hasta su consumo final.

_Si no se cumple:_ Se trabaja con datos inconsistentes entre sistemas, generando análisis contradictorios y decisiones de negocio incorrectas — el clásico "garbage in, garbage out". En mi experiencia en la Norbert Wiener, la ausencia de este requisito hacía que diferentes áreas reportaran cifras distintas para el mismo KPI, paralizando la confianza en la analítica.

**4. Seguridad y cumplimiento normativo**
Cifrado en tránsito y en reposo, control de acceso basado en roles (RBAC), auditoría de accesos y cumplimiento de regulaciones vigentes (Ley N.° 29733 de Protección de Datos Personales en Perú; GDPR si aplica para datos europeos).

_Si no se cumple:_ Se incumplen regulaciones con posibles sanciones económicas y legales; se expone información sensible a accesos no autorizados. En mi anterior empleo, la falta de arquitecturas establecidas llevaba exactamente a este escenario: datos sin trazabilidad, accesos sin auditar y dependencia de proveedores externos que procesaban datos que debían mantenerse internamente, violando parámetros básicos de seguridad.

**5. Latencia adecuada al caso de uso (batch vs. real-time)**
La arquitectura debe diferenciar entre procesamiento batch (tolerante a latencias de horas, para reportes diarios) y streaming en tiempo real (latencia de milisegundos a segundos, para alertas y detección de fraude), y proveer la infraestructura adecuada para cada patrón.

_Si no se cumple:_ O se sacrifica la capacidad de análisis en tiempo real (perdiendo detección de fraudes o alertas operacionales) o se sobredimensiona infraestructura de streaming para casos que no la necesitan, incrementando costos innecesariamente.

---

### Pregunta 9

La empresa en la que trabajas actualmente, ¿tiene algún problema de datos que podría resolverse con Big Data? Describe:

- El problema o necesidad
- Qué tipo de datos implicaría (V's del Big Data)
- Una propuesta inicial de solución (aunque sea básica)

_Respuesta_:

Actualmente no me encuentro empleado; sin embargo, con 13 años de experiencia en el sector y habiendo apoyado a compañeros en la gestión de datos y en la comprensión de flujos como GIT durante ese tiempo, he podido identificar y documentar de primera mano una problemática estructural muy representativa en mi última posición en la **Universidad Norbert Wiener**.

**El problema:**
La institución carecía de arquitecturas de datos bien establecidas. Los sistemas de las distintas áreas (académica, financiera, RRHH, operaciones, plataforma LMS) generaban datos en silos completamente desconectados, con formatos inconsistentes y sin ningún lineamiento de gobierno de datos. Esta situación tenía consecuencias operacionales muy concretas: varios procesos incumplían parámetros de seguridad porque la trazabilidad de los datos era inexistente — no había logs centralizados de quién accedía a qué información, cuándo y para qué fin. Para tareas que internamente deberían ser perfectamente manejables (consolidación de reportes entre áreas, análisis de comportamiento estudiantil, auditorías de pagos), se dependía de terceros y proveedores externos, incrementando costos, generando fricciones operacionales y, lo más grave, exponiendo datos potencialmente sensibles de estudiantes y de la institución a actores fuera de la organización.

**Tipo de datos y V's del Big Data involucradas:**

- **Variedad:** Datos académicos en SQL, pagos en ERP propietario, logs del LMS en JSON, grabaciones de clases en video y documentos de tesis en PDF convivían sin integración
- **Veracidad (crítica):** Distintos sistemas reportaban cifras diferentes para el mismo KPI; sin una fuente única de verdad, nadie confiaba en los datos para tomar decisiones estratégicas
- **Volumen:** Miles de estudiantes, millones de eventos de plataforma y años de histórico acumulados sin procesar ni analizar de forma consolidada
- **Valor (ausente):** Los datos existían pero el valor no se materializaba; sin arquitectura adecuada, el dato era costo de almacenamiento, no activo estratégico

**Propuesta de solución Big Data:**

1. **Data Lake centralizado** (AWS S3 o Azure Data Lake) como capa de ingesta única para todos los sistemas fuente de la institución
2. Pipelines orquestados con **Apache Airflow** para automatizar la integración y generar trazabilidad y auditoría de cada movimiento de datos
3. **Catálogo de metadatos** (AWS Glue Data Catalog o Apache Atlas) para gobierno, linaje de datos y cumplimiento de la Ley N.° 29733
4. **Data Warehouse** (Redshift o BigQuery) para los reportes académicos y financieros, eliminando la dependencia de terceros para análisis básicos
5. **RBAC y cifrado desde el diseño** (en tránsito y en reposo) para cumplir parámetros de seguridad internamente, sin necesidad de externalizar

---

### Pregunta 10

**Análisis crítico**: Lee el siguiente caso y responde las preguntas:

> "Una empresa de telecomunicaciones en Perú tiene 8 millones de clientes. Cada cliente genera en promedio 500 registros de datos al día (llamadas, SMS, datos móviles, pagos). La empresa quiere predecir qué clientes cancelarán su contrato en los próximos 30 días para ofrecerles retención proactiva."

**a)** ¿Cuántos registros se generan por día? ¿Por año?  
**b)** ¿Qué tipo de datos están involucrados?  
**c)** ¿Cuáles de las 5 V's son más relevantes en este caso?  
**d)** ¿Qué tecnologías Big Data necesitarían para resolver este problema?  
**e)** ¿Qué impacto ético podría tener esta solución? (pista: privacidad de datos)

_Respuesta_:

**a) Cálculo de registros:**

- Por día: 8,000,000 clientes × 500 registros = **4,000,000,000 registros/día (4 mil millones)**
- Por año: 4,000,000,000 × 365 = **1,460,000,000,000 registros/año (1.46 billones)**
- Estimando 200 bytes promedio por registro: ~800 GB/día → ~292 TB/año solo en datos estructurados

**b) Tipos de datos involucrados:**

- _Estructurados:_ CDRs (Call Detail Records) con timestamp, duración, número destino y costo; registros de SMS; logs de consumo de datos móviles (MB/día); historial de pagos
- _Semi-estructurados:_ Logs de la app en JSON; registros de geolocalización (coordenadas + timestamp); datos de plan contratado en formato XML/JSON de la API interna
- _No estructurados:_ Grabaciones de llamadas al call center (audio); tickets de atención al cliente en texto libre; comentarios en redes sociales mencionando a la operadora

**c) Las 5 V's más relevantes:**

- **Volumen (crítica):** 4 mil millones de registros diarios define inequívocamente un caso Big Data; ninguna base de datos relacional convencional puede absorber esa carga
- **Velocidad (alta):** Los CDRs se generan en tiempo real; el modelo de churn necesita datos actualizados frecuentemente para ser predictivo, no retrospectivo con semanas de retraso
- **Veracidad (crítica):** Si los CDRs contienen errores (duplicados, timestamps incorrectos, montos erróneos), el modelo de predicción aprenderá patrones falsos y sus predicciones serán inútiles o contraproducentes para el negocio

**d) Tecnologías Big Data necesarias:**

| Capa | Tecnología | Justificación |
| ------ | ----------- | --------------- |
| Ingesta | Apache Kafka | Captura CDRs en tiempo real con alta throughput y baja latencia desde múltiples sistemas fuente |
| Almacenamiento | Hadoop HDFS / AWS S3 | Data Lake para el volumen histórico de 1.46 billones de registros/año sin techo de escala |
| Procesamiento | Apache Spark + MLlib | Procesamiento distribuido de features y entrenamiento/scoring del modelo de predicción de churn |
| Serving analítico | Amazon Redshift / BigQuery | Data Warehouse con las predicciones para que el equipo comercial las consulte con SQL |
| Visualización | Power BI / Apache Superset | Dashboard de clientes en riesgo para que el equipo de retención priorice acciones |

**e) Impacto ético:**

El análisis masivo de metadatos de comunicaciones tiene implicaciones éticas y legales serias:

1. **Privacidad:** Los CDRs revelan patrones de comportamiento muy íntimos (con quién hablas, cuándo, desde dónde). La Ley N.° 29733 de Protección de Datos Personales del Perú exige consentimiento informado para el tratamiento de datos personales con finalidades distintas al servicio contratado.

2. **Discriminación algorítmica:** Si el modelo identifica que ciertos perfiles (zonas geográficas, tipos de plan, nivel de consumo) tienen mayor churn y la empresa les ofrece menos incentivos de retención, se genera discriminación sistemática hacia segmentos más vulnerables.

3. **Transparencia y finalidad:** Los clientes tienen derecho a saber que sus datos de comportamiento se usan para predecir su comportamiento futuro (principio de finalidad del tratamiento de datos personales).

4. **Riesgo de vigilancia:** Almacenar quién llama a quién, cuándo y desde dónde durante años crea una base de datos con potencial de uso para vigilancia que excede el caso de uso declarado.

**Medidas mitigantes recomendadas:** anonimización de CDRs antes del entrenamiento del modelo; políticas de retención de datos (borrar históricos después de X meses); auditorías periódicas del modelo para detectar sesgos; transparencia explícita en la política de privacidad sobre el uso analítico de los datos.

---

## PARTE 2: REFLEXIÓN Y CONEXIÓN CON TU PROYECTO (2 preguntas adicionales)

### Pregunta 11 — Tu Proyecto

Describe brevemente el proyecto Big Data que tu grupo ha elegido:

- Nombre del proyecto
- Empresa o sector al que aplica
- Problema que resuelve
- ¿Cuáles de las 5 V's están presentes en los datos del proyecto?

_Respuesta_:

**Nombre del proyecto:** Predicción Espacio-Temporal de Congestión Vehicular y Optimización Dinámica de Rutas en Lima Metropolitana mediante Procesamiento Big Data y Grafos

**Empresa/sector:** Smart City / Transporte / Gobierno Local — Lima Metropolitana

**Equipo (Grupo 3):** Ferreyra (líder/arquitectura), Huapaya (ingesta GPS/PySpark), Orellano (dashboard/mapas), Paredes y Zevallos (GraphX/Neo4j/ML)

**Problema que resuelve:** Lima es la 5ta ciudad más congestionada de América Latina (TomTom Traffic Index 2024). Los limeños pierden en promedio 117 horas al año atascados en tráfico, con un impacto económico de S/ 6,200 millones/año (MTC 2023). Más de 500,000 puntos GPS de taxis y buses se generan por hora pero no se procesan para optimización en tiempo real: los semáforos tienen tiempos fijos, no existe predicción de congestión con anticipación, y los eventos públicos (partidos, conciertos, desfiles) no se integran al modelo de tráfico.

**5 V's presentes en el proyecto:**

- **Volumen:** 50K vehículos × 1 punto GPS/min × 16h/día = **48 millones de puntos GPS/día** → ~3.3 TB/año
- **Velocidad:** Near Real-time — actualización de rutas alternativas cada 2 minutos; predicción de congestión con 30 minutos de anticipación
- **Variedad:** GPS lat/lon (estructurado), GeoJSON de OpenStreetMap (semi-estructurado), datos climáticos SENAMHI en JSON (semi-estructurado), eventos públicos Lima (no estructurado)
- **Veracidad:** Señales GPS perdidas, coordenadas fuera de rango geográfico, duplicados por re-transmisión; el modelo debe ser robusto ante el ruido GPS inherente
- **Valor:** Reducir 20% el tiempo en viaje = ahorro estimado de S/ 1,240 millones/año para Lima; aplicable directamente al MTC y municipalidades distritales para optimización de semáforos y rutas

---

### Pregunta 12 — Arquitectura inicial

Dibuja (a mano o usando draw.io) una arquitectura inicial **muy básica** de cómo crees que debería funcionar tu proyecto. Incluye: fuentes de datos, almacenamiento, procesamiento y visualización.

 [Imagen Semana 01 Draw.io](https://drive.google.com/file/d/1VjvSks_yewYmNXeJAYyU49_UPiyJJnpB/view?usp=drive_link)

_Link o descripción de tu diagrama_:

**Arquitectura del proyecto Smart Traffic Lima:**

```bash
FUENTES DE DATOS               PROCESAMIENTO SPARK          NEO4J + ML              VISUALIZACIÓN
────────────────────────────────────────────────────────────────────────────────────────────────
┌──────────────────┐           ┌──────────────────────┐    ┌──────────────────┐    ┌───────────┐
│ Simulador GPS    │           │  PySpark 3.5          │    │  NEO4J GRAPH DB  │    │  Folium   │
│ 50K vehículos   │──────────▶│  joins + agrega-      │───▶│  Grafo calles    │───▶│  Kepler   │
│ (lat/lon/ts)     │           │  ciones por           │    │  Lima (15K nodos │    │  .gl      │
├──────────────────┤           │  segmento de calle    │    │  40K aristas)    │    │  (mapa    │
│ OpenStreetMap    │           ├──────────────────────┤    │  Cypher: rutas   │    │  interac- │
│ Lima GeoJSON     │──────────▶│  GraphX               │    │  óptimas < 2s    │    │  tivo)    │
├──────────────────┤           │  PageRank: cuellos    │    └──────────────────┘    └───────────┘
│ SENAMHI API      │──────────▶│  de botella           │
│ Clima Lima       │           ├──────────────────────┤    ┌──────────────────┐
├──────────────────┤           │  MLlib Random Forest  │───▶│  MongoDB Atlas   │
│ Eventos públicos │──────────▶│  Predicción congest.  │    │  (predicciones   │
│ Lima (JSON)      │           │  30 min anticipación  │    │  y eventos)      │
└──────────────────┘           └──────────────────────┘    └──────────────────┘

Stack: PySpark 3.5 | Databricks Community | Neo4j AuraDB Free | MongoDB Atlas M0
```

Diagrama formal adjunto en el link de drive indicado arriba.

---

## CRITERIOS DE EVALUACIÓN

| Criterio | Puntos |
| --------- | -------- |
| Responde todas las preguntas (no deja en blanco) | 4 |
| Usa sus propias palabras, no copia de internet | 4 |
| Da ejemplos reales de su entorno laboral | 4 |
| Las definiciones son técnicamente correctas | 4 |
| Respuestas de reflexión (P9, P11, P12) muestran pensamiento propio | 4 |
| **TOTAL** | **20** |

---

> **Recuerda**: La nota EC (10% del total) se basa en tu dominio conceptual. Esta guía de trabajo es el mejor preparativo. Si puedes responder estas 12 preguntas con seguridad, el examen EC no debería sorprenderte.

---

Entrega: Subir al repositorio de GitHub Classroom o al foro de la plataforma virtual antes de la Semana 2
