
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Aplicando técnicas de Machine Learning en SparkR

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

Como siempre para todos nuestros scripts con SparkR, cargamos la biblioteca, y creamos una nueva sesión de SparkR.


```R
.libPaths(c(file.path(Sys.getenv("SPARK_HOME"),"R/lib/"),.libPaths()))
library(SparkR)
sparkR.session(appName="EntornoInicio", master = "local[*]", sparkConfig = list(spark.driver.memory = "1g"))
```

<HR>
La biblioteca de ``SparkR`` actualmente soporta los siguientes algoritmos de aprendizaje automático : 

- modelo lineal generalizado, 
- modelo de regresión de supervivencia con tiempo de fallo acelerado (AFT), 
- modelo Bayes Naive y 
- modelo KMeans. 

SparkR utiliza MLlib para entrenar el modelo. Por tanto se puede analizar el resumen del modelo ajustado, predecir y   hacer predicciones sobre nuevos datos y escribir/leer el modelo para guardar / cargar los modelos ajustados. 

Además de ello, al igual que ocurre cuando usamos cualquier funcion en R, SparkR soporta el uso de fómulas, lo cual mejora bastante la adopción de SparkR para análisis de datos másivos.  SparkR soporta un subconjunto de los operadores de fórmula R disponibles para el ajuste del modelo, incluyendo '~', '.', ':', '+' y '-'.

####  Funciones enmascaradas

Dado que parte de SparkR está modelado en el paquete dplyr, ciertas funciones de SparkR comparten los mismos nombres con los de dplyr. Dependiendo del orden de carga de los dos paquetes, algunas funciones del paquete cargado primero son enmascaradas por las del paquete cargado después. 

``cov in package:stats``

``filter in package:stats``

``sample in package:base``


Por tanto hay siempre que usar el paquete que queramos usar al final de la importación de las bibliotecas para que se haga efectiva la función que queremos para SparkR.


## Algoritmos

El paquete SparkR soporta las siguientes funcionalidades de Machine Learning y Data mining

### Generalized Linear Model

Usamos la función de R


```R

gaussianDF <- iris
gaussianTestDF <- iris
gaussianGLM <- glm(data = gaussianDF, Sepal.Length ~ Sepal.Width + Species, family = "gaussian")

summary(gaussianGLM)

```

Usamos la función para glm de SparkR


```R

irisDF <- suppressWarnings(createDataFrame(iris))
# Fit a generalized linear model of family "gaussian" with spark.glm
gaussianDF <- irisDF
gaussianTestDF <- irisDF
gaussianGLM <- spark.glm(gaussianDF, Sepal_Length ~ Sepal_Width + Species, family = "gaussian")

# Model summary
summary(gaussianGLM)

```

**¿Qué diferencias hay entre las dos?**

Calculamos el modelo


```R
# Calculamos la predicción
gaussianPredictions <- predict(gaussianGLM, gaussianTestDF)
# Mostramos las predicciones
showDF(gaussianPredictions)

# Usamos la función de R de glm con la familia gaussian
gaussianGLM2 <- glm(Sepal_Length ~ Sepal_Width + Species, gaussianDF, family = "gaussian")
summary(gaussianGLM2)

# Ahora usamos la función de glm de spark para la familia binomial.
binomialDF <- filter(irisDF, irisDF$Species != "setosa")
binomialTestDF <- binomialDF
binomialGLM <- spark.glm(binomialDF, Species ~ Sepal_Length + Sepal_Width, family = "binomial")

# Imprimimos el modelo
summary(binomialGLM)

# Obtenemos la predicción
binomialPredictions <- predict(binomialGLM, binomialTestDF)
showDF(binomialPredictions)
```

### Accelerated Failure Time (AFT) Survival Regression Model

``spark.survreg()`` ajusta a un modelo de regresión AFT (accelerated failure time) sobre un SparkDataFrame. Para esta función no se permite el uso del operador . en la formula.


```R
# Use the ovarian dataset available in R survival package
library(survival)

ovarianDF <- suppressWarnings(createDataFrame(ovarian))

head(ovarianDF)

aftDF <- ovarianDF
aftTestDF <- ovarianDF

#Aplicamos la función
aftModel <- spark.survreg(aftDF, Surv(futime, fustat) ~ ecog_ps + rx)

# Model summary
summary(aftModel)

# Prediction
aftPredictions <- predict(aftModel, aftTestDF)
showDF(aftPredictions)
```

### K-Means


```R

library(ggplot2)
ggplot(iris, aes(Petal.Length, Petal.Width, color = Species)) + geom_point()

set.seed(20)
irisCluster <- kmeans(iris[, 3:4], 3, nstart = 20)
irisCluster

```


```R
# Ajustamos un modelo k-medias. 

irisDF <- suppressWarnings(createDataFrame(iris))
kmeansDF <- irisDF
kmeansTestDF <- irisDF
kmeansModel <- spark.kmeans(kmeansDF, ~ Sepal_Length + Sepal_Width + Petal_Length + Petal_Width,
                            k = 3)

# Vemos el resumen
summary(kmeansModel)

# Vemos los resultados del ajuste
showDF(fitted(kmeansModel))

# y vemos la predicción
kmeansPredictions <- predict(kmeansModel, kmeansTestDF)
showDF(kmeansPredictions)

# Mostramos la información de los grupos.
table(kmeansModel$cluster, iris$Species)

```

### Naive Bayes



```R
### Aplicamos NAIVE.BAYES 

titanic <- as.data.frame(Titanic)
titanicDF <- createDataFrame(titanic[titanic$Freq > 0, -5])
nbDF <- titanicDF
nbTestDF <- titanicDF
nbModel <- spark.naiveBayes(nbDF, Survived ~ Class + Sex + Age)

# Resumen del modelo
summary(nbModel)

# Predicción
nbPredictions <- predict(nbModel, nbTestDF)
showDF(nbPredictions)

```

### Creación de Conjuntos de entrenamiento y prueba

Existen varias formas de hacer los conjuntos de prueba y test. Se pueden usar las funciones de muestreo (sample) que trabajan sobre los SparkDataFrames.


```R
train_df <- sample(df_training, withReplacement=FALSE, fraction=0.85, seed=42)
test_df <- except(df_training, train_df)

count(train_df)
count(test_df)
```

### Persistencia de los MODELOS de ML.

Si necesitamos almacenar el modelo que hemos ajustado podemo hacerlo mediante el uso de la funcion ``write.ml``. 
Al igual que luego podemos recuperarlo con ``read.ml``.


```R
modelPath <- tempfile(pattern = "ml", fileext = ".tmp")
write.ml(gaussianGLM, modelPath)
gaussianGLM2 <- read.ml(modelPath)
```

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)



```R

```
