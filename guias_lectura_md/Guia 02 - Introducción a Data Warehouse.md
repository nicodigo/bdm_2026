# GUÍA DE LECTURA II: DATA WAREHOUSE

## 1. Defina Data Warehouse (DW):
    a.  ¿Qué significa que está orientado al sujeto?
    b.  ¿Es correcto definir un DW como una colección de datos integrados? ¿Por qué?
    c.  Explique la siguiente afirmación teórica: "Es una colección de datos variables en el tiempo y no volátiles". ¿Esto siempre es así?
    d.  ¿Cuáles son los objetivos de un DW? ¿Y sus beneficios?

Un data warehouse se puede definir como **una colección de datos orientada a un tema, integrada, variable en el tiempo y no volátil, organizada para el apoyo al proceso de toma de decisiones de la gerencia**.
Es un repositorio de información unificado y consolidado que se mantiene separado físicamente de las bases de datos operacionales de la organización. Su propósito es almacenar datos históricos transformados que sirven como plataforma para la toma de decisiones estratégicas mediante herramientas analíticas.

**Orientado al sujeto** significa que el DW está organizado en torno a los grandes temas o conceptos clave del negocio (por ejemplo: clientes, productos, ventas, proveedores).
A diferencia de los sistemas operacionales tradicionales (OLTP), que se diseñan orientados a procesos o aplicaciones particulares para la gestión de transacciones del día a día (como dar de alta una factura o actualizar un usuario), el DW se enfoca en el modelado y análisis de los datos para los tomadores de decisiones, proporcionando una visión concisa de cada tema y excluyendo los datos transaccionales irrelevantes para el análisis.

**Si considero correcto definir un DW como una colección de datos integrados** porque un DW se construye extrayendo y reuniendo datos provenientes de múltiples fuentes heterogéneas. Durante esa integración (utilizando ETL), se aplican técnicas de limpieza y homogenización para garantizar la coherencia semántica, por lo que los datos se transforman y convierten antes de set insertados en le DW.

**Variable en el tiempo** significa que los datos almacenados proveen una perspectiva histórica amplia. Cada estructura de clave dentro del DW contiene de forma explícita o implícita, un elemento de tiempo para permitir el análisis de tendencias y evolución a lo largo del tiempo.
**No volátil** significa que los datos, una vez transformados e ingresados al DW, no se modifican ni se eliminan mediante operaciones transaccionales comunes (UPDATE, DELETE).
En la teoría se habla de que esto siempre se cumple para garantizar la estabilidad y trazabilidad analítica. Sin embargo en la práctica existen excepciones: ante errores detectados en la fuente o transacciones fuera de termino pueden aplicarse métodos de actualización o _refresh_. Además el almacenamiento no es infinito, por lo que los datos que superan el horizonte histórico fijado por la organización se purgan o se desplazan a almacenamiento terciario o de archivo.

**Los objetivos principales** son: dar soporte a la toma de desiciones gerenciales, separar el entorno operacional del analítico desacoplando cosultas complejas de análisis del procesamiento diario de transacciones, y consolidar múltiples fuentes heterogéneas mediante un esquema de datos unificados.
**Los beneficios clave** son: que facilitan el análisis de patrones y comportamientos sin requerir desarrollos manuales complejos sobre bases operativas, otorgan una perspectiva unificada de clientes y productos a través de todas las sucursales y líneas de negocio, y mediante estructuras multidimensionales y vistas precalculadas permiten responder consultas complejas de agregación en cuestión de segundos.

## 2. ¿Cómo está conformado un Data Warehouse? Explique los diferentes elementos que lo conforman.

Un DW está conformado por una arquitectura multicapa estructurada en tres niveles principales: capa de almacenamiento, capa analítica OLAP y capa de clientes/front-end). Esta arquitectura es alimentada desde fuentes de datos heterogéneas mediante un proceso de ETL que prepara los datos en un área intermedia antes de organizarlos en tablas de hechos y dimensiones dentro del almacén central.

### Arquitectura multicapa
- Capa inferior - Servidor de Base de Datos: repositorio físico donde residen los datos consolidados (casi siempre un sistema gestor de base de datos relacionales optimizado para la lectura masiva). Además se cuenta con un repositorio de metadatos que almacena la definición del esquema del DW y herramientas de monitoreo para el rendimiento y la carga.
- Capa intermedia - Serivdor OLAP: Es el motor analítico que modela la información en estructuras multidimensionales (cubos de datos) y procesa las operaciones de agregación. Se puede implementar mediante tres arq. físicas:
    * ROLAP (Relational OLAP): utiliza un DBMS relacional extendido para mapear operaciones multidimensionales sobre tablas estándar.
    * MOLAP (Multidimensional OLAP): utiliza motores de almacenamiento basados en arreglos multidimensionales para lograr tiempos de respuesta ultra rápidos.
    * HOLAP (Hybrid OLAP): combina las dos anteriores ROLAP para grandes volúmenes y MOLAP como una especie de caché.
- Capa superior - Front end: Representa las herramientas de acceso directo para los usuario finales y analistas corporativos. Herramientas de consulta y reportes, Interfaces de análisis OLAP, Herramientas de minería de datos.

### Componentes de captura e integración
Mencionar también que en el proceto ETL se cuenta con las fuentes de datos que suelen ser los sistemas transaccionales OLTP. Un área de preparación o _staging_ que es una zona física de almacenamiento temporal donde los datos extraídos se van procesando (limpieza, deduplicación y transformación) antes de ser volcados al DW para no sobrecargar el DW. Y como otro componente el mecanismo de ETL que es el software encargado de la extracción, limpieza, transformación y carga al DW.

## 3. ¿Cuáles son los diferentes esquemas que puede ostentar un DW? Explíquelos brevemente.

El modelado multidimensional en un DW se representa principalmente a través de tres esquemas fundamentales: **De estrella**, **Copo de nieve** y **Constelación de hechos**. La elección entre uno u otro depende del nivel de normalización deseado, el alcance del proyecto (data mart vs. DW empresarial) y la prioridad dada al rendimiento de las consultas.


### Esquema de Estrella
Posee una gran tabla de hechos central sin redundancia que contiene las medidas cuantitativas del negocio y las claves foráneas hacia las dimensiones. A su alrededor se ubican en forma radial las tablas de dimensiones.

Cada dimension se representa mediante **una sola tabla no normalizada**. Introduce cierta redundancia interna en los atributos descriptivos de las dimensiones a cambio de simplificar la estructura.
Su ventaja clave es que al no requerir normalización en las dimensiones, reduce la cantidad de uniones (JOINs) necesarias para resolver las consultas analíticas, logrando un rendimiento muy eficiente. **Es el modelo más popular para Data Marts**.


### Esquema copo de nieve
Es una variante del esquema en estrella en la cual las tablas de dimensiones están normalizadas. Al descomponer las dimensiones en sub-tablas para reflejar las jerarquías explícitas, el diagrama adopta una forma similar a un copo de nieve.

Su ventaja clave es que reduce la redundancia de datos dentro de las dimensiones y ahorra espacio de almacenamiento.
Sin embargo el ahorro de espacio en las dimensiones suele ser insignificante comparado lon el volumen de la tabla de hechos. Además requiere realizar un número mayor de JOINs entre tablas para ejecutar una consulta, lo que puede degradar la velocidad y el rendimiento del sistema.


### Esquema de constelación
Consiste en una colección de múltiples tablas de hechos que comparten tablas de dimensiones entre sí.

Permite modelar múltiples procesos de negocio o temas interrelacionados dentro de la misma organización (ej: conectar Ventas con Envíos compartiendo dimensiones Tiempo, Producto y Ubicación).
Es la estructura estándar y recomendada para un DW Empresarial, dado su alcance integral y multifuncional.

## 4. Esquematice la arquitectura de un Sistema de toma de decisiones basado en un DW. Explique la transición dato -> información -> conocimiento en este contexto.

El esquema es el mencionado anteriormente (Fuentes de datos -> ETL -> Data Staging -> DW server (bottom) -> OLAP server (mid) -> Front-end.

### La transición dato -> información -> conocimiento

- Dato: Son los hechos crudos, cuantitativos o cualitativos, descontextualizados, atomizados e individuales almacenados en las bases de transaccionales operacionales OLTP. Corresponde con los registros de bajo nivel producidos de las operaciones diarias.

- Información: Es el resultado de estructurar, integrar, limpiar, agregar y contextualizar los datos crudo. Se logra cuando el proceso ETL consolida los datos en el DW y el servidor OLAP genera los cubos de datos. Permite observar resúmenes a distintos niveles de abstracción.

- Conocimiento: Es la comprensión profunda de patrones, tendencias, reglas de asociación y modelos inferidos a partir de la información. Se genera en la capa superior (Front-end) mediante técnicas de Data Mining y la evaluación de patrones descubiertos.

## 5. ¿Qué es un Data Mart?

Un Data Mart es un subconjunto de datos consolidado que se enfoca en un tema o área funcional específica de la organización (como Ventas, Markeking o Finanzas), diseñado para satisfacer las necesidades de análisis de un grupo particular de usuarios.

Puede ser **Dependiende** que se alimenta directamente a partir de los datos consolidados de un DW central y garantiza coherencia semántica entre departamentos manteniendo la _fuente de verdad_ o **Independiente** que se alimenta directamente de los sistemas operacionales OLTP, archivos locales o fuentes externas, sin pasar por un DW central, por lo que es más rápido y económico de comenzar a implementar pero a largo plazo genera "islas de información" aisladas.

## 6. Explique y relacione las diferentes áreas de un DW:

### a.  Fuentes de datos.
Son los sistemas origen que generan y custodian la información cruda de la organización. Ingcluyen BD relacionales OLTP, archivos planos, fuentes externas, registros transaccionales y APIs. 
Son altamente heterogéneas, manejan diferentes sistemas gestores de bases de datos, formatos, representacionse, escalas y protocolos de comunicación. La granularidad de la información es alta, orientada a operaciones diarias.

### b.  ETL/Staging Area.
Es el entorno de procesamiento donde el software ETL almacena los datos antes de volcarlos al repositorio central.
La etapa de extracción captura los datos de las fuentes de datos de forma incremental para no sobrecargar los sistemas operacionales.
La _Staging area_ es una zona física de almacenamiento intermedio aislada, donde se ejecutan las tareas de limpieza y transformación sin interferir con las operaciones OLTP.
Luego la etapa de carga o refresco escribe los datos ya consolidados en los almacenes de datos.

### c.  Almacenamiento.
Repositorio físico **consolidado, histórico y no volátil** donde residen los datos limpios y transformados.
Los datos se organizan bajo modelos multidimensionales mediante tablas de hechos y tablas de dimensiones, estructuradas en esquemas de estrella, copo de nieve o constelación.

### d.  Análisis de la información.
Es la fase final de la explotación analítica donde los analistas, gerentes y ejecutivos consumen la información para la toma de decisiones estratégicas.
Toma la esructura de almacenamiento y permite a los usuarios navegar interactivamente la información a distintos niveles de abstracción para convertir datos históricos en conocimiento accionable.

$$\text{Fuentes de Datos} \xrightarrow[\text{Extracción}]{\text{Captura}} \text{ETL / DSA} \xrightarrow[\text{Transformación}]{\text{Limpieza e}} \text{Almacenamiento (DW)} \xrightarrow[\text{OLAP / Mining}]{\text{Explotación}} \text{Análisis (Decisiones)}$$

## 7. En ETL, ¿cuáles son los principales desafíos de la etapa de Extracción?

La etapa de extracción es el primer paso del flujo de integración y consiste en obtener los datos desde múltiples fuentes operativas heterogéneas internas o externas. Aunque parece una simple lectura de datos, representa un gran desafío técnico y organizacional en la construcción de un DW.

### Desafíos
- Minimizar la sobrecarga en los sistemas fuente: las fuentes de datos suelen ser sistemas OLTP que están procesando transacciones en tiempo real. Realizar consultas pesadas de extracción sobre las DB operativas puede saturar los recursos y degradar el rendimiento o la disponibilidad de los sistemas de producción.
- Restricciones políticas y técnicas: tanto por motivos de seguridad como por políticas internas, los administradores de bases de datos de las fuentes de datos suelen oponerse a modificar la configuración, agregar triggers o instalar componentes pesadosen sus servidores.
- Heterogeneidad tecnológica y de formatos: cada fuente posee sus propias características, diferentes motores de BD, distintos SO, variados protocolos de comunicación e incluso formatos semiestructurados o no estructurados (csv, txt, excel, json).
- Complejidad en la captura de datos modificados: salvo en la carga inicial, solo se deben extraer los datos que han cambiado o se han añadido desde la última extracción. Identificar eficientemente las novedades sin alterar la fuente exige recurrir a mecanismos como columna de auditoría, logs, marcas de tiempo, etc.

## 8. La etapa de transformación contempla atender tres problemáticas ¿Cuáles son? ¿Cuán complejas pueden ser según su criterio?

Las tareas de limpieza y transformación abordan los conflictos de los datos distribuidos en tres niveles de problemática:

1. Nivel de esquema: Uso del mismo nombre para objetos diferentes o nombres distintos para el mismo objeto del mundo real entre las fuentes de datos. Diferencias en cómo se representan las entidades y relaciones en los modelos conceptuales de las fuentes.
2. Nivel de Registro: presencia de registros duplicados o tuplas contradictorias provenientes de distintas fuentes que representan la misma entidad o transacción real.
3. Nivel de Valor: problemas técnicos de representación de bajo nivel, como inconsistencias en formatos de fecha, escalas y unidades de medida o manejo de nulos.

En la práctica la etapa de transformación representa una gran parte del timepo y esfuerzo total de un proyecto de Data Warehousin, y la complejidad varía según el nivel:
- Nivel de valor: suele ser la más sencilla. Se resuelve mediante reglas de mapeo, funciones de conversión de tipos, expresiones regulares y diccionarios de traducción.
- Nivel de registro: identificar tuplas duplicadas o contradictorias exige aplicar algoritmos de deduplicación y reglas de desambiguación. Además de decidir qué fuente prevalecen en caso de conflicto.
- Nivel de esquema: desafío más complejo porque no solo es técnico, sino semántico y organizacional. Exige negociar con los responsables del negocio para definir un único diccionario de metadatos y unificar criterios heterogéneos.

## 9.  ¿Qué rol tienen los metadatos en la arquitectura multicapas de DW?

Los metadatos se definen como _datos sobre los datos_. Dentro de la arquitectura multicapa del DW, el repositorio de metadatos se ubica físicamente en la capa inferior. Sin embargo, cumple un rol transversal y crítico que conecta y gobierna todas las capas del sistema, desde la extracción en los sistemas fuente hasta la explotación analítica por parte de los usuarios finales.

### Roles de los metadatos en la arquitectura.

- Guía y mapa para el proceso de ETL (fuentes -> almacenamiento): define las reglas de mapeo entre los sistemas operacionales y el DW. Almacena las especificaciones de extracción, las funciones de limpieza, las transformaciones de código/unidad, los valores por defecto y las reglas de refresco y purga de datos.
- Definición de la estructura multidimensional: contiene la descripción formal del diseño del almacén, el esquema, las tablas de hechos, las dimensiones, las jerarquías de conceptos y las vistas. Almacena los algoritmos de sumarización y agregación, especificando cómo se calculan las métricas a distintos niveles de granularidad.
- Directorio de navegación para el usuario final: actúa como un catálogo o directorio interactivo para que los analistas y tomadores de decisiones conozcan qué información existe en el almacén, dónde se encuentra y cómo consultar los cubos.
- Gestión operacional, auditoría y linaje de datos: mantiene el historial de migraciones y transformaciones aplicadas a cada dato, permitiendo rastrear su origen.
- Metadatos del negocio: Mantiene el glosario de términos y definiciones propias de la organización, la propiedad/responsabilidad sobre los datos y las políticas de seguridad y control de acceso.

Los metadatos son el "cerebro" de la arquitectura del DW. Sin ellos, los usuarios no sabrían qué datos existen ni cómo interpretarlos, el proceso ETL no sabría cómo transformar las fuentes operativas, y el servidor OLAP no sabría cómo construir los cubos multidimensionales.

## 10. ¿En qué consiste el proceso de carga? ¿Qué relación existe entre el DSA y esta tarea?

El proceso de carga es la fase final del flujo ETL en la cual los datos previamentes extraídos, limpiados y transformados se escriben e insertan en las estructuras multidimensionales (tablas de hechos y dimensiones) del DW.
Es el paso operativo que materializa la información en el repositorio final para ser consultada por las herramientas analíticas (OLAP y Data Mining). Involucra dos componenetes estructurales principales: la carga de tablas de dimensiones y la carga de tablas de hechos.
Las tareas en esta fase son:
- Clasificación y agregación: ordenar y resumir los datos al nivel de granularidad requerido por las tablas de hechos.
- Construcción de estructuras de acceso: crear índices y particiones para optimizar el rendimiento de las consultas.
- Cálculo de vistas: precalcular totales y agregaciones en cubos multidimensionales.
- Comprobación de integridad: validar claves primarias, claves foráneas e integridad referencial entre dimensiones y hechos.

Además podemos distinguir entre **Carga Inicial** que ocurre la primera vez que se puebla el DW y la **Carga Incremental** o refresco que es un proceso periódico que propaga únicamente los datos modificados o añadidos desde la última extracción.

### Relacion con el DSA
El proceso de carga y la _Staging Area_ están fuertemente vinculados. El Dsa es la fuente de entrada de la carga, la tarea de carga no lee directamente de las BD transaccionales, sino que lee los datos que ya fueron preparados, deduplicados y consolidados en el DSA.
La carga toma el resultado limpio del DSA y se enfoca en la estructuración e inserción física dentro de las tablas de hechos y dimensiones.
Realizar las transformaciones pesadas en el DSA asegura que el DW de producción y los sistemas operacionales no sufran bloqueos ni caídas de rendimiento mientras se preparan los datos para ser cargados.

> [Bibliografía sugerida]:

- Data Warehouses and OLAP: Concepts, Architectures and Solutions.
  Robert Wrembel y otro. IGI Global, 2006.

- Introducción al Business Intelligence. Curto Díaz. Editorial UOC,
  2011.

- Introducción a la minería de datos. José Hernandez Orallo y otros.
  Pearson, 2004.
