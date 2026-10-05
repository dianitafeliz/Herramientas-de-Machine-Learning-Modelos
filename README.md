# Machine Learning Tools — Taller Segundo Corte

**Universidad Santo Tomás**
Facultad de Ingeniería en Tecnologías de la Información y las Comunicaciones
Programa de Ingeniería en Informática
Espacio Académico: Machine Learning Tools (74273 — Plan 4, Nivel IX)
Docente: Crisman Martinez B.
Ciclo: 2026-02

---

## 1. Descripción del proyecto

Este proyecto desarrolla el **Anexo N.3 — Taller de Algoritmos de Machine Learning** del Segundo Corte. Una empresa dispone de datos históricos de clientes, compras, visitas al sitio web, inversión publicitaria, productos y comportamiento de usuarios, y requiere modelos capaces de:

- Predecir el valor generado por un cliente (**regresión**).
- Determinar si un cliente realizará o no una compra (**clasificación binaria**).
- Clasificar nuevos clientes según su similitud con clientes históricos (**K-NN**).
- Clasificar clientes mediante reglas obtenidas de sus características (**Árbol de Decisión**).

Para ello se investigan, implementan y comparan cuatro algoritmos de aprendizaje supervisado:

| Algoritmo | Tipo | Uso en el caso |
|---|---|---|
| Regresión Lineal | Regresión | Predecir el valor generado por el cliente |
| Regresión Logística | Clasificación | Determinar compra / no compra |
| K-Nearest Neighbors (K-NN) | Clasificación | Clasificar clientes por similitud |
| Árbol de Decisión | Clasificación | Clasificar clientes mediante reglas |

## 2. Dataset

**Nombre:** `datos_clientes_ecommerce.csv`
**Origen:** Dataset sintético, generado con `generar_dataset.py` (numpy/pandas), diseñado a la medida del caso práctico del Anexo N.3 para cubrir todas las variables solicitadas: clientes, compras, visitas al sitio web, inversión publicitaria, productos y comportamiento de usuario.
**Registros:** ~4,100 filas (300 clientes únicos × 12 meses, formato largo por categoría de producto y canal)
**Periodo cubierto:** octubre 2025 a septiembre 2026 (últimos 12 meses), con estacionalidad (más ventas en nov-dic)
**Moneda:** Pesos colombianos (COP)
**Semilla aleatoria:** 42 (reproducible)
**Variable objetivo (clasificación):** `compra_actual` (0/1 — compró ese mes o no)
**Variable objetivo (regresión):** `valor_mensual_ventas` (COP, entre $100.000 y $3.000.000 si hubo compra, 0 si no)
**Diccionario de datos completo:** ver `docs/diccionario_datos.md`

| Categoría del enunciado | Columna |
|---|---|
| a. Clientes | `cliente_id`, `cliente_edad` |
| b. Compras | `compras_categoria` |
| c. Visitas al sitio web | `web_visitas_mes` |
| d. Inversión publicitaria | `publicidad_inversion` |
| e. Productos | `producto_categoria` |
| f. Comportamiento de usuarios | `comportamiento_usuario` (Web / Aplicación / Ambos — siempre poblado) |
| — | `canal_compra` (canal de cada compra específica; `"Sin compra"` si no aplica) |
| Dimensión temporal | `periodo` (YYYY-MM) |
| — | `valor_mensual_ventas` (objetivo regresión) |
| — | `compra_actual` (objetivo clasificación) |

El dataset está en **formato largo con dimensión temporal**: un cliente puede aparecer en varias filas el mismo mes si compró en distintas categorías, o incluso en la **misma categoría por ambos canales por separado**. Para modelar a nivel cliente-mes, agrupar por (`cliente_id`, `periodo`) primero (ver `docs/diccionario_datos.md`).

Solo hay valores faltantes reales en `cliente_edad` (3%) y `publicidad_inversion` (4%), a propósito para el ejercicio de imputación; el resto de columnas relacionadas con la compra usan `"Sin compra"` como categoría explícita en vez de nulos.
- **Outliers** en `publicidad_inversion` (20 registros), para justificar el escalado robusto antes de algoritmos geométricos como K-NN.

## 3. Estructura del proyecto

```
├── README.md                  # Este archivo
├── data/
│   └── datos_clientes_ecommerce.csv
├── docs/
│   ├── diccionario_datos.md     # Diccionario de datos
│   └── generar_dataset.py       # Script de generación del dataset
├── notebooks/
│   ├── 01_eda.ipynb            # Análisis exploratorio de datos
│   ├── 02_regresion_lineal.ipynb
│   ├── 03_regresion_logistica.ipynb
│   ├── 04_knn.ipynb
│   ├── 05_arbol_decision.ipynb
│   └── 06_comparacion_modelos.ipynb
├── src/
│   ├── preprocessing.py        # Imputación, escalado, codificación
│   ├── models.py                # Entrenamiento de los 4 algoritmos
│   └── evaluation.py            # Métricas y validación cruzada
├── docs/
│   └── taller_ml_tools.docx     # Documento de entrega (Anexo N.4)
├── requirements.txt
└── video/
    └── url_video.txt            # URL del video (Anexo N.5)
```

## 4. Requisitos

- Python 3.10+
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

## 5. Metodología (pipeline general)

1. **Carga y exploración de datos** (EDA): tipos de variable, valores nulos, distribución de clases.
2. **Ingeniería de datos**: imputación de valores faltantes, codificación de variables categóricas, escalado de características.
3. **División de datos**: entrenamiento / prueba (train_test_split), estratificado según la variable objetivo.
4. **Entrenamiento de modelos**: Regresión Lineal, Regresión Logística, K-NN y Árbol de Decisión.
5. **Validación cruzada (K-Fold)** y ajuste de hiperparámetros con GridSearchCV.
6. **Evaluación**:
   - Regresión: MAE, MSE, RMSE, R².
   - Clasificación: Accuracy, Precision, Recall, F1-score, ROC-AUC, PR-Curve.
7. **Comparación de modelos** y selección del umbral de decisión óptimo según el impacto de falsos positivos y falsos negativos.
8. **Conclusiones** y recomendaciones para producción (monitoreo, robustez, control de cambios en los datos).

## 6. Resultados

*(Se completa una vez ejecutados los notebooks)*

| Modelo | Métrica principal | Resultado |
|---|---|---|
| Regresión Lineal | R² | — |
| Regresión Logística | ROC-AUC | — |
| K-NN | F1-score | — |
| Árbol de Decisión | F1-score | — |

## 7. Autor

- **Nombre del estudiante:** _[Completar]_
- **Código:** _[Completar]_
- **Programa:** Ingeniería en Informática
- **Universidad Santo Tomás**

## 8. Referencias (APA 7)

Martinez Barrera, C. (2026). *Aula Virtual USTA. Machine Learning Tools*. Universidad Santo Tomás. Consultado en julio de 2026.

Pedregosa, F., et al. (2011). Scikit-learn: Machine Learning in Python. *Journal of Machine Learning Research*, 12, 2825-2830.

Sakar, C., & Kastro, Y. (2018). *Online Shoppers Purchasing Intention Dataset*. UCI Machine Learning Repository.
