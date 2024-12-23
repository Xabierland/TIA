# Aprendizaje Supervisado

- [Aprendizaje Supervisado](#aprendizaje-supervisado)
  - [Introducción](#introducción)
  - [Algoritmos Clásicos](#algoritmos-clásicos)
    - [Perceptrón Binario](#perceptrón-binario)
      - [Definición](#definición)
      - [Algoritmo](#algoritmo)
      - [Pseudocódigo](#pseudocódigo)
    - [Perceptrón Multiclase](#perceptrón-multiclase)
  - [Funciones de Activación y Pérdida](#funciones-de-activación-y-pérdida)
    - [Funciones de Activación](#funciones-de-activación)
      - [Sigmoide](#sigmoide)
      - [Softmax](#softmax)
    - [Funciones de Pérdida o Coste](#funciones-de-pérdida-o-coste)
      - [Cross-Entropy](#cross-entropy)
      - [MSE (Mean Squared Error)](#mse-mean-squared-error)
  - [Optimización y Actualización de Pesos](#optimización-y-actualización-de-pesos)
    - [Perceptrón por defecto](#perceptrón-por-defecto)
    - [Averaged Perceptron](#averaged-perceptron)
    - [MIRA (Margin Infused Relaxed Algorithm)](#mira-margin-infused-relaxed-algorithm)
    - [ReLU (Rectified Linear Unit)](#relu-rectified-linear-unit)
    - [Early Stopping](#early-stopping)
    - [Regularización](#regularización)
    - [Dropout](#dropout)
    - [Learning Rate](#learning-rate)
    - [Stochastic Gradient Descent (SGD)](#stochastic-gradient-descent-sgd)
    - [Batch Gradient Descent](#batch-gradient-descent)
    - [Mini-Batch Gradient Descent](#mini-batch-gradient-descent)
  - [Aprendizaje secuencial](#aprendizaje-secuencial)

## Introducción

El aprendizaje supervisado consiste en entrenar un modelo usando datos etiquetados. El objetivo es encontrar una función que, dada una entrada, prediga una salida.

Los componentes básicos de un sistema de aprendizaje supervisado son:

- Entrada (features): Son las variables de entrada que se utilizan para hacer predicciones.
- Salida (target o label): Es el valor o categoría que se desea predecir.
- Hipótesis: Es la función que se ajusta a los datos de entrenamiento.

Los usos principales del aprendizaje supervisado son:

- Clasificación: Se utiliza cuando la salida es una etiqueta o categoría.
- Regresión: Se utiliza cuando la salida es un valor numérico.

## Algoritmos Clásicos

### Perceptrón Binario

#### Definición

El perceptrón es un algoritmo de clasificación binaria que se utiliza para separar dos clases linealmente.

#### Algoritmo

El algoritmo del perceptrón en su forma más simple es:

1. Inicializar los pesos y sesgos aleatoriamente
2. Calculamos el producto escalar de las entradas y los pesos más el sesgo
3. Clasificamos el resultado usando una función de activación (sigmoide para binario)
4. Actualizamos los pesos si la clasificación es incorrecta.
5. Repetimos los pasos 2-4 hasta que no haya errores o se alcance un número máximo de iteraciones.

#### Pseudocódigo

```pseudo
w = random()
while not converge do
  prodEsc = dotProduct(x, w) + b
  resultado = sigmoide(prodEsc)
  si resultado != y entonces
    actualizarPesos(w)
end while
```

### Perceptrón Multiclase

## Funciones de Activación y Pérdida

### Funciones de Activación

La función de activación introduce no linealidad en una red neuronal y determina la salida de una neurona.
Convierte la combinación lineal de las entradas y los pesos en una salida útil para la siguiente capa o para la predicción final.

Sin funciones de activación, una red neuronal sería simplemente una combinación lineal de las entradas, limitando su capacidad para resolver problemas complejos o no lineales.

#### Sigmoide

La función Sigmoide se define como:

$$ f(x) = \frac{1}{1 + e^{-x}} $$

Donde:

- $x$ es el vector de entrada.

Comprime la salida en el rango [0, 1]. Útil para **clasificación binaria**.

#### Softmax

La función Softmax se define como:

$$ f(x)_i = \frac{e^{x_i}}{\sum_{j=1}^{n} e^{x_j}} $$

Donde:

- $x$ es el vector de entrada.
- $n$ es el número de clases.
- $i$ es la clase a la que se quiere asignar la probabilidad.
- $j$ es el índice de la clase.

Convierte los valores en probabilidades. Útil para **clasificación multiclase**.

### Funciones de Pérdida o Coste

La función de pérdida mide cuánto de buena (o mala) es la predicción del modelo comparándola con los valores reales (etiquetas).
El objetivo es minimizar esta función durante el entrenamiento.

La función de pérdida guía el proceso de aprendizaje. Si el error es alto, los pesos de la red se actualizan más significativamente. Si el error es pequeño, el ajuste es mínimo.

#### Cross-Entropy

**Binaria:**

$$ L(y, \hat{y}) = -y \log(\hat{y}) - (1 - y) \log(1 - \hat{y}) $$

o

$$ L(y, \hat{y}) = -\frac{1}{n} [ y_i \ln(\hat{y}_i) + (1 - y_i) \ln(1 - \hat{y}_i) ]$$ 

**Multiclase:**

$$ L(y, \hat{y}) = -\sum_{i=1}^{n} y_i \log(\hat{y}_i) $$

o

$$ L(y, \hat{y}) = -\frac{1}{n} \sum_{i=1}^{n} y_i \ln(\hat{y}_i) $$

Donde:

- $y$ es la etiqueta real.
- $\hat{y}$ es la predicción del modelo. Tambien se le llama $\sigma$ en el caso de que sea el valor obtenido tras aplicar la función de activación.
- $n$ es el número de clases.

> [!WARNING]
> En internet y segun ChatGPT, la formula correcta es la primera pero en las transparencias de clase aparece la segunda.

#### MSE (Mean Squared Error)

$$ L(y, \hat{y}) = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 $$

o

$$ L(y, \hat{y}) = \frac{1}{2n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2 $$

- $y$ es la etiqueta real.
- $\hat{y}$ es la predicción del modelo. Tambien se le llama $\sigma$ en el caso de que sea el valor obtenido tras aplicar la función de activación.
- $n$ es el número de clases.

> [!WARNING]
> En internet y segun ChatGPT, la formula correcta es la primera pero en las transparencias de clase aparece la segunda.

## Optimización y Actualización de Pesos

### Perceptrón por defecto

El metodo de actualización de pesos del perceptrón es:

$$ w = w + \alpha (y - \hat{y}) x $$

Donde:

- $w$ es el vector de pesos.
- $\alpha$ es el learning rate.
- $y$ es la etiqueta real.
- $\hat{y}$ es la predicción del modelo.
- $x$ es el vector de entrada.

De esta forma, los pesos se actualizan en función del error cometido en la predicción.

### Averaged Perceptron

A diferencia del perceptrón estándar, que utiliza el último conjunto de pesos al final del entrenamiento, el Averaged Perceptron promedia los pesos obtenidos a lo largo de todas las iteraciones del entrenamiento o durante las propias iteraciones.

De esta forma conseguimos solucionar problemas como **datos ruidosos** o **sobreajuste**

### MIRA (Margin Infused Relaxed Algorithm)

MIRA es una extensión del perceptrón que introduce el concepto de margen para asegurar que las predicciones no solo sean correctas, sino que estén bien separadas de la frontera de decisión. Esto permite crear modelos más estables y con mejor capacidad de generalización.

### ReLU (Rectified Linear Unit)

La función ReLU  se define como:

$$ f(x) = \max(0, x) $$

Se utiliza para introducir no linealidades en la red. Evita que los valores de salida sean negativos. Se puede modificar para que tenga un valor mínimo distinto de 0 e incluso que a partir de un cierto valor tome los valores de otra función en vez de tomar el valor nulo.

Desplazamiento de ReLU:

$$ f(x) = \max(0, x - \theta) $$

Sumar una función lineal a ReLU:

$$ f(x) = \max(0, x) + g(x) $$

### Early Stopping

Como indica su nombre, el Early Stopping es una técnica que detiene el entrenamiento de la red antes de que se alcance el número máximo de iteraciones. Se basa en la observación de que, a medida que la red se entrena, el error en el conjunto de validación disminuye hasta un punto, a partir del cual comienza a aumentar. Este punto es el que se utiliza para detener el entrenamiento.

De esta forma, evitamos el sobreajuste y conseguimos un modelo más generalizable.

### Regularización

La regularización es una técnica que se utiliza para evitar el sobreajuste en los modelos de aprendizaje supervisado. Consiste en añadir un término de penalización a la función de coste que penaliza los pesos de la red.

Existen varios tipos de regularización, como L1, L2 o ElasticNet, que se diferencian en la forma en que penalizan los pesos.

- L1: Penaliza los pesos de la red en función de su valor absoluto.
- L2: Penaliza los pesos de la red en función de su cuadrado.
- ElasticNet: Combinación de L1 y L2.

### Dropout

Dropout es una técnica de regularización que consiste en desactivar aleatoriamente un porcentaje de las neuronas de la red durante el entrenamiento.

De esta forma, se evita el sobreajuste y se consigue un modelo más robusto y generalizable.

### Learning Rate

El Learning Rate es un hiperparámetro que controla la cantidad de ajuste que se realiza en los pesos de la red durante el entrenamiento.

### Stochastic Gradient Descent (SGD)

El Stochastic Gradient Descent es una variante del Gradient Descent que actualiza los pesos de la red después de cada muestra de entrenamiento.

No permite vectorización y es más lento que el Batch Gradient Descent, pero es más eficiente en términos de memoria y puede converger más rápidamente.

### Batch Gradient Descent

El Batch Gradient Descent es una variante del Gradient Descent que actualiza los pesos de la red después de procesar todo el conjunto de entrenamiento.

Es más rápido que el Stochastic Gradient Descent, pero requiere más memoria y puede converger más lentamente.

### Mini-Batch Gradient Descent

El Mini-Batch Gradient Descent es una variante del Gradient Descent que actualiza los pesos de la red después de procesar un subconjunto del conjunto de entrenamiento.

Combina las ventajas del Stochastic Gradient Descent y el Batch Gradient Descent, siendo más rápido que el Batch Gradient Descent y más eficiente en términos de memoria que el Stochastic Gradient Descent.

## Aprendizaje secuencial
