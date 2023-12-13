# Semana 05

Para crear Threads, se realiza con el módulo threading

Hay que crear un objeto de la clase thread en el le pasamos en el parámetro target una función para que la ejecute

Aún así no se ejecuta automaticamente, hay que llamar al método start()

Haciendolo de esta forma, no se puede volver a ejecutar el thread, hay que instanciarlo de nuevo.

La clase Thread también admite, el argumento name para poder identificarlos.

`threading.current_thread()` es una función que retorna **una referencia de la instancia del *thread* que está ejecutando este código** (el *thread* que está en la CPU)

Se le pueden pasar los argumentos de la función por los *args y **kwargs de cuando se crea el objeto

También se pueden crear threads creando un clase que herede de threading.Thread y hacer override de run()

si el programa principal necesita que termine algún thread para continuar, se usa el método `join(timeout=None)`(el timeout es para esperar al thread solo ese tiempo)

el metodo `.is_alive()`sirve indicar si un Thread sigue en progreso o no

**Threads Daemons:** son threads que aunque estén corriendo, se cierran cuando el programa principal se cierra

para crearlos, hay que especificarlo cuando se crea el thread o se puede cambiar después llamando al valor, pero no se puede cambiar después correr el thread

**Timers**: subclase de Thread. Ejecuta la función que se le especificó después de un tiempo especificado

# 2 Concurrencia

Al usar threads pueden haber errores inesperados como interrumpir una asignación a una variable, por lo tanto hay soluciones para poder sincronizar threads.

1. **Lock:** Es una clase y para activar el lock se usa el método `aquire` (empezando la parte en la que no puede parar el thread) y se desactiva con el método `release()`
    1. También funciona dentro de un context manager con `with` haciendo el `aquire()` y `release()` solo
2. **Event:** Para que otro thread empiece a partir de un evento en el primer thread.
    1. Esto se puede hacer con eventos (que Event es una clase y los eventos, objetos) en los que podemos modificar su valor bool
    2. También se puede hacer con los metodos `set()` y `wait()` de los threads
    3. `is_set()` es lo mismo que `wait()` pero da un valor bool así el thread puede continuar y volver a consultarlo después

### Deadlocks:

situaciones en los que por error dos threads se esperan mutuamente

Si bien los `deque` permiten agregar y sacar elementos desde ambos extremos en forma segura con *threads*, **nada nos asegura** que si vimos que había un objeto para sacar, ese objeto todavía esté cuando queramos sacarlo. Por lo tanto, tenemos que asegurarnos nosotros mismos – vía *locks* – de que revisar si había algo y sacarlo sea una operación atómica.

### `Queue`

El módulo `queue` tiene implementada una cola hecha para situaciones donde hay varios *threads*. Tiene métodos que la hacen un poco diferente a la implementada en `collections`:

- `put()`: Agrega un ítem al final de la cola (*push*)
- `get()`: Remueve y retorna un ítem de la cola (*pop*). Lo interesante es que este método **espera** hasta que exista algo para sacar de la cola.
- `task_done()`: Requiere ser llamado cada vez que un ítem extraído de la cola ha sido procesado.
- `join()`: El *thread* que llame a este método queda en pausa hasta que todos los ítems de la cola hayan sido procesados.