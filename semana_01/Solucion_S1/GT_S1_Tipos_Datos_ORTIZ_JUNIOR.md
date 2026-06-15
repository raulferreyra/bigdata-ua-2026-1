# GUÍA DE TRABAJO — SEMANA 1

## Big Data (DD283) | Universidad Autónoma del Perú

**Nombre(s)**: \***\*\*\*\*\***\*\*\***\*\*\*\*\***\_\_\_\***\*\*\*\*\***\*\*\***\*\*\*\*\***  
**Grupo de proyecto**: \***\*\*\*\*\***\*\*\***\*\*\*\*\***\_\_\_\***\*\*\*\*\***\*\*\***\*\*\*\*\***  
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
Para mí, Big Data no es solo "tener mucha información", sino lidiar con un volumen tan inmenso, variado y rápido de datos que las bases de datos de toda la vida (como un SQL Server o el MySQL que solemos usar en proyectos convencionales) simplemente colapsarían al intentar procesarlos. La diferencia fundamental es la arquitectura: una base de datos tradicional está pensada para datos estructurados (tablas ordenadas) y centralizados en un solo gran servidor. En cambio, Big Data distribuye el esfuerzo en múltiples servidores (clústers) para poder tragar y analizar desde textos e imágenes hasta millones de logs de servidores en tiempo real sin saturarse.
```

---

### Pregunta 2

Explica las **5 V's del Big Data** con un ejemplo de tu propia empresa o de una empresa peruana que conozcas. Completa la siguiente tabla:

| V         | Definición con tus palabras                                                                     | Ejemplo de tu empresa/empresa conocida                                                                                                               |
| --------- | ----------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Volumen   | La cantidad masiva de datos que se genera y se debe guardar.                                    | Los millones de transacciones de yapeos, pagos de servicios y transferencias que el BCP registra cada día a nivel nacional.                          |
| Velocidad | El ritmo vertiginoso al que llegan los datos y el tiempo casi nulo en el que deben procesarse.  | El procesamiento en milisegundos que ocurre cuando escaneas un QR y el pago se aprueba al instante.                                                  |
| Variedad  | Los distintos formatos de datos: no todo es texto o números; hay fotos, audios, metadatos, etc. | BCP no solo guarda el monto (estructurado), sino los mensajes o emojis del yapeo, la geolocalización y los metadatos del celular (no estructurados). |
| Veracidad | La calidad, limpieza y confiabilidad de los datos. Si entra basura, sale basura.                | Los cruces de seguridad y validación de identidad para garantizar que un pago es legítimo y evitar fraudes.                                          |
| Valor     | El beneficio o dinero que la empresa saca al analizar toda esa montaña de datos.                | Usar el historial de pagos menores y consumos para ofrecerte un microcrédito preaprobado en la app.                                                  |

---

### Pregunta 3

¿Por qué una empresa como BCP (Banco de Crédito del Perú) NO podría usar solo una base de datos Oracle tradicional para procesar todos sus datos de transacciones en tiempo real? Menciona al menos 3 razones técnicas y 1 razón de negocio.

_Respuesta_:

```
Razones técnicas:

Cuello de botella de I/O: Una sola base de datos Oracle colapsaría por la cantidad masiva de lecturas y escrituras simultáneas por segundo que generan millones de usuarios interactuando al mismo tiempo en todo el país.

Escalabilidad vertical limitada: Una BD tradicional requiere mejorar el hardware (más RAM, mejor procesador) del mismo servidor para crecer, pero llega un punto físico donde ya no puedes ponerle un procesador más grande. Big Data escala sumando más servidores económicos (horizontal).

Formatos rígidos: Oracle tradicional sufre si intentas meterle de forma nativa los millones de logs no estructurados que generan las apps móviles.

Razón de negocio:
Si el único servidor centralizado falla o se satura (como a veces ocurre en quincenas), se cae todo el sistema bancario. Esto genera pérdidas millonarias inmediatas y destruye la confianza del cliente.
```

---

### Pregunta 4

Clasifica los siguientes tipos de datos como **Estructurado**, **Semi-estructurado** o **No estructurado**. Justifica tu respuesta:

| Dato                                            | Clasificación     | Justificación                                                                                                       |
| ----------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------- |
| Un archivo Excel con ventas mensuales           | Estructurado      | Tiene un formato rígido, con filas y columnas perfectamente definidas.                                              |
| Un tweet sobre el precio del dólar              | Semi-estructurado | El texto del tweet es libre (no estructurado), pero viene acompañado de metadatos fijos (fecha, usuario, hashtags). |
| Una foto del ticket de compra en Metro          | No estructurado   | Es una imagen (píxeles). Para sacar los datos se requiere un software de reconocimiento óptico (OCR).               |
| Un archivo JSON de la API de SUNAT              | Semi-estructurado | No es una tabla plana, pero tiene etiquetas (tags) y jerarquías que organizan la información.                       |
| Un audio de una llamada al call center de Claro | No estructurado   | Es un archivo de sonido. Requiere procesamiento de lenguaje natural (NLP) para entender qué se está diciendo.       |
| Un archivo CSV de exportaciones del BCRP        | Estructurado      | Es texto plano, pero estructurado rígidamente en formato tabular separado por comas.                                |
| Un video de seguridad de un supermercado        | No estructurado   | Es una secuencia de imágenes sin un esquema de datos relacional interno.                                            |
| Un log de errores de un servidor web            | Semi-estructurado | Contiene datos fijos (fecha, hora, IP) mezclados con texto libre que varía según el error del sistema.              |

---

### Pregunta 5

¿Qué es un **clúster** en el contexto de Big Data? ¿Cuál es la diferencia entre un sistema de **memoria compartida** y un sistema de **memoria distribuida**? Usa un diagrama o esquema para explicarlo.

_Respuesta_:

```
Un clúster es básicamente un grupo de varias computadoras (nodos) conectadas por una red que trabajan juntas como si fueran un solo súper-servidor. Si una tarea es muy pesada, se divide entre todas.

- Memoria compartida: Imagina una sola placa madre gigante con varios procesadores accediendo a la misma memoria RAM. Es rápido, pero tiene un límite físico.

- Memoria distribuida: Cada computadora del clúster tiene su propio procesador y su propia RAM (como armar varias PCs independientes). Se comunican por red. Si necesitas más potencia, solo conectas otra PC al grupo. Esto es infinito.
(Para tu esquema en el documento, dibuja en un lado un solo cajón grande con varios CPUs apuntando a una sola RAM; y en el otro lado, dibuja varias PCs pequeñas conectadas por una línea de red, cada una con su CPU y RAM).
```

---

### Pregunta 6

Investiga y responde: ¿Qué empresa latinoamericana (puede ser peruana) ha implementado Big Data de manera exitosa? Describe:

- El problema que tenían
- La solución Big Data que implementaron
- Los resultados que obtuvieron

_Fuente consultada (URL o libro)_: **\*\***\_\_\_**\*\***

_Respuesta_:

```
Fuente consultada: Análisis de transformación digital de LATAM Pass / Casos de uso de Big Data en aviación comercial.

- Problema: LATAM Airlines necesitaba optimizar las rutas de vuelo, reducir los retrasos por fallas mecánicas sorpresa y mejorar la fidelización de sus viajeros frecuentes.

- Solución Big Data: Implementaron sistemas de analítica predictiva que cruzan en tiempo real los datos de los sensores de las aeronaves (temperatura, vibración de motores), junto con información meteorológica y el historial de compras y rutas del programa de lealtad.

- Resultados: Lograron pasar de un mantenimiento reactivo a uno predictivo, reduciendo las cancelaciones de vuelos por fallas técnicas. Además, pudieron enviar ofertas hiperpersonalizadas a los usuarios basadas en sus destinos habituales, mejorando la retención de clientes.
```

---

### Pregunta 7

Explica la diferencia entre **Data Lake** y **Data Warehouse**. ¿En qué situación usarías cada uno? Da un ejemplo de negocio para cada caso.

|                    | Data Lake                                                                                        | Data Warehouse                                                                                             |
| ------------------ | ------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| Definición         | Repositorio gigante donde se guardan datos crudos y en bruto, sin ningún propósito definido aún. | Repositorio muy estructurado donde los datos ya fueron limpiados y filtrados para un propósito específico. |
| Tipo de datos      | Estructurados, semi-estructurados y no estructurados.                                            | Únicamente datos estructurados e históricos.                                                               |
| Cuándo usarlo      | Para exploración profunda, Machine Learning o cuando aún no sabes qué valor sacarás de la data.  | Para crear reportes rápidos, dashboards gerenciales e inteligencia de negocios (BI).                       |
| Ejemplo de negocio | Guardar todos los diagnósticos de hardware, logs de Azure y tickets de soporte sin filtrar.      | Generar un reporte mensual de cuántos equipos se repararon por área.                                       |
| Herramienta típica | Amazon S3, Hadoop, Azure Data Lake.                                                              | Google BigQuery, Amazon Redshift.                                                                          |

---

### Pregunta 8

¿Qué son los **requisitos de un sistema Big Data**? Identifica y explica los 5 requisitos principales que debe cumplir una arquitectura Big Data robusta. Para cada uno, menciona qué pasa si ese requisito NO se cumple.

_Respuesta_:

```
1.- Escalabilidad: Debe poder crecer sin rehacer el sistema. Si no se cumple, el sistema colapsará cuando el volumen de datos aumente.

2.- Tolerancia a fallos: Debe tener copias de seguridad distribuidas. Si no se cumple, si se quema un disco duro o se cae un servidor, se pierde información irrecuperable.

3.- Baja Latencia / Alto Rendimiento: Debe procesar la información rápido. Si no se cumple, un análisis que requiere respuestas en tiempo real (como evitar un fraude) llegará demasiado tarde para ser útil.

4.- Heterogeneidad / Flexibilidad: Debe soportar cualquier formato de datos. Si no se cumple, no podrías cruzar la base de datos SQL con los audios de los clientes, perdiendo información valiosa.

5.- Seguridad y Privacidad: Debe encriptar y proteger accesos. Si no se cumple, hay riesgo de filtración de datos sensibles, demandas legales y multas severas.
```

---

### Pregunta 9

La empresa en la que trabajas actualmente, ¿tiene algún problema de datos que podría resolverse con Big Data? Describe:

- El problema o necesidad
- Qué tipo de datos implicaría (V's del Big Data)
- Una propuesta inicial de solución (aunque sea básica)

_(Si no puedes compartir información de tu empresa por confidencialidad, usa una empresa pública del sector)_

_Respuesta_:

```
- Problema: En el área de soporte técnico y mantenimiento de hardware IT, el enfoque suele ser reactivo: los servidores, impresoras o computadoras (como las laptops corporativas) solo se revisan cuando ya presentaron una falla o se apagaron inesperadamente. Esto genera tiempos de inactividad que afectan a toda la empresa.

- Datos implicados (V's): Tenemos un gran Volumen de logs diarios de eventos de red, Velocidad en la generación constante de datos de diagnóstico de los componentes, y Variedad porque manejamos reportes CIT, estados de Azure y bases de datos.

- Propuesta de solución: Implementar un modelo predictivo recolectando en tiempo real los logs de temperatura, rendimiento de CPU/RAM y pequeños errores previos de los equipos. Usando Big Data, el sistema analizaría estos patrones para alertar qué equipo tiene una alta probabilidad de fallar en los próximos 15 días, permitiendo agendar un mantenimiento preventivo sin detener la operación de los usuarios.
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
a) Por día: 8 millones * 500 = 4,000,000,000 (4 mil millones) de registros. Por año: 4 mil millones * 365 = 1,460,000,000,000 (1.46 billones) de registros.

b) Datos mayormente estructurados (montos de pago, duración de llamadas en segundos) y semi-estructurados (logs de navegación de datos móviles, metadatos de los SMS).

c) Volumen (manejar billones de registros) y Valor (el objetivo principal es usar esa data para predecir la fuga y salvar dinero reteniendo al cliente).

d) Necesitarían un Data Lake en la nube (AWS o Azure) para almacenar el histórico masivo a bajo costo, un motor de procesamiento distribuido como Apache Spark para limpiar la data, y algoritmos de Machine Learning (Python) para entrenar el modelo predictivo de cancelación.

e) Impacto ético: Hay un riesgo gigantesco de invasión a la privacidad. Analizar con quién habla un cliente, a qué hora, qué páginas navega y dónde está físicamente cruza líneas éticas severas. La solución obliga a que toda esta data esté estrictamente anonimizada; el modelo debe predecir basándose en el comportamiento numérico de un "ID de usuario", sin saber jamás su nombre, identidad o contenido de los mensajes.
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
Nombre del proyecto: Dataspace Institucional.

Sector: Educación Superior / Académico.

Problema que resuelve: En las universidades suele haber una desarticulación total de la información (las notas en un sistema, las asistencias en otro, y el uso del aula virtual por otro lado). Esto impide tener una visión unificada para aplicar análisis predictivo, como detectar a tiempo qué alumnos están en riesgo de desaprobar o abandonar la carrera.

V's presentes: Variedad (mezclamos bases relacionales tradicionales con logs semi-estructurados de plataformas virtuales) y Valor (transformar datos sueltos en predicciones que mejoren la retención estudiantil).
```

---

### Pregunta 12 — Arquitectura inicial

Dibuja (a mano o usando draw.io) una arquitectura inicial **muy básica** de cómo crees que debería funcionar tu proyecto. Incluye: fuentes de datos, almacenamiento, procesamiento y visualización.

_(Adjunta la imagen o el link de draw.io)_

_Link o descripción de tu diagrama_:

```
Para hacer tu esquema en draw.io, te sugiero esta estructura simple de izquierda a derecha mostrando cómo los datos pasan de su estado actual (AS-IS) al modelo futuro (TO-BE):

- Fuentes de Datos (Izquierda): Dibuja tres íconos de bases de datos. Etiquétalos como: BD Notas (MySQL), Logs del Aula Virtual (JSON), y Registros de Asistencia.

- Almacenamiento (Medio-Izquierda): Dibuja una nube (puede ser el logo de Microsoft Azure). Etiquétalo como Data Lake (Datos en crudo).

- Procesamiento (Medio-Derecha): Dibuja unos engranajes o el logo de Apache Spark / Python. Aquí pon una etiqueta que diga Integración y Limpieza de datos.

- Visualización / Analítica predictiva (Derecha): Dibuja una pantalla con gráficos (ej. PowerBI) donde el usuario final ve los dashboards con las predicciones de rendimiento.
```

---

## CRITERIOS DE EVALUACIÓN

| Criterio                                                           | Puntos |
| ------------------------------------------------------------------ | ------ |
| Responde todas las preguntas (no deja en blanco)                   | 4      |
| Usa sus propias palabras, no copia de internet                     | 4      |
| Da ejemplos reales de su entorno laboral                           | 4      |
| Las definiciones son técnicamente correctas                        | 4      |
| Respuestas de reflexión (P9, P11, P12) muestran pensamiento propio | 4      |
| **TOTAL**                                                          | **20** |

---

> **Recuerda**: La nota EC (10% del total) se basa en tu dominio conceptual. Esta guía de trabajo es el mejor preparativo. Si puedes responder estas 12 preguntas con seguridad, el examen EC no debería sorprenderte.

---

_Entrega: Subir al repositorio de GitHub Classroom o al foro de la plataforma virtual antes de la Semana 2_
