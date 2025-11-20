# Project Charter: Radar de Riesgo Migratorio – Censo El Salvador 2024

## 1. Objetivo de Negocio

Las instituciones públicas, ONG y academia en El Salvador necesitan **identificar territorios con mayor riesgo migratorio** para focalizar programas de empleo, educación, protección social y prevención de la migración forzada.  

Actualmente, los microdatos del Censo 2024 son voluminosos y difíciles de utilizar directamente para la toma de decisiones, lo que limita:

- La capacidad de **priorizar municipios y distritos** según su vulnerabilidad.
- La identificación de **patrones territoriales** de hogares con emigrantes.
- La planificación basada en **evidencia cuantitativa**.

Este proyecto busca transformar la base censal en un **radar práctico de riesgo migratorio por territorio**.

---

## 2. Objetivo de Data Science

Desarrollar un **modelo supervisado de clasificación** que estime, para cada hogar censado, la **probabilidad de tener al menos un emigrante internacional**, utilizando variables de:

- Composición del hogar (tamaño, tipo, densidad).
- Condiciones de vivienda y hacinamiento.
- Ubicación geográfica (departamento, municipio).
- Indicadores indirectos de vulnerabilidad.

A partir de ese modelo se construirá un **Índice de Riesgo Migratorio** que luego se agregará por:

- Departamento
- Municipio

para generar tablas e insumos que puedan integrarse en un futuro **dashboard de visualización**.

---

## 3. Alcance del Proyecto

### 3.1 Alcance INCLUIDO

- Uso de los **microdatos de Viviendas y Hogares** del Censo El Salvador 2024 (hogares particulares).
- Integración de las bases mediante la estructura **COD_PROP / COD_VIV / COD_HOG**.
- Definición de una variable objetivo binaria `y_hogar`:
  - `1` = el hogar reporta al menos un emigrante.
  - `0` = el hogar no reporta emigrantes.
- Ingeniería de variables clave, incluyendo:
  - Indicadores de **hacinamiento**: `pers_por_dorm`, `pers_por_cuarto`.
  - Características de vivienda: materiales, servicios básicos, tenencia.
  - Tamaño del hogar y número de hogares en la vivienda.
- Entrenamiento de un modelo base con **RandomForestClassifier** (scikit-learn).
- Cálculo de una columna de **riesgo por hogar**: `riesgo_migra` (probabilidad estimada).
- Agregación de resultados:
  - Riesgo promedio y tasa observada de hogares con emigrantes por **departamento**.
  - Top de municipios con mayor riesgo promedio.
- Generación de:
  - Tablas CSV en `data/outputs/` (`riesgo_migratorio_departamento.csv`, `riesgo_migratorio_municipio.csv`).
  - Gráficos exploratorios (matriz de correlaciones, distribución de riesgo).
- Documentación técnica mínima:
  - `PROJECT_CHARTER.md`
  - `DATA_DICTIONARY.md`
  - `MODEL_CARD.md`
  - `README.md`

### 3.2 Fuera de Alcance (Out of Scope)

- Implementar un **dashboard web en producción** (solo se deja preparado el output para futuras visualizaciones).
- Integración con sistemas internos de instituciones gubernamentales (ministerios, alcaldías, etc.).
- Modelos de series de tiempo o pronósticos futuros de flujos migratorios.
- Análisis cualitativo de causas de migración (se trabaja únicamente con datos censales).
- Diseño de políticas públicas; el proyecto solo provee **evidencia cuantitativa**, no recomendaciones normativas formales.

---

## 4. Entregables Principales

1. **Repositorio GitHub**  
   - Código, notebooks y documentación del proyecto.

2. **Notebooks principales** (carpeta `notebooks/`):
   - `01_preparacion_datos.ipynb` – Limpieza, unión de bases y creación de `y_hogar`.
   - `02_modelo_riesgo_migratorio.ipynb` – Entrenamiento del Random Forest, métricas y variable `riesgo_migra`.
   - `03_analisis_resultados.ipynb` – Agregaciones territoriales, tablas de riesgo y gráficos.

3. **Datos procesados / resultados** (carpeta `data/outputs/`):
   - `riesgo_migratorio_departamento.csv`
   - `riesgo_migratorio_municipio.csv`
   - Otros CSV intermedios que resulten útiles.

4. **Documentación técnica** (carpeta `docs/`):
   - `PROJECT_CHARTER.md` (este documento).
   - `DATA_DICTIONARY.md` (diccionario de variables utilizadas).
   - `MODEL_CARD.md` (ficha técnica del modelo).
   - README del proyecto (en raíz).

---

## 5. Stakeholders y Usuarios Finales

- **Instituciones públicas**:
  - Ministerios relacionados con trabajo, desarrollo local y protección social.
  - Alcaldías y gobiernos locales interesados en priorizar intervenciones.
- **Organismos internacionales y ONG**:
  - Programas que trabajan en migración, protección de derechos, empleo juvenil, etc.
- **Academia y centros de investigación**:
  - Investigadores que analizan patrones de migración internacional y vulnerabilidad territorial.
- **Equipo de Data Science / Analistas**:
  - Responsables de mantener, adaptar y escalar el modelo con nuevas versiones del censo o datos complementarios.

---

## 6. Métricas de Éxito del Proyecto

### 6.1 Métricas de desempeño del modelo

- **ROC-AUC** en conjunto de prueba: objetivo ≥ 0.75.  
  - Resultado modelo base: ~0.79.
- **Recall (sensibilidad) para la clase 1 (hogares con emigrantes)**:  
  - Prioridad media/alta, dado que interesa no dejar escapar territorios de alto riesgo.
- **Estabilidad de las variables importantes**:  
  - Validar que las variables destacadas (hacinamiento, tipo de hogar, remesas, condiciones de vivienda) tengan sentido socioeconómico.

### 6.2 Métricas de valor para el negocio

- Existencia de **tablas de riesgo por departamento y municipio** listas para ser consumidas por otros equipos.
- Capacidad de **identificar “hotspots” territoriales** (municipios en los percentiles más altos de riesgo).
- Facilidad para **actualizar el modelo** si se incorporan nuevas variables o se refinan definiciones.

---

## 7. Riesgos y Supuestos

- Supuesto: la variable censal utilizada para identificar emigrantes es **confiable y bien reportada**.
- Supuesto: la combinación de hogar + vivienda es suficiente para capturar buena parte del riesgo migratorio.
- Riesgo: posible **subregistro** de emigrantes en determinadas zonas.
- Riesgo: el modelo captura patrones estructurales del censo 2024, pero **no implica causalidad** ni predice decisiones individuales.

