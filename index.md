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
**Figura 1**. Comportamiento de la variable Unidades en el tiempo.

Con el fin de tener un mejor entendimiento de cómo es el comportamiento de esta serie de tiempo, en la figura 2 se observa el comportamiento mensual de las unidades de vehículos registrados; esta vista nos permite entender de una mejor manera este comportamiento al reducir el ruido que introduce la alta variación de la inscripción de vehículos diaria de la figura 1. Acá se evidencia mejor los picos producidos al final de cada año.

![image](/images/figura2.png)
**Figura 2**.  Comportamiento de la variable Unidades en el tiempo de manera mensual.

De acuerdo con James, Witten, Hastie y Tibshirani [3, p.427], en una serie de tiempo no se puede suponer que las observaciones son independientes entre sí, más aún, los valores cercanos en el tiempo tienden a tener una autocorrelación similar entre sí. Para ser claros, se puede considerar los pares de observaciones (Ut, Ut-l), un retraso de l días (o meses). Si tomamos todos esos pares en la serie de Ut y calculamos su coeficiente de correlación, esto da la autocorrelación en el rezago l (también llamado lag l). La figura 3 muestra la función de autocorrelación para todos los rezagos (en días) hasta 31. A partir de esta, se observa una correlación considerable con 27, 21, 14, 7, 1 días de retraso, esto nos sugiere que para cada una de estas cantidades de días existe una tendencia estacional, siendo la más fuerte la de 27 días; es decir, cada 27 días la cantidad de vehículos inscritos en el RUNT tiene una intensidad relativamente similar.

![image](/images/figura3.png)

**Figura 3**. Función de autocorrelación donde el eje x representa el número de días de retraso (lags), y el eje *y*, el coeficiente de correlación; los picos corresponden a los días 1, 7, 14, 21 y 27, respectivamente.

El anterior ejercicio también puede ser llevado a cabo tomando las unidades de vehículos inscritas de manera mensual; la figura 4 muestra la función de autocorrelación si se toma este enfoque. En esta se puede observar una clara tendencia estacional anual (línea fucsia), lo cual ratifica lo observado en la figura 2. En otras palabras, cada 12 meses la cantidad de vehículos inscritos en el RUNT tiene una intensidad relativamente similar, y este comportamiento se hace más evidente en el mes de diciembre.


![image](/images/figura4.png)
**Figura 4**. Función de autocorrelación donde el eje x representa el número de meses de retraso (lags), y el eje *y*, el coeficiente de correlación; el pico en fucsia representa el coeficiente de correlación para un retraso de 12 meses.

## **Modelo de predicción**

Para la predicción de valores futuros de una serie de tiempo existen muchos posibles modelos como, por ejemplo, la transformada rápida de Fourier, regresiones lineales, suavisado exponencial, método Theta, redes neuronales convolucionales temporales, redes neuronales recurrentes, bosques aleatorios, entre otros. Para la realización de este proyecto se probaron varios de estos modelos, y se encontró que las redes neuronales convolucionales temporales y los bosques aleatorios presentaban un relativo buen desempeño. No obstante, dado que para cada tipo de modelo puede existir diferentes hiperparámetros, es necesario, por lo menos, conocer el funcionamiento del modelo y el significado de estos, de lo contrario, el proceso de *tuning* (encontrar la mejor combinación de hiperparámetros posible) podría convertirse en una tarea a "ciegas" y, en consecuencia, ineficiente; este es mi caso, por lo cual, prioricé aquellos modelos con los cuales ya estuviera familiarizado, y que a su vez, tuviera un buen desempeño, lo que finalmente me llevó a escoger el modelo de bosques aleatorios. En conclusión, es posible que el modelo propuesto a continuación no sea el más idóneo para lograr el objetivo establecido, sin embargo, puede ser una buena aproximación.

Como ya se ha mencionado, en una serie de tiempo la variable respuesta (en este caso Unidades) depende del tiempo, por tal motivo, una técnica netamente de regresión podría ser insuficiente, ya que no se estaría capturando completamente dicha dependencia para las predicciones futuras. Más específicamente, un modelo netamente de regresión produce una estimación de la variable respuesta (Unidades) dada la variable predictora (fecha) **actual** [4]; sin embargo, si adicionalmente se toman los últimos l valores de la serie (lags) para predecir el siguiente valor podría resultar en un modelo con mejor desempeño, ya que permite que el modelo varíe según la historia a corto plazo de la serie [5, p.206].

Este proyecto fue desarrollado en Python usando la librería especializada en predicción de series de tiempo llamada Darts. Aunque la librería no especifica en su documentación qué técnica de predicción está utilizando, se **presume** que esta es la técnica de **Predicción autorregresiva recursiva de varios pasos**. 

**¿Qué es la Predicción Autorregresiva Recursiva de Varios Pasos?**

Esta técnica consiste en utilizar el modelo que predice un solo paso a la vez, varias veces de manera recursiva; por otro lado, el término autorregresivo hace referencia a cuando un modelo utiliza valores anteriores (lags) para predecir valores futuros. Las figuras 5 y 6 detallan este procedimiento.


![image](/images/figura5.png)
**Figura 5**. Fórmula del proceso recursivo autorregresivo de la predicción.


![image](/images/figura6.gif)

**Figura 6**. Animación del proceso recursivo de predicción. Tomado de: https://www.cienciadedatos.net/documentos/py27-forecasting-series-temporales-python-scikitlearn.html

## **Resultados**

### Predicción del año 2017

con el fin de probar y validar los ajustes del modelo, se creó una primera versión la cual se entrenó sobre los datos del periodo desde 2012 hasta el 2016, donde después de varias combinaciones de hiperparámetros, se encontró que una buena configuración es:

- **lags**: de acuerdo a las conclusiones encontradas en el análisis de autocorrelación, este valor se estableció en 365 días.

- **n_estimators**: Corresponde al número de árboles en el bosque, este valor se estableció en 500.

- **max_deph**: Es la profundidad máxima del árbol, este valor se estableció en 30.

A continuación, se presentan los resultados obtenidos al predecir el año 2017. En la figura 7 muestra en color azul la predicción hecha por el modelo, y en negro, los datos reales; visualmente, la predicción parece ser relativamente buena, sin embargo, no logra capturar completamente bien los altos picos presentados a lo largo del año, ya que aunque estos se presentaron en cierta medida el año anterior (2016), estos no fueron tan altos como los del 2017, estos picos **podrían** deberse a un fenómeno de las series de tiempo llamado **variación irregular**, este fenómeno se trata de una variación aleatoria y, por tanto, impredecible.


![image](/images/figura7.png)

**Figura 7**. Predicción del año 2017. En el eje x se muestra la fecha, y en el eje *y*, las unidades de vehículos. El azul es la predicción de la serie, y el negro son los valores reales. 

Para ver mejor el funcionamiento del modelo, en la figura 8 se puede observar la predicción desde una perspectiva mensual. A partir de esta, se observa que el modelo subestima los primeros 6 meses del año, y sobreestima los siguientes 6, aunque sin alejarse excesivamente de los datos originales.

![image](/images/figura8.png)

**Figura 8**. Predicción del año 2017 desde una perspectiva mensual. En el eje x se muestra la fecha, y en el eje *y*, las unidades de vehículos. El azul es la predicción de la serie, y el negro son los valores reales. 

**Métricas**

Para evaluar la predicción del modelo, se utilizaron las siguientes métricas:

- **RMSE**: el RMSE se calcula de acuerdo a la figura 9. Para esta métrica se obtuvo un valor de aproximadamente **361**, esto quiere decir que en promedio las predicciones se alejaban del dato original en 361 unidades.

![image](/images/figura9.png)
**Figura 9**. Fórmula para calcular la raíz del error cuadrático medio (RMSE)

- **R-cuadrado**: 

## **Referencias**

[1] "Qué es y cómo funciona el RUNT". Inicio - Programa Servicios de Transito. https://serviciosdetransito.com/index.php/noticias/139-que-es-y-como-funciona-el-runt (accedido el 28 de agosto de 2021).

[2] Departamento de Matemáticas. http://www.estadistica.mat.uson.mx/Material/seriesdetiempo.pdf (accedido el 28 de agosto de 2021).

[3] G. James, D. Witten, T. Hastie y R. Tibshirani, *An Introduction to Statistical Learning With Applications in R*, 2a ed. New York, NY: Springer, 2021.

[4] Colaboradores de los proyectos Wikimedia. "Análisis de la regresión - Wikipedia, la enciclopedia libre". Wikipedia, la enciclopedia libre. https://es.wikipedia.org/wiki/Análisis_de_la_regresión (accedido el 31 de agosto de 2021).

[5] T. Hastie, R. Tibshirani y J. Friedman, *The Elements of Statistical Learning Data Mining, Inference, and Prediction*, 2a ed. New York: Springer, 2001.




