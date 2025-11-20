# MODEL_CARD.md – Ficha técnica del modelo

## 1. Información General

- **Nombre del Modelo:** Modelo de Riesgo Migratorio por Hogar v1.0  
- **Proyecto:** Radar de riesgo migratorio – Censo El Salvador 2024  
- **Algoritmo principal:** RandomForestClassifier (scikit-learn)  
- **Fecha de entrenamiento:** 2025-11 (aprox.)  
- **Autor:** David Hernández (@davidhernandezsv)  

---

## 2. Descripción del Problema

### 2.1 Tarea

El modelo resuelve una tarea de **clasificación binaria**:

- **Entrada:** características del hogar y de la vivienda (tamaño del hogar, materiales, servicios, hacinamiento, área, etc.).
- **Salida:** probabilidad de que el hogar tenga **al menos un emigrante internacional**.

### 2.2 Objetivo

Permitir construir un **índice de riesgo migratorio** a nivel de hogar y luego agregarlo por:

- Departamento
- Municipio

para identificar territorios con mayor **vulnerabilidad migratoria**.

---

## 3. Datos de Entrenamiento

- **Fuente:** Microdatos del Censo de Población y Vivienda El Salvador 2024 (bases de Viviendas y Hogares).
- **Unidad de análisis:** hogar.
- **Número de registros:** ~1,9 millones de hogares (1920233 observaciones).
- **Variable objetivo (`y_hogar`):**
  - `1` = el hogar tiene emigrantes.
  - `0` = el hogar no tiene emigrantes.
- **Desbalance de clases (aprox.):**
  - 0: ~91.9 %
  - 1: ~8.1 %
- **Conjunto de validación:** división train/test 70% / 30%, estratificada por `y_hogar`.

### 3.1 Variables de entrada principales

Entre las variables más relevantes según importancia del Random Forest se encuentran:

- `E02_EMI_AYUDA_2` (relacionada con remesas / apoyo externo).
- `pers_por_cuarto`, `pers_por_dorm` (indicadores de hacinamiento).
- `HOGAR_TIPO_*` (tipo de hogar: ampliado, unipersonal, etc.).
- `AREA_hog_2`, `AREA_viv_2` (rural / urbano).
- Características de la vivienda (material de paredes, techo y piso, número de cuartos, número de hogares, etc.).

La lista completa de variables y descripciones se encuentra en `DATA_DICTIONARY.md`.

---

## 4. Algoritmo y Configuración

- **Modelo:** `RandomForestClassifier` de scikit-learn.
- **Parámetros clave:**
  - `n_estimators = 200`
  - `max_depth = 12`
  - `class_weight = 'balanced'`
  - `n_jobs = -1` (usa todos los núcleos disponibles)
  - `random_state = 42`
- **Preprocesamiento:**
  - Conversión explícita a numérico de las variables de conteo de personas y cuartos.
  - Creación de razones `pers_por_dorm` y `pers_por_cuarto`.
  - Variables categóricas codificadas con **one-hot encoding** usando `pandas.get_dummies(drop_first=True)`.

---

## 5. Evaluación del Modelo

Resultados en el **conjunto de prueba (30%)**:

### 5.1 Métricas globales

- **Accuracy:** ≈ 0.906  
- **ROC-AUC:** ≈ 0.79  

### 5.2 Métricas por clase (aprox.)

| Clase | Descripción               | Precision | Recall | F1-Score |
|-------|---------------------------|----------:|-------:|---------:|
| 0     | Hogares sin emigrantes    | ~0.97     | ~0.75  | ~0.84    |
| 1     | Hogares con emigrantes    | ~0.20     | ~0.72  | ~0.31    |

> Nota: Los valores exactos dependen de la corrida final, pero el patrón general es: **muy buena identificación de la clase 0** y un **recall razonable para la clase 1**, con precisión más baja por el desbalance.

### 5.3 Interpretación

- El modelo es **útil para estimar un score de riesgo continuo (`riesgo_migra`)**, más que para tomar decisiones binarias estrictas a nivel de hogar.
- A nivel agregado (departamento/municipio), el score promedio captura bien los **patrones territoriales** de la tasa observada de hogares con emigrantes.

---

## 6. Uso Esperado del Modelo

### 6.1 Casos de uso recomendados

- Construir mapas de calor de **riesgo migratorio** por:
  - Departamento
  - Municipio
- Priorizar territorios para:
  - Programas de empleo y formación.
  - Intervenciones de protección social.
  - Estudios más detallados sobre migración.

### 6.2 No se debe usar para

- Tomar decisiones **individuales** sobre hogares o personas.
- Evaluar la “probabilidad de emigrar” de un individuo específico.
- Justificar decisiones administrativas o legales sin análisis complementarios.

---

## 7. Limitaciones y Sesgos Potenciales

- **Desbalance de clases:** solo ~8% de los hogares reportan emigrantes; la precisión sobre la clase 1 es relativamente baja.
- **Subregistro posible:** la migración puede estar subreportada en ciertas zonas, lo que sesga la variable objetivo.
- **No causalidad:** las variables importantes (hacinamiento, materiales, etc.) están asociadas al riesgo, pero no implican que causen migración.
- **Dependencia del Censo 2024:** el modelo está ajustado a este momento en el tiempo; cambios estructurales futuros pueden requerir re-entrenamiento.

---

## 8. Versionado y Próximos Pasos

- **Versión actual:** v1.0 – Modelo base con Random Forest.
- Próximas mejoras posibles:
  - Ajustar umbrales y calibración de probabilidades (Platt scaling / isotonic).
  - Probar modelos adicionales (XGBoost, LightGBM, regresión logística regularizada).
  - Incorporar variables adicionales de personas (edad, educación, empleo) agregadas a nivel de hogar.
  - Diseñar dashboards interactivos (ej. Power BI, Streamlit) usando los outputs de `data/outputs/`.

---

## 9. Contacto

- **Responsable del proyecto:**  
  David Hernández – Ingeniería Industrial / Data Science  
  GitHub: [@davidhernandezsv](https://github.com/davidhernandezsv)

