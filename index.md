# Trabajo 4 Técnicas en Aprendizaje Estadístico

# **Integrantes**

- **Alejandro Ortiz Mejía**

**Nota**: Puede hacer zoom a este blog para visualizar mejor las imágenes, o puede hacer clic derecho sobre cada imagen y dar clic en "abrir imagen en una nueva pestaña" para una visualización en máxima resolución.

## **Planteamiento del Problema** 

El RUNT significa Registro Único Nacional de Tránsito, y funciona como una gran base de datos centralizada que contiene información sobre todos los vehículos en el país [1].
Diariamente las personas acuden ante la entidad del tránsito para registrar su vehículo, así que se tienen registros del número de vehículos registrados por cada día del año; este es nuestro caso.

Predecir el número de vehículos que serán registrados en el futuro tomando como base los que se han registrado en el pasado podría ser de gran interés para las autoridades de mobilidad en el país, ya que esto les permitiría entender las dinámicas de las futuras situaciones en ámbitos como la congestión vehicular o la contaminación del aire, con lo cual pueden anticiparse y tomar decisiones que permitan prevenir situaciones que lleven a las ciudades al colapso.

Una **serie de tiempo** son datos estadísticos que se recopilan, observan o registran en intervalos de tiempo regulares (diario, semanal, semestral, anual, entre otros) [2]. A partir de esta definición podemos decir que nuestro conjunto de datos es una serie de timepo.

### **Descripción del conjunto de datos**

El conjunto de datos contiene 2192 registros de la cantidad de vehículos registrados ante el RUNT diariamente registrados cronológicamente, este conjunto tiene dos variables:

- Fecha: la fecha está en formato Día/Mes/Año, y contiene las fechas desde 1 de enero de 2012 hasta el 31 de diciembre de 2017 sin ningún dato faltante; es decir, 2192 días en total.

- Unidades: Es la cantidad de vehículos registrados en el correspondiente día.

**¿Cómo se ve el conjunto de datos?**

En la figura 1 se puede observar el comportamiento de la variable en el tiempo. En primera instancia, se puede notar que año tras año hay muchos picos, y en partícular, hay un gran pico que se produce siempre a final de año, a este fenómeno se le conoce como *variación estacional*; es decir, se produce cuando en una serie de tiempo ocurren variaciones o movimientos que recurren año tras año en los mismos meses (o en los mismos trimestres) del año poco más o menos con la misma intensidad.

![image](/images/figura1.png)
Figura 1. Comportamiento de la variable Unidades en el tiempo.

Con el fin de tener un mejor entendimiento de cómo es el comportamiento de esta serie de tiempo, en la figura 2 se observa el comportamiento mensual de las unidades de vehículos registrados; esta vista nos permite entender de una mejor manera este comportamiento al reducir el ruido que introduce la alta variación de la inscripción de vehículos diaria de la figura 1. Acá se evidencia mejor los picos producidos al final de cada año.

![image](/images/figura2.png)
Figura 2.  Comportamiento de la variable Unidades en el tiempo de manera mensual.

De acuerdo con James, Witten, Hastie y Tibshirani [3, p.427], en una serie de tiempo no se puede suponer que las observaciones son independientes entre sí, más aún, los valores cercanos en el tiempo tienden a tener una autocorrelación similar entre sí. Para ser claros, se puede considerar los pares de observaciones (Ut, Ut-l), un retraso de l días (o meses). Si tomamos todos esos pares en la serie de Ut y calculamos su coeficiente de correlación, esto da la autocorrelación en el rezago l (también llamado lag l). La figura 3 muestra la función de autocorrelación para todos los rezagos (en días) hasta 31. A partir de esta, se observa una correlación considerable con 27, 21, 14, 7, 1 días de retraso, esto nos sugiere que para cada una de estas cantidades de días existe una tendencia estacional, siendo la más fuerte la de 27 días; es decir, cada 27 días la cantidad de vehículos inscritos en el RUNT tiene una intensidad relativamente similar.

![image](/images/figura3.png)

Figura 3. Función de autocorrelación donde el eje x representa el número de días de retraso (lags), y el eje *y*, el coeficiente de correlación; los picos corresponden a los días 1, 7, 14, 21 y 27, respectivamente.

El anterior ejercicio también puede ser llevado a cabo tomando las unidades de vehículos inscritas de manera mensual; la figura 4 muestra la función de autocorrelación si se toma este enfoque. En esta se puede observar una clara tendencia estacional anual (línea fucsia), lo cual ratifica lo observado en la figura 2. En otras palabras, cada 12 meses la cantidad de vehículos inscritos en el RUNT tiene una intensidad relativamente similar, y este comportamiento se hace más evidente en el mes de diciembre.


![image](/images/figura4.png)
Figura 4. Función de autocorrelación donde el eje x representa el número de meses de retraso (lags), y el eje *y*, el coeficiente de correlación; el pico en fucsia representa un retraso de 12 meses.

## **Creación, entrenamiento y validación del modelo**

Para la predicción de valores futuros de una serie de tiempo existen muchos posibles modelos como, por ejemplo, la transformada rápida de Fourier, regresiones lineales, suavisado exponencial, método Theta, redes neuronales convolucionales temporales, redes neuronales recurrentes, bosques aleatorios, entre otros. Para la realización de este proyecto se probaron varios de estos modelos, y se encontró que las redes neuronales convolucionales temporales y los bosques aleatorios presentaban un relativo buen desempeño. No obstante, dado que para cada tipo de modelo puede existir diferentes hiperparámetros, es necesario, por lo menos, conocer el funcionamiento del modelo y el significado de estos, de lo contrario, el proceso de *tuning* (encontrar la mejor combinación de hiperparámetros posible) podría convertirse en una tarea a "ciegas" y, en consecuencia, ineficiente; este es mi caso, por lo cual, prioricé aquellos modelos con los cuales ya estuviera familiarizado, y que a su vez, tuviera un buen desempeño, lo que finalmente me llevó a escoger el modelo de bosques aleatorios. En conclusión, es posible que el modelo propuesto a continuación no sea el más idóneo para lograr el objetivo establecido, sin embargo, puede ser una buena aproximación.


## **Referencias**

[1] "Qué es y cómo funciona el RUNT". Inicio - Programa Servicios de Transito. https://serviciosdetransito.com/index.php/noticias/139-que-es-y-como-funciona-el-runt (accedido el 28 de agosto de 2021).

[2] Departamento de Matemáticas. http://www.estadistica.mat.uson.mx/Material/seriesdetiempo.pdf (accedido el 28 de agosto de 2021).

[3] G. James, D. Witten, T. Hastie y R. Tibshirani, *An Introduction to Statistical Learning With Applications in R*, 2a ed. New York, NY: Springer, 2021.




