# radar-riesgo-migratorio-censo2024
Proyecto de ciencia de datos que construye un **índice de riesgo migratorio por hogar y por municipio/departamento** usando los microdatos del **Censo 2024 de El Salvador**.

---

## 1. Título y descripción

Radar de riesgo migratorio para El Salvador usando datos del Censo 2024.  
El objetivo es entregar una herramienta práctica que permita **priorizar territorios** con mayor probabilidad de que sus hogares tengan personas emigradas al extranjero.

---

## 2. Badges

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![Status](https://img.shields.io/badge/Status-En%20desarrollo-yellow.svg)]()

---

## 3. Problema que resuelve (el “por qué”)

- Las instituciones públicas, ONG y programas sociales **no siempre tienen tiempo ni capacidad técnica** para procesar microdatos censales.
- Esto dificulta **identificar municipios y departamentos con mayor riesgo migratorio**, donde conviven:
  - hogares con personas emigradas,
  - envío/recepción de remesas,
  - condiciones de vivienda y hacinamiento.

Este proyecto busca transformar la base censal en un **radar práctico de riesgo migratorio** que apoye la **planificación territorial y el diseño de políticas públicas**.

---

## 4. Solución propuesta (el “qué”)

**Radar-riesgo-migratorio-censo2024** construye un **modelo de riesgo migratorio a nivel de hogar** y lo resume territorialmente:

1. **Integración de microdatos** de:
   - Hogares (`hogares.csv`)
   - Viviendas (`viviendas.csv`)
   - Población (`poblacion.csv`)
2. **Construcción de la variable objetivo `y_hogar`**  
   Hogar con al menos una persona emigrante (1) vs. sin emigrantes (0).
3. **Ingeniería de variables**:
   - Tamaño del hogar, número de dormitorios y cuartos.
   - Razones de hacinamiento: `pers_por_dorm`, `pers_por_cuarto`.
   - Tipo de hogar, área rural/urbana, materiales de la vivienda, servicios básicos.
   - Variables ligadas a migración y remesas (por ejemplo `E02_EMI_AYUDA`).
4. **Modelo de clasificación**:
   - `RandomForestClassifier` (200 árboles, profundidad máxima 12, `class_weight="balanced"`).
   - Entrenado con **todo el dataset** de hogares.
5. **Score de riesgo**:
   - Para cada hogar se calcula `riesgo_migra` = probabilidad modelo de tener emigrantes.
6. **Agregaciones territoriales**:
   - Promedios de `riesgo_migra` por **departamento** (`DEPTO`) y **municipio** (`DEPTO + MUNIC`).
   - Exportación de tablas en `.csv` listas para visualización o GIS.

---

## 5. Características principales

- ✅ **Índice de riesgo migratorio por hogar** basado en probabilidad (0–1).
- ✅ **Tablas de riesgo por departamento y municipio**, con:
  - número de hogares,
  - hogares con emigrantes observados,
  - tasa observada,
  - riesgo promedio modelado.
- ✅ **Análisis de variables clave**:
  - Relación entre **remesas** y presencia de emigrantes.
  - **Hacinamiento** por cuartiles de riesgo.
  - Diferencias de riesgo según **tipo de hogar**.
- ✅ Código organizado en **notebooks** y **scripts reutilizables**.
- ✅ Pensado para integrarse luego con mapas, dashboards o SIG.

---

## 6. Tecnologías utilizadas (Tech stack)

**Lenguaje y entorno**

- Python 3.10+
- Jupyter Notebook

**Librerías principales**

- `pandas`, `numpy`
- `matplotlib`, `seaborn`
- `scikit-learn`

**Gestión del proyecto**

- Git & GitHub
- Estructura inspirada en *Cookiecutter Data Science* (datos, notebooks, src, docs).

---

## 7. Estructura del proyecto

```text
radar-riesgo-migratorio-censo2024/
├─ data/
│  ├─ raw/                       # Microdatos originales (NO incluidos en el repo)
│  │  ├─ hogares.csv
│  │  ├─ viviendas.csv
│  │  └─ poblacion.csv
│  ├─ processed/                 # Datos procesados / intermedios
│  │  └─ hogares_viviendas_modelo.parquet
│  └─ outputs/                   # Resultados finales listos para análisis
│     ├─ riesgo_migratorio_departamento.csv
│     └─ riesgo_migratorio_municipio.csv
│
├─ notebooks/
│  ├─ 01_preparacion_datos.ipynb        # Limpieza, unión de bases y creación de y_hogar
│  ├─ 02_modelo_riesgo_migratorio.ipynb # Entrenamiento del Random Forest y métricas
│  └─ 03_analisis_resultados.ipynb      # Agregaciones territoriales, tablas y gráficos
│
├─ src/
│  └─ features.py                # (Opcional) funciones reutilizables de features
│
├─ docs/                         # Documentación técnica
│  ├─ PROJECT_CHARTER.md
│  ├─ DATA_DICTIONARY.md
│  └─ MODEL_CARD.md
│
├─ README.md
└─ .gitignore
