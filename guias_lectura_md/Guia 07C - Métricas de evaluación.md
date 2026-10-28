# GUÍA DE LECTURA VII (c): Selección y evaluación de modelos

## 1.  ¿Qué es una matriz contingencia?

Una matriz de contingencia, conocida habitualmente como **matriz de confusión**, es una tabla de doble entrada que permite visualizar y cuantificar el desempeño de un modelo de aprendizaje supervisado comparando las clases reales u observadas contra las predicciones realizadas por el modelo.

En el contexto de la evaluación de clasificadores, la matriz organiza las observaciones del conjunto de prueba en filas y columnas. Para un problema de clasificación binaria (con clase positiva y negativa), la matriz de confusión genera cuatro cuadrantes fundamentales:

### Verdaderos positivos
Cantidad de instancias pertenecientes a la clase positiva que el modelo clasificó correctamente como positivas.

### Verdaderos negativos
Cantidad de instancias pertenecientes a la clase negativa que el modelo clasificó correctamente como negativas.

### Falsos positivos (Error Tipo I)
Cantidad de instancias realmente negativas que el modelo clasificó erróneamente como positivas (falsas alarmas).

### Falsos negativos (Error Tipo II)
Cantidad de instancias realmente positivas que el modelo clasificó erróneamente como negativas (casos omitidos).


## 2.  ¿Qué tipo de modelos se pueden evaluar con una matriz de contingencia?

La matriz de confusión se utiliza para evaluar modelos de aprendizaje supervizado de clasificación, donde la variable objetivo es de naturaleza discreta o categórica. No es aplicable a modelos de regresión numéricos ni a algoritmos de aprendizaje no supervisado.

Para que un algoritmo pueda ser evaluado mediante una matriz de confusión, se requiere obligatoriamente disponer de un conjunto de datos etiquetados con respuestas reales conocidas (_ground truth_) y predicciones categóricas emitidas por el modelo. Los tipos de modelos evaluables se dividen según la cantidad de clases:
- Clasificadores binarios: Modelos donde la variable objetivo responde a dos categorías mutuamente excluyentes (ej. Si/No, Veradero/Falso). Generan una matriz básica de _2x2

### Algoritmos de aprendizaje compatibles
- Árboles de decisión
- Clasificadores Bayesianos
- Redes neuronales artificiales
- Vecinos más cercanos y clasificadores basados en reglas.

### Algoritmos no evaluables con matriz de confusión
- Modelos de regresión (variable objetivo continua)
- Modelos de agrupamiento (no supervisado)


## 3.  ¿Qué medidas puedo obtener?

A partir de una matriz de confusión se pueden calcular diversas medidas de desempeño para evaluar cuantitativamente la calidad de un clasificador. Tomando como base los cuatro valores de la matriz (TP, TN, FP, FN), las métricas se estructura en tres grupos:

### Rendimiento Global
- Exactiud: Porcentaje total de instancias clasificadas correctamentes sobre el total de casos.

$$ \text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN} $$

- Tasa de error: Porcentaje total de instancias clasificadas de forma errónea.

$$ \text{Error Rate} = \frac{FP + FN}{TP + TN + FP + FN} = 1 - \text{Accuracy} $$

### Medidas por Clase (clave ante desbalance de datos)
- Sensibilidad o exhaustividad: Proporción de casos positivos reales que el modelo identificó correctamente.

$$ \text{Sensibilidad} = \frac{TP}{TP + FN} $$

- Especificidad: Proporción de casos negativos reales identificados correctamente.

$$ \text{Especificidad} = \frac{TN}{TN + FP} $$

- Precisión: Proporción de instancias calificadas como positivas que resultaron ser realmente correctas.

$$ \text{Precision} = \frac{TP}{TP + FP} $$

### Medidas Compuestas y Avanzadas
#### Puntaje F o F1-Score
Media armónica entre la precisión y la sensibilidad. Permite ponderar la importancia relativa de ambas métricas mediante el parámetro $\beta$ (si $\beta = 1$, reciben la misma ponderación).

$$ F1 = \frac{2 \times \text{Precision} \times \text{Sensibilidad}}{\text{Precision} + \text{Sensibilidad}} $$

#### Coeficiente Kappa de Cohen
Mide la concordancia entre la clase real y la predicha, descontando la probabilidad de que la coincidencia ocurra por puro azar _Pr(e)_.

$$ K = \frac{\text{Accuracy} - \text{Pr}(e)}{1 - \text{Pr}(e)} $$

#### Curva ROC y AUC
Herramienta gráfica que representa la relación entre la tasa de verdaderos positivos (sensibilidad) y la tasa de falsos positivos (1 - especificidad) al variar el umbral de decisión. El área bajo la curva (AUC) cuantifica la presición global del modelo independiente del umbral.


## 4.  ¿Cómo puedo calcular la exactitud general de un modelo a partir de la información de la matriz de contingencia? ¿Qué ocurre cuando las clases están muy desbalanceadas?

Para calcular la exactitud general (_accuracy_) a partir de una matriz de confusión, se divide la suma de las predicciones correctas (**diagonal principal**) entre el número total de observaciones o tuplas del conjunto de prueba:

$$\text{Exactitud (Accuracy)} = \frac{TP + TN}{TP + TN + FP + FN}$$

Donde:

- $TP$ (Verdaderos Positivos): Casos positivos correctamente identificados.
- $TN$ (Verdaderos Negativos): Casos negativos correctamente identificados.
- $FP$ (Falsos Positivos) y $FN$ (Falsos Negativos): Errores cometidos por el modelo.

De forma equivalente, se puede calcular como el complemento de la tasa de error: $\text{Exactitud} = 1 - \text{Tasa de Error}$, donde la tasa de error es la suma de las equivocaciones sobre el total de casos ($\frac{FP + FN}{\text{Total}}$).


### Clases desbalanceadas

Cuando el conjunto de datos presenta un desbalance de clases significativo, la exactitud general resulta una métrica engañosa y poco representativa.

En estas situaciones suceden dos efectos críticos:

1. Falsa sensación de alto rendimiento: Un clasificador ingenuo o nulo que se limite a predecir siempre la clase mayoritaria alcanzará un porcentaje de exactitud numérico muy elevado (ej. 97% o 99%).
2. Falla en la detección de la clase de interés: A pesar de mostrar una exactitud alta, el modelo resulta inútil porque fallará en clasificar los casos de la clase rara, cometiendo un alto número de falsos negativos y ofreciendo una sensibilidad extremadamente baja.

Por estos motivos, ante escenarios desbalanceados, no debe utilizarse la exactitud como único criterio de evaluación, sino métricas alternativas como sensibilidad, especificidad, precisión, el puntaje F1 o la evaluación de la curva ROC (AUC).


## 5.  ¿Qué diferencias existen en las medidas de exactitud general y las tasas de falsos/verdaderos positivos o negativos? ¿Qué evalúan cada una de ellas?

La exactitud general mide la proporción global de aciertos del clasificador sobre el total de datos, evaluando el desempeño de forma agregada sin diferenciar el tipo de clase ni el tipo de error. En cambio las tasas de verdaderos/falsos positivos y negativos desglosan el comportamiento del modelo evaluando aspectos específicos.

### Exactitud general

#### Fórmula

$$\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN} = \frac{\text{Aciertos Totales}}{\text{Total de Casos}}$$

#### Qué evalúa
El porcentaje global de tuplas del conjunto de prueba que fueron correctamente clasificadas por el modelo

#### Alcance y limitación
Evalúa la efectividad promedio bajo la suposición de que todas las clases tienen la misma importancia y que todos los errores tienen el mismo costo. Se vuelve engañosa e inútil cuando existe un fuerte desbalance de clases.

### Tasa de Verdaderos Positivos (Sensibilidad)

#### Fórmula

$$\text{TPR} = \frac{TP}{P} = \frac{TP}{TP + FN}$$

#### Qué evalúa
La completitud o capacidad de detección sobre la clase de interés. Mide qué porcentaje de las instancias que realmente pertenecen a la clase positiva alcanzaron a ser identificadas por el clasificador.

### Tasa de Verdaderos Negativos (Especificidad)

#### Fórmula

$$\text{TNR} = \frac{TN}{N} = \frac{TN}{TN + FP}$$

#### Qué evalúa
La capacidad de descarte o reconocimiento de la clase negativa. Indica qué proporción de los casos que verdaderamenet no pertenecen a la clase de interés fueron reconocidos correctamente como tales.

### Tasa de Falsos Positivos (Especificidad)

#### Fórmula

$$\text{FPR} = \frac{FP}{N} = \frac{FP}{FP + TN}$$

#### Qué evalúa
La frecuencia de falsas alarmas o Error Tipo I. Mide la proporción de casos reales negativos que el modelo clasificó erróneamente como positivos.

### Tasa de Falsos Negativos (Sensibilidad)

#### Fórmula

$$\text{FNR} = \frac{FN}{P} = \frac{FN}{TP + FN}$$

#### Qué evalúa
La frecuencia de omisiones o Error Tipo II. Mide la proporción de casos reales positivos que el modelo no logró detectar y catalogó equivocadamento como negativos.


## 6.  ¿En qué situaciones utilizaría la curva ROC para evaluar modelos? ¿Qué mide la sensibilidad y la especificidad? ¿Cuáles son las ventajas y desventajas de utililzar la curva ROC?

La curva ROC es una herramienta visual y cuantitativa utilizada en el aprendizaje supervisado para evaluar y comparar la capacidad discriminativa de modelos de clasificación binaria. Representa el compromiso entre la tasa de verdaderos positivos (sensibilidad) en el eje vertical y la tasa de falsos positivos (1 - especificidad) en el eje horizontal a lo largo de todos los posibles umbrales de decisión.

### En qué situaciones se utiliza la curva ROC

- Evaluación de clasificadors probabilísticos o de puntuación: Cuando el modelo asigna una probabilidad o puntaje continua de pertenencia a una clase en lugar de una simple etiqueta rígida.

- Selección del umbral de corte óptimo: Permite analizar cómo varía el rendimiento del modelo al modificar el umbral probabilístico, ayudando al analista a seleccionar el punto de equilibrio que maximice los aciertos minimizando los errores según la necesidad del problema.

- Comparación de múltiples algoritmos de clasificación: Facilita la comparación entre distintos modelos midiendo el área bajo la curva.

- Escenarios con desbalance o costos de error asimétricos: Es adecuada en dominios donde la clase de interés es rara o donde el costo de cometer un falso negativo difiere significativamente del de un falso positivo (ej. detección de fraudes, fallas industriales, diagnóstico médico).


### Ventajas

- Evaluación independiente del umbral: Mide la calidad de global de ordenamiento del modelo sin restingirse a un único umbral arbitrario de clasificación.

- Invarianza parcial ante el desbalance de clases: Como la sensibilidad (TPR) evalúa únicamente la columna de positivos reales y la tasa de falsos positivos (FPR) evalúa la columna de negativos reales, la forma de la curva se mantiene estable aunque cambie la proporción global entre clases.

- Interpretación intuitiva mediante el AUC: El área bajo la curva (AUC) resume el desempeño en un único valor numérico: un AUC = 1.0 representa un modelo perfecto, mientras que un AUC = 0.5 equivale a adivinar al azar.

### Desventajas

- Restringida a clasificación binaria: Está diseñada de forma nativa para 2 clases. Para problemas multiclase (_m_ > 2) requiere descomponer el problema en múltiples evaluaciones binarias de tipo "_uno contra todos_", dificultando su interpretación.

- Muestra una visión sobreoptimista en eventos extremadamente raros: Si la clase positiva representa una fracción minúscula de los datos (ej. 0.01%), una tasa de falsos positivos numéricamente baja puede traducirse de todos modos en miles de falsas alarmas absolutas. En tales casos de desbalance extremo, las curvas de **Precisión-Recall** ofrecen un diagnóstico más preciso.

- Incompatible con clasificadores discretos puros: No puede trazarse directamente si el algoritmo solo produce una etiqueta categórica final sin un puntaje continuo o una probabilidad asociada.


## 7.  ¿Cómo se interpreta el AUC?

El AUC se una métrica escalar que cuantifica la capacidad discriminativa global de un modelo de clasificación binaria a lo largo de todos los posibles umbrales de decisión. Su valor oscila entre 0.5 (desempeño equivalente al azar) y 1.0 (clasificación perfecta).

### Interpretación probabilística
Desde un punto de vista estadístico, el AUC tiene una interpretación intuitiva y directa: representa la probabilidad de que el modelo le asigne un puntaje o probabilidad más alta a una instancia positiva elegida al azar que a una instancia negativa elegida al azar.
- Por ejemplo: Si un modelo de detección de fraudes obtiene un AUC = 0.85, significa que si tomamos al azar una transacción fraudulenta real (positiva) y una transacción legítima real (negativa), en el 85% de los casos el modelo asignará una probabilidad de fraude mayor a la transacción fraudulenta legítima.

### Escala Cuantitativa de Interpretación
Para evaluar la calidad de un clasificador según su valor de AUC, se utiliza habitualmente la siguiente escala de referencia:

- **AUC = 1.0**: Clasificador perfecto.
- 0.90 $\le$ AUC < 1.0: Clasificación Excelente / Sobresaliente.
- 0.80 $\le$ AUC < 0.90: Clasificación Buena.
- 0.70 $\le$ AUC < 0.80: Clasificación Regular / Aceptable.
- 0.60 $\le$ AUC < 0.70: Clasificación Pobre / Débil.
- **AUC = 0.50**: Equivalente al azar.
- AUC < 0.50: Indica que el modelo clasifica de forma inversa (predice peor que el azar). En la práctica basta con invertir las etiquetas predichas para transformarlo en un modelo con $\text{AUC} = 1 - \text{AUC}_{\text{original}}$.

### Ventajas de interpretar el rendimiento mediante AUC
- Métrica independiente del umbral: A diferencia de la exactitud (accuracy), que exige fijar un umbral rígido (ej. 0.5), el AUC evalúa la calidad del ordenamiento o ranking del modelo a través de todo el espectro de umbrales posibles.

- Invarianza ante la escala de probabilidad: Mide únicamente la capacidad de separación de clases. Si las probabilidades predichas se multiplican o transforman de forma monótona ascendente, el AUC se mantiene inalterado.

- Facilidad para la selección de modelos: Permite comparar múltiples algoritmos (ej. Árboles de decisión vs. Redes Neuronales) utilizando un único valor sintético representativo.


> Bibliografía sugerida:
> - Mitchell, T. M. (1997). Machine learning. 1997. McGraw Hill, 45(37), 870-877. **Cap 3**.
> - Larose, D. T., & Larose, C. D. (2014). *Discovering knowledge in  data: an introduction to data mining* (Vol. 4). John Wiley & Sons. **Cap 8**
