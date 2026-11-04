# GUÍA DE LECTURA: Clustering

## 1.  ¿Cuál es el objetivo de un algoritmo de clustering?

El objetivo principal de un algoritmo de agrupamiento es descubrir la estructura u organización oculta en un conjunto de datos dividiendo las observaciones en grupos o cúmulos (clusters). Esta partición busca que las observaciones dentro de un mismo grupo sean lo más parecidas posible entre sí y lo más diferentes posible respecto a las de otros grupos.


En el paradigma de aprendizaje no supervisado, los datos no contienen una variable objetivo (target) ni etiquetas (labels) de clase predefinidas. El algoritmo analiza directamente el espacio formado por los atributos o características de cada registro para encontrar patrones intrínsecos sin guía externa.

Para medir qué tan parecidos o distintos son dos registros, los algoritmos utilizan funciones de distancia o disimilitud (como la distancia euclídea). Matemáticamente, la tarea de clustering persigue un doble objetivo de optimización:
1. Minimizar la variación dentro del grupo: Los elementos asignados a un mismo grupo deben estar lo más cercanos posible entre sí (alta similitud intra-clase).
2. Maximizar la variación entre grupos: Los distintos grupos deben estar lo más distantes posibles entre sí (alta **disimilitud** inter-clase).

Adicionalmente en la fase de preprocesamiento de datos, el clustering se utiliza como herramienta para identificar valores atípicos, dado que aquellos registros que quedan aislados o no logran integrarse a ningún grupo denso pueden considerarse anomalías.


## 2.  ¿Qué función tienen las medidas de distancia? ¿Qué diferencia existe entre Euclidiana, Manhattan y Mikowsky?

Las medidas de distancia son funciones matemáticas que cuantifican la disimilitud o separación entre objetos expresados por variables numéricas en un espacio multidimensional. La distrancia de Minkowski es la fórmula métrica general paramétrica, mientras que la distancia Euclídea y la distancia de Manhattan son casos particulares de Minowski cuando el parámetro de potencia se evalúa en $h=2$ (línea recta) y $h=1$ (recorrido en grilla), respectivamente.


En el análisis de agrupamiento, cada registro se representa como un vector de coordenadas en un espacio de _p_ dimensiones numéricas. La función de distancia asigna un valor numérico a la separación entre dos puntos $i = (x_{i1}, x_{i2}, \dots, x_{ip})$ y $j = (x_{j1}, x_{j2}, \dots, x_{jp})$ .

Para que una función sea considerada una métrica de distancia formal, debe cumplir con cuatro propiedades matemáticas fundamentales:

1. No negatividad: $d(i,j) \ge 0$.
2. Indentidad de los indiscernibles: $d(i,i) = 0$ (la distancia entre un punto y sí mismo es cero).
3. Simetría: $d(i,j) = d(j,i)$.
4. Desigualdad triangular: $d(i,j) \le d(i,k) + d(k,j)$ (la distancia directa nunca es mayor que pasar por un punto intermedio $k$).

La diferencia entre los tres tipus de distancia radican en el parámetro de la norma vectorial utilizada:

- Distancia de Minowski: Es una generalización para espacios normados (conocida como norma $L_p$ o $L_h$). Su fórmula matemática general es:

    $$d(i,j) = \left( \sum_{k=1}^{p} |x_{ik} - x_{jk}|^h \right)^{1/h}$$

    donde _h_ es un número real tal que $h \ge 1$ . Variar el parámetro $h$ permite ajustar la sensibilidad de la distancia ante diferencias en los componentes.

- Distancia Euclídea: Es el caso particular donde $h = 2$ (norma $L_2$). Mide la distancia geométrica más corta en "línea recta" entre dos puntos en el espacio:

    $$d(i,j) = \sqrt{\sum_{k=1}^{p} (x_{ik} - x_{jk})^2}$$

- Distancia de Manhattan: Es el caso particular donde $h = 1$ (norma $L_1$). Mide la suma de las diferencias absolutas eje por eje, simulando el recorrido en una cuadrícula o red de calles:

    $$d(i,j) = \sum_{k=1}^{p} |x_{ik} - x_{jk}|$$


### Ejemplo
Dados dos registros de clientes representados en dos atributos $X_1 = \text{Edad}$ y $X_2 = \text{Ingresos (en miles)}$: Punto $x_1 = (1, 2)$ y Punto $x_2 = (3, 5)$.

- Euclídea ($h=2$): Calculamos $\sqrt{(1-3)^2 + (2-5)^2} = \sqrt{(-2)^2 + (-3)^2} = \sqrt{4 + 9} = \sqrt{13} \approx 3.61$. Es la hipotenusa en diagonal entre ambos puntos.

- Manhattan ($h=1$): Sumamos las diferencias absolutas en cada eje: $|1-3| + |2-5| = 2 + 3 = 5$.

- Minkowski: Es la expresión general. Si definimos $h=1$ resulta $5$, si definimos $h=2$ resulta $3.61$, y si definimos $h=3$ resolveríamos $\sqrt$ ${|1-3|^3 + |2-5|^3} = \\sqrt$ ${8 + 27} = \\sqrt$ ${35} \approx 3.27$.


Si imaginamos que estamos en una esquina (Calle 1, Avenida 2), y debemos movernos a otra esquina distinta (Calle 3, Avenida 5)... La distancia de Manhattan es el trayecto a pie siguiendo las calles (avanzar 2 cuadras y luego 3 cuadras), caminando 5 cuadras en total. La distancia Euclídea sería equivalente a volar en línea recta hacia la otra esquina, recorriendo lo equivalente a 3.61 cuadras. Y la distancia de Minkowski es el concepto matemático general utilizado para calcular distancias variando su $h$.


3.  ¿Qué tareas transformación son requeridas para aplicar clustering?

Para aplicar algoritmos de agrupamiento, es indispensable transformar los datos crudos a una representación numérica homogénea y de escala comparable. Las tareas principales de transformación de datos incluyen la normalización o estandarización de variables continuas, la recodificación de variables cualitativas a variables bandera, la corrección del sesgo distribucional y la reducción de dimensionalidad.


Como la mayoría de los métodos de clustering miden la disimilitud mediante funciones de distancia en un esapacio vectorial, los datos de entrada deben cumplir ciertos requeisitos geométricos:

1. Normalización o estandarización: Si los atributos poseen magnitudes o unidades de medida diferentes (ej: sueldo en pesos versus edad en años), el atributo con valores numéricos más grandes dominará el cálculo de la distancia. Para evitar este sesgo, se ajustan los rangos mediante métodos como Min-Max o Z-Score.
2. Transformación de variables categóricas: La distancia Euclídea directa requiere valores cuantitativos. Las variables cualitativas u ordinales deben recodificarse mediante variables dummy o flags (atributos binarios con valor 0 o 1) o evaluarse mediante métricas específicas como el coeficiente de Jaccard para atributos binarios asimétricos.
3. Tratamiento del sesgo y asimetría: Cuando una variable presenta una distribución altamente sesgada hacia un extremo (ej: ingresos de la población), se aplican transformaciones matemáticas (logarítmica, raíz cuadrada) para aproximarla a una distribución normal.
4. Imputación de datos faltantes y filtrado de ruido: Puesto que un registro con campos vacíos no puede ubicarse correctamente en el espacio métrico, se requiere imputar valores ausentes (mediante media, mediana o regresión) o remover valores atípicos aislados que no distorsionen la posición de los centroides.
5. Reducción de dimensionalidad: Cuando existen demasiados atributos, la maldición de la dimensionalidad provoca que todos los puntos parezcan estar a la misma distancia. Por ello, se suele aplicar análisis de componentes principales (PCA) para proyectar los datos en un conjunto menor de variables sintéticas descorrelacionadas antes de agrupar.

### Ejemplo
Consideremos un conjunto de datos de clientes con tres variables: `Edad` (de 18 a 80 años), `Ingreso Anual` (de $10.000 a $1.000.000) y `Estado Civil` ("Soltero", "Casado").

1. Sin transformación: Una variación de $10.000 en sueldo eclipsará por completo una diferencia de 40 años de edad en el cálculo de la distancia Euclídea.

2. Con transformación:
    * Aplicamos normalización Min-Max a `Edad` e `Ingreso`, haciendo que ambas variables tomen valores estrictamente entre 0 y 1.
    * Convertimos `Estado Civil` en la variable bandera `casado_flag` (1 si es casado, 0 en caso contrario).
    * Ahora, todos los atributos aportan de forma equilibrada a la distancia entre clientes.


## 4.  ¿Qué ocurre con las variables categóricas?

Las variables categóricas o nominales no poseen una escala numérica continua ni una geometría vectorial de forma natural, por lo que no es posible aplicarle directamente operaciones geométricas convencionales como la distancia euclídea o el cálculo de promedios. Para utilizarlas en algoritmos de agrupamiento se deben recodificar mediante variables flag o dummy, calcular su disimilitud mediante el ratio de coincidencias o el coeficiente de Jaccard, o bien utilizar algoritmos adaptados como K-modas.


En métodos de particionamiento pasados en distancias como K-medias, las observaciones se posicionan en un espacio multidimensional numérico y los centroides se obtienen mediante la media aritmética de cada atributo. Las variables categóricas representan categorías discretas sin orden numérico (ej: estado civil, color, tipo de contrato), lo que plantea dos impedimentos técnicos principales:
1. Imposibilidad de medir distancias continuas: No se puede restar algebraicamente un valor cualitativo de otro para calcular diferencias geométricas.
2. Imposibilidad de promediar categorías: El cálculo del centroide promediando etiquetas no genera un valor válido en el dominio (el promedio entre "Casado" y "Soltero" no existe).

Para abordar las variables categóricas en clustering se establecen algunas alternativas de procesamiento:

### Ratio de disimilitud
Mide la diferencia entre dos objetos $i$ y $j$ según la cantidad de atributos en los que difieren:

$$d(i,j) = \frac{p - m}{p}$$

donde $p$ es el total de variables evaluadas y $m$ es el número de coincidencias exactas. Si los valores coinciden, la distancia es 0, si son diferentes, es 1.

### Recodificación a variables binarias (dummy/flag)
Cada estado o categoría se convierte en un atributo binario (0 o 1). Si las variable binarias resultantes son asimétricas (donde la presencia compartida de un 1 es más significativa que la de un 0), la similitud entre registros se calcula mediante el coeficiente de Jaccard:

$$sim(i,j) = \frac{q}{q + r + s}$$

donde $q$ es la cantidad de coincidencias de valor 1 y $r$ y $s$ representan las discrepancias entre ambos objetos.

### Transformación de variables ordinales
Si la variable categórica posee un orden intrínseco (ej: "malo", "regular", "excelente"), se asignan rangos enteros $r_{if} \in \{1, \dots, M_f\}$ a cada nivel y se normalizan al rango  $[0.0, 1.0]$ mediante la función:

$$z_{if} = \frac{r_{if} - 1}{M_f - 1}$$

quedando habilitada para tratarse como una variable numérica continua.

### Algoritmo de K-modas
Es una variante de K-media diseñada específicamente para datos categóricos. Reemplaza las medias de los clusters por sus modas y utiliza funciones de disimilitud basadas en frecuencias de coincidencia en lugar de distancias euclídeas. (Cuando se combinan datos numéricos y categóricos en un mismo dataset, la integración de K-medias y K-modas se denomina K-prototipos).


## 5.  Para los algoritmos de clustering presentados en clase (k-mean y jerárquicos), describir los siguientes aspectos:

Tanto el algoritmo de K-medias como el jerárquico buscan organizar observaciones en grupos homogéneos. Sin embargo, se diferencian fundamentalmente en las formas geométricas que pueden capturar, los parámetros requeridos antes de su ejecución y sus restricciones computacionales y operativas.

### a.  La forma de los clusters que pueden ser determinados (Cap. 7 Han)

**K-medias** solo puede descubrir clusters de forma esférica o convexa y de tamaño relativamente similar. Debido a que asigna los puntos en el centroide o media más cercana minimizando la distancia euclídea al cuadrado, particiona el espacio en regiones convexas (celdas de Voronoi). No puede identificar estructuras de formas arbitrarias, alargadas, angulares o con grandes diferencias de densidad.


En el **Clustering Jerárquico** la forma de los clusters depende directamente del método de enlace o amalgamiento utilizado para medir la distancia entre grupos:
- Enlace simple: Evalúa la distancia mínima entre pares de observaciones. Puede encontrar clusters de formas arbitrarias y no convexas, pero sufre del efecto de "encadenamiento", uniendo grupos distintos a través de cadenas de puntos cercanos.
- Enlace completo y Enlace promedio: El enlace completo evalúa la distancia máxima y el promedio evalúa la disimilitud media entre todas las parejas de puntos. Ambos tienden a generar clusters más compactos, esféricos y de tamaños uniformes.

### b.  Parámetros de entrada

#### K- medias
1. Número de clusters (K): Es un parámetro obligatorio que debe ser especificado a priori (antes de ejecutar el algoritmo).
2. Matriz de datos / Observaciones (D) con atributos numéricos.
3. Métrica de distancia (habitualmente distancia euclídea).
4. Semilla / Inicialización aleatoria y número máximo de iteraciones.

#### Clustering Jerárquico
1. Matriz de disimilitud o distancias entre todas las $n$ observaciones (matriz de $n \times n$).
2. Criterio de enlace (linkage): Selección entre enlace simple, completo y promedio o centroide.

No requiere especificar la cantidad de clusters K al inicio. La cantidad de grupos se determina a posteriori realizando un corte horizontal a la altura deseada sobre el dendograma.

### c.  Limitaciones

#### K-medias
1. Sensibilidad a valores atípicos (outliers) y ruido: Un solo punto distante distorsiona fuertemente la media aritmética del centroide y desplaza las fronteras del grupo.
2. Dependencia de la inicialización: Dado que asigna centroides iniciales al azar, puede quedar atrapado en óptimos locales, no encontrando el mejor agrupamiento global, por lo que exige múltiples ejecuciones.
3. Obligatoriedad de prefijar K: Si el número K elegido no refleja la estructura real de los datos, fragmentará o forzará uniones erróneas.
4. Restricción a variables continuas numéricas: Al requerir medias aritméticas, no opera directamente sobre los datos categóricos cualitativos.

#### Clutering Jerárquico
1. Irreversibilidad de las decisiones: Una vez que dos clusters se fusionan o se dividen, la decizión no se puede deshacer en pasos posteriores, propagando errores cometidos en las primeras fases.
2. Alto costo computacional y de memoria: Requiere almacenar una matriz de distancias de orden $O(n^2)$ y su complejidad temporal es de $O(n^3)$ o $O(n^2 \log n)$, lo que lo hace poco escalable e impracticable para conjuntos de datos masivos.
3. Dificultad para elegir la altura de corte: En conjuntos de datos complejos con mucho ruido, determinar la altura óptima del dendograma para definir $K$ puede resultar ambiguo o arbitrario.


## 6.  ¿Utilidad tiene un dendrograma? ¿Cómo se construye?

Un dendograma es un diagrama en forma de árbol que representa de manera gráfica la jerarquía de agrupamientos obtenida mediante un algoritmo de clustering jerárquico. Su principal utilidad es visualizar las fusiones o divisiones de grupos a distintas alturas de disimilitud, lo que permite determinar y seleccionar la cantidad óptima de cúmulos a posteriori mediante un corte horizontal.

### Utilidad
- Visualización de la estructura jerárquica: Permite observar cómo se organizan las observaciones desde elementos individuales hasta formar un único gran grupo, sin necesidad de definir previamente el número de clusters.
- Determinación flexible de K (a posteriori): Al trazar una línea horizontal a una determinada altura en el eje vertical, el dendograma se "corta" y las ramas intersectadas definen los clusters resultantes. Esto sustituye la necesidad de adivinar el parámetro $K$ antes de ejecutar el modelo.
- Medición de similitud: La altura de fusión en el eje vertical indica qué tan parecidos o diferentes son dos elementos o grupos. Cuanto más abajo se unan dos ramas (menor altura), mayor es su similitud.
- Identificación de valores atípicos: Aquellas observaciones aisladas que se unen al árbol a alturas de disimilitud muy elevadas pueden identificarse fácilmente como anomalías.

### Cómo se construye
Su construcción se realiza mayoritariamente mediante el enfoque aglomerativo:

1. Paso 0 (Inicialización): Se parte de $n$ observaciones individuales; cada observación constituye un cluster propio (las hojas en la base del arbol).

2. Cálculo de distancias: Se construye una matriz de disimilitud o distancia midiendo la separación entre todos los pares de observaciones.

3. Fusión de los clusters más cercanos: Se identifica el par de clusters con menor distancia (mayor similitud) y se los une formando una primera rama. La altura vertical del trazo representa el valor numérico de esa distancia.

4. Actualización de la matriz de distancias: Se recalculan las distancias entre el nuevo grupo fusionado y los demás clusters del conjunto mediante un criterio de amalgamiento (linkage) (como enlace simple, completo o promedio).

5. Iteración ascendente: Se repiten los pasos de fusión y actualización de distancias hasta que todas las observaciones quedan combinadas en un único cluster raíz en la cima del diagrama.

De forma inversa, en enfoque divisivo comienza con un único cluster que contiene la totalidad de las observaciones y realiza particiones recursivas descendentes hasta que cada registro queda en su propia hoja.


## 7.  ¿Para qué sirven las estrategias de linkage? ¿Qué tipos existen? ¿Qué limitaciones pueden tener?

Las estrategias de enlace son reglas matemáticas utilizadas en el agrupamiento jerárquico para medir la distancia o disimilitud entre dos clusters que contienen múltiples observaciones. Su función principal es extender las funciones de distancia entre puntos individuales hacia la medición de disimilitud entre grupos de puntos, determinando qué pares de clusters se deben fusionar en cada etapa.


En el clustering jerárquico aglomerativo se comienza tratando a cada observación como un cluster individual. A medida que los puntos se van combinando, surge la necesidad de calcular la distancia entre un objeto y un grupo, o entre dos grupos de objetos. Dado que una distancia euclídea estándar solo compara dos puntos individuales, se aplican diferentes criterios de enlace para evaluar la disimilitud inter-grupo.

### Enlace Simple
Define la disimilitud entre dos clusters como la distancia mínima posible entre cualquier observación del primer cluster y cualquier observación del segundo cluster.

**Limitaciones**: Sufre del efecto de encadenamiento. Al buscar distancias mínimas locales, tiende a unir puntos individuales uno a uno formando clusters alargados y sesgados. Esto puede conectar erróneamente dos clusters distantes mediante una "cadena" de puntos ruidosos intermediarios.


### Enlace Completo
Define la disimilitud como la distancia máxima entre cualquier observación del primer cluster y cualquier observación del segundo cluster.

**Limitaciones**: Es sumamente sensible a valores atípico. Debido a que se basa en la distancia máxima, un solo punto distante dentro de un grupo puede inflar desproporcionadamente la disimilitud inter-grupo, forzando la creación de clusters compactos y esféricos pero rompiendo estructuras con formas naturales irregulables.


### Enlace Promedio
Calcula la distancia promedio entre todos los pare posible formados por una observación del primer cluster y una observación del segundo.

**Limitaciones**: Representa un compromiso equilibrado entre las distancias mínimas y máximas, pero requeiere mayor costo computacional al tener que calcular todas las combinaciones de distancias por pares entre los elementos de los grupos.


### Enlace Centroide
Calcula el centroide (vector de medias) de cada uno de los dos clusters y mide la distancia directa entre ambos centroides.

**Limitaciones**: Presenta un problema denominado inversión en la visualización, en el cual dos clusters pueden fusionarse a una altura menor (menor disimilitud) que las fusiones que les dieron origen, complicando la interpretación geométrica del árbol.


### Impacto en el Dendrograma
La elección del linkage altera drásticamente la forma del dendrograma sobre un mismo conjunto de datos. El enlace simple genera dendrogramas asimétricos y torcidos en forma de escalera, mientras que el enlace completo y el promedio producen árboles equilibrados y simétricos, facilitando la selección de la altura de corte.


## 8.  ¿Cómo se interpretan los centroides en un ajuste de k-means?

En un ajuste de agrupamiento K-medias, un centroide es el vector de medias de todas las observaciones de un cluster determinado. Se interpreta como el "perfil promedio" o prototipo sintético que sintetiza el comportamiento central del grupo en cada uno de sus atributos o características


Durante el algoritmo de K-medias, el espacio de datos posee $p$ dimensiones numéricas. Cuando un grupo de registros es asignado a un cluster $C_k$, el centroide $\vec{c}_k$ se calcula obteniendo la media aritmética de los datos dimensión por dimensión:

$$\vec{c}_k = \left( \frac{1}{|C_k|} \sum_{i \in C_k} x_{i1}, \frac{1}{|C_k|} \sum_{i \in C_k} x_{i2}, \dots, \frac{1}{|C_k|} \sum_{i \in C_k} x_{ip} \right)$$

La interpretación de los centroides se analiza desde los siguientes aspectos clave:

1. Perfilado de clusters: Las coordenadas numéricas del centroide definen los valores típicos de ese grupo. Al comparar los vectores de centroides de distintos clusters entre sí o respecto a la media general del conjunto de datos, el analista puede identificar qué atributos caracterizan o diferencian a cada segmento.

2. Punto sintético o abstracto (no necesariamente real): El centroide es un centro de masa calculado matemáticamente. Rara vez coincide con un registro real existente en la base de datos.

3. Centro de gravedad de optimización: Matemáticamente, el centroide es el punto de referencia que minimiza la variación dentro del cluster medida como la suma de errores al cuadrado.

---

> Bibliografía sugerida:
> - Han, J., Kamber, M., & Pei, J. (2012). Data mining concepts and techniques third edition. University of Illinois at Urbana-Champaign Micheline Kamber Jian Pei Simon Fraser University. Cap. 7
> - Larose, D. T., & Larose, C. D. (2014). *Discovering knowledge in data: an introduction to data mining* (Vol. 4). John Wiley & Sons. **Cap 10**
