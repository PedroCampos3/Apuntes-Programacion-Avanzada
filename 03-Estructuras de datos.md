# semana 03

# Estructuras de datos:

*Def:* Maneras de agrupar datos

## Estructuras secuenciales:

*(que mantienen un orden entre sus elemenetos)*

- tuplas (tuples)
- list
- str

### Tuplas

- Son inmutables: no se puede cambiar sus valores
- Heterogéneas: pueden contener distintos tipos de datos
- Slicing:
    - • `a[:]`: crea una copia (*shallow*) del arreglo completo. Es decir, el arreglo retornado está en una nueva dirección de memoria, pero los elementos que están en este nuevo arreglo, hacen referencia a la dirección de memoria de los elementos del arreglo inicial.

### Named tuples

- Estructuras que permiten “ponerle nombre” a cada posicion de los datos
- Por defecto no están en python por lo cual se requiere importar el módulo namedtuple desde la librería collections
- Ej:

```python
from collectionsimport namedtuple

# Asignamos un nombre a la tupla (Register_type),y los nombres de los atributos que tendrá
Register = namedtuple('Register_type', ['RUT', 'name', 'age'])

# Instanciación e inicialización de la tupla
c1 = Register('13427974-5', 'Christian', 20)
```

- También son inmutables

## Estructuras de datos no secuenciales:

*(almacenan datos sin establecer un orden)*

- Diccionarios (dictionaries)
- conjuntos (sets)

### Diccionarios:

- Funciona por llave-valor (key-value)
- También se le conocen como estructura de “mapeo” (mapping)
- Mutable
- implementados por la clase “dict”
- para llamarlos se puede hacer como `diccionario[llave]` o como `diccionario.get(llave, output si la llave no existe)`
- para eliminar items se puede usar `del diccionario[llave]`
- Para comprobar si existe una llave se usa `in` Ej: `"chile" in monedas`
- Son muy eficientes para acceder a un valor dada una llave pero al revés, es lo contrario
- Están implementados a partir de una estructura llamada tabla de hash, por eso mismo hay requisitos para que algo pueda ser usado como una llave, entre esos que sea hasheable
- **Métodos útiles:**
    - `keys()`: permite obtener una lista con las llaves
    - `values()`: permite obtener una lista con los valores del diccionario
    - `items()`: permite obtener una lista con las llaves y sus valores

### Defaultdicts

*son diccionarios que nos permiten asignar un valor por defecto a cada key con la que se llama el diccionario (para ahorrarnos el escribir algo si el valor no existe)*

- reciben una función que debe devolver el valor que se asignará por defecto ( **no debe recibir parámetros** )

### Sets (Conjuntos)

- Mutables
- No ordenados que no repiten elementos
- No hasheables
- Heterogéneo
- Parecidos a los conjuntos en matemática
- Solo almacenan objetos hasheables
- Es eficiente para consultar las existencia de un elemento
- Se pueden construir con `{}` o con listas a través de la función `set()`
- no soporta acceso indexado (`conjunto[0]`)
- **Operaciones más importantes**:
    - `len()`
    - `add()` (Para añadir elementos)
    - `remove()`
    - `discard()` (lo mismo que remove pero no lanza un error si no está)
    - `for` (**Recordatorio:** no tiene orden)
    - `in`
    - Unión (como en matemática): `|` o `union()` (entrega un nuevo set)
    - Intersección (’’): `&` o `intersection()`(’’)
    - Diferencia: `-` o `difference()`
    - Diferencia simétrica: `^` o `dymmetric_dicfference()`
    - Subconjunto: `<=` o `issubset()`
    - Superconjunto: `>=` o `issuperset()`
    - Se pueden crear listas a partir de un set y viceversa

# Args y kwargs

- **Parámetros:** son los valores que recibe una funcion cuando esta se define
- **Argumentos:** son los valores que se le entregan cuando se **llama** la función
    - Argumento por palabra clave (keyword argument →kwargs): Argumento precedido por un identificador `name=` (lista o tupla)
    - Argumento posicional (o solo argumento): Se guía por el orden en el que es entregado (diccionario)
    - no se puede poner un argumento posicional después de poner un argumento por palabra clave
    - Uno puede entregar a la función estos argumentos de la siguiente forma `funcion(variable, *lista, *tupla, **diccionario)`
    
- Se pueden definir funciones con una cantidad variable de parametros, la más facil es la de con valores por defecto
- cuando se define una variable no se pueden poner parametros después de los **kwargs pero si entre ese y los *args, aunque para después entregarle el argumento solo se puede hacer por palabra clave
- ej:**`def** funcion_general(arg1, arg2, *args, kwarg1, kwarg2, **kwargs):`
    - `arg1` y `arg2` son posicionales **o** por palabra clave.
    - `args` contiene una cantidad **variable** de posicionales.
    - `kwarg1` y `kwarg2` son **solo** por palabra clave.
    - `kwargs` contiene una cantidad **variable** por palabra clave.

# Iterables

- Un iterable siempre tiene implementado el método `.__iter__()` que retorna un objeto de tipo Iterador (que hay que crear)
- Un iterador es el objeto retornado por el método mencionado
    - Este iterador implementa el método `.__next__()`que retorna unos a uno los elementos
        - Esta función entrega los elementos y a la vez los borrar
        - Además debe hacer algo en caso de que se acaben los elementos
    - También debe tener `.__iter__()`que retorna self
    - Cuando no hay más elementos, levanta una excepción StopIteration
    - Cada vez que iteramos un iterable, entrega un interador
        - Por eso también debe tener `.__iter__()`para que sea iterable
    - Solo se puede iterar una vez un iterador, ya que va “eliminando” los elementos que entrega

## Generadores

- Consumen mucha menos memoria

ejemplo de como crear uno:

```python
# Por el sólo hecho de usar paréntesis estamos creando un generador.
generador_pares = (2 * i for in range(10))
```

- Podemos usar el generador directamente en un `for`, ya que como vimos estos implementan `__iter__` devolviendo `self`.

## Funciones Generadoras

- El *statement* `yield` es un análogo a `return`, pero también se asegura que en la próxima llamada a la función, la ejecución parta desde donde se dejó antes.
- Para crear una función generadora se entrega varis veces `yield` (puede ser con un while)
- El método `send()` permite enviar un valor hacia el generador, lo que significa que la expresión `yield` lo recibirá. El valor enviado puede ser usado para asignarlo a otra variable, por ejemplo `v = yield value` guardará en la variable `v` el valor enviado con `send()`.

# Funciones lambda

- Python tiene **funciones de primera clase** (*first-class functions*), es decir, que las funciones son tratadas como cualquier otra variable (objeto).
    - Esto tiene consecuencias como:
        1. **Las funciones pueden ser asignadas a una variable, y luego usar esa variable igual que la función.**
        2. **Las funciones pueden ser pasadas como parámetro a otras funciones.**
- Consiste en `lambda <parámetros>: <valor a retornar>`
- Se pueden combinar (sería el f(x)) con las siguientes funciones (calza muy bien que no tengan nombre)
    - map: Retorna un generador. Aplica la función a cada artículo en el iterable. La cantidad de iterables corresponde al numero de parametros que debe recibir la función f
        - `map(f, iterable)` es equivalente a `(f(x) for x in iterable)`.
    - `filter(f, iterable)` recibe como parámetros una función que retorna `True` o `False` (o función *booleana*), y un iterable. Retorna un generador que entrega aquellos elementos del iterable donde la función `f` retorna `True`.
        - Se puede ver que `filter(f, iterable)` es equivalente a `(x for x in iterable if f(x))`.
    - la idea detrás del `reduce`. Esta operación consiste en aplicar sucesivamente una función `f(x, y)`, donde `x` es el resultado acumulado e `y` es un elemento de la secuencia.
        - Entonces, `reduce(f, iterable)` recibe una función que toma dos valores y un iterable. Retorna lo que resulta de aplicar la función `f` al iterable `[s1, s2, s3, ..., sn]` de la siguiente forma: `f(f(f(f(s1, s2), s3), s4), s5), ...`.

# Funciones built-in en python

- `len():`Hace un llamado al metodo `.__len__()`que viene impolementado
    - *por eso es lo mismo hacer `len(x)` que si hacemos un `x.__len__()`*
- **`__getitem__`:** Es la función encargada de la notación `objeto[valor]`
- `reversed() -> .__reversed__`
- `enumerate()` entrega una especie de generador que retorna tuplas, donde el primer objeto en cada tupla es el indice y el segundo es el ítem original. retorna un objeto de tipo `enumerate`, que se comporta de manera similar a un generador,
- `zip`Toma dos o más secuencias o iterables y retorna un iterador que entrega tuplas, donde cada tupla está formada por los elementos i-ésimos de cada una de las secuencias o iterables. (o sea combina dos tuplas creando una nuevas tuplas y en cada una estan los primeros elementos de las dos primeras tuplas) retorna un objeto de tipo zip también parecido a un generador
    - si `a = zip(tupla1, tupla2)` hace lo que explicamos, `a = zip(*a)` hace lo contrario