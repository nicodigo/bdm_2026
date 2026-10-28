# GUÍA DE LECTURA VII (b): Árboles de decisión


## 1.  ¿Qué es un árbol de decisión?

Un árbol de decisión es un método de **aprendizaje supervisado** utilizado para la clasificación y predicción de datos. Representa un conjunto de reglas ordenadas en una estructura jerárquica similar a un diagrama de flujo, que permite asignar una clase o categoría a una nueva observación siguiendo una secuencia de pruebas lógicas.

Estructuralmente, un árbol de decisón es un grafo acíclico dirigido (_DAG_) que se organiza jerárquicamente desde arriba hacia abajo. Se compone de tres elementos fundamentales:

### Nodo raíz
Se ubica en la parte superior del diagrama y representa el punto de partida del modelo, donde se evalúa el primer atributo para dividir el conjunto de datos.

### Nodos internos (o de decisión)
Evalúan atributos específicos de las observaciones. De cada uno surgen dos o más ramas que corresponden a los posibles resultados de dicha evaluación.

### Nodos hoja o terminales
Se encuentran al final de cada rama y contienen la predicción o clase final asignada, punto en el cual se detiene la evaluación.


Para clasificar un nuevo registro, el algoritmo evalúa sus características comenzando en el nodo raíz y desciende por las ramas correspondientes según los valores observados hasta alcanzar un nodo hoja. Matemáticamente, los árboles representan una disyunción de conjunciones de condiciones (expresiones lógicas compuestas) sobre los atributos. Para poder construirse, requieren un conjunto de datos etiquetados (_training set_) y una variable objetivo de tipo discreta o categórica.


## 2.  ¿Cuáles son los requerimientos para utilizar árboles de decisión?

Para utilizar árboles de decisión, el problema debe pertenecer al ámbito del aprendizaje supervisado, lo que exige disponer de un conjunto de datos de entrenamiento previamente etiquetado donde la variable objetivo sea de naturaleza discreta o categórica.

Los algoritmos de inducción de árboles de decisión requieren cumplir cuatro condiciones técnicas fundamentales sobre los datos de entrada:

### 1. Enfoque supervisado y datos etiquetados
El algoritmo necesita recibir un conjunto de entrenamiento compuesto por instancias compuestas por un **vector de características** _X_ y su correspondiente etiqueta o clase conocida _y_.

### 2. Variable objetivo discreta o categórica
La variable a predecir debe tener valores claramente delimitados y finitos (por ej. Si/No, Aprobado/Desaprobado, Riesgo Alto/Medio/Bajo). No se puede aplicar la inducción de clasificación de árboles directamente sobre una variable respuesta continua.

### 3. Muestra rica y representativa
El dataset de entrenamiento debe ser lo suficientemente variado como para incluir una amplia muestra representativa del problema. Dado que los árboles aprenden mediante ejemplos, si faltan ejemplos de ciertros patrones o clases, la capacidad de generalización se verá comprometida.

### 4. Atributos descriptivos en pares atributo-valor
Los datos deben estar estructurados en atributos o predictores. Mientras que algoritmos clásicos como ID3 imponen como restricción adicional que todos los atributos predictores sean estrictamente categóricos, variantes más avanzadas como C4.5 o CART permiten operar con atributos continuos mediante técnicas de **discretización dinámica** fijando puntos de corte.


## 3.  ¿Cómo funciona ID3? ¿Cuáles son sus limitaciones?

El algoritmo ID3 es un método de aprendizaje supervisado desarrollado por Ross Quinlan para construir árboles de decisión de forma descendente (_top-down_). Su funcionamiento se basa en evaluar la entropía de los datos y seleccionar en cada nodo el atributo que maximiza la ganancia de información.

### Funcionamiento
ID3 opera mediante una búsqueda codiciosa descendente sobre el espacio de hipótesis sin realizar _backtracking_.

#### Cálculo de la entropía
Mide la impureza o desorden de un conjunto de datos _S_ respecto de la variable objetivo. Si el conjunto es puro, la entropía es 0, si está equilibrado al 50%, la entropía alcanza su máximo en 1.

#### Cálculo de la ganancia de información
Cuantifica la reducción esperada de la entropía al particionar los datos mediante un atributo _A_.

#### Selección del nodo y recursión
ID3 elige el atributo con mayor ganancia de información como nodo raíz, creando una rama por cada posible valor categórico del atributo. El procedimiento se repite recursivamente en los subconjuntos resultantes.

#### Criterios de parada
La inducción se detiene cuando todos los ejemplos de un nodo pertenecen a la misma clase, cuando no quedan atributos por evaluar o cuando un subconjunto queda vacío.

### Limitaciones

#### Sesgo hacia atributos con muchos valores
ID3 favorece sistemáticamente a predictores que tienen una gran cantidad de valores categóricos distintos (como un identificador único), ya que generan particiones puras o de entropía 0 aunque no posean poder predictivo real.

#### Incapacidad para manejar atributos continuos
Solo puede procesar atributos cuantitativos o continuos si fueron sometidos previmente a discretización. No realiza particiones dinámicas de datos continuos.

#### Sensibilidad a valores faltantes
En su versión estándar no contiene mecanismos nativos para tratar tuplas con valores faltantes en sus atributos.

#### Propenso al sobreajuste
Al construir el árbol hasta lograr hojas completamente puras sin aplicar ninguna fase de poda, el algoritmo memoriza el ruido del conjunto de entrenamiento y pierde la capacidad de generalización.

#### Riesgo de óptimos locales
Al no realizar bactracking, si selecciona una variable subóptima en la raíz, queda atrapado en esa ruta de búsqueda.


## 4.  ¿Por qué en el ejemplo de Play Tennis la variable Outlook accede a la raíz del árbol?

En el ejemplo de _Play Tennis_, la variable Outlook se ubica en el nodo raíz porque es el atributo que maximiza la ganancia de información entre todos los predictores evaluados. Esto significa que es la variable que más reduce la incertidumbre o entropía inicial sobre si se juega o no al tenis.

El algoritmo ID3 como se mencionó anteriormente, construye el árbol de decisión de forma descendente mediante búsqueda codiciosa. Para determinar qué atributo colocar en la raíz, evalúa estadísticamente cada variable predictora candidata (Outlook, Temperature, Humidity, Wind) respecto a la **variable objetivo** _Play Tennis_.

El procedimiento matemático consta de dos pasos:
1. Cálculo de la entropía inicial: Mide la impureza total del conjunto de datos _S_ (14 días: 9 positivos "Si" y 5 negativos "No"), obteniendo Entropía(_S_) = 0.940 bits.

2. Cálculo de la ganancia de información: Mide la reducción esperada en la entropía al dividir el conjunto _S_ según los valores de cada atributo. Los resultados numéricos para ID3:
    * Ganancia(_S_, Outlook) = 0.246 bits
    * Ganancia(_S_, Humidity) = 0.151 bits
    * Ganancia(_S_, Wind) = 0.048 bits
    * Ganancia(_S_, Temperature) = 0.029 bits


Como Outlook presenta el valor más elevado, es seleccionada como la variable con mayor poder discriminatorio. Al dividir por Outlook, la categoría "Nublado" produce un subconjunto completamente puro donde en el 100% de los casos la respuesta es "Si", transformándose directamente en un nodo hoja.


## 5.  ¿Qué ocurre en ID3 con las variables con muchas etiquetas?

En el algoritmo ID3, las variables que poseen un gran número de valores o categorías sufren un **sesgo de selección** que eleva de forma artificial su ganancia de información, haciendo que el algoritmo las elija prioritariamente para los nodos superiores aunque carezcan de valor predictivo real.


ID3 evalúa los atributos candidatos midiendo la reducción de entropía que produce cada partición. Cuando un predictor presenta una alta cantidad de etiquetas distintas (por ej. una fecha o una clave primaria como número de legajo), al dividir el conjunto de datos por ese atributo se crean múltiples subconjuntos muy pequeños y puros.

Al quedar muy pocos registros por cada valor, la entropía ponderada de esas subdivisiones cae a cero. Como la ganancia de información resulta de restar la entropía de las particiones a la entropía original del nodo, la ganancia obtenida alcanza su valor máximo. Esto provoca que ID3 seleccione prematuramente variables multivaluadas, construyendo árboles anchos y poco profundos que memorizan la muestra de entrenamiento pero son incapaces de generalizar ante observaciones desconocidas.


## 6.  ¿Cuáles son las mejoras que se incorporan en C4.5?

C4.5 es un algoritmo de aprendizaje supervisado desarrollado por Ross Quinlan como una evolución directa de ID3. Su objetivo principal es revolver las limitaciones prácticas de ID3 mediante la incorporación de cuatro mejoras técnicas:

### Selección por ratio de ganancia
ID3 sufría un sesgo que favorecía a predictores con muchas etiquetas. C4.5 corrige esto normalizando la ganancia de información al dividirla por la información de partición. Esta última mide la entropía del propio atributo en función de cuántas divisiones genera. Cuanto más fragmenta los datos, mayor es el denominador, penalizando las variables con excesivas categorías.

### Evaluación de atributos continuos
A diferencia de ID3 (limitado a variables categóricas), C4.5 procesa variables numéricas continuas de forma dinámica. Ordena los valores observados del atributo y prueba puntos de corte para crear condiciones booleanas del tipo _A_ <= _c_ frente _A_ > _c_, seleccionando el punto que maximiza el ratio de ganancia.

### Manejo de valores faltantes
Cuando una instancia de entrenamiento le falta un valor de un predictor, C4.5 no la descarta. Calcula las ganancias sobre las instancias conocidas y distribuye la instancia incompleta asignándole pesos fraccionarios o probabilidades proporcionales a lo largo de las distintas ramas del árbol.

### Poda de árboles y post-poda de reglas
Para controlar el sobreajuste generado al expandir el árbol hasta las hojas puras, C4.5 aplica poda pesimista. Ajusta la tasa de error observada con un factor de penalización para eliminar ramas superfluas o convertir el árbol en un conjunto simplificado de reglas de decisión del tipo "IF... THEN..."


## 7.  ¿Cuál es la relación entre overfitting y la altura del árbol? ¿Cómo se resuelve?

La altura o profundidad del árbol está directamente relacionada con el sobreajuste (_overfitting_), a mayor profundidad y complejidad del árbol, menor es el error sobre los datos de entrenamiento, pero mayor el sa probabilidad de que el árbol memorice el ruido y falle al genaralizar. Este problema se resuelve mediante técnicas de poda de árboles, ya sea prepoda deteniendo el cricimiento anticipadamente o postpoda recortando ramas de un árbol completamente desarrollado.


A medida que un árbol de decisión crece en altura y añade nodos de decisión, el modelo incrementa su capacidad para subdividir las muestras hasta obtener hojas puras. Durante este proceso, el error en el conjunto de entrenamiento disminuye de forma monótona hasta llegar a cero. Sin embargo el error en el conjunto de prueba o validación comienza a incrementarse a partir de cierta altura óptima. Esto sucede porque las ramas muy profundas responden a fluctuaciones estocásticas, excepciones o ruido de la muestra de entrenamiento en lugar de representar patrones verdaderos del dominio.


### Prepoda
Interrumpe la expansión del árbol durante su construcción cuando la ganancia de información o una prueba de significación estadística cae por debajo de un umbral preestablecido, convirtiendo el nodo directamente en una hoja.

### Postpoda
Permite que el árbol crezca completamente hasta memorizar el entrenamiento y luego elimina de forma descendente o ascendente los subárboles menos confiables o que no apertan mejoras en la precisión. Entre sus variantes principales destacan:
* Poda de reducción de error: Evalúa el impacto de eliminar cada nodo sobre un conjunto de validación independiente.
* Poda por complejidad de costo (CART): Evalúa na función que penaliza el error del árbo según su número total de hojas.
* Poda pesimista (C4.5): Ajusta la tasa de error observada en el entrenamiento sumándole un factor de penalización estadística, evitando requerir un dataset de validación separado.
* Postpoda de reglas (C4.5): Convierte el árbol completo en un conjunto equivalente de reglas _IF-THEN_ y elimina las condiciones que no mejoran la precisión estimada.


## 8.  ¿Qué ocurre si mi conjunto de datos está muy desbalanceado, es decir tiene 85% de los casos etiquetado como A y un 15% etiquetado como B?

Cuando un conjunto de datos está desbalanceado, los algoritmos de clasificación tienden a sesgarse hacia la clase mayoritaria (85% en este caso), prediciendo casi siempre esa clase para minimizar el error global. Esto provoca que la métrica de exactitud resulte engañosamente alta, aunque el modelo sea incapaz de detectar los casos de la clase minoritaria (15%), que suele ser la de mayor interés.


El problema del desbalance de clases ocurre cuando la distribución de la variable objetivo es fuertemente asimétrica. Como la mayoría de los algoritmos de aprendizaje supervisado están diseñados para maximizar la exactitud global asumiendo que todos los errores cuestan lo mismo, el modelo descubre que la vía más "eficiente" para reducir la tasa de error es etiquetar casi todo como perteneciente a la clase mayoritaria.

Para solucionar este problema se aplican 3 estrategias principales:

### Tratamiento de los datos de entrenamiento
* Sobremuestreo: Replica o sintetiza regitsros de la clase minoritaria hasta equilibrar proporciones.
* Submuestreo: Elimina de forma aleatoria registros de la clase mayoritaria para igualar la cantidad de ejemplos.

### Ajuste de umbral
Modifica el umbral de decisión en clasificadores probabilísticos para facilitar que los registros sean asignados a la clase rara.

### Métricas de evaluación adecuadas
Reemplazar la exactitud por métricas derivadas de la matriz de confusión:
* Sensibilidad o exhaustividad: Mide qué porcentaje de los casos reales de la clase minoritaria logró detectar el modelo.
* Especificidad: Mide el porcentaje de aciertos sobre la clase mayoritaria.
* Precisión: Mide qué proporción de los casos etiquetados como la clase minoritaria eran realmente correctos.
* Puntaje F1 y Área bajo la curva ROC: Métricas combinadas que evalúan la calidad del equilibrio entre falsos positivos y falsos negativos.


> Bibliografía sugerida:
> - Mitchell, T. M. (1997). Machine learning. 1997. McGraw Hill, 45(37), 870-877. **Cap 3**.
> - Larose, D. T., & Larose, C. D. (2014). *Discovering knowledge in data: an introduction to data mining* (Vol. 4). John Wiley & Sons.**Cap 8**
