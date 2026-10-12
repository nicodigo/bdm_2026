# GUÍA DE LECTURA V: Data Lake

## 1. ¿Qué se entiende por Data Lake?

Un **Data Lake** es una metodología y un repositorio de almacenamiento masivo y centralizado basado en tecnologías de bajo costo, diseñado para capturar, almacenar, refinar y explorar datos en su formato crudo o nativo (_raw data_) a gran escala dentro de una organización.
El término se asocia originalmente a **James Dixon** (CEO de Pentaho) en 2010, quien propuso una analogía conceptua para diferenciarlo de Data Warehouse:

Data Warehouse se asemeja a una botella de agua purificada, donde los datos han sido limpiados, transformados y empaquetados previamente para un consumo directo y estructurado.
Data Lake es un lago de agua en estado natural, donde los datos fluyen desde múltiples fuentes heterogéneas y se almacenan en su estado original sin filtrar ni procesar previamente.

## 2. ¿Cuáles son sus principales características?

- Almacenamiento de datos heterogéneos: Acepta e ingesta cualquier tipo de dato sin importar su estructura o formato: datos estructurados (tablas relacionales), semi-estructurados (archivos JSON, XML) y no estructurados (imágenes, audio, texto). Bajo el principio de _\"no rechazar ningún dato\"_, la información se guarda a nivel de hoja en su estado nativo.
- Paradigma ELT: A diferencia del proceso ETL tradicional del DW, el Data Lake adopta ELT: los datos primero se extraen de las fuentes y se cargan masivamente en el lago; las transformaciones se ejecutan posteriormente, únicamente cuando los datos son requeridos por una aplicación o consulta específica.
- Enfoque _**Schema-on-Read**_: No exige definir un esquema o modelo de datos estricto al momento de escribir o guardar la información. El esquema, la estructura y la interpretación semántica se aplican recién al momento de consultar o leer los datos, según la necesidad particular del usuario o analista.
- Arquitectura Plana y Gestión de Metadatos: Utiliza una arquitectura plana donde cada objeto o entidad de datos almacenada recibe un identificador único y un conjunto de etiquetas de metadatos extendidos que permiten su posterior localización y contexto analítico.
- Infraestructura de Bajo Costo y Alta Escalabilidad: Se implementa habitualmente sobre tecnologías distribuidas y de código abierto diseñadas para hardware genérico.

## 3. ¿Cómo es la arquitectura de un Data Lake?

La arquitectura de un Data Lake se caracteriza por ser una **arquitectura plana**. A diferencia de la estructura jerárquica de un DW tradicional, el Data Lake almacena todos los datos en su formato crudo o nativo.
Cada objeto o entidad almacenada en el lago se asocia a un identificador único y a un conjunto de etiquetas de metadatos extendidos. Esto permite eliminar las transformaciones previas a la carga y aplicar el enfoque ETL junto con la propiedad de esquema en lectura, donde la estructura y el procesamiento se definen al momento de consultar.

### Capas del Data Lake
Para mantener el orden y evitar que el Data Lake se transforme en un _\"Pantano de datos\"_ (_Data Swamp_), la arquitectura se organiza habitualmente en Zonas o Capas de madurez:
1. Zona de Ingesta de Datos Crudos:
    * Almacena los datos tal como llegan de los sistemas fuente.
    * Se conservan sin transformar a nivel de hoja y sin aplicar esquemas estrictos.
2. Zona de Refinamiento o Procesamiento:
    * Contiene datos limpios, desduplicados y consolidados de forma intermedia.
    * Se enriquecen mediante la adición de etiquetas de metadatos a nivel de atributo y enlaces entre conjuntos de datos.
3. Zona Curada o de Explotación:
    * Contiene modelos de datos preparados para casos de uso de negocio específicos, consultas analíticas complejas o algoritmos de Machine Learning.

### Componentes Clave
La arquitectura física del Data Lake se apoya en tres pilares tecnológicos distribuidos:
- Almacenamiento Distribuido de Bajo Costo: Principalmente construido sobre clústeres **Hadoop** o sistemas de almacenamiento de objetos.
- Motores de Procesamiento: Procesamiento por lotes masado en el paradigma MapReduce y procesamiento en tiempo real con marcos de trabajo como Apache Spark y Apache Flink.
- Catálogo y Repositorio de Metadatos: Almacena las definiciones, el linaje, los esquemas aplicados en lectura y el contexto de los datos.

## 4. ¿Qué diferencia sustancial existe entre un Data Lake y un Data Warehouse (DW)?

- **Paradigma de procesamiento (ETL vs. ELT)**:
    * **Data Warehouse**: Aplica el flujo tradicional **ETL** (*Extract, Transform, Load*), donde los datos se extraen de los sistemas origen, se limpian y transforman en un área intermedia (*Staging Area*) y luego se cargan en las tablas multidimensionales.
    * **Data Lake**: Adopta el paradigma **ELT** (*Extract, Load, Transform*), donde los datos se extraen e ingieren directamente en su estado original o crudo (*raw data*), postergando la transformación para el momento en que se ejecuta una consulta.
- **Definición del Esquema (** **Schema-on-Write** **vs.** **Schema-on-Read** **)**:
    * **Data Warehouse**: Se rige por **Schema-on-Write**; los modelos de datos, esquemas (estrella/copo de nieve) y tipos de columnas deben diseñarse estrictamente *antes* de escribir los datos.
    * **Data Lake**: Se rige por **Schema-on-Read**; no exige un esquema predefinido al momento del almacenamiento, aplicando la estructura y la interpretación semántica recién al leer los datos.
- **Tipo y variedad de datos**:
    * **Data Warehouse**: Solo almacena **datos estructurados** y procesados/agregados orientados a temas de negocio.
    * **Data Lake**: Captura **todo tipo de datos** (estructurados, semiestructurados como JSON/XML y no estructurados como imágenes, audios o documentos) a nivel de hoja y en su formato nativo.

## 5. ¿Qué caracteriza la etapa de preprocesamiento y transformación en Data Lake vs DW?
La etapa de preprocesamiento y transformación marca una de las diferencias conceptuales y arquitectónicas más profundas entre Data Lake y Data Warehouse:

### Flujo de procesamiento: ETL vs. ELT
- DW: Sigue el paradigma tradicional ETL. La transformación y limpieza de los datos ocurren antes de guardarse en el almacén de destino, procesándose habitualmente dentro de un área intermedia de almacenamiento.
- DL: Sigue el paradigma ELT. Los datos se extraen de las fuentes y se cargan masivamente en su estado bruto original. Las transformaciones se postergan y se ejecutan a demenda.

### Momento del Esquema: On-Write vs. On-Read
- DW: Se rige por **Schema-on-Write**. Exige definir de antemano el modelo de datos (esquemas en estrella/copo de nieve) y los tipos de atributos. Durante el preprocesamiento, los datos deben moldearse rigurosamente para coincidir con el esquema de destino.
-DL: Se rige por **Schema-on-Read**. Los datos se almacenan sin una estructura preconcebida. La interpretación semántica, el filtrado y el esquema se aplican recién al momento de realizar la consulta o lectura (*query time*).

### Nivel de limpieza e Inclusión de Datos
- DW: Ejecuta un preprocesamiento **intensivo y restrictivo**. Se abordan minuciosamente conflictos de esquema, duplicados y errores de valor (monedas, fechas, codificaciones), descartando o corrigiendo los registros que no cumplan con las reglas de negocio.
- DL: Aplica el principio de *"no rechazar ningún dato"*. Los datos se conservan en su nivel más atómico u hoja en un estado sin transformar o casi sin transformar. En el momento de la ingesta solo se asocian identificadores únicos y etiquetas de metadatos. La preparación y limpieza de datos se efectúan únicamente sobre la porción requerida para cada consulta específica.

### Agilidad vs. Consistencia
- DW: Las transformaciones previas (*upfront*) garantizan una alta calidad e integración semántica de la información para los usuarios finales, aunque restringen la agilidad ante cambios en los requerimientos.
- DL: Evita las costosas transformaciones previas y ofrece gran agilidad y flexibilidad para exploraciones analíticas complejas (por científicos de datos), delegando la responsabilidad de la transformación al nivel de la aplicación o consulta.

## 6. ¿Cuáles son los PROS y CONTRA de una solución Data Lake desde el punto de vista de la agilidad? ¿Y de la seguridad?

### Desde el punto de vista de la Agilidad
#### PROS
- Los datos se cargan directamente en su estado bruto sin necesidad de transformaciones previas. Esto permite disponer de la información de forma casi inmediata.
- Permite exprorar fuentes no estructuradas o semiestructuradas, ajustando los modelos y consultas sobre la marcha según la necesidad específica de cada análisis.
- Al no rechazar ningún tipo de dato, se reduce el tiempo necesatio para integrar nuevas fuentes de datos operativas o externas

#### CONTRAS
- Dado que no existe un esquema prefijado, el esfuerzo de estructuración y limpieza se traslada a la fase de lectura. Construye la lógica de transformacion desde cero en cada consulta.
- La falta de supervivión y de metadatos descriptivos en la ingesta puede dificultar el hallazgo de datos útiles o la verificación de su calidad, realentizando las exploraciones analíticas en lugar de agilizarlas.

### Desde el punto de vista de la Seguridad
#### PROS
- Al consolidar los datos de toda la organización en un único repositorio centralizado, se establece un punto único de control donde es posible aplicar políticas globales de datos en lugar de gestionar la seguridad en múltiples bases fragmentadas.

#### CONTRAS
- Las soluciones de Data Lake aún presentan vulnerabilidades y carecen de respuestas consolidadas en materia de seguridad.
- El principio de aceptar cualquier dato sin filtrado previo aumenta el riesgo de almacenar información sensible o regulada sin el debido etiquetado, control de accesos granular o cifrado adecuado.
- La falta de gestión sólida de metadatos complica el rastreo del origen de los datos (_Data Lineage_) y dificulta garantizar el cumplimiento de normativas de privacidad y requerimientos regulatorios.

---

> Bibliografía sugerida:
>Khine, P. P., & Wang, Z. S. (2018). Data lake: a new ideology in big data era. In ITM web of conferences (Vol. 17, p. 03025). EDP Sciences.
