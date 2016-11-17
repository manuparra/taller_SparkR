
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>

![lpa](https://sites.google.com/site/manuparra/home/logoparty.png)


![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

<HR>

<span style="font-size:2.0em">Visualización dinámica de datos con SparkR</span>
<HR>

En esta parte final del taller, vamos a trabajar con una herramienta llamada APACHE ZEPPELIN.

** Apache Zeppelin **

Es un NOTEBOOK  (similar a Jupyter, del que ya sois expertos), que permite el análisis interactivo de datos.
Se pueden realizar documentos dirijidos a datos, interactivos y colaborativos, con SPARKSQL, SCALA, R, python y muchos más.

Es un NOTEBOOK Multiproposito y contiene:

* Data Ingestion
* Data Discovery
* Data Analytics
* Data Visualization & Collaboration

Usaremos la versión 0.62 de Apache Zeppelin.

Para inicial el entorno de trabajo con Apache ZEPPELIN accedemos a la Máquina Virtual:


    cd /root/zeppelin/
    
    sh conf/zeppeling-env.sh
    
    ./bin/zeppelin-daemon.sh start

Despues de ejecutar estos comandos se habilita un nuevo puerto en la Máquina Virtual que permite acceder a la siguiente URL:

http://localhost:9090

Veremos está aplicación en la Nube:

![zeppelin](https://sites.google.com/site/manuparra/home/zeppelinb.png)

Para comenzar a trabajar, usamos ``create new note``. 

Le asignamos un nombre. Una vez dentro de la nueva nota usamos:

```
    %spark.r
```

Con esto le indicamos que queremos crear un notebook para trabajar con Spark + R.

En Zeppelin, no es necesario abrir la conexión con Spark como se hacía con SparkR, ya que Zeppelin abre una conexión por defecto que se ha confingurado de antemano. Tampoco es necesario importar la biblioteca `sparkR`.

Igualmente podemos trabajar con ``sparklry``, pero no tendremos las mejoras que ofrece sparkR para la visualización dinámica de datos.

```
    df_nyctrips <- read.df("/root/TallerSparkR/datasets/yellow_tripdata_2016-02_small3.csv", "csv", header = "true", inferSchema = "true")
    createOrReplaceTempView(df_nyctrips,"slqdf_filtered_nyc")
```


Ahora usaremos una celda de tipo SQL

```
    %sql 
```

Aquí podemos escribir sentencias SQL para poder tratar datos y visualizarlos.









```R

```
