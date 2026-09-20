

Bases de Datos Masivas (11088) Departamento de Ciencias Básicas 

## **TRABAJO PRÁCTICO 02: Procesos ETL** 

# **Introducción** 

En este trabajo práctico se abordan las cuestiones relacionadas con los procesos de ETL. Se plantean ejercicios y datasets cuyas resoluciones serán realizadas utilizando Pandas en Python y Apache Hop<sup>1</sup> , en el repositorio de la materia se encuentra esta herramienta dockerizada<sup>2</sup> . 

# **Objetivos de la Práctica** 

- Comprender las fases de Extracción, Transformación y Carga (ETL) sobre datasets de datos masivos / públicos. 

- Aplicar dos paradigmas de desarrollo ETL: 

   - Programático (Code-first): Utilizando Python con la librería pandas. 

   - Visual/Orquestado (GUI / Low-code): Utilizando Apache Hop en entorno Dockerizado. 

- Evaluar y comparar la complejidad, mantenibilidad, rendimiento y experiencia de desarrollo de ambos enfoques. 

> 1 <u>https://hop.apache.org/</u> 

> 2 <u>https://github.com/bdm-unlu/2025/tree/main/dockers/hop</u> 

# **Consignas** 

A partir del dataset Education Data<sup>3</sup> del World Bank Group, desarrollar los flujos necesarios para generar los siguientes archivos de salida (en formato CSV): 

1. **CSV de Países con la siguiente estructura:** 

- Campos requeridos: 

   - id: Identificador numérico entero autoincremental (1, 2, 3, ...). 

   - nombre_pais: Nombre oficial del país o entidad regional. 

   - codigo_pais: Código estándar de 3 letras (ISO Alpha-3). 

- Transformaciones esperadas: 

   - Limpieza de registros nulos o datos faltantes en los campos base. 

   - Eliminación de duplicados. 

**2. CSV de Preguntas con la siguiente estructura:** 

   - Campos requeridos: 

      - id: Identificador numérico entero autoincremental. 

      - pregunta: Descripción corta del indicador o pregunta educativa. 

   - Transformaciones esperadas: 

      - Tratamiento de texto: Expresión regular (RegEx) o manipulación de cadenas para remover completamente cualquier contenido que se encuentre entre paréntesis (incluyendo los propios paréntesis) y aplicar un trim para eliminar espacios extra resultantes. 

      - Consolidar indicadores únicos sin duplicados. 

> 3 <u>https://data.worldbank.org/topic/education?view=chart</u>

**3. CSV de Educación primaria en años ordenado por duración de mayor a menor según el año 2023** 

   - Campos requeridos: 

      - País / Código de País. 

      - Duración de la educación primaria en años. 

   - Transformaciones esperadas: 

      - Filtrar exclusivamente el indicador correspondiente a la duración de la educación primaria en años. 

      - Filtrar o considerar únicamente la medición correspondiente al año 2023. 

      - Tratar valores nulos o no reportados para dicho año (especificar en el informe la estrategia adoptada: imputación, descarte, etc.). 

      - Ordenamiento: Ordenar los datos de mayor a menor según la duración en años. 

Se solicita incorporar en el informe final en formato PDF las respuestas detalladas a las siguientes 4 preguntas conceptuales y prácticas: 

1. Facilidad de Desarrollo y Curva de Aprendizaje: ¿Cuál de los dos enfoques (Pandas vs. Apache Hop) resultó más intuitivo para realizar transformaciones de texto complejas como la limpieza mediante expresiones regulares (RegEx)? Justifique evaluando el tiempo de desarrollo y la legibilidad de la solución. 

2. Manejo de Errores, Traceabilidad y Depuración (Debugging): Al momento de inspeccionar los datos intermedios o detectar inconsistencias durante la ejecución, ¿qué diferencias encontró entre el flujo gráfico de Apache Hop (sus transformaciones/steps y preview) y el entorno interactivo de una Notebook de Python (Jupyter/Colab)? 

3. Escalabilidad y Rendimiento en Procesamiento Masivo:Pensando en un escenario de volúmenes masivos de datos (Big Data) que superen la memoria RAM de un solo equipo, ¿cuáles son las limitaciones de pandas y cómo Apache Hop permite gestionar/escalar flujos ETL mediante procesamiento distribuido o en streaming? 

4. Portabilidad, Automatización y Despliegue en Producción:Analice el costo de mantenimiento y orquestación en producción de ambas opciones: ¿Qué ventajas ofrece la dockerización de Apache Hop frente a la ejecución e integración de scripts/notebooks de Python mediante herramientas de orquestación (ej. Apache Airflow, Cron, etc.)? 

Resolver el ejercicio utilizando Apache Hop y realizar el mismo proceso a través de en una Notebook de Colab o Jupyter. Comentar los resultados obtenidos en cada una de las aproximaciones y comparar el proceso. Para esto genere un breve informe y envíelo en formato PDF. 

2° Cuatrimestre 2026 Universidad Nacional de Luján 

