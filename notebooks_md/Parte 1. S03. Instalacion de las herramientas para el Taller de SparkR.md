
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

# Instalación y despliegue de la infraestructura del Taller.

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)


## Para el taller se ha creado una Máquina Virtual  en VirtualBox que contiene

* Spark versión 2.01.

* Scala versión 2.16

* R versión 3.3.1

* Jupyter Notebooks ( y Conda )

* Zeppelin 0.62

Se ha tratado de incluir todos los componentes en las versiones más actualizados posibles.

## Requisitos necesarios para trabajar con la Máquina Virtual:

* Tener instalado VIRTUALBOX, disponible en: https://www.virtualbox.org/wiki/Downloads

* Disponer de al menos 2GB de RAM para la Máquina Virtual

* El PC debe ser de 64bits y contar con al menos 4GB de RAM (2GB para la MVirtual y otros 2GB para el PC)

* Compatible con Windows, Mac OSX y Linux

## Descarga de la Máquina Virtual: All-in-one

La máquina virtual que proveemos para el Taller completo contiene todas las herramientas necesitas para el desarrollo del trabajo, por lo tanto es la opción más sencilla para poder empezar el taller.

* Descargar la máquina virtual del taller: https://drive.google.com/file/d/0ByPBMv-S_GMEakRCVVRTejZKVm8/view?usp=sharing  (aprox: 4GB).
* Una vez descargda la Máquina Virtual, ejecutar el fichero y se importará a VirtualBox.
* Hecho esto, ejecutar la nueva máquina instalada.

Los datos de acceso a la Máquina Virtual son:

* usuario: **``root``**
* clave: **``sparkR``**



## Descarga e instalación manual de las herramientas de taller:

Se recomenda usar la Máquina virtual antes que instalar todo manualmente. 

El tutorial para la instalación de todas las herramientas para el taller es el siguiente:

Actualizamos:

```
yum update
```

Instalamos  R:

```
yum install R
```

Descargamos Spark 2.0.1

```
curl -O http://apache.rediris.es/spark/spark-2.0.1/spark-2.0.1-bin-hadoop2.7.tgz
```

Descomprimimos el fichero de Spark:

```
tar xfvz spark-2.0.1-bin-hadoop2.7.tgz
```

Copiamos Spark a su caperta:

```
mv spark-2.0.1-bin-hadoop2.7 /usr/local/spark
```

Descargamos Scala

```
curl -O http://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz
```

Descomprimimos Scala y lo movemos a su caperta

```
mv scala-2.11.8 /usr/local/scala
```

Establecemos los PATH para scala y spark:

```
echo 'export PATH=$PATH:/usr/local/spark/bin/' >> $HOME/.bash_profile 
echo 'export PATH=$PATH:/usr/local/scala/bin/' >> $HOME/.bash_profile 
echo 'export SPARK_HOME=/usr/local/spark/' >> $HOME/.bash_profile
```

Ejecutamos:

```
source .bash_profile
```

Decargamos CONDA (para JupyterNotebooks)

```
curl -O https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
```

Ejecutamos CONDA y lo instalamos (seguir todos los pasos por defecto):

```
sh Miniconda2-latest-Linux-x86_64.sh 
```

Instalamos Jupyter:

```
conda install jupyter
```

## Inicio del entorno de trabajo

Tanto si te descagas la Máquina virtual, como si instalas todo manualmente, es necesario ejecutar los siguientes comandos antes de trabajar en el taller:

**Arrancamos el entorno de Spark:**

```
/usr/local/spark/sbin/start-all.sh
```

**Arrancamos JupyterNotebook:**

```
jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0
```

Esto hará que funcione JupyterNotebooks en el puerto 8888, por lo tenemos que acceder desde nuestro navegador web a la dirección: http://localhost:8888


Para cerrar todo el entorno de trabajo, cerramos JupyterNotebook y ejecutamos:

```
/usr/local/spark/sbin/stop-all.sh 
```



## Descarga del material del taller

Toda la información del Taller está disponible en GITHUB

Accedemos la Máquina Virtual, escribimos:

```
cd /root/TallerSparkR/
```

Clonamos el repositorio o bien descargamos el zip donde está toda la información:

**Con GIT:**
```
    git clone https://github.com/manuparra/taller_SparkR NOTEBOOKS
```

**Descarga el paquete completo de guiones y descomprime:**
```
 curl -O https://github.com/manuparra/taller_SparkR/archive/master.zip
 
 unzip master.zip
```




```R

```
