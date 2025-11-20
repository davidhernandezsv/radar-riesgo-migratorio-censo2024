# DATA_DICTIONARY.md – Diccionario de Datos

Este documento describe las principales variables utilizadas en el **modelo de riesgo migratorio por hogar** a partir del Censo de El Salvador 2024.

---

## Tabla: hogares_viviendas_modelo

### Identificación geográfica

| Campo   | Tipo      | Descripción                                                                 | Ejemplo | Restricciones |
|--------|-----------|-------------------------------------------------------------------------------|---------|--------------|
| `DEPTO` | entero (int) | Código de departamento (2 dígitos) del hogar.                                 | 06      | Valores 01–14 |
| `MUNIC` | entero (int) | Código de municipio (2 dígitos) dentro del departamento.                      | 02      | Según catálogo DIGESTYC |
| `DISTO` | entero (int) | Código de distrito censal (2 dígitos) dentro del municipio.                   | 03      | Según catálogo DIGESTYC |

---

### Estructura del hogar

| Campo             | Tipo         | Descripción                                                                                       | Ejemplo | Restricciones |
|------------------|--------------|---------------------------------------------------------------------------------------------------|---------|--------------|
| `P01_PERSONAS`   | entero (int) | Número total de personas que conforman el hogar.                                                 | 4       | ≥ 1 |
| `HOGAR_TIPO`     | categórica   | Tipo de hogar (nuclear, ampliado, unipersonal, etc.) según clasificación censal.                | 5       | Ver catálogo de tipos de hogar |
| `H01_HOG_DORMITORIOS` | entero (int) | Número de dormitorios disponibles para el hogar.                                              | 2       | ≥ 0 |
| `M01_MORT_MORTALIDAD` | categórica | Indicador de si el hogar reportó fallecimientos recientes (variable de mortalidad en el hogar). | 1       | Codificación según cuestionario |
| `M01_1_TOTAL_FALL` | entero (int) | Número total de personas fallecidas reportadas en el hogar (periodo de referencia censal).     | 0       | ≥ 0 |
| `E02_EMI_AYUDA`  | categórica   | Variable asociada a apoyo/remesas relacionadas con emigrantes (p.ej., reciben ayuda del exterior). | 2       | 1 = No recibe, 2 = Recibe (dependiendo del diseño de la variable) |

---

### Condiciones de vivienda

| Campo                | Tipo         | Descripción                                                                                                    | Ejemplo | Restricciones |
|---------------------|--------------|----------------------------------------------------------------------------------------------------------------|---------|--------------|
| `I01_IDEM_ESTRUC`   | categórica   | Identificador de la estructura física / inmueble (COD_PROP).                                                  | 060305000323 | Cadena estructurada según DIGESTYC |
| `V01_VIV`           | categórica   | Tipo de vivienda (particular/colectiva).                                                                      | 1       | 1 = Particular, 2 = Colectiva, etc. |
| `V01_1_VIV_PARTICULAR` | binaria   | Indicador dummy para vivienda particular.                                                                     | 1       | 0 / 1 |
| `V01_2_VIV_COLECTIVA` | binaria    | Indicador dummy para vivienda colectiva.                                                                      | 0       | 0 / 1 |
| `V02_VIV_OCUPACION` | categórica   | Condición de ocupación de la vivienda (ocupada, desocupada, de uso temporal, etc.).                           | 1       | Ver catálogo oficial. |
| `V2_1_VIV_OCUPADA`  | binaria      | Dummy: vivienda ocupada.                                                                                       | 1       | 0 / 1 |
| `V2_2_VIV_DESOCUPADA` | binaria   | Dummy: vivienda desocupada.                                                                                    | 0       | 0 / 1 |
| `V03_VIV_PARED`     | categórica   | Material predominante de las paredes (bloque, adobe, madera, etc.).                                           | 5       | Ver etiquetas de materiales. |
| `V04_VIV_TECHO`     | categórica   | Material predominante del techo.                                                                              | 3       | Ver catálogo. |
| `V05_VIV_PISO`      | categórica   | Material predominante del piso.                                                                               | 7       | Ver catálogo. |
| `V06_VIV_CUARTOS`   | entero (int) | Número total de cuartos de la vivienda (incluye dormitorios y otros cuartos).                                 | 4       | ≥ 0 |
| `V07_VIV_TENENCIA`  | categórica   | Régimen de tenencia de la vivienda (propia, alquilada, cedida, etc.).                                        | 2       | Ver catálogo. |
| `V08_VIV_TIPO_SANIT` | categórica  | Tipo de servicio sanitario (inodoro con descarga, letrina, ninguno, etc.).                                   | 3       | Ver catálogo. |
| `V09_VIV_USO_SANIT` | categórica   | Uso compartido o exclusivo del servicio sanitario.                                                            | 1       | Ver catálogo. |
| `V10_VIV_AGUAS_GRISES` | categórica | Forma de disposición de aguas grises.                                                                         | 2       | Ver catálogo. |
| `V11_VIV_AGUA`      | categórica   | Forma de abastecimiento de agua para la vivienda.                                                             | 1       | Ver catálogo. |
| `V12_VIV_PROVIENE_AGUA` | categórica | Fuente principal de donde proviene el agua.                                                               | 4       | Ver catálogo. |
| `V12_1_VIV_ABASTO_AGUA` | categórica | Variable derivada asociada al tipo de abastecimiento de agua.                                             | 1       | Según derivación. |
| `V13_VIV_FUENTE_ENERGIA` | categórica | Fuente principal de energía eléctrica.                                                                    | 1       | Ver catálogo. |
| `V14_VIV_COMBUSTIBLE_COCINA` | categórica | Combustible principal usado para cocinar.                                                            | 2       | Ver catálogo. |
| `V15_VIV_ELIM_BASURA` | categórica | Forma de eliminación de basura.                                                                             | 3       | Ver catálogo. |
| `V16_VIV_OLLA_COMUN` | categórica | Participación de la vivienda en olla común u otros mecanismos colectivos.                                   | 1       | Ver catálogo. |
| `V17_VIV_NUM_HOGARES` | entero (int) | Número de hogares que residen en la misma vivienda.                                                       | 2       | ≥ 1 |
| `V18_VIV_NUM_PERSONAS` | entero (int) | Número total de personas que habitan la vivienda (suma de todos los hogares).                            | 6       | ≥ 1 |
| `AREA_viv`          | categórica   | Área geográfica de la vivienda (urbana/rural) según variable censal de viviendas.                            | 2       | Valores codificados (ej. 1 = Urbano, 2 = Rural). |
| `GHS_DUG_L1_viv`    | categórica   | Clasificación de grado de urbanización (nivel 1) para la vivienda.                                          | 3       | Según GHS-DUG. |
| `GHS_DUG_L2_viv`    | categórica   | Clasificación de grado de urbanización (nivel 2) para la vivienda.                                          | 10      | Según GHS-DUG. |

---

### Variables derivadas / ingeniería de características

| Campo           | Tipo          | Descripción                                                                                  | Ejemplo | Restricciones |
|----------------|---------------|----------------------------------------------------------------------------------------------|---------|--------------|
| `pers_por_dorm` | numérico (float) | Personas del hogar por dormitorio (`P01_PERSONAS / H01_HOG_DORMITORIOS`, con denominador ≥ 1). | 2.5     | Si `H01_HOG_DORMITORIOS` = 0 se reemplaza por 1 para evitar división por cero. |
| `pers_por_cuarto` | numérico (float) | Personas de la vivienda por cuarto (`V18_VIV_NUM_PERSONAS / V06_VIV_CUARTOS`, con denominador ≥ 1). | 1.8     | Si `V06_VIV_CUARTOS` = 0 se reemplaza por 1. |

---

### Variables de área asociadas al hogar

| Campo        | Tipo        | Descripción                                                                            | Ejemplo | Restricciones |
|-------------|-------------|----------------------------------------------------------------------------------------|---------|--------------|
| `AREA_hog`  | categórica  | Área geográfica del hogar (urbano/rural) según clasificación en la base de hogares.   | 1       | 1 = Urbano, 2 = Rural (según codificación). |
| `GHS_DUG_L1_hog` | categórica | Grado de urbanización (nivel 1) asociado al hogar.                               | 3       | Según GHS-DUG. |
| `GHS_DUG_L2_hog` | categórica | Grado de urbanización (nivel 2) asociado al hogar.                               | 30      | Según GHS-DUG. |

---

### Variable objetivo y score de riesgo

| Campo          | Tipo           | Descripción                                                                                  | Ejemplo | Restricciones |
|---------------|----------------|----------------------------------------------------------------------------------------------|---------|--------------|
| `y_hogar`     | binaria (0/1)  | Variable objetivo: indica si el hogar tiene al menos un emigrante internacional reportado.  | 0       | 0 = Sin emigrantes, 1 = Con emigrantes. |
| `riesgo_migra` | numérico (float entre 0 y 1) | Probabilidad estimada por el modelo de que el hogar tenga emigrantes.             | 0.54    | Estimada con RandomForestClassifier (`predict_proba`). |

---

### Notas Generales

- Los **microdatos originales** de viviendas y hogares se almacenan en `data/raw/` y **no se versionan** en el repositorio (por tamaño y confidencialidad).
- Los datos intermedios (unión de viviendas y hogares, más variables derivadas) se almacenan en `data/processed/hogares_viviendas_modelo.parquet`.
- Los agregados territoriales listos para análisis se almacenan en `data/outputs/`.

