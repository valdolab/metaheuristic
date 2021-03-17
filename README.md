# Practica 2
**Profesora**: Dra. Yenny Villuendas Rey

**Escuelas**:
$^{1}$ Escuela Superior de Cómputo del IPN
$^{2}$ Centro de Investigación en Computación del IPN

**Equipo**: 
- Enya Quetzalli Gómez Rodríguez$^{1}$
- Luis Fernando Quezada Mata$^{2}$
- Osvaldo David Velazquez Gonzalez$^{2}$

[Ver en línea](https://hackmd.io/@metah/practica2)

<br>

<style tyle="text/css">
p {
text-align: justify;
}
</style>

### **1. Enuncie las ventajas y desventajas de la Ascensión de Colinas** ###

**Ventajas:**
* <div class=text-justify>Los métodos de ascensión de colinas nos permiten encontrar soluciones aceptables para problemas complejos que no pueden ser resueltos (o no fácilmente) por algoritmos tradicionales.</div>
* <div class=text-justify>Tiene la posibilidad de encontrar las mejores soluciones en un espacio razonable de estados, a un costo en tiempo relativamente bajo dependiendo de la técnica de ascensión utilizada.</div>
* <div class=text-justify>Toma ventajas en donde los algoritmos deterministas no tienen control del camino a tomar, ya que las heurísticas le proveen de la capacidad de decisión sobre el mejor rumbo a tomar, mientras que los algoritmos tradicionales, tienen que realizar una búsqueda completa o encontrar una gorma Greedy de resolver dicho problema, junto con su respectiva demostración.</div>
* <div class=text-justify>Pueden utilizar diferentes técnicas para mejorar las posibles soluciones en zonas donde la ascensión de colinas puede verse detenida.</div>
* <div class=text-justify> Proporciona múltiples ventajas contra los algoritmos tradicionales de búsqueda ciega como lo puede ser la optimización en tiempo, o la optimización en memoria. </div>

**Desventajas:**
* <div class=text-justify>Necesita de una función generadora de estados que permita moverse alrededor de los estados vecinos para lograr un correcto ascenso, lo cual puede llegar a ser complicado dado el origen del problema.</div>
* <div class=text-justify>Se necesita de una heurística evaluadora que pondere el valor de un estado respecto a un estado deseado, de modo tal que un gran peso para la efectividad de la ascensión de colinas termina siendo la heurística.</div>
* <div class=text-justify>No tiene un buen balance entre el costo en tiempo contra el resultado obtenido, ya que, para obtener un resultado correcto, surge la necesidad de explorar más estados vecinos al mejor estado actual, lo cual lo hará más tardado para encontrar soluciones totalmente correctas.</div>
* <div class=text-justify>Tiene problemas en zonas donde la función sobre la que se evalúa la solución no cambia su valor con los estados vecinos, y es necesario aplicar diferentes técnicas más allá del propio algoritmo para superarlas.</div>

<br>

### **2. Detalle el pseudocódigo del algoritmo Steepest-ascent hill-climbing (NAHC)** ###
```clike=
Estado estadoActual, estadoVecino, mejorEstadoVecino.
Real puntuacionActual, puntuacionVecino, mejorPuntuacionVecino.
Iguala estadoActual a generarEstadoAleatorio()
Iguala puntuacionActual a evaluarEstado(estadoActual)
Repite:
	Iguala mejorPuntuaciónVecino a NULO
	Para cada operación en OperacionesPosibles, haz:
		Iguala estadoVecino a aplicarOperador(operación, estadoActual)
		Iguala puntuaciónVecino a evaluarEstado(estadoVecino)
		Si mejorPuntuaciónVecino es igual a NULO, entonces:
			Iguala mejorPuntuaciónVecino a puntuaciónVecino
			Iguala mejorEstadoVecino a estadoVecino
		Si puntuaciónVecino es mejor a mejorPuntuaciónVecino, entonces:
			Iguala mejorPuntuaciónVecino a puntuaciónVecino
			Iguala mejorEstadoVecino a estadoVecino

	Si mejorPuntuaciónVecino es mejor a puntuaciónActual, entonces:
		Iguala estadoActual a mejorEstadoVecino
		Iguala puntuacionActual a mejorPuntuaciónVecino
	De lo contrario:
		Regresa estadoActual.
```

### **3. Detalle el pseudocódigo del algoritmo Next-ascent hill-climbing (NAHC)** ###

**Algoritmo Next-ascent hill-climbing (NAHC):**
1. <div class=text-justify>Elegir una cadena de manera aleatoria, llamada “cima de la colina actual”.</div>
2. <div class=text-justify>Mutar bits individuales en la cadena “cima de la colina actual” de izquierda a derecha y guardar la función objetivo de las cadenas resultantes.</div>
3. <div class=text-justify>Si se encuentra algún aumento en la función objetivo de la cadena mutada, entonces se establece esa cadena mutada como la “cima de la colina actual” sin evaluar más mutaciones de bits individuales de la cadena original.</div>
4. <div class=text-justify>Ir al paso 2 con la nueva “cima de la colina actual”, pero continuando la mutación de la nueva cadena comenzando después de la posición del bit en la que se encontró el aumento de la función objetivo anterior.</div>
5. <div class=text-justify>Si no se encuentra algún aumento en la función objetivo, entonces guardar la “cima de la colina actual” e ir al paso 1.</div>
6. <div class=text-justify>Cuando la función objetivo se haya evaluado un número determinado de veces, retornar la cima de la colina más alta que se encontró.</div>

**Pseudocódigo del algoritmo Next-ascent hill-climbing (NAHC):**

\begin{align}
&\text{cimaactual } \gets\text{solución aleatoria} 
\\
&\text{evaluar(x)} 
\\
&\text{prev } \gets\text{null} 
\\
&\textbf{repeat:}
\\
&| \quad\text{hilltop } \gets\text{true}
\\
&| \quad\textbf{for } \text{ s } \in\ vecinos(x)  \textbf{ do}
\\
&| \quad| \quad \text{evaluar(x) }
\\
&| \quad| \quad \textbf{if } \text{s.fitness} > \text{x.fitness} \textbf{ then }
\\
&| \quad| \quad | \quad \text{hilltop} \gets{false}.
\\
&| \quad| \quad | \quad \text{prev} \gets{x}.
\\
&| \quad| \quad | \quad \text{x} \gets{s}.
\\
&| \quad| \quad | \quad \textbf{break}
\\
&| \quad| \quad \textbf{end}
\\
&| \quad \textbf{end}
\\
&\textbf{hasta } \text{ hilltop}
\\
& \textbf{return } \text{x}
\end{align}

<br>

### **4. Detalle el pseudocódigo del algoritmo Random mutation hill-climbing (RMHC)** ###

**Algoritmo Random mutation hill-climbing (RMHC):**

1. <div class=text-justify>Elegir una cadena de bits aleatoriamente. Llamar a esta cadena la “mejor evaluada”.</div>
2. <div class=text-justify>Elegir una posición de la cadena de manera aleatoria y mutar (negar) el bit en esa posición.</div>
3. <div class=text-justify>Si la cadena mutada está mejor (o igualmente) evaluada que la cadena original (por una función objetivo), entonces llamamos a la cadena mutada la cadena “mejor evaluada”.</div>
4. Ir al paso 2.
5. <div class=text-justify>Cuando la función objetivo se haya evaluada un número determinado de veces, regresar la cadena “mejor evaluada”.</div>

**Pseudocódigo del algoritmo Random mutation hill-climbing (RMHC):**

En el primer paso se elige aleatoriamente una cadena de bits:
\begin{equation}
\bar{x}\in \left\lbrace 0,1 \right\rbrace^n,
\end{equation} y a esta cadena la llamamos la \`\`mejor evaluada'' (best), es decir
$$ \text{best}\ \gets\bar{x}.$$

Después, se elige aleatoriamente una componente $x_\ell$ de $\bar{x}$ y se define la cadena mutada $\bar{y}$ como:
\begin{align}
y_i&=x_i \:\forall  i\neq\ell, \\
y_{\ell}&= \neg x_{\ell}.
\end{align}

Sea $f$ la función objetivo (la altura de la colina):

$$\textbf{If} \quad f(\bar{y})\geq f(\bar{x}) \quad \textbf{then} \quad \text{best} \gets\bar{y}.$$

Por último, la función objetivo se evalúa un número determinado de veces $k$ antes de regresar el valor de best. De manera que el pseudocódigo queda como:

\begin{align}
&\textbf{Input: } \bar{x}, \: f, \: k
\\
&\textbf{Output: } \text{best}
\\
&best \gets \bar{x}
\\
&\textbf{For } i=1,\dots,k \textbf{ do}
\\
&| \quad \text{Elegir aleatoriamente una componente } x_\ell \text{ de } \bar{x}.
\\
&| \quad \text{Definir } \bar{y} \text{ tal que } y_i=x_i \: \forall i\neq\ell, \: y_\ell=\lnot\ x_\ell.
\\
&| \quad \textbf{If } f(\bar{y})\geq f(\bar{x}) \textbf{ then }
\\
&| \quad | \quad \text{best} \gets\bar{y}.
\\
&| \quad \textbf{end}
\\
& \textbf{end}
\\
& \textbf{return } \text{best}
\end{align}

<br>

### **5. Mencione aplicaciones de los algoritmos de Ascensión de Colinas** ###

Los algoritmos de ascensión de colinas son algoritmos iterativos que inician en un estado arbitrario e intentan mejorar éste hasta encontrar un estado al que no se le puedan hacer más mejoras. Debido a esto, los algoritmos de ascensión de colinas pueden aplicarse a cualquier problema en el que la descripción del estado contenga toda la información suficiente para encontrar una solución; es decir, problemas en los que no es necesario almacenar el camino seguido. Ejemplos de problemas que pueden resolverse con los algoritmos de ascensión de colinas son: el problema de las ocho reinas, el problema de la mochila y el problema del viajero.

Podemos encontrar algunas aplicaciones de los algoritmos de ascensión de colinas en modelos de aprendizaje inductivo <b>[1]</b> y en la coordinación de pequeños equipos de robots <b>[2]</b>.

<br>

### **6. Realice la modelación matemática necesaria para la solución, mediante RMHC, del problema de la mochila** ### 

En el problema de la mochila se tienen $n$ distintos items y una cantidad $q_{i} \geq 1$ de cada item $i$. Estos items tienen dos características: peso y beneficio, denotados respectivamente por $w_{i}$ y $b_{i}$, los cuales se consideran números reales no negativos. La mochila soporta un peso máximo $W \geq 0$, por lo que el problema consiste en meter items a la mochila de tal forma que se maximice el beneficio total $B$ sin que el peso total exceda $W$. Es decir, se busca maximizar la función

\begin{align}
B(\bar{x}) = \sum_{i=1}^{n} x_{i}b_{i},
\end{align}

sujetos a las restricciones

\begin{align}
&\sum_{i=1}^{n} x_{i}w_{i} \leq W,
\\
&0 \leq x_{1} \leq q_{i},
\end{align}

en donde $\bar{x}=(x_{1},\dots,x_{n})$ representa la cantidad de cada item a introducir en la mochila.

Para resolver el problema usando el algoritmo RMHC, definimos el espacio de estados como el conjunto

$$
\left\lbrace \bar{x} \in \mathbb{N}^{n} \: \middle| \: 0 \leq x_{i} \leq q_{i}, \: \sum_{i=1}^{n} x_{i}w_{i} \leq W, \right\rbrace,
$$

en donde el estado inicial será cualquier elemento elegido aleatoriamente de este conjunto.

Los operadores, es decir las acciones posibles, están dadas por las mutaciones de los estados; las cuales, en el algoritmo RMHC, son cambios aleatorios en una de las posiciones de $\bar{x}$. De esta forma, los operadores pueden modelarse como
$$
\hat{O}_{i}(x_{1},\dots,x_{n}) = (x_{1}, \dots,y_{i},\dots x_{n}),
$$

donde $y_{i}$ es un número aleatorio distinto de $x_{i}$ y el vector $(x_{1}, \dots,y_{i},\dots x_{n})$ satisface las restricciones del espacio de estados.

La función objetivo en este caso está dada por la función beneficio $B$, que es justo la que se busca maximizar, evaluando en cada paso de la iteración si se cumple que $B(x_{1}, \dots,y_{i},\dots x_{n}) \geq B(x_{1},\dots,x_{n})$.

<br>

### 7. Dado el problema de la mochila, proponga las estructuras de datos necesarias para su implementación.

- Se definirá el *estado* como un arreglo de tamaño $n$, donde $n$ es el número de objetos diferentes disponibles para agregar a la mochila, y donde $estado_i$ es el número de veces que el objeto $i$ ha sido agregado a la mochila.
- Se definirá la matriz *objetos_disponibles* como una matriz de tamaño $3 \times n$, donde las filas corresponderán al *peso*, *beneficio* y *cantidad disponible* respectivamente. 

### 8. Código
Utilizaremos la librería *random* para acceder a número aleatorios discretos, y la librería *copy* se utiliza para crear copias fieles y no editar los estados que se mutarán.
```python=1
import random
import copy
```


En este bloque de código, estableceremos los valores iniciales sobre los cuales trabajará el algoritmo. 
```python=4
#Datos de entrada
max_number_of_elements_in_backpack = 10
max_weight_supported_by_backpack = 700
available_objects = [
  [  9,  9, 50,153,230,250],  #Weight
  [150,150,160,200,355,591],  #Benefit
  [  1,  1,  1,  1,  1,  1]   #Max Items
]
```


Esta función retornará la suma total de los pesos que se encuentran en la mochila de un determinado estado.
```python=13
def backpackWeight(items): 
  weight = 0
  for i in range(0,len(items)):
    weight += items[i] * available_objects[0][i]
  return weight
```


Esta función retornará la suma total del beneficio de los objetos que se encuentran en la mochila de un determinado estado.
```python=19
def backpackBenefit(items):
  benefit = 0
  for i in range(0,len(items)):
    benefit += items[i] * available_objects[1][i]
  return benefit
```


Esta función regresará la puntuación a maximizar a nuestro algoritmo. en este caso, únicamente regresará el beneficio del estado.
```python=25
def getStateScore(state):
  return backpackBenefit(state)
```


Esta función valida que un estado sea válido verificando que la suma de sus pesos sea menor o igual a la suma total soportada por la mochila y que el número total de elementos sea menor o igual al máximo de elementos soportado por la mochila. 
```python=28
def isValidState(state):
  if backpackWeight(state) <= max_weight_supported_by_backpack 
      and sum(state) <= max_number_of_elements_in_backpack:
    return True
  return False
```


En esta función generamos un estado aleatorio que sea válido según las restriccines del problema. 
```python=33
def randomState():
  random.seed(a=None)
  state = [0]*len(available_objects[0])
  while True:
    for i in range(0, len(state)):
      state[i] = random.randint(0,available_objects[2][i])
    if isValidState(state):
      break
  return state
```


En esta función realizamos una mutación del estado proporcionado que sea válida según las restricciones del problema. 
```python=43
def mutateState(state):
  while True:
    state_copy = copy.copy(state)
    random_position = random.randint(0, len(state)-1)
    random_mutation = random.randint(0,available_objects[2][random_position])
    state_copy[random_position] = random_mutation
    if isValidState(state_copy) and state != state_copy:
      break
  return state_copy
```


Algoritmo de **Random mutation hill-climbing**
```python=1
def rmhc(max_iterations):
  state = randomState()
  state_score = getStateScore(state)
  print("Starting state ->", state)

  for i in range(0, max_iterations):
    print("Iteration #", i, " ", end='')
    mutation = mutateState(copy.copy(state))
    mutation_score = getStateScore(mutation)
    print("State -> ", state, "(", state_score, " score)", "\tMutation ->", mutation, "(", mutation_score, " score)", end='')

    if mutation_score > state_score:
      print(" !better", end='')
      state = mutation
      state_score = mutation_score
      
    print()
  return state
```


Finalmente, en la salida podemos observar que el estado únicamente se actualiza cuando encontramos una mutación aleatoria sobre los vecinos que fue mejor que el estado actual. De modo que al finalizar las iteraciones logramos obtener un resultado que tiende hacia la cima de la colina. 
**Output:**
```
Starting state -> [0, 0, 1, 1, 1, 1]
Iteration # 0  State ->  [0, 0, 1, 1, 1, 1] ( 1306  score)
	Mutation -> [0, 0, 1, 1, 0, 1] ( 951  score)
Iteration # 1  State ->  [0, 0, 1, 1, 1, 1] ( 1306  score)
	Mutation -> [1, 0, 1, 1, 1, 1] ( 1456  score) !better
Iteration # 2  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 1, 0] ( 865  score)
Iteration # 3  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 1, 0] ( 865  score)
Iteration # 4  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 0, 1, 1, 1] ( 1296  score)
Iteration # 5  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 0, 1, 1, 1] ( 1296  score)
Iteration # 6  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 0, 1] ( 1101  score)
Iteration # 7  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [0, 0, 1, 1, 1, 1] ( 1306  score)
Iteration # 8  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 0, 1, 1] ( 1256  score)
Iteration # 9  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 1, 0] ( 865  score)
Iteration # 10  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [0, 0, 1, 1, 1, 1] ( 1306  score)
Iteration # 11  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 0, 1] ( 1101  score)
Iteration # 12  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 1, 0] ( 865  score)
Iteration # 13  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 0, 1, 1, 1] ( 1296  score)
Iteration # 14  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 0, 1] ( 1101  score)
Iteration # 15  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 0, 1, 1, 1] ( 1296  score)
Iteration # 16  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [0, 0, 1, 1, 1, 1] ( 1306  score)
Iteration # 17  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 0, 1] ( 1101  score)
Iteration # 18  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 1, 1, 0, 1] ( 1101  score)
Iteration # 19  State ->  [1, 0, 1, 1, 1, 1] ( 1456  score)
	Mutation -> [1, 0, 0, 1, 1, 1] ( 1296  score)
[1, 0, 1, 1, 1, 1]
```


### **Referencias** ###
**[1]** Cohen, W., Greiner, R., Schuurmans, D. 1994. Probabilistic Hill-Climbing. Computational Learning Theory and Natural Learning Systems, Vol. II. Edited by Hanson, S., Petsche, T., Rivest R., Kearns M. MITCogNet, Boston:1994.

**[2]** Gerkey, B; Thrun, S., Gordon, G. 2005. Parallel Stochastic Hillclimbing with Small Teams. Multi-Robot Systems: From Swarms to Intelligent Automata, Volume III, 65–77.






