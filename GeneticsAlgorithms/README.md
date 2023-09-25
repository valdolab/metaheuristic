### Solución de problemas mediante Algoritmos Genéticos


### **1. Enuncie las ventajas y desventajas de los Algoritmos Genéticos (GA).**

**Ventajas [1, 2]:**
* <div class=text-justify>Es fácilmente paralelizable, pueden explorar el espacio de soluciones en múltiples direcciones al mismo tiempo.</div>
* <div class=text-justify>Los algoritmos GA  realizan  un  amplia  exploración  en el intervalo de búsqueda factible, logrando espacar eficientemente de los óptimos locales y alcanzar el óptimo global.</div>
* <div class=text-justify>Se puede encontrar una solución sin un análisis profundo previo del problema.</div>
* <div class=text-justify>Los algortimos GA trabajan bien en problemas complejos y cambiantes, así como aquéllos en los que la función objetivo es discontinua, ruidosa, o que tiene muchos óptimos locales, ademas de poner soportar las optimizaciones multi-objetivo.</div>
* <div class=text-justify>Los algoritmos GA puede operar en varias representaciones.</div>

**Desventajas [1, 2]:**

* <div class=text-justify>La función objetvo es muy sensible para los algotimos GA, ya que si se elige mal o se define incorrectamente, puede que sea incapaz de encontrar una solución al problema. </div>
* <div class=text-justify>La elección de los parámentros (tamaño de la población, cruzamiento, selección de padres, mutación, entre otras más) de los algoritmos GA puede llegar a ser muy complejo. Una mala elección en los parámetros puede provocar un mal desempeño.</div>
* <div class=text-justify>Consumen mucho tiempo de ejecución y potencia de cómputo</div>
* <div class=text-justify>Existe el riesgo de encontrarse con una convergencia prematura; es decir, se puede reproducir abundantemente un individuo haciendo que merme la diversidad de la población demasiado pronto, provocando que converja hacia un óptimo local, el cual representa a ese individuo. </div>


<br>

### **2. Diga las diferencias entre fenotipo y genotipo.**

El fenotipo es una posible solución del problema, está posible solución puede tomar varias formas, como números reales o enteros. Por otro lado, el genotipo (tambien llamado cromosoma) esta constituido apartir del fenotipo, el cual es su representación (codigicación) de sus caracteristicas. El genotipo o cromosoma se representan generalmente como cadenas, donde cada componente representa un gen; es decir, una de las variables del problema a resolver. A continuación, se plasma un ejemplo. 

<div style = 'text-align:center;'>
<img  src="https://i.imgur.com/T62m4GR.png" width="400px"><br>
Figura 1. Descripción grafica del fenotipo, genotipo, cromosoma y gen.
</div>

<br>

### **3. Mencione 3 operadores de Selección de padres. Explique el funcionamiento de uno de ellos.**

* <div class=text-justify> Selección por torneo (TS).</div>
* <div class=text-justify> Selección proporcional (PS).</div>
* <div class=text-justify> Selección por ruleta.</div>

La **selección proporcional** o PS para abreviar, es un operador de selección basado en el fitness de cada individuo en la población. Éste consiste en ordenar a los individuos en función de su fitness y asociar una probabilidad de selección a cada uno dependiendo del orden, es decir, individuos con un fitness altos tendrán una probabilidad alta de ser seleccionados, mientras que individuos con un fitness bajo tendrán una probabilidad baja.

La implementación se realiza como sigue. Dados una población $\mathcal{P}$ y un individuo $i \in \mathcal{P}$, y denotando por $f_{j}$ al fitness de cualquier individuo $j \in \mathcal{P}$, la probabilidad de selección del individuo $i$ se define como:

$$
\begin{equation}
p_{i} = \frac{f_{i}}{\displaystyle\sum_{j \in \mathcal{P}} f_{j}}.
\end{equation}
$$

<br>

### **4. Mencione 3 operadores de Cruzamiento. Explique el funcionamiento de uno de ellos.**

* <div class=text-justify> Operador de cruce en dos puntos para representación binaria. </div>
*  Operador de cruce BLX-$\alpha$ para representación real.
* <div class=text-justify> Operador de cruce OX para representación de orden.</div>

El **operador de cruce OX** es utilizado  para representaciones de orden (como en el problema del viajero vendedor). Éste consiste en seleccionar aleatoriamente dos puntos de corte en un cromosoma; después, copiar la subsecuencia de genes entre estos dos puntos del cromosoma de uno los padres ($P_{1}$) al cromosoma del hijo ($c_{1}$); y por último, colocar los genes del segundo padre ($P_{2}$) después del segundo punto de corte del cromosoma de $c_{1}$, conservando estos el mismo orden que tenían en $P_{2}$ pero omitiendo aquellos que fueron copiados de $P_{1}$. Para obtener un segundo hijo ($c_{2}$) se cambian los roles de los padres y se aplica el mismo procedimiento con los mismos puntos de corte.

Considere el siguiente caso para ejemplificar el operador de cruce OX. Se seleccionan dos padres $P_{1}$ y $P_{2}$:

$$
\begin{align}
P_{1} = (5,4,3,6,1,2)
\\
P_{2} = (3,1,6,2,5,4)
\end{align}
$$

Se eligen aleatoriamente dos puntos de corte (en este caso entre $1$ y $5$), supónganse $3$ y $5$; entonces, se copia a $c_{1}$ la subsecuencia de genes de $P_{1}$ (marcada en rojo) entre estos dos puntos:

$$
\begin{align}
P_{1} = (5,4,3,\color{red}6,\color{red}1,2)
\\
c_{1} = (\_,\_,\_,\color{red}6,\color{red}1,\_)
\end{align}
$$

Por último, después del segundo punto de corte se colocan en $c_{1}$ los genes de $P_{2}$ (marcados en verde) en el mismo orden que aparecen en su cromosoma, omitiendo aquellos que fueron copiados de $P_{1}$ (marcados en rojo):

$$
\begin{align}
P_{2} = (\color{green}3,1,6,\color{green}2,\color{green}5,\color{green}4)
\\
c_{1} = (\color{green}2,\color{green}5,\color{green}4,\color{red}6,\color{red}1,\color{green}3)
\end{align}
$$

Para obtener un segundo hijo $c_{2}$ se repite el mismo procedimiento usando los mismos puntos de corte e intercambiando el rol de los padres, dando lugar en este caso a:

$$
\begin{align}
P_{2} = (3,1,6,\color{green}2,\color{green}5,4)
\\
P_{1} = (5,\color{red}4,\color{red}3,\color{red}6,\color{red}1,2)
\\
c_{2} = (\color{red}3,\color{red}6,\color{red}1,\color{green}2,\color{green}5,\color{red}4)
\end{align}
$$


<br>

### **5. Mencione 3 operadores de Mutación. Explique el funcionamiento de uno de ellos.**

* <div class=text-justify> Mutación de un gen para representación binaria.</div>
* <div class=text-justify> Mutación uniforme para representación real.</div>
* <div class=text-justify> Mutación SWAP para representación de orden.</div>

La **mutación de un gen** es un operador de mutación para representaciones binarias que consiste en seleccionar aleatoriamente un locus en el cromosoma del individuo en cuestión y negar el gen correspondiente.

Considere el siguiente caso para ejemplificar el operador de mutación de un gen. Supóngase que el cromosoma del individuo al que se le aplicará el operador de mutación es el siguiente:

$$
\begin{equation}
x = (1,1,0,1,0,0,1)
\end{equation}
$$

Se elige entonces un locus aleatorio (en este caso entre $1$ y $7$), supóngase $5$; entonces, se niega el gen en la posición $5$ del cromosoma (marcado en rojo):

$$
\begin{align}
x = (1,1,0,1,\color{red}0,0,1)
\\
x' = (1,1,0,1,\color{red}1,0,1)
\end{align}
$$

en donde $x'$ denota el cromosoma ya mutado.

<br>

### **6. Valore qué impacto tiene el tamaño de la población en la convergencia de un Algoritmo Genético.**

El tamaño de la población de un algoritmo genético es un parámetro sumamente importante, el cual puede afectar al desempeño de dicho algoritmo. La población está constituida por un conjunto de cromosomas, los cuales representan las posibles soluciones del problema. Si el tamaño de la población es muy pequeña, la población de soluciones contiene poca diversidad, el cual puede ocacionar una convergencia prematura, provocando un atascamiento en un óptimo local. **[1]**

<br>

### **7. Realice la modelación matemática necesaria para la solución, mediante GA, de los problemas siguientes:** 
#### Recuerde que la modelación matemática incluye: definición de los estados inicial y final, definición del test objetivo, y definición de las acciones posibles (operadores).

**a. Problema de la mochila (Knapsack problem)**

En el problema de la mochila se tienen $n$ distintos items $i \in \left\lbrace 1,\dots,n \right\rbrace$. Estos items tienen dos características: peso y beneficio, denotados respectivamente por $w_{i}$ y $b_{i}$, los cuales se consideran números reales no negativos. La mochila soporta un peso máximo $W \geq 0$, por lo que el problema consiste en meter items a la mochila de tal forma que se maximice el beneficio total $B$ sin que el peso total exceda $W$. Es decir, se busca maximizar la función

$$
\begin{align}
B(x) = \sum_{i=1}^{n} x_{i}b_{i},
\end{align}
$$

sujetos a las restricciones

$$
\begin{align}
\sum_{i=1}^{n} x_{i}w_{i} \leq W,
\end{align}
$$

en donde $x_{i} \in \left\lbrace 0,1 \right\rbrace$ representa si el item $i$ está dentro de la mochila ($x_{i} = 1$) o no ($x_{i} = 0$). Por tanto, el espacio de soluciones está dado por

$$
\begin{equation}
\text{Sol} = \left\lbrace x \in \left\lbrace 0,1 \right\rbrace^{n} \: : \: \sum_{i=1}^{n} x_{i}w_{i} \leq W \right\rbrace.
\end{equation}
$$

Para resolver el problema usando GA, fijamos primero el tamaño de la población en $m \in \mathbb{N}$ con $2 \leq m \leq \left| \text{Sol} \right|$. Entonces, el espacio de estados está dado por el conjunto

$$
\begin{equation}
S = \left\lbrace S_{m} \subseteq \text{Sol} \: : \: \left|S_{m} \right| = m \right\rbrace,
\end{equation}
$$

es decir, los estados son todos los subconjuntos de cardinalidad $m$ del espacio de soluciones. En este caso, el estado inicial estará dado por cualquier elemento elegido aleatoriamente de $S$.

Una vez que se tiene una población, es decir un elemento de $S$, el siguiente paso es seleccionar a los individuos que se van a reproducir, lo cual se puede realizar con un operador de selección, en este caso el operador de selección proporcional (véase la sección 3). Dada una población $S_{m} \in S$ y un individuo $x \in S_{m}$, la probabilidad de selección para el operador de selección proporcional estará dada por:

$$
\begin{equation}
p_{x} = \frac{B(x)}{\displaystyle\sum_{y \in S_{m}} B(y)}.
\end{equation}
$$

El siguiente paso en un GA consiste en el cruzamiento, el cual es implementado con un operador de cruzamiento, en este caso el operador de cruce en un punto, el cual consiste en seleccionar aleatoriamente un punto de corte (entre $1$ y $n-1$) en el cromosoma de los padres, y combinar la subsecuencia de genes antes de ese punto de uno de los padres con la subsecuencia de genes después de ese punto del otro padre. Cabe destacar que el operador de cruzamiento se aplica con una probabilidad de $0.9$ sobre cada pareja.

Después, ya que se tiene la descendencia (incluidos los padres sobre los cuales no actuó el operador de cruzamiento), se aplica con una probabilidad de $0.1$ un operador de mutación sobre cada individuo de ésta. En este caso se utilizará el operador de mutación de un gen (véase la sección 5).

El test o función objetivo debe actuar sobre los estados, en este caso sobre las poblaciones. Entonces, definimos éste como una función $f:S \rightarrow \mathbb{R}$ tal que

$$
\begin{equation}
f(S_{m}) = \max \left\lbrace B(x) : x \in S_{m} \right\rbrace,
\end{equation}
$$

es decir, la función objetivo de una población nos devuelve el máximo de los beneficios de sus individuos.

El estado final es, idealmente, cualquier población $S_{m}$ en la que por lo menos uno de sus individuos tenga el máximo beneficio posible.


**b. Problema del viajero vendedor (Travel Salesman Problem, TSP)**

En el problema del viajero vendedor se plantea la existencia de $n$ ciudades conectadas por vías, cada una de las cuales tiene asociada un costo. El problema consiste en encontrar un camino de costo mínimo en el que se visiten todas las ciudades sin repetir el paso por ninguna de ellas, con excepción de la ciudad de la cual se partió, que es justo a la que se regresará al final del camino.

El conjunto de $n$ ciudades y vías entre ellas puede modelarse con el grafo simple y conexo $G$, asignando a cada arista un peso. Cada nodo del grafo representa una ciudad, las aristas representan las vías entre ellas y el peso de cada arista representa el costo de la vía. El problema del viajero vendedor consiste entonces en encontrar un ciclo de Hamilton de costo mínimo, en donde el costo del ciclo está dado por la suma de los costos de las aristas que lo conforman. En este trabajo se considerará el grafo $G$ como no dirigido.

Debido a que la existencia y cantidad de ciclos de Hamilton en un grafo arbitrario es díficil de caracterizar, y considerando que todo grafo simple y conexo de orden $n$ es un subgrafo del grafo completo $K_{n}$; en este trabajo se considerará al grafo $G$ de $n$ ciudades como un subgrafo del grafo completo $K_{n}$, asignando un costo infinito (o muy grande) a las aristas que no existan en el grafo $G$.

El conjunto de vértices de $K_{n}$ se denotará como $V$, y está dado por
$$
\begin{equation}
V = \left\lbrace V_{1},\dots,V_{n} \right\rbrace.
\end{equation}
$$

Por otro lado, el conjunto de aristas, denotado como $E$, es de la forma

$$
\begin{equation}
E = \left\lbrace  \left\lbrace u,v \right\rbrace \subseteq V \: \middle| \:  u \neq v \right\rbrace,
\end{equation}
$$

en donde cada arista $\left\lbrace u,v \right\rbrace$ tiene un costo asociado $Cost(\left\lbrace u,v \right\rbrace)$ dada por una función $Cost : E \rightarrow \mathbb{R}^{+}$. Por tanto, el espacio de soluciones está dado por

$$
\begin{equation}
\text{Sol} = \left\lbrace (P_{1},\dots,P_{n}) \in V^{n} \: \middle| \: \; P_{i} \neq P_{j} \; \forall \, i \neq j \right\rbrace.
\end{equation}
$$

Nótese que el espacio de soluciones está bien definido gracias a que se está trabajando sobre el grafo $K_{n}$, de manera que todas las posibles aristas existen. Dada una solución $P = (P_{1},\dots,P_{n}) \in \text{Sol}$, se define su costo total como:

$$
\begin{equation}
TotalCost(P) = Cost(\left\lbrace P_{1},P_{n} \right\rbrace) + \sum_{i=1}^{n-1} Cost(\left\lbrace P_{i},P_{i+1} \right\rbrace),
\end{equation}
$$

Para resolver el problema usando GA, fijamos primero el tamaño de la población en $m \in \mathbb{N}$ con $2 \leq m \leq \left| \text{Sol} \right|$. Entonces, el espacio de estados está dado por el conjunto

$$
\begin{equation}
S = \left\lbrace S_{m} \subseteq \text{Sol} \: : \: \left|S_{m} \right| = m \right\rbrace,
\end{equation}
$$

en donde el estado inicial será cualquier elemento elegido aleatoriamente de este conjunto, es decir, una población.

Una vez que se tiene una población, es decir un elemento de $S$, el siguiente paso es seleccionar a los individuos que se van a reproducir, lo cual se puede realizar con un operador de selección, en este caso el operador de selección proporcional (véase la sección 3). Dada una población $S_{m} \in S$ y un individuo $P \in S_{m}$, la probabilidad de selección para el operador de selección proporcional estará dada por:

$$
\begin{equation}
p_{P} = \frac{1}{m-1} \cdot \left(1-\frac{TotalCost(P)}{\displaystyle\sum_{Q \in S_{m}} TotalCost(Q)}\right).
\end{equation}
$$

El siguiente paso en un GA consiste en el cruzamiento, el cual es implementado con un operador de cruzamiento, en este caso el operador de cruce OX (véase la sección 4), el cual se aplicará con una probabilidad de $0.9$ sobre cada pareja.

Después, ya que se tiene la descendencia (incluidos los padres sobre los cuales no actuó el operador de cruzamiento), se aplica con una probabilidad de $0.1$ un operador de mutación sobre cada individuo de ésta. En este caso se utilizará el operador SWAP, el cual consiste en elegir aleatoriamente dos genes del cromosoma del individuo e intercambiarlos.

El test o función objetivo debe actuar sobre los estados, en este caso sobre las poblaciones. Entonces, definimos éste como una función $f:S \rightarrow \mathbb{R}$ tal que

$$
\begin{equation}
f(S_{m}) = \min \left\lbrace TotalCost(P) : P \in S_{m} \right\rbrace,
\end{equation}
$$

es decir, la función objetivo de una población nos devuelve el mínimo de los costos de sus individuos.

El estado final es, idealmente, cualquier población $S_{m}$ en la que por lo menos uno de sus individuos tenga el mínimo costo posible.


**c. Obtención de mínimos de la función $f(x)=\sum_{i=1}^D{x^2_i}$, con $-10 \leq x_i \leq 10$.**

El problema requiere que encontremos el mínimo global para la función $f(x)$ en el dominio de $\mathbb{R}^{D}$ definido por cada componente $x_{i}$ tomando valores reales en el intervalo $\left[-10,10\right]$. Entonces, podemos definir el espacio de soluciones como:

$$\text{Sol} = \left\lbrace x \in \mathbb{R}^D \: : \:  -10 \leq x_i \leq 10 \: \forall i \right\rbrace.$$

Para resolver el problema usando GA, fijamos primero el tamaño de la población en $m \in \mathbb{N}$ con $2 \leq m \leq \left| \text{Sol} \right|$. Entonces, el espacio de estados está dado por el conjunto

$$
\begin{equation}
S = \left\lbrace S_{m} \subseteq \text{Sol} \: : \: \left|S_{m} \right| = m \right\rbrace,
\end{equation}
$$

en donde el estado inicial será cualquier elemento elegido aleatoriamente de este conjunto, es decir, una población.

Una vez que se tiene una población, es decir un elemento de $S$, el siguiente paso es seleccionar a los individuos que se van a reproducir, lo cual se puede realizar con un operador de selección, en este caso el operador de selección proporcional (véase la sección 3). Dada una población $S_{m} \in S$ y un individuo $x \in S_{m}$, la probabilidad de selección para el operador de selección proporcional estará dada por:

$$
\begin{equation}
p_{x} = \frac{1}{m-1} \cdot \left(1 - \frac{f(x)}{\displaystyle\sum_{y \in S_{m}} f(y)}\right).
\end{equation}
$$

El siguiente paso en un GA consiste en el cruzamiento, el cual es implementado con un operador de cruzamiento, en este caso el operador de cruce aritmético, en el cual la componente $i$ del hijo estará dada por el promedio aritmético de las componentes $i$ de los padres, lo anterior para toda $i \in \left\lbrace 1,\dots,D \right\rbrace$. Este operador se aplicará con una probabilidad de $0.9$ sobre cada pareja.

Después, ya que se tiene la descendencia (incluidos los padres sobre los cuales no actuó el operador de cruzamiento), se aplica con una probabilidad de $0.1$ un operador de mutación sobre cada individuo de ésta. En este caso se utilizará el operador de mutación uniforme, en el cual se elige un gen de manera aleatoria y se reemplaza por un valor aleatorio en el intervalo $\left[-10,10 \right]$.

El test o función objetivo debe actuar sobre los estados, en este caso sobre las poblaciones. Entonces, definimos éste como una función $F:S \rightarrow \mathbb{R}$ tal que

$$
\begin{equation}
F(S_{m}) = \min \left\lbrace f(x) : x \in S_{m} \right\rbrace,
\end{equation}
$$

es decir, la función objetivo de una población nos devuelve el mínimo valor de $f$ evaluada en sus individuos.

El estado final es, idealmente, cualquier población $S_{m}$ en la que uno de sus individuos sea el mínimo global de $f$.

<br>

### **Referencias**

**[1]** Katoch, S., Chauhan, S. S., & Kumar, V. (2020). A review on genetic algorithm: past, present, and future. Multimedia Tools and Applications, 1-36.

**[2]** Natyhelem G. L. (2006). Algoritmos Genéticos. Universidad Nacional de Colombia. Escuela de Estadística, , 1-36.


