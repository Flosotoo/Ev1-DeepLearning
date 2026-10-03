# Ev1-DeepLearning

# Clasificación de imágenes CIFAR-10 con un Perceptrón Multicapa (MLP)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/[USUARIO]/[REPOSITORIO]/blob/main/DLY0100_Deep_Learning_Parcial_1_Jara_Soto.ipynb)

Evaluación Parcial N°1 — Deep Learning (DLY0100), Duoc UC.

**Integrantes:** Florencia Soto · Antonio Jara
**Docente:** Matías Rojas
**Fecha:** 2 de octubre de 2026

## Descripción

Implementación y evaluación de una red neuronal de tipo perceptrón multicapa (MLP) para clasificar
imágenes a color del dataset CIFAR-10 en 10 categorías. El trabajo incluye experimentos controlados
sobre tasa de aprendizaje, tamaño de batch, arquitectura, funciones de activación, funciones de
pérdida y optimizadores, además del análisis del impacto de técnicas de regularización (Dropout,
L2 y Batch Normalization).

## Dataset

[CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html) (Krizhevsky, 2009), Universidad de Toronto.

| Característica | Valor |
| --- | --- |
| Imágenes de entrenamiento | 50 000 |
| Imágenes de prueba | 10 000 |
| Resolución | 32 × 32 píxeles, RGB |
| Clases | airplane, automobile, bird, cat, deer, dog, frog, horse, ship, truck |

El dataset **no está incluido en el repositorio** por su tamaño (~163 MB). Ver instrucciones de ejecución.

## Estructura del repositorio

```
├── DLY0100_Deep_Learning_Parcial_1_Jara_Soto.ipynb   # Notebook principal con código y análisis
├── resultados_experimentos.csv                       # Registro de todos los experimentos
├── README.md
└── .gitignore
```

## Cómo ejecutar el proyecto

1. Abrir el notebook en Google Colab con el botón **Open in Colab** de arriba.
2. Activar la GPU: `Entorno de ejecución → Cambiar tipo de entorno de ejecución → GPU (T4)`.
3. Obtener el dataset con **una** de estas opciones:
   - **Recomendada:** descargar `cifar-10-python.tar.gz` desde la
     [página oficial](https://www.cs.toronto.edu/~kriz/cifar.html) y subirlo al panel lateral
     de Archivos de Colab (carpeta `/content`).
   - **Automática:** si no se sube el archivo, el notebook lo descarga con `wget` desde el servidor
     oficial. Esta descarga puede tardar varios minutos.
4. Ejecutar todas las celdas en orden: `Entorno de ejecución → Ejecutar todas`.

> **Importante:** el almacenamiento de `/content` es temporal. Si la sesión de Colab se reinicia,
> es necesario volver a subir el archivo.

**Tiempo estimado de ejecución completa:** [X] minutos con GPU T4.

## Requisitos

El notebook está diseñado para Google Colab, que incluye todas las dependencias preinstaladas.
Versiones utilizadas:

| Librería | Versión |
| --- | --- |
| Python | [versión] |
| TensorFlow / Keras | [versión] / [versión] |
| NumPy | [versión] |
| scikit-learn | [versión] |
| pandas | [versión] |
| matplotlib | [versión] |

## Metodología

1. Carga, exploración y preprocesamiento (estandarización, codificación one-hot, división
   estratificada train / validación / test).
2. Definición de un modelo base: MLP de 3 capas ocultas (512-256-128), ReLU, softmax y
   entropía cruzada categórica.
3. Experimentos controlados variando un hiperparámetro a la vez, con semilla fija.
4. Aplicación y comparación de técnicas de regularización.
5. Evaluación final sobre el conjunto de prueba con accuracy, precision, recall y F1-score.

## Resultados principales

| Métrica (conjunto de prueba) | Valor |
| --- | --- |
| Accuracy | 54.99 % |
| Precision (macro) | 0.553 |
| Recall (macro) | 0.550 |
| F1-score (macro) | 0.547 |

**Configuración final:** MLP 512-256-128 · ReLU · Adam (lr = 0.001) · batch 128 ·
Dropout 0.3 · L2 = 10⁻⁴ · Batch Normalization · early stopping (paciencia 10).

**Hallazgos clave:**
- La tasa de aprendizaje fue el hiperparámetro más determinante: con lr = 0.01 el entrenamiento diverge.
- El dropout fue la técnica de regularización más efectiva (+6.3 puntos de val_accuracy).
- Aumentar la capacidad de la red no mejora la generalización: el límite lo impone el aplanado
  de la imagen, que destruye su estructura espacial.
- El desempeño (~55 %) se encuentra en el extremo superior del rango esperado para un MLP en
  CIFAR-10 (45–55 %).

## Reproducibilidad

Se fija la semilla (`SEMILLA = 42`) en NumPy y TensorFlow. Aun así, las operaciones de TensorFlow
en GPU no son completamente deterministas, por lo que al re-ejecutar el notebook los resultados
pueden variar en torno a 1 punto porcentual. El análisis del notebook corresponde a la ejecución
guardada en este repositorio.
