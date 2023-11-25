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

## REGEX

