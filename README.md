# Taller de "Procesamiento masivo de datos con SparkR"
=================

   * [Taller de "Procesamiento masivo de datos con SparkR"](#taller-de-procesamiento-masivo-de-datos-con-sparkr)      
      * [Objetivos](#objetivos)
      * [Contenidos](#contenido)
   * [Material del taller. ¿Cómo comenzar?](#material-del-taller)
      * [Descarga de la Máquina Virtual VirtualBox: All-in-one](#descarga-de-la-máquina-virtual-virtualbox-all-in-one)
      * [Inicio del entorno de trabajo](#inicio-del-entorno-de-trabajo)
      * [Instalación de SparkR   extras manualmente para CentOS 7](#instalación-de-sparkr--extras-manualmente-para-centos-7)






![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

## VIII Jornadas Usuarios de R, Albacete (España) el 17-18 de noviembre de 2016

![Jornadas_R_Albacete](https://sites.google.com/site/manuparra/home/jornadas_R_albacete.png)


## Imparte el taller:


<img src="https://sites.google.com/site/manuparra/home/manuparra.jpg">

Manuel Jesús Parra Royón  <a href="mailto:manuelparra@decsai.ugr.es">manuelparra@decsai.ugr.es</a>  

PhD student. BigData time series data mining.

<a href="http://sci2s.ugr.es/">Departamento de Ciencias de la Computación e Inteligencia Artificial.</A>

<a href="http://sci2s.ugr.es/dicits/">Distributed Computational Intelligence and Time Series Laboratory </A>

<a href="http://www.ugr.es">Universidad de Granada</a>





## Objetivos

Los objetivos del taller de SparkR son los siguientes:

* Conocer la problemática del procesamiento masivo de datos.
* Fijar concepto y práctica sobre uso R sobre Spark para el procesado masivo de datos.
* Instalar y configurar el sistema completo para poder trabajar con R y Spark.
* Trabajar con datos masivos (filtrado, agregado, transformaciones), procesar datasets masivos son SparkSQL, etc..
* Analizar datasets con las librerías de Machine Learning de los paquetes SparkR y sparklry
* Utilizar herramientas para visualizar los datos de datasets masivos.


## Contenido

En el taller de procesamiento masivo de datos con SparkR veremos lo siguiente:

1.- Introducción al procesamiento de datos masivos.
    Breve introducción al procesamiento de datos, el problema de trabajar con grandes conjuntos de datos, Hadoop,Motivación de Spark, características, etc...<BR>

2.- **Notas sobre R, Spark y SparkR**<BR>
    Introducción a R, motivación de R para datos 'pequeños' y datos 'grandes', Spark y sus características, biblioteca de SparkR para análisis de datos masivos con R.<BR>

3.- **Instalación de las herramientas necesarias para el taller**<BR>
    Veremos todas las herramientas necesarias para poder trabajar con el entorno de SparkR, así como la instalación y puesta en marcha de toda la infraestructura necesaria para el taller. Inicio del entorno de trabajo habitual para trabajar en el taller.<BR>

4.- **Entorno de trabajo del taller**<BR>
    Detalles del manejo del entorno de trabajo con JupyterNotebooks y Spark + R<BR>

5.- **Inicio del entorno de trabajo**<BR>
    Flujo de trabajo con Spark + R<BR>

6.- **Primeros pasos con SparkR**<BR>
    Trabajo con ejemplos de uso de Spark + R <BR>

7.- **Lectura y Escritura de datos con SparkR**<BR>
    Trabajo con fuentes de datos, y tipos de conjuntos de datos, CSV, JSON, Parquet, ... Lectura y Escritura. Esquemas, y breve trabajo con SparkSQL. <BR>

8.- **Operaciones y procesado de SparkDataFrames**<BR>
    Trabajamos y procesamos conjuntos de datos masivos con SparkSQL y funciones de agregación, filtrado, selección, etc. Usamos flujos de trabajo con magrittr. Revisamos la funcionalidad completa de la biblioteca de SparkR.<BR>

9.- **Minería de datos con la biblioteca de SparkR**<BR>
    Aplicamos las técnicas de minería de datos y Machine Learning que proporciona SparkR: GLM, KMeans, NaiveBayes y AFT.<BR>

10.- **Minería de datos con la biblioteca sparklyr**<BR>
    Utilizamos la funcionalidad de la biblioteca ``sparklyr`` para procesar conjuntos de datos. Aplicamos los métodos de minería de datos y otras operaciones.<BR>

11.- **Visualización de datos**<BR>
    Visualización de datos masivos con la herramienta Zeppelin (beta) y ``spark.ggplot2``.

# Material del taller

## Descarga de la Máquina Virtual VirtualBox: All-in-one

La máquina virtual para el Taller completo contiene todas las herramientas necesitas para el desarrollo del trabajo, por lo tanto es la opción más sencilla para poder empezar y ponerse manos a la obra con el taller.


Descargar la máquina virtual del taller: https://drive.google.com/file/d/0ByPBMv-S_GMEakRCVVRTejZKVm8/view?usp=sharing (aprox: 4 GB)


Los datos de acceso a la Máquina Virtual son:

	usuario: root
	clave: sparkR

**Requisitos necesarios para trabajar con la Máquina Virtual:**

* Tener instalado VIRTUALBOX, disponible en: https://www.virtualbox.org/wiki/Downloads
* Disponer de al menos 2GB de RAM para la Máquina Virtual
* El PC debe ser de 64bits y contar con al menos 4GB de RAM (2GB para la MVirtual y otros 2GB para el PC)
* Compatible con Windows, Mac OSX y Linux


## Inicio del entorno de trabajo en la Máquina Virtual

Tanto si te descagas la Máquina virtual, como si instalas todo manualmente, es necesario ejecutar los siguientes comandos dentro de la Máquina Virtual antes de trabajar en el taller:

1 Arrancamos el entorno de Spark:

```
	/usr/local/spark/sbin/start-all.sh
```

2 Arrancamos JupyterNotebook:

```
	jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0
```

## Entornos de trabajo disponibles :

Hay 2 entornos de trabajo disponibles para trabajar con la Máquina Virtual en SparkR.

### Jupyter

Para usar SparkR desde Jupyter Notebooks, accede desde tu navegador a:

```
  http://localhost:25980
```

![JupyterNotebook](https://sites.google.com/site/manuparra/home/jupyter.jpg)


### RStudio

Para usar SparkR desde RStudio, accede desde tu navegador a:

```
  http://localhost:8787
```

El usuario por defecto es: ```test``` y la clave: ```test```

![RStudio](https://sites.google.com/site/manuparra/home/rstudio.jpg)

Para trabajar con SparkR desde RStudio, es necesario indicar al principio de los scripts en R:

```
# Biblioteca y ruta absoluta a SparkR
.libPaths(c(file.path("/usr/local/spark/","R/lib/"),.libPaths()))
library(SparkR)
```


## Taller práctico.

Puedes empezar el taller práctico tanto desde Jupyter como RStudio, siguiendo los siguientes enlaces a la documentación: 


* [Parte 2. S01. Inicio del entorno de trabajo](../../blob/master/Parte%202.%20S01.%20Inicio%20del%20entorno%20de%20trabajo%20con%20SparkR.ipynb)

* [Parte 2. S02. Primer ejemplo con SparkR](../../blob/master/Parte%202.%20S02.%20Primer%20ejemplo%20con%20SparkR.ipynb)

* [Parte 2. S03. Lectura y Escritura de datos en SparkR](../../blob/master/Parte%202.%20S03.%20Lectura%20y%20escritura%20de%20datos%20con%20SparkR.ipynb)

* [Parte 2. S04. Operaciones con SparkDataFrames](../../blob/master/Parte%202.%20S04.%20Operaciones%20con%20SparkDataFrames.ipynb)

* [Parte 2. S05. Minería de datos y Machine Learning con SparkR](../../blob/master/Parte%202.%20S05.%20Mineria%20de%20datos%20y%20Machine%20Learning%20con%20SparkR.ipynb)

* [Parte 2. S06. Minería de datos y Machine Learning con sparklyr](../../blob/master/Parte%202.%20S06.%20Mineria%20de%20datos%20y%20Machine%20Learning%20con%20sparklyr.ipynb)

* [Parte 2. S07. Visualización de datos masivos con SparkR y Zeppelin](../../blob/master/Parte%202.%20S07.%20Visualizacion%20dinamica%20de%20datos%20con%20SparkR.ipynb)




