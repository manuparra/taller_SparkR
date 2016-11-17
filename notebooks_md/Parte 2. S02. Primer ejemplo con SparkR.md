
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

## Primer ejemplo  y toma de contacto con SparkR

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

Como siempre para todos nuestros `scripts` con **SparkR**, cargamos la biblioteca, y creamos una nueva sesión de SparkR.


```R
# Cargamos el path donde estará la biblioteca
.libPaths(c(file.path(Sys.getenv("SPARK_HOME"),"R/lib/"),.libPaths()))
# Añadimos la bibioteca
library(SparkR)
# Abrimos la conexión
sparkR.session(appName="Primeros_Pasos", master = "local[*]", sparkConfig = list(spark.driver.memory = "1g"))
```

Con **SparkR** podemos crear un `DataFrame` de Spark desde un `data.frame` habitual usado en R. 

Un ``DataFrame`` es una colección distribuida de datos organizada en columnas. 

Los ``dataframes`` son conceptualmente equivalentes a bases de datos relacionales o a `data.frames` en R o Python, pero con una ventaja: son mucho más eficientes para el trabajo con grandes volumenes de datos. Los ``DataFrames``pueden ser creados desde una amplio surtido de fuentes muy diferentes. Es decir, casi cualquier cosa puede ser un ``dataframe``, por ejemplo ficheros estructurados, tablas en ``HIVE``, bases de datos externas o RDDs.


Los RDDs son la principal abstracción de datos en Spark. Un RDD es una colección resilente y distribuida de registros. Esta es una de las claves de Spark y es uno de los componentes fundamentales del `core` de Spark.


```R
# Vamos a usar un dataset sencillo integrado en R
# El dataset contiene el tiempo de espera entre erupciones y duración 
# de la erupción de un geiser de Yellowstone
class(faithful)

# Convertimos un dataframe de R en un DataFrame de Spark, que llamaremos SparkDataFrame
df_faithful <- createDataFrame(faithful)

# Vemos el tipo de dataset nuevo
class(df_faithful)

# Visualizamos de forma rápida el contenido
head(df_faithful)

# Usamos la función printSchema de SparkR para 'deducir' el esquema de datos (la estructura)
printSchema(df_faithful)
```

Un ``SparkDataFrame`` puede ser registrado como una vista temporal en ``SparkSQL`` y que permite ejecutar sentencias SQL sobre los datos. La funcionalidad de SQL permite a las aplicaciones y flujos de trabajo ejecutar consultas SQL de forma programatica, devolviendo el resultado también como SparkDataFrame.

Esto es importante, ya que todas las transformaciones a los conjuntos de datos que están en formato SparkDataFrames, siguen siendo SparkDataFrames, lo que hace que toda su manipulación corra por parte de Spark con todas las ventajas que eso tiene:

- Volumen masivo de datos
- Almacenamiento distribuido
- Resilencia


## Operaciones sencillas con SparkR sobre SparkDataFrames

En estos ejemplos vamos a tratar de ver una parte muy muy simple sobre la manipulación de los datos en el formato que entiende SparkR.


```R
# Contamos los elementos a partir de un filtro normal
count(filter(df_faithful,"eruptions>3.0"))

# Convertimos a vista temporal de datos en SparkSQL y le damos el nombre faithful a la 'tabla'
createOrReplaceTempView(df_faithful,"faithful")

# Usamos SparkSQL para hacer consultas a los datos.
eruptions_sql <- sql("SELECT eruptions FROM faithful WHERE eruptions >= 3.0")

# Contamos el resultado
count(eruptions_sql)

# Mostramos un resumen
head(eruptions_sql)
```

## Operaciones sobre los conjuntos de datos

La guía de referencia de funciones de la API de Spark con R se puede ver en: 

    https://spark.apache.org/docs/2.0.0-preview/api/R/

Veamos las más importantes y sus diferencias con las de R equivalentes.

Recordamos que siempre para trabajar con SparkR, tenemos que terminar la sesión de Spark.


```R
sparkR.session.stop()
```

Con esta sentencia se cierra el contexto abierto en SparkR y se liberan todos los recursos.

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)


Escribe todas las pruebas en R que necesites a partir de aquí

<HR>


```R

```
