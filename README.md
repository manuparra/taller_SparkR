Indice: Procesamiento masivo de datos con SparkR
=================

   * [Taller de "Procesamiento masivo de datos con SparkR"](#taller-de-procesamiento-masivo-de-datos-con-sparkr)      
      * [Objetivos](#objetivos)
      * [Contenidos](#contenido)
   * [Material del taller. ¿Cómo comenzar?](#material-del-taller)
      * [Descarga de la Máquina Virtual VirtualBox: All-in-one](#descarga-de-la-máquina-virtual-virtualbox-all-in-one)
      * [Inicio del entorno de trabajo](#inicio-del-entorno-de-trabajo)
      * [Instalación de SparkR   extras manualmente para CentOS 7](#instalación-de-sparkr--extras-manualmente-para-centos-7)




# Taller de "Procesamiento masivo de datos con SparkR"

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)

## VIII Jornadas Usuarios de R, Albacete (España) el 17-18 de noviembre de 2016

![Jornadas_R_Albacete](https://sites.google.com/site/manuparra/home/jornadas_R_albacete.png)


## Objetivos

Los objetivos del taller de SparkR son los siguientes:

* Conocer la problemática del procesamiento masivo de datos.
* Fijar concepto y práctica sobre uso R sobre Spark para el procesado masivo de datos.
* Instalar y configurar el sistema completo para poder trabajar con R y Spark.
* Trabajar con datos masivos (filtrado, agregado, transformaciones), procesar datasets masivos son SparkSQL, etc..
* Analizar datasets con las librerías de Machine Learning de los paquetes SparkR y sparklry
* Utilizar herramientas para visualizar los datos de datasets masivos.


## Contenido

En el taller de procesamientos masivo de datos con SparkR se va a ver lo siguiente:

* **Introducción al procesamiento de datos masivos**<BR>
    Breve introducción al procesamiento de datos, el problema de trabajar con grandes conjuntos de datos, Hadoop,Motivación de Spark, características, etc...<BR>

* **Notas sobre R, Spark y SparkR**<BR>
    Introducción a R, motivación de R para datos 'pequeños' y datos 'grandes', Spark y sus características, biblioteca de SparkR para análisis de datos masivos con R.<BR>

* **Instalación de las herramientas necesarias para el taller**<BR>
    Veremos todas las herramientas necesarias para poder trabajar con el entorno de SparkR, así como la instalación y puesta en marcha de toda la infraestructura necesaria para el taller. Inicio del entorno de trabajo habitual para trabajar en el taller.<BR>

* **Entorno de trabajo del taller**<BR>
    Detalles del manejo del entorno de trabajo con JupyterNotebooks y Spark + R<BR>

* **Inicio del entorno de trabajo**<BR>
    Flujo de trabajo con Spark + R<BR>

* **Primeros pasos con SparkR**<BR>
    Trabajo con ejemplos de uso de Spark + R <BR>

* **Lectura y Escritura de datos con SparkR**<BR>
    Trabajo con fuentes de datos, y tipos de conjuntos de datos, CSV, JSON, Parquet, ... Lectura y Escritura. Esquemas, y breve trabajo con SparkSQL. <BR>

* **Operaciones y procesado de SparkDataFrames**<BR>
    Trabajamos y procesamos conjuntos de datos masivos con SparkSQL y funciones de agregación, filtrado, selección, etc. Usamos flujos de trabajo con magrittr. Revisamos la funcionalidad completa de la biblioteca de SparkR.<BR>

* **Minería de datos con la biblioteca de SparkR**<BR>
    Aplicamos las técnicas de minería de datos y Machine Learning que proporciona SparkR: GLM, KMeans, NaiveBayes y AFT.<BR>

* **Minería de datos con la biblioteca sparklyr**<BR>
    Utilizamos la funcionalidad de la biblioteca ``sparklyr`` para procesar conjuntos de datos. Aplicamos los métodos de minería de datos y otras operaciones.<BR>

* **Visualización de datos**<BR>
    Visualización con la herramienta de experimentación reproducible Zeppelin (beta)

# Material del taller

## Descarga de la Máquina Virtual VirtualBox: All-in-one

La máquina virtual para el Taller completo contiene todas las herramientas necesitas para el desarrollo del trabajo, por lo tanto es la opción más sencilla para poder empezar y ponerse manos a la obra con el taller.


Descargar la máquina virtual del taller: https://www.dropbox.com/s/83i4cll8lgo6dzr/SparkR%3A%20Jornadas7.ova. (aprox: 4 GB)


Los datos de acceso a la Máquina Virtual son:

	usuario: root
	clave: sparkR

**Requisitos necesarios para trabajar con la Máquina Virtual:**

* Tener instalado VIRTUALBOX, disponible en: https://www.virtualbox.org/wiki/Downloads
* Disponer de al menos 2GB de RAM para la Máquina Virtual
* El PC debe ser de 64bits y contar con al menos 4GB de RAM (2GB para la MVirtual y otros 2GB para el PC)
* Compatible con Windows, Mac OSX y Linux


## Inicio del entorno de trabajo

Tanto si te descagas la Máquina virtual, como si instalas todo manualmente, es necesario ejecutar los siguientes comandos antes de trabajar en el taller:

Arrancamos el entorno de Spark:

	/usr/local/spark/sbin/start-all.sh

Arrancamos JupyterNotebook:

	jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0

Esto hará que funcione JupyterNotebooks en el puerto 8888, por lo tenemos que acceder desde nuestro navegador web a la dirección: http://localhost:8888

Para cerrar todo el entorno de trabajo, cerramos JupyterNotebook y ejecutamos:
	
	/usr/local/spark/sbin/stop-all.sh

## Instalación de SparkR + extras manualmente para CentOS 7

```
> yum update
> yum install -y epel-release
> yum install -y R
> curl -O http://apache.rediris.es/spark/spark-2.0.1/spark-2.0.1-bin-hadoop2.7.tgz
> tar xvfz spark-2.0.1-bin-hadoop2.7.tgz
> mv spark-2.0.1-bin-hadoop2.7 /usr/local/spark
> curl -O http://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz
> tar xvfz scala-2.11.8.tgz
> mv scala-2.11.8 /usr/local/scala


> echo 'export PATH=$PATH:/usr/local/spark/bin/' >> $HOME/.bash_profile 
> echo 'export PATH=$PATH:/usr/local/scala/bin/' >> $HOME/.bash_profile 
> echo 'export SPARK_HOME=/usr/local/spark/' >> $HOME/.bash_profile 

> source .bash_profile


> curl -O https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
> yum install -y bzip2 openssl-devel curl-devel pandoc
> sh Miniconda2-latest-Linux-x86_64.sh 

#exit 

> conda install jupyter

> R

> install.packages(c('repr', 'IRdisplay', 'crayon', 'pbdZMQ', 'devtools'))
> devtools::install_github('IRkernel/IRkernel')
> IRkernel::installspec()  # to register the kernel in the current R installation

#Execute:

> /usr/local/spark/sbin/start-all.sh
> jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0 

```

