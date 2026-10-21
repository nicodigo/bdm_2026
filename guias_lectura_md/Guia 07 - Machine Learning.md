# GUÍA DE LECTURA VII: Machine Learning

## 1.  ¿Qué se entiende por Machine Learning?

El aprendizaje automático (_Machine Learning_) es una disciplina de la inteligencia artificial enfocada en construir programas informáticos capaces de mejorar automáticamente su rendimiento en una tarea determinada a medida que acumulan experiencia o datos, sin necesidad de ser programados mediante reglas explícitas.

Un problema de aprendizaje está bien planteado si se definen tres componentes fundamentales: una **tarea _T_**, una medidad de **desempeño _P_** y una **experiencia de entrenamiento _E_**. Un sistema aprende si su rendimiento en la tarea _T_, evaluado mediante _P_, mejora progresivamente con al experiencia _E_.

En lugar de aplicar la programación tradicional, donde un profesional escribe manualmente un algoritmo sobre los datos para calcular un resultado, el aprendizaje automático invierte este flujo: ingresa datos y respuestas de ejemplo para que el algoritmo infiera una función matemática _f(x)_. Durante esta búsqueda en el _espacio de hipótesis_[^1], el algoritmo construye un modelo predictivo capaz de generalizar, es decir, de clasificar o predecir correctamente instancias nuevas o no vistas previamente.

## 2.  ¿Cuál es la diferencia entre aprendizaje supervisado y no supervisado?

La diferen central entre el aprendizaje **supervisado** y el **no supervisado** radica en la presencia o ausencia de una variable objetivo o etiqueta en los datos. En el supervisado el algoritmo aprende a predecir una respuesta conocida, mientras que en el no supervisado busca descubrir patrones o estructura ocultas sin una guía previa.

### Aprendizaje Supervisado
El algoritmo se entrena con un conjunto de datos formados por pares _(x,y)_, donde _x_ es el **vector de características** (variables explicativas) e _y_ es la **variable objetivo** o clase conocida. La meta es estimar una función _f(x)_ tal que _y = f(x)_, con el fin de **generalizar** y predecir la etiqueta _y'_ correcta en las nuevas instancias de _x'_ no vistas.

### Aprendizaje no supervisado
El conjunto de entrenamiento contiene únicamente el vector de características _x_, sin inguna variable objetivo _y_ ni respuestas predefinidas. El algoritmo analiza los datos para encontrar patrones, regularidades o agrupamientos implícitos. La técnica más común es el **agrupamiento** (_clustering_), donde el modelo decide de forma autónoma cómo dividir las instancias en grupos (_clusters_) en función de su similitud.

## 3.  ¿Cuáles son los métodos de aprendizaje supervisado?

Los métodos de aprendizaje supervisado son algoritmos diseñados para construir modelos predictivos a partir de un conjunto de datos etiquetados (pares de entrada-salida). Se dividen principalmente en tareas de **clasificación** para predecir variables de salida discretas o categóricas, y **regresión** para predecir salidas numéricas continuas.

### Árboles de decisión
Construyen de forma recursiva una estructura jerárquica en forma de árbol mediante una búsqueda descendente y codiciosa[^2].
- **ID3**: Selecciona las varibles nodo por nodo buscando maximizar la **ganancia de información** basada en la **entropía**.
- **C4.5**: Evolución de ID3 que soluciona el sesgo hacia atributos con muchos valores utilizando el **ratio de ganancia**, permite incorporar atributos continuos y aplica pode de árboles (_pruning_) para controlar el sobreajuste.
- **CART**: Genera árboles estrictamente binarios. A diferencia de los anteriores, puede utilizarse tanto para clasificación como para regresión.

### Vecinos más cercanos
Método basado en instancias (o _lazy learning_) que clasifica un dato nuevo buscando las _k_ instancias de entrenamiento más cercanas en el espacio de características según una función de distancia (como la _distancia euclidiana_).

### Redes neuronales artificiales
Modelos compuestos por capas de nodos (neuronas) interconectadas. Ajustan los pesos de sus conexiones de forma iterativa mediante algoritmos como la **propagación hacia atrás** con descenso por el gradiente para minimizar el error de predicción.

### Modelos de regresión
Incluyen la regresión lineal simple y múltiple, que estiman mediantes el método de mínimos cuadrados una relación funcional continua para predecir una variable respuesta cuantitativa.

## 4.  ¿Qué tareas se asocian a aprendizaje no supervisado?

Las tareas asociadas al aprendizaje no supervisado buscan descubrir la estructura interna, patrones ocultos o relaciones naturales en un conjunto de datos que carece de una **variable objetivo** o etiqueta predefinida. Sus principales tareas son el **agrupamiento**, la **asociación** y la **reducción de dimensionalidad**.

### Agrupamiento (_clustering_)
Consiste en segmentar un conjunto de datos en subgrupos o cúmulos (_clusters_) homogéneos de modo que los registros pertenecientes a un mismo grupo sean muy similares entre sí y muy diferentes de los registros de otros grupos. Los métodos centrales son el agrupamiento jerárquico, _k_-medias, y los mapas auto-organizados (_redes de Kohonen_).

### Asociación
Se enfoca en descubrir relaciones de afinidad o atributos que "van juntos" con frecuencia en grandes bases de datos. Produce reglas de decisión del estilo "Si _antecedente_, entonces _consecuente_", evaluadas mediante métricas de **soporte** y **confianza** con algoritmos _a priori_.

### Reducción de la dimensionalidad
Busca simplificar conjuntos de datos con un número elevado de atributos, comprimiendo la información en menos variables sin perder la estructura o variabilidad fundamental.

### Descripción
Identifica patrones generales, tendencias o anomalías para caracterizar la distribución global de los datos mediante herramientas estadísticas y de análisis exploratorio.

5.  Describa la diferencia de los diferentes conjuntos de muestras en un esquema de holdout.

En un esquema de **reserva o retención** (_holdout_), los datos disponibles se dividen aleatoriamente en subconjuntos independientes para evitar que el modelo memorice el ruido y para medir su verdadera capacidad de generalización. La partición clásica separa la muestra disponible en conjunto de entranamiento, conjunto de validación y conjunto de prueba, asignando a cada uno una función estricta durante el ciclo de vida del modelado.

Consiste en particionar los datos etiquetados mediante un muestreo aleatorio en subconjuntos distintos:

### Conjunto de entrenamiento
Es la fracción principal de los datos (entre el 50% y el 70%). Se utiliza de forma directo por el algoritmo para inducir la estructura del modelo, aprender las reglas de decisión o ajustar sus parámetros internos (por ej. construir las divisiones iniciales de un árbol de decisión).

### Conjunto de validación
Representa una porción intermedia de la muestra (entre el 15% y el 30%). Se emplea durante la **selección de modelos**, el ajuste de **hiperparámetros** y el control de **sobreajuste** mediante técnicas de **poda de árboles**. Sirve para realizar evaluaciones intermedias y seleccionar la variante del modelo que mejor generaliza antes de finalizar el entrenamiento.

### Conjunto de prueba
Es un subconjunto completamente independiente que se mantiene aislado desde el principio (habitualmente entre el 15% y el 20%). Sus etiquetas permanecen ocultas para el algoritmo durante toda la fase de construcción y ajuste. Se utiliza al final para obtener una estimación objetiva y rigurosa del desempeño del modelo ante datos desconocidos del mundo real.

### Ejemplo
En la construcción de un clasificador de riesgo crediticio bancario sobre 100.000 clientes:
- *Training set* (50.000 registros): El algoritmo analiza los ingresos y deudas de estos clientes para crear las reglas del árbol.
- *Validation set* (30.000 registros): Se usa para recortar las ramas del árbol que memorizaron casos raros y probar cuál es la altura óptima del árbol.
- *Test set* (20.000 registros): Se evalúa el árbol final sobre estos clientes nunca vistos para estimar cuántos errores cometerá el banco al evaluar nuevas solicitudes de crédito el mes que viene.

## 6.  ¿Qué entiende por overfitting? ¿Qué ocurre cuando tengo un valor de exactitud muy alto en mis datos de entrenamiento?

El **sobreajuste** (_overfitting_) es un fenómeno en el cual un modelo de aprendizaje automático aprende tan minusiosamente los detalles, el ruido y las particularidades del **conjunto de entrenamiento** que pierde la capacidad de **generalizar** correctamente ante datos nuevos no vistos. Cuando se obtiene un valor de **exactitud** (_accuracy_) muy alto o perfecto en los datos de entrenamiento, se trata de una estimación ilusoria que suele esconder la memorización de la muestra, provocando que el rendimiento real del modelo en el **conjunto de prueba** se degrade significativamente.

Formalmente, una hipótesis _h_ sobreajusta los datos de entrenamiento si existe una hipótesis alternativa _h'_ tal que _h_ comete menos errores que _h'_ sobre las muestras de entrenamiento, pero _h'_ comete menos errors que _h_ al ser evaluada sobre la distribución global de instancias.

El sobreajuste ocurre cuando la **complejidad del modelo** aumenta en exceso. A medida que un algoritmo (como un árbol de decisión muy profundo o una red nauronal con excesivas iteraciones) añade reglas o parámetros, reduce progresivamente su **sesgo** sobre la muestra. Sin embargo, al ajustar de forma forzada las anomalías, el ruido o las coincidencias estocásticas de los datos, el modelo adquiere una alta **varianza**.

Cuando medimos el rendimiento únicamente sobre el conjunto de entrenamiento, la métrica resultante se denomina **error de resustitución**. Este indicador ofrece una estimación engañosamente optimista porque el modelo es evaluado sobre la misma información que utilizó para ajustar sus parámetros. Una exactitud del 100% en entrenamiento no refleja un modelo perfecto, sino un algoritmo que memorizó los datos sin abstraer la regla subyacente.

### Ejemplo
Un árbol de decisión generado con el algoritmo ID3 para clasificar clientes con alto poder adquisitivo puede crear una regla específica en una hoja que indique: _"Si el cliente se llama 'David' y vive en la calle X, entonces tiene ingresos altos"_. Esta regla logra una exactitud del 100% sobre el conjunto de entrenamiento al clasificar a la persona 'David' de la muestra, pero fallará al evaluar a nuevos clientes en el mundo real.

## 7.  ¿Qué significa que el conocimiento es generalizable?

Que el conocimiento extraído por un modelo sea **generalizable** significa que tiene la capicidad de realizar predicciones o clasificaciones correctas sobre datos nuevos no vistos previamente. Indica que el sistema aprendió la estructura lógica subyacente del problema y no simplemente una memorización de los datos de laboratorio.

En el contexto del aprendizaje automático (_machine learning_), los datos se dividen para que el algoritmo aprenda patrones a partir de un **conjunto de entrenamiento** y posteriormente aplique dicho modelo a instancias desconocidas en el **conjunto de prueba** o en el entorno real.

La **hipótesis del aprendizaje inductivo** establece que cualquier hipótesis que aproxime bien la función objetivo sobre un conjunto de entrenamiento suficientemente grande también aproximará dicha función sobre instancias no observadas. Para lograr la **capacidad de generalización**, el algoritmo debe abstraer una **regla de decizión** o función predictiva general, evitando ajustar su estructura al ruido o a las particularidades de la muestra recibida. Si el modelo no logra generalizar, los patrones descubiertos resultan inútiles en la práctica porque carecen de validesz y certidumbre frente a nuevos datos.

## 8.  ¿Cómo se asocia el trade-off de sesgo y varianza con overfitting y underfitting?

El intercambio entre sesgo y varianza (_bias-variance trade-off_) es un dilema matemático fundamental que describe cómo la complejidad de modelo impacta en sus errores. Un modelo demasiado simple presenta un alto sesgo provocando subajuste, mientras que un modelo demasiado complejo presenta una alta varianza provocando sobreajuste[^3].

El error total de predicción de un modelo (por ej. medido a través del error cuadrático medio - MSE) se compone de dos elementos principales vinculados a la complejidad del modelo.

### Sesgo
Es la diferencia entre la predicción esperada del modelo y el valor real. Ocurre cuando el algoritmo asume supuestos rígidos y simplifica en exceso la estructura subyacente de los datos. Un alto sesgo conduce al **subajuste**, donde el modelo no logra capturar los patrones ni en los datos de entrenamiento ni en los de prueba.

### Varianza
Mide cuánto cambian las predicciones del modelo si se entrena con un conjunto de datos diferente. Un modelo con **alta** varianza es extremadamente flexible y sensible al ruido o particularidades de la muestra de entrenamiento, lo que causa **sobreajuste**: obtiene un error mínimo en entrenamiento pero un error elevado en prueba.

### Dilema
El _trade-off_ o dilema consiste en que intentar minimizar el sesgo incrementando la complejidad del modelo aumenta inevitablemente la varianza, y viceversa. El objetivo es encontrar el nivel óptimo de complejidad donde la suma de la varianza y el sesgo al cuadrado alcance su punto mínimo sobre el conjunto de prueba.[^4]
$MSE = \text{varianza} + \text{sesgo}^2$

> Bibliografía sugerida:
> - [Mitchell, T. M. (1997). Machine learning. 1997. McGraw Hill, 45(37), 870-877. **Cap 1**.
> - Larose, D. T., & Larose, C. D. (2014). *Discovering knowledge in data: an introduction to data mining* (Vol. 4). John Wiley & Sons.

[^1]: Buscar qué es exactamente el espacio de hipótesis
[^2]: Refrescar el concepto de greedy search
[^3]: Por qué una alta varianza provoca sobreajuste? La varianza mide el sobreajuste entiendo, no lo provoca, si hay alta varianza es signo de sobre ajuste?
[^4]: Si se tiene una varianza alta, pero un conjunto de datos de entrenamiento de calidad excelente, no sería bueno que haya sobreajuste?
