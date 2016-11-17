
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Procesamiento masivo de datos con SparkR

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

<HR>

<span style='font-size:1.5em'>Hola ! Si estás en este taller, es porque estás interesado en R y además no te conformas con procesar y analizar datos de poco tamaño; Quieres procesar datos masivamente !!</span>

<span style='font-size:1.2em'>R es un popular lenguaje de programación estadística con una serie de extensiones que soportan el procesamiento de datos y tareas de aprendizaje automático. Sin embargo, el análisis interactivo de datos en R suele ser limitado, ya que el tiempo de ejecución R es de subproceso único y sólo puede procesar conjuntos de datos que se ajusten a la memoria de una sola máquina. <BR><BR>Presentamos SparkR, un paquete R que proporciona un frontend a Apache Spark y utiliza el motor de cálculo distribuido de Spark para permitir el análisis de datos a gran escala desde el shell R. Describimos los principales objetivos de diseño de SparkR, discutimos cómo el API de DataFrame de alto nivel permite el cálculo escalable y presenta algunos de los detalles clave de nuestra implementación.</span>

# Introducción


## Herramientas tradicionales de análisis de datos:

* Unix scripts
* Pandas
* R
* ...

Todas ellas operan sobre una única máquina, no conectadas o conectadas, con recursos limitados, etc.

## El problema de los grandes conjuntos de datos

* Los datos crecen más rápido que la velocidad de computación.
* Fuentes de datos creciendo: datos de la web, datos científicos, datos móviles, datos de sistemas, ...
* Almacenamiento muy barato: El tamaño se dobla cada 18 meses.
* Cuellos de botella en la CPU y almacenamiento.

### Ejemplos 

* Logs diarios de FaceBook: 60 TB
* 1000 genomas: 200 TB
* Indice web de Google: 10+ PB
* Coste de 1 TB de discoduro: ~35 ... 50 €
* Tiempo de lectura de 1 TB: 3 horas !!!! (100 MB/s)

# El problema de analizar grandes conjuntos de datos

Una maquina sola no puede procesar o incluso almacenar todos los datos

* Solo una solución: distribuir  los datos sobre clusters


![sd](https://sites.google.com/site/manuparra/home/mpareduce.png)

<center>MapReduce es el corazón de Hadoop.</center>

Este paradigma de programación permite una escalabilidad masiva a través de cientos o miles de servidores en un clúster con Hadoop. El concepto de MapReduce es bastante simple de entender.

![Hadoop](http://hadoop.apache.org/images/hadoop-logo.jpg)   ![hdfs](http://www.happyminds.es/wp-content/uploads/2013/01/hdfs-logo.jpg)

Limitaciones de Hadoop y MapReduce:

* Latencia para el acceso a datos.
* Cantidades grandes de ficheros pequeños.
* Escribe una vez, lee varias.
* No se puede acceder con los comandos tradicionales de Linux (ls, cat, vim...).

En cuanto a MapReduce:

* Es muy difícil de depurar: Al procesarse el programa en los nodos donde se encuentran los bloques de datos.
* **No todos los algoritmos se pueden escribir** con el paradigma MapReduce. Por ejemplo, el famoso algoritmo de grafos Dijkstra, al necesitar un procesamiento secuencial, no se puede escribir en MapReduce.

# Motivación de Apache Spark

El uso de MAP REDUCE para trabajos complejos, consultas interactivas y proceamiento, involucra **enormes cantidades de entrada** y salida de disco.

El trabajo con Disco es muy lento !

![memory](https://sites.google.com/site/manuparra/home/memory.png)

![memchange](https://sites.google.com/site/manuparra/home/memchange.png)

Almacenar más datos en memoria. Usar más memoria en lugar de disco.

![faster](https://sites.google.com/site/manuparra/home/faster.png)


## Resilient Distributed Datasets (RDDs)

* Colecciones de objetos particionados y repartidos a lo largo del cluster, almacenado en memoría o disco.
* Manipulados a través de deversas transformaciones y acciones (map, filter, join, count, collect, save, ...)
* Son automáticamente reconstruidos en caso de fallo de la máquina 

## Hadoop (Map Reduce) y Spark

|    Caracteristica    |   Hadoop   |              Spark              |
|:--------------------:|:----------:|:-------------------------------:|
|    Almacenamiento    |    Disco   |        En memoria o disco       |
|      Operaciones     | Map Reduce |     Map Reduce Join, Sample     |
| Modelo de  ejecución |  Por lotes | Por lotes Interactivo Streaming |
|       Lenguaje       |    Java    |     Scala, Java, R y Python     |

Procesamiento en Memoria, puede marcar gran diferencia

![compare](https://sites.google.com/site/manuparra/home/inmemory.png)


**Las ventajas principales de Spark:**

1. Una plataforma de código abierto con una comunidad activa
2. Una plataforma rápida 
3. Una plataforma unificada para gestionar datos
4. Consola interactiva
5. Una gran API para trabajar con los datos



```R

```
