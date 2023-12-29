# Módulo 3: Estadística descriptiva.  
Obtención de estadísticas básicas de posición (media, mediana, moda, cuartilos), de dispersión (variancia, desvío estándar, rango, coeficiente de variación), de distribución (simetría, curtosis y normalidad).  

paquete: psych

## 1. Definiciones y conceptos generales
- Población: es el conjunto o universo de todos los elementos a partir de los cuales se quieren tomar decisiones.  
- Unidad elemental: es cada uno de los elementos de la población, es el ítem sujeto a ser observado.
- Variable: es cualquier característica que toma diferentes valores en las unidades elementales.  
- Clasificación de variables: pueden ser cualitativas o categóricas (no medibles numéricamente) o cuantitativas (las operaciones matemáticas tienen sentido en éste tipo de variables). Las variables cualitativas pueden ser nominales u ordinales (categorías jerárquicas). Las variables cuantitativas pueden ser continuas (pueden asumir cualquier valor de un intervalo) o discreta (pueden tomar valores aislados de un intervalo).
- Población estadística: conjunto de los valores que asume la variable en cada unidad de la población.  
- Parámetro: medida que resume información de la población.  
- Muestra: subjconjunto de elementos de la población bajo estudio.  
- Estadístico: medida que resume información de la muestra.  
- Análisis descriptivo de datos: tablas, gráficos e indicadores utilizados para resumir y presentar un conjunto de datos. En este módulo trabajaremos con tablas y medidas resumen y en el módulo 4 continuaremos con los gráficos.   
  
## 2. Medidas de descripción de datos muestrales
### Medidas de localización o posición: permiten localizar al conjunto de datos de destintos puntos de vista.   
* valor mínimo: menor valor observado entre los datos   
* valor máximo: máximo valor observado entre los datos  
* percentiles: p $\alpha$ es el valor de la variable que acumula el $\alpha$ de las observaciones ordenadas  
* promedio o media aritmética $(\overline{x})$: es la suma de los valores observados dividida por el número total de datos  
$$\overline{x} = 1/n \sum_{i=1}^{n} x_i $$   
* primer cuartil $(Q_1)$: primer cuartil, es el valor de la variable que acumula el 25% inferior de las observaciones ordenadas de menor a mayor  
* mediana $(Q_2)$: segundo cuartil, es el valor de la variable que acumula el 50% de las observaciones ordenadas de menor a mayor  
* tercer cuartil $(Q_3)$: tercer cuartil, es el valor de la variable que acumula el 75% inferior de las observaciones ordenadas de menor a mayor  
* modo/moda: es el valor de la variable que se presenta una mayor cantidad de veces  

### Medidas de dispersión o variabilidad: ponen de manifiesto las diferencias entre los distintos valores de un conjunto de datos.  
* rango (R): es la máxima diferencia que observada para la variable  
$$R = x_{máx} - x_{mín}$$

* rango intercuartil (RI): diferencia entre $Q_1$ y  $Q_3$, en valor absoluto, representa al 50% central de los datos  
* variancia $(s^2)$: medida de la variavilidad, en promedio, de la media   
$$s^2 = \frac{1}{n - 1} \displaystyle\sum_{i=1}^{n} (x_i - \bar{x})^2$$
* desvío estándar (s): raiz cuadrada positiva de la variancia, su ventaja es que está expresada en las mismas unidades de la variable  
* coeficiente de variación (CV): es la desviación estándar dividida por la media aritmética, represanda la desviación estandar medida en unidades de la media aritmética:  
$$CV = \frac{s}{|\bar{x}|}$$

### Medidas de forma: Describen la forma en la que distribuyen los datos.  
* asimetría: la forma de distribución de los datos puede ser simétrica, con asimetría hacia la derecha (positiva) o hacia la izquierda (negativa).   
* curtosis: la forma de distribución de los datos puede ser muy concentrada alrededor de la media (curtosis grande, leptocúrtica), más _achatada_ (curtosis pequeña, platicúrtica) o ninguna de las dos (mesocúrtica, coeficiente igual a cero). También se suele decir de cola pesadas (curtosis pequeña, coeficiente negativo, platicúrtica) o de cola livianas (curtosis grande, coeficiente positivo, leptocúrtica).  
  
## 3. Medidas de descripción de datos poblacionales  
Como se mencionó anteriormente, las medidas descriptivas de la población se donimnan parámetros. A los fines de este curso haremos una analogía directa entendiendo que los estadísticos muestrales media $(\bar{x})$, variancia $(s^2)$ y desvío (s) son _buenos estimadores_ del promedio general $(\mu)$, variancia poblacional $(\sigma^2)$ y desvío poblacional $(\sigma)$.

## 4. Obtención de estadísticas en R  
Asignación del directorio de trabajo y carga del set de datos. Utilizaremos nuevamente la base de datos del módulo anterior 
```{r wd}
setwd("G:/Mi unidad/Curso R - IPS/Clase 3") # chequear previamente la dirección
WQT <- read.csv("G:/Mi unidad/Curso R - IPS/Clase 3/Datos_WQT.csv") 
colnames(WQT)<-c("ID","pH", "Temp.(ºC)", "Turbidez (NTU)", "DO (mg/L)", "Conductividad (µS/cm)") #reasignación de nombres a las columnas
```  
Consulta de las estadísticas por columna  
-- Medidas de tendencia central  
```{r tendencia central}
mean(WQT$pH) # de este modo obtendremos la media (promedio) de los datos incluidos en la columna pH
median(WQT$pH)
quantile(WQT$pH,probs = 0.5)
quantile(WQT$pH,probs = 0.25)#tambien considerados de pocisión relativa
quantile(WQT$pH,probs = 0.75)
IQR(WQT$pH)
moda=function(x) {   #genero una función auxiliar para hallar la moda
  y<- unique(x) # genero un vector con todos los valores únicos, es decir no cuento los repetidos. Revisar la función unique en:https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/unique
  y[which.max(tabulate(match(x, y)))] # pido el valor máximo del conteo hecho con la función tabulate a la vector generado por el match realizado entre el primer argumento al que le apliqué el segundo. Para ampliar esta función revisar: https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/tabulate y https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/match 
}
moda(WQT$pH)     
table(WQT$pH) #siempre chequear, ya que puede haber más valores
```
  
- Medidas de dispersión o variabilidad
```{r variabilidad}
min(WQT$pH)  
max(WQT$pH)
range(WQT$pH)
var(WQT$pH)
sqrt(var(WQT$pH))
sd(WQT$pH)
CV<-function(x){
  y<-100*sd(WQT$pH)/mean(WQT$pH)
  return(y)
}
CV(WQT$y)
```  
  
- Medidas de forma  
```{r forma}  
library(psych)
skew(WQT$pH) # coeficiente positivo, asimetria hacia la derecha
kurtosi(WQT$pH) # coeficiente negativo, curtosis pequeña o platicúrtica
shapiro.test(WQT$pH) # recordemos que la H0) del test es que la muestra proviene de una población normal.En este caso el p-value nos lleva a rechazar H0 (p<0.05), por lo tanto la muestr no proviene de una distribución normal.
```  
  
Consulta de las estadísticas en conjunto  
- Con las funciones apply
```{r apply}
apply(WQT,2,mean) # (data frame, fila/columna, función)
lapply(WQT,mean) # el resultado tiene formato de fila
sapply(WQT, mean) # el resultado es un vector
tapply(WQT$pH,WQT$`DO (mg/L)`>7,mean) # (columna a aplicar la función, condición, función)
```  
- Con la función summary  
```{r summary}
summary(WQT) 
```  
- Con la libreria psych
```{r library, include=TRUE}
library(psych)
describe(WQT$pH) 
describe(WQT)
```
