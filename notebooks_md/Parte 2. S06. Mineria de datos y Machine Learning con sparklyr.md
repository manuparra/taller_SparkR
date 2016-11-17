
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Machine Learning con sparklyr — Interfaz R para Apache Spark y la MLLib

** Biblioteca de Machine Learning Spark (MLlib) con la Interfaz sparklyr **

*Carácteristicas fundamentales:*

La bilbioteca ```sparklyr``` proporciona enlaces a la biblioteca de ML distribuida de Spark. En particular, sparklyr le permite acceder a las rutinas de ML proporcionadas por el paquete ``spark.ml`` de Spark. Además junto con la interfaz ``dplyr`` de ``sparklyr``, puede crear y afinar fácilmente los flujos de trabajo de ML en Spark, orquestados enteramente dentro de ``R``.

Sparklyr proporciona tres familias de funciones que puede utilizar con el aprendizaje de máquina Spark:

* Algoritmos de aprendizaje automático para el análisis de datos (funciones  ``ml_*``)
* Transformadores de características para manipular características individuales (funciones ``ft_*``)
* Funciones para manipular Spark DataFrames (funciones ``sdf_*``)

El flujo de trabajo para el análisis de datos  con ``sparklyr`` podría estar compuesto de las siguientes etapas:

- Realizar consultas SQL a través de la interfaz sparklyr dplyr,
- Utilizar la familia de funciones ``sdf_`` * y ``ft_`` * para generar nuevas columnas o particionar su conjunto de datos,
- Elegir un algoritmo de aprendizaje automático apropiado de la familia de funciones ``ml_ *`` para modelar los datos,
- Inspeccionar la calidad del ajuste de su modelo y usarlo para hacer predicciones con nuevos datos,
- Recopilar los resultados para la visualización y análisis posterior en R

<HR size=1>

En esta sección vamos a trabajar con las herramientas que proporciona la biblioteca ``sparklyr``.

Como vamos a ver, la funcionalidad es muy similar a la de la biblioteca SparkR, aunque cambia el nombre de las funciones y varios extras más.

## Inicialización del entorno

<span style="background-color:red;color:white">&nbsp; &nbsp; Es necesario reiniciar Spark para poder trabajar con esta sesión de sparklyr &nbsp; &nbsp; <BR>&nbsp; &nbsp; Para ello, ve a la Máquina Virtual para el proceso de Spark, para luego volver a cargarlo &nbsp; &nbsp;</span> 

Ahora para conectar con Spark y abrir una sesión usaremos la siguiente sintaxis (simiar a la del paquete SparkR aunque  con ligeras diferencias):


```R
# Usamos la libreria sparklyr y dplyr.
# Ajustar el nivel de visualización de errores !
options(warn=0)

# Incluimos la bilbioteca de sparklyr
library(sparklyr)
# Usamos la biblioteca para el manejo de los datos.
library(dplyr)

# Abrimos la conexión. Importante indicar la versión de Spark que tenemos instalada. En nuestro caso tenemos la 2.0.1
sc <- spark_connect(master = "local", version = "2.0.1")
```

# Características

La biblioteca sparklyr tiene asociado un paquete que hace de complemento ideal para la manipulación de datos masivos. Este paquete es ``dplyr``  un paquete en R para trabajar con datos estructurados dentro y fuera de R. ``dplyr`` hace la manipulación de datos muy sencilla para los usuarios de R, además ofrece interfaces consistentes y con un buen rendimiento. La librería tiene las siguientes funcionalidades básicas: 

* Seleccion, filtrado y agregación.
* Funciones windows (para muestreo).
* Funciones de JOIN para Dataframes.
* Funciones Collect para transformar datos de Spark a R.
* ...


## Lectura y escritura de datos con ``sparklyr``

** Para la lectura y escritura de datos tenemos las siguientes funciones:**

- ``spark_read_csv``: Lee un CSV y el resultado lo hace compatible con las funciones de ``dplyr``.
- ``spark_read_json``: Lee un fichero JSON y el resultado es compatible con la interfaz de ``dplyr``.
- ``spark_read_parquet``: Lee un fichero PARQUET.

Además del formato de los datos, Spark soporta la lectura de datos desde una variedad de fuentes de datos. Estos incluyen, almacenamiento en  HDFS (hdfs:// protocol), Amazon S3 (s3n:// protocol), o ficheros locales disponibles en en los nodos (file:// protocol).

** Para la escritura de DataFrames existen las mismas funcione según el tipo de fuente de datos:**

- ``spark_write_csv``: Escribe a CSV y recibe una fuente de datos compatible con ``dplyr``.
- ``spark_write_json``: Escribe a  JSON.
- ``spark_write_parquet``: Escribe a parquet desde cualquier fuente compatible con ``dplyr``.


```R
# Lectura de un fichero de datos CSV

tttm <- spark_read_csv(sc, 
                       name="tttm", 
                       path="/root/TallerSparkR/datasets/databig/ECBDL14_10tst.data", 
                       delimiter = ",", 
                       header=TRUE,
                       overwrite = TRUE)

```

¿Carga los datos rápido?


```R
count(tttm)
```

La escritura de datos es sencilla y simplemente requiere la función concreta para almacenar los datos.

El valor del parámetros ``path`` puede ser de diferente origen de datos:

    * HDFS (``path="hdfs://...."``)
    * AmazonS3 (``path="s3://..."``)
    * Local (``path="..."``)


```R
# Escritura de un fichero de datos CSV

spark_write_csv(tttm, 
                path="/root/TallerSparkR/datasets/databig/ECBDL14_10tst_saved.data", 
                delimiter = ",", 
                header=TRUE)


```

Para los demás formatos, se usa la función correspondiente, teniendo en cuenta que la entrada de la función de escritura, siempre tiene que ser compatible con ``dplyr``


```R
#spark_write_json(tttm, .....

#spark_write_parquet(tttm, .....
```

## Filtrado, selección, agrupación.

Las funciones son comandos dplyr para manipular datos... for manipulating data. When connected to a Spark DataFrame, dplyr translates the commands into Spark SQL statements. Remote data sources use exactly the same five verbs as local data sources. Here are the five verbs with their corresponding SQL commands:

* select ~ SELECT
* filter ~ WHERE
* arrange ~ ORDER
* summarise ~ aggregators: sum, min, sd, etc.
* mutate ~ operators: +, *, log, etc.




```R
head(tttm)
```

Los resultados y las tablas, ya no son tan "bonitas" como con la biblioteca SparkR.


```R
# Seleccionamos las columnas que queremos con select
select(tttm, f5,f10,class)
```

Filtrado de instancias


```R
# Hago un filtrado de datos tal que la columna f5 sea mayor de 0 y la clase sea 1
res_filtered<-filter(tttm, f5 > 0 & class==1)

# Contamos los registros.
count(res_filtered)
```

### ¿Cómo es el dataset: Balanceado o no Balanceado con respecto a la variable de clase ``class`` ?


```R
# Agrupamos el dataset por clase y luego contamos los registros.

# En SQL sería select count(class) ...group by class
num_regs <- as.integer(collect(count(tttm)))

# Mostramos el número de registros
print(num_regs)

# Agrupamos por clase y contamos el numero de elementos de cada clase, ademñas 
# añadimos una columna que calcula el porcentaje que supone cada clase del total
summarize( group_by(tttm,class), count = n(), percent= n()/num_regs *100.0)
```

La clase con valor *1*, tiene sólo un *1.67%* de instancias del total, por tanto, es un dataset NO balanceado. Interesante !.

### Preprocesado de dataframes


```R
library(magrittr)


# También podemos usar magittr para hacer los mismo de un modo más claro. El ejemplo de arriba y este son lo mismo.
tttm %>% 
    group_by(class) %>%
    summarize(count = n(), percent= n()/num_regs *100.0 )

tttm %>% 
    group_by(class) %>%
    summarize(count = n(), maxf1=max(f1),minf1= min(f1))
head(tttm)

```


```R

# Añadimos una columna con el doble del valor de la columna f6
tttm %>%
    mutate(f6*2)

```


```R
# Si del anterior ejemplo queremos construir un nuevo dataframe, usamos 
# select para tomar las columnas que nos interesen.
tttm %>%
    mutate(f6*2)%>%
    select (f1,f8, `f6 * 2` )
```


```R
# La función arrange permite aplicar funciones de ordenación, ... sobre dataframes.

# Si del anterior ejemplo queremos ordenar la columna f8 y filtrar luego por f1 mayor de 0.2
tttm %>%
    mutate(f6*2) %>%
    select (f1,f8, `f6 * 2` ) %>%
    arrange (desc(f8)) %>%
    filter (f1> 0.2)
```

### Muestreo aleatorio de datos.

El uso de sampling aleatorio sobre los datos es muy usado para trabajar con dataframes que serán usados para su  tratamiento con Machine Learning o Minería de datos.


```R
# Crea una muestra de los datos.  10% de los datos
test <- sample_frac(tttm, 0.10)
count(test)
```


```R
# Crea una muestra aleatoria de los datos.  80% de los datos
train <- sample_frac(tttm, 0.90)
count(train)
```

También podemos usar:


```R
# sdf_partition(dataset,training = 0.5, test = 0.5, seed = 1099)
```

Vamos a comprobar como está ahora el porcetaje de balanceo de las clases:

Comprobamos que más o menos están igual que en proporción que con el conjunto grande cuando hacemos un muestreo.


```R
# También podemos usar magittr para hacer los mismo de un modo más claro. El ejemplo de arriba y este son lo mismo.
num_regs <- as.integer(collect(count(test)))

print(num_regs)
# Calculamos la distribución de clases
test %>% 
    group_by(class) %>%
    summarize(count = n(), percent= n()/num_regs *100.0 )

```


```R
# También podemos usar magittr para hacer los mismo de un modo más claro. El ejemplo de arriba y este son lo mismo.
num_regs <- as.integer(collect(count(train)))

# Calculamos la distribución de clases
train %>% 
    group_by(class) %>%
    summarize(count = n(), percent= n()/num_regs *100.0 )
```

### Preparación del dataset para Machine Learning

Al tener un conjunto de datos grande y desbalanceado, podemos tomar varias alternativas para trabajar con el mismo: 

* Hacer un sobremuestreo de la clase minoritaria
* Hacer un submuestreo de la clase mayoritaria

Vamos a trabajar con los algoritmos de ML utilizando submuestreo ``Random Undersampling`` básico, para ello:

1. Calculamos el número de instacias de la clase minoritaria.
2. Hacemos un muestreo sólo de la clase mayoritaria para igualar en instancias la clase minoritaria.
3. Fusionamos ambos muestreos


```R
# Contamos los registros de la clase minoritaria
regs_minor <- tttm %>% 
            filter(class==1) %>% 
            count %>%
            collect %>% 
            as.integer


# Extraemos un sample con un numero similar de instancias que de la clase minoritaria
only_class_0 <- tttm %>% 
        filter(class==0) %>%
        sdf_sample(regs_minor, fraction=as.double(regs_minor/as.integer(collect(count(tttm)))))

# Extraemos las instancias de la clase minoritaria 
only_class_1 <- tttm %>% 
            filter(class==1)


```

Contamos los registros de ambos


```R

count(only_class_0)
# as.integer(collect(count(only_class_0)))
# only_class_0 %>% count %>% collect %>% as.integer
count(only_class_1)
# as.integer(collect(count(only_class_1)))
# only_class_1 %>% count %>% collect %>% as.integer

```

¿Por qué no tienen el mismo tamaño?. **Precisión**.

Unimos los dos dataframes con ``rbind``


```R
# Unimos 
ds_ml <- rbind(only_class_1,only_class_0, name="ds_ml")

# Calculamos el tamaño de cada clase.
ds_ml %>% 
        filter(class==0) %>%
        count()

ds_ml %>% 
        filter(class==1) %>%
        count()

```

### Particionado de datos para conjuntos de Entrenamiento y Prueba (TRAIN & TEST)

Para el particionado usamos una función de sparklyr: ``sdf_partition``


```R
# La función sdf_partition devuelve el dataframe separado en training y test.
# Para acceder a cada dataframe usamos ...$training , ...$test
partitions <- sdf_partition(ds_ml,training=0.80,test=0.20)
```

Contamos el número de registros de cada conjunto:


```R
count(partitions$test)
```


```R
count(partitions$training)
```

# Algoritmos de ML para el análisis de datos

Con Spark + R y la biblioteca sparklyr se pueden orquestar algoritmos de ML en un cluster con Spark. 

Estas funciones de ML, conectan directamente con la API de Spark

1. Ajustamos un modelo a nuestro conjunto de entrenamiento
2. Evaluamos nuestro rendimiento predictivo sobre el conjunto de test.

La biblioteca tiene mayor número de funciones para la minería de datos que la propia de SparkR.


### Regresión Lineal

En estadística la regresión lineal o ajuste lineal es un modelo matemático usado para aproximar la relación de dependencia entre una variable dependiente Y, las variables independientes Xi y un término aleatorio ε


```R
# La función para la regresión lineal puede usarse con la sintaxis propia 
# de la función y tambien con la forma tradicional de R: formulae

# Opción 1
#model <- partitions$training %>%
#    ml_linear_regression(response = "f1", features = c("f2","f3"))

# Opción 2
#model <- partitions$training %>%
#    ml_linear_regression(f1~f2+f3)

# Opción 3
model <- ml_linear_regression(partitions$training,f1~f7)
```


```R
# Vemos la calidad del ajuste ...
summary(model)
```


```R
predicted <- predict(model, newdata = partitions$test)

# extract the true 'strength' values from our test dataset
actual <- partitions$test %>%
  select(f1) %>%
  collect() %>%
  `[[`("f1")
     
```


```R

# produce a data.frame housing our predicted + actual 'strength' values
data <- data.frame(
  predicted = predicted,
  actual    = actual
) 
```


```R
# plot predicted vs. actual values
library(ggplot2)
ggplot(data, aes(x = actual, y = predicted)) +
  geom_abline(lty = "dashed", col = "red") +
  geom_point(size=0.2) +
  theme(plot.title = element_text(hjust = 0.5)) +
  coord_fixed(ratio = 4) +
  labs(
    x = "Actual F1",
    y = "Predicted F1",
    title = "Predicted vs. Actual Feature 1"
  )
```

### Regresión logística

En estadística, la regresión logística es un tipo de análisis de regresión utilizado para predecir el resultado de una variable categórica (una variable que puede adoptar un número limitado de categorías) en función de las variables independientes o predictoras. Es útil para modelar la probabilidad de un evento ocurriendo como función de otros factores. El análisis de regresión logística se enmarca en el conjunto de Modelos Lineales Generalizados (GLM por sus siglas en inglés)


```R
# Aplica la función de regresión logística
ml_log <- partitions$training %>%
    ml_logistic_regression(response = "class", features = c("f1","f2","f3","f4"))
```


```R
predicted <- predict(model, newdata = partitions$test)
```

<HR>

### K-Means

K-means es un método de agrupamiento, que tiene como objetivo la partición de un conjunto de n observaciones en k grupos en el que cada observación pertenece al grupo cuyo valor medio es más cercano. 

Forma parte de las técnicas de ``clustering``



```R
#model <- ml_kmeans(select(iris_tbl,Petal_Width, Petal_Length),centers = 3)

model <- iris_tbl %>%
  select(Petal_Width, Petal_Length) %>%
  ml_kmeans(centers = 3)

# Cuidado con collect !!!
iris_tbl %>%
  select(Petal_Width, Petal_Length) %>%
  collect %>%
  ggplot(aes(Petal_Length, Petal_Width)) +
    geom_point(data = model$centers, aes(Petal_Width, Petal_Length), size = 60, alpha = 0.1) +
    geom_point(aes(Petal_Width, Petal_Length), size = 2, alpha = 0.5)
```

### Random Forest

Random forest también conocido en castellano como '"Selvas Aleatorias"' es una combinación de árboles predictores tal que cada árbol depende de los valores de un vector aleatorio probado independientemente y con la misma distribución para cada uno de estos. Es una modificación sustancial de bagging que construye una larga colección de árboles no correlacionados y luego los promedia. Se usa para regresión y para clasificación.



```R
training_cv <- partitions$training %>% 
        select(f1,f2,f3,f4,class) %>%
        mutate(class1=as.character(class)) %>%
        select(f1,f2,f3,f4,class=class1)



ml_rf <- ml_random_forest(training_cv,response="class",features=c("f1","f2","f3","f4"))
```


```R
print(fit_model)

sdf_predict(fit_model, training_cv)
```


```R
#Probamos con el dataset completo. Cuidado !!! Coste en tiempo elevado !
ml_rf <- ml_random_forest(tttm,response="class",features=c("f1","f2","f3","f4"),num.trees = 20,type = "classification")

#sdf_predict(ml_rf, partitions$test)
```


```R
ml_rf
print(ml_score)
```

### Análisis de componentes principales

En estadística, el análisis de componentes principales es una técnica utilizada para reducir la dimensionalidad de un conjunto de datos.

Técnicamente, el PCA busca la proyección según la cual los datos queden mejor representados en términos de mínimos cuadrados. Esta convierte un conjunto de observaciones de variables posiblemente correlacionadas en un conjunto de valores de variables sin correlación lineal llamadas componentes principales.

El PCA se emplea sobre todo en análisis exploratorio de datos y para construir modelos predictivos.


```R
# PCA
pca_model <- ml_pca(partitions$training)
print(pca_model)

# Calculate explained variance
# pca_model$sdev^2 / sum(pca_model$sdev^2) # Sólo para R

```

### Comparación de modelos 


```R
# Hacemos una lista para verificar que modelo nos da mejores resultados.
ml_models <- list(
  "Logistic" = ml_log,
#  "Decision Tree" = ml_dt,
  "Random Forest" = ml_rf
#  "Gradient Boosted Trees" = ml_gbt,
#  "Naive Bayes" = ml_nb,
#  "Neural Net" = ml_nn
)

# Create a function for scoring
score_test_data <- function(model, data=partitions$test){
  pred <- sdf_predict(model, data)
  select(pred, class, prediction)
}

# Score all the models
ml_score <- lapply(ml_models, score_test_data)
```

<HR>

### Cierre de la conexión con SPARK



```R
spark_disconnect(sc)
```

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)



```R

```
