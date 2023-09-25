# Práctica 6 (Recocido Simulado)

### **1. Enuncie las ventajas y desventajas del Recocido Simulado (SA)** ###

**Ventajas [1]:**
* <div class=text-justify>El recocido simulado puede tratar con muchas restricciones, como lo son: problemas altamente no lineales, datos caóticos y ruidos. Además de ser una técnica robusta y general.</div>
* <div class=text-justify>Una de sus principales ventajas sobre otros algoritmos de búsqueda son su flexibilidad y su fuerte capacidad para acercarse al óptimo global.</div>
* <div class=text-justify>Los algoritmo SA son bastantes versátil ya que no depende de ninguna propiedad restrictiva del modelo.</div>
* <div class=text-justify>Se puede adaptar fácilmente a las restricciones debido a que los valores de restricción se pueden agregar fácilmente a los valores evaluados en cada iteración.</div>


**Desventajas [1]:**
* <div class=text-justify>La velocidad de convergencia es muy lenta ademas de que su implementación paralela puede ser muy complicada.</div>
* <div class=text-justify>Existe una clara compensación entre la calidad de las soluciones y el tiempo necesario para calcularlas.</div>
* <div class=text-justify>Para lograr una implementación de alta calidad, es necesario un laborioso ajuste de parámetros, lo cual puede llegar a ser complicado. </div>
* <div class=text-justify>La precisión de los números utilizados en la implementación puede tener un efecto significativo sobre la calidad del resultado.</div>

<br>

### **2. Detalle el pseudocódigo del algoritmo de Recocido Simulado** ###

El algoritmo de recocido simulado se puede ver como una iteración de algoritmos de Metrópolis. Si se baja la temperatura suficientemente lento se puede alcanzar el equilibrio térmico en cada temperatura. Esto se hace mediante la generación de varias transiciones en cada temperatura. Se puede demostrar que bajando suficientemente lento el parámetro asociado a la temperatura y generando suficientes transiciones en cada temperatura se puede alcanzar la configuración óptima.

La probabilidad de aceptación se expresa como:

$P_c\{acepta, j\} =$

$$
\begin{cases}
 & \text{ 1 } \quad\quad\quad\quad\quad\quad \quad si f(j) \leq f(i) \\ 
 & \ exp(\frac{f(i)-f(j)}{c_k}) \quad\quad si f(j) >  f(i)
\end{cases}
$$

Donde $$c \in R^+$$ denota al parámetro de control.

$$
\begin{align}
&k=0, i=i_{start}
\\
&\textbf{repite:}
\\
&\quad \textbf{para } l=1,\dots,L_k \textbf{ haz:}
\\
& \quad \quad \text{genera } j \in S_j
\\
& \quad \quad  \textbf{si } f(j) \leq f(i) \textbf{ entonces: }
\\
& \quad \quad \quad i=j
\\
& \quad \quad \textbf{si no, si } exp(\frac{f(i)-f(j)}{c_k}) > aleatorio[0,1] \textbf{ entonces: }
\\
& \quad \quad \quad i=j
\\
& \quad k = k + 1
\\
& \quad \text{actualiza } L_k \text{ y } c_k
\\
& \quad \textbf{end}
\\
& \textbf{hasta } \text{criterio de paro}
\end{align}
$$

La velocidad de convergencia del algoritmo está determinada por $L_k$ y $c_k$.

### **3. Compárelo con el algoritmo de RMHC** ###

Partiendo de la elección inicial de estado, ambos algoritmos eligen uno de manera aleatoria.

Con respecto a la exploración del espacio de estados, en el caso de el algoritmo RMHC, el estado es sujeto a una mutación aleatoria; mientras que en el algortimo SA, el estado tiene asociada una vecindad, de la cual se elige aleatoriamente un estado diferente. Es posible, sin embargo, que la vecindad de un estado en el algoritmo SA coincida con el conjunto de las posibles mutaciones generadas por ese mismo estado en el algoritmo RMHC.

En el algoritmo RMHC se eligen únicamente las mutaciones con menor costo (o mayor beneficio) que el estado original. Por otra parte, en el algoritmo SA, si bien estos estados también se eligen en caso de ser seleccionados, los estados con mayor costo (o menor beneficio) también pueden ser seleccionados con cierta probabilidad, la cual irá disminuyendo conforme la temperatura lo haga. Cabe destacar que no existe un parámetro análogo a la temperatura del algoritmo SA en el algoritmo RMHC.

Por último, el algoritmo SA tiene la característica de encontrar óptimos globales; mientras que el algoritmo RMHC, al seleccionar únicamente mejores opciones en cada evaluación, puede quedarse atascado en un ótimo local.



<br>

### **4. Mencione aplicaciones recientes (últimos 3 años) de los algoritmos de Recocido Simulado** ###


Los algoritmos de Recocido Simulado (SA) son algoritmos iterativos que inician en un estado arbitrario e intentan mejorar éste hasta encontrar un estado al que no se le puedan hacer más mejoras. Debido a está caracteristica, los algoritmos de Recocido Simulado pueden aplicarse a diversos problemas en el que la descripción del estado contenga toda la información suficiente para encontrar una solución; es decir, problemas en los que no es necesario almacenar el camino seguido. Algunos ejemplos de problemas clasicos que pueden resolverse con los algoritmos de Recocido Simulado son: el problema de las ocho reinas, el problema de la mochila y el problema del viajero.

Determinar el minimo local de un funcion es uno de los principales problemas en diferentes areas, como lo son: biología, física, matemáticas aplicadas, finanzas, química, computación, ingeniería, y entre muchas otras más. Es por ello que su aplicación se encuentre muy presente dentro del estado del arte. Por ejemplo, en la areá de reconocimiento de patrones, aplicándolo para mejorar un método de selección de caracisticas en grandes conjuntos de datos   **[2]**.

Podemos encontrar algunas otras aplicaciones de los algoritmos de Recocido Simulado en la ingeniería en **[3,4,5]** y aplicaciones en el sector financiero, como lo es la predicción del precio de los bienes raices en el mercado inmobilario **[6]**.

<br>

### **5. Realice la modelación matemática necesaria para la solución, mediante SA, del Problema de la mochila (Knapsack problem). Recuerde que la modelación matemática incluye: definición de los estados inicial y final, definición del test objetivo, y definición de las acciones posibles (operadores).** ###

En el problema de la mochila se tienen $n$ distintos items $i \in \left\lbrace 1,\dots,n \right\rbrace$. Estos items tienen dos características: peso y beneficio, denotados respectivamente por $w_{i}$ y $b_{i}$, los cuales se consideran números reales no negativos. La mochila soporta un peso máximo $W \geq 0$, por lo que el problema consiste en meter items a la mochila de tal forma que se maximice el beneficio total $B$ sin que el peso total exceda $W$. Es decir, se busca maximizar la función

\begin{align}
B(\bar{x}) = \sum_{i=1}^{n} x_{i}b_{i},
\end{align}

sujetos a las restricciones

\begin{align}
\sum_{i=1}^{n} x_{i}w_{i} \leq W,
\end{align}

en donde $x_{i} \in \left\lbrace 0,1 \right\rbrace$ representa si el item $i$ está dentro de la mochila ($x_{i} = 1$) o no ($x_{i} = 0$).

Para resolver el problema usando el algoritmo SA, definimos el espacio de estados como el conjunto

\begin{equation}
S = \left\lbrace \bar{x} \in \left\lbrace 0,1 \right\rbrace^{n} \: \middle| \: \sum_{i=1}^{n} x_{i}w_{i} \leq W, \right\rbrace,
\end{equation}

en donde el estado inicial será cualquier elemento elegido aleatoriamente de este conjunto.

El algoritmo SA requiere definir una vecindad para cada estado en $S$. En este caso, dado un estado $\bar{x} \in S$, su vecindad será un conjunto $V_{\bar{x}} \subseteq S$ que contenga todos los estados que difieran de $\bar{x}$ en una sola de sus entradas. Formalmente:

\begin{equation}
V_{\bar{x}} = \left\lbrace \bar{y} \in S \: \middle| \: d_{h}(\bar{x},\bar{y}) = 1  \right\rbrace
\end{equation}

en donde $d_{h}(\bar{x},\bar{y})$ denota la distancia de Hamming entre los estados $\bar{x}$ y $\bar{y}$.

Además, dado un estado $\bar{x}$ con una vecindad $V_{\bar{x}}$, cada elemento $\bar{y}$ de la vecidad tiene una probabilidad asociada $P_{\bar{x}}(\bar{y})$ dada por:

\begin{equation}
P_{\bar{x}}(\bar{y}) =  \text{exp}\left( \frac{B(\bar{y})-B(\bar{x})}{T} \right)
\end{equation}

donde $T$ representa la temperatura.

Entonces, en el algoritmo SA, los operadores o acciones posibles sobre un estado $\bar{x}$, están dados por la acción de elegir aleatoriamente un elemento $\bar{y}$ de la vecidad $V_{\bar{x}}$. Mientras que, por otra parte, el test objetivo consiste en evaluar $\delta(\bar{x},\bar{y}) = B(\bar{x})-B(\bar{y})$:

* Si $\delta(\bar{x},\bar{y}) \leq 0$, es decir que el beneficio del estado $\bar{y}$ sea mayor o igual que el del estado $\bar{x}$, entonces $\bar{y}$ reemplazará a $\bar{x}$ como el estado en turno.
* Si $\delta(\bar{x},\bar{y}) > 0$, es decir que el estado $\bar{y}$ tiene un menor beneficio que el estado $\bar{x}$, entonces $\bar{y}$ reemplazará a $\bar{x}$ con base en su probabilidad asociada $P_{\bar{x}}(\bar{y})$.

Otra acción posible (operador) es la disminuir la temperatura hasta alcanzar $T=0$ (o un valor muy cercano). Esto hace que la probabilidad de aceptar estados con peor beneficio disminuya conforme el algoritmo avanza.

El estado final en el algoritmo SA será el óptimo global, es decir, el estado de $S$ con el mayor beneficio.

<br>

### **6. Realice una corrida manual del algoritmo de Recocido Simulado sobre el problema anterior. Defina para ello un esquema de recocido de su preferencia.** ### 




Para comenzar con el código necesitaremos importar 3 librerías, random, copy y math
```python=1
import random
import copy
import math
```

Escribimos una función que nos devuelva el beneficio en la mochila
```python=5
def backpackBenefit(items):
  benefit = 0
  for i in range(0,len(items)):
    benefit += items[i] * available_objects[0][i]
  return benefit
```

Una que nos regrese el peso total en la mochila
```python=11
def backpackWeight(items): 
  weight = 0
  for i in range(0,len(items)):
    weight += items[i] * available_objects[1][i]
  return weight
```

Y otra que nos diga la cantidad total de items en ella
```python=17
def backpackItems(items):
  return sum(items)
```

El estado se conformará el encapsulado de los 3 datos anteriores, esto para poder cacular la diferencia para los estados vecinos en lugar de hacer un calculo de tiempo lineal
```python=20
def getStateScore(state):
  return (backpackBenefit(state), backpackWeight(state), backpackItems(state))
```

Como vemos, esta función regresa el nuevo encapsulado del score en tiempo constante para un estado vecino.
```python=23
def getDeltaScore(state, score, new_state, position):
  delta = new_state[position]-state[position]
  new_benefit = score[0] + delta*available_objects[0][position]
  new_weight = score[1] + delta*available_objects[1][position]
  new_items = score[2] + delta
  return (new_benefit, new_weight, new_items)
```

Is valid state de igual forma, ahora recibe el score y puede regresar su veredicto en tiempo constante
```python=30
def isValidState(state, score):
  if score[1] < max_weight_supported_by_backpack and score[2] < max_number_of_elements_in_backpack:
    return True
  return False
```

Random state se quedó igual con el objetivo de mantener la aleatoreidad del estado inicial
```python=35
def randomState():
  random.seed(a=None)
  state = [0]*len(available_objects[0])
  while True:
    for i in range(0, len(state)):
      state[i] = random.randint(0,available_objects[2][i])
    score = getStateScore(state)
    if isValidState(state, score):
      break
  return state, score
```

El vecino de igual forma en tiempo lineal
```python=46
def neighbour(state, score):
  state_copy = copy.copy(state)
  score_copy = copy.copy(score)
  while True:
    random_position = random.randint(0, len(state)-1)
    random_items = random.randint(1,available_objects[2][random_position])
    state_copy[random_position] = (state_copy[random_position]+random_items)%(available_objects[2][random_position]+1)
    score_copy = getDeltaScore(state, score, state_copy, random_position)
    if isValidState(state_copy, score_copy) and state[random_position] != state_copy[random_position]:
      break
    state_copy[random_position] = state[random_position]
    score_copy = copy.copy(score)
  return state_copy, score_copy
```

Y finalmente el algorimo.
```python=60
def rs(cooler, time_per_temperature, final_temperature, verbose=True, k=1.380649e-23):
  state, score = randomState()
  temperature = score[0]*0.4

  while temperature >= final_temperature:
    for i in range(0, time_per_temperature):
      new_state, new_score = neighbour(state,score)
      delta = score[0]-new_score[0]
      
      if delta < 0 or random.uniform(0,1) < math.exp(-delta/temperature):
        state,score = new_state, new_score
        
    temperature = cooler(temperature)
  return state
```
```python=78
cooler = lambda t : t*random.uniform(0.8,0.99)
```
```python=80
#Datos de entrada
max_number_of_elements_in_backpack = 10
max_weight_supported_by_backpack = 800
available_objects = [
  [150,150,160,200,355,591],  #Benefit
  [  9,  9, 50,153,230,250],  #Weight
  [  1,  1,  1,  1,  1,  1]   #Max Items
]
#State will be the number of each item in backpack
```
```python=90
rs(cooler, 10, 0.1)
```
