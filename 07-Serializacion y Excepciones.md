# semana 07

# 1.- Serializacion

guardar weas

### Pickle

tiene los metodos:

- `dumps(data)`: serializa un objeto, es decir, lo **guarda**.
- `loads(data)`: deserializa un objeto serializado, es decir, **carga** un objeto a su estado original.

```python
tupla = ("a", 1, 3, "hola")
tupla_serializada = pickle.dumps(tupla)

print(f"Resultado serialización: {tupla_serializada}")
print(f"Tipo de versión serializada: {type(tupla_serializada)}")

tupla_deserializada = pickle.loads(tupla_serializada)
print(f"Resultado deserialización: {tupla_deserializada}")
```

También tiene: (OJO SON DISTINTOS)

Estos métodos también serializan y deserializan, pero **a través de archivos**:

- `dump(data, file)`: guarda un archivo con el objeto serializado.
- `load(data, file)`: deserializa un objeto almacenado en un archivo (lo "trae de vuelta").

```python
with open(path.join("data", "mi_lista.bin"), "wb") as file:
    pickle.dump(lista, file)

with open(path.join("data", "mi_lista.bin"), "rb") as file:
    lista_cargada = pickle.load(file)
```

pickle no es seguro, puede entrar malware al pc

- para serializar un objeto, su clase debe tener `__getstate__` (este retorna un diccionario con los atributos que se quieren serializar)
- si no está, entonces guardar el atributo `__dict__` del objeto (es un diccionario que guarda todos los atributos y métodos de un objeto, o sea `objeto.atributo` es equivalente a `objeto.__dict__["atributo"]` )

```python
def __getstate__(self):
        """
        Retorna el estado actual del objeto, para que sea serializado por pickle

        Aquí creamos una copia del diccionario actual, para modificar la copia 
        y no el objeto original
        """
        # Usamos una copia del diccionario original para alterar solo la copia
        nueva = self.__dict__.copy()
        # Modificamos un atributo en el objeto serializado.
        # Sin embargo, el objeto original no ha cambiado.
        nueva.update({"mensaje": "¡Me están serializando!"})
        # Lo que retornemos es lo que será serializado por pickle
        return nueva
```

### Deserialización

- Implemental el método `__setstate__` que lo que hace es recibe el diccionario y lo asigna a `__dict__`
- si, no está, se hará lo mismo

## JSON

Solo se puede serializar instancias de ciertas clases

Importar módulo `json`

Es parecido a pickle, existe `json.dumps(dicc)` y `json.loads(str)`

también podemos personalizar la serialización, Para esto debemos crear una clase que hereda de la clase `json.JSONEncoder` y sobreescribir el método `default`:

```python
def default(self, obj):
        """Serializa en forma personalizada el objeto de tipo Persona"""
        if isinstance(obj, Persona):
            return {
                "Persona_id": obj.id_,
                "nombre": obj.nombre,
                "edad": obj.edad,
                "estado_civil": obj.estado_civil,
                "ano_nacimiento": datetime.now().year - obj.edad,
            }
        # Mantenemos la serialización por defecto para otros tipos
        return super().default(obj)
```

### Deserializaciónn en json

`object_hook`, es el analogo a `__setstate__`

# 2.- Manejo de Bytes

`ord(caracter)` para ver el codigo

para pasar de decimal a hexadecimal `hex(num)`

con `.decode(codificación)` pasamos de bytes a un str decodificado

### Bytearray

so como listas:

- son mutables
- existe la notación slicing (y se puede indexar)
- se pueden agregar bytes con el método extend

### Chunks

es un grupo de bytes

### Transformar números (metodos de ints)

Big endian: como escribimos nosotros

little endian: el byte más significativo va al final

`integer.to_bytes(length, byteorder)` : retorna el integer representado por un arreglo de bytes

- `int.from_bytes(byte, byteorder)` : retorna un arreglo de bytes representando un integer

### print de bytes

no confiar

# 3.- Excepciones

Objetos tipo Exception

- Syntax error
- Indentation error: Hereda de Syntax error
- NameError
- ZeroDivisionError
- IndexError
- KeyError: similar al index pero con diccionarios y mappings
- AttributeError: metodos que no están en una clase
- TypeError: para casos como sumar un int con un str o cuando se llama a objetos que no lo son (`__call__`)
- ValueError: cuando se le da un argumento que no era apropiado para a ejecucion (Un ValueError es un TypeError pero no al revés)

# 4.- Levantamiento y manejo excepciones

### Levantando excepciones: `raise`

### Manejo de Excepciones: try y except

La sentencia `try` permite definir un *scope* (bloque de código). Si se levanta una excepción dentro del *scope* de `try`, entonces la excepción es **capturada**. A continuación del bloque de `try` debe haber una o más instrucciones `except`. Las instrucciones `except` permiten implementar el manejo de la excepción capturada.

En el momento que se captura una excepción dentro de `try` el flujo del programa salta inmediatamente al bloque de una de las sentencias `except`. Una vez que el bloque `except` ha terminado, el programa continúa en la instrucción **posterior** al bloque `try`/`except`. El programa **NO regresa** a la sentencia que gatilló la excepción.

Cómo se mencionó al inicio de esta sección, solo se atrapan excepciones que surgen durante la ejecución del código. Esto implica que excepciones del tipo `SyntaxError` o `IndentationError` no son posible de **atrapar** porque estas surgen durante la lectura del programa, no su ejecución.

si importamos un archivo que si tiene estos errores, si funcionará try/except porque el error pasa en la ejecucion del import

### Flujos complementarios: `else` y `finally`

else: ese bloque se ejecutara cuando no haya habido alguna excepción

finally: SIEMPRE se ejecuta, independiente si hubo una excepción

si hubo un return en el try y no hubo errores, el else no se ejecutará