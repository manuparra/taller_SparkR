
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Operaciones sobre SparkDataFrames

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

Como siempre para todos nuestros `scripts` con **SparkR**, cargamos la biblioteca, y creamos una nueva sesión de SparkR.

En este caso:

<span style="background-color:red;color:white">&nbsp; &nbsp; Cuidado con la cantidad de MEMORIA que usamos para esta sección ! &nbsp; &nbsp; </span>


```R
.libPaths(c(file.path(Sys.getenv("SPARK_HOME"),"R/lib/"),.libPaths()))
library(SparkR)
sparkR.session(appName="EntornoInicio", master = "local[*]", sparkConfig = list(spark.driver.memory = "1g"))
```

Los ``SparkDataFrames`` soportan un alto número de funciones para hacer un procesado de datos estructurado. 

Vamos a poner en práctica las más utilizadas. La lista completa de operaciones que se pueden aplicar se puede ver desde API de SparkR en https://spark.apache.org/docs/latest/api/R/index.html

![funcSparkR](https://sites.google.com/site/manuparra/home/functionSparkR.jpg)


# Operaciones con SparkDataFrames

Cargamos un conjunto de datos masivo desde el repositirio de datasets.


Podemos verlos desde: http://localhost:25980/tree


El dataset que vamos a usar para el procesamiento de dato masivos, corresponde con un conjunto de datos de los registros de viaje en *TAXI*, donde se capturan las fechas y horas de recogida y devolución de pasajeros, lugares de recogida y entrega (coordenadas), distancias de viaje, tarifas detalladas, tipos de tarifas, tipos de pago y conteos de pasajeros que van en el taxi.  

El dataset tiene **MUCHAS** posibilidades de procesamiento y también extracción de conocimiento.

Estos conjuntos de datos adjuntos fueron recopilados y proporcionados por la Comisión de Taxisde Nueva York (TLC) http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml.


![Alt](http://www.nyc.gov/html/tlc/images/features/fi_about_photo_trip_records.png)



**Características del conjunto de datos original:**

- El conjunto de datos NYCTaxiTrips en total tiene sobre **267GB**, que pueden ser manejados sin problema por SparkR (en un cluster real, no sobre una máquina virtual sencilla). 
- En total contiene 1100 millones de registros.
- Más información de como se gestionan 1100 millones de instancias en la siguiente web y se soluciona este problema  problema real: http://toddwschneider.com/posts/analyzing-1-1-billion-nyc-taxi-and-uber-trips-with-a-vengeance/


Más datasets masivos de NYCTaxiTrips en:  http://www.nyc.gov/html/tlc/html/about/trip_record_data.shtml


Primero revisamos los distintos dataset que se han preparado en: http://localhost:25980/tree/datasets

* yellow_tripdata_2016-01.csv
* yellow_tripdata_2016-02_small1.csv
* yellow_tripdata_2016-02_small2.csv
* yellow_tripdata_2016-02_small3.csv


```R
# Cargamos una versión reducida de los datos en CSV
df_nyctrips <- read.df("/root/TallerSparkR/datasets/yellow_tripdata_2016-02_small3.csv", "csv", header = "true", inferSchema = "true")
```

Estudiamos de manera superficial el dataset


```R
# Comprobamos los campos del dataset
printSchema(df_nyctrips)

# Comprobamos como son los datos:
head(df_nyctrips)

# Contamos el total del registros:
count(df_nyctrips)
```

## Selección de instancias y columnas

Para la selección de columnas y filas, usamos ``select`` y ``filter``. 

Todas las operaciones se pueden combinar para producir un nuevo dataset o ``SparkDataFrame``. **Son equivalentes a usar SPARKSQL **.

Estas operaciones son esenciales si queremos transformar el dataset en otra versión preprocesada del mismo.


```R
# Seleccionamos sólo la columna longitud, por el id de la columna
# Por ID de columna 
head(select(df_nyctrips,df_nyctrips$pickup_longitude))
```


```R
# Seleccionamos sólo la columna longitud, por el nombre de la columna.
# Por nombre de columna del dataset
head(select(df_nyctrips,"pickup_longitude"))
```

Para aplicar filtros de para las filas usamos ``filter`` que admite expresiones con operadores condicionales: 

```
    < = > ! & | ...
```


```R
# Aplicamos un filtro para ver los viajes aquellos viajes de taxi de más de 10 millas.
head(filter(df_nyctrips, df_nyctrips$trip_distance > 10 & df_nyctrips$total_amount> 20 ))
```


```R
# Aplicamos un filtro para ver los viajes aquellos viajes de taxi de más de 10 millas y el importe mayor de $ 20
head(filter(df_nyctrips, df_nyctrips$trip_distance > 10 & df_nyctrips$total_amount> 20 ))
```


```R
# Aplicamos un filtro para ver el viaje más caro en Taxi que se ha hecho:
head( agg(df_nyctrips ,max = max(df_nyctrips$total_amount)))
```


```R
# Aplicamos un filtro para ver el viaje menos caro en Taxi que se ha hecho:
head(agg(df_nyctrips, min = min(df_nyctrips$total_amount)))
```

## Uso de Agrupamiento y Agregación

Los SparkDataFrames soportan funciones de agregado despues de agrupar. 

Por ejemplo podemos:


```R
# Agrupamos por Vendedor y mostramos el número de viajes.
head(summarize(groupBy(df_nyctrips, df_nyctrips$VendorID), count = n(df_nyctrips$VendorID)))
```


```R
# Agrupamos por Vendedor y mostramos el número de viajes.
head(summarize(groupBy(df_nyctrips, df_nyctrips$VendorID), max = max(df_nyctrips$total_amount)))
```


```R
# Agrupamos y ordenamos

numsum <- summarize(groupBy(df_nyctrips, df_nyctrips$VendorID), num = n(df_nyctrips$VendorID))
head(arrange(numsum,asc(numsum$num)))
```


```R
# Agrupamos por numero de pasajeros y mostramos el numero de viajes
trips_passenger <- summarize(groupBy(df_nyctrips, df_nyctrips$passenger_count), count = n(df_nyctrips$passenger_count))
```


```R
# Cuidado con el COLLECT !
trips_df <- head(collect(trips_passenger))
```


```R
head(trips_df)
```

## Operaciones con columnas

Otras operaciones muy familiares en R, corresponden con la manipulación o transformación de valores en los registros de un dataset. En este caso la manipulación es muy sencilla:


```R
# Convertimos la columna de millas a kilómetros, igual que en R.
df_nyctrips$trip_distance <- df_nyctrips$trip_distance*1.6
```


```R
head(df_nyctrips)
```

## Añadir columnas


```R
# Usamos mutate para añadir columnas que operan con elementos de las demás columnas.

# mutate(sql_nyc,  uniform = rand(10),  normal  = randn(27))

head(mutate(df_nyctrips,  uniform = rand(10),  normal  = randn(27)))
head(mutate(df_nyctrips,  uniform =df_nyctrips$total_amount*1.1355,  normal  = randn(27)))
```


```R
# Otro modo de hacerlo es:

head(withColumn(df_nyctrips,"uniform",rand(20)))
```

### dapply -- dapplayCollect

Aplicar una función a un conjunto datos masivo con ``dapply`` y ``dapplyCollect`` 

**dapply**

Aplica una función a cada partición de un ``SparkDataFrame``. La función que será aplicada para cada partición y debería tener sólo un parámetro. La salida de la función deberá ser igualmente un data.frame. Además hay que especificar el ``schema`` del formato de los datos del ``SparkDataFrame`` resultante y deberá corresponder con tipo de datos del valor devuelto.




```R
# Hacemos una copia del SparkDataFrame para usarla en una vista temporal en SQL
createOrReplaceTempView(df_nyctrips,"slqdf_filtered_nyc")

# Hacemos una selección de los registros, donde calculamos el tiempo del viaje de cada viaje
sql_nyc <- sql("select VendorID,INT(unix_timestamp(tpep_dropoff_datetime)- unix_timestamp(tpep_pickup_datetime)) AS trip_time,passenger_count,trip_distance,total_amount from slqdf_filtered_nyc")

# Mostramos un trozo de SparkDataFrame
head(sql_nyc)

schema(sql_nyc)

# Indicamos el Schema, que debe coincidir con lo que queremos
schema <- structType(
    structField("VendorID", "integer"),
    structField("trip_time", "integer"), 
    structField("passenger_count", "integer"),
    structField("trip_distance", "double"),
    structField("total_amount", "double"),
    structField("total_amount_euro", "double")
)

# Creamos la función que hará los cambios.
new_sql_nyc <- dapply(
    sql_nyc, 
    function(x) { 
        x <- cbind(x, x$total_amount*1.1355) 
    }, 
    schema)

# Vemos el cambio
head(new_sql_nyc)

```

### gapply -- gapplyCollect


Aplica una función a cada uno de los grupos de un ``SparkDataFrame``. La función será aplicada a cada grupo del ``SparkDataFrame`` y debería tener sólo dos parámetros: agrupamiento por llave y data.frame al que corresponde esa llave. La salida de la función debería ser un data.frame. 


```R
# Esquema del SparkDataFrame
schema <- structType(
    structField("VendorID", "integer"),
    structField("trip_time", "integer"), 
    structField("passenger_count", "integer"),
    structField("trip_distance", "double"),
    structField("total_amount", "double"),
    structField("max_amount", "double")
)

# Aplicamos la función gapply. Calculamos el máximo de cada Vendedor.
result <- gapply(
    sql_nyc,
    "VendorID",
    function(key, x) {
        y <- data.frame(key, max(x$total_amount))
    },
    schema)

# Mostramos el resultado.
head(result[order(result$trip_distance, decreasing = TRUE), ])


```


```R
head(sql_nyc)

# Ahora probamos el gapplycollect: 
# Como el gapply, aplica una funcion a cada partición y luego hace un collect del resultado en un data.frame en R.
result <- gapplyCollect(
            
    sql_nyc,
    "VendorID",
    function(key, x) {
        y <- data.frame(key, max(x$trip_distance))
        colnames(y) <- c("VendorID", "max_trip_distance")
        y
    })

# Vemos el resultado.
head(result[order(result$trip_distance, decreasing = TRUE), ])
```

## Operando con SparkSQL sobre cojuntos masivos de datos.

Todas las funciones de manejo de datos que se han usado con SparkR, pueden hacerse de una forma sencilla e intuitiva  con SparkSQL


```R
# sql_nyc es nuestro DataFrameSpark de SQL
createOrReplaceTempView(sql_nyc,"slqdf_filtered_nyc")

# Hacemos una consulta para extraer el viaje de mayor distancia de cada venderor.
results <- sql("select VendorID, MAX(trip_distance) from slqdf_filtered_nyc GROUP BY VendorID ")
```


```R
# Vemos el resultado.
head(results)
```

Buscamos el total de kilómetros recorridos por cada vendedor:


```R
results <- sql("select VendorID, SUM(trip_distance) from slqdf_filtered_nyc GROUP BY VendorID ")

# Vemos el resultado
head(results)
```

Calculamos el tiempo en segundos


```R
results <- sql("select VendorID, SUM(trip_time) from slqdf_filtered_nyc GROUP BY VendorID ")

# Vemos los resultados
head(results)
```

Calculamos el tiempo en minutos


```R
results <- sql("select VendorID, SUM(trip_time)/60.0 as min_trip from slqdf_filtered_nyc GROUP BY VendorID ")

# Vemos los resultados
head(results)
```

Buscamos la ganacia total cada vendedor:


```R
results <- sql("select VendorID, SUM(total_amount)*1.10373 as Total_Amount_Euro from slqdf_filtered_nyc GROUP BY VendorID ")

# Vemos el resultado
head(results)
```

Calculamos la media y la desviación típica del tiempo de recorrido y ganancia por numero de personas: 


```R
results <- sql("select passenger_count, AVG(trip_time), AVG(total_amount) ,AVG(trip_distance)   
                from slqdf_filtered_nyc 
                GROUP BY passenger_count 
                order by passenger_count ASC ")
head(results)
```

Coeficiente de correlación


```R
results <- sql("select corr(total_amount,trip_distance) as correlation_coef
                from 
                slqdf_filtered_nyc")
# Ver resultados
head(results)
```


```R
results <- sql("select corr(total_amount,trip_time) as correlation_coef
                from 
                slqdf_filtered_nyc")
head(results)
```


```R
results <- sql("select corr(trip_time,trip_distance) as correlation_coef
                from 
                slqdf_filtered_nyc")
head(results)
```

**¿ Qué deducimos de estos coeficiente de corelación ?**

## Uso de magittr para el trabajo con los datos

El paquete magrittr permite: 

* mejorar el tiempo de desarrollo y 
* mejorar enormemente la legibilidad y mantenibilidad del código. 

Para usarlo hay que importar la biblioteca magrittr dentro del proyecto y apartir de ese momentos podemos utilizar el operador 

```
%>%
``` 

para concaternar operaciones y poder trabajar con flujos de datos y pipelines.

Provee de un operador que sirve para hacer `pipes` con el cual se puede `encauzar` un valor hacia adelante dentro de una expresión o llamada a función.

Veamos todas las operaciones que hemos realizado sobre los datos y su equivalente con `pipes`.


```R
# Hacemos una copia del SparkDataFrame para usarla en una vista temporal en SQL
createOrReplaceTempView(df_nyctrips,"slqdf_filtered_nyc")

# Hacemos una selección de los registros, donde calculamos el tiempo del viaje de cada viaje
sql_nyc <- sql("select VendorID,INT(unix_timestamp(tpep_dropoff_datetime)- unix_timestamp(tpep_pickup_datetime)) AS trip_time,passenger_count,trip_distance,total_amount from slqdf_filtered_nyc")

head(sql_nyc)

```


```R
# Usamos magrittr
library(magrittr)

# results <- sql("select VendorID, MAX(trip_distance) from slqdf_filtered_nyc GROUP BY VendorID ")
#summarize(groupBy(df_nyctrips, df_nyctrips$passenger_count), count = n(df_nyctrips$passenger_count))

df_nyctrips %>% 
        groupBy( df_nyctrips$passenger_count) %>%
        summarize(count = n(df_nyctrips$passenger_count)) %>%
        head()
```


```R
df_nyctrips %>% 
        groupBy( df_nyctrips$passenger_count) %>%
        summarize( avg_total_amount=avg(df_nyctrips$total_amount) ,avg_trip_distance=avg(df_nyctrips$trip_distance)) %>%
        head()
        
```


```R
df_nyctrips %>% 
         groupBy( df_nyctrips$passenger_count) %>%
        summarize(min = min(df_nyctrips$trip_distance),max = max(df_nyctrips$trip_distance)) %>%
        head()
```


```R
df_nyctrips %>% 
         groupBy( df_nyctrips$passenger_count, hour(df_nyctrips$tpep_pickup_datetime)) %>%
        summarize(total_pickup = n(df_nyctrips$tpep_pickup_datetime)) %>%
        head()
```


```R
count(sql_nyc)
num_regs <- as.integer(count(sql_nyc))

# Mostramos el número de registros
print(num_regs)
```

** ¿Qué es lo mejor: `pipes`, SPARKSQL o funciones? **

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)


Escribe todas las pruebas en R que necesites a partir de aquí

<HR>
