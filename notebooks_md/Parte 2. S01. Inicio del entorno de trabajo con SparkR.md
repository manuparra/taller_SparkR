
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

## Inicio del entorno de trabajo con SparkR

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

Lo primero que hacemos para trabajar con el entorno es añadir la bilioteca de **SparkR** al **LibPath** de ``R`` y cargamos la biblioteca de **SparkR**.



```R
.libPaths(c(file.path(Sys.getenv("SPARK_HOME"),"R/lib/"),.libPaths()))
```

Comprobamos que está bien indicado el path y está tomando la bilbioteca adecuada:


```R
Sys.getenv("SPARK_HOME")
```

Cargamos la biblioteca de SparkR


```R
# Usamos a opción de warn=-1 si queremos que no muestre warning y sólo 
# muestre errores (más limpias las salidas)
# options(warn=-1)
library(SparkR)
```

Comenzamos una sesión con Spark. 

Para ello creamos una sesión en Spark, indicándole el modo de inicio de la sesión que queremos: *modo local* `local`, *todos los núcleos disponibles* `[*]` y la *cantidad de memoria RAM a utilizar* `2g`.



```R
# Iniciamos la sesión con Spark.
# Parametros mínimos: appName => Nombre del trabajo en Spark
#                     master  => local[*] Usará tantas hebras como cores disponibles.
#                     sparkConfig => opciones de ejecución de Spark, por ejemplo limitación de memoria a 2GB.
sparkR.session(appName="EntornoInicio", master = "local[*]", sparkConfig = list(spark.driver.memory = "2g"))
```

Otros modos posibles de inicialización de Spark:


```R
# sparkR.session(appName="NombreAplicacion", master = "local", sparkConfig = list(spark.driver.memory = "4g"))
# sparkR.session(appName="AplicacionMLData", master = "local[20]", sparkConfig = list(spark.driver.memory = "2g"))
```

Más información sobre los métodos de inicialización de Spark: https://spark.apache.org/docs/latest/sparkr.html#starting-up-from-rstudio

Una vez que hayamos terminado el script, **siempre al fina**l, es necesario siempre cerrar la sesión con Spark para liberar recursos.

El problema que tiene no usar este comando es que si no se liberan recursos y los dataframes están en memería y además tenemos limitada la memoria, entonces Spark no funcionará correctamente al acumuluar y no despejar lo que hemos usado.


```R
# Cerramos la sesión con Spark y liberamos recursos.
sparkR.session.stop()
```

Por tanto el flujo de trabajo con SparkR debe ser el siguiente:

![FlujoTrabajoSparkR](https://sites.google.com/site/manuparra/home/flujotrabajo.png)

### Algunas diferencias entre SparkR 1.5 la versión de SparkR 2.0

* Se ha cambiado la función ``table`` por ``tableToDF``.
* La clase DataFram se ha sustituido por SparkDataFrame para evitar conflictos.
* Ahora se usa sparkR.session() para arrancar la sesión con SparkR, en lugar de sparkR.init().
* No se necesita un sparkContext para las funciones para crearDataFrame, read.json, cacheTables, read.df, ...
* La función `registerTempTable` se ha reemplazado por `createOrReplaceTempView` para trabajar con SparkSQL.
* La función `dropTempTable` se ha cambiado por `dropTempView`.

### Software vivo y en continuo cambio

* Es importante estar siempre informado de las mejoras y cambios que trae una u otra versión que se publica de SparkR, ya que posiblemente mejore aspectos como rendimiento, definiciones, flujos de trabajo, herramientas, etc.

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)


Escribe todas las pruebas en R que necesites a partir de aquí

<HR>


```R

```
