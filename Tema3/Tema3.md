# Espacio de estados y búsqueda

- [Espacio de estados y búsqueda](#espacio-de-estados-y-búsqueda)
  - [Problemas y espacio de estados](#problemas-y-espacio-de-estados)
  - [Métodos de búsqueda no informados (Uninformed Search Methods)](#métodos-de-búsqueda-no-informados-uninformed-search-methods)
    - [Búsqueda en profundidad (Depth-First Search)](#búsqueda-en-profundidad-depth-first-search)
      - [Implementación iterativa](#implementación-iterativa)
      - [Implementación recursiva](#implementación-recursiva)
      - [Propiedades de DFS](#propiedades-de-dfs)
    - [Búsqueda en anchura (Breadth-First Search)](#búsqueda-en-anchura-breadth-first-search)
      - [Implementación](#implementación)
      - [Propiedades de BFS](#propiedades-de-bfs)
    - [Búsqueda de coste uniforme (Uniform-Cost Search)](#búsqueda-de-coste-uniforme-uniform-cost-search)
      - [Implementación de UCS](#implementación-de-ucs)
      - [Propiedades de UCS](#propiedades-de-ucs)
  - [Metodos de búsqueda informados (Informed Search Methods)](#metodos-de-búsqueda-informados-informed-search-methods)
    - [Búsqueda voraz (Greedy Search)](#búsqueda-voraz-greedy-search)
    - [Búsqueda A Star (A\* Search)](#búsqueda-a-star-a-search)
  - [Arboles de juego](#arboles-de-juego)
  - [Búsqueda adversarial](#búsqueda-adversarial)
    - [Minimax](#minimax)
      - [Implementación de Minimax](#implementación-de-minimax)
      - [Propiedades de Minimax](#propiedades-de-minimax)
    - [Poda Alfa-beta](#poda-alfa-beta)
      - [Implementación de la poda alfa-beta](#implementación-de-la-poda-alfa-beta)
      - [Propiedades de la poda alfa-beta](#propiedades-de-la-poda-alfa-beta)
    - [Expectimax](#expectimax)
      - [Implementación de Expectimax](#implementación-de-expectimax)
      - [Propiedades de Expectimax](#propiedades-de-expectimax)

## Problemas y espacio de estados

Los problemas de IA se modelan de manera simbólica y discreta, definiendo todas las configuraciones posibles (espacio de estados). El objetivo es encontrar una secuencia de acciones que lleve desde un estado inicial hasta un estado objetivo, minimizando el coste.

Un Espacio de Estados se define por una cuádrupla **N,A,I,F**:

- N: Conjunto de nodos (estados).
- A: Conjunto de arcos (acciones).
- I: Estados iniciales.
- F: Estados finales (objetivos).

## Métodos de búsqueda no informados (Uninformed Search Methods)

Los métodos de búsqueda no informados no tienen información sobre el problema, por lo que no pueden tomar decisiones basadas en el coste de las acciones.

Encuentran soluciones comparando nodos con el objetivo y expandiendo los nodos que no lo son.

Son increíblemente ineficientes, ya que no tienen en cuenta la información del problema.

Los tres son el mismo algoritmo, pero con diferentes estructuras de datos (frontera/fringe) para almacenar los nodos a visitar.

### Búsqueda en profundidad (Depth-First Search)

- Estrategia: Expande el nodo más profundo primero (último en ser accesible).
- Problema: Puede caer en caminos infinitamente largos.

#### Implementación iterativa

```pseudo
DFS(u)
    MIENTRAS haya elementos en la pila:
        nodo = sacar nodo de la pila
        SI nodo no ha sido visitado:
            SI nodo es objetivo:
                DEVOLVER nodo
            SI NO:
                Añadir nodo a visitados
                Añadir sucesores a la pila
```

```python
def depth_first_search(problem):
    visited = set()
    stack = [problem.initial_state]
    while stack:
        state = stack.pop()
        if problem.is_goal(state):
            return state
        visited.add(state)
        for action in problem.actions(state):
            new_state = problem.result(state, action)
            if new_state not in visited:
                stack.append(new_state)
    return None
```

> Se utiliza una **pila** para almacenar los nodos a visitar. Se añaden los nodos sucesores a la **pila**, y se comprueba si son el estado objetivo. Si no lo son, se añaden a la lista de visitados.

#### Implementación recursiva

```python
def depth_first_search(problem):
    return recursive_dfs(problem, problem.initial_state)

def recursive_dfs(problem, state):
    if problem.is_goal(state):
        return [state]
    for action in problem.actions(state):
        new_state = problem.result(state, action)
        if new_state not in problem.visited:
            problem.visited.add(new_state)
            path = recursive_dfs(problem, new_state)
            if path is not None:
                return [state] + path
    return None
```

> En vez de usar una pila, se llama recursivamente a la función `recursive_dfs` para cada nodo sucesor. Se añade el nodo actual a la lista de visitados. Es lo mismo dado que Python utiliza una pila para almacenar las llamadas a funciones.

#### Propiedades de DFS

- Complejidad en tiempo: O(b^m), donde b es el factor de ramificación y m la profundidad máxima.
- Complejidad en espacio: O(bm), donde b es el factor de ramificación y m la profundidad máxima.

### Búsqueda en anchura (Breadth-First Search)

- Estraegia: Expande el nodo más superficial primero (primero en ser accesible).
- Problema: Puede caer en caminos infinitamente anchos.

#### Implementación

```pseudo
BFS(u)
    MIENTRAS haya elementos en la cola:
        nodo = sacar nodo de la cola
        SI nodo no ha sido visitado:
            SI nodo es objetivo:
                DEVOLVER nodo
            SI NO:
                Añadir nodo a visitados
                Añadir sucesores a la cola
```

```python
def breadth_first_search(problem):
    visited = set()
    queue = [problem.initial_state]
    while queue:
        state = queue.pop(0)
        if problem.is_goal(state):
            return state
        visited.add(state)
        for action in problem.actions(state):
            new_state = problem.result(state, action)
            if new_state not in visited:
                queue.append(new_state)
    return None
```

> Se utiliza una **cola** para almacenar los nodos a visitar. Se añaden los nodos sucesores a la **cola**, y se comprueba si son el estado objetivo. Si no lo son, se añaden a la lista de visitados.

#### Propiedades de BFS

- Complejidad en tiempo: O(b^s), donde b es el factor de ramificación y s la profundidad de la solución más corta.
- Complejidad en espacio: O(b^s), donde b es el factor de ramificación y s la profundidad de la solución más

### Búsqueda de coste uniforme (Uniform-Cost Search)

- Estrategia: Expande el nodo con menor coste acumulado.
- Problema: Puede caer en caminos con costes infinitos.

#### Implementación de UCS

```pseudo
UCS(u)
    MIENTRAS haya elementos en la cola de prioridad:
        nodo = sacar nodo de la cola de prioridad
        SI nodo no ha sido visitado:
            SI nodo es objetivo:
                DEVOLVER nodo
            SI NO:
                Añadir nodo a visitados
                Añadir sucesores a la cola de prioridad
```

```python
def uniform_cost_search(problem):
    visited = set()
    queue = [(0, problem.initial_state)]
    while queue:
        cost, state = queue.pop(0)
        if problem.is_goal(state):
            return state
        visited.add(state)
        for action in problem.actions(state):
            new_state = problem.result(state, action)
            new_cost = cost + problem.cost(state, action)
            if new_state not in visited:
                queue.append((new_cost, new_state))
                queue.sort()
    return None
```

> Se utiliza una **cola de prioridad** para almacenar los nodos a visitar. Se añaden los nodos sucesores a la **cola de prioridad**, y se comprueba si son el estado objetivo. Si no lo son, se añaden a la lista de visitados.

#### Propiedades de UCS

- Complejidad en tiempo: O(b^(C*/e)), donde b es el factor de ramificación, C* el coste de la solución óptima y e el menor coste de una acción.
- Complejidad en espacio: O(b^(C*/e)), donde b es el factor de ramificación, C* el coste de la solución óptima y e el menor coste de una acción.

## Metodos de búsqueda informados (Informed Search Methods)

Los métodos de búsqueda informados utilizan información sobre el problema para tomar decisiones.

Esta información se conoce como **heurística**, y se utiliza para estimar el coste de llegar al objetivo.

La **heurística** es una función que estima el coste de llegar al objetivo desde un estado dado y debe ser diseñado específicamente para cada problema.

Para que un algoritmo de búsqueda informado sea óptimo, la heurística debe ser **admisible** y **consistente**.

- Una heurística es **admisible** si nunca sobreestima el coste de llegar al objetivo.
- Una heurística es **consistente** si para cada nodo n y cada sucesor n' de n generado por cualquier acción a, el coste estimado de llegar al objetivo desde n no es mayor que el coste de llegar a n' más el coste de aplicar a.

### Búsqueda voraz (Greedy Search)

- Estrategia: Expande el nodo que parece más cercano al objetivo.
- Problema: Puede caer en caminos no óptimos.

### Búsqueda A Star (A* Search)

- Estrategia: Expande el nodo con menor coste acumulado y menor coste estimado al objetivo. (Combinación de UCS y Greedy)

## Arboles de juego

Varias cosas a tener en cuenta en los juegos:

- **Determinístico**: El resultado de una acción es predecible.
- **Estocástico**: El resultado de una acción es aleatorio.
- **Suma cero**: La ganancia de un jugador es la pérdida del otro.
- **No suma cero**: La ganancia de un jugador no es la pérdida del otro.
- **Perfecto**: Los jugadores conocen el estado del juego en todo momento.
- **Imperfecto**: Los jugadores no conocen el estado del juego en todo momento.

## Búsqueda adversarial

### Minimax

El algoritmo Minimax se utiliza en juegos determinísticos de suma cero:

- Se construye un árbol de búsqueda donde los jugadores alternan turnos.
- El valor Minimax de cada nodo se calcula como el mejor resultado que un jugador puede obtener contra un oponente racional.
- Los jugadores "MAX" intentan maximizar el valor de la utilidad, mientras que los jugadores "MIN" intentan minimizarlo.

#### Implementación de Minimax

```pseudo
minimax(u, agente, profundidad)
    SI u es terminal o profundidad = 0:
        DEVOLVER utilidad(u)
    SI agente = MAX:
        DEVOLVER max_value(u, profundidad)
    SI NO:
        DEVOLVER min_value(u, profundidad)

max_value(u, profundidad)
    mejor_valor = -inf
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = minimax(nuevo_estado, MIN, profundidad - 1)
        mejor_valor = max(mejor_valor, valor)
    DEVOLVER mejor_valor

min_value(u, profundidad)
    mejor_valor = inf
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = minimax(nuevo_estado, MAX, profundidad - 1)
        mejor_valor = min(mejor_valor, valor)
    DEVOLVER mejor_valor
```

```python
def minimax(state, agent, depth):
    if state.is_terminal() or depth == 0:
        return state.utility()
    if agent == 'MAX':
        return max_value(state, depth)
    else:
        return min_value(state, depth)

def max_value():
    best_value = -inf
    for action in state.actions():
        new_state = state.result(action)
        value = minimax(new_state, 'MIN', depth - 1)
        best_value = max(best_value, value)
    return best_value

def min_value():
    best_value = inf
    for action in state.actions():
        new_state = state.result(action)
        value = minimax(new_state, 'MAX', depth - 1)
        best_value = min(best_value, value)
    return best_value
```

#### Propiedades de Minimax

- Óptimo: Contra un oponente perfecto, Minimax garantiza el mejor resultado posible.
- Eficiencia:
  - Tiempo: O(b^m), donde b es el factor de ramificación y m es la profundidad máxima.
  - Espacio: O(b^m), lo que puede ser ineficiente en juegos como el ajedrez donde el árbol de búsqueda puede ser enorme.

La eficiencia de Minimax como podemos ver es la misma que la de DFS ya que es un algoritmo de búsqueda en profundidad.

Aunque Minimax es exhaustivo y garantiza el mejor resultado, su uso en juegos complejos es inviable sin optimizaciones como la poda alfa-beta.

### Poda Alfa-beta

El podado Alpha-Beta mejora la eficiencia del algoritmo Minimax al eliminar ramas del árbol que no necesitan ser exploradas, sin afectar el valor minimax calculado.

- **Alfa**: El mejor valor encontrado por el jugador MAX hasta el momento.
- **Beta**: El mejor valor encontrado por el jugador MIN hasta el momento.

#### Implementación de la poda alfa-beta

```pseudo
alfa_beta(u, agente, alfa, beta, profundidad)
    SI u es terminal o profundidad = 0:
        DEVOLVER utilidad(u)
    SI agente = MAX:
        DEVOLVER max_value(u, alfa, beta, profundidad)
    SI NO:
        DEVOLVER min_value(u, alfa, beta, profundidad)

max_value(u, alfa, beta, profundidad)
    mejor_valor = -inf
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = alfa_beta(nuevo_estado, MIN, alfa, beta, profundidad - 1)
        mejor_valor = max(mejor_valor, valor)
        alfa = max(alfa, mejor_valor)
        SI beta <= alfa:
            ROMPER
    DEVOLVER mejor_valor

min_value(u, alfa, beta, profundidad)
    mejor_valor = inf
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = alfa_beta(nuevo_estado, MAX, alfa, beta, profundidad - 1)
        mejor_valor = min(mejor_valor, valor)
        beta = min(beta, mejor_valor)
        SI beta <= alfa:
            ROMPER
    DEVOLVER mejor_valor
```

```python
def alpha_beta(state, agent, alpha, beta, depth):
    if state.is_terminal() or depth == 0:
        return state.utility()
    if agent == 'MAX':
        return max_value(state, alpha, beta, depth)
    else:
        return min_value(state, alpha, beta, depth)

def max_value(state, alpha, beta, depth):
    best_value = -inf
    for action in state.actions():
        new_state = state.result(action)
        value = alpha_beta(new_state, 'MIN', alpha, beta, depth - 1)
        best_value = max(best_value, value)
        alpha = max(alpha, best_value)
        if beta <= alpha:
            break
    return best_value

def min_value(state, alpha, beta, depth):
    best_value = inf
    for action in state.actions():
        new_state = state.result(action)
        value = alpha_beta(new_state, 'MAX', alpha, beta, depth - 1)
        best_value = min(best_value, value)
        beta = min(beta, best_value)
        if beta <= alpha:
            break
    return best_value
```

#### Propiedades de la poda alfa-beta

- El podado no afecta el valor minimax calculado para la raíz.
- Mejora la eficiencia, reduciendo la complejidad a O(b^(m/2)) con una ordenación perfecta de los nodos.

Con una buena ordenación, el podado Alpha-Beta puede doblar la profundidad explorada en el árbol.

### Expectimax

Mientras que Minimax supone que siempre se tomará la mejor decisión, Expectimax asume que los oponentes pueden toman decisiones aleatorias.

Expectimax calcula las utilidades esperadas mediante una media ponderada de los posibles resultados.

#### Implementación de Expectimax

```pseudo
expectimax(u)
    SI u es terminal:
        DEVOLVER utilidad(u)
    SI agente = MAX:
        DEVOLVER max_value(u)
    SI NO:
        DEVOLVER expect_value(u)

max_value(u)
    mejor_valor = -inf
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = expectimax(nuevo_estado)
        mejor_valor = max(mejor_valor, valor)
    DEVOLVER mejor_valor

expect_value(u)
    total_valor = 0
    PARA cada acción en acciones(u):
        nuevo_estado = resultado(u, acción)
        valor = expectimax(nuevo_estado)
        total_valor += valor
    DEVOLVER total_valor / acciones(u)
```

```python
def expectimax(state):
    if state.is_terminal():
        return state.utility()
    if state.agent == 'MAX':
        return max_value(state)
    else:
        return expect_value(state)

def max_value(state):
    best_value = -inf
    for action in state.actions():
        new_state = state.result(action)
        value = expectimax(new_state)
        best_value = max(best_value, value)
    return best_value

def expect_value(state):
    total_value = 0
    for action in state.actions():
        new_state = state.result(action)
        value = expectimax(new_state)
        total_value += value
    return total_value / len(state.actions())
```

#### Propiedades de Expectimax
