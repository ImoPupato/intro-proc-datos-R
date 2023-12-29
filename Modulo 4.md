## Módulo 4: Gráficos representativos.
Introducción al manejo de gráficos.  

### 1. Descripción gráfica de una muestra cuya variable *cualitativa*  
_Recordemos que una variable es cualquier característica que toma diferentes valores en las unidades elementales. Cuando hablamos de variable cualitativa o categórica, nos referimos a aquellas que no medibles numéricamente. Pueden ser nominales u ordinales (categorías jerárquicas)._  
Ejemplo de variable cualitativa nominal puede ser: tipos de fertilizante usado en suelos.  
Ejemplo de variable cualitativa ordinal puede ser: cantidad de fallas por producto.  
* **Gráfico de sectores**: Consiste en un círculo que representa el 100% de las observaviones. Se divide en cuantas categorías haya para la variable.  
* **Gráfico de barras**: Consiste en barras horizontales, una para cada catergoría. La longitud de cada barra es proporcional al porcentaje (frecuencia relativa porcentual) de unidades de dicha categoría.   
* **Gráfico de Pareto**: El diagrama de Pareto consiste en barras verticales, una por categoría. La altura de cada barra es proporcional a la cantidad (frecuencia absulta) de unidades. Además tiene una línea sobre el diagrama que conecta los porcentajes acumulados a través de las distintas categorías. Por esto, el diagrama de Pareto cuenta con un doble eje vertical (Frecuencia absoluta y Porcentaje), y uno horizontal (categorías).     

### 2. Descripción gráfica de una muestra cuya variable *cuantitativa*   
_Recordemos que una variable cuantitativa es aquella que toma cualquier valor dentro de un intervalo (continua) o valores aislados dentro de un intervalo (discreta)._ 
Ejemplos de variable cuantitativa continua sería el pH, Temperatura.   
Ejemplos de variable cuantitativa discreta sería la edad, Conductividad.   
* **Diagrama de puntos**: Suele hacerse siempre que tengamos variables cuantitativas a modo eploratorio, para conocer cómo están distribuidos los datos. En el eje *x* se indican los valores observados para la variable y, en el eje *y* la frecuencia absoluta o número de veces que se presentó cada valor observado.   
* **Diagrama de tallo y hoja**: Cada observación se separa en un tallo y una hoja; y se ordenan de manera creciente, asignando un lugar a cada valor observado.  
* **Diagrama o gráfico de bastones**: Suele utilizarse cuando el número de observaciones es grande y con valores repetidos. En el eje *x* se ubica el valor observado de la variable y en el eje *y* su frecuencia absoluta  
* **Histograma de frecuencias**: Suele utilizarse cuando la variable toma muchos valores diferentes. Para ello se construyen intervalos para los valores observados y se asigna la frecuencia absoluta a cada barra de intervalo. En el eje *x* se indican los intervalores y en el eje *y* se indica la frecuencia absoluta.  
* **Boxplot**: suelen llamarlo diagrama de caja y bigote. Su representación gráfica resume características de posición, dispersión, asimetría, entre otras, e identificar la presencia de valores atípicos. La caja de un boxplot comienza en $Q_1$ y termina en $Q_3$, representando el 50% de los datos centrales, con una línea en su interior que representa la mediana. A cada lado de la caja se dibuja un segmento con los datos más lejanos sin contar los valores atípicos (outliers) del box plot, que en caso de existir, se representarán con círculos.  

### 3. Plots base de R:  
Asignación del directorio de trabajo y carga del set de datos. Utilizaremos nuevamente la base de datos del módulo anterior y generaremos dos nuevas columnas auxiliares que nos servirán para los gráficos   
```{r wd}
setwd("G:/Mi unidad/Curso R - IPS/Clase 4") # chequear previamente la dirección
WQT <- read.csv("G:/Mi unidad/Curso R - IPS/Clase 4/Datos_WQT.csv") 
colnames(WQT)<-c("ID","pH", "Temp", "Turbidez", "DO", "Conductividad") #reasignación de nombres a las columnas
WQT$Conductividad_2=ifelse(WQT$Conductividad<350, "Baja", "Alta") # así tenemos una categórica
WQT$Temp_2=round(WQT$Temp,digits = 0) # así redondeamos a valores sin coma
WQT$pH_2=ifelse(WQT$pH<7,"Ácido",ifelse(WQT$pH>7.1, "Básico", "Neutro"))
colores<-c("lightgreen","violet")
```  
**Variables cualitativas**   
* Diagrama de sectores   
```{r pie}
pie(table(WQT$pH_2)) # en pie debemos introducir los datos desde una tabla
pie(table(WQT$pH_2), col=c("lightgreen","lightpink","lightblue")) #cambiamos los colores
porcentaje= round(100*(table(WQT$pH_2))/sum(table(WQT$pH_2)), 1) # generamos un vector auxiliar con los porcentajes de cada categoría
pie(table(WQT$pH_2), labels = paste(porcentaje,"%"), col=c("lightgreen","lightpink","lightblue"), main = "Diagrama de sectores: Categorías de pH") #agregamos porcentajes y el título
legend("topleft", legend = c("Ácido", "Básico", "Neutro"),
       fill =  c("lightgreen","lightpink","lightblue")) # agregamos una leyenda para relacionar las categorías con colores
```  
  
* Diagrama de barras  
```{r barplot}
barplot(table(WQT$pH_2), horiz=TRUE) # en barplot también debemos introducir los datos desde una tabla, el argunto horiz=TRUE es para que las barras sean horizontales
barplot(table(WQT$pH_2), xlab="Cantidad de muestras", ylab="Categoría de pH", xlim=c(0,500), col="violet", main = "Diagrama de bastones: Categorías de pH",horiz=TRUE) 
```
  
* Construcción de un diagrama de Pareto  
```{r Pareto}
library(qcc)
pareto.chart(table(WQT$pH_2), ylab = "Frecuencia", ylab2= "Porcentaje",xlab = "Categoría de pH", las=1,main="Diagrama de Pareto")
```  
  
Como pueden observar, esta función genera y muestra la tabla de frecuencias y porcentajes.   
   
**Variables cuantitativas**  
* Diagrama de puntos  
```{r plot}
plot(WQT$pH)
```  
```{r plot2}
plot(WQT$pH, xlab="Observaciones", ylab="pH") # modificamos los nombres de los ejes
plot(WQT$pH, xlab="Observaciones", ylab="pH", pch=16) # modificamos el tipo de punto
plot(WQT$pH, xlab="Observaciones", ylab="pH", pch=16, col="darkblue") # cambiamos el color de los puntos
plot(WQT$pH, xlab="Observaciones", ylab="pH", pch=16, col="darkblue", main= "Diagrama de puntos: pH en muestras de agua") # agregamos el título
plot(WQT$pH, ylab="Observaciones", xlab="pH", 
pch=16, col=colores[factor(WQT$Conductividad_2,levels = c("Baja","Alta"))], 
main= "Diagrama de puntos: pH en muestras de agua") # agrego diferenciación de los puntos, por colores, de acuerdo a la categoría conductividad
```  
  
* Diagrama de tallo y hoja  
```{r stem}
stem(WQT$pH) # la salida está en la consola
stem(WQT$pH,2)
```  
  
* Diagrama de bastones  
```{r barplot, default}
barplot(table(WQT$pH)) # en barplot también debemos introducir los datos desde una tabla, el argunto horiz=FALSE es por default por lo que no es necesario agregarlo 
barplot(table(WQT$pH), ylab="Cantidad de muestras", xlab="pH", ylim=c(0,30), col="lightyellow", main = "Diagrama de bastones")
```  
  
* Histograma de frecuencias  
```{r histograma}
hist(WQT$pH)
hist(WQT$pH, xlab = "pH", ylab="Frecuencia", ylim = c(0,100), main="Histograma de frecuencias", col= "lightgreen")
```  
  
* Diagrama de caja   
```{r boxplot}
boxplot(WQT$pH)
boxplot(WQT$pH, ylim=c(6.6,7.6)) # modificación de límites del eje
boxplot(WQT$pH, ylim=c(6.6,7.6), ylab="pH") # agregamos título al eje
boxplot(WQT$pH, ylim=c(6.6,7.6), ylab="pH", main="Boxplot: pH en muestras de agua") # agregamos título
boxplot(WQT$pH, ylim=c(6.6,7.6), ylab="pH", main="Boxplot: pH en muestras de agua", col="lightblue") # modificamos el color de fondo del boxplot
boxplot(WQT$pH~WQT$Conductividad_2, ylim=c(6.6,7.6), ylab="pH", main="Boxplot: pH en muestras de agua", col=colores) # con el operador ~ indicamos que nos interesa ver graficados los valores de pH separados por la categoría conductividad
```  
  
## 4. ggplot2  
Como vimos anteriormente, la librería ggplot2 forma parte del conjunto de librerías de tidyverse. Particularmente, ggplot2, es un sistema organizado de visualización de datos que funciona en capas. Consta de 7 capas: Data, Aesthetics, Geometries, Facets, Statistics, Coordinates, Theme.   
Para construir un gráfico utilizazndo esta librería utilizaremos la sintaxis: ggplot(data, aes() ) + geom_tipo().  
Los argumentos son:  
- _data_: un data frame con los datos a visualizar.  
- _aes_: _aesthetics_; características referidas a aspectos gráficos como color de los ejes, nombres de los ejes, etc.   
- _geoms_: es un argumento que indica la representación gráfica elegida. Entre varias opciones se encuentran:   
  - geom_point (puntos)  
  - geom_lines (lineas)  
  - geom_histogram (histograma)  
  - geom_boxplot (boxplot)  
  - geom_bar (barras)  
  - geom_polygons (polígonos en un mapa)
  
Te invito a utilizar el comando help.search("geom_", package = "ggplot2") para tener más información. Tambien podes realizar la búsqueda en la lupa en la subpantalla derecha inferior.  
- facets: _facetics_ o agrupaciones. Nos permite agrupar los resultados de acuerdo a categorías.   
- statistics: se puede pedir que agregue al gráfico medidas estadísticas.
- coordinates: nos permite modificar la escala a otra base. Puede ser log10, o modificar los límites de los ejes que quiero mostrar.   
- theme: tema o fondo del gráfico.   
- colores: se pueden cargar paquetes que utilizan gama de colores amigables, continuos, o con temas de películas.   

Todos estos elementos y argumentos van siendo añadidos en distintas las capas, lo que facilita el orden y la comprensión de cada comando. Cada capa se adiciona utilizando un +.   

```{r ggplot2}
library(tidyverse) # también podemos llamar a ggplot2 si no queremos llamar a todo el conjunto de librerias tidyverse.  
```
  
- Empecemos con uno básico  
```{r ggplot22}
ggplot(WQT) +
  geom_point(aes(x= pH, y=Temp))#así graficamos pH vs Temp
```  
  
- Podemos agregar categorías:  
```{r ggplot23}
ggplot(WQT) +
  geom_point(aes(x= pH, 
y=Temp,
col=Conductividad_2))# con el comando col asignamos e criterio para los colores  
```  
  
- Generamos un nuevo plot con dos variables cuantitativas y con la capa facet, lo separamos por categoría de pH:  
```{r ggplot24}
ggplot(WQT,aes(y=Conductividad,x=DO))+
  geom_line()+
  facet_wrap(~pH_2)  
```  

- Generamos un boxplot:  
```{r ggplot25}
ggplot(WQT,aes(y=pH,x=Conductividad_2))+
  geom_boxplot()+
  labs(x="Conductividad",y="pH")  
```  
  
- Generamos un histograma:  
```{r ggplot26}
ggplot(WQT)+
   geom_histogram(aes(x=pH))+
   labs(x="pH",y="Frecuencias")
```  

- Modificamos el histograma y agregamos unas capas más:  
```{r ggplot27}
ggplot(WQT)+
  geom_histogram(aes(x=pH, y=..density..),
                 bins=10,
                 fill="orange",
                 col="black")+
  labs(x="pH",y="Frecuencias", title= "Histograma de Frecuencias")+
facet_wrap(~Conductividad_2)+
theme_minimal()
```  

- Generamos un diagrama de barras:  
```{r ggplot28}
ggplot(WQT)+
geom_bar(aes(y=pH_2))
```  
  
## 5. Colores  
La elección de los colores para los gráficos no es una decisión que deba tomarse a la ligera. R base tiene su propia paleta y podemos consultar los valores en https://r-charts.com/es/colores/.  
Existen paquetes como  ColorBrewer o wesanderson que contienen otras paletas. La librería wesanderson contiene una paleta por película y se puede acceder instalando el paquete, llamando la libreria y escribiendo el names(wes_palettes).  
Las paletas que recomiendo utilizar son aquellas llamadas paletas discretas _colorBlindness_, también se pueden ver en https://r-charts.com/es/colores/.
