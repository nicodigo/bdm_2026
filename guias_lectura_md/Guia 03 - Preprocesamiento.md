# GUÍA DE LECTURA III: Preprocesamiento

## 1.  ¿Qué se entiende por preprocesamiento según Han & Kamber? ¿Por qué es necesario preprocesar los datos?

El preprocesamiento de datos segun _Han_ es la fase fundamental de preparación que aplica técnicas de limpieza, integración, transformación y reducción sobre los datos crudos. Es **absolutamente necesario** porque los datos del mundo real son masivos y "sucios" (incompletos, ruidosos e inconsistentes).
**Sin datos de calidad es imposible extraer patrones confiables.**

En bases de datos y almacenes de datos reales, la recolección automática o humana genera imperfecciones constantes. _Han_ señala que la calidad de los datos no se limita solo a la precisión, sino que abarca múltiples dimensiones:
- Completitud: Qué tan completos están los registros (_missing values_).
- Exactitud: Que los valores no contengan errores o ruido.
- Consistencia: Que no existan discrepancias entre fuentes (formatos de fecha, duplicados).
- Oportunidad: Que la información esté actualizada a tiempo para la toma de decisiones.
- Credibilidad e Interpretabilidad: Cuánto confían los usuarios en los datos y qué tan fácil resulta comprender sus codificaciones.

Y esto afecta al proceso de KDD y hace que preprocesar sea indispensable, ya que si se ingresan datos con errores o incompletos al algoritmo de minería de datos lo que se obtenga seguramente sean patrones erroneos o engañosos.
Además el algoritmo va a poder operar mucho más eficientemente, sin tener que hacer tantas correcciones ni compensar por datos "sucios".
Finalmente mencionar que muchos algoritmos de aprendizaje automático basados en distancias como _k-nearest neighbors_ o _clustering_ exigen que los datos estén normalizados en un rango para evitar que atributos con números grandes dominen u "opaquen" a los pequeños.

## 2.  ¿Cuáles son las etapas de las tareas de preprocesamiento?

Según _Han_ las tareas de preprocesamiento consisten en cuatro etapas: limpieza, integración, reducción y transformación de datos. El objetivo es obtener la mejor calidad posible de los datos crudos e imperfectos y que los algoritmos de minería puedan operar correcta y eficientemente.

Si bien se presentan de forma secuencial _Han_ menciona que estas cuatro etapas no son cajas cerradas e independientes, sino operaciones que se solapan y colaboran entre si:

1. Limpieza de datos: se encarga de remover el "ruido", rellenar o gestionar valores faltantes _(missing values)_, identificar o eliminar valores atípicos _(outliers)_ y corregir inconsistencia entre los registros.
2. Integración de datos: Combina múltiples fuentes heterogéneas (bd relacionales, cubos de datos o archivos de texto o csv) en un almacén unificado. Resuelve el problema de identificación de entidades, elimina redundancias mediante el análisis de correlación (como _X²_ o coeficientes de correlación) y detecta duplicados.[^1]
3. Reducción de datos: Obtienen una representación comprimida o más pequeña en volumen de los datos, conservando la integridad analítica para que los resultados del minado sean idénticos o prácticamente los mismos. Incluye:
    - Reducción de dimensionalidad: Eliminación o combinación de atributos (Selección de subconjuntos o Análisi de Componentes Principales - _PCA_). [^2]
    - Reducción de numerosidad: Reemplazo del volumen original por modelos alternativos paramétricos (regresión) o no paramétricos (histogramas, clustering).
    - Compresión de datos: Transformaciones con pérdida o sin pérdida.
4. Transformación de datos: Consolida o escala los atributos en formas o rangos adecuados para los algoritmos. Sus principales estrategias son:
    - Escalado/Normalización: Ajusta los datos a un rango específico mediante min-max, z-score o escalado decimal.
    - Discretización: Reemplaza valores numéricos continuos por etiquetas de intervalo o conceptos más altos (ej transformar rangos de edades en "Joven", "Adulto", "Viejo").
    - Agregación: Operaciones de resumen o creación de nuevos atributos.

## 3.  😁 **Para ir conectando**: ¿Qué paralelo encuentra con lo visto en el curso hasta el momento con la etapa de sumarización y descripción de datos planteada por Han & Kamber? ¿Qué nombre recibe esa etapa o tarea en el proceso de KDD visto?

La sumarización y descripción de datos según _Han_ es la tarea descriptiva de minería de datos que condensa y abstrae un volumen masivo de datos de bajo nivel para generar perfiles comprensibles de alto nivel sobre una clase o concepto de interés. Comprende dos modalidades principales: la caracterización de datos, que resume las propiedades de una clase objetivo, y la discriminación de datos, que compara la clase objetivo frente a clases de contraste.

Desde la primera definición de KDD que analizamos, se establece el problema básico de las bases de datos masivas en el mapeo de datos de bajo nivel hacia formas más compactas, abstractas y comprensibles. Las organizaciones acumulan bases de datos con millones de transacciones individuales que son imposibles de asimilar por un individuo. La sumarización y descripción ayuda en este caso tomando el detalle atómico y elevenado a un nivel conceptual comprensible.

En el proceso de KDD visto hasta el momento la tarea puede relacionarse a dos etapas:
- Como tarea de minería de datos _Sumarización_ o _Descripción de conceptos_. Además de mencionarse como _Caracterización de datos_ y _Discriminación o Comparación de Datos_.
- Como operación de transformación: _Generalización de Datos_ y _Agregación_.

[^3]

## 4.  ¿Cuáles son las principales tareas que se realizan en la etapa de limpieza de datos?

La limpieza de datos es la etapa del preprocesamiento que busca detectar y corregir los errores, imperfecciones y variaciones indeseadas en los datos crudos. Sus tareas principales son: manejar valores faltantes, suavizar el ruido, identificar y tratar valores atípicos y resolver inconsistencias y duplicados.

Cómo no hace mal recalcar **las bases de datos reales del mundo real son masivas e imperfectas debido a errores humanos, fallas en instrumentos o problemas de transmisión**. La etapa de limpieza de datos opera mediantes cuatro tareas:
- Manejo de valores faltantes _(missing values)_: Ocurre cuando existen tuplas o atributos sin valores registrados (por formularios incompletos, fallas de sensores o respuestas no aplicables). Para tratarlos existen dos grandes caminos:
    * Eliminación: Descarta la tupla completa o eliminar variables con alto porcentaje de faltantes.
    * Imputación de datos: Rellenar el dato faltante usando una constante global (ej: "Desconocido"), medidas de tendencia central (media o mediana global o por clase), o métodos predictivos avanzados como regresión, k-vecinos más cercanos o árboles de decisión.
- Suavizado: El ruido es un error aleatorio o la varianza no deseada de una variable medida. Para eliminarlo se aplican técnicas de suavizado:
    * Binning: Particinoar los datos ordenados en recipientes de igual frecuencia o igual ancho y remplazar los valores por la media, mediana o límites **DEL BIN**.
    * Regresión: Ajustar una función lineal o múltiple para conformar la tendencia general.
    * Agrupamiento: Agrupar valores similares e identificar puntos aislados.
- Detección de valores atípicos: un valor atípico _(outlier)_ es una observación que se desvía tanto de las demás quue genera sospechas de haber sido generada por un mecanismo diferente. Se detectan mediante:
    * Métodos univariados: Evalúnan una sola variable usando el rango intercuartílico (IQR) en diagramas de caja (box-plot), z-score (distancias en desviaciones estándar respecto a la media) o z-score modificado (usando la mediana y el desvío estandar de la mediana).
    * Métodos multivariados: Evalúan espacios de múltiples dimensiones mediante distancias.
- Resolución de duplicados: Corrige discrepancias de representación de datos (formatos de fecha por ej), sobrecarca de campos, violaciones de reglas de negocio y elimina registros duplicados.

## 5.  ¿Qué diferencia existe entre ruido y outliers?

La diferencia fundamental entre ruido y valores atípicos radica en su origen y valor analítico:

- Ruido: Es una imperfección técnica, varianza o error de medición introducido durante la recolección, digitalización o transmisión de los datos (falla en sensores, errores de tipeo, etc.). **No contiene información útil** y suele estar disperso en múltiples registros.
- Valor atípico: Es una observación que se desvía tanto de las otras observaciones como para despertar sospechas de que fue generada por un mecanismo diferente. **No es necesariamente un error** pero representa un evento raro, excepcional o anómalo que rompe la distribución general del resto de la masa de datos.

## 6.  ¿Cómo puedo intervenir en una variable que presenta ruido?

Para intervenir en una variable que presenta ruido se aplican técnicas de suavizado de datos. El objetivo es eliminar o atenuar las fluctuaciones ruidosas para revelar la tendencia general de los datos. Las tres técnicas principales son: **binning**, **regresión** y **agrupamiento**.

- Binning: Es un método de suavizado **local**[^4] que consulta la "vecindad" de cada dato. Se realiza en tres pasos operativos:
    1. Ordenar los datos de menor a mayor.
    2. Particionar en recipientes (bins), ya sea por igual ancho (mismo rango de valores o igual frecuencia (misma cantidad de observaciones en cada bin).
    3. Suavizar el bin aplicando una regla de reemplazo:
        * Suavizado por medias del bin: se calcula la media del bin y se reemplaza cada valor del bin por ese promedio.
        * Suavizado por medianas del bin: cada valor se reemplaza por la mediana de su bin.
        * Suavizado por límites del bin: se identifican los valores mínimo y máximo del bin y cada dato dentro del bin se reemplaza por el límite más cercano.
- Regresión: Permite conformar o ajustar datos a una función matemática. En una lineal simple se busca la línea que mejor se ajusta a dos variables para suavizar una en función de la otra. En una lineal múltiple los datos se ajustan a una superficie multidimensional.
- Agrupamiento: Los datos se organizan en grupos o clusters de elementos similares. Las fluctuaciones o valores aislados que no pertenecen a ningún cluster consolidado se detectan como ruido o atípicos y se ajustan al centroide de su grupo más cercano.

## 7.  ¿Cuál es la técnica univariada para identificar valores atípicos?

Para identificar valores atípicos existen tres técnicas univariadas principales:

- Análisis de Box-Plot e IQR: técnica gráfica y no paramétrica basada en el resumen de cinco números (Mínimo, Cuartil 1 (q1), Mediana (q2), Cuartil 3 (q3), y Máximo). Se calcula el rango intercuartílico (Q3 - Q1) y se considera un valor atípico a toda aquella observación que caiga por fuera del intervalo definido por 1.5 X IQR respecto a los cuartiles 1 y 3.
- Z-Score: Evalúa cuántas desviaciones estándar se aleja una observación de la media muestral, asumiendo que los datos siguen una distribución normal. Zi = (xi - media) / desvio. Se utiliza habitualmente como "regla de oro" etiquetar como atípicos a los valores que superan un umbral de |Z| > 3 (más de 3 desvíos de la media en cualquier dirección).
- Z-Score Modificado: La media y el desvío pueden verse afectados por la presencia de los propios valores extremos. Para solucionar esto, se reemplaza la media por la mediana y el desvío estandar por la **Desviación Absoluta de la Mediana** (MAD). MAD = mediana{|xi - mediana|}. Z-Score mod formula: Mi = (0.6745 (xi - mediana)) / MAD. Como regla de oro acá los valores Mi > 3.5 son atípicos.

## 8.  ¿Qué desafíos presenta la integración de datos? ¿Cómo se puede abordar el problema de redundancia?

La integración de datos combina información proveniente de múltiples fuentes heterogéneas en un almacen único y coherente. Sus tres grandes desafíos son el problema de identificación de entidades, los conflictos en los valores de los datos y la redundancia. El problema de la redundancia se aborda principalmente mediante el uso de metadatos y el análisis de correlación **(_X²_ para datos nominales y Pearson/Covarianza para numéricos)**.

El problema de identificación de entidades consiste en emparejar entidades equivalentes del mundo real provenientes de distintas fuentes. Donde podemos encontrarnos con inconsistencia de nombre de atributos: un mismo atributo tiene nombres distintos en cada BD (id-cliente, cliente-id, cli-id, y demás).
Además existe la inconsistencia en los valores, donde la misma entidad real esta registrada de forma diferente (Nicolas Huici, Nicolas G. Huici, Huici Nicolas, etc.).

Otro problema de integración son los conflictos de valores, que ocurre cuando la misma entidad presenta valores disímiles entre fuentes debido a: Diferencia de escalas y unidades, un sistema registra el peso en kg y otro en lb. Diferencia de codificación o representación (parecido a la inconsistencia de valores), codificar el tipo de factura como "a", "b" directamente en una BD y como tipo: 1, 2 en otra.

Todos estos problemas además de los atributos duplicados o atributos repetidos sobrecargan y realentizan los algoritmos de minería.


Para abordar el problema de la redundancia debemos tener en cuenta que **un atributo es redundante si se puede derivar de otro atributo o conjunto de atributos** (ej: ingresos_anuales a partir de ventas mensuales) o debido a la falta de unificación de nombres. Y esto se oborda mediante ciertas estrategias:

- Análisis de correlación para datos nominales (_X²_ - Chi-Cuadrado): Evalúa si existe una relación de dependencia estadística entre dos atributos categóricos o cualitativos A y B:
    Se construye una _tabla de contingencia_ comparando los recuentos reales u observados (oij) contra los recuentos esperados (eij) si las variables fueran independientes. Y un valor de _X²_ elevado indica que las variables están fuertemente relacionadas, señalando que una de ellas es potencialmente redundante.
- Análisis de correlación para datos numéricos (Pearson y Covarianza):
    * Coeficiente de correlación de Pearson (r A,B) mide la fuerza y dirección de la relación lineal entre dos variables continuas. Si r A,B = 1 o -1, la correlación es perfecta, indicando que un atributo es completamente redundante respecto del otro.
    * Covarianza (Cov(A,B)) elavúa cómo serían conjuntamente dos atributos respecto a sus valores esperados. Si Cov(A,B) > 0, ambas tienden a crecer juntas. Si es 0, no existe variación lineal conjunta.
- Uso de metadatos y Herramientas ETL: permiten analizar de antemano el significados, tipo de dato, rango de valores permitidos y reglas de nulos de cada atributo, lo cual previene errores de integración de esquemas. Además las erramientas ETL como Kettle/Pentaho permiten orquestar la extracción, limpieza, resolución de duplicados y carga ordenada de datos hacia el almacén.

## 9.  ¿Cuál es el objetivo de la etapa de transformación de datos? ¿Qué procesos involucra esta etapa?

El objetivo de la etapa de transformación de datos es consolidar, escalar y reorganizar los datos preprocesados en formatos, rangos y niveles de abstracción adecuados para que los algoritmos de minería de datos funcionen con máxima precisión, eficiencia e interpretabilidad.

Los procesos involucrados son:

- Nomalización: Escalar los atributos numéricos continuos a un rango compacto y común, tiene como objetivo evitar que atributos con magnitudes numéricas grandes dominen la medición sobre atributos con rangos pequeños en algoritmos basados en distancias. Los métodos principales son min-max, z-score y escalado decimal.
- Discretización: Divide el rango de una variable continua numérica en un númera finito de intervalos o categorías discretas. Tiene como objetivo reemplazar valores continuos por etiquetas consisas que reducen el espacio de búsqueda y permiten la minería a múltiples niveles de abstracción. Pueden ser **No Supervisadas** (binning, histogramas, k-means) o **Supervisadas** (basadas en entropía o _X²_).
- Generación de jerarquías de Conceptos: Transforma atributos nominales o numéricos hacia niveles de abstracción conceptual superior mediante un ascenso en árbol de conceptos. Por ejeplo reemplazar direcciones específicas por calle, ciudad, provincia o país. Esto permite la extracción de patrones a múltiples niveles de granularidad.
- Agregación de datos: Aplica operaciones de resumen o consolidación sobre los datos (group by o construcción de cubos OLAP). Por ejemplo agrupar transacciones de ventas diarias individuales para obtener totales o mensuales.
- Construcción de atributos: Crear nuevos atributos derivados a partir del conjunto de variables originales para aportar mayor variabilidad y capturar relaciones implícitas. Por ejemplo: calcular superficie = ancho * alto.
- Suavizado: Elimina el ruido residual mediante binning, regresión o clustering.

## 10. ¿Cuál es el desafío de la reducción de datos? ¿Qué diferencias existe en la selección de atributos con respecto a la reducción de dimensionalidad?

El objetivo de la reducción de datos es obtener una representación simplificada del dataset que sea significativamente menor en volumen, pero que conserve la integridad analítica para producir resultados iguales o equivalentes a los del conjunto original. Su gran desafío es lograr este ahorro de espacio y velocidad sin que el costo computacional de la reducción supere el tiempo ahorrado en el minado, ni se pierda información crítica.

Dentro de esta etapa, la selección de atributos y la reducción de dimensionalidad abordan la disminución de variables de formas fundamentalmente distintas: la primera filta y conserva un subconjunto de las variables originales, mientras que la segunda transforma y combina las variables originales en un nuevo espacio de menor dimensión.


### El desafío de la reducción de datos
- El tiempo y recurso computacional invertido en ejecutar el algoritmo de reducción no debe superar el tiempo que se ahorra posteriormente durante la etapa de minería de datos.
- Se debe reducir la cantidad de datos (filas o atributos) minimizando la pérdida de información esencial, garantizando que los modelos o patrones descubiertos sean válidos y representativos.
- A medida que aumenta el número de atributos, los datos en el espacio multidimensional se vuelven extremadamente dispersos, lo que incrementa exponencialmente el tiempo de cómputo y degrada la efectividad de las métricas de distancia.

### Diferencias entre Selección de Atributos y Reducción de Dimensionalidad

| Criterio | Selección de Atributos | Reducción de dimensionalidad |
|----------|------------------------|------------------------------|
| **Mecanismo de acción** | Filtra y elimina atributos irrelevantes o redundantes | Transforma y combina las variables originales proyectándolas sobre un espacio numérico de menor dimensión |
| **Naturaleza de las variables** | Conserva un subconjunto de los atributos originales | Crea nuevas variables sintéticas/combinadas (ej: componenetes principales en PCA) |
| **Interpretabilidad** | Alta, ya que mantiene las variables originales con sus nombres y dominios, mas interpretable para el usuario final | Baja, ya que se pierde el significado directo de cada variable individual, ya que las nuevas dimensiones son combinaciones matemáticas de las anteriores |
| **Técnicas utilizadas** | _Forwar selection, Backward elimination, inducción por árboles de decisión_ | _Análisis de componentes principales (PCA), transformada discreta de wavelet (DWT)_ |

## 11. 🤓 📏**Situación 1**: Tenemos un conjunto de datos con 5 variables medidas en las siguientes unidades: metros, milímetros, kilogramos, pesos (\$) y grados celsius respectivamente. ¿Qué transformación sería necesaria realizar si mi tarea de KDD utiliza un algoritmo basado en distancias?

Para esta situación sin dudas _(imposible)_ se necesita aplicar la normalización de datos. Las variables están expresadas en unidades heterogéneas, al cambiar la unidad o manejar magnitudes distintas, los rangos numéricos varían (por ejemplo pesos puede ser cientos de miles, mientras que grados Celcius seguramente se mantenga entre un rango acotado de enteros de 2 cifras).
Y al usar un **algoritmo basado en distancias** seguramente pase que las variables de mayor magnitud dominen a las de menor magnitud.
Por lo que normalizando garantizamos que todas las variables tengan el mismo peso en el análisis.

En cuanto al método de normalización habría que ver la distribución y elegir entre min-max, z-score y escalado decimal.

## 12. 🤓 🍐🍏**Situación 2**: Si tenemos una variable definida en el dominio de los reales R y mi tarea de data mining solo trabaja con etiquetas. a) ¿Qué operación se debe realizar? b) ¿Qué tipo de variable consigo tener al finalizar el proceso?

La operación a realizar es la discretización, ya que una variable definida en R es una variable continua que adopta infinitos valores posibles. Si la tarea o el algoritmo de minería de datos requiere trabajar exclusivamente con etiquetas o categorías, no queda otra que aplicar discretización. Esta transformación divide el rango continuo de la variable en un conjunto finito de intervalos disjuntos y asigna una etiqueta conceptual o representante a cada intervalo.

La variable que se obtiene al finalizar es una variable **Discreta / Categórica** ya que se pasa de un dominio continuo infinito a uno finito de estados o símbolos.
Además depende como se efectúe la discretización puede ser el resultado: **Ordinal** si las etiquetas asignadas conservan el orden numérico de los rangos originales (ej: "bajo" < "medio" < "alto").
O puede ser **Nominal** si las etiquetas simplemente actúan como nombres cualitativos disjuntos.

## 13. 🥇**Bonus Track**: Si tengo dos variables de igual tipo que la resultante del punto 12 b) que vienen de diferentes fuentes de datos. ¿Cómo puedo saber si están relacionadas entre sí? ¿Con qué etapa de preprocesamiento se resuelve ese requerimiento? ¿Qué técnica es la indicada para esas variables?

Teniendo en cuenta que en el punto anterior dijimos que luego de discretizar nos queda una **variable discreta o categórica (nominal/ordinal)**, vamos a pensar en integrar dos bases de datos heterogéneas que contienen atributos de este tipo y debemos identificar si ambas variables representan la misma información o si están asociadas, y van a generar redundancia en el dataset unificado.

La etapa de procesamiento en la que se puede resolver esto es la de integración de datos, dentro de la tarea de _Detección y Eliminación de Redundancias_ mediante un análisas de correlación.

La técnia a utilizar es la **Prueba de Chi-Cuadrado(_X²_)**, que es la utilizada para este tipo de variables (discretas - nominales).

Para determinar estadísticamente si dos variables nominales A (con _c_ categorías) y B (con _r_ categorías) están asociadas, se sigue el siguiente procedimiento:

1. Se organiza el conteo conjunto de eventos en una matriz de _r_ x _c_, donde cada celda almacena la frecuencia observada o _ij_ de la combinación de la categoría de _i_ de A y la categoría _j_ de B.
2. Se calcula el valor que tendría cada celda si las dos variables fueran completamente independientes:
    $$e_{ij} = \frac{\text{count}(A = a_i) \times \text{count}(B = b_j)}{N}$$
    _donde N es el número total ed observaciones **del conjunto integrado**_
3. Cálculo estadístico _X²_:
    $$\chi^2 = \sum_{i=1}^{c} \sum_{j=1}^{r} \frac{(o_{ij} - e_{ij})^2}{e_{ij}}$$
4. Interpretación y Toma de Decisión
    Cuanto mayor sea el valor de _X²_, mayor es la discrepancia entre lo observado y lo esperado, indicando que las variables estan **fuertemente relacionadas**.
    **La prueba se evalúa con (_r_ - 1)x(_c_ - 1) grados de libertad a un nivel de significación (ej. alfa = 0.001).** [^5]

Si la prueba confirma que las dos variables categóricas provenientes de fuentes distintas están fuertemente correlacionadas, una de ellas se considera redundante. Y por lo tanto se elimina uno de los atributos redundantes para evitar sobrecargar los algoritmos de minería de datos, y como resultado mejorar la velocidad de cómputo y prevenir sesgos no deseados en el modelado

## Notas
[^1]: Entender un poco mejor cómo se detectan duplicados, y cómo el análisis de correlación puede llegar a eliminar redundancias. Si bien tengo la base práctica del TP, indagar un poco más sobre la teoría de _Por qué_ funciona.
[^2]: Entender bien cómo funciona, para qué sirve y cuándo puede _(y conviene)_ aplicarse PCA. Además entender bien la diferencia entre red. de dimensionalidad y de numerosidad.
[^3]: No me convence la respuesta, debería repasar un poco mejor de qué se trata dicha etapa en _Han_.
[^4]: Confirmar si la localidad hace referencia a que la medida se calcula con los datos dentro del bin en lugar de los globales, o hace referencia a otra cosa.
[^5]: No termino de comprender qué son los grados de libertad, investigar un poco ese tema.

> [Bibliografía sugerida]:

- Data Mining. Concepts and Techniques. Jiawei Han and Micheline Kamber.
  Capítulo 2.

- Brown, M. L., & Kros, J. F. (2003). Data mining and the impact of
  missing data.Industrial Management & Data Systems, 103(8), 611-621.

