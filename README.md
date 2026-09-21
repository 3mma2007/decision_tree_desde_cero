# 🌳 Árbol de decisión desde cero (clasificación)

Implementación paso a paso de un **árbol de decisión para clasificación** usando solo **NumPy**, y comparación de sus resultados contra `DecisionTreeClassifier` de **scikit-learn**.

El objetivo del proyecto es entender qué hace un árbol de decisión por dentro: cómo mide la impureza, cómo elige la mejor división y cómo predice recorriendo el árbol.

## 📋 Contenido

- Dataset: **Iris** (150 muestras, 4 features, 3 clases)
- Criterio de impureza: **Gini**
- Implementación propia con recursividad y una clase `node`
- Comparación con scikit-learn (accuracy y matrices de confusión lado a lado)

## 🧠 Cómo funciona

| Paso | Función | Qué hace |
|------|---------|----------|
| 1 | `gini(y)` | Mide la impureza de un conjunto de etiquetas: `1 - Σ pᵢ²` |
| 2 | `decision(x, y, feature, threshold)` | Separa las muestras: izquierda si `x[feature] <= threshold`, derecha si no |
| 3 | `gini_division(y_left, y_right)` | Gini ponderado por el tamaño de cada lado después de dividir |
| 4 | `mejor_division(x, y)` | Prueba todas las features y umbrales y devuelve la división con menor Gini |
| 5 | `node` | Clase que guarda la feature, el umbral, los hijos y, si es hoja, la clase (`valor`) |
| 6 | `decision_tree(x, y, profundidad, max_depth)` | Construye el árbol de forma recursiva |
| 7 | `predict_one` / `predict` | Recorre el árbol desde la raíz para predecir una o varias muestras |

**Condiciones de parada** al construir el árbol (el nodo se vuelve hoja):

1. Todas las muestras del nodo son de la misma clase.
2. Se alcanza la profundidad máxima (`max_depth`).
3. No se encuentra una división válida.

La clase de una hoja es la **clase mayoritaria** de las muestras que llegan a ella.

> El árbol completo es simplemente su **nodo raíz**: cada nodo guarda referencias a sus hijos, y por eso `predict` le pasa la raíz a `predict_one`, que baja por las ramas de forma recursiva.

## 📊 Resultados

Partición 80 % entrenamiento / 20 % prueba (`random_state=42`, `stratify=y`) y `max_depth=3`:

| Modelo | Accuracy |
|--------|----------|
| Implementación propia | **0.9667** |
| scikit-learn | **0.9667** |

Las predicciones de ambos modelos son idénticas sobre el conjunto de prueba. Matriz de confusión (filas = clase real, columnas = predicción):

|  | setosa | versicolor | virginica |
|--|:--:|:--:|:--:|
| **setosa** | 10 | 0 | 0 |
| **versicolor** | 0 | 9 | 1 |
| **virginica** | 0 | 0 | 10 |

El único error (1 de 30 muestras) es una versicolor clasificada como virginica, las dos clases que más se parecen entre sí.

## 📁 Estructura

```
.
├── decision_tree_desde_cero_para_clasificacion.ipynb
└── README.md
```

## 🛠️ Tecnologías

Python · NumPy · scikit-learn · Matplotlib
