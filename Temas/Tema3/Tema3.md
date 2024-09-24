# 3. - Espacio de estados y búsqueda

- [3. - Espacio de estados y búsqueda](#3---espacio-de-estados-y-búsqueda)
  - [3.1. Problemas y espacio de estados](#31-problemas-y-espacio-de-estados)
  - [3.2. Metodos de busqueda no informados](#32-metodos-de-busqueda-no-informados)
    - [3.2.1. Busqueda en profundidad (Depth-First Search)](#321-busqueda-en-profundidad-depth-first-search)
    - [3.2.2. Busqueda en anchura (Breadth-First Search)](#322-busqueda-en-anchura-breadth-first-search)
    - [3.2.3. Busqueda de coste uniforme (Uniform-Cost Search)](#323-busqueda-de-coste-uniforme-uniform-cost-search)
  - [3.3. Metodos de busqueda informados](#33-metodos-de-busqueda-informados)
    - [3.3.1. Heuristicos](#331-heuristicos)
      - [3.3.1.1 Busqueda voraz](#3311-busqueda-voraz)
      - [3.3.1.2 Busqueda A\*](#3312-busqueda-a)
  - [3.4. Busqueda adversarial](#34-busqueda-adversarial)
    - [3.4.1. Minmax](#341-minmax)
    - [3.4.2. Pda alfa-beta](#342-pda-alfa-beta)
    - [3.4.3. Expectimax](#343-expectimax)

## 3.1. Problemas y espacio de estados

- Tamaño del espacio de estados: Cuantas configuraciones posibles existen.
- Factor de ramificación: Cuantas acciones como maximo se pueden llevar a cabo en un estado
  - Ejemplo: El pacman puede moverse en 4 direcciones, por lo que el factor de ramificación es 4

Un problema de grafos se puede representar como un arbol de busqueda, donde cada nodo es un estado y cada arista es una accion. Tenenemos que tener en cuenta que el arbol de busqueda puede ser infinito, por lo que no se deben revisitarse nodos ya visitados. para evitar bucles.

## 3.2. Metodos de busqueda no informados

Los metodos de busqueda no informados son aquellos que no tienen informacion sobre el problema, por lo que se basan en la exploracion de todos los nodos del arbol de busqueda. Estos metodos son muy costosos en terminos de tiempo y memoria, por lo que no son muy eficientes.

### 3.2.1. Busqueda en profundidad (Depth-First Search)

La busqueda en profundidad es un algoritmo de busqueda no informado que se basa en explorar todos los nodos de un arbol de busqueda hasta encontrar la solucion. Para ello, se va explorando un camino hasta llegar a un nodo hoja, y si no se ha encontrado la solucion, se retrocede y se explora otro camino.

```python
def dfs(graph, start, goal):
    stack = [(start, [start])]
    while stack:
        (vertex, path) = stack.pop()
        for next in graph[vertex] - set(path):
            if next == goal:
                yield path + [next]
            else:
                stack.append((next, path + [next]))
```

Este algoritmo tiene una complejidad de O(b^m), donde b es el factor de ramificación y m es la profundidad del arbol de busqueda. Por lo que es un algoritmo muy costoso en terminos de tiempo y memoria.

### 3.2.2. Busqueda en anchura (Breadth-First Search)

La busqueda en anchura es un algoritmo de busqueda no informado que se basa en explorar todos los nodos de un arbol de busqueda a la misma profundidad antes de explorar los nodos de la siguiente profundidad. Para ello, se va explorando todos los nodos de un nivel antes de pasar al siguiente nivel.

```python
def bfs(graph, start, goal):
    queue = [(start, [start])]
    while queue:
        (vertex, path) = queue.pop(0)
        for next in graph[vertex] - set(path):
            if next == goal:
                yield path + [next]
            else:
                queue.append((next, path + [next]))
```

Este algoritmo tiene una complejidad de O(b^s), donde b es el factor de ramificación y s es la profundidad de la solucion del arbol de busqueda. Por lo que es un algoritmo mas eficiente que la busqueda en profundidad.

### 3.2.3. Busqueda de coste uniforme (Uniform-Cost Search)

La busqueda de coste uniforme es un algoritmo de busqueda no informado que se basa en explorar todos los nodos de un arbol de busqueda en funcion del coste de los nodos. Para ello, se va explorando los nodos con menor coste antes de explorar los nodos con mayor coste.

```python
def ucs(graph, start, goal):
    queue = [(0, start, [start])]
    while queue:
        (cost, vertex, path) = heapq.heappop(queue)
        for next in graph[vertex] - set(path):
            if next == goal:
                yield path + [next]
            else:
                heapq.heappush(queue, (cost + 1, next, path + [next]))
```

El heap es una estructura de datos que permite mantener los elementos ordenados en funcion de una prioridad. En este caso, el heap se ordena en funcion del coste de los nodos.

Este algoritmo tiene una complejidad de O(b^E/C), donde b es el factor de ramificación, E es el coste de la solucion y C es el coste de la arista. Por lo que es un algoritmo mas eficiente que la busqueda en anchura.

## 3.3. Metodos de busqueda informados

Los metodos de busqueda informados son aquellos que tienen informacion sobre el problema, por lo que se basan en la exploracion de los nodos del arbol de busqueda que tienen mas probabilidad de llevar a la solucion. Estos metodos son mas eficientes que los metodos de busqueda no informados, ya que permiten reducir el espacio de busqueda.

### 3.3.1. Heuristicos

Un heuristico es una funcion que estima el coste de llegar a la solucion desde un nodo dado. Los heuristicos se utilizan en los metodos de busqueda informados para guiar la exploracion de los nodos del arbol de busqueda. Los heuristicos se pueden clasificar en dos tipos:

#### 3.3.1.1 Busqueda voraz

#### 3.3.1.2 Busqueda A*

## 3.4. Busqueda adversarial

### 3.4.1. Minmax

### 3.4.2. Pda alfa-beta

### 3.4.3. Expectimax
