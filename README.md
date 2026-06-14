# 🐘 BIG DATA — DD283 | Universidad Autónoma del Perú

## Semestre 2026-1 | Ingeniería de Sistemas | Ciclo VIII

[![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python)](https://python.org)
[![PySpark](https://img.shields.io/badge/Apache_Spark-3.5-orange?logo=apache-spark)](https://spark.apache.org)
[![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-green?logo=mongodb)](https://cloud.mongodb.com)
[![Docker](https://img.shields.io/badge/Docker-Desktop-blue?logo=docker)](https://docker.com)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

---

## 📋 INFORMACIÓN DEL CURSO

| Item | Detalle |
|------|---------|
| **Código** | DD283 |
| **Duración** | 8 semanas |
| **Horas/semana** | 3h Teoría + 2h Práctica |
| **Modalidad** | Semipresencial |
| **Nota mínima** | 10.5 / 20 |
| **Fórmula** | `(EC × 0.10) + (EP × 0.40) + (EF × 0.50)` |

---

## 🗓️ CALENDARIO DE CONTENIDOS

| Semana | Tema | Herramientas | Entregable |
|--------|------|-------------|-----------|
| [S01](./semana_01/) | Intro Big Data · Arquitectura · Clusters | Python, Draw.io | Lab S1 + PR |
| [S02](./semana_02/) | Cloud · Hadoop · HDFS · MapReduce · HBase | Docker, PySpark | Lab S2 + PR |
| [S03](./semana_03/) | Hadoop Avanzado · NoSQL | MongoDB Atlas | Lab S3 + PR |
| [S04](./semana_04/) | NewSQL · **Examen Parcial EC + EP** | CockroachDB | PR + Sustentación |
| [S05](./semana_05/) | Apache Spark SQL · Streaming · GraphX | Databricks | Lab S5 + PR |
| [S06](./semana_06/) | Machine Learning en Big Data | PySpark MLlib | Modelo ML + PR |
| [S07](./semana_07/) | Web Scraping · Limpieza de Datos | Scrapy, Pandas | Dataset limpio + PR |
| [S08](./semana_08/) | Casos de Éxito · **Evaluación Final EF** | Power BI | Proyecto Final |

---

## 🚀 INICIO RÁPIDO

### Paso 1: Fork del repositorio

```bash
# 1. Haz FORK de este repositorio en tu cuenta de GitHub
# 2. Clona TU fork (no el original)
git clone https://github.com/TU_USUARIO/bigdata-ua-2026-1.git
cd bigdata-ua-2026-1
```

### Paso 2: Configurar el entorno

```bash
# Opción A: Conda (recomendado para trabajo local)
conda env create -f setup/environment.yml
conda activate bigdata-ua

# Opción B: pip
pip install -r setup/requirements.txt

# Opción C: Google Colab (sin instalación)
# Ir a colab.research.google.com y abrir los notebooks directamente
```

### Paso 3: Verificar instalación

```bash
python setup/verificar_entorno.py
```

---

## 📂 ESTRUCTURA DEL REPOSITORIO

```
bigdata-ua-2026-1/
├── README.md                    ← Este archivo (syllabus + instrucciones)
├── setup/
│   ├── environment.yml          ← Entorno Conda completo
│   ├── requirements.txt         ← Dependencias pip
│   ├── docker-compose.yml       ← Stack Hadoop local
│   └── verificar_entorno.py     ← Script de verificación
├── semana_01/                   ← Cada semana tiene:
│   ├── slides/                  ←   PPT de la sesión
│   ├── notebooks/               ←   Notebooks Jupyter (demos + labs)
│   │   ├── demo_s1_docente.ipynb
│   │   └── lab_s1_estudiante.ipynb
│   └── datasets/                ←   Datos para los ejercicios
├── semana_02/ ... semana_08/    ← Misma estructura
├── proyectos/
│   ├── README_proyectos.md      ← Descripción de los 10 proyectos
│   └── template_proyecto/       ← Template que debe copiar cada grupo
│       ├── README.md
│       ├── notebooks/
│       ├── data/
│       └── docs/
└── datasets/
    ├── README_datasets.md        ← Descripción de todos los datasets
    └── (datasets compartidos para todos los labs)
```

---

## 🔄 FLUJO DE TRABAJO SEMANAL (¡IMPORTANTE!)

### Cada semana DEBES hacer un Pull Request con tu trabajo

```
Semana N comienza
     ↓
1. git pull upstream main      ← Sincronizar con el repo del docente
     ↓
2. git checkout -b semana-N-tu-nombre   ← Crear rama para la semana
     ↓
3. Trabajar en el lab de la semana
   (notebook, datasets, análisis)
     ↓
4. git add . && git commit -m "S0N: lab completado"
     ↓
5. git push origin semana-N-tu-nombre
     ↓
6. Crear Pull Request en GitHub
   (title: "[S0N] Lab Semana N - Tu Nombre")
     ↓
Docente revisa y da feedback en el PR
```

---

## 📊 SISTEMA DE EVALUACIÓN

```
Nota Final = (EC × 0.10) + (EP × 0.40) + (EF × 0.50)

EC  (10%) → Examen individual de conceptos — Semana 4
EP  (40%) → Sustentación grupal avance del proyecto — Semana 4
EF  (50%) → Sustentación grupal proyecto completo — Semana 8

Mínimo aprobatorio: 10.5 / 20
```

### Rúbrica de Pull Requests semanales
>
> Los PRs semanales son **formativos** (no tienen nota directa) pero el docente
> los revisa y su calidad impacta en EP y EF.

| Criterio | Descripción |
|---------|-------------|
| ✅ Notebook ejecutado sin errores | Todas las celdas corren correctamente |
| ✅ Reflexión en Markdown | Celda final con aprendizajes y conexión con tu empresa |
| ✅ Commit message descriptivo | `[S02] Word Count MapReduce - Juan Pérez` |
| ✅ PR en tiempo | Antes del inicio de la siguiente sesión |

---

## 🛠️ HERRAMIENTAS DEL CURSO

| Herramienta | Uso | Cómo obtener |
|-------------|-----|-------------|
| **Google Colab** | Notebooks Python sin instalación | [colab.research.google.com](https://colab.research.google.com) |
| **Docker Desktop** | Cluster Hadoop local | [docker.com](https://docker.com/products/docker-desktop) |
| **MongoDB Atlas** | Base de datos NoSQL cloud | [cloud.mongodb.com](https://cloud.mongodb.com) (gratis) |
| **Databricks Community** | Apache Spark cloud | [community.cloud.databricks.com](https://community.cloud.databricks.com) (gratis) |
| **CockroachDB Cloud** | Base de datos NewSQL | [cockroachlabs.cloud](https://cockroachlabs.cloud) (gratis) |
| **Draw.io** | Diagramas de arquitectura | [diagrams.net](https://diagrams.net) (gratis online) |
| **Power BI Desktop** | Visualización final | [powerbi.microsoft.com](https://powerbi.microsoft.com) (gratis) |

---

## 🚀 10 PROYECTOS INNOVADORES

Ver descripción completa en [proyectos/README_proyectos.md](./proyectos/README_proyectos.md)

| # | Proyecto | Sector |
|---|---------|--------|
| P1 | Sistema de detección de fraude bancario en tiempo real | Finanzas |
| P2 | Predicción de churn en telecomunicaciones | Telco |
| P3 | Plataforma de análisis de sentimiento en RRSS | Digital |
| P4 | Motor de recomendación para e-commerce peruano | Retail |
| P5 | Predicción de demanda hospitalaria EsSalud | Salud |
| P6 | Detección de evasión fiscal con Big Data SUNAT | Gobierno |
| P7 | Análisis de tráfico urbano Lima con datos GPS | Smart City |
| P8 | Plataforma de análisis de calidad ambiental IoT | Ambiente |
| P9 | Sistema de crédito alternativo para microempresas | Fintech |
| P10 | Análisis de deserción estudiantil universitaria | EdTech |

---

## 📚 BIBLIOGRAFÍA

**Principales:**

- Marques Perez, M. (2019). *Big Data: técnicas, herramientas y aplicaciones*. Alfaomega.
- Caballero, R. (2019). *Big Data con Python*. Alfaomega.
- Zaharia, M. et al. (2020). *Learning Spark, 2nd Edition*. O'Reilly. [PDF gratuito](https://pages.databricks.com/rs/094-YMS-629/images/LearningSpark2.0.pdf)

**Recursos Online Gratuitos:**

- [Fundamentos de Big Data - UOC](https://openaccess.uoc.edu/server/api/core/bitstreams/4efc24a4-9563-4a0b-97d8-b790954ba50d/content)
- [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
- [MongoDB University (cursos gratis)](https://university.mongodb.com/)
- [Kaggle Courses](https://kaggle.com/learn)

---

## 🤝 CÓMO CONTRIBUIR (Estudiantes)

1. **No commitees directamente a `main`** — siempre usa ramas
2. **Un PR por semana** — siguiendo la nomenclatura `semana-N-tu-nombre`
3. **Reviewea el PR de un compañero** — el docente asigna revisores
4. **Responde el feedback** — actualiza tu PR si el docente pide cambios

---

## ❓ SOPORTE

- **Foro del curso**: Plataforma Virtual de la UA
- **Issues de GitHub**: Para bugs en el código del repositorio
- **Office Hours**: Consultar horario con el docente
- **Email**: Usar el correo institucional con asunto `[DD283] Tu consulta`

---

<div align="center">

**Universidad Autónoma del Perú | Facultad de Ingeniería y Arquitectura**

*"Los datos son el nuevo petróleo — pero solo si saben refinarlo."*

</div>
# bigdata-ua-2026-1
Curso de Big Data 2026-1
