# Aprendizaje por Refuerzo (Reinforcement Learning)

- [Aprendizaje por Refuerzo (Reinforcement Learning)](#aprendizaje-por-refuerzo-reinforcement-learning)
  - [Introducción](#introducción)
  - [Multi-armed Bandit](#multi-armed-bandit)
  - [Markov Decision Process](#markov-decision-process)

## Introducción

El aprendizaje por refuerzo se basa en la teoría conductivista de Skinner. En este tipo de aprendizajes, un angete, en base a pruebas y errores, aprende a interactuar con un entorno guiado por una recompensa.

El agente a diferencia de otros aprendizajes como el supervisado no conoce la mejor acción a tomar pero si es capaz de valorar las acciones que toma y aprender de ellas.

Algunas características a tomar en cuenta en el RL son:

- El agente genera sus propios datos de entrenamiento mediante la interacción con el entorno.
  - El entorno es incierto, no es determinista. Existen probabilidades ocultas que desconocemos.
- No existe una accion correcta, el agente debe aprender a tomar la mejor decisión.
- El premio mayor puede ser a largo plazo.
  - El premio o recompensa indica lo bien o mal que ha sido la acción del agente. El objetivo es obtener la mayor acumulación de premios.
  - Por lo tanto, el objetivo es maximizar el cumulo de premios esperados.
- El tiempo y la secuencia de acciones son importantes.
  - Para ello debemos tomar una secuencia de acciones que mire el futuro, no unicamente el presente.

## Multi-armed Bandit

El problema del Multi-armed Bandit es un problema clásico en el aprendizaje por refuerzo. En este problema, el agente se enfrenta a una máquina tragaperras con varias palancas, cada una con una recompensa diferente. El objetivo del agente es encontrar la palanca que le proporciona la mayor recompensa.

En estos problemas hay una fase de exploración y una fase de explotación. En la fase de exploración el agente prueba todas las palancas para obtener información sobre las recompensas. En la fase de explotación el agente toma la mejor decisión basada en la información obtenida.

El agente debe empezar probando de forma aleatoria cada palanca y ir evaluando las recompensas obtenidas. A medida que obtiene información dejara de actuar de forma tan aleatoria y empezara a explotar la palanca que mejor recompensa le da según la información obtenida.

En estos problemas cada accion sobre el entorno no influye sobre el siguiente, es decir, no hay un estado que se vea afectado por la acción anterior. Usar mucho una palanca no cambia las probabilidades del resto de palancas.

## Markov Decision Process