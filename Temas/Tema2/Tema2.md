# 2. - Agentes

- [2. - Agentes](#2---agentes)
  - [2.1 - Agentes](#21---agentes)
  - [2.2 - Problemas de búsqueda](#22---problemas-de-búsqueda)
    - [Espacio de estados](#espacio-de-estados)
  - [2.3 - Ejemplos](#23---ejemplos)
    - [2.3.1 - Aspiradora](#231---aspiradora)
    - [2.3.2 - Tres en raya](#232---tres-en-raya)
    - [2.3.3 - Otro tablero](#233---otro-tablero)

## 2.1 - Agentes

Un agente es cualquier cosa capaz de percibir su entorno a través de sensores y actuar sobre él a través de actuadores.

Un agente racional elige acciones que maximizan la utilidad esperada.

El tipo de entorno determina en gran parte el diseño del agente.

Tipo de entornos:

Tipos de agentes:

- Agente reflex
  - Eligen la accion basandose en la percepcion actual
  - Pueden tener memoria
  - No consideran el futuro
  - No son racionales
- Agente planificador
  - Eligen la accion basandose en la secuencia de percepciones, se plantean hipótesis.
  - Pueden tener memoria
  - Consideran el futuro
  - Son racionales
- Agentes recalificadores
  - Empiezan siendo reflex y luego se convierten en planificadores, punto intermedio.

## 2.2 - Problemas de búsqueda

Un problema de busqueda tiene tres componentes:

- El espacio de estados
- Una función de sucesión
- Un estado inicial

Una solución es una secuencia de acciones que llevan del estado inicial a un estado objetivo.

### Espacio de estados

Son todas las posibles configuraciones del mundo. Cada estado es una configuración del mundo.

## 2.3 - Ejemplos

### 2.3.1 - Aspiradora

- 2 posiciones
- 2² combinaciones de limpieza de las casillas

2x4 = 8 estados posibles

### 2.3.2 - Tres en raya

- 9! = 362.880 estados posibles
- Hay que considerar los que cumplen el tres en raya

9! - (Los tres en raya)

### 2.3.3 - Otro tablero

- 4 posiciones de la pelota
- 4 posicion de el cubo

4x4 = 16 estados posibles
