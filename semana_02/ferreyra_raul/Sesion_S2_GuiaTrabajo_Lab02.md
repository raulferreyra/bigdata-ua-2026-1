# SEMANA 2 — COMPUTACIÓN EN LA NUBE + HADOOP + HDFS + MAPREDUCE + HBASE

## Big Data (DD283) | Universidad Autónoma del Perú

---

# PARTE A — GUÍA DE SESIÓN

**Duración**: 3 horas  
**Resultado**: El estudiante describe una propuesta de arquitectura de Big Data para la nube y procesa un archivo distribuido usando Hadoop con integridad.

## APERTURA (15 min) — Warm-Up

**Pregunta gancho**:
> "La semana pasada diseñaron una arquitectura. Hoy van a ver cómo funciona por dentro. Si tuvieras que guardar 1 MILLÓN de archivos, ¿los guardarías todos en la misma carpeta? ¿Por qué no?"

**Demostración de velocidad** (ejecutar en vivo):

```bash
# En local: contar palabras en un archivo pequeño vs. distribuido
# Mostrar el tiempo con time command
time cat grande.txt | tr ' ' '\n' | sort | uniq -c | sort -rn | head -10
```

Pregunta: "¿Cómo escala esto si el archivo tiene 1 TB?"

## TEORÍA (45 min)

### Bloque 1: Cloud para Big Data (15 min)

**Los 3 grandes proveedores**:

| | AWS | Azure | GCP |
| -- | ----- | ------- | ----- |
| Servicio Big Data | EMR | HDInsight | Dataproc |
| Data Warehouse | Redshift | Synapse | BigQuery |
| Data Lake | S3 | ADLS | Cloud Storage |
| Streaming | Kinesis | Event Hub | Pub/Sub |
| Free tier | 1 año | 200 USD crédito | 300 USD crédito |

**¿Qué es la Computación en la Niebla (Fog Computing)?**

- Edge Computing: procesamiento cerca de la fuente de datos
- Ejemplo: cámara de seguridad con IA local en una tienda → no necesita enviar video completo a la nube
- Reduce latencia y ancho de banda

### Bloque 2: HADOOP — El ecosistema (20 min)

**La historia de Hadoop** (contar como historia):
> "En 2003, Google tenía un problema: tenía más datos de los que nadie había tenido. Publicaron 2 papers: GFS (Google File System) y MapReduce. Doug Cutting y Mike Cafarella los leyeron y crearon Hadoop en su tiempo libre. El nombre viene del elefante de juguete del hijo de Doug."

**Los 4 pilares de Hadoop**:

```sh
HADOOP ECOSYSTEM
├── HDFS (Hadoop Distributed File System)
│   ├── NameNode: el directorio (sabe dónde están los datos)
│   └── DataNodes: donde viven los datos reales (bloques de 128MB)
│
├── MapReduce (el motor de procesamiento)
│   ├── Map: divide y procesa en paralelo
│   └── Reduce: combina los resultados
│
├── YARN (Yet Another Resource Negotiator)
│   ├── ResourceManager: administra los recursos del cluster
│   └── NodeManager: administra recursos por nodo
│
└── Common (librerías compartidas)
```

**HDFS en detalle — dibujar en pizarrón**:

```sh
Cliente quiere guardar un archivo de 300MB:
  → HDFS lo divide en bloques de 128MB
  → Bloque 1 (128MB): guardado en Nodo1, Nodo3, Nodo5 (3 réplicas)
  → Bloque 2 (128MB): guardado en Nodo2, Nodo4, Nodo6 (3 réplicas)  
  → Bloque 3 (44MB):  guardado en Nodo1, Nodo4, Nodo2 (3 réplicas)

Si Nodo1 falla → los datos siguen en Nodo3 y Nodo5 → SIN PÉRDIDA DE DATOS
```

**MapReduce — Word Count como ejemplo canónico**:

```txt
TEXTO: "el cielo es azul el sol es amarillo el cielo brilla"

MAP (divide y cuenta):
  Nodo1: "el"→1, "cielo"→1, "es"→1, "azul"→1
  Nodo2: "el"→1, "sol"→1, "es"→1, "amarillo"→1  
  Nodo3: "el"→1, "cielo"→1, "brilla"→1

SHUFFLE (reagrupa por clave):
  "el" → [1,1,1]
  "cielo" → [1,1]
  "es" → [1,1]

REDUCE (suma):
  "el" → 3
  "cielo" → 2
  "es" → 2
  "azul" → 1, "sol" → 1, "amarillo" → 1, "brilla" → 1
```

### Bloque 3: HBase (10 min)

**HBase = NoSQL columnar sobre HDFS**:

- Inspirado en Bigtable de Google
- Ideal para: datos de series de tiempo, logs, sensor data
- NO es reemplazante de SQL — es complementario

**Cuándo usar HBase vs. MongoDB vs. HDFS**:

| Caso de uso | HBase | MongoDB | HDFS |
| ------------- | ------- | --------- | ------ |
| Datos de IoT en tiempo real | ✓✓ | ✓ | ✗ |
| Documentos JSON | ✗ | ✓✓ | ✗ |
| Análisis histórico masivo | ✓ | ✗ | ✓✓ |
| Búsqueda full-text | ✗ | ✗ | ✗ (usar Elasticsearch) |

## DEMO EN VIVO (50 min)

### Demo 1: Hadoop con Docker (30 min)

**Prerequisito**: Docker Desktop instalado y corriendo

```bash
# Paso 1: Descargar imagen de Hadoop
docker pull sequenceiq/hadoop-docker:2.7.0

# Paso 2: Iniciar contenedor
docker run -it sequenceiq/hadoop-docker:2.7.0 /etc/bootstrap.sh -bash

# Dentro del contenedor:
# Paso 3: Ver el sistema de archivos HDFS
hdfs dfs -ls /

# Paso 4: Crear directorio en HDFS
hdfs dfs -mkdir /datos_curso
hdfs dfs -mkdir /datos_curso/entrada

# Paso 5: Crear archivo de texto local
echo "el big data es el futuro el futuro es ahora ahora aprendemos big data" > texto_prueba.txt
cat texto_prueba.txt

# Paso 6: Subir archivo a HDFS
hdfs dfs -put texto_prueba.txt /datos_curso/entrada/

# Paso 7: Verificar que se subió
hdfs dfs -ls /datos_curso/entrada/
hdfs dfs -cat /datos_curso/entrada/texto_prueba.txt

# Paso 8: Ejecutar Word Count (MapReduce incluido en Hadoop)
hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.0.jar \
  wordcount /datos_curso/entrada/ /datos_curso/salida/

# Paso 9: Ver resultados
hdfs dfs -cat /datos_curso/salida/part-r-00000
```

**Output esperado**:

```sh
ahora   2
aprendemos  1
big     2
data    2
el      3
es      2
futuro  2
```

### Demo 2: PySpark como alternativa a MapReduce (20 min)

*(Para mostrar que Spark es más fácil)*

```python
# En Google Colab — instalar PySpark
!pip install pyspark -q

from pyspark.sql import SparkSession
from pyspark import SparkContext

# Crear contexto Spark
sc = SparkContext('local', 'WordCount')
spark = SparkSession(sc)

# Crear datos
texto = ["el big data es el futuro el futuro es ahora ahora aprendemos big data"]

# MapReduce en Spark (mucho más simple)
palabras = sc.parallelize(texto) \
    .flatMap(lambda line: line.split(" ")) \
    .map(lambda word: (word, 1)) \
    .reduceByKey(lambda a, b: a + b) \
    .sortBy(lambda x: -x[1])

print("=== WORD COUNT CON SPARK ===")
for palabra, count in palabras.collect():
    print(f"  {palabra}: {count}")

sc.stop()
```

## TRABAJO COLABORATIVO (30 min)

**Actividad**: "Procesar datos del sector que tu grupo eligió para el proyecto"

Cada grupo debe:

1. Tomar el problema de su proyecto
2. Diseñar el flujo MapReduce que necesitarían: ¿Cuál sería el Map? ¿Cuál sería el Reduce?
3. Implementar un mini Word Count con los datos de su dominio (ej: contar palabras en tweets sobre su empresa)

## CIERRE (20 min)

**Tarea semana 3**:

1. Completar Guía de Trabajo S2 (preguntas teóricas)
2. Completar el Lab S2 (Word Count con Hadoop/PySpark)
3. Instalar MongoDB Compass o crear cuenta en MongoDB Atlas
4. Leer: Capítulo de Hadoop del libro "Big Data con Python" de Rafael Caballero

---

# PARTE B — GUÍA DE TRABAJO ESTUDIANTE S2

**Nombre**: Raúl Ferreyra | **Fecha entrega**: Antes de Semana 3 | **Individual**

## PREGUNTAS TEÓRICAS (10 preguntas)

### P1: HDFS vs. Sistema de Archivos Tradicional

Explica cómo almacena un archivo de 1 GB el sistema de archivos de tu laptop vs. cómo lo almacenaría HDFS. Usa un diagrama o esquema.

**Respuesta:**

**Sistema de archivos tradicional (laptop — NTFS/ext4):**
El archivo de 1 GB se almacena en un único disco físico. El sistema operativo lo divide en clusters (unidades mínimas de asignación, típicamente 4 KB), los registra en una tabla de asignación (MFT en NTFS o inode en ext4) y los escribe secuencialmente o fragmentados en el mismo disco. Si ese disco falla, el archivo se pierde por completo.

```sh
LAPTOP (disco único):
┌─────────────────────────────────┐
│  DISCO LOCAL (1 TB)             │
│  ┌─────────────────────────┐    │
│  │ archivo.dat (1 GB)      │    │
│  │ clusters 4KB × 262144   │    │
│  │ [todo en el mismo disco]│    │
│  └─────────────────────────┘    │
│  MFT/inode: metadata local      │
└─────────────────────────────────┘
Riesgo: disco falla → datos perdidos
```

**HDFS (clúster distribuido):**
El archivo de 1 GB se divide en 8 bloques de 128 MB. Cada bloque se replica 3 veces en DataNodes distintos. El NameNode mantiene el mapa de qué bloque está en qué nodo (metadata). Si un DataNode falla, los otros 2 que tienen la réplica cubren la pérdida automáticamente.

```sh
HDFS (clúster distribuido):
NameNode ── metadata: bloque→[nodo_A, nodo_C, nodo_E]
    │
    ├── Bloque 1 (128MB) → Nodo1, Nodo3, Nodo5
    ├── Bloque 2 (128MB) → Nodo2, Nodo4, Nodo6
    ├── Bloque 3 (128MB) → Nodo1, Nodo4, Nodo2
    ├── Bloque 4 (128MB) → Nodo3, Nodo5, Nodo6
    ├── Bloque 5 (128MB) → Nodo2, Nodo1, Nodo4
    ├── Bloque 6 (128MB) → Nodo6, Nodo3, Nodo5
    ├── Bloque 7 (128MB) → Nodo4, Nodo2, Nodo1
    └── Bloque 8 (128MB) → Nodo5, Nodo6, Nodo3

Nodo1 falla → bloques siguen disponibles en réplicas → SIN PÉRDIDA
```

La diferencia fundamental es que HDFS sacrifica el acceso aleatorio rápido (no está optimizado para leer bytes arbitrarios de un archivo) a cambio de alta tolerancia a fallos y capacidad de escalar horizontalmente añadiendo más DataNodes sin rediseñar el sistema.

---

### P2: NameNode y DataNodes

¿Qué pasaría si el NameNode de un clúster Hadoop falla? ¿Cómo soluciona Hadoop este problema? (pista: busca "Hadoop HA - High Availability")

**Respuesta:**

**Si el NameNode falla en un clúster sin HA:**
El clúster HDFS queda completamente inoperativo. El NameNode es el único componente que sabe dónde están almacenados los bloques de datos — es el directorio central de todo el sistema de archivos. Sin él, los DataNodes existen pero nadie sabe qué datos contienen ni cómo acceder a ellos. Los datos no se pierden físicamente (siguen en los DataNodes), pero son inaccesibles hasta que el NameNode se recupere. Este es el clásico punto único de falla (SPOF — Single Point of Failure).

En organizaciones donde yo mismo lo he observado con 13 años de experiencia, este tipo de dependencia en un único componente crítico sin redundancia es exactamente el mismo anti-patrón que encontré en la Universidad Norbert Wiener con sus sistemas de datos: todo centralizado en un único servidor sin failover, lo que significaba que cualquier falla dejaba a toda la institución sin acceso a la información.

**Solución: Hadoop High Availability (HA)**

Hadoop 2.x introduce el modo HA con la siguiente arquitectura:

```sh
┌─────────────────────────────────────────────────┐
│              HADOOP HA ARCHITECTURE              │
│                                                  │
│  ┌──────────────┐       ┌──────────────────┐    │
│  │ NameNode     │       │ NameNode         │    │
│  │ ACTIVO       │◀─────▶│ STANDBY          │    │
│  │ (escribe)    │       │ (solo lectura)   │    │
│  └──────┬───────┘       └────────┬─────────┘    │
│         │                        │               │
│         └──────────┬─────────────┘               │
│                    ▼                             │
│  ┌─────────────────────────────────────────┐    │
│  │  JournalNodes (mínimo 3)                │    │
│  │  Edit log compartido en quórum          │    │
│  └─────────────────────────────────────────┘    │
│                    │                             │
│  ┌─────────────────▼───────────────────────┐    │
│  │  ZooKeeper                              │    │
│  │  Detecta falla del NameNode Activo      │    │
│  │  Promueve Standby → Activo en segundos  │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

El NameNode Activo escribe todos los cambios en los JournalNodes. El Standby lee esos cambios en tiempo real y mantiene su estado sincronizado. Cuando ZooKeeper detecta que el Activo falla, el Standby toma el control automáticamente (failover automático) en cuestión de segundos, sin pérdida de datos ni intervención manual.

---

### P3: El algoritmo MapReduce

Explica con un ejemplo diferente al Word Count (usa algo de tu industria) cómo funcionaría el patrón Map → Shuffle → Reduce.

**Respuesta:**

**Caso: Detección de segmentos congestionados en Lima (proyecto Smart Traffic)**

El problema: dado un dataset de 48 millones de puntos GPS diarios de 50,000 vehículos en Lima, identificar qué segmentos de calle tienen velocidad promedio menor a 20 km/h (congestión) en las últimas 2 horas.

```sh
DATOS DE ENTRADA (puntos GPS):
  (v_001, 08:05:12, -12.046, -77.031, vel=15, seg="AV_JAVIER_PRADO_KM3")
  (v_002, 08:05:45, -12.046, -77.030, vel=12, seg="AV_JAVIER_PRADO_KM3")
  (v_003, 08:06:01, -12.089, -77.052, vel=48, seg="AV_BRASIL_KM5")
  (v_004, 08:06:23, -12.046, -77.031, vel= 8, seg="AV_JAVIER_PRADO_KM3")
  (v_005, 08:07:00, -12.089, -77.051, vel=52, seg="AV_BRASIL_KM5")

═══════════════════════════════════════════════════

MAP — cada nodo procesa su partición del dataset:
  Nodo1: (v_001, vel=15, seg="AV_JAVIER_PRADO_KM3") → ("AV_JAVIER_PRADO_KM3", 15)
  Nodo1: (v_002, vel=12, seg="AV_JAVIER_PRADO_KM3") → ("AV_JAVIER_PRADO_KM3", 12)
  Nodo2: (v_003, vel=48, seg="AV_BRASIL_KM5")       → ("AV_BRASIL_KM5", 48)
  Nodo2: (v_004, vel= 8, seg="AV_JAVIER_PRADO_KM3") → ("AV_JAVIER_PRADO_KM3", 8)
  Nodo3: (v_005, vel=52, seg="AV_BRASIL_KM5")       → ("AV_BRASIL_KM5", 52)

═══════════════════════════════════════════════════

SHUFFLE — reagrupa todos los pares por clave (segmento):
  "AV_JAVIER_PRADO_KM3" → [15, 12, 8, 20, 5, 18, 9, ...]
  "AV_BRASIL_KM5"       → [48, 52, 45, 50, 55, ...]
  "AV_AREQUIPA_KM8"     → [30, 35, 28, ...]

═══════════════════════════════════════════════════

REDUCE — calcula velocidad promedio y clasifica:
  "AV_JAVIER_PRADO_KM3" → promedio = 12.4 km/h → CONGESTIONADO  (< 20 km/h)
  "AV_BRASIL_KM5"       → promedio = 50.0 km/h → FLUIDO          (> 30 km/h)
  "AV_AREQUIPA_KM8"     → promedio = 31.0 km/h → NORMAL          (20-30 km/h)

SALIDA FINAL: mapa de calor de congestión → Neo4j actualiza pesos de aristas
```

Este es exactamente el pipeline que PySpark GraphX ejecuta en nuestro proyecto cada 2 minutos para recalibrar los pesos del grafo de calles de Lima y que Dijkstra pueda calcular rutas óptimas con información actualizada.

---

### P4: YARN — El cerebro del clúster

¿Qué problema resolvió YARN en Hadoop 2.x que no existía en Hadoop 1.x? ¿Por qué fue tan importante este cambio?

**Respuesta:**

**El problema de Hadoop 1.x: el JobTracker como cuello de botella**

En Hadoop 1.x, el **JobTracker** era responsable de dos funciones completamente distintas al mismo tiempo:

1. Gestión de recursos del clúster (quién tiene CPU y RAM disponible)
2. Seguimiento y coordinación de todos los jobs MapReduce en ejecución

Esto generaba tres problemas críticos:

- **SPOF (Single Point of Failure):** si el JobTracker caía, todos los jobs activos fallaban y el clúster quedaba paralizado
- **Escalabilidad limitada:** el JobTracker podía manejar como máximo ~4,000 nodos y ~40,000 tasks concurrentes antes de saturarse
- **Framework único:** el clúster solo podía correr MapReduce; si querías usar otro modelo de procesamiento, necesitabas un clúster separado

**La solución de YARN (Hadoop 2.x): separación de responsabilidades**

YARN divide esas dos funciones en componentes independientes:

```
HADOOP 1.x:                    HADOOP 2.x (YARN):
┌────────────────┐             ┌──────────────────────┐
│   JobTracker   │             │  ResourceManager      │
│  (recursos +   │     →       │  (solo recursos)      │
│   seguimiento) │             └──────────┬───────────┘
└────────────────┘                        │
                                          ▼
                               ┌──────────────────────┐
                               │  ApplicationMaster    │
                               │  (uno por job —       │
                               │   solo seguimiento)   │
                               └──────────────────────┘
```

**¿Por qué fue tan importante?**

El cambio permitió que sobre el mismo clúster HDFS pudieran correr simultáneamente múltiples frameworks de procesamiento: MapReduce, Apache Spark, Apache Tez, Flink. Ya no se necesitan clústeres separados para cada tecnología. En nuestro proyecto Smart Traffic Lima esto es directamente relevante: PySpark y GraphX corren sobre YARN compartiendo los mismos nodos del clúster Databricks, algo imposible en Hadoop 1.x.

---

### P5: Replicación de datos

Si un bloque de HDFS tiene factor de replicación = 3 y el clúster tiene 5 nodos, ¿en cuántos nodos diferentes estará guardado ese bloque? ¿Si fallan 2 nodos simultáneamente, se pierden los datos?

**Respuesta:**

**¿En cuántos nodos estará el bloque?**
Con factor de replicación = 3, el bloque estará almacenado en **3 nodos diferentes** de los 5 disponibles. HDFS selecciona los nodos siguiendo la política de rack-awareness: idealmente coloca la primera réplica en el nodo del cliente, la segunda en un rack diferente y la tercera en otro nodo del mismo rack que la segunda, para balancear entre tolerancia a fallos y eficiencia de red.

**¿Si fallan 2 nodos simultáneamente, se pierden los datos?**

Depende de cuáles 2 nodos fallan:

```sh
Ejemplo: bloque X está en Nodo1, Nodo3, Nodo5

Escenario A: fallan Nodo1 y Nodo2
  → Bloque X sigue disponible en Nodo3 y Nodo5 ✅ SIN PÉRDIDA
  → HDFS detecta que solo quedan 2 réplicas y crea una nueva en Nodo4

Escenario B: fallan Nodo1 y Nodo3
  → Bloque X sigue disponible en Nodo5 ✅ SIN PÉRDIDA (1 réplica)
  → HDFS crea urgentemente 2 réplicas nuevas (factor bajo = alerta)

Escenario C: fallan Nodo1, Nodo3 Y Nodo5 (los 3 que tienen el bloque)
  → Pérdida total del bloque ❌
  → Con 5 nodos y replicación=3, probabilidad muy baja pero posible
```

**Conclusión:** Con factor=3 y 5 nodos, la falla simultánea de 2 nodos cualesquiera **NO necesariamente implica pérdida de datos**, siempre que al menos uno de los 3 nodos que contienen el bloque siga activo. Para eliminar prácticamente cualquier riesgo, la recomendación es usar factor=3 con clústeres de al menos 6 nodos y rack-awareness configurado correctamente.

---

### P6: AWS vs. Azure vs. GCP para Big Data

Investiga: Si una empresa peruana quisiera migrar su data warehouse a la nube, ¿cuál proveedor recomendarías y por qué? Compara al menos en 3 dimensiones.

**Respuesta:**

Recomendaría **AWS** como primera opción para una empresa peruana, con **GCP** como alternativa sólida dependiendo del caso de uso específico.

| Dimensión | AWS | Azure | GCP |
| --- | --- | --- | --- |
| **Proximidad geográfica** | Región São Paulo (BR) + Miami; Lima en roadmap | Región Brasil disponible | Región São Paulo disponible |
| **Ecosistema Big Data** | EMR (Hadoop/Spark), Redshift, Glue, Athena, Kinesis — el más maduro y completo | HDInsight, Synapse, Event Hub — integración fuerte con Microsoft 365 | Dataproc, BigQuery, Pub/Sub — BigQuery es técnicamente superior para analítica SQL |
| **Disponibilidad de talento en Perú** | Mayor cantidad de profesionales certificados AWS en Lima | Segundo lugar; fuerte en empresas con stack Microsoft | Creciendo rápido pero menor base de talento local |
| **Costo para cargas analíticas** | Redshift Serverless compite bien; costos predecibles | Synapse puede ser caro a escala; modelos de licencia complejos | BigQuery on-demand (pago por query) puede ser muy económico para cargas irregulares |
| **Soporte local** | Partners AWS certificados en Lima (Softtech, Pragma, etc.) | Partners Microsoft bien establecidos | Menor red de partners locales |

**Recomendación final:**

- **AWS** si la empresa quiere el ecosistema más maduro, mayor disponibilidad de talento local y una ruta de migración gradual
- **GCP + BigQuery** si el caso de uso principal es analítica SQL a gran escala y las cargas son intermitentes (BigQuery cobra por dato escaneado, no por clúster encendido)
- **Azure** si la empresa ya tiene contratos Microsoft Enterprise y quiere integración nativa con Office 365 y Active Directory

Para el proyecto Smart Traffic Lima usamos **Databricks Community** (que corre sobre AWS internamente) precisamente porque ofrece PySpark + GraphX gestionado sin necesidad de administrar un clúster Hadoop propio.

---

### P7: Computación en la Niebla (Fog Computing)

Da un ejemplo de un caso de uso real en Perú donde Fog Computing sería mejor solución que Cloud Computing. Justifica técnicamente.

**Respuesta:**

**Caso: Sistema de semáforos inteligentes en Lima Metropolitana**

Este caso es directamente relevante para nuestro proyecto de tráfico. Lima tiene miles de cámaras de vigilancia y sensores de tráfico operados por la Municipalidad Metropolitana de Lima y el MTC. Actualmente la mayoría de semáforos operan con tiempos fijos programados, sin adaptarse al flujo real.

**¿Por qué Fog Computing y no Cloud?**

```sh
PROBLEMA CON CLOUD PURO:
  Cámara → [video 4K @ 30fps] → Internet → Cloud → análisis → respuesta → semáforo
  Latencia total: 500ms - 2 segundos
  Ancho de banda necesario: ~15 Mbps por cámara × 5,000 cámaras = 75 Gbps
  → INVIABLE: Lima no tiene esa infraestructura de red ni ese presupuesto

SOLUCIÓN CON FOG COMPUTING:
  Cámara + Edge Device (Raspberry Pi / NVIDIA Jetson) en el poste del semáforo
  → El Edge Device procesa localmente: cuenta vehículos, mide cola, detecta congestión
  → Solo envía metadata liviana a la nube: {"interseccion": "Javier Prado/Primavera",
     "vehiculos_norte": 45, "vehiculos_sur": 12, "espera_promedio_seg": 78}
  → Latencia local: < 50ms (decisión de cambio de semáforo en tiempo real)
  → Ancho de banda: ~1 KB/min por intersección en vez de 15 Mbps de video
```

**Justificación técnica:**

1. **Latencia:** Un semáforo que decide cambiar de fase basándose en tráfico real necesita respuesta en milisegundos, no segundos. La latencia de round-trip a la nube lo hace inviable para control en tiempo real
2. **Ancho de banda:** Transmitir video de miles de cámaras a la nube requeriría infraestructura de red que Lima no tiene. El Edge procesa localmente y solo sube los datos ya resumidos
3. **Resiliencia:** Si la conexión a internet falla, el semáforo sigue funcionando con lógica local. En cloud puro, una caída de internet paraliza el sistema

Este es exactamente el modelo que el proyecto Smart Traffic Lima eventualmente propone escalar: los nodos de cómputo cerca de los sensores GPS, y solo las predicciones y dashboards en la nube.

---

### P8: HBase vs. MySQL

Una empresa de telecomunicaciones necesita almacenar el historial de llamadas de 8 millones de clientes (última semana: 500M registros). ¿HBase o MySQL? ¿Por qué?

**Respuesta:**

**HBase, sin duda. Pero con un matiz arquitectónico importante.**

**Por qué MySQL falla aquí:**
500 millones de registros en una sola tabla MySQL generan tiempos de consulta inaceptables incluso con índices optimizados. MySQL escala verticalmente (más CPU/RAM al servidor) y tiene un techo físico y económico. Los JOINs a esa escala son prohibitivos. La replicación maestro-esclavo de MySQL no está diseñada para escrituras masivas simultáneas de millones de eventos de llamada por minuto.

**Por qué HBase es la elección correcta:**

- **Modelo columnar y orientado a filas amplias:** la row key puede ser el número de teléfono + timestamp invertido, lo que permite recuperar el historial completo de un cliente con una sola lectura de rango secuencial (extremadamente eficiente)
- **Escala horizontal nativa:** añadir nodos al clúster HBase aumenta la capacidad linealmente
- **Escrituras masivas concurrentes:** diseñado para ingestar millones de eventos por segundo
- **Time-series por diseño:** el historial de llamadas es esencialmente una serie temporal por cliente, que es exactamente el caso de uso para el que fue creado HBase (inspirado en Bigtable de Google)

**El matiz que no se debe ignorar — separación de dominios:**
Sin embargo, el historial de llamadas no debe vivir en el mismo store que los datos de facturación, el perfil del cliente o los datos de contratos. Así como no se deben poner todos los huevos en la misma cesta, mezclar estos dominios en una sola base de datos — aunque sea HBase — genera problemas de control de acceso, seguridad y gobernanza.

Lo correcto es separar en stores independientes con un identificador común (el número de teléfono como clave de integración):

```sh
HBase:      historial de llamadas (series temporales de eventos)
MongoDB:    perfil del cliente (documentos flexibles, datos enriquecidos)
PostgreSQL: contratos y facturación (relacional, ACID, auditabilidad)
Redis:      sesiones activas y caché de consultas frecuentes
```

Esto garantiza que el área de auditoría accede solo a facturación, el sistema de CDR escribe solo en HBase, y si HBase es comprometido, los datos contractuales siguen intactos. Lo viví al revés en la Universidad Norbert Wiener: todo mezclado, sin separación, sin control de acceso granular.

---

### P9: El problema de los datos calientes

En un clúster HDFS, los datos más accedidos se convierten en "hot data". ¿Qué solución propone Hadoop para esto? ¿Cómo se llama esta optimización?

**Respuesta:**

**El problema del hot data en HDFS:**
Cuando ciertos archivos o directorios se consultan con mucha frecuencia (por ejemplo, las últimas 24 horas de datos GPS en nuestro proyecto de tráfico Lima), los DataNodes que los contienen se saturan mientras los demás permanecen subutilizados. Esto genera cuellos de botella que degradan el rendimiento del clúster completo.

**Las soluciones que propone Hadoop:**

**1. HDFS Centralized Cache Management (la principal)**
Permite marcar archivos o directorios específicos para que sean cacheados en la RAM de los DataNodes, eliminando el acceso a disco para esos datos:

```bash
# Crear un pool de caché
hdfs cacheadmin -addPool trafico_hot_data -limit 10737418240  # 10 GB

# Marcar directorio como cacheable
hdfs cacheadmin -addDirective -path /datos/trafico/ultimas_24h -pool trafico_hot_data
```

Los DataNodes que reciben esta directiva mantienen los bloques en memoria RAM. Las consultas PySpark que accedan a esos datos los leen directamente de RAM sin tocar disco.

**2. HDFS Balancer**
Redistribuye automáticamente los bloques entre DataNodes cuando detecta desequilibrio de utilización:

```bash
hdfs balancer -threshold 10  # rebalancear si diferencia > 10% entre nodos
```

**3. Short-Circuit Local Reads**
Cuando un proceso Spark corre en el mismo nodo que tiene el bloque de datos, HDFS permite leer directamente del disco sin pasar por el protocolo de red (DataNode socket). Reduce latencia de I/O significativamente para datos accedidos localmente.

**4. Data Locality (principio subyacente)**
YARN asigna preferentemente las tareas de procesamiento a los nodos que ya tienen los datos, evitando transferencias de red. Spark lo implementa como "preferredLocations" en cada RDD partition.

En nuestro proyecto, los datos GPS de las últimas 2 horas (los más consultados para recalcular el grafo de tráfico) serían candidatos ideales para HDFS Centralized Cache Management.

---

### P10: Reflexión crítica

"Hadoop está muriendo. Spark lo reemplazó completamente." ¿Estás de acuerdo o en desacuerdo con esta afirmación? Argumenta con evidencia técnica.

**Respuesta:**

**En desacuerdo — la afirmación confunde una parte del ecosistema con el todo.**

Lo que es correcto de la afirmación: **Hadoop MapReduce como motor de procesamiento sí está en declive.** Spark es 10-100x más rápido que MapReduce clásico en la mayoría de cargas analíticas, procesa en memoria en vez de escribir a disco entre cada fase, y su API (DataFrames, SQL, MLlib, Streaming) es sustancialmente más productiva. Nadie escribe MapReduce nuevo cuando puede usar Spark.

Lo que es incorrecto: **Hadoop como ecosistema no murió — evolucionó.**

**Evidencia técnica:**

1. **HDFS sigue siendo ampliamente usado** — incluyendo como storage layer para Spark. Spark no almacena datos; necesita un sistema de archivos distribuido. En entornos on-premise, ese sistema sigue siendo HDFS. Databricks, la empresa que comercializa Spark, construyó Delta Lake que corre sobre HDFS o S3.

2. **YARN sigue siendo el resource manager** de muchos clústeres que corren Spark. Apache Spark puede ejecutarse sobre YARN, Kubernetes o en modo standalone. La mayoría de clústeres empresariales on-premise corren Spark sobre YARN.

3. **HBase, Hive, Kafka, ZooKeeper** — componentes del ecosistema Hadoop — son activamente mantenidos y ampliamente desplegados en producción en 2024-2026.

4. **Los grandes sustitutos de HDFS** no son Spark sino el almacenamiento cloud (AWS S3, Google Cloud Storage, Azure ADLS). La tendencia es migrar de HDFS a object storage cloud + Spark. Eso no es "Spark reemplazó Hadoop" sino "cloud reemplazó on-premise".

**Conclusión:**
La narrativa correcta es: *MapReduce fue reemplazado por Spark como motor de procesamiento, mientras HDFS y YARN evolucionaron para coexistir con él, y el almacenamiento object storage cloud está reemplazando a HDFS en entornos cloud-native.* Hadoop no murió — cambió de rol. En el contexto de nuestro proyecto Smart Traffic Lima, usamos Spark (moderno) sobre Databricks (cloud), que internamente maneja recursos con conceptos heredados directamente de YARN.

---

# PARTE C — LABORATORIO ESTUDIANTE S2

## Objetivo: Implementar Word Count con PySpark en Google Colab

### PASO 1: Configurar PySpark en Colab

```python
# Celda 1: Instalar PySpark
!pip install pyspark findspark -q
print("PySpark instalado!")
```

### PASO 2: Word Count básico

```python
# Celda 2: Word Count básico
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("WordCount_Ferreyra") \
    .master("local[*]") \
    .getOrCreate()

sc = spark.sparkContext
sc.setLogLevel("ERROR")

# Texto del dominio del proyecto: Smart Traffic Lima
texto_negocio = """
Lima es la quinta ciudad más congestionada de América Latina según el índice TomTom de 2024.
Los conductores limeños pierden en promedio 117 horas al año atrapados en el tráfico vehicular.
El impacto económico de la congestión en Lima asciende a 6200 millones de soles anuales según el MTC.
Los datos GPS de taxis colectivos y buses generan más de 500000 puntos por hora en Lima Metropolitana.
El procesamiento de datos de tráfico en tiempo real requiere arquitecturas Big Data distribuidas.
PySpark y GraphX permiten modelar el grafo de calles de Lima con 15000 nodos y 40000 aristas.
La predicción de congestión vehicular con Random Forest puede anticipar el tráfico 30 minutos antes.
Neo4j y el algoritmo Dijkstra calculan rutas óptimas en menos de 2 segundos actualizando pesos dinámicamente.
OpenStreetMap provee datos geoespaciales gratuitos de las calles y avenidas de Lima Metropolitana.
El modelo de Machine Learning busca un F1-Score mayor a 0.78 para la predicción de congestión vehicular.
Folium y Kepler generan mapas interactivos que visualizan el tráfico en tiempo real en Lima.
Los eventos públicos como partidos de fútbol entre Alianza Lima y Universitario generan picos de congestión.
El clima de Lima incluyendo garúa y lluvia afecta significativamente la velocidad promedio del tráfico.
La optimización dinámica de rutas puede reducir el tiempo de viaje promedio en un 20 por ciento.
El proyecto utiliza datos GPS sintéticos generados con OSMnx y Faker para simular 50000 vehículos.
Los semáforos de Lima operan con tiempos fijos sin adaptarse al flujo vehicular en tiempo real.
PageRank adaptado identifica las intersecciones con mayor centralidad de flujo en el grafo vial.
MongoDB Atlas almacena las predicciones de congestión y los eventos de tráfico del sistema.
La arquitectura Big Data del proyecto incluye ingesta procesamiento almacenamiento y visualización.
Databricks Community Edition permite ejecutar PySpark de forma gratuita en la nube para el proyecto.
"""

# Procesar con MapReduce
palabras_rdd = sc.parallelize([texto_negocio]) \
    .flatMap(lambda x: x.split()) \
    .map(lambda w: (w.lower().strip('.,!?'), 1)) \
    .reduceByKey(lambda a, b: a + b) \
    .filter(lambda x: len(x[0]) > 3)  # filtrar palabras muy cortas
    
resultado = sorted(palabras_rdd.collect(), key=lambda x: -x[1])

print("=== TOP 20 PALABRAS MÁS FRECUENTES — SMART TRAFFIC LIMA ===")
print(f"{'Palabra':<25} {'Frecuencia':>10}")
print("-" * 37)
for palabra, freq in resultado[:20]:
    print(f"{palabra:<25} {freq:>10}")
```

### PASO 3: Análisis de un dataset real con Spark SQL

```python
# Celda 3: Análisis con Spark SQL
import pandas as pd
import numpy as np

# Crear dataset de ventas (simula datos de tu empresa)
np.random.seed(42)
n = 100000  # 100K registros — ya es interesante para Spark

datos = {
    'fecha': pd.date_range('2024-01-01', periods=n, freq='min').astype(str),
    'producto': np.random.choice(['ProductoA', 'ProductoB', 'ProductoC', 'ProductoD'], n),
    'region': np.random.choice(['Lima', 'Arequipa', 'Trujillo', 'Cusco'], n, p=[0.5, 0.2, 0.2, 0.1]),
    'cantidad': np.random.randint(1, 50, n),
    'precio': np.random.uniform(10, 500, n).round(2),
    'canal': np.random.choice(['Online', 'Tienda', 'App_Movil'], n, p=[0.4, 0.4, 0.2])
}

pdf = pd.DataFrame(datos)
pdf['monto'] = pdf['cantidad'] * pdf['precio']

# Convertir a Spark DataFrame
df_spark = spark.createDataFrame(pdf)
df_spark.createOrReplaceTempView("ventas")

print(f"Dataset: {df_spark.count():,} registros cargados en Spark")
print(f"Schema:")
df_spark.printSchema()
```

```python
# Celda 4: Queries con Spark SQL
print("=== ANÁLISIS 1: Ventas totales por región ===")
spark.sql("""
    SELECT region, 
           COUNT(*) as num_transacciones,
           SUM(monto) as monto_total,
           AVG(monto) as ticket_promedio
    FROM ventas
    GROUP BY region
    ORDER BY monto_total DESC
""").show()

print("=== ANÁLISIS 2: Top 3 productos por canal ===")
spark.sql("""
    SELECT canal, producto, SUM(monto) as ventas
    FROM ventas
    GROUP BY canal, producto
    ORDER BY canal, ventas DESC
""").show(12)

print("=== ANÁLISIS 3: Ventas por hora del día ===")
spark.sql("""
    SELECT HOUR(CAST(fecha AS TIMESTAMP)) as hora,
           COUNT(*) as transacciones
    FROM ventas
    GROUP BY hora
    ORDER BY hora
""").show()
```

### PASO 4: Comparar velocidad Pandas vs. Spark

```python
# Celda 5: Benchmark — esto muestra el VALOR de Spark
import time

# Con Pandas
start = time.time()
resultado_pandas = pdf.groupby('region')['monto'].sum().sort_values(ascending=False)
tiempo_pandas = time.time() - start
print(f"Pandas: {tiempo_pandas*1000:.2f} ms")

# Con Spark
start = time.time()
resultado_spark = df_spark.groupBy('region').sum('monto').orderBy('sum(monto)', ascending=False)
resultado_spark.collect()  # forzar ejecución
tiempo_spark = time.time() - start
print(f"Spark: {tiempo_spark*1000:.2f} ms")

print(f"\nNota: Con 100K registros Pandas puede ser más rápido.")
print(f"Con 100M registros Spark sería 10-100x más rápido porque distribuye el trabajo.")
print(f"Esta es la clave de Big Data: escala a donde Pandas no puede llegar.")
```

```python
# Celda 6: Reflexión — ¿cuándo usar Spark?
reflexion = """
=== REFLEXIÓN: ¿CUÁNDO SPARK SUPERA A PANDAS? ===

Con 100K registros, Pandas puede ser igual o más rápido que Spark porque:
- Pandas opera en memoria de un solo proceso sin overhead de planificación distribuida
- Spark tiene un costo inicial de inicialización del contexto y serialización de datos
- La distribución del trabajo tiene un costo de red que supera el beneficio a pequeña escala

Spark supera a Pandas cuando:
1. El dataset no cabe en la RAM de una sola máquina (> 10-50 GB típicamente)
2. El procesamiento puede paralelizarse en múltiples nodos simultáneamente
3. Se necesita tolerancia a fallos durante el procesamiento (Spark reintenta tareas fallidas)
4. Se combina con streaming: Spark Streaming procesa datos en tiempo real, Pandas no puede

En el proyecto Smart Traffic Lima:
- 48 millones de puntos GPS por día NO caben en un laptop (requieren ~9 GB solo en RAM)
- El procesamiento de cada segmento de calle es independiente → paralelización perfecta
- Necesitamos actualizar el grafo cada 2 minutos → Spark Streaming es indispensable
- Pandas nos sirve para EDA inicial sobre muestras pequeñas; Spark para producción
"""
print(reflexion)
```

### ENTREGABLES LAB S2

- [ ] `lab_s2_wordcount_pyspark.ipynb` — Notebook completo ejecutado
- [ ] Análisis: ¿Qué palabras más frecuentes aparecieron en tu texto de negocio?
- [ ] Reflexión: ¿En qué momento Spark supera a Pandas? (escribe en una celda Markdown)

---

## Error más común en el Lab S2

**Error**: `java.lang.OutOfMemoryError: Java heap space`  
**Causa**: Colab tiene memoria limitada  
**Solución**: Reducir n a 10000 en vez de 100000, o reiniciar el kernel de Colab

## Criterio de evaluación Lab S2

| Criterio | Puntos |
| --------- | -------- |
| Word Count ejecutado con texto propio del proyecto | 4 |
| Spark SQL queries ejecutadas correctamente | 7 |
| Benchmark Pandas vs. Spark con reflexión | 4 |
| Reflexión en Markdown sobre cuándo usar Spark | 5 |
| **TOTAL** | **20** |
