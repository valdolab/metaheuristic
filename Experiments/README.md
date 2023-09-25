# Practica 9 - Lab6
### Solución de problemas mediante Recocido Simulado (SA)


### **1. Analice detalladamente las seis funciones definidas en el documento “Funciones de prueba.pdf”.** (https://github.com/valdolab/metaheuristic/blob/main/Funciones%20de%20prueba.pdf)


Las funciones encontradas en el documento "Funciones de prueba.pdf" son las siguientes:
**Función Alpine 1:**
$$f_1(x)=\sum_{i=1}^{D}{\;|\; x_i\sin(x_i)+0.1x_i \;|\;}$$
Punto mínimo: $x^*=f(0,0)$ y valor mínimo: $f(x^*)=0$

**Función Dixon & Price:**
$$f_2(x)=(x_1-1)^2+\sum_{i=2}^{D}{i(2 \sin(x_i)\;-\;x_{i-1})^2}$$
Punto mínimo: $x^*=f(2(\frac{2^i-2}{2^i}))$ y valor mínimo: $f(x^*)=0$

**Función Quintic:**
$$f_3(x)=\sum_{i=1}^{D}{\;|\; x^5_i-3x^4_i+4x^3_i-2x^2_i-10x_1-4 \;|\;}$$
Punto mínimo: $x^*=f(-1\;or\;2)$ y valor mínimo: $f(x^*)=0$

**Función Schwefel 2.23:**
$$f_4(x)=\sum_{i=1}^{D}{x^{10}_i}$$
Punto mínimo: $x^*=f(0,0)$ y valor mínimo: $f(x^*)=0$

**Función Streched V Sine Wave:**
$$f_5(x)=\sum_{i=1}^{D-1}{(x^2_{i+1}+x^2_i)^{0.25} \Big[\sin^2\{50(x^2_{i+1}+x^2_i)^{0.1}\}+0.1\Big] }$$
Punto mínimo: $x^*=f(0,0)$ y valor mínimo: $f(x^*)=0$

**Función Sum Squares:**
$$f_6(x)=\sum_{i=1}^{D}{ix^2_i}$$
Punto mínimo: $x^*=f(0,0)$ y valor mínimo: $f(x^*)=0$

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
  
def f_delta(state, score, new_state, position):
  new_score = score[0]
  new_score-=abs(state[position]*math.sin(state[position])+0.1*state[position])
  new_score +=abs(new_state[position]*math.sin(new_state[position])+0.1*
                new_state[position])
  return [new_score]
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
  
def f_delta(state, score, new_state, position):
  new_score = score[0]
  if position == 0:
    new_score -= (state[0]-1)**2
    new_score += (new_state[0]-1)**2
  else:
    new_score -= position*((2*math.sin(state[position])-state[position-1])**2)
    new_score += position*((2*math.sin(new_state[position])-
                    new_state[position-1])**2)
  return [new_score]
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
  
def f_delta(state, score, new_state, position):
  new_score = score[0]
  new_score -= abs((state[position]**5)-(3*state[position]**4)+
          (4*state[position]**3)-(2*state[position]**2)-(10*state[position])-4)
  new_score += abs((new_state[position]**5)-(3*new_state[position]**4)+
          (4*new_state[position]**3)-(2*new_state[position]**2)-
          (10*new_state[position])-4)
  return [new_score]
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
  
  def f_delta(state, score, new_state, position):
  new_score = score[0]
  new_score -= state[position]**10
  new_score += new_state[position]**10
  return [new_score]
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
  
  def f_delta(state, score, new_state, position):
  new_score = score[0]
  new_score -= (((state[position+1]**2)+(state[position]**2))**0.25) *
     (math.sin(50*(((state[position+1]**2)+state[position]**2)**0.1))**2+(0.1))
  new_score += (((new_state[position+1]**2)+(new_state[position]**2))**0.25) *
      (math.sin(50*(((new_state[position+1]**2)+new_state[position]**2)
      **0.1))**2+(0.1)) 
  return [new_score]
```

**Función Sum Squares:**
$$f_6(x)=\sum_{i=1}^{D}{ix^2_i}$$

```python=
#funcion sum squares
#funcion sum squares
def f(x):
  total = 0
  for i,xi in enumerate(x):
    total += (i+1)*(xi**2)
  return total

def f_delta(state, score, new_state, position):
  new_score = score[0]
  new_score -= (position+1)*(state[position]**2)
  new_score += (position+1)*(new_state[position]**2)
  return [new_score]
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
  return f_delta(state, score, new_state, position)
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
    if isValidState(state_copy, score_copy) and state[random_position] != 
                                            state_copy[random_position]:
      break
    state_copy[random_position] = state[random_position]
    score_copy = copy.copy(score)
  return state_copy, score_copy
```

```python=
def rs(cooler, time_per_temperature, final_temperature, verbose=True, k=1.380649e-23):
  state, score = randomState()
  temperature = 100
  if verbose: print("T", round(temperature,2), " Score=", printPretty(score[0]), " -> ", [printPretty(x) for x in state])
  best = [f(state), temperature, 0, False]
  evaluation = 0
 
  #while temperature >= final_temperature:
  for k in range(0,50):
    for i in range(0, time_per_temperature):
      evaluation = evaluation + 1
      new_state, new_score = neighbour(state,score)
      delta = new_score[0]-score[0]
      if verbose: print("\t", "Neighbour: Score=", printPretty(new_score[0]), " -> ", [printPretty(x) for x in state], end='')
      if delta < 0 or random.uniform(0,1) < math.exp(-delta/temperature):
        state,score = new_state, new_score
        if verbose: print(" !New Accepted", end='')
      if verbose: print()
      if f(state) <= best[0]:
        best[0] = round(f(state),2)
        best[1] = temperature
        best[2] = evaluation
    if verbose: print("T", round(temperature,2), " Score=", printPretty(score[0]), " -> ", [printPretty(x) for x in state])
    
    temperature = cooler(temperature)
 
  if best[0] == round(f(state),2):
    best[3] = True  
  return best, state
```

```python=
cooler = lambda t : t*random.uniform(0.8,0.99)
```

### **Experimentos**

Para realizar las ejecuciones indicadas en el punto 4 y 5, se implementó lo siguiente.

```python=
d = 30
eva = 20
results = [0]*eva
ex_time = [0]*eva
min_temp = [0]*eva
 
for i in range(0,eva):
  start_time = time.time()
  mejor, solutionPath = rs(cooler, 10, 0.1,verbose=False)
  end_time = time.time()
  results[i] = f(solutionPath)
  ex_time[i] = end_time - start_time
  min_temp[i] = round(mejor[1],2)
  print(round(mejor[0],2), round(mejor[1],2), round(mejor[2],2), mejor[3])
 
print()
print('Estadísticas de los resultados (' + str(eva) + ' evaluaciones, dimensión ' + str(d) + '):')
print('Peor = ' + str(round(max(results),3)))
print('Mejor = ' + str(round(min(results),3)))
print('Promedio = ' + str(round(np.mean(results),3)))
print('Mediana = ' + str(round(s.median(results),3)))
print('Desviación estándar = ' + str(round(s.stdev(results),3)))
print()
print('Estadísticas del tiempo de ejecución en segundos (' + str(eva) + ' evaluaciones, dimensión ' + str(d) + '):')
print('Peor = ' + str(round(max(ex_time),4)))
print('Mejor = ' + str(round(min(ex_time),4)))
print('Promedio = ' + str(round(np.mean(ex_time),4)))
print('Mediana = ' + str(round(s.median(ex_time),4)))
print('Desviación estándar = ' + str(round(s.stdev(ex_time),4)))
print()
print('Estadísticas de la temperatura mínima (' + str(eva) + ' evaluaciones, dimensión ' + str(d) + '):')
print('Máximo = ' + str(round(max(min_temp),4)))
print('Mínimo = ' + str(round(min(min_temp),4)))
print('Promedio = ' + str(round(np.mean(min_temp),4)))
print('Mediana = ' + str(round(s.median(min_temp),4)))
print('Desviación estándar = ' + str(round(s.stdev(min_temp),4)))
```

<br>

### **4. Reporte los resultados obtenidos. Para ello, realice 20 ejecuciones independientes, con la siguiente configuración:** 

### b) Muestre el mejor, peor, promedio, mediana y desviación estándar de los resultados en las 20 ejecuciones.
#### Para d = 10

| Función | Mejor | Peor | Promedio | Mediana | Desvianción Estándar|
| ------- | ----- | ---- | -------- | ------- | --------------------|
|Alpine 1	|1.311|	9.710	|4.499	|4.058|	2.248|
|Dixon & Price|	301.937	|1989.071	|1040.791	|1052.343|	469.563|
|Quintic	|8.456	|37.292	|21.364	|20.549|	7.551|
|Schwefel |2.23|	0.019|	22.654|	2.581|	1.317	|4.881|
|Streched V Sine Wave |2.7|15.93|9.43|9.287|3.389|				
|Sum Squares	|3.735	|19.835	|9.794	|10.072	|4.068|
|Promedio|	63.0916	|415.7124	|215.8058|	217.6678	|97.6622|


#### Para d = 30
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1	|16.785	|36.348|	22.758	|21.809	|4.945|
|Dixon & Price	|5042.568|	15669.961	|10414.838	|10933.326|	2502.303|
|Quintic	|109.244|	1962.651	|270.745|	173.855|	400.951|
|Schwefel 2.23	|79.473	|256947.648|	19310.068	|3549.058|	56719.741|
|Streched V Sine Wave| 21.99|43.66|29.02|27.98|50.3|					
|Sum Squares|	160.923	|504.000	|312.649|	309.514|	87.350|
|Promedio	|1081.7986|	55024.1216	|6066.2116	|2997.5124|	11943.0580|



### c) Muestre el mejor, peor, promedio, mediana y desviación estándar de los tiempos de ejecución (en segundos) en las 20 ejecuciones
#### Para d = 10
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1	|0.0049	|0.0096	|0.0061	|0.0060|	0.0012|
|Dixon & Price|	0.0055|	0.0114|	0.0076	|0.0074|	0.0017|
|Quintic	|0.0091|	0.2520	|0.0126|	0.0114|	0.0038|
|Schwefel 2.23	|0.0037	|0.0053|	0.0041|	0.0040|	0.0004|
|Streched V Sine Wave|0.0081|0.0132|0.0098|0.0096|0.0012|					
|Sum Squares	|0.0042|	0.0101|	0.0054|	0.0050|	0.0014|
|Promedio|	0.0055	|0.0577|	0.0072	|0.0068|	0.0017|



#### Para d = 30
| Función | Mejor | Peor | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1|	0.0074|	0.0170	|0.0098	|0.0097|	0.0020|
|Dixon & Price	|0.0094	|0.0191|	0.0123|	0.0112	|0.0025|
|Quintic	|0.0230|	0.0315|	0.0273|	0.0267|	0.0027|
|Schwefel 2.23	|0.0068	|0.0118	|0.0085|	0.0083	|0.0013|
|Streched V Sine Wave	|0.0184|0.0287|0.0212|0.0209|0.0023|					
|Sum Squares|	0.0068	|0.0132	|0.0088|	0.0086	|0.0016|
|Promedio	|0.0107|	0.0185	|0.0133|	0.0129|	0.0020|



### d) Muestre el mínimo, máximo, promedio, mediana y desviación estándar de la temperatura mínima alcanzada por el algoritmo en las 20 ejecuciones (falta por hacer)
#### Para d = 10
| Función | Máximo | Mínimo | Promedio | Mediana | Desviación Estándar |
| ------- | ----- | ---- | -------- | ------- | ------------------- |
|Alpine 1	|3.22|	0.1500	|0.5960	|0.3600	|0.6927|
|Dixon & Price|	100.0000	|0.3800	|19.0570|	7.1300	|29.5900|
|Quintic	|8.4400|	0.2000	|1.0220	|0.5000	|1.8000|
|Schwefel |2.23	|8.1000|	0.2300|	1.8210|	0.8200|	2.2316|
|Streched V Sine Wave|85.36|0.23|6.597|1.09|18.871|					
|Sum Squares	|1.1700|	0.2000|	0.4760|	0.3450	|0.2877|
|Promedio|	24.1860|	0.2320|	4.5944|	1.8310|	6.9204|


#### Para d = 30
| Función | Máximo | Mínimo | Promedio | Mediana | Desviación Estándar |
| ------- | ------ | ------ | -------- | ------- | ------------------- |
|Alpine 1	|0.7100	|0.2300|	0.4320|	0.3600|	0.1565|
|Dixon & Price	|67.6700	|0.3100	|9.9795	|2.5300	|16.6024|
|Quintic	|0.6900|	0.1400|	0.4150|	0.4250|	0.1857|
|Schwefel |2.23	|1.1300	|0.1400|	0.4595|	0.3900	|0.2447|
|Streched V Sine Wave |8.08|0.23|0.922|0.47|1.709	|					
|Sum Squares|	1.0300|	0.1800|	0.4730|	0.4350|	0.2412|
|Promedio	|14.2460	|0.2000|	2.3518|	0.8280	|3.4861|


<br>

### **5. Reporte, además, de forma independiente, para cada función y cada ejecución: mejor solución encontrada por el algoritmo, la temperatura a la que se encontró, el número de evaluaciones de la función objetivo a la que se encontró, y si fue conservada o no por el algoritmo (fue conservada si coincide con la solución devuelta, NO fue conservada si el algoritmo la desechó durante su ejecución).**

#### **Para $d=10$**
#### Función Alpine:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	2.3	|0.42|	477|FALSO|
|2| 4.48|0.52|	500|VERDADERO|
|3|	5.38|3.22|	325|FALSO|
|4|	4.12|1.09|	455|FALSO|
|5|	2.6	|0.62|	492|FALSO|
|6|	2.3	|0.3|	495|VERDADERO|
|7| 3.23|0.15|	500|VERDADERO|
|8|	2.78|0.29|	500|VERDADERO|
|9|	3.08|0.37|	483|FALSO|
|10| 5.69|0.47|	495|FALSO|
|11| 2.09|0.25|	500|VERDADERO|
|12| 3.39|0.22|	500|VERDADERO|
|13| 5.98|1.38|	449|FALSO|
|14| 4.4|0.29|	500|VERDADERO|
|15| 6.25|0.8|	443|FALSO|
|16| 4.86|0.49|	500|VERDADERO|
|17| 1.3|0.15|	485|FALSO|
|18| 2.15|0.35|	474|FALSO|
|19| 3.78|0.34|	493|FALSO|
|20| 4   |0.2 | 499|VERDADERO|



#### Función Dixon & Price:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	331.24|	28.93|	122|	FALSO|
|2|	38.08	|0.38|	488	|FALSO|
|3|	481.35|	41.29|	90	|FALSO|
|4|	165.18|	15.32|	177	|FALSO|
|5|	255.85|	2.6	|301	|FALSO|
|6|	283.87|	0.79|	461	|FALSO|
|7|	364.35|	1.73|	321	|FALSO|
|8|	161.64|	8.7	|203	|FALSO|
|9	|405.89|	100	|10	|FALSO|
|10	|531.94	|100	|0	|FALSO|
|11	|77.03	|4.34	|290|	FALSO|
|12	|220.25	|2.79	|279|	FALSO|
|13	|416.46	|1.47	|370|	FALSO|
|14	|264.16	|17.43	|170|	FALSO|
|15	|233.04	|15.44	|181|	FALSO|
|16	|131.91	|5.06	|278|	FALSO|
|17	|92.34	|5.56	|197|	FALSO|
|18	|361.3	|11.35	|174|	FALSO|
|19	|458.17	|16.69|	156	|FALSO|
|20|	269.14|	1.27|	348|	FALSO|


#### Función Quintic:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1	|24.29|	0.49|	500|	VERDADERO|
|2	|15.86|	1.55|	355	|VERDADERO|
|3	|21.29|	0.28|	500	|VERDADERO|
|4	|16.84|	0.23|	493	|VERDADERO|
|5	|26.77|	0.47|	443	|FALSO|
|6	|37.29|	0.51|	479	|VERDADERO|
|7	|16.12|	1.82|	362	|FALSO|
|8	|17.74|	0.31|	500	|VERDADERO|
|9	|8.46|	0.7	|500	|VERDADERO|
|10	|29.43|	0.22	|500|	VERDADERO|
|11	|15.29	|0.43|	440	|FALSO|
|12	|15.67	|1.14|	413	|VERDADERO|
|13	|31.36|	0.4	|500	|VERDADERO|
|14	|16.17	|0.53|	485	|VERDADERO|
|15	|15.98|	8.44|	255	|FALSO|
|16	|20.81|	0.2	|490	|VERDADERO|
|17	|25.3|	0.87|	457	|FALSO|
|18	|29.61|	0.66|	500	|VERDADERO|
|19	|27.54|	0.87|	435	|FALSO|
|20	|9.2|	0.32|	500	|VERDADERO|


#### Función Schwefel 2.23:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	0.43|	5.06|	280	|FALSO|
|2|	1	|2.34	|342	|FALSO|
|3|	2.66|	0.54|	476	|FALSO|
|4|	0.06|	0.94|	446	|FALSO|
|5|	1.14|	1.48|	328	|FALSO|
|6|	0.7	|0.32	|500	|VERDADERO|
|7|	1.79	|0.23|	491	|VERDADERO|
|8|	0.38	|6.02|	241	|FALSO|
|9|	0.02	|0.27|	500	|VERDADERO|
|10|	3.01|	1.18   |	495	|VERDADERO|
|11|22.59	|0.7|	425	|FALSO|
|12|	0.69|	0.4	|491	|FALSO|
|13|	0.07|	8.1	|245	|FALSO|
|14|	0.37|	2.16|	358	|FALSO|
|15|	0.05|	0.51|	442	|FALSO|
|16|	2.19|	1.02|	426	|FALSO|
|17|	1.14|	0.5	|496	|VERDADERO|
|18|	0.09|	0.43|	462	|FALSO|
|19|	0.51|	3.99|	304	|FALSO|
|20|	2.77|	0.23|	500|	VERDADERO|

#### Función Streched V Sine Wave:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|5.85| 1.48| 471| False|
|2|5.34| 0.86| 431| False|
|3|5.57| 2.35| 473| False|
|4|5.48| 1.32| 428| False|
|5|4.74| 1.13| 404| False|
|6|5.91| 0.45| 492| False|
|7|5.82| 5.25| 340| False|
|8|5.31| 1.05| 426| False|
|9|5.62| 0.31| 500| True|
|10|4.48| 1.05| 398| False|
|11|3.4| 0.83| 432| False|
|12|4.64| 1.3| 382| False|
|13|5.17| 3.59| 261| False|
|14|5.71| 85.36| 28| False|
|15|5.8| 0.49| 482| False|
|16|4.77| 9.98| 201| False|
|17|5.13| 0.54| 475| False|
|18|4.18| 0.46| 499| True|
|19|2.71| 0.23| 500| True|
|20|6.18| 13.91| 180| False|


#### Función Sum Squares:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	5.83	|0.69|	417	|FALSO|
|2|	10.57	|0.35|	493	|VERDADERO|
|3|	10.93	|0.34|	462	|FALSO|
|4|	5.53	|1.15|	463	|VERDADERO|
|5|	7.18	|0.27|	500	|VERDADERO|
|6|	8.99	|0.81|	447	|FALSO|
|7|	3.72	|0.22|	465	|FALSO|
|8|	16.73	|0.27|	500	|VERDADERO|
|9|	9.57	|0.67|	432	|VERDADERO|
|10|	11.79|	0.23|	495	|VERDADERO|
|11|	8.93|0.32	|493	|VERDADERO|
|12|	11.06|	0.56|	463	|VERDADERO|
|13|	19.83|	0.2	|476	|VERDADERO|
|14|	11.44|	0.34|	500	|VERDADERO|
|15|	5.93|	0.27|	492	|VERDADERO|
|16|	13.16|	1.17|	457	|FALSO|
|17|	6.27|	0.32|	500	|VERDADERO|
|18|	3.9	|0.43	|497	|FALSO|
|19|	11.6|	0.44|	480	|VERDADERO|
|20|	11.49|	0.47|	500|	VERDADERO|

<br>

#### **Para $d=30$**
#### Función Alpine:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	25.59|	0.33	|498|	FALSO|
|2|	21.21|	0.28	|499|	VERDADERO|
|3|	35.46|	0.67	|484|	FALSO|
|4|	16.64|	0.35	|490|	FALSO|
|5|	20.58|	0.71	|498|	VERDADERO|
|6|	23.12|	0.61	|500|	VERDADERO|
|7|	24.62|	0.34	|497|	VERDADERO|
|8|	23.84|	0.33	|495|	FALSO|
|9|	18.55|	0.24	|492|	VERDADERO|
|10|	22.54|	0.23|	500|	VERDADERO|
|11|	17.66|	0.48|	493|	VERDADERO|
|12|	21.35|	0.32|	500|	VERDADERO|
|13|	17.91|	0.64|	480|	FALSO|
|14|	18.65|	0.56|	466|	FALSO|
|15|	17.98|	0.36|	476|	FALSO|
|16|	28.24|	0.6	|499|	FALSO|
|17|	22.26|	0.26|	496|	FALSO|
|18|	26.7|0.57|	500|	VERDADERO|
|19|	17.89|	0.4|	499|	VERDADERO|
|20|	30.29|	0.36|	500|	VERDADERO|



#### Función Dixon & Price:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	7272.26|	15.88|	154|	FALSO|
|2|	6862.51|	1.51|	407|	FALSO|
|3|	5889.16|	3.35|	333|	FALSO|
|4|	7241.24|	0.35|	479|	FALSO|
|5|	4739.37|	3.14|	294|	FALSO|
|6|	7932.99|	0.47|	498|	FALSO|
|7|	4891.57|	1.05|	429|	FALSO|
|8|	7671.79|	5.14|	260|	FALSO|
|9|	6523.36|	7.14|	240|	FALSO|
|10|	7638.42|	1.92|	347|	FALSO|
|11|	6363.54|	0.31|	480|	FALSO|
|12|	6183.86|	0.53|	395|	FALSO|
|13|	4882.22|	12.42|	204|	FALSO|
|14|	8852.19|	0.39|	495|	FALSO|
|15|	7985.33|	1.86|381|	FALSO|
|16|	8513.67|	67.67|	31|	FALSO|
|17|	6672.23|	27.01|	114|	FALSO|
|18|	7257.29|	0.95|	369|	FALSO|
|19|	7054.83|	13.6|	170|	FALSO|
|20|	6778.13|	34.9|	97|	FALSO|



#### Función Quintic:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	195.45|	0.38|	497|	FALSO|
|2|	143.91|	0.22|	490|	VERDADERO|
|3|	151.37|	0.46|	465|	VERDADERO|
|4|	176|	0.14|	500|	VERDADERO|
|5|	173.21|	0.48|	499|	VERDADERO|
|6|	194.18|	0.39|	491|	VERDADERO|
|7|	170.75|	0.26|	500|	VERDADERO|
|8|	168.52|	0.68|	490|	VERDADERO|
|9|	208.49|	0.3	|494|	VERDADERO|
|10|	157.65|	0.65|	494|	VERDADERO|
|11|	109.24|	0.19|	483|	VERDADERO|
|12|	1962.65|	0.69|	495|	VERDADERO|
|13|	326.92|	0.55|	500	|VERDADERO|
|14|	174.5|	0.16|	500	|VERDADERO|
|15|	238.01|	0.24|	500	|VERDADERO|
|16|	229.91|	0.69|	491	|VERDADERO|
|17|	146.62|	0.48|	500	|VERDADERO|
|18|	156.48|	0.27|	500	|VERDADERO|
|19|	129.12|	0.51|	477	|FALSO|
|20|	201.54|	0.56|	500	|VERDADERO|



#### Función Schwefel 2.23:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	9773.18|	1.13|	431	|FALSO|
|2|	1014.18|	0.36|	500	|VERDADERO|
|3|	138.17|	0.25	|499	|VERDADERO|
|4|	143.63|	0.14	|500	|VERDADERO|
|5|	15445.73|	0.36|	490	|VERDADERO|
|6|	323.29|	0.58	|497	|VERDADERO|
|7|	117.29|	0.56	|462	|FALSO|
|8|	256947.6|	0.29	|486|	FALSO|
|9|	4746.48|	0.25	|476|	FALSO|
|10|	5584.19|	0.42|	491	|VERDADERO|
|11|	3235.01|	0.32|	491	|VERDADERO|
|12|	3863.1|	0.35	|497	|VERDADERO|
|13|	254.3|	0.45	|500	|VERDADERO|
|14|	34466.78|	0.54|	500	|VERDADERO|
|15|	22687.96|	0.2|	500	|VERDADERO|
|16|	77.42|	0.89|	450	|FALSO|
|17|	232.85|	0.59|	500	|VERDADERO|
|18|	2251.39|	0.58|	483	|VERDADERO|
|19|	4423.41|	0.24|	500	|VERDADERO|
|20|	20471.63|	0.69|	500	|VERDADERO|



#### Función Streched V Sine Wave:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|23.14| 0.47| 481| False|
|2|23.34| 0.39| 492| False|
|3|25.45| 0.34| 494| False|
|4|25.55| 8.08| 211| False|
|5|28.57| 0.83| 481| False|
|6|28.52| 0.73| 413| False|
|7|26.22| 0.28| 500| True|
|8|21.1 |0.47 |498 |False|
|9|30.16| 1.01| 477| False|
|10|31.08| 0.49| 499| False|
|11|19.39| 0.23| 478| False|
|12|23.34| 0.92| 487| False|
|13|25.47| 0.59| 498| False|
|14|20.53| 1.36| 340| False|
|15|27.89| 0.27| 500| True|
|16|31.9 |0.41 |483 |False|
|17|24.68| 0.38| 461| False|
|18|20.3 |0.35 |490 |False|
|19|33.52 |0.29| 500| True|
|20|24.5 |0.56 |486 |False|

#### Función Sum Squares:
| Ejecución | Valor de la Mejor Solución | Temperatura | Evaluación | Conservada |
| --------- | -------------------------- | ----------- | ---------- | ---------- |
|1|	501.62|	0.82|	471	|FALSO|
|2|	316.84|	0.45|	489	|VERDADERO|
|3|	283.45|	0.24|	500	|VERDADERO|
|4|	468.05|	0.29|	500	|VERDADERO|
|5|	376.47|	0.19|	500	|VERDADERO|
|6|	194.9|	0.18|	500	|VERDADERO|
|7|	366.99|	0.72|	493	|FALSO|
|8|	230.23| 0.51|	491	|VERDADERO|
|9|	350.68|	0.32|	492	|VERDADERO|
|10|	221.3|	0.53|	490	|VERDADERO|
|11|	219.57|	0.47|	500	|VERDADERO|
|12|	372.01|	0.24|	500	|VERDADERO|
|13|	298.22|	0.72|	497	|VERDADERO|
|14|	358.83|	0.32|	486	|VERDADERO|
|15|	262.38|	1.03|	459	|FALSO|
|16|	160.92|	0.54|	455	|VERDADERO|
|17|	345.4|	0.3	|500	|VERDADERO|
|18|	262.18|	0.33|	463	|VERDADERO|
|19|	300.54|	0.84|	453	|FALSO|
|20|	358.08|	0.42|	500|	VERDADERO|


