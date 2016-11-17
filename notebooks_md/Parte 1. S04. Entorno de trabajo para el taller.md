
<span style='font-size:2.4em'>Procesamiento masivo de datos con SparkR</span>

<span style='font-size:1.5em'>VIII Jornadas de usuarios de R. Albacete, Castilla-La Mancha, 17 y 18 de noviembre de 2016</span>

Taller impartido por: <span style='font-size:1.2em'>Manuel Jesús Parra Royón</span>


![Alt](https://sites.google.com/site/manuparra/home/logoparty.png)

<HR>

## Entorno de trabajo para el taller 

![Spark+R](https://sites.google.com/site/manuparra/home/SparkRlogo.png)


Lo primero a realizar es ejecutar sobre el terminal de la Máquina Virtual o de vuestro sistema:

**Arrancamos el entorno de Spark:**

```
/usr/local/spark/sbin/start-all.sh
```

**Arrancamos JupyterNotebook:**

```
jupyter notebook --notebook-dir=/root/TallerSparkR --no-browser --port=8888 --ip=0.0.0.0
```

Esto hará que funcione JupyterNotebooks en el puerto 8888, por lo tenemos que acceder desde nuestro navegador web a la dirección: http://localhost:25980    

¿Por qué es diferente el puerto donde se lanza Jupyter y la url? *En VirtualBox se ha redireccionado el puerto del 25980 al 8888.*

En entorno que se ha elegido para el  desarrollo de las sesiones de trabajo en el taller de Spark + R es **Jupyter Notebook** en lugar de la herramienta **RStudio**.

![AltJupyter](http://blog.jupyter.org/content/images/2015/02/jupyter-sq-text.png)

Algunas de sus caracteriticas principales:

1. Jupyter Notebook es **una aplicación Web en la nube** que nos permite crear y compartir documentos que contienen código vivo, ecuaciones, textos, visualizaciones, etc. 

2. De esta forma podemos crear **tutoriales interactivos**, donde el lector pueda ir probando y comprobando en tiempo real aquellos conceptos o ejercicios que le vamos mostrando.

3. Se ejecuta **"en la nube"** por lo que no hay que instalar nada.


El objeto más importante en jupyter es el **notebook**. 

Cada notebook está formado por un conjunto de **celdas** de contenido, donde el tipo de las mismas varía desde código hasta headings, pasando por markdown (para la inclusión de texto narrativo):

* Codigo fuente (de cualquier lenguaje, con excepciones, siempre que sean interpretados)
* Texto en formato MARKDOWN


Además todas las celdas de un documento de Jupyter pueden ser ejecutadas y, en base a su tipo, obtendremos un resultado u otro. 

Las celdas markdown y headings nos van a servir para añadir texto y formato a nuestro notebook. Es decir estas celdas permitirán diseñar nuestro documento y hacerlo perfecto para presentarlo.





### Revisamos la interfaz de usuario de **Jupyter Notebooks**

#### 1.- Manejo de celdas y tipo de celdas

#### 2.- Opciones de uso rápido y menú de operaciones

#### 3.- Gestión de las las celdas y formato

#### 3.- Publicación de los notebooks

## Primer ejemplo en R para Jupyter Notebooks

Utilizamos la zona de pruebas para trabajar con los Notebooks en Jupyter, R y SparkR.

<HR>
# Zona de pruebas del NOTEBOOK en SparkR
![FooterSparkR](https://sites.google.com/site/manuparra/home/footer_SparkR_v2.png)


Escribe todas las pruebas en R que necesites a partir de aquí

<HR>


```R
# Vamos manos a la obra !
# Escribe tu codigo ! No te asustes !
```
