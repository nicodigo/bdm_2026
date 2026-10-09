# GUÍA DE LECTURA I: Proceso de KDD

## 1.  Definir Knowledge Discovery in Databases (KDD)
## 2.  ¿Qué significa patrones válidos y novedosos en la definición de KDD?

KDD es el **descubrimiento de conocimiento en bases de datos**, es el proceso completo el cual **es interactivo e incremental** de transformar datos crudos y masivos en patrones válidos, novedosos, potencialmente útiles y comprensibles. 
**Importante**: Su principil objetivo es resolver un problema de sobrecarga de datos, para extraer conocimiento estratégico para la toma de decisiones.

De acuerdo a la definición de _Fayyad (1996)_ es el proceso no trivial de identificar patrones válidos, novedosos, potencialmente útiles y finalmente comprensibles a partir de los datos.

- Proceso: consta de múltiples etapas (preparación, limpieza, transformación, minado, evaluación) y no de un solo paso o algoritmo aislado.
- No trivial: la extracción no se logra mediante cálculos sencillos o predefinidos (como un promedio o un COUNT en SQL), sino que requiere de algoritmos de búsqueda, inferencia o modelado computacional.[^1]
- Válidos: los patrones descubiertos deben mantenerse ciertos sobre datos nuevos o futuros con un determinado grado de certeza. Interpretado como que los patrones extraídos no sirven de nada si solo "funcionan" con el set de datos utilizados y con ninguno más.
- Novedosos: deben aportar relaciones o hallazgos que eran previamente desconocidos para el sistema o el usuario. _Un poco obvio pero importante._
- Potencialmente útiles: deben ser aplicables para lograr un beneficio práctico (generalmente en cuanto a la tomas de decisiones). [^2]
- Comprensibles: El resultado final debe ser interpretable por seres humanos y no una "caja negra" incomprensible. _Obvio pero importante_ lo intepreto como que un ser humano debe poder comprender _de donde sale(?)_ el patrón.

## 3.  ¿Cuales son las características del proceso de KDD?

Las principales características de manera consisa es que se trata de un marco de trabajo **iterativo, interactivo, multietapa e interdisciplinario**. No es una ejecución algorítmica aislada, es una metodología de trabajo guiada por el usuario.

Basándomen en _Fayyad (1996)_ voy a expandir sobre lo que trata cada cosa:

- Iterativo: No se trata de una secuencia lineal inflexible. Si bien cuenta con una serie de etapas ordenadas, el proceso de KDD también contiene múltiples bucles de retroalimentación entre sus fases. Si durante la evaluación de los patrones o al finalizar [^3] una fase se detecta un fallo o no se está conforme con los resultados hasta el momento siempre se puede volver o al inicio o a etapas anteriores y ajustar o corregir.
- Interactivo: Requiere indispensablemente la participación del ser humano (analista o experto del dominio). El usuario debe intervenir como mínimo para: definir los objetivos del negocio, aportar conocimiento previo, seleccionar atributos de interes y configurar los umbrales de los algoritmos de minería de datos.
- Multietapa: Como se dijo anteriormente, comprende una serie de fases o etapas ordenadas que abarcan desde el entendimiento del problema hasta la consolidación del conocimiento. Fayyad habla de un proceto de 9 pasos, mientras que Han lo condensa en 7. _Por lo que entiendo que no tiene una separación formal definida el proceso._
- Interdisciplinario: Nace de la convergencia de diversas disciplinas científicas y tecnológicas. Combina la gestión de los sistemas de bases de datos y almacenes de datos, con el modelado probabilístico de la estadística y el aprendizaje automático de la IA.
- Guiado por la evaluación de interés: el objetivo no es simplemente extraer patrones o todos los patrones posible, sino filtrar el universo de patrones para entregar aquellos que aportan valor.
- Orientado a la escalabilidad: Diseñado para operar sobre volúmenes masivos de datos y alta dimensionalidad.

Características principales: Iterativo, Interactivo y Multietapa.

## 4.  ¿Cuales son los pasos del proceso KDD?

Como mencioné anteriormente la bibliografía utilizada discrepa en la cantidad de etapas, pero no tanto en el contenido total o las tareas totales del proceso.
Voy a exponer las 9 etapas de _Fayyad_ y luego comparar brevemente con cómo _Han_ condensa este proceso, pero ambos organizaen el proceso en tres bloques: Preprocesamiento, Minería de datos y Evaluación.

**El flujo de 9 etapas de _Fayyad_:**

1. Comprensión del dominio y conocimiento previo: Definir los objetivos del proyecto desde la perspectiva del usuario o negocio y relevar el conocimiento existente. Entiendo que es sumamente importante para comprender bien qué estamos buscando exactamente en los datos.
2. Creación del conjunto de datos objetivo: Seleccionar el subconjunto de datos, registros o atributos sobre los cuales se realizará el descubrimiento.
3. Limpieza y preprocesamiento de dato: Remover ruido o datos inconsistentes, decidir estrategias para gestionar valores faltantes y tratar secuencias temporales.
4. Reducción y transformación de datos: Encontrar características útiles mediante reducción de dimensionalidad [^4], selección de atributos o transformaciones invariantes.
5. Elección de la tarea de minería: Se busca alinear el objetivo del KDD con una tarea específica de minería, como son clasificación, agrupamiento, regresión, sumarización.
6. Análisis exploratorio y selección de algoritmos: Elegir el algoritmo específico de la minería de datos y definir sus parámetros. [^5]
7. Minería de datos: Acá entiendo que es específicamente la ejecución del algoritmo elejido. Surgen patrones de interés en las formas de representación seleccionadas (reglas, árboles de decisión, etc.). _Supongo que más adelante veremos estas formas_.
8. Interpretación y evaluación de patrones: Evaluar los patrones extraídos mediante métricas cuantitativas y cualitativas, con herramientas de visualización.
9. Acción sobre el conocimiento: Usar el conocimiento obtenido directamente, incorporarlo para alimentar otros sistemas, o resolver conflictos con conocimiento previo.

**En cuanto a la condensación de los 7 pasos de _Han_**:
Se agrupa el flujo en:
- Preprocesamiento: 1. Limpieza, 2. Integración, 3. Selección, y 4. Transformación de datos.
- Minería: 5. Minería de datos.
- Postprocesamiento: 6. Evaluación de patrones, y 7. Presentación del conocimiento.

## 5.  ¿Qué son las preguntas/objetivos de KDD? ¿Y cuán importantes son para la metodología?

Los objetivos del KDD representan la meta final del análisis desda la perspectiva del usuario o del negocio y pueden clasificarse en dos categorías: **verificación** (confirmar una hipótesis previa y **descubrimiento** (identificar patrones nuevos, divididos en predictivos y descriptivos).
Formular correctamente el problema y la pregunta de investigación es la fase más crítica de la metodología, ya que va a condicionar todas las decisiones técnicas posteriores.

Según _Fayyad_ y _Han_ los objetivos de un proceso de KDD están clasificados en las dos categorías previamente mencionadas, procedo a expadirlas:

1. Objetivos de Verificación: El sistema se limita a comprobar o validar una hipótesis planteada explícitamente por el usuario. Por ejemplo: probar si un aumento en el límite de crédito incrementa el riesgo de impago.
2. Objetivos de Descubrimiento: Se busca patrones o modelos de manera autónoma sin requerir una hipótesis previa. Se subdivide en:
    - Predictivos: Utilizan atributos conocidos para estimar valores desconocidos o futuros de otras variables de interés **(Clasificación o Regresión)**.
    - Descriptivos: Se centran en encontrar patrones comprensibles por el ser humano que caractericen la estructura o propiedades de los datos analizados **(Clustering, Asociación o Sumarización)**.

**La importancia de definir bien**
Definir con precisión el objetivo es la base de toda la metodología en KDD:

- Determina la preparación de los datos: Define qué subconjunto de datos seleccionar, cómo lipiarlos, y qué transformaciones o reducciónes de atributos aplicar.
- Guía la selección del algoritmo de Data Mining: como mencioné antes un objetivo predictivo requiere algoritmos supervisados (como árboles de decisión), mientras que uno descriptivo requiere métodos no supervisados (como k-means). [^6]
- Evita la minería a ciegas: expuesto como _data fishing_ en la bibliografía, sería como buscar en los datos a ver si se encuentra algo interesante, sin un objetivo concreto. Generalmente esto resulta en patrones irregulares o inválidos.
- Ahorro de recursos: Lo que entiendo que quiere decir _Fayyad_ con esto es que por más óptimo que sea el algoritmo de minería, si no sabe que está buscando no vamos a obtener buenos resultados en un tiempo aceptable. Conviene invertir más tiempo en definir el objetivo y así saber qué buscar.

**Conclusiones**
Podemos dividir los objetivos en verificación y descubrimiento (predictivos y descriptivos), el primero valida una hipótesis, el segundo busca patrones de interés.
Los objetivos guían todo el proceso por lo que son sumamente importantes, y conviene gastar tiempo en definirlos bien.
Ejecutar algoritmos de minería sin objetivos claros suele generar resultados pobres.

## 6.  ¿Cómo relaciona (Han & Kamber) las tareas de preprocesamiento y transformación explicadas por (Fayyad)?

_Han & Kamber_ sistematizan las tareas de preparación de datos expresadas por _Fayyad_ (selección, limpieza, reducción y transformación) englobándolas dentro de una categoría llamada **prepocesamiento de datos**. Además incorporan de forma explícita la **integración de datos** como una tarea fundamental previa al minado para consolidar fuentes heterogéneas.

_Fayyad_ distribuye la preparación de los datos en tres pasos secuenciales: 2. Selección, 3. Limpieza y preprocesamiento, 4. Reducción y transformación. Lo cuáles expuse más a detalle en una respuesta anterior.

_Han_ en lugar de tratar la limpieza, selección y transformación como pasos desconectados, los unifica bajo cuatro grandes tareas del **Preprocesamiento de datos**:
1. Limpieza: corresponde al paso 3 de _Fayyad_, rellena valores faltantes, suaviza ruido y corrige inconsistencias.
2. Integración: Tarea enfatizada para combinar múltiples bses de datos, o fuentes heterogéneas bajo un esquema unificado (data warehouse).
3. Selección: corresponde al paso 2 de _Fayyad_. Recupera los atributos y registros relevante para el objetivo de análisis.
4. Transformacion y reducción: Agrupa y amplía el paso 4 de _Fayyad_ mediante escalado o normalización, agregación, discretización y reducción de volumen o atributos (PCA), preparando los datos para ser procesados eficientemente por los algoritmos de minería.

_Han_ destaca que estas tareas no son fases rígidas simplemente aisladas, sino que son operaciones que se solapan y colaboran entre sí.

## 7.  ¿En qué consiste la etapa de Data Mining?

Es la fase algorítmica del proceso de KDD, en la cuál se utilizan algoritmos especializados para explorar **datos preprocesados** y extraer patrones o modelos de interés.

Una vez que los datos fueron preprocesados la etapa de Data Mining se enfoca en resolver un problema de optimización computacional: explorar un espacio de búsqueda masivo para enumerar patrones de interés bajo restricciones de eficiencia. Lo que derivo de esta definición es que: el espacio de búsqueda es **muy** grande, lo que causa que tengamos que sacrificar cierta cobertura de patrones en favor de tiempo o eficiencia, ya que no sería computacionalmente posible (o conveniente) buscar **todos** los patrones de interés.

Según _Fayyad_ cualquier algoritmo de minería de datos se construye operacionalmente combinando tres componentes primarios:

1. Representación del modelo: Lenguaje o Estructura formal utilizada para expresar los patrones descubiertos. (reglas lógicas IF-THEN, árboles de desición, ecuaciones, clusering).
2. Evaluación del modelo: Es la función de ajuste o métrica cuantitativa empleada para medir qué tan bien responde un patrón a los objetivos planteados.
3. Método de búsqueda: Es la estrategia algorítmica utilizada para explorar el espacio de modelos y parámetros.

A su vez estas taresa se dividen en dos grandes clases:
- Predictivas: Utilizan variables conocidas para estimar o predecir valores futuros o desconocidos de otras variables **(Clasificación y Regresión)**.
- Descriptivas: Identifican propiedades generales y relaciones inherentes comprensibles en los datos analizados **(Reglas de asociación, Clustering, Detección de outliers)**.

## 8.  ¿Cuales son las principales tareas de Data Mining?

Las tareas son las metas algorítmicas específicas que se aplican para extraer patrones sobre los datos.
Tanto _Fayyad_ como _Han_ clasifican las tareas principales de minería de datos de la siguiente manera:

**Tareas Predictivas:**
Tienen como objetivo construir un modelo a partir de datos conocidos para hacer deducciones o pronósticos sobre datos futuros o no etiquetados.
1. Clasificación: Construir un modelo o función (classifier) a partir de un conjunto de datos de entrenamiento etiquetados para predecir la clase categórica o discreta de nuevos objetos de datos.
2. Regresión: Predicción numérica que modela funciones de valor continuo para estimar cantidades numéricas desconocidas o variables con orden implícito.

**Tareas Descriptivas:**
Tienen como objetivo descubrir patrones intrínsecos y propiedades generales comprensibles para el ser humano sobre el conjunto de datos analizado.
1. Clustering: Organiza objetos de datos no etiquetados en grupos homogéneos, maximizando la similitud entre objetos del mismo grupo y minimizando la similitud con objetos de otros grupos.
2. Análisis de Outliers: Identifica objetos de datos que se desvían de manera significativa del comportamiento esperado o de la distribución general del resto del conjunto de datos.
3. Caracterización y discriminación: Sintetiza las características generales de una clase de datos de interés o compara las propiedades de dos o más clases contrastantes.

**Importante y spoiler:** la clasificación exige obligatoriamente un atributo objetivo/etiqueta, mientras que el clustering opera sobre todo el espacio de atributos sin variable dependiente.

## 9.  ¿Cuál es la diferencia entre clasificación y regresión? ¿Y entre estas dos y clustering?

La clasificación y la regresión son tareas predictivas de aprendizaje supervisado[^7] que estiman un valor a partir de datos etiquetados: la clasificación predice etiquetas categóricas discretas y la regresión predice valores numéricos continuos.
En contraste el agrupamiento o clustering es una tarea descriptiva de aprendizaje no supervisado que descubre grupos naturales en datos no etiquetados basándose en la similitud intrínseca entre objetos.

**Supervisado vs. No supervisado:**
- Supervisado: Requiere obligatoriamente un conjunto de entrenamiento (training set) compuesto por registros que incluyen una variable respuesta o etiqueta de clase conocida por el algoritmo.
- No supervisado: El clustering opera sobre conjunto de datos sin etiquetar, por lo que debe inferir la estructura subyacente sin la guía de una respuesta previa.

**Clasificación:**
Construye un modelo a partir de muestras conocidas para predecir a qué categoría cualitativa, discreta y sin orden numérico pertenece un nuevo registo. Por ej: Aprobado/Desaprobado, Riesgo Alto/Riesgo Bajo.

**Regresión:**
También denominada predicción numérica, modela funciones matemáticamente continuas para estimar valores cualitativos continuos u ordenados. Por ej: estimar ingresos monetarios, temperaturo o edad exacta.

**Agrupamiento:**
Particiona una colección de objetos en subconjuntos o grupos siguiendo el principio fundamental de maximizar similitud intra-grupo y minimizar similitud inter-grupo.

## 10. Dar un ejemplo de aplicación donde se puedan usar cada una de las tareas de Data Mining.

**Sistema Integrado de Gestión de Clientes en un Comercio Electrónico (** **e-commerce** **/ tienda** **AllElectronics** **)**:

- **Clasificación (Predictiva - Discreta)**: Determinar si un cliente que visita la web tiene una alta probabilidad de responder a una campaña promocional de computadoras (asignando las etiquetas categóricas `"Respuesta Alta"`, `"Respuesta Media"` o `"Sin Respuesta"`).
- **Regresión (Predictiva - Numérica)**: Pronosticar el **monto exacto en pesos ($)** o ingresos que generará cada cliente en las ventas del próximo trimestre en función de sus compras históricas y presupuesto publicitario.
- **Clustering (Descriptiva - No supervisada)**: Analizar la masa total de clientes registrados y agruparlos en 3 o 4 perfiles sociodemográficos o de hábitos de navegación homogéneos (sin conocer las categorías de antemano) para asignar un gestor comercial dedicado a cada segmento.

## NOTAS
[^1]: Estaría bueno buscar qué son los algoritmos mencionados, para tener una mejor idea de que se trata KDD.

[^2]: Tal vez más adelante entiendo mejor este concepto. Investigar un poco más sobre cómo un modelo de minado evalúa la útilidad de un patrón. O lo evalúa el usuario al completar el minado?

[^3]: No estoy seguro de que solo se pueda evaluar resultados al finalizar una fase o si también se pueda a "mitad de fase". Es un tecnicismo, pero lo dejo planteado.

[^4]: Entiendo el concepto de reducción de dimensionalidad, no entiendo del todo lo que abarcan las técnicas de dicho concepto, profundizar de ser necesario.

[^5]: Asumo por esta definición que por cada tarea de minería existen más de un algoritmo, y además cada algoritmo está definido con un conjunto de parámetros que afectan al output.

[^6]: Importante remarcar que: ** Objetivo Predictivo -> Algoritmo Supervisado** y ** Objetivo descriptivo -> Algoritmo NO Supervisado**

[^7]: Investigar más a detalle que significa y qué implica el aprendizaje supervisado vs. el no supervisado.

> [Bibliografía:]

- Fayyad, U., Piatetsky-Shapiro, G., & Smyth, P. (1996). From data
  mining to knowledge discovery in databases. AI magazine, 17(3), 37.

- Jiawei Han, Micheline Kamber, Jian Pei. 2011. Tercera edición. Data
  Mining: Concepts and Techniques.

- Daniel T. Larose. 2014. Segunda edición. Discovering Knowledge in
  Data: An Introduction to Data Mining.
