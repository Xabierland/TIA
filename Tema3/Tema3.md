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
    - [Búsqueda A\* (A\* Search)](#búsqueda-a-a-search)
  - [Búsqueda adversarial](#búsqueda-adversarial)
    - [Minimax](#minimax)
    - [Poda Alfa-beta](#poda-alfa-beta)
    - [Expectimax](#expectimax)

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

### Búsqueda voraz (Greedy Search)

### Búsqueda A* (A* Search)

## Búsqueda adversarial

### Minimax

### Poda Alfa-beta

### Expectimax