# Taller de SparkR.

## VIII Jornadas Usuarios de R, Albacete (España) el 17-18 de noviembre de 2016


### Descarga de la Máquina Virtual VirtualBox: All-in-one

La máquina virtual para el Taller completo contiene todas las herramientas necesitas para el desarrollo del trabajo, por lo tanto es la opción más sencilla para poder empezar y ponerse manos a la obra con el taller.


Descargar la máquina virtual del taller: https://www.dropbox.com/s/83i4cll8lgo6dzr/SparkR%3A%20Jornadas7.ova. (aprox: 2GB)


Los datos de acceso a la Máquina Virtual son:

	usuario: root
	clave: sparkR

**Requisitos necesarios para trabajar con la Máquina Virtual:**

* Tener instalado VIRTUALBOX, disponible en: https://www.virtualbox.org/wiki/Downloads
* Disponer de al menos 2GB de RAM para la Máquina Virtual
* El PC debe ser de 64bits y contar con al menos 4GB de RAM (2GB para la MVirtual y otros 2GB para el PC)
* Compatible con Windows, Mac OSX y Linux


### Inicio del entorno de trabajo

Tanto si te descagas la Máquina virtual, como si instalas todo manualmente, es necesario ejecutar los siguientes comandos antes de trabajar en el taller:

Arrancamos el entorno de Spark:
> /usr/local/spark/sbin/start-all.sh
Arrancamos JupyterNotebook:
> jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0
Esto hará que funcione JupyterNotebooks en el puerto 8888, por lo tenemos que acceder desde nuestro navegador web a la dirección: http://localhost:8888
Para cerrar todo el entorno de trabajo, cerramos JupyterNotebook y ejecutamos:
> /usr/local/spark/sbin/stop-all.sh

### Instalación de SparkR + extras manualmente para CentOS 7

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

