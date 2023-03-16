# Examen Final
## 0.	Clonar el repositorio oneAPI-samples [1 punto]
En la pagina se copie el enlace

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
