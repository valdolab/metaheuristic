## Solución de problemas mediante Ascensión de Colinas

<br>

### **1. Analice detalladamente las seis funciones definidas en el documento “Funciones de prueba.pdf”.**

Falta Poner un analisis detallado de las seis funciones

Las funciones encontradas en el documento "Funciones de prueba.pdf" son las siguientes:
**Función Alpine 1:**
$$f_1(x)=\sum_{i=1}^{D}{\;|\; x_i\sin(x_i)+0.1x_i \;|\;}$$
Punto mínimo: $$x^*=f(0,0)$$ y valor mínimo: $$f(x^*)=0$$

**Función Dixon & Price:**
$$f_2(x)=(x_1-1)^2+\sum_{i=2}^{D}{i(2 \sin(x_i)\;-\;x_{i-1})^2}$$
Punto mínimo: $$x^*=f(2(\frac{2^i-2}{2^i}))$ y valor mínimo: $f(x^*)=0$$

**Función Quintic:**
$$f_3(x)=\sum_{i=1}^{D}{\;|\; x^5_i-3x^4_i+4x^3_i-2x^2_i-10x_1-4 \;|\;}$$
Punto mínimo: $$x^*=f(-1\;or\;2)$$ y valor mínimo: $$f(x^*)=0$$

**Función Schwefel 2.23:**
$$f_4(x)=\sum_{i=1}^{D}{x^{10}_i}$$
Punto mínimo: $$x^*=f(0,0)$$ y valor mínimo: $$f(x^*)=0$$

**Función Streched V Sine Wave:**
$$f_5(x)=\sum_{i=1}^{D-1}{(x^2_{i+1}+x^2_i)^{0.25} \Big[\sin^2\{50(x^2_{i+1}+x^2_i)^{0.1}\}+0.1\Big] }$$
Punto mínimo: $$x^*=f(0,0)$$ y valor mínimo: $$f(x^*)=0$$

**Función Sum Squares:**
$$f_6(x)=\sum_{i=1}^{D}{ix^2_i}$$
Punto mínimo: $$x^*=f(0,0)$$ y valor mínimo: $$f(x^*)=0$$

### **2. Implemente dichas funciones.**

**Función Alpine 1:**
$$f_1(x)=\sum_{i=1}^{D}{\;|\; x_i\sin(x_i)+0.1x_i \;|\;}$$


```python=
#funcion alpine1
def f(x):
  total = 0
  for xi in x:
    total += abs(xi*math.sin(xi)+0.1*xi)
  return total
```

**Función Dixon & Price:**
$$f_2(x)=(x_1-1)^2+\sum_{i=2}^{D}{i(2 \sin(x_i)\;-\;x_{i-1})^2}$$

```python=
#funcion dixon and price
def f(x):
  total = 0
  binomio = (x[0]-1)**2
  for i in range(1,len(x)):
    total += i*((2*math.sin(x[i])-x[i-1])**2)
  
  total = binomio + total
  return total
```

**Función Quintic:**
$$f_3(x)=\sum_{i=1}^{D}{\;|\; x^5_i-3x^4_i+4x^3_i-2x^2_i-10x_1-4 \;|\;}$$

```python=
#funcion quintic
def f(x):
  total = 0
  for xi in x:
    total += abs((xi**5)-(3*xi**4)+(4*xi**3)-(2*xi**2)-(10*xi)-4)
  return total
```

**Función Schwefel 2.23:**
$$f_4(x)=\sum_{i=1}^{D}{x^{10}_i}$$

```python=
#funcion schwefel 2.23
def f(x):
  total = 0
  for xi in x:
    total += xi**10
  return total
```

**Función Streched V Sine Wave:**
$$f_5(x)=\sum_{i=1}^{D-1}{(x^2_{i+1}+x^2_i)^{0.25} \Big[\sin^2\{50(x^2_{i+1}+x^2_i)^{0.1}\}+0.1\Big] }$$

```python=
#funcion strched V sine wave
def f(x):
  total = 0
  for i in range(0,len(x)-1):
    total += (((x[i+1]**2)+(x[i]**2))**0.25)*(math.sin(50*(((x[i+1]**2) +
                x[i]**2)**0.1))**2+(0.1))   
  return total
```

**Función Sum Squares:**
$$f_6(x)=\sum_{i=1}^{D}{ix^2_i}$$

```python=
#funcion sum squares
def f(x):
  total = 0
  for i,xi in enumerate(x):
    total += (i+1)*(xi**2)
  return total
```

<br>

### **3. Implemente el algoritmo de Recocido Simulado para la solución de los problemas de minimización de las funciones anteriores. Considere D=10 y posteriormente D=30 dimensiones.** 

```python=
import random
import copy
import math
import numpy as np
import statistics as s
import time
```

Para el resto del código, hemos utilizado el mismo que el presentado para la practica anterior, motivo por el cual no incluiremos los comentarios respectivos.

```python=
d = 0
printPretty = lambda number:round(number,2)
```

```python=
def getStateScore(state):
  return [f(state)]
```

```python=
def getDeltaScore(state, score, new_state, position):
  new_score = score[0]
  new_score -= state[position]*state[position]
  new_score += new_state[position]*new_state[position]
  return [new_score]
```

```python=
def isValidState(state, score):
  if len(state) != d:
    return False
  return True
```

```python=
def randomState():
  random.seed(a=None)
  state = [0]*d
  while True:
    for i in range(0, d):
      state[i] = random.uniform(-10,10)
    score = getStateScore(state)
    if isValidState(state, score):
      break
  return state, score
```

```python=
def neighbour(state, score):
  state_copy = copy.copy(state)
  score_copy = copy.copy(score)
  while True:
    random_position = random.randint(0, len(state)-1)
    state_copy[random_position] = random.uniform(-10,10)
    score_copy = getDeltaScore(state, score, state_copy, random_position)
    if isValidState(state_copy, score_copy) and state[random_position] != state_copy[random_position]:
      break
    state_copy[random_position] = state[random_position]
    score_copy = copy.copy(score)
  return state_copy, score_copy
```

```python=
def rs(cooler, time_per_temperature, final_temperature, verbose=True, k=1.380649e-23):
  state, score = randomState()
  temperature = score[0]*0.4
  if verbose: print("T", round(temperature,2), " Score=", printPretty(score[0]), " -> ", [printPretty(x) for x in state])
  con = 0
  con2 = 0

  while temperature >= final_temperature:
    for i in range(0, time_per_temperature):
      con2 += 1
      new_state, new_score = neighbour(state,score)
      delta = new_score[0]-score[0]
      if verbose: print("\t", "Neighbour: Score=", printPretty(new_score[0]), " -> ", [printPretty(x) for x in state], end='')
      if delta < 0 or random.uniform(0,1) < math.exp(-delta/temperature):
        state,score = new_state, new_score
        if verbose: print(" !New Accepted", end='')
      if verbose: print()
    if verbose: print("T", round(temperature,2), " Score=", printPretty(score[0]), " -> ", [printPretty(x) for x in state])
    temperature = cooler(temperature)
    con += 1
  return state, con, con2
```

```python=
cooler = lambda t : t*random.uniform(0.8,0.99)
```

### **Experimentos**



<br>

### **4. Reporte los resultados obtenidos. Para ello, realice 20 ejecuciones independientes, con la siguiente configuración:** 

### b) Muestre el mejor, peor, promedio, mediana y desviación estándar de los resultados en las 20 ejecuciones.
#### Para d = 10

| Función | Mejor | Peor | Promedio | Mediana | Desvianción Estándar|
| ------- | ----- | ---- | -------- | ------- | --------------------|
|Alpine 1	|0.344	|4.288	|2.217	|2.169	|1.016|
|Dixon & Price	|7.698	|112.67	|52.289	|50.357|	27.289|
|Quintic|	22.398	|107.702|	55.329|	55.956|	17.796|
|Schwefel |2.23	|0	|169.255	|14.447	|1.208	|38.177|
|Streched V Sine Wave	|1.318|	4.905|	2.942	|2.987|	0.796|
|Sum Squares	|0.766	|8.058|	3.8	|3.68	|1.864|
|Promedios Globales	|5.420|	67.813	|21.837|	19.392|	14.489|

#### Para d = 30
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1	|1.25	|4.015|	2.73	|2.73	|0.814|
|Dixon & Price	|69.692	|562.982	|208.554|	170.237	|118.694|
|Quintic	|95.018|	151.5	|130.068|	138.337|	15.85|
|Schwefel 2.23	|0.004|	8.64|	0.706|	0.067|	2.001|
|Streched V Sine Wave	|7.05	|12.622	|10.611	|10.882	|1.404|
|Sum Squares	|19.821|	83.882|	47.61	|45.378|	15.055|
|Promedios Globales	|32.139	|137.273|	66.713|	61.271|	25.636|


### c) Muestre el mejor, peor, promedio, mediana y desviación estándar de los tiempos de ejecución (en segundos) en las 20 ejecuciones
#### Para d = 10
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1	|0.0015	|0.0023	|0.0019|	0.002|	0.0002|
|Dixon & Price|	0.0031|	0.0049|	0.0035|	0.0034	|0.0005|
|Quintic	|0.0045	|0.0083	|0.0054	|0.0051	|0.0011|
|Schwefel 2.23|	0.0086	|0.0139	|0.0096	|0.0092|	0.0012|
|Streched V Sine Wave|	0.013	|0.0242	|0.017	|0.0168	|0.0026|
|Sum Squares	|0.0371	|0.0515	|0.0437	|0.0433|	0.0039|
|Promedios Globales|	0.011|	0.017|	0.013|	0.013|	0.001|


#### Para d = 30
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1|	0.0206|	0.0311|	0.0237|	0.0231|	0.0027|
|Dixon & Price|	0.0383	|0.0541	|0.0449|	0.0443	|0.0037|
|Quintic	|0.051	|0.0676	|0.0573	|0.056	|0.0047|
|Schwefel 2.23	|0.0909	|0.1074	|0.0986|	0.0991|	0.0049|
|Streched V Sine Wave|	0.018|	0.0268|	0.0214|	0.021	|0.0022|
|Sum Squares	|0.0378	|0.0533	|0.0436	|0.0438	|0.0037|
|Promedios Globales|	0.0427	|0.0567	|0.0482	|0.0478	|0.0036|


### d) Muestre el mínimo, máximo, promedio, mediana y desviación estándar de la temperatura mínima alcanzada por el algoritmo en las 20 ejecuciones (falta por hacer)
#### Para d = 10
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1|0.0035|0.0073|0.0038|0.0036|0.0008|
|Dixon & Price|0.0043|0.0058|0.0046|0.0044|0.0004|
|Quintic|0.0064|0.0091|0.0068|0.0066|0.0006|
|Schwefel 2.23|0.0031|0.0038|0.0033|0.0032|0.0002|
|Streched V Sine Wave|0.0066|0.0098|0.0070|0.0067|0.0009|
|Sum Squares|0.0033|0.0036|0.0034|0.0033|0.0001|

#### Para d = 30
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1|0.0069|0.0128|0.0077|0.0073|0.0014|
|Dixon & Price|0.0092|0.0129|0.0097|0.0094|0.0008|
|Quintic|0.0154|0.0225|0.0174|0.0161|0.0023|
|Schwefel 2.23|0.0058|0.0086|0.0062|0.0059|0.0007|
|Streched V Sine Wave|0.0170|0.0223|0.0187|0.0182|0.0016|
|Sum Squares|0.0059|0.0063|0.0060|0.0060|0.0001|

<br>

