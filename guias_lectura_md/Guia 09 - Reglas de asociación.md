# GUÍA DE LECTURA: Reglas de asociación

## 1.  ¿Qué es una regla de asociación? ¿Qué interpretación tiene?

Una regla de asociación es una expresión de la forma $X \Rightarrow Y$ (si antecedente $X$, entonces consecuente $Y$) que identifica patrones de presencia o coocurrencia entre conjunto de atributos dentro de un mism ogrupo de transacciones. Su interpretación indica la probabilidad o frecuencia con la que un conjunto de elementos se observa junto a otro en la base de datos, sin implicar nunca una relación de causa y efecto.

En el tiempo de la minería de datos, el análisis de reglas de asociación surge históricamente a partir del análisis del carrito de compras. Este método busca cuantificar qué artículos "van juntos" en una misma entidad de registro llamada transacción.

Una regla de asociación se compone de las siguientes partes y métricas:

- Antecedente y Consecuente: En la regla $X \Rightarrow Y$, $X$ representa el conjunto de ítems condición e $Y$ el conjunto resultante (consecuente). Ambos deben ser conjuntos de items disjuntos ($X \cap Y = \emptyset$).

- Soporte: Representa la proporción o porcentaje del total de transacciones que contienen tanto el antecedente como el consecuente de forma simultánea ($P(X \cup Y)$).

- Confianza: Mide la certeza de la regla mediante la probabilidad condicional $P(Y|X)$, es decir, el porcentaje de transacciones con el antecedente $X$ que también incluyen el consecuente $Y$.

**Interpretación estricta**: La flecha $\Righarrow$ en una regla de asociación simboliza únicamente una relación de ocurrencia conjunta o afinidad estadística. NO implica causalidad (que comprar $X$ cause la compra de $Y$) ni una secuencia temporal obligatoria.

### Ejemplo
En un supermercado se analizan 1.000 transacciones de compra. Se observa que en 200 de esas compras los clientes llevaron "pañales" y "cerveza" juntos, lo que equivale a un **soporte** del 20% (200/1.000). De los 200 clientes que compraron "pañales", 50 de ellos agregaron también "cerveza", lo que representa una **confianza** del 25% (50/200). La regla se expresa como: `{pañales} => {cerveza}` con `[soporte = 20%, confianza = 25%]`.


## 2.  Defina los conceptos de item, itemset y k-itemset.

Los conceptos de item, conjunto de items y k-conjunto de items son los bloques de construcción fundamentales del análisis de reglas de asociación. Definen desde los objetos o productos individuales que integran una transacción hasta las agrupaciones de distinto tamaño que se analizan para encontrar patrones de coocurrencia en las bases de datos.

En el modelo de datos de transacciones (ej. carrito de compras), la información se organiza a partir de un universo de elementos y sus combinaciones:

1. Item: Es la unidad u objeto individual dentro del dominio de análisis. Matemáticamente, si $I = \{i_1, i_2, \dots, i_m\}$ representa el conjunto universal de todos los productos o atributos disponibles, un item es un elemento $i \in I$. En el esquema clásico binario, cada item se registra según su presencia o ausencia dentro de una transacción.

2. Conjunto de items (itemset): Es una colección o subconjunto no vacío formado por uno o más items. Se denota como $A \subseteq I$. Una transacción contiene a un itemset si todos los elementos de ese conjunto están presentes simultáneamente en dicho registro.

3. k-conjunto de items (k-itemset): Es un itemset que contiene exactamente $k$ items distintos, donde $k$ indica la cantidad de elementos o longitud del conjunto ($|A| = k$). El parámetro $k$ permite clasificar y procesar de forma ordenada los conjuntos de items según su tamaño durante la ejecución de los algoritmos de minería de datos.

### Ejemplo

Consideremos la base de datos de transacciones de un supermercado cuyo catálogo total consta de los productos: `I = {Pan, Leche, Pañales, Cerveza, Manteca}`.

- **Ítem**: Cada producto individual. Por ejemplo, `Pan` es un ítem, `Cerveza` es otro ítem.

- **Itemset**: Cualquier combinación de estos productos elegida para analizar. Por ejemplo, `{Pan, Leche}` o `{Pañales, Cerveza, Manteca}`.

- **k-itemset**:
    * 1-itemset ($k=1$): {Leche} (conjunto formado por 1 ítem).
    * 2-itemset ($k=2$): {Pañales, Cerveza} (conjunto formado por 2 ítems).
    * 3-itemset ($k=3$): {Pan, Leche, Manteca} (conjunto formado por 3 ítems).


## 3.  ¿Qué mide y para qué se utiliza la medida de soporte?

El soporte es una métrica cuantitativa de frecuencia que mide la proporción o porcentaje de transacciones en una base de datos que contienen un conjunto de items específico o la combinación de antecedente y consecuente de una regla de asociación. Se utiliza para evaluar la relevancia estadística y la cobertura de los patrones descubiertos, sirviendo como filtro de soporte mínimo para podar combinaciones infrecuentes en el espacio de búsqueda.

En el análisis de patrones de coocurencia (ej. carrito de compras), el soporte cuantifica la presencia compartida de elementos dentro del registro de datos. Puede expresarse formalmente en dos formatos:

- Soporte absoluto: Representado habitualmente por la letra $\sigma$ (sigma) o count, mide la cantidad entera de transacciones en las que aparece el conjunto de items analizados.

- Soporte relativo: Es la probabilidad estadística $P(X \cup Y)$ calculada como la razón entre el soporte absoluto y el número total de transacciones $N$ presentes en la bsae de datos:

$$\text{soporte}(X \Rightarrow Y) = P(X \cup Y) = \frac{\text{count}(X \cup Y)}{N}$$

### Para qué se utiliza la medida de soporte

- Medir la significancia estadística y cobertura: Permite distinguir patrones representativos en al población de aquellos que ocurren de manera marginal. Si una regla posee un soporte insignificante, representará un evento raro o ruido sin utilidad práctica.

- Poda y reducción de la complejidad computacional: Algoritmos como Apriori utilizan un umbral de soporte mínimo prefijado por el usuario para determinar qué conjuntos son frecuentes. Gracias a la propiedad de anti-monotonía o propiedad Apriori, si un conjunto de items no alcanza el soporte mínimo, ninguno de sus superconjuntos podrá alcanzarlo. Esto permite descartar prematuramente ramas enteras de combinaciones sin necesidad de escanear la base de datos para contarlas.

### Ejemplo
Supongamos una base de datos con $N = 100$ transacciones de ventas en una farmacia. Si en 20 de esas ventas las facturas registran simultáneamente {protector solar, agua mineral}, el soporte absoluto de dicho conjunto de ítems es 20, y su soporte relativo es $20 / 100 = 0,20$ o 20%. Si fijamos un **soporte mínimo** del 15%, este conjunto se clasifica como frecuente y se utilizará para construir reglas. Si en cambio fijamos el umbral en 25%, se clasificará como infrecuente y se podará automáticamente.


## 4.  Dado que el soporte sigue una distribución asimétrica de cola pesada, qué interpretación puede hacer del histograma del soporte para una base de transacciones cualquiera.

En el histograma del soporte para una base de datos de transacciones refleja una distribución asimétrica de cola pesada: muestra que un número sumamente reducido de conjuntos de elementos (itemsets) o artículos individuales concentra la mayor frecuencia de compra, mientras que la inmensa mayoría de las combinaciones posibles tiene un soporte muy cercano a cero.

En el análisis del carrito de compras, los comportamientos de consumo siguen patrones no uniformes que responden a leyes de distribución de cola larga:

1. La cambeza del histograma: En el extremo izquierdo se ubican muy pocos items o combinaciones (ej. pan, leche, agua) que aparecen de forma recurrente en una gran proporción del total de transacciones de la base de datos.

2. La cola pesada: A medida que se evalúan los k-conjuntos de elementos (k-itemsets) de mayor longitud o combinaciones específicas, la cantidad de compbinaciones posibles crece de manera combinatoria. Sin embargo, la probabilidad de que todos esos items ocurran juntos dentro de una misma transacción cae drásticamente. Esto genera una barra muy elevada en valores de soporte mínimo y una cola muy larga que se extiende hacia la derecha con frecuencias insignificantes.

**Interpretación para la minería de datos**: Esta asimetría justifica la necesidad metodológica de definir un soporte mínimo como filtro. Si se intenta analizar la base de datos sin un umbral de corte o con un 'min_sup' demasiado bajo, el algoritmo intentará procesar la "cola pesada", desencadenando una explosión combinatoria de candidatos que desbordará la memoria y el tiempo de cómputo. El corte de soporte mínimo permite descartar la masa de combinaciones irrelevantes de la cola y conservar únicamente la cabeza de patrones frecuentes.

### Ejemplo
En un supermercado que comercializa 10.000 productos distintos:

- **En la cabeza**: Las combinaciones populares como `{leche, pan}` o `{gaseosa}` tienen un soporte del 25% (aparecen en 1 de cada 4 facturas).
- **En la cola pesada**: Combinaciones específicas como `{alimento para peces, queso brie, pilas AAA}` tienen un soporte de 0,001% (aparecen en 1 sola factura de 100.000). El histograma revela que más del 99% de las combinaciones matemáticamente posibles pertenecen a esta cola pesada de casi nula presencia.


## 5.  ¿Qué mide la confianza?

La confianza mide la certeza o probabilidad condicional de una regla de asociación. Cuantifica la frecuencia con la que el consecuente aparece en aquellas transacciones que ay coentienen el antecedente.

En el análisis de coocurrencia, una regla de asociasión se expresa como $X \Rightarrow Y$, donde $X$ representa el conjunto de atributos antecedente e $Y$ el consecuente. La confianza evalúa la fuerza de esa implicación respondiendo a la pregunta: _si una transacción contiene $X$, ¿qué tan probable es que también contenga $Y$?_

Matemáticamente, equivale a la probabilidad $P(Y|X)$. Se calcula como el cociente entre el soporte conjunto de la combinación $(X \cup Y)$ y el soporte individual del antecedente $X$:

$$\text{confianza}(X \Rightarrow Y) = P(Y|X) = \frac{\text{soporte}(X \cup Y)}{\text{soporte}(X)} = \frac{\text{count}(X \cup Y)}{\text{count}(X)}$$

_Una nota sobre precisión_: Al concentrarse exclusivamente en $P(Y|X)$, la confianza ignora la frecuencia apriori del consecuente $P(Y)$ por sí solo. Por esta razón, una regla puede presentar una confianza elevada simplemente porque el consecuente es un elemento masivamente popular en la base de datos y no por una verdadera relación entre $X e Y$.

### Ejemplo
En una tienda de computación se analizan las compras registradas. Se detecta que 100 clientes compraron una computadora ($X$). De esos 100 clientes, 60 agregaron también un software ($Y$) a la compra. La confianza de la regla {computadora} => {software} es $60 / 100 = 0,60$ o $60\%$. Esto significa que existe un $60\%$ de probabilidad de que un cliente que compra una computadora adquiera también software.


## 6.  La generación de reglas a través de enfoques basados en fuerza bruta son inaplicables. ¿Verdadero o falso? Justifique.

**Verdadero**. La generación de reglas mediante fuerza bruta es completamente inaplicable en la práctica debido a la explosión combinatoria que sufre el espacio de búsqueda a medida que aumenta la cantidad de artículos.

La minería de reglas de asociación consta de dos fases principales: el descubrimiento de todos los conjuntos de items frecuentes que superen un soporte mínimo, y la posterior extracción de reglas que cumplan con una confianza mínima. La primera fase constituye el paso de mayor costo computacional de todo el proceso.

Si se intentara resolver la búsqueda de conjuntos frecuentes por fuerza bruta, para un catalógo de $d$ items distintos existirá un espacio teórico de $2^d - 1$ subconjuntos posibles. Para evaluar el soporte de cada uno de esos candidatos, el algoritmo tendría que recorrer y contar de forma repetida la totalidad de las transacciones en la base de datos. Cuando el número de artículos $d$ es elevado (por ej. miles de productos en un comercio), calcular e inspeccionar las $2^d - 1$ combinaciones genera un consumo de memoria y tiempo de cómputo prohibitivo. Por esta razón, se emplean algoritmos eficientes como Apriori, que utilizan la propiedad anti-monotonía del soporte para podar los candidaros infrecuentes y reducir el espacio de búsqueda.


## 7.  ¿En qué consiste el criterio de apriori?

El criterio o principio de Apriori establece la propiedad de anti-monotonía del soporte: si un conjunto de elementos (itemset) es infrecuente porque no alcanza el soporte mínimo, se garantiza matemáticamente que todos sus superconjuntos (superset) también serán infrecuentes.

En el descubrimiento de reglas de asociación, evaluar todas las combinaciones posibles de productos en una base de transacciones generaría un colapso por explosión combinatoria. El principio de Apriori resuelve este problema reduciendo drásticamente el espacio de búsqueda mediante un mecanisto de poda:

- Propiedad anti-monotonía del soporte: La frecuencia de un conjunto de elementos no puede ser nunca mayor que la frecuencia de cualquiera de sus subconjuntos. Es decir, al agregar más items a un conjunto $X$ para formar un superconjunto $X \cup Y$, la probabilidad conjunta disminuye o se mantiene igual ($P(X \cup Y) \le P(X)$).

- Estrategia de poda: Si un itemset $X$ no satisface el umbral de min_sup, no es necesario contar ni evaluar en la base de datos ninguna combinación que contenga $X$, ya que se sabe de antemano que fallará la prueba de soporte.

- Búsqueda por niveles: El algoritmo Apriori utiliza este principio de forma iterativa (bottom-up). En el nivel $k = 1$, encuentra los 1-itemsets frecuentes ($L_1$); luego cobina estos elementos entre sí para generar los candidatos de tamaño 2 ($C_2$), evalúa su soporte en la base de datos para obtener $L_2$, y repite el proceso hasta que no se puedan formar más conjuntos frecuentes.

### Ejemplo
Supongamos que en un supermercado analizamos las ventas con un umbral de soporte mínimo del 1% (`min_sup = 1%`). Si el conjunto `{caviar}` aparece en solo 2 de 10.000 facturas (soporte del 0,02%), es clasificado como infrecuente. Por el principio de Apriori, sabemos automáticamente que `{caviar, leche}`, `{caviar, pan}` y `{caviar, leche, pan}` tendrán un soporte menor o igual al 0,02%. El algoritmo poda de inmediato todas esas combinaciones sin necesidad de realizar lecturas adicionales en la base de datos.


## 8.  Describa el algoritmo Apriori. ¿Cuáles son sus etapas?

El algoritmo Apriori es un método de búsqueda por niveles diseñado para descubrir conjuntos de elementos frecuentes y extraer reglas de asociación en bases de datos transaccionales. Su funcionamiento se apoya en el pricipio de Apriori o propiedad anti-monotonía descrita previamente, permitiendo podar candidatos infrecuentes antes de contar su soporte.

El proceso global del algoritmo se divide en dos fases secuenciales principales:

### Fase 1: Descubrimiento de conjuntos de elementos frecuentes
Es la fase de mayor costo computacional. Se ejecuta de manera iterativa nivel por nivel ($k = 1, 2, 3,...$), donde los conjuntos frecuentes de tamaño $k - 1 (L_{k-1})$ se utilizan para explorar los conjuntos candidatos de tamaño $k$ ($C_k$):

1. Paso inicial ($k = 1$): Se realiza un primer barrido de la base de datos para contar el soporte de cada elemento individual. Aquellos que superan el umbral de soporte mínimo forman el conjunto de 1-elementos frecuentes ($L_1$).

2. Generación de candidatos por autounión: Para $k \ge 2$, se genera la lista de candidatos $C_k$ uniendo $L_{k-1}$ consigo mismo ($L_{k-1} \bowtie L_{k-1}$). Dos conjuntos de $L_{k-1}$ se unen únicamente si comparten sus primeros $k - 2$ elementos en orden lexicográfico.

3. Paso de poda: Se aplica la propiedad anti-monotonía sobre $C_k$. Si algún subconjunto de tamaño $k - 1$ de un candidato $c \in C_k$ no se encuentra presente en $L_{k-1}$, ese candidato se elimina de $C_k$ de inmediato. Esto evita contar en la base de datos combinaciones que matemáticamente no podrán ser frecuentes.

4. Conteo de soporte y filtrado: Se escanea la base de datos para contabilizar la frecuencia real de los candidatos sobrebivientes $C_k$. Aquellos cuyo soporte sea mayor o igual al 'min_sup' se conservan para formar $L_k$.

5. Criterio de parada: El bucle se repite incrementando $k$ en 1 hasta que no se puedan generar más candidatos o la lista de conjuntos frecuentes resulte vacía ($L_k = $L_k = \emptyset$).

### Fase 2: Generación de reglas de asociación
A partir de todos los conjuntos frecuentes de $L$ hallados en la fase 1, se derivan las reglas de asociación fuertes:

- Para cada conjunto frecuente $l \in L$, se obtienen todos sus subconjuntos no vacíos $s$.

- Se evalúan las reglas de la forma $s \Rightarrow (l - s)$.

- Se calcula su confianza como la razón de soporte $\frac{\text{soporte}(l)}{\text{soporte}(s)}$. Si alcanza o supera el umbral de confianza mínima, se emite como regla válida.


## 9.  ¿Por qué es tan importante medir el Lift?

Es fundamental medir la sustentación (lift) porque la métrica de confianza por sí sola puede generar reglas engañosas al ignorar la frecuencia o probabilidad individual del consecuente. El _lift_ evalúa si la presencia del antecedente realmente incrementa la probabilidad de ocurrencia del consecuente o si la coocurencia entre ambos es un mero resultado del azar o la independencia estadística.

En el análisis de reglas de asociación, la confianza mide la probabilidad condicional $P(Y|X)$. Sin embargo, si el consecuente $Y$ es un producto extremadamente popular dentro de la base de transacciones, la confianza resultará alta de forma artificial sin que exista una relación real entre los atributos.

La sustentación corrige esta limitación al relativizar la confianza observada respecto a la probabilidad a priori del consecuente. Matemáticamente, para una regla de la forma $X \Rightarrow Y$, el lift se define como: 

$$\text{Lift}(X \Rightarrow Y) = \frac{\text{Confianza}(X \Rightarrow Y)}{\text{Soporte}(Y)} = \frac{P(X \cap Y)}{P(X) \cdot P(Y)}$$

El resultado numérico del lift se interpreta en tres escenarios clave:

- $\text{Lift} > 1$: Existe una correlación positiva. Indica que la presencia del antecedente $X$ incrementa o "eleva" verdaderamente la probabilidad de que ocurra el consecuente $Y$.

- $\text{Lift} = 1$: Representa la independencia estadística. La coocurrencia se debe únicamente al azar y la regla no aporta conocimiento útil ni interesante.

- $0 \le \text{Lift} < 1$: Indica una correlación negativa. Significa que la presencia de $X$ desalienta o disminuye la probabilidad de que aparezca $Y$.


## 10. Definir *closed* y *maximal frequent* itemset. ¿Qué información podemos obtener desde estos itemsets especiales?

Los conjuntos de items frecuentes cerrados (closed frecuent itemset) y los conjuntos de items frecuentes maximales (maxial frecuent itemsets) son representaciones compactas del espacio de patrones frecuentes. Los cerrados conservan de forma exacta el conteo de soporte de todos sus subconjuntos (compresión sin pérdida de información / lossless compression), mientras que los maximales identifican únicamente la frontera de combinaciones frecuentes sin preservar sus conteos individuales (compresión con pérdida / lossy compression).

Al reducir el umbral de soporte mínimo o trabajar con bases de datos densas, la cantidad de conjuntos de items frecuentes sufre una explosión combinatoria. Para mitigar este costo computacional y de almacenamiento, se definen dos tipos de itemsets especiales:

- Closed frequent itemset: Un conjunto de elementos $X$ es cerrado si no existe ningún superconjunto estricto $Y$ ($X \subset Y$) que posea el mismo conteo de soporte que $X$ en la base de datos. Es un closed frequent itemset si cumple la condición de cierre y además supera el 'min_sup'.
    * Información obtenida: Proporciona una cobertura completa sin pérdida de información. A partir de la lista de closed frequent itemsets y sus soportes asociados, es poible calcular e inferir la presencia y el conteo de soporte exacto de cualquier subconjunto frecuente sin necesidad de volver a escanear las transacciones.

- Maximal frequent itemset o max-itemset: Un conjunto de elementos $X$ es maximal si es frequente y no existe ningún superconjunto estricto $Y$ ($X \subset Y$) que sea frecuente.
    * Información obtenida: Define la frontera de frecuencia del espacio de búsqueda ofreciendo la máxima compresión. Permite saber con certeza que todos los subconjuntos de un max-itemset son frecuentes (por la propiedad anti-monotonía del soporte), pero no permite conocer el conteo de soporte exacto de esos subconjuntos (solo garantiza que están por encima del 'min_sup').

### Ejemplo
Supongamos una base de transacciones con solo dos compras registradas: $T_1 = \{a_1, a_2, \dots, a_{100}\}$ y $T_2 = \{a_1, a_2, \dots, a_{50}\}$, con un conteo de min_sup = 1.

- Frecuentes totales: Existen $2^{100} - 1 \approx 1,27 \times 10^{30}$ combinaciones frecuentes (inmanejables para procesar).

- Closed frequent itemsets: Se obtienen únicamente dos conjuntos cerrados: $\{a_1, a_2, \dots, a_{50}\}$ con soporte 2, y $\{a_1, a_2, \dots, a_{100}\}$ con soporte 1. Con esta información deducimos exactamente que $\{a_2, a_{45}\}$ tiene soporte 2 y $\{a_8, a_{55}\}$ tiene soporte 1.

- Maximal frequent itemsets: Se obtiene un único conjunto maximal: $\{a_1, a_2, \dots, a_{100}\}$ con soporte 1. Con esto confirmamos que $\{a_2, a_{45}\}$ es frecuente, pero perdemos el dato de si su soporte es 1 o 2.


> Bibliografía sugerida:
> - Jiawei Han, Micheline Kamber, Jian Pei. 2012. Tercera edición. Data Mining: Concepts and Techniques. Cap. 2 y Cap. 3
> - Daniel T. Larose. 2014. Segunda edición. Discovering Knowledge in Data: An Introduction to Data Mining.
> - Introduction to Data Mining (Second Edition) Pang-Ning Tan et al.
