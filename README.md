[README.md](https://github.com/user-attachments/files/34913572/README.md)
# PROYECTO2_PARTE_2
Este conjunto de datos recopila información sobre comportamiento de compra e interacción en una plataforma de comercio electrónico (E-commerce) a nivel de cliente.
# Proyecto 2 - Parte 2: Regresión Lineal Múltiple

**Alumnos:** Ricardo Alexis Lagos Mora y Mariana González
**Universidad Provincial del Sudoeste**

## Descripción

Se entrenó un modelo de Regresión Lineal Múltiple sobre el dataset *Ecom Expense* (2362 transacciones de una plataforma de e-commerce).

- **Variable a predecir (Y):** `Total Spend` (gasto total)
- **Features (X):** `Age` (edad), `Monthly Income` (ingreso mensual) y `Transaction Time` (tiempo de transacción)
- **Partición:** 80 % entrenamiento (1889 muestras) / 20 % test (473 muestras), `random_state=42`

Notebook: `RICARDO_ALEXIS_LAGOS_MORA_MARIANA_GONZALEZ.ipynb`

## Resultados

| Métrica | Entrenamiento | Test |
|---|---|---|
| MSE | 6 363 042.19 | 6 191 305.96 |
| RMSE | 2 522.51 | 2 488.23 |
| R² | 0.2065 | 0.1272 |

Parámetros del modelo:

| Parámetro | Valor |
|---|---|
| Intercepto (b0) | 3025.8040 |
| Age | 9.5822 |
| Monthly Income | 0.1597 |
| Transaction Time | 0.3861 |

## Conclusiones

### 1. ¿Cuál fue el desempeño del modelo?

El desempeño fue **bajo**. El modelo explica solo el 20.7 % de la variabilidad del gasto total en entrenamiento y el 12.7 % en test. El RMSE ronda los 2 500, mientras que el gasto promedio del dataset es de aproximadamente 6 163 y su desvío estándar de 2 800. Es decir, el error típico de predicción es cercano al 40 % del gasto medio, y apenas mejora respecto de predecir siempre el promedio. El modelo captura una tendencia general, pero no sirve para hacer predicciones precisas de cada cliente.

### 2. ¿Qué variable fue la más influyente según los coeficientes?

A simple vista, `Age` tiene el coeficiente más grande (9.58), pero **los coeficientes no son directamente comparables** porque las variables están en escalas muy distintas (edad: 20–60, ingreso: 2 000–30 000, tiempo: 0–1 000). Para compararlas hay que ver cuánto cambia el gasto ante una variación de un desvío estándar de cada variable (coeficiente × desvío estándar):

| Variable | Coeficiente × desvío estándar | Efecto aprox. en Total Spend |
|---|---|---|
| Monthly Income | 0.1597 × 8 046.5 | ≈ 1 285 |
| Age | 9.5822 × 11.97 | ≈ 115 |
| Transaction Time | 0.3861 × 286.07 | ≈ 110 |

Por lo tanto, la variable más influyente es **`Monthly Income`**: a mayor ingreso mensual, mayor gasto total. Esto es coherente con lo esperable. `Age` y `Transaction Time` tienen un aporte mucho menor, casi marginal. (Para que la comparación fuera directa desde los coeficientes, convendría estandarizar las variables antes de entrenar.)

### 3. ¿Las features fueron buenas predictoras? ¿Se sugieren otras?

Solo **`Monthly Income`** aportó información relevante; `Age` y `Transaction Time` casi no ayudan. En conjunto, las tres features resultaron **malas predictoras** del gasto total, como lo muestra el R² bajo.

Se sugiere probar con:

- **`Items`** (cantidad de ítems comprados): es probable que tenga relación directa con el gasto total.
- **`Record`**: variable numérica disponible en el dataset que no se utilizó.
- **Variables categóricas** `Gender` y `City Tier`, que se descartaron al principio del análisis. Codificadas (por ejemplo con one-hot encoding) podrían aportar información, sobre todo `City Tier`, que puede reflejar diferencias de poder adquisitivo.
- Modelos no lineales (árboles, Random Forest) o transformaciones e interacciones entre variables, ya que la relación podría no ser lineal.

### 4. Comparativa entre entrenamiento y test

- **R²:** 0.2065 en entrenamiento contra 0.1272 en test. El modelo generaliza algo peor sobre datos nuevos, pero ambos valores son bajos.
- **RMSE:** 2 522.5 en entrenamiento contra 2 488.2 en test. El error en test es incluso ligeramente menor, lo que indica que **no hay sobreajuste (overfitting)**.
- La diferencia de R² puede deberse en parte a la variabilidad propia de la partición aleatoria (el conjunto de test es pequeño, 473 muestras).
- El problema principal es de **subajuste (underfitting)**: el modelo es demasiado simple o las variables elegidas no contienen suficiente información para explicar el gasto, y esto se refleja por igual en entrenamiento y test.
