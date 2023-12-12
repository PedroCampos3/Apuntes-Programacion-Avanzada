##### Semana 15 - Estructuras con nodos

# 1. Listas Ligadas

## Nodo

Un nodo contiene datos y se puede identificar con una key; se puede relacionar con otros nodos (Nodos vecinos). Lo vamos a usar para varias estructuras de datos

## Lista ligada

Es un conjunto de nodos que están conectados entre si linealmente, el primero se llama Head y el último, tail.

Para usarlo, tenemos que crear la Clase Nodo y que un atributo sea el siguiente nodo (El tail tendrá `None`)

E.G.
```python
class Nodo:
    def __init__(self, valor):
        self.valor = valor
        self.siguiente = None
```

Ahora si, la lista ligada, le ponemos como atributos los nodos head y tail.

```python
class ListaLigada:
    def __init__(self):
        self.head = None
        self.tail = None
```

Para que funcione, debemos implementar mínimo 3 métodos:
1. **Agregar Nodo**(valor): Agrega un nodo **nuevo** al final (con su valor) y actualiza el tail (Similar al Append de las list)
2. **Agregar en posición** (valor, posición): Agrega un nodo **nuevo** en medio de 2 nodos (En la posición indicada)
3. **Consultar Nodo**: Retorna el valor del nodo que está en la posición indicada
4. `__repr__`: Retorna una representación de la lista.


# 2. Grafos y Representación

## Grafos

Los grafos son una conexion entre `vertices` que pueden estar dirigidos o no. A cada conexión (relación) se le llama `arista` y esta puede tener peso o no

Hay varias formas de implementar los grafos en python:
1. **Con Nodos**: Se hace una clase nodo que guarda 2 variables, el valor del nodo y una lista con los otros nodos con los que está conectado. Se le puede agregar otros nodos con un método que tenemos que crear.
2. **Con listas de adyacencia**: Cada key representa un vertice y el valor de esta son los vertices con los que está relacionado.
Ventaja: Podemos saber fácilmente sus vecinos solo llamando a una key
3. **Con Matrices de adyacencia**: Es una matriz en la que cada lista es un vertice y los valores de esa lista son o 1 o 0 dependiendo de a que nodo esté conectado.
Ventaja: Es fácil de visualizar
Desventaja: Para grafos muy grandes, se agranda mucho una matriz que mayoritariamente tendrá solo 0.

E.g.:

```python
grafo = [
    [0, 0, 0, 1, 0],
    [0, 1, 0, 0, 0],
    [1, 0, 0, 0, 1]
    ]
```

# 3. Operaciones Básicas.

Ahora agregaremos operaciones básicas a nuestro grafo; como ejemplo haremos nuestro grafo como una lista de adyacencia.
Las operaciones serán:

- `adyacentes(G, x, y)`: Comprueba si dos vertices (`x` e `y`) están relacionados
- `vecinos(G, x)`: Entrega todos los vertices con los que está relacionado el vertice `x`
- `agregar_vertice(G, x)`: Agrega un vertice sin relaciones al grafo.
- `remover_vertice(G, x)`: Remueve el vertice indicado, y las relaciones que tenía con otros vértices
- `agregar_arista(G, x, y)`: Agrega una relacion entre dos vertices
- `remover_arista(G, x, y)`: Remueve una relación entre dos vertices.

# 4. Stacks y Colas

## Stack

Un stack (pila) es otra estructura de datos y su comportamiento es de First In - Last Out

Tiene algunas operaciones como para sacar el elemento del tope, apilar otro elemento, conocer sin sacar el elemento del tope, saber su largo, etc.

Podríamos implementarlas en python con una lista y haciendo estas operaciones con `list.append()` y `list.pop(-1)`

## Colas

Una cola también es una estructura de datos y se comporta como First In - First Out (Como una cola/fila cualquiera)

También lo podríamos implementar con listas pero para sacar el primer elemento tendríamos que usar `list.pop(0)` y esta operación no es eficiente, por lo cual en vez de usar listas vamos a usar `deque` del modulo `collections` (deque viene de **Double Ended Queue**)

Para hacer una cola (queue) usaremos las siguientes operaciones de deque

- `deque()`: Crear cola
- `deque.append()`: Agregar a la fila
- `deque.popleft()`: Sacar elemento de la fila
- `len(deque)`: Saber su tamaño
- `deuque[0]`: Conocer el primer elemento de la fila

Otras operaciones básicas de deque:
- `deque.appendleft()`: Agregar al principio de la fila
- `deque.pop()`: Sacar el último de la fila
- `deque.clear()`: Limpia la cola
- `deque.count(elemento)`: Cuenta el número de veces que está `elemento` en la cola
- `deque.remove(elemento)`: Quita el primer elemento que sea igual a `elemento`

# 5. Recorrido

## BFS

Es un algoritmo para recorrer un grafo, su forma de hacerlo es, desde el inicio, ver todos los vecinos de ese vertice y los visita, guardandolos para recorrerlos más tarde.

En otra palabras, a partir de un vertice de inicio, BFS recorre el resto del grafo de acuerdo a la cantidad de vertices de distancia con el vertice de inicio

## DFS

La forma que tiene de recorrer un grafo este algoritmo es: a partir de un vertice de inicio, ve sus vecinos y visita el primero, después ve los vecinos de este y hace lo mismo hasta que termine, una vez esto recorre el segundo vecino.

Hay otra forma de implementar esto algoritmos que es guardando el camino que hacen, esto puede ser util para, por ejemplo, un GPS
