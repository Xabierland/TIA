# 1. Agentes

- [1. Agentes](#1-agentes)
  - [1.1. Agentes y Entornos](#11-agentes-y-entornos)
    - [1.1.1. Agentes Reflex vs. Agentes que Planifican vs. Agentes que Replanifican](#111-agentes-reflex-vs-agentes-que-planifican-vs-agentes-que-replanifican)
  - [1.2. Problemas de Búsqueda](#12-problemas-de-búsqueda)
  - [1.3. Tamaño del Espacio de Estados](#13-tamaño-del-espacio-de-estados)
  - [1.4. Representación de Búsqueda: Grafos y Árboles](#14-representación-de-búsqueda-grafos-y-árboles)
  - [1.5. Ejemplos](#15-ejemplos)
    - [1.5.1. Aspiradora](#151-aspiradora)
    - [1.5.2. Tres en raya](#152-tres-en-raya)
    - [1.5.3. Otro tablero](#153-otro-tablero)

## 1.1. Agentes y Entornos

Un agente percibe su entorno a través de **sensores** y actúa sobre él usando **actuadores**.
El comportamiento de un agente se ve influido por su capacidad de observación y las características del entorno

- **Fully observable/partially observable**: Si el entorno es completamente observable, el agente no requiere memoria.
- **Discrete/continuous**: Si el entorno es continuo, el agente podría no ser capaz de enumerar todos los estados.
- **Stochastic/deterministic**: En un entorno estocástico, el agente debe estar preparado para contingencias.
- **Single-agent/multi-agent**: En entornos multi-agente, puede ser necesario usar comportamientos aleatorios.

### 1.1.1. Agentes Reflex vs. Agentes que Planifican vs. Agentes que Replanifican

- **Agentes Reflex**:
Actúan en función de la percepción actual sin considerar las consecuencias futuras de sus acciones.

- **Agentes que Planifican**:
Evalúan cómo evolucionará el mundo con base en las acciones que tomen. Requieren un modelo del mundo para hacer hipótesis y planificar de forma óptima.

- **Agentes que Replanifican**:
Empiezan con un plan y lo ajustan conforme se desarrolla la situación. Pueden ser más eficientes que los agentes que planifican, pero requieren más recursos.

## 1.2. Problemas de Búsqueda

Un problema de busqueda tiene tres componentes:

- El espacio de estados
- Una función de sucesión
- Un estado inicial

Una solución es una secuencia de acciones que llevan del estado inicial a un estado objetivo.

## 1.3. Tamaño del Espacio de Estados

El tamaño del espacio de estados varía en función de la complejidad del problema. Por ejemplo, en un problema de búsqueda donde el agente come todos los puntos, los estados podrían incluir.

- Posición del agente.
- Comidas restantes.
- Tiempo restante de susto para los fantasmas.

## 1.4. Representación de Búsqueda: Grafos y Árboles

- Grafos de espacio de estados: Los nodos representan configuraciones del mundo, y los arcos representan las acciones que llevan de un nodo a otro. El objetivo es un nodo en el grafo.

- Árboles de búsqueda: Representan planes y sus resultados en un formato jerárquico. El estado inicial es la raíz, y los nodos hijos son los sucesores.

## 1.5. Ejemplos

### 1.5.1. Aspiradora

- 2 posiciones
- 2² combinaciones de limpieza de las casillas

2x4 = 8 estados posibles

### 1.5.2. Tres en raya

- 9! = 362.880 estados posibles
- Hay que considerar los que cumplen el tres en raya

9! - (Los tres en raya)

### 1.5.3. Otro tablero

- 4 posiciones de la pelota
- 4 posicion de el cubo

4x4 = 16 estados posibles
