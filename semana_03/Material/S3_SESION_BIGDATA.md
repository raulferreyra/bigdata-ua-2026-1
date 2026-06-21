# S3 — SESIÓN COMPLETA: HADOOP MEJORAS + SISTEMAS NoSQL
## Universidad Autónoma del Perú | Big Data DD283 | 2026-1

---

| Campo | Detalle |
|-------|---------|
| **Curso** | Big Data DD283 |
| **Semana** | 3 |
| **Tema** | Hadoop Mejoras (YARN, Pig, Hive, Impala, Drill, Ambari) + Sistemas NoSQL: Clasificación e Implementación |
| **Logro de aprendizaje** | El estudiante resuelve un caso aplicando Hadoop e implementa una base de datos NoSQL con integridad |
| **Duración** | 3 horas (180 min) + 20 min receso |
| **Stack principal** | Databricks Community (Hive/Spark SQL) + MongoDB Atlas M0 (NoSQL gratuito) |
| **Valor** | Integridad al estructurar bases de datos sin sesgar información |

---

## TABLA DE CONTENIDOS

1. [Cronograma](#cronograma)
2. [Bloque 1 — Inicio y Utilidad](#bloque-1)
3. [Bloque 2 — Transformación](#bloque-2)
4. [Receso](#receso)
5. [Bloque 3 — Práctica y Cierre](#bloque-3)
6. [Guion verbal](#guion)
7. [Casos reales](#casos-reales)
8. [Evaluación formativa](#evaluacion)
9. [Stack gratuito recomendado](#stack)
10. [Referencias APA 7](#referencias)

---

## CRONOGRAMA {#cronograma}

| Bloque | Actividad | Duración | Responsable |
|--------|-----------|----------|-------------|
| **Bloque 1** | INICIO: Rompe-hielo + Logro + Revisión S2 + Diagnóstico | 20 min | Docente |
| **Bloque 1** | UTILIDAD: ¿Por qué NoSQL y las mejoras de Hadoop importan? | 10 min | Docente |
| **Bloque 1** | TRANSFORMACIÓN 1: YARN + Pig + Hive + Impala/Drill/Ambari | 30 min | Docente + Alumnos |
| **RECESO** | Descanso | 20 min | — |
| **Bloque 2** | TRANSFORMACIÓN 2: RDBMS limitaciones + NoSQL fundamentos + CAP + 4 tipos | 40 min | Docente + Alumnos |
| **Bloque 3** | PRÁCTICA: Caso grupal (Claro Perú) + Ejercicio individual MongoDB | 40 min | Estudiantes |
| **Bloque 3** | CIERRE: Síntesis + Metacognición + Tarea | 10 min | Docente + Alumnos |

---

## BLOQUE 1 — INICIO (20 min) {#bloque-1}

### 1. ROMPE-HIELO (5 min) — "La base de datos que conoces vs. la que necesitas"

> **Instrucción verbal al docente:**
>
> *"Voy a proyectar tres escenarios. Ustedes me dirán, en 10 segundos cada uno, si usarían un Excel, un SQL tradicional, o algo que todavía no conocen. Listo:"*
>
> **Escenario 1:** "WhatsApp tiene 100 mil millones de mensajes enviados por día. ¿Los guardan en una tabla de Excel?" → (Respuesta esperada: No)
>
> **Escenario 2:** "Instagram necesita guardar la foto de perfil de 2 mil millones de usuarios. ¿La ponen en una columna VARCHAR de Oracle?" → (Esperada: No)
>
> **Escenario 3:** "Amazon necesita recomendar productos en menos de 100 ms analizando el historial de compras de 300 millones de clientes. ¿Usan un JOIN de 5 tablas en PostgreSQL?" → (Esperada: No)
>
> *"Exactamente. Las tres respuestas son 'no'. Hoy van a descubrir QUÉ usan en cambio — y van a implementarlo."*

---

### 2. LOGRO DE APRENDIZAJE (3 min)

> **Guion verbal textual:**
>
> *"El logro de hoy tiene dos partes muy concretas que están conectadas.*
>
> *Primero: van a entender por qué Hadoop 1.x tenía problemas serios en producción y cómo la industria los resolvió con YARN, Hive, Impala y otras herramientas. No como historia, sino para que cuando escuchen a un ingeniero de datos hablar de 'latencia en consultas SQL sobre HDFS' sepan exactamente de qué habla.*
>
> *Segundo — y esto es lo que más valor les da hoy: van a IMPLEMENTAR una base de datos NoSQL real. En MongoDB Atlas, que es gratuito y es la base de datos NoSQL más usada en el mundo según DB-Engines 2025. Van a crear colecciones, insertar documentos, hacer consultas y correr una pipeline de agregación.*
>
> *Al final de la sesión, si un reclutador les pregunta: '¿Tienes experiencia con NoSQL?', van a poder decir: 'Sí, implementé una base de datos MongoDB para un caso de análisis de clientes con Python.' Eso es lo que hacemos hoy."*

---

### 3. REVISIÓN SESIÓN ANTERIOR (7 min)

**Pregunta 1:**
> *"La semana pasada vimos HDFS. ¿Qué pasaba en Hadoop 1.x cuando el NameNode fallaba?"*

**Respuesta esperada detallada:** En Hadoop 1.x, el NameNode era un **Single Point of Failure (SPOF)**: si caía, todo el clúster quedaba inaccesible porque era el único nodo que tenía el mapa de bloques en RAM. No había NameNode secundario activo que pudiera tomar el control automáticamente. Hadoop 2.x resolvió esto con **High Availability (HA)**: un NameNode activo y uno standby sincronizados via JournalNodes, con ZooKeeper coordinando el failover automático en ~30 segundos.

**Si responde mal:** *"Piensen en el NameNode como el único mesero que sabe dónde está cada mesa. Si ese mesero se va... ¿cómo sirves a los clientes? Hadoop 1 no tenía sustituto. Hadoop 2 sí."*

---

**Pregunta 2:**
> *"¿Cuál es la diferencia fundamental entre HDFS y HBase en términos de tipo de acceso? ¿Cuándo usarían cada uno?"*

**Respuesta esperada detallada:** HDFS está optimizado para **acceso secuencial batch** — lee archivos completos de forma distribuida, ideal para procesar 1 TB de logs en un job nocturno. HBase está optimizado para **acceso aleatorio en tiempo real** por Row Key — ideal para buscar el saldo de un cliente específico en milisegundos. Regla práctica: "¿necesito respuesta en milisegundos buscando un registro específico?" → HBase. "¿Necesito procesar millones de registros en un análisis?" → HDFS.

**Si responde mal:** *"Piensen en una biblioteca. HDFS es como leer todo un libro de principio a fin. HBase es como ir al índice, buscar la página exacta y leer solo esa."*

---

**Pregunta 3:**
> *"Expliquen con sus propias palabras las fases MAP y REDUCE con un ejemplo diferente al de contar palabras o transacciones."*

**Respuesta esperada detallada:** MAP transforma cada registro de forma local e independiente, emitiendo pares (clave, valor). REDUCE consolida todos los pares con la misma clave. Ejemplo válido: calcular el promedio de temperatura por ciudad a partir de 1 millón de lecturas de sensores. MAP: cada sensor emite (ciudad, temperatura). REDUCE: para cada ciudad, suma las temperaturas y divide por el número de lecturas. Otro ejemplo: encontrar el producto más vendido por categoría → MAP: (categoría, cantidad_vendida) por cada venta. REDUCE: sumar por categoría y tomar el máximo.

---

### 4. DIAGNÓSTICO INICIAL (5 min)

> *"Tres preguntas rápidas — no se califican, son para que yo ajuste la clase:"*

**Pregunta diagnóstica 1:**
> *"¿Alguien puede decirme qué es SQL? ¿Y qué significa NoSQL — 'No SQL' o algo diferente?"*

**Respuesta esperada:** SQL (Structured Query Language) es el lenguaje estándar para consultar bases de datos relacionales con tablas, filas y columnas. NoSQL NO significa "no se puede usar SQL" — significa **"Not Only SQL"**: bases de datos que van más allá del modelo relacional y pueden almacenar datos en formatos flexibles (documentos JSON, pares clave-valor, grafos, columnas). Muchos sistemas NoSQL incluso tienen interfaces SQL.

---

**Pregunta diagnóstica 2:**
> *"¿Han escuchado MongoDB? ¿Alguien sabe para qué sirve o qué tipo de datos guarda?"*

**Respuesta esperada (cualquier nivel es válido aquí):** MongoDB es una base de datos NoSQL de tipo **documento** que almacena datos en formato BSON (similar a JSON), sin esquema fijo. Cada documento puede tener diferentes campos. Es usada por Uber, LinkedIn, Forbes, eBay, entre otras empresas. Si nadie sabe nada: "Perfecto — parten de cero y van a terminarlo implementando."

---

**Pregunta diagnóstica 3:**
> *"¿Por qué creen que una base de datos como PostgreSQL o MySQL no es suficiente para manejar los datos de TikTok, que tiene 1 mil millones de videos con likes, comentarios, hashtags y métricas de cada usuario?"*

**Respuesta esperada:** Las bases relacionales tienen limitaciones a esa escala: (1) **Escala horizontal difícil** — SQL escala verticalmente (comprar un servidor más caro), pero distribuir una tabla relacional en 100 nodos con JOINs es muy complejo. (2) **Esquema rígido** — cada video de TikTok tiene metadatos diferentes (algunos tienen dueto, otros no; algunos tienen música, otros no) — un esquema fijo con ALTER TABLE para 1B de registros es inviable. (3) **Latencia en JOINs masivos** — recomendar el "siguiente video" en 200ms con un JOIN de 5 tablas sobre 1B de registros no es factible en SQL tradicional.

---

## UTILIDAD (10 min)

### ¿Por qué Hadoop mejoras + NoSQL son críticos hoy?

**Dato real:** En 2025, el 60% de las empresas Fortune 500 usa MongoDB, Redis o Cassandra como parte de su stack de datos (DB-Engines, 2025). En Perú, el BCP usa Cassandra para su sistema de transacciones de Yape, Interbank usa MongoDB para su historial de clientes digital, y SUNAT usa arquitecturas híbridas SQL + NoSQL para el sistema de facturación electrónica.

**El problema que resuelven:**

En 2012, Twitter tenía 340 millones de tweets al día. Su base MySQL principal alcanzó el límite: los JOINs tardaban minutos, las migraciones de esquema tomaban horas (el sistema caía), y agregar un nuevo campo requería alterar una tabla de 500 GB. La solución: migrar tweets a **Manhattan** (su sistema NoSQL interno basado en Cassandra). Un tweet ahora es un documento flexible — puede tener imágenes, videos, encuestas, hilos — sin alterar el esquema.

**Aplicación en empresas peruanas hoy:**

| Empresa | Problema resuelto con NoSQL | Tipo NoSQL |
|---------|-----------------------------|-----------|
| Yape (BCP) | Historial de transacciones en tiempo real para 10M usuarios | Cassandra (Column) |
| Rappi Perú | Catálogo de 500K productos con atributos variables | MongoDB (Document) |
| Claro Perú | Sesiones activas de 8M suscriptores | Redis (Key-Value) |
| Migraciones Perú | Red de relaciones de personas naturales/jurídicas | Neo4j (Graph) |

**Pregunta retadora:**

> *"Si Hadoop MapReduce es tan poderoso para procesar datos, ¿por qué las empresas siguieron construyendo Hive, Impala y Drill ENCIMA de Hadoop en lugar de usar MapReduce directamente? ¿Qué les faltaba a los ingenieros con solo MapReduce?"*

**Respuesta esperada:** MapReduce requería que los ingenieros escribieran código Java o Python complejos para consultas que en SQL toman 1 línea. Una consulta como "dame las ventas totales por región del mes pasado" toma 1 línea en SQL pero 50-100 líneas en Java MapReduce. Los analistas de negocio saben SQL pero no Java. La solución fue construir capas de abstracción SQL sobre Hadoop: **Hive** traduce HiveQL → MapReduce, **Impala** ejecuta SQL directamente en HDFS sin MapReduce (más rápido), **Drill** hace SQL sobre cualquier fuente (HDFS, MongoDB, S3) sin esquema previo. La evolución: hacer que Hadoop sea accesible sin programar MapReduce directamente.

---

## BLOQUE 2 — TRANSFORMACIÓN (70 min) {#bloque-2}

---

### SUBTEMA 1: PROBLEMAS DE HADOOP 1.x Y LA SOLUCIÓN: YARN (12 min)

#### Explicación conceptual

**El problema de Hadoop 1.x:** En la arquitectura original, el **JobTracker** era responsable de DOS cosas simultáneamente: (1) gestionar los recursos del clúster (qué nodo tiene CPU/RAM libre) Y (2) coordinar la ejecución de cada job MapReduce. Con cientos de jobs simultáneos y miles de nodos, el JobTracker se convertía en cuello de botella y punto único de fallo.

**La solución: YARN (Yet Another Resource Negotiator) — Hadoop 2.x:**

```
HADOOP 1.x (PROBLEMA):
  ┌─────────────────────────────────┐
  │        JOBTRACKER               │
  │  Gestiona recursos + jobs       │ ← SATURADO con 1000s de jobs
  │  Un solo proceso en el master   │ ← SINGLE POINT OF FAILURE
  └─────────────────────────────────┘

HADOOP 2.x con YARN (SOLUCIÓN):
  ┌──────────────────┐    ┌──────────────────────────────┐
  │ RESOURCE MANAGER │    │ APPLICATION MASTER           │
  │ Solo gestiona    │    │ (uno por job/aplicación)     │
  │ recursos         │    │ Coordina SU propio job       │
  │ CPU/RAM del      │    │ → MapReduce job              │
  │ clúster          │    │ → Spark job                  │
  └──────────────────┘    │ → Tez job                    │
  ┌──────────────────┐    └──────────────────────────────┘
  │ NODE MANAGER     │
  │ (en cada nodo)   │
  │ Ejecuta containers│
  └──────────────────┘
```

**La innovación clave de YARN:** Al separar gestión de recursos de ejecución de aplicaciones, YARN permite que el clúster Hadoop corra **cualquier framework** de procesamiento (no solo MapReduce): Spark, Tez, Flink, Storm — todos compitiendo por recursos del mismo clúster YARN. Esto convirtió a Hadoop en una **plataforma de cómputo distribuido** general, no solo un sistema MapReduce.

#### Ejemplo real: Yahoo! y el salto a YARN

Yahoo! fue el principal usuario de Hadoop 1.x con un clúster de 42,000 nodos. Su JobTracker manejaba 500+ jobs simultáneos y colapsaba frecuentemente. Al migrar a YARN en 2013, el tiempo de respuesta del clúster mejoró 10x y pudieron correr Spark jobs junto a MapReduce jobs en el mismo clúster. Hoy Yahoo! corre sobre YARN: MapReduce para batch, Spark para ML, Storm para streaming — tres frameworks distintos, un solo clúster.

#### Pregunta al grupo #1

> *"YARN permite que Spark, MapReduce y Storm corran en el mismo clúster simultáneamente. ¿Qué ventaja tiene esto para una empresa como el BCP que tiene múltiples equipos de datos?"*

**Respuesta esperada detallada:** Sin YARN, cada framework requeriría su propio clúster de servidores separado: un clúster para batch MapReduce, otro para Spark ML, otro para Storm streaming — triplicando el costo de hardware. Con YARN, los tres equipos (equipo de reportes, equipo de ML, equipo de alertas en tiempo real) comparten el mismo pool de recursos físicos. YARN asigna dinámicamente CPU y RAM según la demanda: si el equipo de ML lanza un job grande a las 2 AM, YARN le da más recursos; cuando el equipo de reportes lanza a las 7 AM, YARN redistribuye. Resultado: 3x menor costo de infraestructura con mejor utilización de recursos.

---

### SUBTEMA 2: HERRAMIENTAS SQL SOBRE HADOOP — HIVE, PIG, IMPALA, DRILL (18 min)

#### Apache Pig — El lenguaje de flujos de datos

**Pig** fue la primera capa de abstracción sobre MapReduce. Usa un lenguaje propio llamado **Pig Latin** que describe flujos de transformación de datos. Un script Pig se compila automáticamente en uno o más jobs MapReduce.

```pig
-- Pig Latin: encontrar ventas mayores a S/1000 por región
-- (equivale a 100+ líneas de Java MapReduce)

ventas = LOAD 'hdfs://datos/ventas.csv' 
         USING PigStorage(',') 
         AS (id:int, region:chararray, monto:float, fecha:chararray);

ventas_grandes = FILTER ventas BY monto > 1000;

por_region = GROUP ventas_grandes BY region;

resultado = FOREACH por_region GENERATE 
    group AS region, 
    SUM(ventas_grandes.monto) AS total;

DUMP resultado;
-- Output: (Lima, 4500000.0) (Arequipa, 1200000.0) ...
```

**¿Por qué Pig es relevante hoy si ya no es tan usado?** El concepto de **Pig Latin** (describe QUÉ transformaciones hacer, no CÓMO implementarlas en paralelo) es exactamente el mismo paradigma de PySpark DataFrames y Spark SQL. Entender Pig ayuda a entender POR QUÉ Spark DataFrames son más intuitivos que MapReduce puro.

> **Estado actual en la industria (2025):** Pig está en modo mantenimiento. Las empresas migraron a PySpark (mismo paradigma, 10x más rápido). Cloudera oficialmente descontinuó Pig en 2022.

---

#### Apache Hive — SQL sobre Hadoop (HiveQL)

**Hive** traduce consultas **HiveQL** (muy similar a SQL) en jobs MapReduce o Spark, permitiendo a analistas sin conocimiento de Java consultar datos en HDFS con SQL familiar.

```sql
-- HiveQL: misma consulta, lenguaje SQL familiar
-- Se ejecuta sobre HDFS vía MapReduce o Tez

CREATE TABLE ventas (
    id INT,
    region STRING,
    monto FLOAT,
    fecha STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
STORED AS TEXTFILE
LOCATION '/datos/ventas/';

-- Consulta analítica:
SELECT region, SUM(monto) AS total_ventas
FROM ventas
WHERE monto > 1000
GROUP BY region
ORDER BY total_ventas DESC;

-- Detrás: Hive compila esto en 1-2 jobs MapReduce
-- Latencia: minutos (no milisegundos)
```

**Hive hoy = Databricks SQL / Spark SQL:** La sintaxis HiveQL es 95% compatible con Spark SQL. Lo que hacemos en Databricks con `spark.sql("SELECT ...")` es Hive evolucionado. Por eso Databricks es el sucesor moderno de Hive: mismo concepto, motor Spark 100x más rápido.

**Comparación Hive vs. SQL tradicional:**

| Aspecto | Hive (sobre HDFS) | PostgreSQL/MySQL |
|---------|------------------|------------------|
| **Latencia consulta** | Segundos a minutos | Milisegundos |
| **Escala de datos** | Petabytes | Terabytes (con tuning) |
| **Esquema** | Schema-on-read (flexible) | Schema-on-write (fijo) |
| **Transacciones ACID** | Limitado (HiveACID) | Completo |
| **Caso de uso ideal** | Análisis batch de Big Data | OLTP, reportes operacionales |

#### Pregunta al grupo #2

> *"Un analista de negocio de RIMAC Seguros sabe SQL muy bien pero nunca programó Java. ¿Por qué Hive fue revolucionario para perfiles como el suyo en 2010, y por qué Databricks SQL es aún mejor en 2025?"*

**Respuesta esperada detallada:** Hive fue revolucionario porque por primera vez un analista de negocio podía consultar petabytes de datos en HDFS sin aprender Java, MapReduce ni configurar nada — solo escribía SELECT y el sistema hacía el trabajo paralelo. Databricks SQL es mejor en 2025 porque: (1) ejecuta sobre Spark (no MapReduce), reduciendo la latencia de minutos a segundos; (2) tiene un editor SQL visual con autocompletado y visualización integrada; (3) soporta Delta Lake con transacciones ACID; (4) tiene conectores directos a Power BI y Tableau; (5) el analista no necesita instalar nada — accede desde el navegador.

---

#### Impala, Dremel y Drill — SQL Interactivo en tiempo real

**El problema de Hive:** Una consulta Hive tardaba 2-5 minutos incluso para preguntas simples porque pasaba por MapReduce. Los analistas querían respuestas en segundos para exploración interactiva.

**La solución — Motores de consulta interactiva:**

| Motor | Origen | Mecanismo | Latencia | Estado actual |
|-------|--------|-----------|----------|---------------|
| **Dremel** | Google (2010) | Columnar, parallel query | Segundos | Evolucionó → BigQuery |
| **Impala** | Cloudera (2012) | SQL directo sobre HDFS (sin MapReduce) | 1-10 seg | Usado en CDH, compite con Presto |
| **Drill** | MapR/Apache (2013) | SQL sin esquema sobre cualquier fuente | 1-30 seg | Open source, menos popular |
| **Presto/Trino** | Facebook (2013) | Motor distribuido multi-fuente | Sub-segundo | **Más usado en industria 2025** |
| **Spark SQL** | Databricks (2014) | In-memory SQL sobre Spark | Sub-segundo | **Dominante en 2025** |

**La evolución que importa:**
```
2010: Dremel (Google) → Paper publicado → Inspiró todo
2012: Impala (Cloudera) → SQL directo sobre HDFS
2013: Presto (Facebook) → Multi-source SQL
2014: Spark SQL → SQL en memoria
2016: BigQuery (Google) → Dremel como servicio
2020: Databricks SQL → Spark SQL + Delta Lake
2025: Starburst/Trino → Open source Presto empresarial
```

**¿Por qué usar Databricks SQL y no instalar Impala?**
- Impala requiere instalación en clúster Cloudera (pesado, costoso, licencia)
- Databricks SQL corre en cloud, gratis con Community Edition
- Rendimiento comparable o superior (Photon Engine de Databricks)
- Misma sintaxis SQL

#### Mini actividad — Diagrama de evolución (3 min)

> *"En parejas, en papel: dibujen una línea de tiempo 2003→2025 con las herramientas SQL de Hadoop que vimos y una flecha mostrando qué reemplazó a qué. Tienen 2 minutos."*

**Respuesta esperada:**
```
2003: Google GFS + MapReduce (papers)
2006: Hadoop open source (Yahoo!)
2008: Pig (Yahoo!) — scripts sobre MapReduce
2010: Hive (Facebook) + Dremel paper (Google)
2012: Impala (Cloudera) — SQL interactivo
2013: Presto (Facebook)
2014: Spark SQL
2016: BigQuery = Dremel como SaaS
2020: Databricks SQL = Spark SQL + Delta
2025: Databricks/Trino/BigQuery dominan
```

---

#### Apache Ambari — Gestión de Clústeres Hadoop

**Ambari** es la interfaz web de gestión para clústeres Hadoop (parte del stack Hortonworks/Cloudera). Permite monitorear, instalar y configurar todos los servicios del ecosistema Hadoop desde un navegador.

**¿Qué hace Ambari?**
- Dashboard de salud del clúster en tiempo real (CPU, RAM, disco por nodo)
- Instalar/desinstalar componentes (HDFS, YARN, Hive, HBase, Kafka, etc.)
- Alertas automáticas cuando un servicio falla
- Configuración centralizada de parámetros
- Logs centralizados de todos los nodos

**Ambari hoy = Databricks UI / AWS EMR Console:**
```
2012-2020: Ambari (Hortonworks HDP)
2020-hoy:  Cloudera Data Platform (CDP) reemplaza Ambari
Alternativas cloud:
  AWS: Amazon EMR Console (EMR = Elastic MapReduce)
  GCP: Dataproc UI
  Azure: HDInsight
  Databricks: Compute + Monitoring UI (lo que usamos)
```

> **Mensaje clave para estudiantes:** No instalen Ambari en local — es para clústeres de producción con decenas de servidores. En Databricks Community, el "Compute" panel con el Spark UI hace exactamente lo mismo para su clúster de aprendizaje.

#### Transferencia de datos: Sqoop y Flume

**Sqoop** (SQL-to-Hadoop): transfiere datos entre bases relacionales (MySQL, Oracle, SQL Server) y HDFS/Hive en ambas direcciones.

```bash
# Importar tabla MySQL a HDFS
sqoop import \
  --connect jdbc:mysql://servidor:3306/ventas_db \
  --username root --password secret \
  --table clientes \
  --target-dir /hdfs/clientes/ \
  --num-mappers 4    # 4 conexiones paralelas a MySQL

# El equivalente moderno en PySpark:
df = spark.read.format("jdbc") \
    .option("url", "jdbc:mysql://servidor:3306/ventas_db") \
    .option("dbtable", "clientes") \
    .option("user", "root").option("password", "secret") \
    .load()
df.write.parquet("/dbfs/clientes/")
```

**Flume**: ingesta de logs y eventos en streaming hacia HDFS/Kafka. Hoy reemplazado por **Apache Kafka** + conectores nativos.

---

### SUBTEMA 3: POR QUÉ LOS RDBMS FALLAN A ESCALA DE BIG DATA (10 min)

#### Limitaciones del modelo relacional a escala Big Data

```
LIMITACIONES DE RDBMS PARA BIG DATA:

1. ESCALABILIDAD VERTICAL (Scale-Up):
   SQL tradicional → comprar servidor más potente
   Big Data → imposible: los datos crecen más rápido que el hardware
   
   Costo: servidor 2x potente ≈ 10x más caro (ley de rendimientos decrecientes)

2. ESQUEMA RÍGIDO (Schema-on-Write):
   ALTER TABLE sobre 1 billón de filas = horas de downtime
   Un tweet en 2008: texto, usuario, fecha
   Un tweet en 2024: texto, usuario, fecha, imágenes, video, encuesta,
                     hilo, mención, hashtag, geolocalización, idioma,
                     fuente, card, sticker... → 50+ campos opcionales
   
3. JOINS A ESCALA:
   JOIN entre dos tablas de 1TB cada una en SQL = horas/días
   Los datos Big Data a menudo no tienen relaciones fijas

4. DISPONIBILIDAD vs. CONSISTENCIA:
   RDBMS prioriza: CONSISTENCIA (ACID)
   Big Data apps necesitan: DISPONIBILIDAD 24/7 global sin downtime
```

#### El Teorema CAP — La Elección Fundamental

**Teorema CAP (Brewer, 2000):** Un sistema distribuido NO puede garantizar simultáneamente las tres propiedades:

```
           CONSISTENCIA (C)
           Todos los nodos ven
           los mismos datos
           al mismo tiempo
                 ▲
                /|\
               / | \
              /  |  \
        CA   /   |   \ CP
            /    |    \
           /     |     \
          /_____|_____\
         ▼             ▼
    DISPONIBILIDAD   PARTICIÓN
         (A)         TOLERANCIA (P)
    El sistema     El sistema sigue
    siempre        funcionando aunque
    responde       la red falle
    
    CA: RDBMS tradicional (MySQL, Oracle)
    CP: HBase, MongoDB (en modo por defecto), Zookeeper
    AP: Cassandra, CouchDB, DynamoDB
```

**ACID vs. BASE:**

| Paradigma | ACID (SQL) | BASE (NoSQL) |
|-----------|-----------|-------------|
| **Significado** | Atomicity, Consistency, Isolation, Durability | Basically Available, Soft state, Eventually consistent |
| **Prioridad** | Consistencia perfecta | Disponibilidad alta |
| **Ejemplo** | Transferencia bancaria (o se completa o no) | Feed de Instagram (está bien si tarda 2s en verse globalmente) |
| **Escala** | Vertical (un servidor más potente) | Horizontal (más nodos) |
| **Latencia** | Baja para pocos datos | Variable pero escala |

**Pregunta al grupo #3:**

> *"Un banco en Perú necesita que cuando transfieres S/1,000 de tu cuenta a otra, el monto se descuente exactamente una vez — ni cero ni dos veces. ¿Elegiría ACID o BASE? ¿Y una red social como TikTok para contar los 'me gusta' de un video?"*

**Respuesta esperada detallada:** El banco necesita **ACID** (Consistencia perfecta): es inaceptable que la transferencia quede en estado intermedio. Si el sistema falla a mitad de la operación, debe revertirse completamente (Atomicidad). Las transacciones financieras son el caso paradigmático de ACID.

TikTok puede usar **BASE** (Disponibilidad alta): si un video tiene 1,234,567 likes y 1,000 usuarios hacen like simultáneamente en diferentes regiones del mundo, está bien que por 2-3 segundos algunos nodos muestren 1,234,580 y otros 1,234,575 — eventualmente convergen al número correcto. La "consistencia eventual" es aceptable aquí. Un error de ±0.001% en el conteo de likes no tiene consecuencias reales. Un error de ±0.001% en transferencias bancarias puede ser millones de soles.

---

### SUBTEMA 4: CLASIFICACIÓN DE BASES DE DATOS NoSQL (20 min)

#### Los 4 tipos principales de NoSQL

```
┌─────────────────────────────────────────────────────────────┐
│              CLASIFICACIÓN NoSQL                             │
├──────────────┬──────────────┬──────────────┬──────────────┐  │
│  KEY-VALUE   │  DOCUMENTO   │   COLUMNAR   │    GRAFO     │  │
│              │              │              │              │  │
│  Redis       │  MongoDB     │  Cassandra   │  Neo4j       │  │
│  DynamoDB    │  CouchDB     │  HBase       │  Amazon      │  │
│  Memcached   │  Firestore   │  ScyllaDB    │  Neptune     │  │
│              │              │              │              │  │
│  Clave →     │  Documento   │  Columnas    │  Nodos y     │  │
│  Valor       │  JSON/BSON   │  agrupadas   │  aristas     │  │
├──────────────┼──────────────┼──────────────┼──────────────┤  │
│ Redis Cloud  │ MongoDB      │ Astra DB     │ Neo4j        │  │
│ Free 30MB    │ Atlas M0     │ Free 80GB    │ AuraDB Free  │  │
│ (gratis)     │ 512MB gratis │ (Cassandra)  │ (gratis)     │  │
└──────────────┴──────────────┴──────────────┴──────────────┘  │
```

---

#### TIPO 1: KEY-VALUE — Redis

**Concepto:** El modelo más simple. Cada dato tiene una clave única y un valor (puede ser string, list, set, hash, sorted set).

```python
# Redis en Python — caso real: sesiones de usuario en Claro Perú
import redis

r = redis.Redis(host='localhost', port=6379)

# Guardar sesión de usuario (expira en 30 minutos)
r.setex(
    name="sesion:usuario_12345",     # Clave única
    time=1800,                        # TTL: 30 minutos
    value='{"nombre":"Juan","plan":"Claro Total","saldo":45.50}'
)

# Recuperar sesión (latencia: < 1ms)
sesion = r.get("sesion:usuario_12345")
print(sesion)
# b'{"nombre":"Juan","plan":"Claro Total","saldo":45.50}'

# ¿Por qué no guardar esto en MySQL?
# Porque Claro tiene 8M de sesiones activas simultáneas.
# Redis responde en <1ms. MySQL tardaría 50-100ms con índices.
```

**Casos de uso:**
- Caché de sesiones de usuario (Pinterest, Twitter)
- Rate limiting (¿cuántas solicitudes hizo esta IP en 1 minuto?)
- Contadores en tiempo real (likes de TikTok)
- Cola de mensajes simple

---

#### TIPO 2: DOCUMENTO — MongoDB (PRINCIPAL DEL LAB)

**Concepto:** Los datos se almacenan como documentos en formato BSON (Binary JSON). Cada documento puede tener estructura diferente. Los documentos similares se agrupan en **colecciones** (equivalente a tablas en SQL).

**Modelo de datos:**

```
SQL (RDBMS):              MongoDB (NoSQL Document):
─────────────────         ──────────────────────────
TABLA: clientes           COLECCIÓN: clientes
┌────┬──────┬──────┐      [
│ ID │Nombre│Email │        {
├────┼──────┼──────┤          "_id": ObjectId("64a1..."),
│  1 │Juan  │j@e   │          "nombre": "Juan García",
│  2 │María │m@e   │          "email": "juan@gmail.com",
└────┴──────┴──────┘          "plan": "Premium",
                              "dispositivos": ["iPhone", "iPad"],
TABLA: planes                 "historial_pagos": [
┌────┬──────┬───────┐           {"mes": "Enero", "monto": 89.90},
│ ID │Plan  │Precio │           {"mes": "Febrero", "monto": 89.90}
├────┼──────┼───────┤         ]
│  1 │Basic │ 39.90 │        },
│  2 │Prem. │ 89.90 │        {
└────┴──────┴───────┘          "_id": ObjectId("64a2..."),
                               "nombre": "María López",
→ Necesitas JOIN para          "email": "maria@gmail.com"
  saber el plan de Juan        // Sin plan ni historial (esquema flexible)
                             }
                           ]
```

**Operaciones MongoDB con Python (pymongo):**

```python
from pymongo import MongoClient

# Conectar a MongoDB Atlas (cloud gratuito)
client = MongoClient("mongodb+srv://usuario:password@cluster.mongodb.net/")
db = client["bigdata_ua"]        # Base de datos
clientes = db["clientes"]         # Colección

# INSERT — equivale a INSERT INTO SQL
nuevo_cliente = {
    "nombre": "Carlos Quispe",
    "departamento": "Arequipa",
    "plan": "Claro Total",
    "consumo_gb": 45.2,
    "facturas": [
        {"mes": "Enero 2025", "monto": 79.90, "pagado": True},
        {"mes": "Febrero 2025", "monto": 79.90, "pagado": False}
    ]
}
resultado = clientes.insert_one(nuevo_cliente)
print(f"Insertado con ID: {resultado.inserted_id}")

# FIND — equivale a SELECT WHERE SQL
clientes_arequipa = clientes.find(
    {"departamento": "Arequipa", "consumo_gb": {"$gt": 30}}
)
for c in clientes_arequipa:
    print(f"{c['nombre']}: {c['consumo_gb']} GB")

# AGGREGATION — equivale a GROUP BY SQL (pero más poderoso)
pipeline = [
    {"$group": {
        "_id": "$departamento",
        "total_clientes": {"$sum": 1},
        "consumo_promedio": {"$avg": "$consumo_gb"}
    }},
    {"$sort": {"total_clientes": -1}},  # Ordenar descendente
    {"$limit": 5}                        # Top 5
]
resultado = list(clientes.aggregate(pipeline))
for r in resultado:
    print(f"{r['_id']}: {r['total_clientes']} clientes, {r['consumo_promedio']:.1f} GB prom.")
```

---

#### TIPO 3: COLUMNAR — Apache Cassandra

**Concepto:** Los datos se organizan por columnas agrupadas en "familias de columnas". Optimizado para escrituras masivas y lecturas por rango temporal. No tiene un maestro único — todos los nodos son iguales (arquitectura "ring").

```
CASSANDRA — Modelo de datos (Claro: historial de llamadas):

Partition Key: número_telefono "987654321"
─────────────────────────────────────────────────────
Clustering Key: timestamp (ordenado)
│
├─ 2025-01-15 10:23:00 │ destino: "998877665" │ duración: 120s │ costo: 0.20
├─ 2025-01-15 11:45:00 │ destino: "Lima_fijo" │ duración: 45s  │ costo: 0.10
└─ 2025-01-16 09:00:00 │ destino: "998877665" │ duración: 300s │ costo: 0.50

Consulta óptima: "Dame todas las llamadas del número 987654321"
→ Busca partition key → datos ya ordenados por timestamp
→ Latencia: < 5ms incluso con BILLONES de registros
```

**¿Por qué Cassandra y no MongoDB para llamadas?**
- Yape: 5M transacciones/día → MongoDB maneja bien
- Claro: 500M eventos de red/día → Cassandra (diseñado para esto)
- Regla: escrituras muy masivas y continuas + consultas por clave temporal → Cassandra

---

#### TIPO 4: GRAFO — Neo4j

**Concepto:** Almacena entidades como **nodos** y sus relaciones como **aristas**. Optimizado para consultas que navegan relaciones complejas (detección de fraude, redes sociales, recomendaciones).

```
Grafo de transacciones sospechosas (Detección de fraude):

  [Cuenta A] ──────────────► [Cuenta B]
      │                            │
      │ S/5000                     │ S/4900
      │ (21:00)                    │ (21:05)
      ▼                            ▼
  [Cuenta C] ◄────────────── [Cuenta D]
                 S/4800
                 (21:08)

Neo4j detecta: "Cuenta A → B → D → C en 8 minutos"
→ Patrón de lavado de dinero circular

SQL: 3 JOINs recursivos en millones de filas = imposible en tiempo real
Neo4j: recorre el grafo en milisegundos
```

#### Pregunta al grupo #4

> *"El equipo de SUNAT quiere construir un sistema que detecte empresas fantasma: empresas que facturan entre sí para inflar gastos y reducir impuestos. La red puede tener 10 niveles de profundidad: empresa A → B → C → ... → A. ¿Qué tipo de base de datos NoSQL usarían? ¿Por qué no SQL?"*

**Respuesta esperada detallada:** **Neo4j (base de datos de grafo)**. Las relaciones de facturación entre empresas son exactamente un grafo dirigido. Con SQL, detectar un ciclo de N niveles requiere N JOINs recursivos sobre millones de empresas — la consulta puede tardar horas. Neo4j almacena las relaciones como aristas nativas: la consulta "encuentra todos los ciclos de longitud <= 10 que involucren a empresa X" se ejecuta en milisegundos porque el motor de grafo sigue las aristas sin necesidad de JOINs. SUNAT ya utiliza tecnología de análisis de grafos para su programa de Cumplimiento Tributario y detección de empresas relacionadas.

#### Mini actividad — Clasificar casos de uso (4 min)

> *"Para cada caso, digan qué tipo NoSQL usarían y por qué. Tienen 3 minutos:"*

1. Almacenar el carrito de compras de 5M usuarios de Rappi que cambia cada segundo
2. Guardar el catálogo de 500K productos de Falabella con atributos variables
3. Analizar la red de seguidores de Instagram Perú (200M conexiones)
4. Registrar 1M de lecturas/hora de sensores de temperatura en Lima

**Respuestas:**
1. **Redis** (Key-Value) — carrito es una sesión temporal, acceso por userID en < 1ms
2. **MongoDB** (Documento) — cada producto tiene atributos diferentes, esquema flexible
3. **Neo4j** (Grafo) — relaciones de seguidores son el dato principal, no los usuarios
4. **Cassandra** (Columnar) — escrituras masivas continuas ordenadas por timestamp

---

## RECESO (20 min) {#receso}

> *"Perfecto — 20 minutos de descanso. Al regresar, van a implementar su primera base de datos MongoDB Atlas en vivo. Asegúrense de tener su notebook abierto."*

---

## BLOQUE 3 — PRÁCTICA Y CIERRE (50 min) {#bloque-3}

### 4. PRÁCTICA (40 min)

#### a) CASO PRÁCTICO GRUPAL — Claro Perú NoSQL Architecture (25 min)

**Escenario:**

> *"Acaban de ser contratados como consultores de data engineering en Claro Perú. El Director de Tecnología presenta el problema: Claro tiene 8 millones de suscriptores móviles en el Perú. Actualmente toda la información de clientes está en una base de datos Oracle relacional con 45 tablas y 120 GB de datos. El sistema lleva 15 años en producción.*
>
> *Los problemas actuales:*
> - El portal web carga el perfil del cliente en 8 segundos (los clientes se quejan)
> - La app de Claro se cae en las noches de partido de la selección (pico de 2M conexiones simultáneas)
> - Agregar un nuevo campo al sistema (como "puntos de fidelización") requiere 3 semanas de trabajo y testing
> - El historial de llamadas de los últimos 3 años (18 mil millones de registros) hace que las consultas de facturación tarden 4 minutos
>
> *El director pregunta: ¿Qué arquitectura NoSQL proponen para resolver estos 4 problemas? No tienen que migrar todo — pueden tener una arquitectura híbrida.*"

**Preguntas de análisis para los grupos:**

1. Para cada problema, identifica qué tipo de NoSQL (Key-Value, Documento, Columnar, Grafo) lo resuelve mejor y por qué.
2. ¿Mantienen Oracle para algo? ¿Para qué?
3. Dibuja el diagrama de arquitectura con todos los componentes.
4. ¿Qué riesgos tiene migrar de Oracle a NoSQL para un sistema crítico como este?

**Producto esperado:** Diagrama en papel + tabla de decisión tipo/problema/justificación

**Preguntas de andamiaje del docente:**
- *"¿Por qué el portal web carga en 8 segundos? ¿Qué tipo de consulta SQL creen que está haciendo?"* (JOIN entre múltiples tablas por cada carga de perfil)
- *"¿Qué datos del cliente necesita la app para mostrar el perfil en < 500ms?"* (nombre, plan, saldo — datos simples, acceso por ID)
- *"Si el historial de llamadas tiene 18B de registros, ¿qué tipo NoSQL manejaría escrituras de millones de llamadas/hora?"*

**Puesta en común — Respuesta modelo:**

```
ARQUITECTURA HÍBRIDA CLARO PERÚ:

PROBLEMA 1: Portal web lento (8 seg)
→ SOLUCIÓN: Redis (Key-Value)
  Cache del perfil del cliente: key="perfil:12345678", value="{JSON del cliente}"
  Primera carga: consulta Oracle → guarda en Redis (TTL: 15 min)
  Cargas siguientes: Redis → < 50ms
  
PROBLEMA 2: App cae en picos
→ SOLUCIÓN: MongoDB Atlas (Document)
  Perfil completo del cliente en MongoDB (sin JOINs)
  MongoDB escala horizontalmente con auto-sharding
  Auto-scaling: de 5 a 50 nodos en minutos según demanda

PROBLEMA 3: Agregar nuevos campos (3 semanas)
→ SOLUCIÓN: MongoDB (Documento, esquema flexible)
  Agregar "puntos_fidelizacion" a todos los documentos:
  db.clientes.updateMany({}, {$set: {puntos: 0}})
  → Listo en segundos, sin downtime, sin ALTER TABLE

PROBLEMA 4: Historial de llamadas lento (18B registros)
→ SOLUCIÓN: Cassandra (Columnar)
  Partition Key: número_telefono
  Clustering Key: timestamp
  Consulta "dame llamadas del mes de enero de número X": < 100ms

ORACLE SE MANTIENE PARA:
  - Facturación (ACID estricto: cobro exacto o no cobro)
  - Contabilidad y reportes regulatorios (OSIPTEL)
  - Contratos y datos legales de suscriptores

RIESGOS:
  - Consistencia eventual: ¿qué pasa si Redis y Oracle muestran saldos diferentes?
  → Solución: Redis se invalida cuando Oracle actualiza el saldo
  - Complejidad operacional: 4 sistemas en lugar de 1
  → Solución: equipo de DBA capacitado en cada tecnología
  - Migración de datos: 18B de registros de Oracle a Cassandra
  → Plan: migración gradual por año, validación paralela
```

---

#### b) EJERCICIO INDIVIDUAL — Primera consulta en MongoDB Atlas (15 min)

> *"Ahora implementan. Abran su navegador y van a Atlas: mongodb.com/atlas → 'Try Free'. Si ya tienen cuenta, conecten su cluster M0."*

**Instrucciones paso a paso:**

```
1. Ir a: mongodb.com/atlas → "Try Free" → registrarse con cuenta Google
2. Crear cluster: "Create" → M0 Free → región: AWS São Paulo
   Nombre del cluster: "BigDataUA-TuApellido"
3. Crear usuario: Security → Database Access → Add user
   Username: bigdata_user | Password: BigData2026!
4. Abrir red: Security → Network Access → Add IP → 0.0.0.0/0 (Allow all)
5. Ir a: Clusters → "Browse Collections" → Add My Own Data
   Database: bigdata_s3 | Collection: empresas_sunat
6. Insert Document → pegar el JSON del ejercicio
```

**Documento a insertar (datos de empresa SUNAT simulada):**

```json
{
  "ruc": "20123456789",
  "razon_social": "Tecnología Andina SAC",
  "departamento": "Lima",
  "sector": "Tecnología",
  "regimen_tributario": "Régimen General",
  "ventas_anuales": {
    "2023": 850000,
    "2024": 1200000
  },
  "num_empleados": 45,
  "estado": "ACTIVO",
  "productos": ["Software", "Consultoría", "Soporte"],
  "contacto": {
    "email": "contacto@tecandina.com",
    "telefono": "01-4567890"
  }
}
```

**Criterio de éxito:** El estudiante puede mostrar en pantalla el documento insertado en el Atlas Data Explorer con todos los campos, incluyendo el `_id` generado automáticamente por MongoDB.

---

### 5. CIERRE (10 min)

#### a) SÍNTESIS COLABORATIVA (4 min)

**Pregunta de cierre 1:**
> *"¿Cuál es la diferencia más importante entre YARN en Hadoop 2.x y el JobTracker de Hadoop 1.x?"*

**Respuesta esperada:** YARN separó la gestión de recursos (ResourceManager) de la ejecución de jobs (ApplicationMaster por job). Esto elimina el cuello de botella del JobTracker único y permite que múltiples frameworks (Spark, MapReduce, Tez) compartan el mismo clúster simultáneamente.

**Pregunta de cierre 2:**
> *"Si alguien les pregunta '¿cuándo usar MongoDB y cuándo Cassandra?', ¿qué responderían en 30 segundos?"*

**Respuesta esperada:** MongoDB cuando los documentos tienen estructura variable y necesitan consultas flexibles por cualquier campo. Cassandra cuando hay millones de escrituras por segundo con una clave de acceso predecible (partition key) y los datos son series temporales.

**Pregunta de cierre 3:**
> *"¿Qué es el Teorema CAP y por qué importa cuando diseñas una arquitectura NoSQL?"*

**Respuesta esperada:** CAP dice que un sistema distribuido solo puede garantizar 2 de 3: Consistencia, Disponibilidad, Tolerancia a particiones. Importa porque define qué base de datos elegir: si el negocio no puede tolerar datos inconsistentes (banco) → elegir CP (MongoDB). Si el negocio necesita disponibilidad 24/7 aunque los datos tarden en sincronizarse (redes sociales) → elegir AP (Cassandra).

---

#### b) METACOGNICIÓN (3 min)

> *"En silencio, en su notebook, respondan:"*
> 1. ¿Qué tipo de NoSQL usarías para el proyecto de tu grupo? ¿Por qué?
> 2. ¿Qué parte de la sesión de hoy te costó más entender?
> 3. Si tuvieras que explicar el Teorema CAP a alguien de Contabilidad, ¿qué analogía usarías?

---

#### c) TAREA Y PUENTE (3 min)

> *"La próxima semana — Semana 4 — es el Examen Parcial (EC) y vemos NewSQL: CockroachDB y sistemas que tienen lo mejor de SQL y NoSQL juntos. La tarea es:*
>
> *1. Completar el Laboratorio en Casa (S3_LAB_CASA_BIGDATA.md) — implementar MongoDB con Python en Databricks o Colab. 2 horas.*
>
> *2. Crear su cuenta MongoDB Atlas si no la tienen ya: mongodb.com/atlas — es gratis.*
>
> *3. Leer el README de su repo de proyecto grupal y actualizar la sección de base de datos según lo que vimos hoy: ¿usarán MongoDB, Cassandra, Redis, Neo4j, o combinación?*
>
> *El que termine el lab con todos los puntos de verificación tiene ventaja para el examen parcial — las preguntas del EC incluyen NoSQL."*

---

## GUION VERBAL SUGERIDO {#guion}

**Al iniciar la clase:**
> *"Hoy tienen dos grandes aprendizajes. El primero es histórico: cómo la industria evolucionó Hadoop para hacerlo útil más allá de Java MapReduce. El segundo es práctico: van a implementar su primera base de datos MongoDB, que es la tecnología NoSQL más contratada en LinkedIn Perú según búsquedas de empleo Data Engineer 2025."*

**Al explicar el CAP Theorem:**
> *"El CAP Theorem suena teórico pero es una decisión de arquitectura que cambia todo. ¿Prefiero que mis datos sean siempre correctos o siempre disponibles? No pueden tener los dos en un sistema distribuido. Los bancos eligen correcto. Las redes sociales eligen disponible."*

**Al mostrar MongoDB:**
> *"MongoDB no tiene tablas. No tiene columnas fijas. No tiene esquemas. Esto suena a caos — ¿cómo organizas algo sin estructura? La respuesta: cada documento lleva su propia estructura. Es como la diferencia entre un formulario impreso con campos fijos y una hoja en blanco. El formulario es SQL. La hoja en blanco es MongoDB."*

**Al cerrar:**
> *"Hoy diseñaron arquitecturas que empresas como Claro, BCP y SUNAT están implementando ahora mismo. La diferencia entre ustedes y un ingeniero de 5 años de experiencia no es el conocimiento — es que él lo hizo 1,000 veces en producción. El laboratorio de casa es donde empiezan a contar esas repeticiones."*

---

## CASOS REALES RECOMENDADOS {#casos-reales}

1. **Twitter y la "Fail Whale" (2012-2013):** Twitter creció de MySQL a Manhattan (Cassandra) después de años de la famosa "Fail Whale" (error de servicio no disponible). Con MySQL, los queries de timeline tardaban segundos y los picos de tráfico (mundiales de fútbol) colapsaban el sistema. Con Cassandra, el timeline se sirve en < 5ms para 500M de usuarios. Fuente: Twitter Engineering Blog.

2. **LinkedIn y Neo4j para "Personas que quizás conozcas":** LinkedIn analiza una red de 900M de profesionales para sugerir conexiones de 2do y 3er grado. Con SQL, el JOIN recursivo para encontrar "amigos de amigos de amigos" sobre 900M nodos es imposible en tiempo real. Con Neo4j, la consulta "dame todas las conexiones a 3 grados de Juan García" tarda milisegundos. Fuente: LinkedIn Engineering.

3. **SUNAT Perú y la facturación electrónica:** En 2018, SUNAT migró el sistema de facturación electrónica a una arquitectura que incluye bases NoSQL para manejar 3M+ de facturas electrónicas al día. El sistema anterior con SQL puro no podía procesar el volumen en tiempo real. Fuente: SUNAT Informe de Modernización 2019.

4. **Uber y Cassandra para geolocalización:** Uber usa Cassandra para almacenar la posición GPS de millones de conductores que se actualiza cada 4 segundos. Con factor de escritura masivo (millones de updates/segundo) y consulta por driver_id + timestamp, Cassandra es la única opción viable. Fuente: Uber Engineering Blog 2016.

5. **BCP Yape y Redis para rate limiting:** Cuando lanzaron Yape, el BCP implementó Redis para controlar que un usuario no pudiera enviar más de X transferencias por minuto (anti-fraude). Redis procesa 1M operaciones/segundo por instancia — ningún RDBMS puede hacer eso. Fuente: BCP Tech Blog (LinkedIn).

---

## EVALUACIÓN FORMATIVA {#evaluacion}

| Momento | Técnica | Indicador de logro |
|---------|---------|-------------------|
| Rompe-hielo | Respuestas intuitivas | Activa conocimiento previo de bases de datos |
| Pregunta #1 YARN | Respuesta oral | Menciona ResourceManager + ApplicationMaster separados |
| Pregunta #2 Hive | Respuesta oral | Diferencia latencia (minutos) vs. SQL normal (ms) |
| Pregunta #3 CAP | Respuesta oral | Da ejemplo correcto de banco (CP) vs. red social (AP) |
| Mini actividad Clasificar | Respuestas escritas | Clasifica correctamente los 4 casos de uso |
| Caso grupal Claro | Diagrama producido | Incluye 4 tipos NoSQL + justificación + Oracle para ACID |
| Ejercicio individual | Documento en Atlas | Documento insertado visible con `_id` generado |

---

## STACK GRATUITO INDUSTRIA 2025 — PARA SEMANA 3 {#stack}

| Herramienta | Reemplaza | Gratis | Link |
|-------------|----------|--------|------|
| **Databricks SQL** | Hive + Impala | Community Edition | community.cloud.databricks.com |
| **MongoDB Atlas M0** | Oracle/MySQL NoSQL | 512 MB permanente | mongodb.com/atlas |
| **Astra DB (DataStax)** | Cassandra | 80 GB gratis | astra.datastax.com |
| **Redis Cloud Free** | Memcached/Redis local | 30 MB gratis | redis.com/try-free |
| **Neo4j AuraDB Free** | Neo4j local | 200K nodos gratis | neo4j.com/aura |
| **Google Colab** | Todo lo anterior | CPU+GPU gratis | colab.research.google.com |
| **MongoDB Compass** | CLI mongo | Gratis | mongodb.com/compass |

> **Por qué MongoDB Atlas y no local:** Instalar MongoDB local requiere configurar replicaset, autenticación, binpath. Atlas lo hace automático, tiene UI web para explorar datos, y la versión M0 es gratuita permanentemente. En industria, 90% de los MongoDB de producción están en Atlas o en cloud.

---

## REFERENCIAS APA 7 {#referencias}

Fowler, M., & Sadalage, P. J. (2013). *NoSQL distilled: A brief guide to the emerging world of polyglot persistence*. Addison-Wesley. https://doi.org/10.5555/2380992

Chodorow, K. (2013). *MongoDB: The definitive guide* (2nd ed.). O'Reilly Media. https://www.oreilly.com/library/view/mongodb-the-definitive/9781449344795/

Brewer, E. (2012). CAP twelve years later: How the "rules" have changed. *IEEE Computer, 45*(2), 23–29. https://doi.org/10.1109/MC.2012.37

White, T. (2015). *Hadoop: The definitive guide* (4th ed.). O'Reilly Media. [Cap. 4: YARN]

Hewitt, E. (2010). *Cassandra: The definitive guide*. O'Reilly Media. https://www.oreilly.com/library/view/cassandra-the-definitive/9781098115159/

Robinson, I., Webber, J., & Eifrem, E. (2015). *Graph databases: New opportunities for connected data* (2nd ed.). O'Reilly Media. https://neo4j.com/graph-databases-book/

Kleppmann, M. (2017). *Designing data-intensive applications*. O'Reilly Media. [Cap. 2: Data Models and Query Languages] https://dataintensive.net/

---

*Big Data DD283 | Universidad Autónoma del Perú | Semana 3 | 2026-1*
