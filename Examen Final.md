# Examen Final
## 0.	Clonar el repositorio oneAPI-samples [1 punto]
En la pagina https://github.com/oneapi-src/oneAPI-samples se copia el enlace

![This is an image](/Desarrollo/Imagen1.png)

Enlace:
https://github.com/oneapi-src/oneAPI-samples.git

Luego en mobaXterm, se ejecuta los siguientes códigos

![This is an image](/Desarrollo/Imagen2.png)

-Crear una carpeta de trabajo: 

`mkdir Examen_Final`

-Entramos a la carpeta creada:

`cd Examen_Final/`

-Clonamos el repositorio:

`git clone https://github.com/oneapi-src/oneAPI-samples.git`

Resultado:
 
![This is an image](/Desarrollo/Imagen3.png)

## 1.	Cambiar al directorio del ejemplo Nbody [1 punto]

- Directorio: oneAPI-samples/tree/master/DirectProgramming/C++SYCL/N-BodyMethods/Nbody

-Revisamos los archivos y carpetas:

`ls`

-Ingresamos a oneAPI-samples:

`cd oneAPI-samples`

-Revisamos los archivos y carpetas:

`ls`

-Ingresamos a DirectProgramming:

`cd DirectProgramming`

-Revisamos los archivos y carpetas:

`ls`

-Ingresamos a C++SYCL:

`cd C++SYCL`

-Revisamos los archivos y carpetas:

`ls`

-Ingresamos a N-BodyMethods:

`cd N-BodyMethods`

-Revisamos los archivos y carpetas:

`ls`

-Ingresamos a Nbody:

`cd Nbody`

Resultado:

![This is an image](/Desarrollo/Imagen4.png)

## 2.	Explicar brevemente el algoritmo de Nbody [3 puntos]

Nbody es un algoritmo de simulación de un sistema dinámico de partículas bajo la influencia de fuerzas físicas como la gravedad. El código de ejemplo del algoritmo Nbody simula 16 000 partículas en diez pasos de integración. Cada partícula posee sus parámetros de posición, velocidad y aceleración, el cual dependen de otras (N-1) partículas. La dinámica de las partículas se ejecuta en paralelo, por lo que el algoritmo se debe ejecutar en la GPU.

1. Se inicializa los valores de posición, velocidad, aceleración y peso. Además del número de partículas y los números de paso.
2. Luego se calcula dx,dy y dz para generar los cambios de distancias
3. Se calcula las aceleraciones dado las aceleraciones accx,accy y accz
4. Se utiliza las ecuaciones diferenciales para calcular la posición, velocidad y aceleración
5. Se realiza iteraciones

## 3.	Acceder en modo interactivo a un nodo de cómputo con GPUs (gen9 o gen11) [3 puntos]

- Compilar y ejecutar Nbody
- Proporcionar screenshot(s) de los resultados
- Por cada screenshot, añadir una breve descripción

Se busca un nodo con gpu disponible de la siguiente lista:

- nodes=1:gpu:ppn=2
- nodes=1:gen9:ppn=2
- nodes=1:gen11:ppn=2
- nodes=1:iris_xe_max:ppn=2
- nodes=1:iris_xe_max:dual_gpu:ppn=2
- nodes=1:iris_xe_max:quad_gpu:ppn=2
- nodes=1:iris_xe_max:ppn=2

-Se selecciona un nodo de la lista:

`qsub -I -l nodes=1:gen9:ppn=2 -d .`

-Ejecutamos el siguiente comando para observar los recursos disponibles:

`sycl-ls`

![This is an image](/Desarrollo/Imagen5.png)
Descripcion: Se observa que el id del nodo es 2254276.v-qsvr-1.aidevcloud, con el usuario u185963 (con el cual se conecto). Además me indica la fecha y hora en que se ha realizado la conexión: Jueves 16 de marzo del 2023 a las 12:56:05 PM PDT. El plazo máximo de uso es por 6 horas según el parámetro walltime.

Para compilar se realiza los siguientes procedimientos

`mkdir build`

`cd build`

`cmake ..`

`make`

-El siguiente comando ejecuta al algoritmo:

`make run`

![This is an image](/Desarrollo/Imagen6.png)


## 4.	Realizar un análisis de GPU Hotspots con VTune [8 puntos]
- Indicar los hotspots del programa
- Proporcionar screenshot(s) de los resultados
  - Por cada screenshot, añadir una breve descripción

Primero se ejecuta los siguientes comandos para elegir un nodo y revizar si posee gpu:

`ssh devcloud`

`qsub -I -l nodes=1:gen9:ppn=2 -d .`

`sycl-ls`

Luego se busca el archivo binario para realizar el análisis:

`cd /home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src`

`ls`

Se realiza el análisis GPU Hostpots con el VTune:

`vtune -collect gpu-hotspots -- /home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody`

En otra ventana del MobaXterm se descarga los resultados:

`scp -r devcloud:/home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody/r000gh .`

Los hostpots con GPU del programa es el siguiente:

GSimulation::Start(void)::{lambda(sycl::_V1::hander&)#1} 

Se presenta los siguientes screenshots que muestran los resultados:

![This is an image](/Desarrollo/Imagen7.png)

Descripción: El tiempo transcurrido del análisis es 1.891 segundo, de ese tiempo 0.354 segundos se utilizaron el GPU. El límite de ancho de banda GPU L3 es el 16,6% del valor pico. El muestreador ocupado es del 0.0% del valor pico. La utilización del procesador de punto flotante (FPU) es de 33.5% del tiempo transcurrido con GPU ocupado.

![This is an image](/Desarrollo/Imagen8.png)

Descripción: Se presenta un histograma de utilización del ancho de banda indicando el ancho de banda de la memoria del GPU

![This is an image](/Desarrollo/Imagen9.png)

Descripción: Información de los dispositivos utilizados en la ejecución. Además, se detalla del tiempo y peso en MB de la ejecución del programa

![This is an image](/Desarrollo/Imagen10.png)

Descripción: En el hotspot seleccionado, se utiliza el GPU para la ejecución. La unidad de ejecución especialmente de punto flotante (FPU) se utiliza el 90,6%.

## 5.	Realizar un análisis Roofline con Advisor [4 puntos]
- Indicar los hotspots del programa
- Proporcionar screenshot(s) de los resultados
  - Por cada screenshot, añadir una breve descripción
-	Indicar potenciales soluciones para optimizar la ejecución del programa

Primero se ejecuta los siguientes comandos para elegir un nodo y revizar si posee gpu:

`ssh devcloud`

`qsub -I -l nodes=1:gen9:ppn=2 -d .`

`sycl-ls`

Luego se busca el archivo binario para realizar el análisis:

`cd /home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src`

`ls`

Se realiza el análisis Roofline con el Advisor:

`advisor --collect=roofline --project-dir=./advi_results -- /home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody`

En otra ventana del MobaXterm se descarga los resultados:

`scp -r devcloud:/home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/advi_results .`

![This is an image](/Desarrollo/Imagen11.png)

Descripción: También se descarga los siguientes archivos ya que en análisis se necesita, además del archivo binario:

El archivo GSimulation.cpp:

`scp -r devcloud:/home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src/GSimulation.cpp .`

El archivo main.cpp:

`scp -r devcloud:/home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/src/main.cpp .`

El archivo nbody:

`scp -r devcloud:/home/u185963/Examen_Final/oneAPI-samples/DirectProgramming/C++SYCL/N-BodyMethods/Nbody/build/src/nbody .`

![This is an image](/Desarrollo/Imagen12.png)

Descripción: Los archivos se copia en la carpeta advi_results para que el advisor lo utilice en el analisis

![This is an image](/Desarrollo/Imagen13.png)

Descripción: se muestra las métricas obtenidas desde el cpu. Sus métricas principales son los siguientes: CPU Time con 1.65 segundos, Time in 1 Vectorized Loop con 0.06 s y el Time in sacalar code 1.59s 

![This is an image](/Desarrollo/Imagen14.png)

Descripción: Se muestra el análisis roofline, el cual se presenta los límites de memoria y de hardware. En la grafíca se observa un punto de intersección, el cual ayuda a determinar la forma que se debe mejorar. La eficiencia dependerá del área donde se encuentre las secciones del algoritmo. Si el punto se encuentra a la derecha se necesita hacer más operaciones. Y si se encuentra a la izquierda se debe retener datos como de la memoria cache.

![This is an image](/Desarrollo/Imagen15.png)

Descripción: se encuentra un punto verde, el cual es una sección poco usada con un tiempo de ejecución de 0.012 s

![This is an image](/Desarrollo/Imagen16.png)

Descripción: se encuentra un punto verde, el cual es una sección poco usada con un tiempo de ejecución de 0.008 s

Conclusión: En el gráfico no presenta alguna sección potencial que se pueda mejorar ya que no posee puntos rojos.

![This is an image](/Desarrollo/Imagen17.png)

Descripción: El hotspot en el programa es la línea 86, en la función void GSimulation::Start(), este le toma 1.650 segundos

![This is an image](/Desarrollo/Imagen18.png)

Descripción: En el main.cpp, se toma 1.65 segundos para ejecutar la función main. Cabe resaltar que esta función su tiempo esta definida por la función anteriormente mencionada.
