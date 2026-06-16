# GUÍA DE TRABAJO — SEMANA 1

## Big Data (DD283) | Universidad Autónoma del Perú

**Nombre(s)**: Javier Flores Condeña

**Grupo de proyecto**: Grupo 1

**Fecha de entrega**: 13 Junio

**Modalidad**: Individual

---

# PARTE 1: CONCEPTOS FUNDAMENTALES DE BIG DATA

## Pregunta 1

### Define Big Data con tus propias palabras. ¿Cuál es la diferencia fundamental entre Big Data y una base de datos tradicional como SQL Server o MySQL que probablemente usas en tu empresa?

### Respuesta

Big Data es un conjunto de tecnologías y metodologías que permiten almacenar, procesar y analizar enormes cantidades de datos provenientes de diferentes fuentes y formatos. Su objetivo es transformar grandes volúmenes de información en conocimiento útil para la toma de decisiones.

La diferencia principal entre Big Data y una base de datos tradicional como SQL Server o MySQL es la capacidad de manejar grandes volúmenes de información de manera distribuida. Las bases de datos tradicionales funcionan muy bien para datos estructurados y volúmenes moderados, mientras que Big Data permite procesar datos estructurados, semiestructurados y no estructurados en múltiples servidores trabajando en conjunto.

Por ejemplo, una empresa de telecomunicaciones puede generar miles de millones de registros al año. Procesar esa cantidad de información únicamente con SQL Server sería costoso y poco eficiente, mientras que una arquitectura Big Data puede escalar agregando nuevos nodos de procesamiento.

---

## Pregunta 2

### Explica las 5 V's del Big Data con un ejemplo de tu propia empresa o de una empresa peruana que conozcas.

| V         | Definición con tus palabras                              | Ejemplo                                                                          |
| --------- | -------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Volumen   | Gran cantidad de datos generados y almacenados.          | Una empresa de telecomunicaciones registra millones de llamadas diariamente.     |
| Velocidad | Rapidez con la que los datos son generados y procesados. | Transacciones bancarias realizadas en tiempo real.                               |
| Variedad  | Diferentes tipos y formatos de datos.                    | Correos electrónicos, videos, registros web y bases de datos.                    |
| Veracidad | Calidad y confiabilidad de la información.               | Validación de datos de clientes para evitar errores.                             |
| Valor     | Beneficio obtenido a partir del análisis de los datos.   | Identificar clientes con riesgo de abandono para ejecutar campañas de retención. |

---

## Pregunta 3

### ¿Por qué una empresa como BCP (Banco de Crédito del Perú) NO podría usar solo una base de datos Oracle tradicional para procesar todos sus datos de transacciones en tiempo real? Menciona al menos 3 razones técnicas y 1 razón de negocio.

### Respuesta

### Razones técnicas

1. El volumen de datos generado diariamente es demasiado grande para ser procesado eficientemente por un único servidor.
2. Las transacciones ocurren en tiempo real y requieren procesamiento inmediato.
3. Se manejan múltiples formatos de datos provenientes de aplicaciones móviles, cajeros automáticos, portales web y sistemas internos.
4. Los modelos analíticos y predictivos requieren procesamiento distribuido y escalable.

### Razón de negocio

El banco necesita responder rápidamente a las necesidades de los clientes, detectar fraudes y ofrecer productos personalizados. Una plataforma Big Data permite analizar información en tiempo real y obtener ventajas competitivas frente a otras entidades financieras.

---

## Pregunta 4

### Clasifica los siguientes tipos de datos como Estructurado, Semi-estructurado o No estructurado. Justifica tu respuesta.

| Dato                                     | Clasificación     | Justificación                                            |
| ---------------------------------------- | ----------------- | -------------------------------------------------------- |
| Archivo Excel con ventas mensuales       | Estructurado      | Tiene filas y columnas definidas.                        |
| Tweet sobre el precio del dólar          | No estructurado   | Contiene texto libre.                                    |
| Foto del ticket de compra en Metro       | No estructurado   | Es una imagen digital.                                   |
| Archivo JSON de la API de SUNAT          | Semi-estructurado | Tiene etiquetas y estructura flexible.                   |
| Audio de llamada al call center de Claro | No estructurado   | Es un archivo de audio.                                  |
| Archivo CSV de exportaciones del BCRP    | Estructurado      | Posee una estructura tabular.                            |
| Video de seguridad de supermercado       | No estructurado   | Es información multimedia.                               |
| Log de errores de un servidor web        | Semi-estructurado | Tiene un formato definido pero no completamente tabular. |

---

## Pregunta 5

### ¿Qué es un clúster en el contexto de Big Data? ¿Cuál es la diferencia entre un sistema de memoria compartida y un sistema de memoria distribuida? Usa un diagrama o esquema para explicarlo.

### Respuesta

Un clúster es un conjunto de computadoras conectadas que trabajan juntas como si fueran un único sistema para almacenar y procesar datos.

### Sistema de memoria compartida

Todos los procesadores utilizan una misma memoria central.

```text
CPU 1 ─┐
CPU 2 ─┼── Memoria Compartida
CPU 3 ─┘
```

### Sistema de memoria distribuida

Cada nodo posee su propia memoria y comparte resultados mediante la red.

```text
Nodo 1 (CPU + RAM)
        │
Nodo 2 (CPU + RAM)
        │
Nodo 3 (CPU + RAM)
```

Los sistemas Big Data utilizan principalmente memoria distribuida porque permiten crecer agregando nuevos nodos.

---

## Pregunta 6

### Investiga y responde: ¿Qué empresa latinoamericana (puede ser peruana) ha implementado Big Data de manera exitosa?

### Fuente consultada

https://www.viabcp.com

### Respuesta

Una empresa peruana que ha implementado Big Data exitosamente es el Banco de Crédito del Perú (BCP).

El banco enfrentaba el reto de procesar grandes volúmenes de información provenientes de transacciones financieras, aplicaciones móviles, cajeros automáticos y canales digitales. A medida que aumentaba el número de clientes y operaciones, resultaba más difícil analizar toda la información utilizando únicamente sistemas tradicionales.

Para resolver este problema, implementó soluciones de análisis masivo de datos que permiten integrar información de múltiples fuentes y procesarla en menor tiempo. Gracias a estas tecnologías, el banco puede identificar patrones de comportamiento, detectar posibles fraudes y personalizar ofertas para sus clientes.

Los resultados obtenidos incluyen una mejor experiencia para los usuarios, reducción de riesgos operativos, campañas comerciales más efectivas y una toma de decisiones más rápida basada en información actualizada. La implementación de Big Data ha contribuido significativamente a la transformación digital del banco y a mantener su liderazgo en el mercado financiero peruano.

---

## Pregunta 7

### Explica la diferencia entre Data Lake y Data Warehouse. ¿En qué situación usarías cada uno?

| Aspecto            | Data Lake                                               | Data Warehouse                                   |
| ------------------ | ------------------------------------------------------- | ------------------------------------------------ |
| Definición         | Repositorio de datos en formato original.               | Repositorio optimizado para análisis y reportes. |
| Tipo de datos      | Estructurados, semi-estructurados y no estructurados.   | Principalmente estructurados.                    |
| Cuándo usarlo      | Cuando aún no se conoce el uso final de los datos.      | Cuando existen necesidades analíticas definidas. |
| Ejemplo de negocio | Almacenar información de redes sociales y sensores IoT. | Elaborar reportes financieros mensuales.         |
| Herramienta típica | Hadoop, Azure Data Lake, Amazon S3.                     | Snowflake, Oracle, SQL Server, Redshift.         |

---

## Pregunta 8

### ¿Qué son los requisitos de un sistema Big Data? Identifica y explica los 5 requisitos principales que debe cumplir una arquitectura Big Data robusta.

### Respuesta

### 1. Escalabilidad

Capacidad de crecer conforme aumenta la cantidad de datos.

**Si no se cumple:** El sistema se vuelve lento y limita el crecimiento de la empresa.

### 2. Disponibilidad

Capacidad de mantenerse operativo continuamente.

**Si no se cumple:** Los usuarios pierden acceso a la información.

### 3. Tolerancia a fallos

Capacidad de continuar funcionando ante fallas de hardware o software.

**Si no se cumple:** Existe riesgo de pérdida de datos.

### 4. Rendimiento

Capacidad de procesar grandes volúmenes de datos eficientemente.

**Si no se cumple:** Los análisis tardan demasiado tiempo.

### 5. Seguridad

Protección de los datos frente a accesos no autorizados.

**Si no se cumple:** Puede ocurrir fuga o alteración de información sensible.

---

## Pregunta 9

### La empresa en la que trabajas actualmente, ¿tiene algún problema de datos que podría resolverse con Big Data?

### Respuesta

Una necesidad que podría resolverse mediante Big Data es la predicción de abandono de clientes (Churn) en una empresa de telecomunicaciones.

Actualmente se generan millones de registros relacionados con consumo de servicios, reclamos, pagos, llamadas al soporte técnico y uso de aplicaciones. Aunque estos datos son almacenados, muchas veces solo se utilizan para reportes históricos y no para generar acciones preventivas.

Las V's involucradas son Volumen, Velocidad, Variedad, Veracidad y Valor. La empresa maneja grandes cantidades de información provenientes de diferentes fuentes que requieren ser procesadas rápidamente y con calidad suficiente para obtener resultados confiables.

Como solución inicial, se propone implementar un Data Lake para centralizar la información y utilizar modelos de Machine Learning capaces de identificar clientes con alta probabilidad de abandono. De esta forma, la empresa podría realizar campañas de retención antes de que los clientes cancelen sus servicios, reduciendo pérdidas económicas y mejorando la satisfacción de los usuarios.

---

## Pregunta 10

### Análisis crítico

> "Una empresa de telecomunicaciones en Perú tiene 8 millones de clientes. Cada cliente genera en promedio 500 registros de datos al día (llamadas, SMS, datos móviles, pagos). La empresa quiere predecir qué clientes cancelarán su contrato en los próximos 30 días para ofrecerles retención proactiva."

### a) ¿Cuántos registros se generan por día? ¿Por año?

**Por día:**

8,000,000 × 500 = 4,000,000,000 registros

**Resultado:** 4 mil millones de registros diarios.

**Por año:**

4,000,000,000 × 365 = 1,460,000,000,000 registros

**Resultado:** 1.46 billones de registros al año.

---

### b) ¿Qué tipo de datos están involucrados?

* Datos de llamadas.
* SMS enviados y recibidos.
* Consumo de datos móviles.
* Información de pagos y facturación.
* Datos de clientes.
* Reclamos y consultas al soporte técnico.
* Grabaciones de llamadas y chats.

---

### c) ¿Cuáles de las 5 V's son más relevantes en este caso?

Las V's más relevantes son:

* **Volumen:** miles de millones de registros.
* **Velocidad:** generación constante de información.
* **Variedad:** múltiples fuentes y formatos.
* **Valor:** reducción del abandono de clientes.
* **Veracidad:** calidad de los datos para entrenar modelos predictivos.

---

### d) ¿Qué tecnologías Big Data necesitarían para resolver este problema?

| Función          | Tecnología                        |
| ---------------- | --------------------------------- |
| Ingesta de datos | Apache Kafka                      |
| Almacenamiento   | Hadoop HDFS o Data Lake           |
| Procesamiento    | Apache Spark                      |
| Machine Learning | Spark MLlib, Python, Scikit-Learn |
| Visualización    | Power BI o Tableau                |
| Automatización   | Apache Airflow                    |

---

### e) ¿Qué impacto ético podría tener esta solución? (Pista: privacidad de datos)

La solución debe respetar la privacidad de los clientes y cumplir con la legislación de protección de datos personales. Es importante garantizar que la información sea utilizada únicamente para fines autorizados, mantener mecanismos de seguridad adecuados y evitar que los algoritmos generen discriminación o decisiones injustas.

---

# PARTE 2: REFLEXIÓN Y CONEXIÓN CON TU PROYECTO

## Pregunta 11 — Tu Proyecto

### Describe brevemente el proyecto Big Data que tu grupo ha elegido.

### Respuesta

**Nombre del proyecto:**
Predicción de Churn en Empresas Peruanas mediante Big Data y Machine Learning.

**Empresa o sector al que aplica:**
Telecomunicaciones.

**Problema que resuelve:**
Identificar clientes con alta probabilidad de abandonar el servicio para ejecutar acciones preventivas de retención.

**¿Cuáles de las 5 V's están presentes?**

* Volumen: miles de registros diarios.
* Velocidad: actualización constante de eventos.
* Variedad: CRM, facturación, soporte técnico y navegación.
* Veracidad: validación de datos.
* Valor: reducción de pérdida de clientes e incremento de ingresos.

---

## Pregunta 12 — Arquitectura inicial

### Dibuja una arquitectura inicial muy básica de cómo crees que debería funcionar tu proyecto.

### Respuesta

```text
+--------------------+
| Fuentes de Datos   |
| CRM, Facturación   |
| Call Center, Web   |
+---------+----------+
          |
          v
+--------------------+
| Ingesta de Datos   |
| Kafka / ETL        |
+---------+----------+
          |
          v
+--------------------+
| Data Lake          |
| Hadoop / S3        |
+---------+----------+
          |
          v
+--------------------+
| Procesamiento      |
| Apache Spark       |
+---------+----------+
          |
          v
+--------------------+
| Machine Learning   |
| Modelo Churn       |
+---------+----------+
          |
          v
+--------------------+
| Data Warehouse     |
+---------+----------+
          |
          v
+--------------------+
| Power BI / Tableau |
| Dashboards         |
+--------------------+
```

Esta arquitectura permite recopilar información de múltiples fuentes, almacenarla en un Data Lake, procesarla mediante Apache Spark, entrenar modelos de Machine Learning para predecir el abandono de clientes y visualizar los resultados mediante dashboards ejecutivos.

## CRITERIOS DE EVALUACIÓN

| Criterio | Puntos |
|---------|--------|
| Responde todas las preguntas (no deja en blanco) | 4 |
| Usa sus propias palabras, no copia de internet | 4 |
| Da ejemplos reales de su entorno laboral | 4 |
| Las definiciones son técnicamente correctas | 4 |
| Respuestas de reflexión (P9, P11, P12) muestran pensamiento propio | 4 |
| **TOTAL** | **20** |