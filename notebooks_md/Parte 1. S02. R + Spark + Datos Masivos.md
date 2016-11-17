
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# R + Spark + Big Data

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

<HR>

# Introducción

La tendencia más reciente en el análisis de grandes conjuntos de datos indica la necesidad de un análisis *INTERACTIVO* de grandes conjuntos de datos.

Debido a ello se han desarrollado herramientas academicas y comerciales para trabajar de este modo con los datos.

Está claro que cada vez más el uso de herramientas como R para el análisis de datos, ya que permiten una mejora sustancial y más avanzada para el estudio de los datos. R, como sabemos, es ya hoy en día bastante popular y provee de soporte para procesamiento de datos estructurados usando dataframes y lo mejor de todo es que incluye en CRAN un número muy elevado de paquetes para análisis estadístico y visualización.

<HR>

* R es la herramienta ``de facto`` para el análisis de datos
* R soporta procesamiento de datos estrcuturados ``dataframes``
* R tiene miles de paquetes para todo tipo de tareas: analíticas, visualización, minería de datos, ...


# R

El proyecto R, o el lenguaje R es un lenguaje de programación, un entorno de desarrollo interactivo, y un conjunto de bibliotecas de computación estadistica.

R es un lenguaje interpretado y provee de soporte para ejecución condicional, bucles, etc. 
R también incluye caracteristicas para computación nmérica, con tipos de datos vectores, matrices, etc.
Otra característica fundamental es el uso de dataframes: estructuras de datos en tablas donde cada columna  está formada por elementos con su tipo de dato. Estos dataframes pueden ser manipulados para filtrado, agregación, etc.

## Entonces, ¿cuál es el problema? ¿Por qué no se usa para Datos Masivos?

El análisis de datos usando R está limitado por **la cantidad de memoria disponible** en una sola máquina y además R trabaja en un sólo hilo/proceso (*parallel*), por lo que **es poco práctico** usar R para grandes conjuntos de datos. 



# Spark

Algunas de estas limitaciones se han abordado, a través de un mejor soporte de Entrada/Salida, la integración con Hadoop y el diseño de  aplicaciones R distribuidas que se pueden integrar con los motores de Bases de Datos más extendidos

Por tanto, analizamos cómo podemos escalar los programas R al tiempo que facilitamos su uso y despliegue a través de varias cargas de trabajo. 

Hay una serie de beneficios para el diseño de un frontend R que está  integrado con Spark:



* Bibliotecas de soporte: Spark, contiene bilbiotecas para trabajar con fuentes de datos y  SQL, 
   Machine Learning Distrbuido, análisis de grafos, etc.
* Fuentes de datos: Desde bases de datos, HDFS, HBase, Cassandra, JSON, CSV, Parquet, ...
* Rendimiento: Planificación de tareas, generación de código, gestión de memoria, ...

Apache Spark es un motor de proposito general para procesamiento de conjuntos de datos masivos. El proyecto inicialmete introdujo el RDD, Resilient Distributed Datasets, una API para computación tolerante a fallos en un entorno de cluster. 

Más recientemte muchas más APIs de alto nivel están siendo desarrolladas en Spark. Estás son las siguientes:

* Machine Learning Library (MLLib) una biblioteca para machine learning a gran escala.
* GraphX, una biblioteca para procesamiento de grafos a gran escala 
* SparkSQL, una API para consultas analíticas en SQL

Las bibliotecas están integradas en el CORE de Spark, por lo que Spark, puede permitir flujos de trabajos complejos, donde consultas SQL pueden ser usadas para preprocesar posteriormente los datos y analizarlos de forma avanzada con la parte de Machine Learning


## ¿Por qué Spark? ¿Qué ventajas tiene?

Desde el inicio de Hadoop hace más de 10 años, poco a poco han aparecido nuevos problemas que ya no se podían resolver usando el paradigma de MapReduce. Esto ha echo que hayan aparecido nuevos sistemas para afrontar estos problemas, apareciendo :


![flora](https://sites.google.com/site/manuparra/home/flora.png)


![lang](https://sites.google.com/site/manuparra/home/lenguajes.png)


Una característica importante es que muchas de las aplicaciones ya existentes se han hecho compatibles con Spark y que estén surgiendo nuevas enfocadas en trabajar con Spark en áreas especificas de procesamiento de datos masivos.

![fauna](https://sites.google.com/site/manuparra/home/fauna.png)






# SparkR

SparkR está construido como un paquete en R, luego no es necesarios hacer cosas especiales para que funcione.

El componente principal de SparkR es un DDF, Distributed Data Frame, que permite procesamiento de datos estructurados utilizando la sintaxis familiar para los usuarios acostumbrados a R.

Para mejorar el rendimiento, sobre grandes conjuntos de datos, SparkR provee de "evaluación perezosa" para operaciones sobre dataframes y utiliza además un optimizador de consultas relacionales para optimiar la ejecución.

![coresparkr](https://sites.google.com/site/manuparra/home/rspark_arch.png)


# R sobre Spark: Aplicación

**BigData, Small Learning**: 

Habitualmente los usuarios comienzan con un conunto de datos enorme almacenado en algún formato. El procesamientos de los datos comienza con la limpieza, eliminación de instancias, ... El siguiente paso se realizan operaciones de agregación o muestreo de datos para reducir el tamaño del conjunto de datos. Por último el preprocesado de datos es usado para construir modelos o realizar otras tareas estadísticas.

**Partition Aggregate**: 

Esto es util para un número de aplicaciones estadisticas como `ensembles` para machine learning, ajuste de parámetros, etc. El usuario necesita que una función sea ejecutada en paralelo a lo largo de las diferenctes particiones y que luego sean combinadas realizando una agregación. 


**Large Scale Machine Learning**: 

Es necesario ejecutar algoritmos de Machine Learning sobre conjunto de datos masivos. El modelo ajustado es sustancialmente más pequeño que el tamaño de la entrada de datos, El modelo luego será utilizado para hacer las predicciones.

<HR>




```R

```
