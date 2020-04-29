# Polygon: Poligono convexo, Delaunay y Triangulacion minima



# ANÁLISIS Y DISEÑO DE ALGORITMOS II

### Polígono convexo, Triangulación de Delaunay y mínimo costo de triangulación en polígonos

## De Fiore Ignacio Fermin

##### 19/12/2018


### INTRODUCCIÓN

En el siguiente informe se muestra el desarrollo y la resolución de los trabajos asignados
para el final. En el mismo, se analiza lo producido, comentando y explicando las
soluciones dadas en forma detallada.

### TEMAS ASIGNADOS

```
● Cálculo del polígono convexo para una nube de puntos dados.
● Triangulación de Delaunay.
● Costo mínimo de triangulación de polígonos.
```
### DESARROLLO

Inicialmente, se requiere el ingreso de una serie de puntos que forman una “nube”. Así,
se procesan los datos para el estudio y la aplicación de los temas nombrados.

### FUNCIONAMIENTO

Para el correcto funcionamiento del programa se deberán cargar las coordenadas de los
puntos que forman la nube en el archivo points.txt de la manera:
1,
4,
...
14,
siendo el valor del lado izquierdo de la coma la coordenada en X y el del lado derecho la
coordenada en Y.


### PROCESAMIENTO

### 1. Cálculo del polígono convexo para una nube de puntos dados:

Como segunda aplicación, dada la nube de puntos
nombrada anteriormente, se calcula el mínimo
polígono convexo donde cada punto es interior al
polígono o pertenece a uno de sus lados. Entre los
distintos algoritmos para resolver este problema,
vamos a citar dos basados en barrido rotacional. Estos
procesan vértices en el orden de los ángulos polares
que forma con referencia a un vértice, por ejemplo,
Algoritmo de Jarvis con una complejidad de O(n h)
siendo h el número de vértices en el menor polígono.
Para este trabajo en particular se utilizó el ​Algoritmo de Graham con una complejidad
temporal de O(n log n).
Seguimiento del algoritmo:
● Encontrar el punto con menor coordenada en el eje y, en caso de que haya más de
uno con el mismo valor en y, nos quedaremos con el punto con la menor
coordenada en x. Este punto será P0 y será nuestro primer punto de salida.
● Para los n-1 puntos restantes los ordenaremos según su ángulo polar en sentido
contrario a las agujas del reloj alrededor de punto P0, en caso de que dos puntos
tenga mismo ángulo polar se pondrá primero el punto más cercano.
Para el ordenamiento utilizaremos una función qsort a la cual le enviaremos los
n-1 puntos que restaban y una función “compare” para que realice el
ordenamiento según el criterio requerido, en este caso, “compare” obtiene la
orientación para tres puntos dados (p0, q, r), donde devolverá  0  si los puntos son
colineales (los tres caen en una misma recta),  1  o  2  dependiendo de la orientación
resultante.
● Luego de ordenar los puntos, se busca si hay puntos que comparten el mismo
ángulo. En caso de encontrar dos o más , eliminaremos todos excepto el más
lejano a P0, ya que en el ordenamiento anterior el criterio era mantener el punto
más lejano al final cuando dos puntos tenían un mismo ángulo.


```
● Después del paso 3, verificar que el nuevo arreglo de puntos tenga como mínimo  3 
puntos, sino no puedo generar un polígono. En caso de tener menos de 3,
devuelvo una pila vacía para luego en el main devolver el error.
● Si había al menos tres puntos, agregó a una pila los primeros tres.
● Para los restantes m-3 puntos, uno por uno, mientra que la orientación de los
siguientes tres puntos no sea contraria al giro de las agujas del reloj, continuó
removiendo.
● Devuelvo la pila que contiene los puntos que forman el polígono convexo.
```
### 2. Triangulación de Delaunay:

Concepto creado por el matemático ruso Boris Nikolaevich Delone en el año 1934, es una
red de triángulos conexa y convexa que cumple la condición de Delaunay. Esta dice que
la circunferencia circunscrita de cada triángulo que posea la red no debe contener
ningún vértice de otro triángulo en su interior, aunque se permite que se encuentren
situados en la misma circunferencia. Ejemplificando:
El vértice D, se encuentra dentro de la circunferencia circunscrita de los vértices A,B y C,
por lo cual no cumple con la condición de Delaunay.
En este otro caso, el vértice D se encuentra fuera de la circunferencia por lo que se
cumple con la condición de Delaunay.


Este concepto será aplicado entonces para obtener una triangulación sobre la nube de
puntos dada inicialmente. Como retorno, mostrará una serie de triángulos formados por
tres puntos del plano, que cumplirán con la condición de Delaunay.
Si bien la frontera externa de esta triangulación formará la envolvente convexa del
conjunto de puntos, más adelante se explicará un algoritmo para encontrar la misma.
Existen diferentes estrategias para abordar el cálculo de la triangulación de Delaunay,

#### como por ejemplo la utilización de un ​ algoritmo de fuerza bruta ​, con un orden O(n​^4 ​),

inviable salvo que el conjunto de entradas sea de unos pocos puntos. ​ _Algoritmo de giro de_

#### aristas ​, para triangulaciones en dos dimensiones y con una complejidad de O(n​^2 ​).

Entre otras implementaciones, también se encuentra un ​ _Algoritmo de barrido_ y un

#### algoritmo recursivo que utiliza el principio conocido de​ Divide y Vencerás ​.

En este trabajo particularmente se utilizó y se explicará el ​Algoritmo de Bowyer-Watson​,
método que resuelve la triangulación de Delaunay y fue creado por Adrian Bowyer y
David Watson en el año 1981. En cuanto a su tiempo de ejecución, tiene un orden O(n​^2 ​)
en su peor caso.
Es un método incremental donde se añaden los vértices a una triangulación que se
corrige en cada paso para mantener la condición de Delaunay. Después de cada
inserción se eliminan aquellos triángulos cuyo circuncirculo contenga al punto recién
introducido, por lo que el orden de inserción tiene influencia en el tiempo de ejecución
del algoritmo.
A continuación se mostrará un pseudocódigo del mismo.


function BowyerWatson (pointList)
// pointList es una lista de coordenadas de los puntos de entrada
triangulation := super-triangle // Crea un triángulo suficientemente
grande como para contener todos los puntos de entrada
for each point in pointList do // Añade los puntos uno a uno
badTriangles := empty set
for each triangle in triangulation do // busca los triángulos que
van a dejar de ser válidos
if point is inside circumcircle of triangle
add triangle to badTriangles
for each triangle in badTriangles do // Elimina los triángulos de la
estructura, creando un hueco
remove triangle from triangulation
polygon := empty set
for each triangle in badTriangles do // Calcula la frontera del
hueco creado por bad_triangles
for each edge in triangle do
if edge is not shared by any other triangles in badTriangles
add edge to polygon
for each edge in polygon do // re-triangula el hueco usando el nuevo
punto.
newTri := form a triangle from edge to point
add newTri to triangulation
for each triangle in triangulation // Limpieza de los triángulos
externos a la lista de vértices.
if triangle contains a vertex from original super-triangle
remove triangle from triangulation
return triangulation


### 3. Cálculo del mínimo costo de triangulation para un polígono:

```
Para la resolución de esta parte del trabajo, trabajaremos con el polígono
resultante de la aplicación anterior y no con la nube de puntos utilizada en un
principio. El desarrollo y explicación de esta parte del trabajo ya fue explicada en
una primer entrega, por eso sé adjunta el link al informe del mismo.
Primer Informe - Triangulation Polígonos (costo mínimo)
```
### RESULTADOS

A lo largo del presente trabajo se estudiaron y se analizaron en detalle distintos
conceptos relacionados a polígonos. La implementación práctica de cada uno de los
algoritmos anteriormente mencionados brindó una mejor comprensión de los mismos y
de sus principales ideas.


### EJEMPLO EJECUCIÓN

Dado los puntos: 1,1 - 5,2 - 6,5 - 3,7 - 1,6 - 1,3 - 3,
Se calcula la triangulación de Delaunay para la nube de
puntos dada, la cual devuelve una serie de triángulos
conformados por tres puntos cada uno, que se muestran de
la forma:
Una vez completado este procedimiento se calcula,
para la nube de puntos, el mínimo polígono convexo
que incluye todos los puntos.
Para finalizar se calcula, teniendo en
cuenta el polígono convexo formado en
la aplicación anterior el costo de
triangulation mínima:
Dado que el programa no brinda una
interfaz gráfica donde se puedan ver
plasmados los puntos, sé recomienda utilizar
https://www.geogebra.org/graphing
para graficar la nube de puntos ingresados y
los triángulos dados por la triangulación de
Delaunay y así poder ver como queda la
figura.


