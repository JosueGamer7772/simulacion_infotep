# Parcial de Simulación: Seguimiento de Objeto (Botella)

Este proyecto implementa el control de un humanoide en el simulador **Webots**, permitiendo que un objeto (una botella de cerveza) permanezca fijado a su mano derecha durante todo el movimiento, utilizando transformaciones de sistemas de referencia.

## 1. Modificaciones Realizadas

### i. Adición del Nodo de la Botella
Se agregó un nodo de tipo `BeerBottle` a la escena de Webots. Para poder manipularlo desde el código, se le asignó el DEF `beer_bottle`.

### ii. Implementación del Seguimiento Automático
Se creó la función `actualizar_posicion_botella()` dentro del controlador. Esta función se ejecuta en cada paso de la simulación (`supervisor.step`), asegurando que la botella se mueva y rote solidariamente con el humanoide.

---

## 2. Razonamiento Matemático y Lógico

El desafío principal fue convertir las coordenadas locales de la "mano" del humanoide a coordenadas globales del mundo de Webots, ya que el humanoide cambia su posición y ángulo constantemente.

### A. Traslación (Posición de la Botella)
Para ubicar la botella, se definió un **Offset** (distancia fija relativa al centro del humanoide). Sin embargo, si el humanoide rota, ese offset también debe rotar para mantener la posición relativa a la mano.

1. **Definición del Offset Local:** Se estableció un vector con la distancia aproximada del centro del cuerpo a la mano:
   $$d_{local} = \begin{bmatrix} OFFSET\_X \\ OFFSET\_Y \\ OFFSET\_Z \end{bmatrix}$$

2. **Cálculo en el Mundo:** Para hallar la posición global, multiplicamos la **Matriz de Rotación $R_z(\theta)$** del humanoide por el vector local y sumamos su posición actual ($P_{humanoide}$):
   $$P_{botella} = P_{humanoide} + (R_z(\theta) \cdot d_{local})$$

Donde la matriz de rotación en el eje Z es:
$$R_z(\theta) = \begin{bmatrix} \cos(\theta) & -\sin(\theta) & 0 \\ \sin(\theta) & \cos(\theta) & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

### B. Rotación (Orientación de la Botella)
Para que la botella no solo se traslade, sino que también rote cuando el humanoide gira:

1. Se tomó el eje Y local del humanoide y se transformó al sistema mundial mediante la misma matriz de rotación $R_z$.
2. Se aplicó un ángulo de inclinación constante de $1.571$ radianes ($90^\circ$) para que la botella se mantenga en posición horizontal (simulando que la sostiene).

