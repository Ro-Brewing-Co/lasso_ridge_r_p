# Modelado Predictivo Avanzado con Datos Abiertos de Colombia

**Por:** Robinson Alexander Otálvaro  
**Curso:** Análisis de datos avanzado grupo 2  
**Código:** [Enlace a Colab](#)

---

## 1. Introducción

El análisis tiene como objetivo desarrollar un modelo predictivo para estimar el número de accidentes de tránsito en función de características clave relacionadas con los accidentes. Este análisis es crítico para comprender los factores que contribuyen a los accidentes y proporcionar información para la toma de decisiones orientadas a la prevención.

---

## 2. Justificación de las Variables Seleccionadas

- **territorial (Localización geográfica):** Representa la ubicación administrativa donde ocurrieron los accidentes. Es relevante porque los factores locales, como la infraestructura vial, las regulaciones y el clima, pueden influir en la ocurrencia de accidentes.
- **clase_accidente (Tipo de accidente):** Clasifica el tipo de accidente (e.g., colisión, atropello, vuelco). Proporciona información sobre las causas y circunstancias específicas de los accidentes.
- **n_heridos (Número de heridos):** Indica el impacto del accidente en términos de personas lesionadas. Es un proxy del grado de severidad de los accidentes y tiene una correlación directa con el número de eventos.
- **n_muertos (Número de muertos):** Indica el impacto más severo de un accidente. Su inclusión es crucial para reflejar la gravedad de los eventos y su relación con otros factores.
- **n_accidentes (Variable objetivo):** Representa el total de accidentes, definido como la suma de heridos y muertos. Es la variable que se busca predecir, ya que proporciona una métrica consolidada del impacto de los accidentes en una región y situación específica.

---

## 3. Proceso Realizado

### 3.1 Carga del Archivo

- Se cargó el archivo en un DataFrame de pandas.
- Se seleccionaron únicamente las columnas relevantes (`territorial`, `clase_accidente`, `n_heridos`, `n_muertos`).
- Se creó la columna objetivo `n_accidentes` como la suma de `n_heridos` y `n_muertos`.

### 3.2 Limpieza de Datos

- **Manejo de valores nulos.**  
- **Conversión de tipos de datos.**  
- **Eliminación de outliers.**

### 3.3 Escalamiento de los Datos

- **Descripción:** Dado que las variables numéricas tenían escalas diferentes, se aplicó escalamiento estándar (StandardScaler) para normalizarlas.
- **Pasos:** Se ajustó el escalador a los datos de entrenamiento y se aplicó tanto a los datos de entrenamiento como a los datos de prueba. Esto asegura que todas las variables numéricas contribuyan de manera equitativa al modelo, sin que las escalas más grandes dominen.

---

## 4. Modelado Predictivo

- **Modelos implementados:** Se evaluaron modelos como Regresión Polinómica, Ridge y Lasso.  
- **Optimización de hiperparámetros:** Se realizó una búsqueda en cuadrícula (GridSearchCV) para encontrar los mejores valores del hiperparámetro `α` en los modelos Ridge y Lasso.

---

## 5. División de los Datos

### Descripción

Para garantizar una evaluación justa y evitar el sobreajuste, los datos se dividieron en dos conjuntos:  
1. **Conjunto de entrenamiento:** Utilizado para ajustar los modelos.  
2. **Conjunto de prueba:** Utilizado para evaluar el desempeño de los modelos.  

### Pasos

1. Separación de la variable objetivo (`n_accidentes`) y las variables predictoras (`territorial`, `clase_accidente`, `n_heridos`, `n_muertos`).  
2. División del dataset en 80% para entrenamiento y 20% para prueba.  
3. Escalamiento aplicado únicamente al conjunto de entrenamiento y luego transformado al conjunto de prueba.

---

## 6. Implementación de los Modelos

### 6.1 Regresión Polinómica

- **Descripción:** Se implementó un modelo de Regresión Polinómica para capturar relaciones no lineales entre las características y la variable objetivo.
- **Técnica:** Se generaron términos polinómicos de grado 2 y se ajustó un modelo de regresión lineal.

### 6.2 Ridge

- **Descripción:** La regresión Ridge utiliza regularización L2 para evitar sobreajuste al reducir la magnitud de los coeficientes.
- **Optimización:** GridSearchCV para optimizar `α` con valores evaluados: `[0.1, 1.0, 10.0]`.

### 6.3 Lasso

- **Descripción:** Aplica regularización L1 para eliminar variables irrelevantes al reducir sus coeficientes a cero.
- **Optimización:** GridSearchCV para optimizar `α` con valores evaluados: `[0.01, 0.1, 1.0]`.

---

## 7. Interpretación de los Resultados

### 7.1 Ridge (`α = 0.1`)

- **Impacto:** Este valor indica una ligera regularización L2, ideal para escenarios donde todas las características tienen cierta relevancia.
- **Resultados:** Mantiene todos los coeficientes, reduciéndolos ligeramente para evitar el sobreajuste.

### 7.2 Lasso (`α = 0.01`)

- **Impacto:** Este valor regula la intensidad de la regularización L1, seleccionando solo las características más importantes y eliminando las irrelevantes.
- **Resultados:** Modelo más simple e interpretable, sacrificando precisión para mejorar la generalización.

---

## 8. Validación Cruzada y Evaluación

- **Validación cruzada:** 5 pliegues.
- **Métricas utilizadas:**  
  - **RMSE (Root Mean Squared Error):** Mide el error promedio en las mismas unidades de la variable objetivo.  
  - **R² (Coeficiente de Determinación):** Proporción de la variabilidad explicada por las características.

---

## 9. Resultados y Evaluación

| Modelo              | RMSE     | R²     | Observaciones                                          |
|---------------------|----------|--------|-------------------------------------------------------|
| Regresión Polinómica| 0.0000   | 1.0000 | Ajuste perfecto a los datos; sobreajuste severo.      |
| Ridge               | 0.0018   | 1.0000 | Ajuste casi perfecto; controla sobreajuste.           |
| Lasso               | 0.1009   | 0.9897 | Modelo interpretable; elimina variables irrelevantes. |

---

## 10. Conclusiones y Recomendaciones

- **Ridge:** Ideal para mantener la información completa; control efectivo del sobreajuste.
- **Lasso:** Excelente para simplificar modelos e identificar variables clave.
- **Recomendaciones:**  
  - Optimizar hiperparámetros mediante búsqueda avanzada (e.g., búsqueda bayesiana).  
  - Incluir predictores adicionales como condiciones climáticas o días festivos.  
  - Aumentar el tamaño del dataset y utilizar validación cruzada con más pliegues.  
  - Explorar enfoques ensemble como Random Forest o Gradient Boosting.

En conjunto, Ridge y Lasso son herramientas complementarias que equilibran precisión, simplicidad y generalización para el análisis de accidentes de tránsito.
