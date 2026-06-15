# GUÍA DE TRABAJO — SEMANA 1
## Big Data (DD283) | Universidad Autónoma del Perú

**Nombre(s)**: WILDER NICANOR NORABUENA RAMIREZ  
**Grupo de proyecto**: GRUPO 3 
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
```
[Usar SQL Server o MySQL, sirbe para optimizada las operaciones transaccionales y datos estructurados. Encambio si se necesita analizar grandes volúmenes de datos variados en tiempo real, es mejor usar Big Data. en el caso de la empresa donde trabajo solo se usa base de datos en SQL Server.]
```

---

### Pregunta 2
Explica las **5 V's del Big Data** con un ejemplo de tu propia empresa o de una empresa peruana que conozcas. Completa la siguiente tabla:

| V | Definición con tus palabras | Ejemplo de tu empresa/empresa conocida |
|---|---------------------------|---------------------------------------|
| Volumen | Cantidad masiva de datos generados y almacenados.|Interbank procesa millones de transacciones diarias de tarjetas, transferencias y pagos digitales.|
| Velocidad |Rapidez con la que los datos se generan y deben ser procesados.|El sistema detecta en tiempo real posibles fraudes en operaciones con tarjeta, bloqueando transacciones sospechosas al instante.|
| Variedad |Diversidad de formatos y fuentes de datos.|Se analizan datos estructurados (transacciones), semiestructurados (logs de aplicaciones) y no estructurados (comentarios en redes sociales sobre el banco).|
| Veracidad |Calidad y confiabilidad de los datos. |Se aplican algoritmos de limpieza para evitar errores en registros de clientes y asegurar que la información sea precisa. |
| Valor |Capacidad de transformar datos en conocimiento útil. |El análisis de patrones de consumo permite ofrecer promociones personalizadas a clientes según su historial de compras. |

---

### Pregunta 3
¿Por qué una empresa como BCP (Banco de Crédito del Perú) NO podría usar solo una base de datos Oracle tradicional para procesar todos sus datos de transacciones en tiempo real? Menciona al menos 3 razones técnicas y 1 razón de negocio.

_Respuesta_:  
```
[Oracle es excelente para operaciones transaccionales críticas, pero BCP necesita Big Data para manejar la escala, velocidad y diversidad de información que exige la banca moderna.]
```

---

### Pregunta 4
Clasifica los siguientes tipos de datos como **Estructurado**, **Semi-estructurado** o **No estructurado**. Justifica tu respuesta:

| Dato | Clasificación | Justificación |
|------|-------------|--------------|
| Un archivo Excel con ventas mensuales |Estructurado |Los datos están organizados en filas y columnas con un formato tabular definido, fácil de consultar con SQL. |
| Un tweet sobre el precio del dólar |Semi-estructurado |Contiene texto libre (no estructurado) pero también metadatos definidos (usuario, fecha, hashtags, ID). |
| Una foto del ticket de compra en Metro |No estructurado |Es una imagen sin estructura tabular; requiere técnicas de visión computacional (OCR) para extraer información. |
| Un archivo JSON de la API de SUNAT |Semi-estructurado |Tiene un formato definido (llaves y valores) pero no es tabular; se adapta bien a bases NoSQL. |
| Un audio de una llamada al call center de Claro | No estructurado | Es un archivo de sonido sin estructura; necesita procesamiento de voz para convertirlo en texto.|
| Un archivo CSV de exportaciones del BCRP | Estructurado| Similar a Excel, está organizado en filas y columnas con delimitadores claros.|
| Un video de seguridad de un supermercado | No estructurado| Es un archivo multimedia sin estructura tabular; requiere análisis de imágenes y video.|
| Un log de errores de un servidor web | Semi-estructurado| Aunque es texto libre, sigue un patrón definido (fecha, hora, tipo de error), lo que permite análisis automatizado.|

---

### Pregunta 5
¿Qué es un **clúster** en el contexto de Big Data? ¿Cuál es la diferencia entre un sistema de **memoria compartida** y un sistema de **memoria distribuida**? Usa un diagrama o esquema para explicarlo.

_Respuesta_:  
```
[graph TD
    A[Clúster Big Data] --> B[Memoria Compartida]
    A --> C[Memoria Distribuida]

    B --> B1[Nodo 1: CPU]
    B --> B2[Nodo 2: CPU]
    B --> B3[Nodo 3: CPU]
    B --> M[Memoria Central Compartida]

    C --> C1[Nodo 1: CPU + RAM]
    C --> C2[Nodo 2: CPU + RAM]
    C --> C3[Nodo 3: CPU + RAM]
    C1 -->|Comunicación por red| C2
    C2 -->|Comunicación por red| C3
]
```

---

### Pregunta 6
Investiga y responde: ¿Qué empresa latinoamericana (puede ser peruana) ha implementado Big Data de manera exitosa? Describe:
- El problema que tenían
- La solución Big Data que implementaron
- Los resultados que obtuvieron

_Fuente consultada (URL o libro)_: https://es.linkedin.com/pulse/caso-de-%C3%A9xito-tasa-impulsa-su-transformaci%C3%B3n-bnmae?utm_source=copilot.com

_Respuesta_:  
```
[TASA – Perú

Problema: Datos dispersos en silos, procesos manuales de reporting y baja trazabilidad.

Solución: Implementación de un Lakehouse en Microsoft Azure con integración de múltiples fuentes y dashboards en Power BI.

Resultados: Mayor eficiencia operativa, decisiones en tiempo real y sostenibilidad en la pesca industrial.]
```

---

### Pregunta 7
Explica la diferencia entre **Data Lake** y **Data Warehouse**. ¿En qué situación usarías cada uno? Da un ejemplo de negocio para cada caso.

| | Data Lake | Data Warehouse |
|--|----------|---------------|
| Definición | Repositorio que almacena grandes volúmenes de datos en bruto (sin procesar), de cualquier tipo y formato.| Sistema estructurado que organiza datos limpios y transformados en tablas para análisis empresarial.|
| Tipo de datos | Estructurados, semiestructurados y no estructurados (texto, imágenes, videos, logs, IoT).| Principalmente datos estructurados y altamente organizados en esquemas relacionales.|
| Cuándo usarlo | Cuando se necesita almacenar datos masivos y diversos para exploración, machine learning o análisis avanzado.| Cuando se requiere generar reportes confiables, KPIs y análisis de negocio con datos consistentes.|
| Ejemplo de negocio | Una cadena de supermercados en Perú (ej. Plaza Vea) que guarda datos de cámaras de seguridad, tickets escaneados, redes sociales y sensores IoT en un Data Lake para detectar patrones de consumo.| Un banco como BCP que usa un Data Warehouse para consolidar transacciones financieras y generar reportes diarios de riesgo, cumplimiento y balances.|
| Herramienta típica | Hadoop, Amazon S3, Azure Data Lake, Google BigQuery (modo lake).| Oracle, Teradata, Snowflake, Microsoft SQL Server, SAP BW.|

---

### Pregunta 8
¿Qué son los **requisitos de un sistema Big Data**? Identifica y explica los 5 requisitos principales que debe cumplir una arquitectura Big Data robusta. Para cada uno, menciona qué pasa si ese requisito NO se cumple.

_Respuesta_:  
```
[Unos requisitos de un sistema Big Data son las condiciones técnicas y organizativas que debe cumplir una arquitectura para poder procesar grandes volúmenes de datos de manera eficiente, segura y útil.
Escalabilidad: asegura que el sistema crezca con el negocio.
Disponibilidad: garantiza continuidad operativa.
Velocidad: permite reaccionar en tiempo real.
Flexibilidad: abre la puerta a análisis más ricos.
Seguridad: protege el activo más valioso: la información.]
```

---

### Pregunta 9
La empresa en la que trabajas actualmente, ¿tiene algún problema de datos que podría resolverse con Big Data? Describe:
- El problema o necesidad
- Qué tipo de datos implicaría (V's del Big Data)
- Una propuesta inicial de solución (aunque sea básica)

*(Si no puedes compartir información de tu empresa por confidencialidad, usa una empresa pública del sector)*

_Respuesta_:  
```
[Problema o necesidad
Plaza Vea recibe millones de transacciones diarias en sus tiendas físicas y en su plataforma de e-commerce.

Los datos de ventas, inventarios, promociones y comportamiento de clientes estaban dispersos en distintos sistemas.

Esto dificultaba detectar patrones de consumo en tiempo real y optimizar la cadena de suministro.

Tipos de datos implicados (5 V’s del Big Data)
Volumen: millones de tickets de compra, registros de inventario y transacciones online.

Velocidad: necesidad de procesar datos en tiempo real para ajustar precios y stock.

Variedad: datos estructurados (ventas), semiestructurados (logs de apps) y no estructurados (comentarios en redes sociales).

Veracidad: depuración de datos duplicados o inconsistentes en inventarios.

Valor: generar insights para promociones personalizadas y optimización logística.

Propuesta inicial de solución
Implementar un Data Lake en la nube (ej. Azure Data Lake o AWS S3) para centralizar todas las fuentes de datos.

Usar Apache Spark para procesar en tiempo real las transacciones y detectar patrones de compra.

Integrar dashboards en Power BI para que gerentes de tienda y marketing visualicen KPIs en tiempo real.

Aplicar modelos predictivos para anticipar la demanda de productos y ajustar inventarios.]
```

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
```
[a) Registros generados
Por día: 8,000,000 clientes x 500 registros = 4,000,000,000 → 4 mil millones de registros diarios.

Por año: 4,000,000,000 x 365=1,460,000,000,000 → 1.46 billones de registros al año.

b) Tipo de datos involucrados
Estructurados: pagos, duración de llamadas, consumo de datos.

Semi-estructurados: logs de aplicaciones, registros de red.

No estructurados: mensajes SMS, notas de atención al cliente, posibles audios de call center.

c) 5 V’s más relevantes
Volumen → miles de millones de registros diarios.

Velocidad → necesidad de procesar en tiempo real para detectar riesgo de cancelación.

Variedad → múltiples tipos de datos (estructurados, semiestructurados y no estructurados).

Valor → transformar datos en predicciones útiles para retención de clientes.

Veracidad también importa, pero menos crítica que las anteriores: los datos deben ser confiables para que el modelo no falle.

d) Tecnologías Big Data necesarias
Data Lake (ej. Azure Data Lake, Amazon S3) para almacenar todo tipo de datos.

Procesamiento distribuido con Apache Spark o Hadoop.

Streaming en tiempo real con Apache Kafka para ingestión de datos.

Machine Learning con TensorFlow, PyTorch o MLlib para modelos de predicción de churn (cancelación).

Dashboards con Power BI o Tableau para visualizar clientes en riesgo.

e) Impacto ético (privacidad de datos)
El análisis de datos de clientes puede invadir su privacidad si no se gestiona con transparencia.

Riesgo de uso indebido de información sensible (ubicación, hábitos de consumo).

Necesidad de cumplir con normativas de protección de datos (ej. Ley de Protección de Datos Personales en Perú).

Éticamente, la empresa debe informar y obtener consentimiento de los clientes para usar sus datos en modelos de predicción.]
```

---

## PARTE 2: REFLEXIÓN Y CONEXIÓN CON TU PROYECTO (2 preguntas adicionales)

### Pregunta 11 — Tu Proyecto
Describe brevemente el proyecto Big Data que tu grupo ha elegido:
- Nombre del proyecto
- Empresa o sector al que aplica
- Problema que resuelve
- ¿Cuáles de las 5 V's están presentes en los datos del proyecto?

_Respuesta_:  
```
[Escribe tu respuesta aquí]
```

---

### Pregunta 12 — Arquitectura inicial
Dibuja (a mano o usando draw.io) una arquitectura inicial **muy básica** de cómo crees que debería funcionar tu proyecto. Incluye: fuentes de datos, almacenamiento, procesamiento y visualización.

*(Adjunta la imagen o el link de draw.io)*

_Link o descripción de tu diagrama_:  
```
[Escribe aquí o adjunta imagen]
```

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
