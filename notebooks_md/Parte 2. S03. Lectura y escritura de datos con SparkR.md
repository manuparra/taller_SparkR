
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Lectura de datos desde SparkR

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

Como siempre, para todos nuestros `scripts` con **SparkR**, cargamos la biblioteca, y creamos una nueva sesión de SparkR.


```R
.libPaths(c(file.path(Sys.getenv("SPARK_HOME"),"R/lib/"),.libPaths()))
library(SparkR)
sparkR.session(appName="EntornoInicio", master = "local[*]", sparkConfig = list(spark.driver.memory = "1g"))
```

Hoy en día el trabajo con BigData parece que siempre está asociado al ecosistema HADOOP. Hace unos años esto significaba que si también eras un buen programador en JAVA, trabajar en tal entorno, incluso un simple programa para hacer un WORDCOUNT, implicaba varias docenas de líneas de código. 

Pero hace 3-4 años la cuestión ha cambiado gracias a Apache Spark con su API de estilo funcional. 

Está escrito en SCALA pero también puede ser utilizado desde Python, JAVA y como estais viendo por este Taller: también en R

## Fuentes de datos

Dentro de una sesión de Spark, las aplicaciones pueden crear `SparkDataFrames` desde variadas fuentes de datos, como por ejemplo:  un fichero local (`data.frame`), desde HDFS (``hdfs:///``),  desde tablas en `HIVE` o desde otras múltiples fuentes de datos (AmazonS3, etc).

Concretamente las principales fuentes u orígenes de datos desde las que **cargar datos** son los siguientes:

* Ficheros locales
* Ficheros en sistemas distribuidos de almacenamiento Hadoop HDFS
* Sistemas de almacenamiento de datos tipo HIVE
* Desde bases de datos relacionales a través de JDBC
* ...


## Tipos de fuentes de datos

Una cosa son las **fuentes de datos** y otras cosa son los **tipo de fuentes de datos**. El tipo de fuente de datos puede ser visto como el formato de los datos.

Los conjuntos de datos pueden están almacenados en diferentes formatos, los más utilizados para SparkR (y Spark):

* Ficheros planos y CSV
* **Ficheros JSON**
* Ficheros de tipo ```avro``` (row-based)
* Ficheros de tipo ```parquet``` (column-based)
* Ficheros de tipo ```orc``` (column-based)

![ListOfSources](https://sites.google.com/site/manuparra/home/files_API.png)


## Repositorio de Datasets para todo el taller

Todos los conjuntos de datos que vamos a tratar para el Taller se encuentran disponibles en el directorio ```datasets```. Para consultar, modificar y añadir datasets, ficheros, etc, puedes hacerlo usando el gestor de fichero de ```Jupyter``` desde:  http://localhost:25980/tree/datasets

<BR>

## Trabajo con ficheros en formato CSV 

Vamos a revisar todas las funcionalidades que ofrece SparkR para el manejo de CSV

### Lectura

Comprobamos que hay en el directorio donde tenemos los datasets


```R
list.files("/root/TallerSparkR/datasets/csv",full.names = T,recursive = T)
```


Leemos un fichero concreto de datos en formato ```CSV``` del directorio ```datasets/csv```. El fichero de ejemplo sólo tiene 1000 registros:


Para la lectura de datos con SparkR usamos la función ``read.df( )``


```R
# Sólo indicamos un fichero concreto .... No hay problema Spark es muy listo ! ;)
df <- read.df("/root/TallerSparkR/datasets/csv/buy_costumers_amazon01.csv", "csv", header = "true", inferSchema = "true")
printSchema(df)
count(df)
head(df)
```


```R
# Sólo indicamos un fichero concreto .... No hay problema Spark es muy listo ! ;)
df <- read.df("/root/TallerSparkR/datasets/csv/buy_costumers_amazon01.csv", "csv", header = "true", inferSchema = "true")
print("Estructura sin parsear:")
printSchema(df)

# Creamos un esquema para definir cual será la estructura del fichero a leer.
schema_amazon <- structType(structField("id", "integer"),
                     structField("first_name", "string"),
                     structField("last_name", "string"),
                     structField("buy_hours", "string"),
                     structField("amount", "double"),
                     structField("credit_card", "string"))

df <- read.df("/root/TallerSparkR/datasets/csv/buy_costumers_amazon01.csv", "csv", header = "true", schema=schema_amazon)
print("Estructura parseada:")
printSchema(df)
head(df)

```

Si queremos leer todos los ficheros de un directorio sin entrar en los subdirectorios:


```R
# Esto leería todos los ficheros de la carpeta pero no entraría a cada subdirectorio... Spark no eres muy listo !
df <- read.df("/root/TallerSparkR/datasets/csv/", "csv", header = "true", inferSchema = "true", schema=schema_amazon)
count(df)
```

Comprobamos que estructura de ficheros y directorios tenemos. Observamos que en ```datasets/csv/``` existen subdirectorios, por lo que hay que usar comodines: *****



```R
list.files("/root/TallerSparkR/datasets/csv",full.names = T,recursive = T)
```

Ejecutamos la siguiente instrucción para poder leer todo lo que cuelgue del directorio donde están los CSV.


```R
# Leemos todos los ficheros CSV que haya en el directorio y todo en las subcarpetas... Spark que bien, no?
df_full <- read.df("/root/TallerSparkR/datasets/csv/*/", "csv", header = "true", inferSchema = "true")
```


Verificamos que ahora tenemos todos los datos cargados desde todos los ficheros ```CSV``` de la estructura de directorios:



```R
count(df_full)
```

### Escritura

Una vez que hemos realizado transformaciones con los datos del SparkDataFrame, podemos dejarlo en memoria o bien pasarlo a DISCO (local) o HDFS (distribuido).

La API de fuentes de datos puede también ser usada para guardar y almacenar ``SparkDataFrames`` en múltiples formatos. Por ejemplo podemos almacenar el ``SparkDataDrame`` desde/hacia otros formatos como ``CSV``, ``PARQUET`` usando la función ``write.df``. 

Esto da mucha versatilidad, ya que independiente del tipo de fuente, podemos almacenarlo y leerlo desde cualquiera otra fuente. Como no podía ser de otra forma.


```R
# Escritura desde CSV a CSV:
write.df(df_full, path = "datasets/results/df_full.csv", source = "csv", mode = "overwrite")

# Escritura desde CSV a PARQUET
write.df(df_full, path = "datasets/results/df_full.parquet", source = "parquet", mode = "overwrite")
```

En ``mode``	podemos usar 'append', 'overwrite', 'error', 'ignore'.

## PARQUET

Parquet es un formato de **almacenamiento en columnas** disponible para cualquier proyecto dentro del ecosistema de Hadoop, enfocado en la mejora del procesamiento de datos, modelado de datos y programación.

Parquet está diseñado para soportar esquemas de compresión y codificación muy eficientes. Múltiples proyectos han demostrado el impacto en el rendimiento de aplicar el correcto sistema de compresión y codificación a los datos. Parquet permite que los esquemas de compresión se especifiquen a nivel de columna.

Es un formato bien estructurado para ser usado **para problemas de BigData**.

La estructura del fichero se **segmenta en N columnas partidas en M grupos de filas**:

![Alt](https://raw.github.com/Parquet/parquet-format/master/doc/images/FileLayout.gif)


Leemos el dataset en formato Parquet, luego el resultado de la lectura es un SparkDataFrame, compatible con el trabajo en SparkR


```R
# Leemos un dataset que contiene los datos en formato Parquet
df_parquet <- read.df("/root/TallerSparkR/datasets/parquet/", "parquet")
```


```R
# Vemos la estructura del fichero y sus atributos
printSchema(df_parquet)
```


```R
# Vemos un resumen de los datos del fichero ...
head(df_parquet)
```


```R
# Hacemos un pequeño cambio en el nombre de las columnas del SparkDataFrame.
colnames(df_parquet) <- c("user_id","cat","R1","R2","R3")
```

Vemos de nuevo el cambio de las columnas:


```R
head(df_parquet)
```


```R
# Contamos los registros del dataset ... es pequeño, no es BigData...
count(df_parquet)
```

Aplicamos unas transformaciones sencillas al SparkDataFrame, copiando la tabla en una Vista Temporal para poder trabajar con ella en SQL.


```R
# Creamos una vista SparkDataFrame con el nombre "tmp_parquet".
# Este nombre tmp_parquet es el nombre que se usará ahora.
createOrReplaceTempView(df_parquet,"tmp_parquet")
```

Una vista temporal, permite trabajar con una copia temporal de los datos.

Contamos el número de registros:


```R
# Usamos SparkSQL para hacer consultas a los datos.
count_rows <- sql("SELECT user_id,count(user_id) as registers FROM tmp_parquet group by user_id")
# Cuidado como obtener las cosas en SparkR: ---> Nooooooooo !!!! ;)
# print(collect(count_rows))
```

Compara el tiempo la opción anterior y la siguiente


```R
head(count_rows)
```

Si usamos una vista temporal, está estará disponible durante toda la sesión a no ser que se elimine la vista temporal con `unpersist(....)`

Probamos con otro ejemplo, para saber las categorías que hay:


```R
# createOrReplaceTempView(df_parquet,"tmp_parquet") --> No volvermos a cargarla!
# Usamos SparkSQL para hacer consultas a los datos.
categories <- sql("SELECT cat FROM tmp_parquet group by cat")
```


```R
head(categories)
```

¿Cómo se calcularía el número de elementos de cada categoría?


```R
# createOrReplaceTempView(df_parquet,"tmp_parquet")   --> No volvemos a cargarla !
# Usamos SparkSQL para hacer consultas a los datos.
categories_list <- sql("SELECT cat,count(user_id) as num_users FROM tmp_parquet group by cat")
```


```R
head(categories_list)
```

¿Cuando usuarios distintos hay y que suma total tienen por usuario?


```R
# createOrReplaceTempView(df_parquet,"tmp_parquet") --> No volvemos a cargarla!
# Usamos SparkSQL para hacer consultas a los datos.
table_summary <- sql("SELECT user_id,SUM(R1) as sum_index FROM tmp_parquet group by user_id")
```


```R
count(table_summary)
head(table_summary)
```


```R
unpersist(table_summary)
```

### Escritura de los datos

Al igual que con los otros formatos, se pueden exportar a cualquier otro.


```R
# Escritura del fichero de formato parquet a formato parquet
write.df(finals, path = "/root/TallerSparkR/datasets/results/finals.parquet", source = "parquet", mode = "overwrite")

# Escritura del fichero de formato csv a formato a CSV
write.df(finals, path = "/root/TallerSparkR/datasets/results/finals.csv", source = "csv", mode = "overwrite")
```

## JSON

JSON, acrónimo de JavaScript Object Notation, es un formato de texto ligero para el intercambio de datos. 

JSON es un subconjunto de la notación literal de objetos de JavaScript aunque hoy, debido a su amplia adopción como alternativa a XML, se considera un formato de lenguaje independiente.

Es un formato actualmente muy utilizado ya que se ha impuesto como modelo para la entrada y salida de datos desde multiples y variados servicios web. Por ejemplo por citar varios:

- Twitter. https://dev.twitter.com/rest/public 
- Google APIs
- FaceBook API
- ...

Veamos en http://localhost:25980/tree/datasets como son los ficheros JSON por dentro.

Es muy utilizado este formato para servicios web de información, donde lo que prima es la sencillez y versatilidad. 



### Lectura de datos

La sintaxis es la misma, pero varía el identificador del tipo de fuente, en este caso JSON.


```R
costumers <- read.df("/root/TallerSparkR/datasets/json/buy_costumers_amazon.json", "json")
```

Revisamos la información del SparkDataFrame y su estructura.


```R
# Un resumen
head(costumers)

# El numero de registros
count(costumers)

# La estructura
printSchema(costumers)
```

**¿Por qué no se lee correctamente el JSON?** Revisamos el dataset http://localhost:25980/tree/datasets y arreglamos el error.

La lectura de multiples ficheros es similar lo que ocurre con CSV, donde podemos indica que hay más archivos que queremos leer desde el directorio.

Lectura desde varios ficheros JSON:


```R
costumers_double <- read.json(c("/root/TallerSparkR/datasets/json/buy_costumers_amazon.json", "/root/TallerSparkR/datasets/json/buy_costumers_amazon.json"))

```

¿Cuántos registros hay ahora?


```R
count(costumers_double)
```

### Escritura de datos a formato JSON

Al igual que para los otros formatos se usa el mismo esquema para guardar datasets a disco.


```R
write.df(costumers_double, path = "datasets/results/costumers.json", source = "json", mode = "overwrite")

```

Como siempre cerramos la sesion de Spark


```R
sparkR.session.stop()
```

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)


Escribe todas las pruebas en R que necesites a partir de aquí

<HR>


```R

```
