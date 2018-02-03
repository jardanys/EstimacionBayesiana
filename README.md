# Estimación Bayesiana de intención de voto por Gustavo Petro en elecciones presidenciales del 2018 
#### Nestor J. Serrano - Camilo A. Aguilar

### Abstract
En la estadística existen dos tipos de enfoques, el clásico y el bayesiano. Éste último es un tipo de inferencia estadística donde las evidencias u observaciones pasadas, se emplean para actualizar o inferir la probabilidad de que una hipótesis pueda ser cierta. El objetivo central del presente documento, consiste en aplicar el enfoque de la estadística bayesiana a la estimación de la intensión de voto por un candidato (Gustavo Petro) en las próximas elecciones presidenciales en Colombia, las cuales serán llevadas a cabo en el año 2018. Es posible utilizar la misma metodología para estimar resultados para otros candidatos. La información a priori utilizada corresponde a los resultados electorales del 2010, donde el candidato de estudio estuvo entre los elegibles. Para la información actual, se realiza una recolección de información contenida en diferentes encuestas aplicadas a lo largo del año 2017.

Como resultado se espera que el porcentaje de votos por Gustavo Petro para las elecciones presidenciales de Colombia 2018 sea $10.29$%. Con un nivel de credibilidad del 95% el resultado se encuentraría entre $9.92$% y $10.66$%. Por otra parte, la probabildiad que en 2018 se obtenga un resultado mas favorable que en el año 2010, es un valor muy cercano a 1, es decir que con un nivel amplio de certeza se peude asegurar que Gustavo Petro obtendrá más votos en las próximas eleeciones que en las elecciones de 2010, pero la probabilidad que en 2018 dicho candidato sea Presidente de Colombia en primera vuelta, suponiendo porcentaje de votos mayor que $50$%, es casi cero.


```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(ggplot2)
library(readxl)
library(dplyr)
library(knitr)
```


## I. INTRODUCCIÓN

Durante el presente artículo se aborda el enforque bayesiano para estimar la intención de voto por el candidato presidencial Gustavo Petro. En particular, el candidato cuenta con un reconocido recorrido político el cual será utilizado como conocimiento a priori del problema. Además, como es costumbre en fechas cercanas a los comicios electorales, varias compañías encuestadoras (o de investigación), realizan diferentes estudios basados en encuestas por muestreo, información que será estructurada de forma estadísticamente conveniente para ser utilizada como nuestra información actual. En este documento primero se define el enfoque bayesiano y cómo lo utilizaremos. Segundo se describe la información y distribución a priori y actual (función de verosimilitud). Tercero se define la distribución de la posteriori y ponderación de los estimadores. Cuarto se realizan los cálculos de la estimación y de los intervalos de confianza. Quinto se estudian algunas aplicaciones de interés sobre los resultados generados y por último se muestran las conclusiones del estudio.


## II. ENFOQUE BAYESIANO

En general el objetivo de la estadística bayesiana, es representar la incertidumbre previa sobre los parámetros del modelo con una distribución de probabilidad, y actualizar esta incertidumbre anterior con datos para producir una distribución de probabilidad posteriori para el parámetro que contiene incertidumbre. La diferencia con la inferencia estadística es que esta toma a los parámetros fijos y en la Bayesiana se utiliza parámetros variables. Resulta plausible escribir el teorema de Bayes en términos de distribuciones:

\begin{equation}
f(\theta | Y) = \dfrac{f(Y|\theta) \cdot f(\theta)}{f(Y)}
\end{equation}

donde $f(\theta | Y)$ es la distribución a posteriori para el parámetro, $f(Y|\theta)$ se llama la función de verosimilitud, $f(\theta)$ es la función a priori del parámetro y $f(Y)$ es la función marginal de los datos. En particular ésta última función resulta ser un escalar, por lo que la distribución posteriori resulta ser entonces proporcional al producto de la función de \textit{verosimilitud} por la fución \textit{a priori}:

\begin{equation}\label{ec2}
f(\theta | Y) \propto f(Y|\theta) \cdot f(\theta)
\end{equation}

Finalmente, la estimación bayesiana de $\theta$ se define como el valor esperado, de la función de densidad posteriori:

\begin{equation}\label{ec3}
\theta_{B} = E[\theta | Y]
\end{equation}

En resúmen, los pasos que se siguen durante el presente artículo, para el desarrollo del objetivo son:

  * Definir una distribución a priori.
  * Obtener información actual con la cual calcular la función de verosimilitud.
  * Calcular la función de verosimilitud
  * Calcular la distribución a posteriori.
  * Realizar la estimación bayesiana.


## III. DESCRIPCIÓN DE LA INFORMACIÓN

De acuerdo con el enfoque bayesiano se utilizan dos fuentes de información. Para la \textit{a priori}, se escogió el resultado de las votaciones en las elecciones presidenciales de $2010$ y para la \textit{verosimilitud} es la función de las diferentes encuestas sobre la intención de voto para las próximas elecciones de $2018$. El objetivo es estimar la votación para las próximas elecciones, utilizando los dos tipos de información descritos a continuación. 


### Información \textit{a priori}

Las elecciones presidenciales en Colombia del año $2010$, para el período $2010-2014$, se efectuaron el domingo 30 de mayo de $2010$ y su proceso de escrutinio terminó oficialmente el 8 de junio de 2010. Sin embargo, los resultados de dicho escrutinio mostraron que matemáticamente, ningún candidato alcanzó la mayoría absoluta de los votos por lo que se llevó a cabo una segunda vuelta el día domingo 20 de junio, la cual dio como vencedor al candidato Juan Manuel Santos, quien fue elegido presidente.

Entre los candidatos a estas elecciones, para la primera vuelta, se encontraba el sujeto de interés y ejemplo para este trabajo, Gustavo Petro, quien obtuvo un porcentaje de votación de $9.13\%$, valor que utilizaremos como nuestro parámetro $\theta_{p}$. El total de personas que votaron por Gustavo Petro fue $1'331.267$ de acuerdo con el escrutinio final. El total de personas que votaron fue de $14'781.020$. 


### Distribución \textit{a priori}

Es posible determinar el parámetro a priori $\theta_{p}$, definido en el intervalo $[0,1]$, para lo cuál usaremos la distribución $Beta(\alpha,\beta)$ como distribución asociada a dicho parámetro. La función de densidad está dada por la siguiente ecuación:

\begin{equation}\label{ec4}
\begin{aligned}
f(\theta) &= \dfrac{\Gamma(\alpha + \beta)}{\Gamma(\alpha) \cdot \Gamma(\beta)} \cdot \theta^{\alpha-1} \cdot (1-\theta)^{\beta-1} \\\\
          &\propto \theta^{\alpha-1} \cdot (1-\theta)^{\beta-1}
\end{aligned}
\end{equation}

con $\alpha, \beta >0$. Para nuestro caso particular, $\alpha=1'331.267$ y $\beta=14'781.020-\alpha=13'449.753$.


```{r infopriori, message=FALSE, warning=FALSE, echo=F}
theta_p <- 0.0913
alfa_p <- 1331267
```


### Información \textit{actual}

La información actual se obtiene del análisis de las encuestas que muestran la intención de voto por Gustavo Petro para las elecciones presidenciales de 2018 en Colombia. En total se tienen $9$ encuestas con fecha de elaboración, participación de votantes por candidato y número de sujetos encuestados. La información fue obtenida a través de los comunicados de prensa Colombiana (Ver apéndice), de diferentes empresas que han realizado este tipo de sondeos, \textit{Intención de voto de los Colombianos para las elecciones presidenciales de 2018}.

La información recolectada se encuentra contenida en el fichero \textit{Datos.xlsx}. La siguiente instrucción cargará la información recolectada al entorno de trabajo en **R**, y además ejecutará una muestra de la misma:


```{r info_Actual1, echo=T, results='asis'}
require(readxl)
encuestas <- read_excel("Datos.xlsx", sheet = "Encuestas")
knitr::kable(head(encuestas[,3:7],9))
```

Se define la información actual como una ponderación de las $9$ encuestas, otorgando mayor peso a aquellas más cercanas a la fecha del presente artículo. La metodología de ponderación, se deriva de constrastar encuestas de intención de voto con los resultados electorales finales, inherentes a las encuestas.

El calculo de la ponderación utiliza $31$ encuestas realizadas previo a las elecciones a la alcaldía de Bogotá 2012^[Se utilizaron estas encuestas debido a que el candidato análizado en este documento resultó siendo en particular el ganador, sin embargo es posible utilizar encuestas para cualesquiera otras elecciones], donde Gustavo Petro fué elegido como el ganador. Se define el indice de precisión $IP_i$, para las encuestas como sigue:

\begin{equation}
IP_i = 1-\dfrac{|R_{f}-e_i|}{e_i}
\end{equation}

donde $R_f=32.22\%$ es el porcentaje real de personas que votaron a favor del candidato vencedor y $e_i$, $i\in\{1,2,...,31\}$ son los porcentajes estimados por cada una de las $31$ encuestas. El resultado para cada $IP_i$ puede leerse en la hoja \textit{'Precisión'}, del fichero \textit{Datos.xlsx}, como sigue:


```{r inf_Actual2, echo=TRUE, message=FALSE, warning=FALSE, results='asis'}
Precision <- read_excel("Datos.xlsx", sheet = "Precision")
knitr::kable(head(Precision))
```


Nótese que entre más cercana sea la encuesta a la fecha de elecciones (o más reciente sea la encuesta), la precisión $IP_i$ resultante es mayor, es decir, sin tener en cuenta otro tipo de variables como credibilidad de la empresa, tipo de muestreo o enfoque de las encuestas; generalmente las encuestas entre más recientes o cercanas a la fecha de votación tienen una tendencia a ser más precisas o acertar. De esta manera es posible no discriminar o rechazar ninguna encuesta y utilizar toda la información disponible actual. 


```{r inf_Actual3, echo=TRUE, message=FALSE, warning=FALSE, fig.height = 3, fig.width = 5, fig.align='center'}
ggplot(Precision, aes(x=Fecha_Encuesta, y=Precision), ylim=c(0,1)) + 
       geom_point(shape=1) + geom_smooth(method=lm , color="red", se=TRUE) +
       ggtitle("Precisión Encuestas Elecciones Alcaldia Bogotá 2012") + 
       xlab("Fecha de las encuestas") + 
       ylab("Precisión")
```


Por esta razón, la metodología de ponderación usada, otorga un mayor peso a las encuestas más recientes, valor que disminuye cuando la fecha es más lejana. Se aplica la ponderación de la siguiente manera: 


```{r regla, echo=TRUE, message=FALSE, warning=FALSE}
encuestas$Peso <- ifelse(encuestas$Orden_Fecha<=4, 0.15, 
                         ifelse(encuestas$Orden_Fecha<=7, 0.1, 
                                ifelse(encuestas$Orden_Fecha<=9,0.05)))
```


Nótese, que la variable $Orden_Fecha$ enumera de $1$ a $9$ la fecha más reciente a la más antigua. La suma de los pesos ponderados es igual a 1, para las nuevas fechas. Como se oberva en la gráfica anterior, la variabilidad de las fechas más reciente es alta, por ello se toman las cuatro fechas más recientes con el un valor ponderado de $0.5$, las sigueintes tres con un peso de $0.1$ y las últimas dos con un peso de $0.05$. 

Finalmente, el porcentaje ponderaro de personas $(parámetro de la información actual)$, con intensión de votar por G. Petro, estará dado por $\theta_{a} = \sum_{i=1}^{9} (VotosPetro_i \cdot Peso_i)$, así:


```{r par_vero, echo=TRUE, message=FALSE, warning=FALSE}
encuestas$parametro <- encuestas$Votos_Petro * encuestas$Peso
theta_c <- sum(encuestas$parametro)
theta_c
```


### Distribución de la información \textit{actual}: Función de Verosimilitud

Si la población de votantes se encuentra dividida en dos grupos, referenciados como \textit{éxitos}, cuando tienen intensión de votar por G. Petro o \textit{fallos}, en caso contrario. El parámetro $\theta_{a}$ de la información actual, resulta ser la probabilidad de éxito, para una sucesión de eventos (intensión de voto individual), que se repiten un número determinado de veces (número de personas que votarían), y cuyo resultado en cada suceso puede ser éxito o fracaso. Matemáticamente decimos que cada uno de estos eventos $Y_i$, suponiendo independencia entre ellos, sigue una distribución \textit{Bernoulli} con parámetro $\theta_{a}$.

La variable $Y_i$ por tanto puede tomar los valores $0$ ó $1$. De esta manera, la expresión que define su verosimilitud (es decir, la información de los datos observados) puede expresarse como:

\begin{equation}\label{ec6}
\begin{aligned}
P(Y|\theta_a) &= \prod_{i=1}^{n} P(Y_i|\theta_a) \\
              &= \prod_{i=1}^{n} \theta_{a}^{Y_i} \cdot (1 - \theta_{a})^{1-Y_i} \\
              &= \theta^{\mathbb{Y}} \cdot  (1-\theta)^{(n-\mathbb{Y})}
\end{aligned}
\end{equation}

con $\mathbb{Y}=\sum Y_i$.


## IV. DISTRIBUCIÓN \textit{A POSTERIORI}

De acuerdo con la ecuación \eqref{ec2}, la distribución posterior $f(\theta|Y)$ está definida a partir de la distribución de la información previa $f(\theta)$, y de la función de de verosimilutud $f(Y|\theta)$ definidas en las ecuaciones \eqref{ec4} y \eqref{ec6} respectivamente, Entonces:

\begin{equation}\label{ec7}
\begin{aligned}
f(\theta|Y) &\propto  \theta^{(\alpha-1)} \cdot (1-\theta)^{(\beta-1)} \cdot \theta^{\mathbb{Y}} \cdot  (1-\theta)^{(n-\mathbb{Y})}   \\
            &=  \theta^{\mathbb{Y}+\alpha+1} \cdot (1-\theta)^{n-\mathbb{Y}+\beta-1}
\end{aligned}
\end{equation}

Nótese que la distribución \textit{posteriori} resulta ser conjugada, es decir, esta distribución es de la misma familia de la distribución \textit{a priori}, con $\theta|Y \sim Beta(\mathbb{Y}+\alpha, n-\mathbb{Y}+\beta)$.

Por lo tato, la estimación bayesiana $\theta_{Bay}$, definida en la ecuación \eqref{ec3} como la esperanza de $\theta|Y$, estará definida por

\begin{equation}\label{ec8}
\begin{aligned}
\theta_{Bay}  &=  \dfrac{\mathbb{Y}+\alpha}{n-\mathbb{Y}+\beta}   \\
              &=  \left(\dfrac{n}{n+\alpha+\beta} \cdot \dfrac{\sum_{i=1}^{n} Y_i}{n} \right) + \left( \dfrac{\alpha+\beta}{n +\alpha+\beta} \cdot \dfrac{\alpha}{\alpha+\beta}\right)
\end{aligned}
\end{equation}

Donde  $\dfrac{\sum_{i=1}^{n} Y_i}{n} = \theta_{clásico}$ resulta ser el estimador clásico de la información actual y $\dfrac{\alpha}{\alpha+\beta} = \theta_{p}$ es el estimador de la información a priori. Además $A=\dfrac{n}{n+\alpha+\beta}$ y $B=\dfrac{\alpha+\beta}{n +\alpha+\beta}$ pueden ser interpretados como una ponderación entre los estimadores clásico y a priori, la cual depende del tamaño $\alpha + \beta$ de la información previa y $n$ de la información actual. Si $n$ es muy grande respecto a $\alpha + \beta$, el estimador bayesiano se acercará al estimador clásico.


### Ponderación de los estimadores

Es muy importante la ponderación otorgada a éstos estimadores, en particular $\alpha + \beta=14'781.020$ de la información a priori resulta ser más grande que $n=11.135$ de la información actual u observada, por esto resulta necesario definir la ponderación entre las dos informaciones. Con estos datos y  la estimación bayesiana, el parámetro sería muy similar a la información a priori y no tendría sentido realizar la estimación bayesiana. Con la finalidad de proporcionar credibilidad a la información más reciente. Se define diferentes pesos o ponderaciones a cada información y se obtenien nuevos valores para $n$ y $\alpha + \beta$.

Esto resulta natural al momento de estimar algún resultado electoral sobre un mismo sujeto. En Colombiana, la percepción (favorable o desfavorable) sobre un político, podría llegar a ser muy variable em fechas electorales. 

Entonces, con el fin de balancear los dos tipos de información, a los valores $\alpha+\beta$ y $n$ se le otorgan un porcentaje de confianza del $0,1\%$ y $99,9\%$ respectivamente. Por lo tanto definimos nuevos parámetros $N^*=\alpha^*+\beta^*=14.781$ para la distribución a priori de donde $\alpha^*=\theta_p \cdot N^*$ y para la información actual tendremos $n^* =11.124$ se define $\mathbb{Y}^*=\theta_{clásico} \cdot n^*$. 

Así, el estimador bayesiano tiene una distribución bajo estos nuevos parámetros; $\theta|Y^* \sim Beta(\mathbb{Y}^*+\alpha^*, n^*-\mathbb{Y}^*+\beta^*)$, por lo que su valor esperado será

\begin{equation}\label{ec9}
\begin{aligned}
E[\theta|Y^*] &= \dfrac{\mathbb{Y}^{*}+\alpha^{*}}{\mathbb{Y}^{*}+\alpha^{*} + n^{*}-\mathbb{Y}^{*}+\beta^{*}}\\
              &= \dfrac{\mathbb{Y}^{*}+\alpha^{*}}{N^{*}+n^{*}} \\
              &= 0.1029372
\end{aligned}
\end{equation}

con intervalo de credibilidad definido como: 


```{r IC, message=T, warning=FALSE}
alpha_f <- theta_p * 14781 # alpha*
N_f <- 14781 # N*
beta_f <- N_f-alpha_f # beta*

Y_f <- theta_c*11124 # Y*
n_f <- 11124 # n*

Estimador_Bayesiano <- (Y_f+alpha_f)/(N_f+n_f)
Estimador_Bayesiano

## Intervalo de credibilidad
lim_inf <- qbeta(0.025, alpha_f+Y_f , n_f-Y_f+beta_f)
lim_sup <- qbeta(0.975, alpha_f+Y_f , n_f-Y_f+beta_f)

message("IC 95% = (",lim_inf, ", ",lim_sup, ")")
```


### Calculos de interés

Con la información anterior es posible estimar, entre otras:

- Probabildiad que en 2018 se obtenga más porcentaje de votos que en 2010, es un valor muy cercano a 1.


```{r cal_1, echo=TRUE, message=FALSE, warning=FALSE}
P_mayor_theta_p <- pbeta(theta_p, alpha_f+Y_f , n_f-Y_f+beta_f, lower.tail = F)
P_mayor_theta_p
```


- Probabilidad que en 2018 obtenga más de 15% de votos es casi cero.


```{r cal_2, echo=TRUE, message=FALSE, warning=FALSE}
P_mayor_0.15 <- pbeta(0.15, alpha_f+Y_f , n_f-Y_f+beta_f, lower.tail = F)
P_mayor_0.15
```


- Probabilidad que en 2018 G. Petro sea Presidente de Colombia (suponiendo porcentaje de votos mayor de 50%) es cero.


```{r cal_3, echo=TRUE, message=FALSE, warning=FALSE}
P_Gane <- pbeta(0.5, alpha_f+Y_f , n_f-Y_f+beta_f, lower.tail = F)
P_Gane
```


## V. CONCLUSIONES

+ Se espera que el porcentaje de votos por G. Petro para las elecciones presidenciales de Colombia 2018 es $10.29$%, con un nivel de credibilidad de 95% este porcentaje se encuentra entre $9.92$% y $10.66$%. 

+ Se debe tener en cuenta para el cálculo del estimador e intervalo de credibilidad se balancearon los dos tipos de información. La información a priori son las votaciones de G. Petro para las elecciones de 2010 y la información actual son las encuestas hasta $01/10/2017$ públicadas por diferentes medio de información, donde no se tiene en cuenta las diferentes metodologías de muestreo y algunas empresas pueden brindar información poco confiable. Por esta razón se creo a discreción de los autores el Indice de Precisión. 

+ El porcetaje de votos para G. Petro estimado es menor al obtenido en las elecciones presidenciales de 2010, esto se debe al recorrido político de los últimos años y a la imagen favorable o no favorable en su gestión como alcalde de la ciudad de Bogotá D.C. y la política realizada después de la dirección de la ciudad.

+ Probabildiad que en 2018 se obtenga más porcentaje de votos que en 2010, es un valor muy cercano a 1, es decir, con certeza se peude asegurar que G. Petro obtendrá más votos en las próximas eleeciones que en las elecciones de 2010.

+ Probabilidad que en 2018 obtenga más de 15% de votos es casi cero o la probabilidad que en 2018 G. Petro sea Presidente de Colombia (suponiendo porcentaje de votos mayor de 50%) es cero. Es decir, para que G. Petro gane las elecciones u obtenga un porcentaje alto de votos, es necesario que de un giro a su campaña política o sus aspiraciones a presidencia no podrán ir más allá de la candidatura. 

## VI. APÉNDICE

El contenido estadístico de este documento se obtiene de la clase de Estadística Bayesiana dictada por la Ph.D Hanwen Zhang. En la Maestría en Estadística Aplicada de la Universidad Santo Tomas de Colombia.

A continuación se muestra las referencias informáticas y enlaces, donde se obtiene la información actual, esto fue tomado el 4 de Octubre de 2017.

* [\textcolor{blue}{Enlace, encuesta 1 - Pulso País - 20170911}](https://www.publimetro.co/co/colombia/2017/09/11/gustavo-petro-lidera-encuesta-presidencial.html)
* [\textcolor{blue}{Enlace, encuesta 2 - Pulso País - 20170801}](https://www.publimetro.co/co/colombia/2017/09/11/gustavo-petro-lidera-encuesta-presidencial.html)
* [\textcolor{blue}{Enlace, encuesta 3 - Cifrras y Conceptos - 20170803}](http://caracol.com.co/radio/2017/08/03/politica/1501721978_775306.html)
* [\textcolor{blue}{Enlace, encuesta 4 - Cifras y Conceptos - 20170501}](http://caracol.com.co/radio/2017/08/03/politica/1501721978_775306.html)
* [\textcolor{blue}{Enlace, encuesta 5 - Yanhaas - 20170920}](http://www.eltiempo.com/politica/partidos-politicos/encuesta-de-yanhaas-de-los-candidatos-a-la-presidencia-en-2018-133012)
* [\textcolor{blue}{Enlace, encuesta 6 - Invamer - 20170927}](https://noticias.caracoltv.com/colombia/sergio-fajardo-puntea-intencion-de-voto-en-encuesta-de-invamer-para-elecciones-presidenciales-2018)
* [\textcolor{blue}{Enlace, encuesta 7 - Datexco - 20170501}](http://www.eltiempo.com/politica/gobierno/encuesta-intencion-de-voto-de-datexco-a-la-presidencia-de-colombia-2018-93140)
* [\textcolor{blue}{Enlace, encuesta 8 - Guarumo - 20170701}](https://www.larepublica.co/asuntos-legales/ivan-duque-encabeza-intencion-de-voto-a-presidencia-de-2018-segun-reciente-encuesta-2527185)
* [\textcolor{blue}{Enlace, encuesta 9 - Semana - 20170520}](http://www.semana.com/nacion/articulo/gran-encuesta-presidencial/525789)









