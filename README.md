# Machine Learning Tools — Taller Segundo Corte

**Universidad Santo Tomás**  
Facultad de Ingeniería en Tecnologías de la Información y las Comunicaciones  
Programa de Ingeniería en Informática  
Espacio académico: **Machine Learning Tools**  
Docente: **Crisman Martinez B.**  
Ciclo: **2026-02**

## 1. Descripción del proyecto

Este repositorio contiene el desarrollo del **Taller de Algoritmos de Machine Learning del Segundo Corte**.

El ejercicio parte de un caso en el que una empresa cuenta con información histórica de sus clientes, sus visitas al sitio web, inversión en publicidad y comportamiento de los usuarios. A partir de estos datos se aplican diferentes algoritmos de aprendizaje supervisado para resolver problemas de regresión y clasificación.

Los modelos utilizados son:

| Algoritmo | Tipo de problema | Aplicación |
|---|---|---|
| Regresión Lineal | Regresión | Predecir el valor mensual de ventas |
| Regresión Logística | Clasificación binaria | Determinar si un cliente realiza una compra |
| K-Nearest Neighbors (K-NN) | Clasificación | Clasificar clientes según su similitud |
| Árbol de Decisión | Clasificación | Clasificar clientes mediante reglas |

El objetivo no es solamente entrenar los modelos, sino también preparar correctamente los datos, evitar fuga de información, comparar diferentes configuraciones y analizar los resultados obtenidos.

## 2. Datos utilizados

El conjunto de datos utilizado en el ejercicio es `base_ML.csv`.

Cuenta con:

- **6.000 registros**
- **500 clientes**
- **12 periodos**
- Información sobre edad, visitas al sitio web, inversión en publicidad y comportamiento del usuario.
- Variables relacionadas con las compras y el valor mensual de ventas.

Las variables objetivo utilizadas son:

- `valor_mensual_ventas`: objetivo para el problema de regresión.
- `compra_actual`: objetivo para los problemas de clasificación.

Antes de entrenar los modelos se realizó una revisión de los datos para identificar valores faltantes, variables categóricas y posibles problemas de fuga de información.

### Preparación de los datos

Se identificaron **367 valores faltantes** en total:

- `cliente_edad`: 158
- `publicidad_inversion`: 209

Para el preprocesamiento se utilizaron:

- Imputación por mediana para las variables numéricas.
- `StandardScaler` para estandarizar las variables numéricas.
- `OneHotEncoder` para transformar las variables categóricas.
- División de los datos en **80 % para entrenamiento y 20 % para prueba**.
- División estratificada para los modelos de clasificación.

También se revisaron las variables que podían generar **data leakage**. Algunas variables relacionadas directamente con la compra actual no se utilizaron para predecir `compra_actual`. Para el comportamiento del visitante se creó `tipo_visitante_lag`, utilizando la información del periodo anterior.

## 3. Metodología

El desarrollo de los modelos siguió un flujo común:

**Carga de datos → exploración → preparación → selección de variables → preprocesamiento → división train/test → entrenamiento → validación y ajuste → predicción → evaluación**

Para los modelos de clasificación se utilizaron métricas como:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Curva Precision-Recall

Para la regresión se utilizaron:

- MAE
- MSE
- RMSE
- R²

Además, se utilizó **validación cruzada y GridSearchCV** para analizar diferentes configuraciones de K-NN y del Árbol de Decisión.

## 4. Modelos desarrollados

### Regresión Lineal

Se utilizó para predecir `valor_mensual_ventas`, una variable numérica continua.

Resultados obtenidos en el conjunto de prueba:

| Métrica | Resultado |
|---|---:|
| MAE | 179.304 |
| MSE | 59.445.055.154 |
| RMSE | 243.814 |
| R² | 0,237 |

El R² indica que el modelo explica aproximadamente el 23,7 % de la variabilidad observada en las ventas.

### Regresión Logística

Se utilizó para determinar si un cliente realiza una compra (`1`) o no realiza una compra (`0`).

Con el umbral de clasificación de 0,5 se obtuvieron aproximadamente:

| Métrica | Resultado |
|---|---:|
| Accuracy | 0,637 |
| Precision | 0,649 |
| Recall | 0,844 |
| F1-score | 0,734 |
| ROC-AUC | ≈ 0,659 |

También se probaron diferentes valores de threshold. Entre los valores evaluados, el umbral de **0,4** obtuvo el mejor F1-score, con aproximadamente **0,759**.

### K-Nearest Neighbors (K-NN)

K-NN clasifica una nueva observación según la similitud que presenta con otras observaciones del conjunto de datos.

Para seleccionar la configuración se utilizó `GridSearchCV` con validación cruzada. La configuración seleccionada fue:

- `n_neighbors = 21`
- `weights = 'uniform'`

En la comparación final, el modelo obtuvo aproximadamente:

| Métrica | Resultado |
|---|---:|
| Accuracy | 0,628 |
| Precision | 0,654 |
| Recall | 0,791 |
| F1-score | 0,716 |
| ROC-AUC | 0,635 |

### Árbol de Decisión

El Árbol de Decisión permite clasificar los clientes mediante una serie de reglas basadas en sus características.

El modelo inicial presentó señales de **sobreajuste**, por lo que se utilizó `GridSearchCV` para controlar su complejidad.

La configuración seleccionada fue:

- `max_depth = 5`
- `min_samples_leaf = 5`
- `min_samples_split = 2`

En la ejecución optimizada se obtuvo:

| Métrica | Resultado |
|---|---:|
| Accuracy | 0,645 |
| Precision | 0,655 |
| Recall | 0,850 |
| F1-score | 0,740 |
| ROC-AUC | 0,663 |

El ajuste de la complejidad permitió obtener un comportamiento más estable sobre los datos de prueba que el árbol inicial.

## 5. Comparación

Los resultados de los modelos de clasificación muestran que no existe un único modelo que sea el mejor para cualquier situación. La elección depende del objetivo y de la métrica que se considere más importante.

De forma general:

- **Regresión Lineal**: adecuada para predecir valores continuos como las ventas.
- **Regresión Logística**: adecuada para determinar compra o no compra y trabajar con probabilidades.
- **K-NN**: útil cuando la similitud entre clientes es un criterio importante.
- **Árbol de Decisión**: útil cuando se necesitan reglas de decisión fáciles de interpretar.

También se observó la importancia de utilizar diferentes métricas. Por ejemplo, Precision permite analizar los falsos positivos, mientras que Recall permite observar qué proporción de los casos positivos reales fue detectada.

## 6. Estructura del repositorio

La estructura debe corresponder a los archivos que realmente se publiquen en GitHub. Una organización sencilla puede ser:

```text
/
├── README.md
├── base_ML.csv
├── notebooks/
├── docs/
└── requirements.txt
```

## 7. Requisitos

Para ejecutar el proyecto se requiere Python 3.10 o superior y las principales librerías utilizadas son:

- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- jupyter

Instalación:

```bash
pip install -r requirements.txt
```

## 8. Conclusiones

El desarrollo del ejercicio permitió comprobar que la preparación de los datos es una parte fundamental del proceso de Machine Learning. La imputación de valores faltantes, la transformación de variables categóricas, el escalamiento y la revisión de posibles fugas de información influyen directamente en la calidad de los modelos.

También se observó que cada algoritmo responde a una necesidad diferente y que la evaluación no debe depender de una sola métrica. La validación cruzada y `GridSearchCV` permitieron comparar configuraciones y controlar aspectos como la elección de K en K-NN y la complejidad del Árbol de Decisión.

Finalmente, el análisis del Árbol de Decisión permitió identificar un caso de sobreajuste y comprobar cómo el ajuste de hiperparámetros puede mejorar la generalización del modelo frente a datos nuevos.

## 9. Referencias

Géron, A. (2022). *Hands-on machine learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly Media.

James, G., Witten, D., Hastie, T., Tibshirani, R., & Taylor, J. (2023). *An introduction to statistical learning: With applications in Python*. Springer.

Martinez Barrera, C. (2026). *Aula Virtual USTA. Machine Learning Tools*. Universidad Santo Tomás.

McKinney, W. (2022). *Python for data analysis: Data wrangling with Pandas, NumPy, and Jupyter* (3rd ed.). O'Reilly Media.

Müller, A. C., & Guido, S. (2016). *Introduction to machine learning with Python: A guide for data scientists*. O'Reilly Media.

Pedregosa, F., et al. (2011). Scikit-learn: Machine learning in Python. *Journal of Machine Learning Research, 12*, 2825–2830.
