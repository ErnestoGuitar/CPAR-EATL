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

1. Se inicializa los valores de posición, velocidad, aceleración y peso
2. Luego
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

## 5.	Realizar un análisis Roofline con Advisor [4 puntos]
- Indicar los hotspots del programa
- Proporcionar screenshot(s) de los resultados
- Por cada screenshot, añadir una breve descripción
-	Indicar potenciales soluciones para optimizar la ejecución del programa
