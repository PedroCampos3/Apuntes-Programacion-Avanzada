##### Semana 11 - I/O de archivos

# 1.- I_O

Parámetros que se suman en la función `open()`: `encoding=utf-8` y `errors="replace"`

### Escribir y leer bytes

Para escribir se le pasa el parametro `"wb"` o `"rb"`

La función `bytes()` recibe un iterable

### Context Manager

O sea usar with. (Para considerar la posibilidad de errores)

**La función `dir()` muestra los métodos del objeto**

Para que un objeto acepte context manager tiene que tener los métodos `__enter__` y `__exit__`

Ejemplo:

```python
class StringUpper(list):

    def __enter__(self):
        return self

    def __exit__(self, exc_type, exc_value, traceback):
        for index, char in enumerate(self):
            self[index] = char.upper()
```

Para casos en los que necesitamos un archivo si o si podemos emular uno con `StringIO` o `BytesIO`

Ejemplo:

```python
# Aquí simulamos tener un archivo que contiene el string dado
file_in = StringIO("información como texto y más")

# Aquí simulamos un archivo de Bytes para escribir la información
file_out = BytesIO()
```


# 2.- String

Para agregar llaves en un f-string podemos usar `{{` y `}}`

El método `str.format()` recibe kwargs y podemos asignar palabras claves dentro del string y asignarlas en el format

Podemos alinear un fstring pasandole un parametro en cada `{}`.
Por ejemplo: `f"{producto:8s} | {cantidad: ^8d}"`

- El número es para rellenar con espacios o con ceros
- El `^`, `<`, `>` es para que quede centrado, a la izquierda o a la derecha
- La `d` o `f` significa que es un entero (**d**ecimal) o un **f**loat

Los parámetros tienen orden:
1. Carácter para los espacios vacios
2. Alineamiento
3. Tamaño
4. Tipo

## REGEX (finalmente)

Las Expresiones Regulares nos ayudan a encontrar un string (palabra o frase) dentro de otro string.

Para encontrar este string, usamos una combinación de caracteres, a esto le llamaremos una expresión.

Entonces le pasamos a un programa la expresión y el string donde vamos a buscar.

Ahora vamos a ver como son estas expresiones:

Para esto vamos a usar "meta-caracteres" o sea carácteres que cumplen alguna funcionalidad en la búsqueda, estos pueden ser `[]*+?.^$()`

### `[]`

Este se usa como un `OR` sígnifica que alguno de los caracteres de dentro de los caracteres puede ser lo que estamos buscando.

_Los meta-caracteres que se pongan dentro de estos corchetes no harán su función y se comportarán solo como caraccteres_

Por ejemplo: la expresión `[Hh]ola` hace _match_ con `Hola` y con `hola`

A esto se le agrega que podemos usar otros caracteres dentro de los `[]`:

#### `^`

Es como un `NOT`, si antes calzaba alguno de los caracteres que estaban dentro, ahora calzan **todos excepto** los que están dentro de los caracteres

#### `-`

Se usa para especificar un rango de letras o números. e.g. `a-zA-Z0-9`

### `*`

Significa que el carácter (o grupo) que estaba antes puede estar repetido varias veces o no estar.

e.g. `Ho*la` hace match con `Hla`, `Hola`, `Hoola`

### `+`

Lo mismo que `*` pero en vez de estar 0 veces tiene que estar mínimo 1

### `?`

Significa que el caracter anterior puede estar máximo 1 vez o no estar

### `.`

Significa cualquier caracter excepto un salto de línea

### `()`

Es una forma de agrupar caracteres, para usarlos con los otros metacaracteres

e.g. `(Ho)+la` hace match con `Hola`, `HoHola`, `HoHoHola` ...

### `^`

Se usa para informar que solo haga match si está al principio del string

### `$`

Lo mismo que `^` pero en vez de al principio es al final

### `{m, n}`

Sirve para indicar que el caracter anterior puede estar desde m hasta n veces

### `\`

Sirve para indicar que el meta-caracter lo queremos usar como caracter y no como su función

### `|`

Indica que queremos hacer match con la expresión regular A o con la expresión regular B, en el ejemplo: `A|B`

E.G. Expresión: `ab+c|de*f` Strings: `abc`, `abbc`, `abbbc`, ... , `df`, `def`,  `deef`, ...

En python para usar expresiones regulares podemos usar el módulo `re` que tiene las siguientes funciones

- `re.match()`: Verifica si la expresión regular hace match en el principio del string
- `re.matchall()`: Verifica si la expresión hace match con todo el string
- `re.sub()`: Reemplaza la parte del string con la que hizo match con el otro parámetro
- `re.split()`: Retorna una lista y sus elementos son los caracteres del string separados por cada vez que hizo match con la expresión regular que le pasamos
- `re.search()`: Entrega el primer match que hace con un string
- `re.findall()`: Entrega todos los matchs que hizo en un string
- `re.finditer()`: Lo mismo que `findall` pero en vez de entregar una lista, entrega un iterador

Por otra parte en REGEX hay abreviaturas:

- `\d`: Significa algún digito, es decir, `[0-9]`
- `\D`: La negación
- `\s`: Significa cualquier espacio en blanco, i.e. `[\t\n\r\f\v]`
- `\S`: Lo contrario
- `\w`: Significa cualquier caracter alfanúmerico
- `\W`
