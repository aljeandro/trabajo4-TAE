# Trabajo 4 Técnicas en Aprendizaje Estadístico

# **Integrantes**

- **Alejandro Ortiz Mejía**

## **Planteamiento del Problema** 

El RUNT significa Registro Único Nacional de Tránsito, y funciona como una gran base de datos centralizada que contiene información sobre todos los vehículos en el país [1].
Diariamente las personas acuden ante la entidad del tránsito para registrar su vehículo, así que se tienen registros del número de vehículos registrados por cada día del año; este es nuestro caso.

Predecir el número de vehículos que serán registrados en el futuro tomando como base los que se han registrado en el pasado podría ser de gran interés para las autoridades de mobilidad en el país, ya que esto les permitiría entender las dinámicas de las futuras situaciones en ámbitos como la congestión vehicular o la contaminación del aire, con lo cual pueden anticiparse y tomar decisiones que permitan prevenir situaciones que lleven a las ciudades al colapso.

Una **serie de tiempo** son datos estadísticos que se recopilan, observan o registran en intervalos de tiempo regulares (diario, semanal, semestral, anual, entre otros) [2]. A partir de esta definición podemos decir que nuestro conjunto de datos es una serie de timepo.

### **Descripción del conjunto de datos**

El conjunto de datos contiene 2192 registros de la cantidad de vehículos registrados ante el RUNT diariamente, este conjunto tiene dos variables:

- Fecha: la fecha está en formato Día/Mes/Año, y contiene las fechas desde 1 de enero de 2012 hasta el 31 de diciembre de 2017 sin ningún dato faltante; es decir, 2192 días en total.

- Unidades: Es la cantidad de vehículos registrados en el correspondiente día.

**¿Cómo se ve el conjunto de datos?**

En la figura 1 se puede observar el comportamiento de la variable en el tiempo. En primera instancia, se puede notar que año tras año hay muchos picos, y en partícular, hay un gran pico que se produce siempre a final de año, a este fenómeno se le conoce como *variación estacional*; es decir, se produce cuando en una serie de tiempo ocurren variaciones o movimientos que recurren año tras año en los mismos meses (o en los mismos trimestres) del año poco más o menos con la misma intensidad.




## Referencias

[2] "Qué es y cómo funciona el RUNT". Inicio - Programa Servicios de Transito. https://serviciosdetransito.com/index.php/noticias/139-que-es-y-como-funciona-el-runt (accedido el 28 de agosto de 2021).

[2] Departamento de Matemáticas. http://www.estadistica.mat.uson.mx/Material/seriesdetiempo.pdf (accedido el 28 de agosto de 2021).




