# Módulo 2: Manipulación de datos.  
Examinar y modificar propiedades de datos, separación y unión de tablas.  
paquete: tidyverse  

## 1. Paquete tidyverse  
Tydiverse es un conjunto de paquetes que permiten mejorar el acceso, manipulación y visualización de datos. Para más información pueden ingresar a <https://www.tidyverse.org/packages/>  
Los paquetes que incluye son:  
- ggplot2, para visualización de datos.  
[Cheatsheet ggplot2] <https://github.com/rstudio/cheatsheets/blob/main/data-visualization.pdf>  
- dplyr, para la manipulación de datos.  
[Cheatsheet dplyr] <https://github.com/rstudio/cheatsheets/blob/main/data-transformation.pdf>  
- tidyr, para ordenar datos.  
[Cheatsheet tidyr] <https://github.com/rstudio/cheatsheets/blob/main/tidyr.pdf>  
- readr, para la importación de datos.  
[Cheatsheet readr] <https://readr.tidyverse.org/>  
- purrr, para la programación funcional.  
[Cheatsheet purr] <https://github.com/rstudio/cheatsheets/blob/main/purrr.pdf>  
- tibble, para marcos de datos.  
- stringr, para cadenas.  
[Cheatsheet stringr] <https://github.com/tidyverse/stringr>  
- forcats, para factores.  
[Cheatsheet forcats] <https://github.com/tidyverse/forcats>  
- lubridate, para fecha/hora.  
[Cheatsheet lubridate] <https://github.com/tidyverse/lubridate>   
Se pueden instalar por separado o en conjunto a través de tydiverse.  

A continuación instalaremos y exploraremos los posibles usos de esta librería.  
```{r installing tidyverse, eval=FALSE}
install.packages("tidyverse") # con la linea install.packages instalamos cualquier paquete. En algunas ocasiones hay que indicar desde qué repositorio se instala el paquete (CRAN by default)
library("tidyverse") # cargamos la librería
setwd("G:/Mi unidad/Curso R - IPS/Clase 2") # chequear previamente la dirección
```
Para utilizar este paquete vamos a trabajar con una tabla de datos reales. Primero realizaremos la carga, luego algunas modificaciones y finalmente utilizaremos tydiverse para manipularla.   

## 2. Carga e inspección de tabla  
Cargar la tabla "Water Quality Testing".  
La tabla obtenida de <https://www.kaggle.com/datasets/shreyanshverma27/water-quality-testing> contiene información sobre distintas muestras de agua a las que se les midieron seis parámetros fisicoquímicos como pH, indica la acidez del agua (cuanto más bajo su valor, más ácida la muestra), temperatura, turbidez, oxígeno disuelto y contuctividad. Estos parámetros suelen medirse para establecer la calidad del agua.    
```{r WQT, include=TRUE}
WQT <- read.csv("G:/Mi unidad/Curso R - IPS/Clase 2/Water Quality Testing.csv") 
summary(WQT) # nos devuelve un resumen de la tabla
head(WQT) # nos devuelve las primeras filas de las columnas a modo exploratorio
colnames(WQT)<-c("ID","pH", "Temp.(ºC)", "Turbidez (NTU)", "OD (mg/L)", "Conductividad (µS/cm)") # vamos a cambiar los nombres de las columnas
```
## 3. Modificar parámetros  
Para nuestro hipotético análisis puede resultar interesante saber recodificar algunas variables, como por ejemplo la conductividad. 
```{r modif 1 WQT, include=TRUE}
WQT$Conductividad<-ifelse(WQT$`Conductividad (µS/cm)`<=344,"Baja","Alta") #ifelse es una función que nos permite asignar un valor de acuerdo a uno observado en otra columna. Aquí estamos generando la nueva columna llamada "Conductividad" que tendrá la etiqueta "Baja" si el valor de la Condutividad medida es menor o igual a 344, caso contrario pondrá "Alta"
```
Supongamos que se trata de mediciones repetidas de las mismas 100 muestras pero realizadas en 5 instancias diferentes. Vamos a generar una tabla auxiliar donde estará dicha información. 
```{r aux WQT, include=TRUE}
info_muestras<-data.frame(
  "ID"=seq(1,500,1),
  "Muestra"=c(rep(seq(1,100,1),5)),
  "Día"=c(rep("Lunes",100),
                     rep("Martes",100),
                     rep("Miércoles",100),
                     rep("Jueves",100),
                     rep("viernes",100)))
```
Antes de pasar a tydiverse, R tiene ciertos comandos base que nos permiten manipular tablas. Por ejemplo; si queremos unir ambas tablas para generar una nueva con toda la información podemos utilizar la función 'merge':
```{r merge, include=TRUE}
Datos<-merge(info_muestras,WQT,by="ID") # merge (primera tabla, segunda tabla, criterio de unión)
```
## 4. Tydiverse  
```{r library, include=TRUE}
library(tidyverse)
```  
1. Unir tablas
Si queremos unir tablas, podríamos hacerlo uniendo por columnas (variables) o por filas (casos). Para ello usamos las funciones 'bind_cols' y 'bind rows'
```{r bind, include=TRUE}
library(tidyverse)
Datos_2<-bind_cols(info_muestras,WQT) # devuelve una nueva tabla con las columnas de la primera tabla seguidas de la segunda
Datos_3<-Datos[1:200,] # aquí estamos generando una tabla auxiliar para unirla con bind_row luego. 
Datos_4<-Datos[201:500,] # aquí estamos generando una tabla auxiliar para unirla con bind_row luego. 
Datos_5<-bind_rows(Datos_3,Datos_4)
```  
También puede ser que queramos unir tablas pero haciendo distintos tipos de match, utilizando las funciones de 'mutating_join':
```{r mutating join, include=TRUE}
Datos_2[,7:10]<-NULL # de este modo borramos de la tabla Datos_2 las columnes 7,8,9 y 10, generamos una tabla auxiliar para hacer los diferentes joins
Datos_6<-left_join(Datos_2,Datos_3) # genera una tabla con los valores coincidentes de la segunda tabla que se encuentren en la primeraa
Datos_7<-right_join(Datos_2,Datos_3) # genera una tabla con los valores coincidentes de la primera tabla que se encuentren en la segunda
Datos_8<-inner_join(Datos_2,Datos_3) # genera una tabla con los valores coincidentes entre ambas tablas
Datos_9<-anti_join(Datos_2,Datos_3) #devuelve una tabla con los valores que no son coincidentes entre ambas tablas
```
2. Unir  y separar celdas
Si queremos generar un código interno que incluya el número de muestra y el día del muestreo, utilizamos la función 'merge':
```{r unite and separate, include=TRUE}
Datos_10<-unite(Datos,sep="- ",Muestra,Día,col="ID_interno",remove=FALSE) # unite(tabla, separador, primera columna a unir, segunda columna a unir, columna destino, sin remover celdas originales)
Datos_11<-unite(Datos,sep="-", 'Conductividad (µS/cm)',Conductividad,col="Prueba",remove=TRUE) # unite(tabla, separador, primera columna a unir, segunda columna a unir, columna destino, remover celdas originales)
Datos_12<-separate(Datos_11,Prueba, sep="-", into=c('Conductividad (µS/cm)',"Conductividad"),remove=TRUE) # separate(tabla, columna a separar, separador, primera columna, segunda columna, remover celda original)
```
3. Reformatear tabla   
Si queremos separar o juntar columnas de acuerdo a la información que contienen, podemos utilizar la función 'pivot_':
```{r pivot_longer and pivot_wider, include=TRUE}
Datos_12[,1]<-NULL
Datos_12<-pivot_wider(Datos_12,names_from = Conductividad, values_from = Día) # pivot_wider(tabla, columna a separar, columna con valores a separar)
Datos_12<-pivot_longer(Datos_12,cols = 7:8,names_to="Conductividad",values_to="Día", values_drop_na = TRUE) # pivot_longer(tabla, columnas a unir, columna con etiquetas, columna con valores, no incluir na's)
```
4. Recortar tabla según criterios
Si queremos _seleccionar_ cierto contenido en función de ciertos valores de corte, es decir manipular casos, variables, combinar o recortar tablas; utilizamos las funciones del paquete dplyr, incluido en tydiverse:
```{r filter, select, mutate, bind, join, include=TRUE}
Datos_13<-filter(Datos, pH>7) # generamos una nueva tabla que contine aquellas muestras con pH mayor a 7, utilizando un operador comparativo
Datos_14<-filter(Datos,pH>7 & Conductividad=="Alta") #generamos una nueva tabla que contiene aquellas muestras con pH mayor a 7 y Conductividad alta. Aqui estamos utilizando operadores comparativos y lógicos
Datos_15<-select(Datos, Muestra, "OD (mg/L)",)#generamos una nueva tabla que contiene aquellas columnas que querramos seleccionar. 
Datos_16<-subset(Datos, Conductividad!="Baja",select=c("Muestra", "OD (mg/L)")) # aqui combinamos dos funciones, subset (R base) y select (dplyr) para generar una nueva tabla con las columnas seleccionadas y que cumplan cierta operación lógica.
Datos_17<-mutate(Datos_6, pH_modif=Datos_6[,5]+2)
```

