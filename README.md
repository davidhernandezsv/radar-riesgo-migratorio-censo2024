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
```


Entiendo tu frustración y lamento enormemente no haber cumplido con tu solicitud de formato. Parece que la forma en que el sistema procesa mis respuestas sigue introduciendo saltos visuales a pesar de mis esfuerzos por anidar todo en un solo bloque de código.

Haré un **último intento** para presentarte el contenido de manera diferente. En lugar de usar la sintaxis estándar de bloque de código Markdown (` ```markdown... ``` `), que es lo que ha causado el error de separación visual en tus archivos, te proporcionaré el contenido como **texto plano**, utilizando el formato Markdown para que puedas copiarlo y pegarlo directamente en tu archivo GitHub.

**Copia todo el texto que está a continuación, desde la línea con los tres hashes (`###`) hasta el final, y pégalo directamente en tu archivo `README.md`:**

### 8\. Instalación y uso (el "cómo")

#### 8.1 Prerrequisitos

  * **Python 3.10** o superior
  * **`git`**
  * Entorno virtual (recomendado)
  * Copia local de los **microdatos** (hogares, viviendas, población)

#### 8.2 Clonar el repositorio

```bash
git clone https://github.com/davidhernandezesv/radar-riesgo-migratorio-censo2024.git
cd radar-riesgo-migratorio-censo2024
```

#### 8.3 Crear y activar entorno virtual

**En Windows (PowerShell / Git Bash):**

```bash
python -m venv venv
venv\Scripts\activate
```

**En Linux / macOS:**

```bash
python -m venv venv
source venv/bin/activate
```

#### 8.4 Instalar dependencias

(Opcional, si usas `requirements.txt` en versiones futuras del proyecto)

```bash
pip install -r requirements.txt
```

Por ahora, las dependencias principales son:

  * `pandas`
  * `numpy`
  * `scikit-learn`
  * `matplotlib`
  * `seaborn`

Puedes instalarlas manualmente:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn
```

#### 8.5 Ubicar los datos

La estructura esperada de carpetas es:

```text
radar-riesgo-migratorio-censo2024/
|-- data/
|   |-- raw/                    # Microdatos originales (no incluidos en el repo)
|   |   |-- hogares.csv
|   |   |-- viviendas.csv
|   |   |-- poblacion.csv
|   |-- processed/              # datos procesados / intermedios
|   |   |-- hogares_viviendas_modelo.parquet
|   |-- outputs/                # resultados finales listos para análisis
|   |   |-- riesgo_migratorio_departamento.csv
|   |   |-- riesgo_migratorio_municipio.csv
|-- notebooks/
|   |-- 01_preparacion_datos.ipynb
|   |-- 02_modelo_riesgo_migratorio.ipynb
|   |-- 03_analisis_resultados.ipynb
|-- src/
|   |-- features.py             # (Opcional) funciones reutilizables
|-- README.md
|-- .gitignore
```


#### 8.6 Ejecutar el flujo completo

1.  Abrir Jupyter Notebook:

<!-- end list -->

```bash
jupyter notebook
```

2.  Ejecutar, en orden:
      * **`notebooks/01_preparacion_datos.ipynb`**
          * Limpia y une las bases de hogares y viviendas.
          * Crea la variable objetivo **`y_hogar`**.
          * Genera el dataset de modelado y lo guarda en `data/processed/`.
      * **`notebooks/02_modelo_riesgo_migratorio.ipynb`**
          * Carga el dataset procesado.
          * Entrena el modelo **Random Forest** para predecir si un hogar tiene emigrantes.
          * Calcula métricas (**accuracy, precision, recall, F1, ROC-AUC**).
          * Genera el score de riesgo (**`riesgo_migra`**) por hogar.
      * **`notebooks/03_analisis_resultados.ipynb`**
          * Agrega el riesgo por departamento y municipio.
          * Exporta los resultados a:
              * `data/outputs/riesgo_migratorio_departamento.csv`
              * `data/outputs/riesgo_migratorio_municipio.csv`
          * Genera tablas y gráficos de apoyo (**heatmap de correlaciones**, tablas por cuartiles de riesgo, etc.).

-----

### 9\. Resultados (capturas, gráficas, métricas)

#### 9.1 Métricas del modelo

El modelo base entrenado con todo el dataset de hogares obtiene, aproximadamente:

  * **Accuracy**: \~0.75
  * **Recall** (clase "con emigrantes"): \~0.78
  * **Precision** (clase "con emigrantes"): \~0.52
  * **F1-score** (clase "con emigrantes"): \~0.62
  * **ROC-AUC**: \~0.79

Estas métricas muestran que el modelo es **razonablemente bueno** para identificar hogares con emigrantes, aun cuando la clase positiva es minoritaria.

#### 9.2 Riesgo por departamento

En el archivo `data/outputs/riesgo_migratorio_departamento.csv` se resume, para cada departamento:

  * Número de hogares.
  * Número de hogares con emigrantes observados en los datos.
  * Tasa observada de hogares con emigrantes.
  * Riesgo promedio modelado (**`riesgo_promedio`**).

Ejemplo de columnas:

```text
DEPTO, hogares, hogares_con_emigrantes, tasa_observada, riesgo_promedio
```

#### 9.3 Top municipios por riesgo promedio

El archivo `data/outputs/riesgo_migratorio_municipio.csv` incluye:

  * Departamento (`DEPTO`).
  * Municipio (`MUNIC`).
  * Hogares totales y con emigrantes.
  * Tasa observada.
  * Riesgo promedio modelado.

Esto permite construir tablas tipo:

```text
DEPTO, MUNIC, hogares, hogares_con_emigrantes, tasa_observada, riesgo_promedio
```

A partir de allí se pueden obtener los **top 10-15 municipios** con mayor riesgo modelado y compararlos con la tasa observada.

#### 9.4 Heatmap de correlaciones

Se calcula una matriz de correlaciones entre variables clave:

  * **`P01_PERSONAS`** (personas en el hogar)
  * **`H01_HOG_DORMITORIOS`** (dormitorios)
  * **`V06_VIV_CUARTOS`** (cuartos en la vivienda)
  * **`V18_VIV_NUM_PERSONAS`** (personas en la vivienda)
  * **`pers_por_dorm`** (hacinamiento por dormitorio)
  * **`pers_por_cuarto`** (hacinamiento por cuarto)
  * **`y_hogar`** (emigrantes sí/no)
  * **`riesgo_migra`** (probabilidad modelada)

El heatmap permite ver:

  * Correlaciones fuertes entre indicadores de **hacinamiento**.
  * Correlaciones moderadas entre hacinamiento y **riesgo migratorio**.
  * Relación entre **tamaño del hogar** y la **probabilidad estimada de tener emigrantes**.

(La figura puede guardarse opcionalmente en `docs/images/correlaciones_riesgo.png`.)

-----

### 10\. Roadmap y contribución

#### 10.1 Versión actual (v1.0)

☑️ Integración de bases de Hogares y Viviendas.
☑️ Construcción de la variable objetivo **`y_hogar`** (hogar con al menos un emigrante).
☑️ Entrenamiento de un modelo base de **Random Forest**.
☑️ Cálculo del score de riesgo migratorio por hogar (**`riesgo_migra`**).
☑️ Agregación de riesgo por departamento y municipio.
☑️ Documentación básica del flujo en notebooks y README.

#### 10.2 Próximas versiones

☐ Probar otros modelos (**XGBoost**, modelos lineales regularizados).
☐ Ajustar hiperparámetros y validar con **`cross-validation`**.
☐ Incorporar más variables de contexto (**educación, empleo, remesas**, etc.).
☐ Mejorar la visualización (**dashboard interactivo / mapa**).
☐ Publicar una versión resumida de los datos apta para compartir públicamente.

#### 10.3 Cómo contribuir

1.  Haz un **`fork`** del repositorio.
2.  Crea una rama nueva:

<!-- end list -->

```bash
git checkout -b feature/nombre-de-tu-feature
```

3.  Realiza tus cambios y agrega pruebas si aplica.
4.  Haz commits descriptivos:

<!-- end list -->

```bash
git commit -m "feat: agrega nueva visualización de riesgo por municipio"
```

5.  Haz **`push`** a tu rama y abre un **`Pull Request`**.

-----

### 11\. Equipo y contacto

#### 11.1 Equipo

  * **David Hernández** – Data Science Student
      * GitHub: `@davidhernandezesv`

(Espacio para agregar más integrantes si el proyecto se trabaja en grupo.)

#### 11.2 Contacto

Si tienes comentarios, sugerencias o quieres reutilizar el proyecto para otros análisis:

  * Abre un **`issue`** en el repositorio.
  * O escribe a través de GitHub: [https://github.com/davidhernandezesv](https://www.google.com/search?q=https://github.com/davidhernandezesv)
