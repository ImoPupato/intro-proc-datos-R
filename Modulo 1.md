# Módulo 1: Introducción  R.  
Instalación de R y RStudio, uso de librerías o paquetes, introducción de datos, estructuras y tipos de datos (vectores, data.frame, matrices, listas) y elementos (numéricos, strings).  
paquete: readxl  

## 1. Conociendo a R  
R es un lenguaje y un entorno diseñado para la implementación de técnicas y análisis estadísticos. Se trata de un lenguaje abierto y se encuentra disponible de manera gratuita, GPL ( _general public license_). R fue creado en 1993 por Ross Ihaka y Robert Gentleman en el Departamento de Estadística de la Universidad de Auckland, Nueva Zelanda. A partir de 1997 se conformó el equipo llamado "R Core Team" quienes tienen acceso a las modificaciones del código fuente. Para conocer más sobre el proyecto pueden ingresar a: <https://cran.r-project.org/>.  
CRAN: The Comprehensive R Archive Network.  
Todas las variables, funciones, datos, salidas, operaciones (lógicas, comparativas, etc.), etc. son guardadas como objetos, por lo que R es un lenguaje _orientado a objetos_.   

### Paquetes en R  
Llamamos paquete ( _package_) a un subdirectorio compuesto de códigos y documentación. Cada paquete tiene diferentes funciones que permiten desde leer y manipular datos; hasta graficarlos y realizar funciones.  
Toda la comunidad de R puede diseñar y compartir sus paquetes a través de repositorios.  
  
## 2. RStudio
RStudio (ahora Posit) es un software de código abierto fundado en 2009, es la interfase que utilizaremos para trabajar con el lenguaje R y tener una mejor visualización. Para más información ingresar a: <https://posit.co/about/>.  
AGREGAR SOBRE EL ENTORNO

## 3. Instalación de R y RStudio  
En el video 1 de esta primera clase se muestra como instalar R y RStudio para Windows. Para quienes usen otro sistema operativo a continuación está la guía provista por R para su instalación.  

### Instalación R  
- Para Unix: <https://cran.r-project.org/doc/FAQ/R-FAQ.html#How-can-R-be-installed-_0028Unix_002dlike_0029>  
- Para Windows: <https://cran.r-project.org/doc/FAQ/R-FAQ.html#How-can-R-be-installed-_0028Mac_0029>  
- Para Mac: <https://cran.r-project.org/doc/FAQ/R-FAQ.html#How-can-R-be-installed-_0028Windows_0029>  
  
## 4. Tipos de objetos en R
Como se mencionó anteriormente R es un lenguaje orientado a objetos. Bajo la categoría objeto podemos guardar cadenas de caracteres (character), lógicos o booleanos (logical), números reales (numeric), números enteros (integer), números complejos (complex). Estos objetos se estructuran en clases que que pueden ser vectores, matrices, factores, listas o data frames. Éstos últimos 2 se caracterizan por tener elementos de distintas clases.  

Antes de comenzar el trabajo en RStudio, debemos seleccionar el directorio de trabajo en el cual se consultarán y guardarán los documentos generados. Esto puede realizarse por consola o seleccionándolo en el apartado de archivos (Files).  
```{r wd}

setwd("~/Curso R /Modulo 1") # chequear previamente la dirección
getwd() # de esta manera verificamos el directorio en el que nos encontramos trabajando
```  
### Vectores  
Los vectores constituyen una estructura simple y son una colección ordenada de elementos. A continuación construiremos distintos vectores:  
```{r vectores}
vector_numerico_1<-c(1,2,3,4,5)
vector_numerico_2<-c(1:5) # indico el valor de inicio y valor de finalización
vector_numerico_3<-seq(1,5,1) # indico el valor de inicio, de finalización y el valor de incremento 
vector_numerico_4<-runif(5,1,5) # indico la cantidad de números, el menor y el mayor
vector_caracter_1<-c("a","b","c","d","e") # el encomillado indica que son caracteres
vector_caracter_2<-c(rep("a",3),rep("b",2)) # indico primero el valor que quiero que se repita y luego separado con coma el número de veces
vector_logico<-c(T,T,F,F,T) # al utilizar la T (True) y la F (False) que son letras sin encomillado, lo toma como vector lógico
```
A través de las líneas anteriores conocimos las funciones c(), seq (), rep(), runif() y el operador ":". Estas funciones se encuentran en lo que llamamos R base y no necesita de paquetes auxiliares.  
### Factores  
Los factores constituyen un tipo especial de vectores que se utilizan para datos categóricos.  
```{r factores}
factor_1<-factor(vector_caracter_2,c("a","b"),c("Amarillo","Blanco"))# convertimos el vector_caracter_2 en un objeto factor_1 de dos niveles
```  
Aquí también utilizamos una función, factor () para generar un nuevo objeto a partir de otro ya creado.  
La función factor necesita 3 datos: el vector a convertir, los niveles del factor y las etiquetas de dichos niveles:  
factor( vector_a_convertir_en_un_factor, niveles del factor, etiquetas de los niveles)  
### Matrices  
Las matrices son arreglos de números en dos dimesiones y se pueden generar con la función matrix (). La función solicita 3 datos: matrix(valores, nº de filas, nº de columnas).  
```{r matrices}
matriz_1<-matrix(1:10,4,5,byrow = TRUE) # aquí le indicamos que coloque los números del 1 al 10 ordenados en 4 filas y 5 columnas, comenzando a ubicarlos por fila. Por default el orden lo hace por columna
matriz_2<-matrix(1:10,4,5) #comparemos ambas matrices
```  
### Listas  
Las listas son una colección ordenada de objetos y pueden crearse con la función list(). Sus argumentos son list(nombre_del_objeto_1=objeto_1, …, nombre_del_objeto_i=objeto_i).  
```{r listas}
lista_1<-list(Dias=c("Lunes", "Martes", "Miercoles", "Jueves", "Viernes"), Cantidad=c(6,5,3,5,9)) # en esta lista hay dos componentes; uno llamado Dias y el otro Cantidad
lista_1$Dias # de esta manera inspecciono los datos guardados en el componente Dias del objeto lista_1
```
### Data frames
Los data frames o, en español, hoja de datos; tienen una estructura similar a la de una matriz pero puede contener clases de datos heterogéneos. Podemos generar un data frame con la función data.frame(), cuyos argumentos son vectores que constituirán las columnas.  
```{r data frames}
data_frame_1<-data.frame(lista_1$Dias,factor_1,lista_1$Cantidad,vector_logico)
```  
## 5. Reasignar y modificar elementos  
Con el siguiente ejemplo trabajaremos sobre el data frame anterior, estamos considerando qué dias está libre el aula según el color indicado.  
```{r data frames modificado}
colnames(data_frame_1)<-c("Dias","Color Aula","Bancos","Libre") # de esta manera asignamos el nombre a cada columna  
rownames(data_frame_1)<-vector_caracter_1 # de esta manera asignamos el nombre a cada fila  
matriz_1[1,1]<-11 #la indexación es un sistema que permite acceder o modificar elementos de un objeto. M[i,j] es el valor de la i-ésima fila y j-ésima columna de la matriz M.  
```
## 6. Ingresar datos utilizando utilizando las funciones read  
Muchas veces nos interesa ingrear los datos desde archivos ya generados. 
- función read.delim: nos permite copiar la información que tenemos en el portapapeles (clipboard):  
```{r data frames clipboard}
tabla_1<-read.delim("clipboard") # este comando funciona para Windows y Unix, para Mac utilizar tabla1<-read.delim(pipe("pbpaste"))
tabla_2<-read.delim("clipboard", sep="\t", row.names=TRUE, col.names=TRUE) #añadiendo estos argumentos puedo mejorar la visualización de la tabla
```
- función read.csv: nos permite ingresar la tabla en formato de csv o txt desde el entorno de Rstudio o por la consola:  
```{r data frames read.csv}
tabla_3<-read.csv("~/Curso R /Modulo 1/Tabla.txt", sep="") # chequear antes la ruta de acceso al documento
```
- función read_excel: nos permite ingresar la tabla en formato xlsx desde el entorno de Rstudio o por la consola:  
```{r data frames read.xlsx}
install.packages("readxl") # hay versiones que requieren previa instalación
library("readxl") # si lo hacemos desde la consola primero debemos "llamar" a la librería. Si lo cargamos desde el entorno, este paso no es necesario 
tabla_4<-read_excel("~/Curso R /Modulo 1/Tabla.xlsx", sep="") # chequear antes la ruta de acceso al documento
```  
## 7. Guardar datos en una tabla utilizando la función write.table() 
```{r write}
write.table(data_frame_1, "Tabla propia.txt", row.names= TRUE, col.names = TRUE) # guardamos el data frame 1 antes creado
write.table(data_frame_1, "Tabla propia 2.txt", col.names = FALSE) # guardamos el data frame 1 antes creado pero sin los nombres de columna y filas
```
## 8. Operadores  
Existen distintos tipos de operadores en R, agrupados según si son:  
- Aritméticos: adición (+), sustracción (-), multiplicación (*), división (/) y potencia (^).  
- Comparativos: menor (<), mayor (>), menor o igual (<=), mayor o igual (>=), igual (==) y distinto (!=).  
- Lógicos: dados "a" e "a" tenemos los operadores y (a&b) u o (a|b).  
A continuación utilizaremos operadores aritméticos
```{r operadores aritméticos}
1+2
1*3
1-4
4/2
2^3  
```
Ahora vamos a generar una columna nueva que modifique los valores de la columna llamada Bancos y duplique los valores: 
```{r modif 1 tabla propia}
tabla_1$'Bancos modif'<-tabla1$Bancos*2 # la primera parte de la sentencia genera una columna nueva llamada "Bancos modif" en la cual van a estar los valores correspondiente a la operación Bancos * 2
```
Así mismo podemos realizar distintas operaciones sobre las columnas, es decir modificandolas:
```{r modif 2 tabla propia}
tabla_1[,5]<-tabla1$Bancos*2.5 # la primera parte de la sentencia indica, mediante indexación, que en todas las filas de la columna 5 coloque los valores correspondiente a la operación Bancos * 2.5
```
