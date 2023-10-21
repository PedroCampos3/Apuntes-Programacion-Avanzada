# semana 04

# 1.- Interfaces gráficas

Un *framework* se puede entender como un entorno de desarrollo o conjunto de reglas estandarizadas y definidas para crear algo

En PyQt, los elementos básicos que permiten recibir eventos para interactuar con el usuario, y que permiten desplegar una representación gráfica en la pantalla, se conocen como *widgets*.

Para crear una ventana usamos la clase `QWidget` desde el módulo `QtWidgets` en `PyQt6`.

El primer paso es crear la aplicación que contendrá la ventana y todos los ***widgets*** dentro de esa ventana. Hacemos esto mediante la clase `QApplication`, también del módulo `QtWidgets`

La clase `QApplication` debe ser instanciada **antes** que todos los demás *widgets*. Por cada aplicación que use PyQt, existe **solo una instancia** de `QApplication`

`QApplication` recibe como argumento parámetros que por lo general son entregados desde la línea de comandos

`1-interfaces-graficas-ejemplo_1.py`.

En el programa principal (`'__main__'`), después de que creamos una instancia de `MiVentana`, esta solo existe en memoria. Para mostrar la ventana en la pantalla usamos su método `show()`. Finalmente, el método `exec()` de `QApplication` ejecuta el *main loop*, que permite iniciar la detección de todos los eventos del sistema.

Revisar archivo 1 que tiene un script que muestra errores en pantalla que nos ayudaran cuando queramos debuggear

# 2.- elementos graficos basicos

`MiVentana`, se definió la geometría de la ventana, con la línea: `self.setGeometry(200, 100, 300, 300)`.

Mostrar texto → etiquetas → `QLabel(texto, padre)` ← `.move(cord, cord)`

Ingresar texto → cuadros de texto → `QLineEdit(texto, padre)`  ← `.setGeometry(cord, cord, tamaño, tamaño)`

Estos widgets deben estar dentro de otro widget (el parent o padre)

Se puede crear un método `init_gui` para crear los widgets y llamar a este desde el init

se hace `self.show()` al final de este método

después cuando se instancia esta clase `QApplication([])`, se intancia la ventana y después se ejecuta `sys.exit(app.exec())`

### Imagenes!

con la clase `QPixMap(ruta)` del módulo QtGui, para agregarlo a unna ventana, se deben cargar los pixeles dentro de un elemento QLabel

```python
# Cargamos la imagen como pixeles 
        pixeles = QPixmap(ruta_imagen)
        
        # Agregamos los pixeles al elemento QLabel
        self.label.setPixmap(pixeles)
        
        # Finalmente, ajustamos tamaño de contenido al tamaño del elemento (100 x 100)
        self.label.setScaledContents(True)
```

### Botones!

con el widget `QPushButton('Texto', self)`

```python
    		"""
        El uso del caracter & al inicio del texto de algún botón o menú permite
        que la primera letra del mensaje mostrado esté destacada. La
        visualización depende de la plataforma utilizada.
        El método sizeHint provee un tamaño sugerido para el botón.        
        """
        self.boton1 = QPushButton('&Procesar', self)
        self.boton1.resize(self.boton1.sizeHint())
        self.boton1.move(5, 70)
```

### Layouts

para alinear los *widgets* horizontal y verticalmente: `QHBoxLayout` y `QVBoxLayout` del módulo `QtWidgets`.

Los objetos deben ser agregados a cada *layout* mediante el método `addWidget(widget)`.

Se tiene que crear el el layout horizontal y vertical

```python
				"""
        Creamos el layout horizontal y agregamos los widgets mediante el
        método addWidget(). El método addStretch() nos permite incluir
        opcionalmente espaciadores.
        """
        hbox = QHBoxLayout()
        hbox.addStretch(1)
        hbox.addWidget(self.label1)
        hbox.addWidget(self.edit1)
        hbox.addWidget(self.boton1)
        hbox.addStretch(1)

        """
        Creamos el layout vertical y le agregamos el layout horizontal.
        Opcionalmente agregamos espaciadores para distribuir los widgets.
        Notar el juego entre el valor recibido por los espaciadores.
        """
        vbox = QVBoxLayout()
        vbox.addStretch(5)
        vbox.addLayout(hbox)
        vbox.addStretch(1)
        self.setLayout(vbox)
```

### Grid Layout

Con `QGridLayout` se crea una grilla y para agregar widgets a esta matriz se usa `addWidget(widget, i, j)` 

```python
def init_gui(self):

        # Creamos una etiqueta para status. Recordar que los widgets simples
        # no tienen StatusBar.
        self.label = QLabel('Status:', self)

        # Creamos la grilla para ubicar los widgets de manera matricial
        self.grilla = QGridLayout()

        valores = ['1', '2', '3',
                   '4', '5', '6',
                   '7', '8', '9',
                   '0', 'CE', 'C']

        # Generamos las posiciones de los botones en la grilla y le asociamos
        # el texto que debe desplegar cada botón guardados en la lista valores
        posiciones = [(i, j) for i in range(4) for j in range(3)]
        
        for posicion, valor in zip(posiciones, valores):
            boton = QPushButton(valor, self)
            self.grilla.addWidget(boton, *posicion)

        # Creamos un layout vertical
        vbox = QVBoxLayout()

        # Agregamos el label al layout con addWidget
        vbox.addWidget(self.label)

        # Agregamos el layout de la grilla al layout vertical con addLayout
        vbox.addLayout(self.grilla)
        self.setLayout(vbox)

        self.move(300, 150)
        self.setWindowTitle('Calculator')
```

# 3.- Eventos y señales

para conectar un widget con una función y que haga algo se usan eventos (en los botones son los eventos `clicked` para hacer esto se usa el método `connect(funcion)` , se le pasa la función sin los parentesis porque si no le estariamos entregando el resultado de la funcion.

*(comunicación evento y slot - mecanismo signal y slot)*

```python
for posicion, valor in zip(posiciones, valores):
            boton = QPushButton(valor, self)

            """
            Aquí conectamos el evento clicked() de cada boton con el slot
            correspondiente. En este ejemplo todos los botones usan el
            mismo slot (self.boton_clickeado).
            """
            boton.clicked.connect(self.boton_clickeado)

            self.grilla.addWidget(boton, *posicion)
```

sobre la función:

```python
def boton_clickeado(self):
        """
        Esta función se ejecuta cada vez que uno de los botones de la grilla
        es presionado. Cada vez que el botón genera el evento, la función inspecciona
        cual botón fue presionado y recupera la posición en que se encuentra en
        la grilla.
        """

        # Sender retorna el objeto que fue clickeado.
        # Ahora, la variable boton referencia una instancia de QPushButton
        boton = self.sender()

        # Obtenemos el identificador del elemento en la grilla
        idx = self.grilla.indexOf(boton)

        # Con el identificador obtenemos la posición del ítem en la grilla
        posicion = self.grilla.getItemPosition(idx)

        # Actualizamos label1
        self.label1.setText(f'Status: Presionado boton {idx}, en fila/columna: {posicion[:2]}.')
```

El método `sender()`retorna el objeto que generó el evento.

Laa clase QCoreApplication de QtCore hace referencia al objeto QApplication que está en ejecución

ej: `QCoreApplication.instance().quit()`

### Eventos de mouse

todo widget tiene distintos gatillos de eventos, por ejemplo la clase `QWidget` incluye el método `mousePressEvent`

```python
def mousePressEvent(self, event):
        x = event.position().x()
        y = event.position().y()
        print(f"El mouse fue presionado en {x},{y}")
        self.click_dentro_del_label = self.label.underMouse()
        if self.click_dentro_del_label:
            print("\tFue presionado dentro del QLabel")
        else:
            print("\tFue presionado fuera del QLabel")

    def mouseReleaseEvent(self, event):
        x = event.position().x()
        y = event.position().y()
        print(f"El mouse fue liberado en {x},{y}")

        if self.click_dentro_del_label:
            print("\tAntes se había presionado dentro del QLabel")
        else:
            print("\tAntes habías presionado fuera del QLabel")

    def mouseMoveEvent(self, event):
        x = event.position().x()
        y = event.position().y()
        print(f"El mouse se mueve... está en {x},{y}")
```

hay más eventos sobre el mouse

### Eventos del teclado

La clase `QWidget` inclute el método `keyPressEvent`, para que haga algo hay que hacer override

```python
def keyPressEvent(self, event):
        """
        Este método maneja el evento que se produce al presionar las teclas.
        """
        self.etiqueta.setText(f'Presionaron la tecla: {event.text()} de código: {event.key()}')
        self.etiqueta.resize(self.etiqueta.sizeHint())
```

### Generar señales personalizadas

para esto se debe crear una subclase de `QtCore.pyqtSignal`. Dentro de la subclase se crea la nueva señal como una instancia de `QtCore.pyqtSignal`.

- Mediante el método `emit` de `pyqtSignal` se emite la señal (indicando que ocurrió un evento).
- Mediante el método `connect`, también de `pyqtSignal`, se define la función o método a ejecutar cuando la señal es emitida.
1. ¿Por qué se encapsula la instancia `pyqtSignal` dentro de una clase? ¿No se puede instanciar directamente y trabajar con el objeto `pyqtSignal`?
2. ¿Por qué se define como un atributo de clase y no de instancia?
3. ¿Esto último significa que todas las instancias de la clase `VentanaPresionable` compartirán la misma señal?

¡Muy buenas preguntas! Las tres prácticamente se responden de la misma forma: la razón de fondo es la implementación interna de PyQt que requiere estas condiciones. Ahora, específicamente:

1. Las `pyqtSignal` deben estar encapsulada por objetos que hereden de `QObject`. Se pueden instanciar señales fuera de una clase, pero estas no serán capaces de conectarse a funciones o ser emitidas por alguna entidad. ¿Por qué entonces podemos encapsular la señal en `VentanaPresionable`, que no es un `QObject`? Esto es simplemente porque nuestras ventanas heredan de `QWidget` y todo `QWidget` es a su vez un `QObject`.
2. Las instancias de `pyqtSignal` deben ser definidas como atributo de clase y no de instancia por diseño interno de PyQt. Puedes hacer la prueba e instanciarla como un atributo de instancia, y verás que esta no será capaz de conectarse a llamables o de emitirse.
3. Hemos visto que el comportamiento de un atributo de clase es distinto para un atributo de instancia para objetos mutables. En general, si tenemos un atributo de clase mutable (como una lista), si cualquier instancia modifica este atributo, todas las instancias tienen acceso a este atributo y ven el efecto. Entonces, ¿todas las instancias de `MiSenal` compartirían la señal `senal`? Soprendentemente, la respuesta es **no**. Esto se debe a implementación interna de PyQt que logra que a pesar de que se define a nivel de atributo de clase, para cada instancia la señal creada como atributo es única y distinta. Entonces, a pesar de que se defina a ese nivel, podemos estar tranquilos en que cada instancia tendrá su propia señal única.

```python
def mousePressEvent(self, event):
        # Se emite la señal simple, sin argumento
        self.senal_simple.emit()
        # Se emite la señal que permite envir un str
        # Se envia el contenido de la etiqueta de la ventana
        self.senal_texto.emit(self.etiqueta.text())

    def mouseMoveEvent(self, event):
        # Se emite la señal que permite enviar dos ints, enviamos la posición del mouse
        self.senal_coordenadas.emit(event.pos().x(), event.pos().y())
```

```python
if __name__ == '__main__':
    app = QApplication([])
    # senales = MisSenales()
    ventana_click = VentanaPresionable()
    ventana_edit = VentanaQueSeEdita()

    # Conectamos las señales
    ventana_click.senal_simple.connect(ventana_edit.edita_etiqueta_click)
    ventana_click.senal_texto.connect(ventana_edit.edita_etiqueta_texto)
    ventana_click.senal_coordenadas.connect(ventana_edit.edita_etiqueta_posicion_mouse)

    sys.exit(app.exec())
```

# 4 Diseño Front-Back

- **Cohesión**: Cada una de las componentes del software debe realizar **solo las tareas para las cuales fue creada**, delegando otras tareas a otras componentes según corresponda. Por ejemplo, si tengo una clase `SimulaciónDeParque`, un diseño altamente cohesionado incluiría métodos como `iniciar_simulación()` o `detener_simulación()`, pero no métodos como `limpiar_atracción()` o `ingresar_clientes_a_restaurant()`, ya que la clase `SimulaciónDeParque` fue diseñada para administrar la simulación y no para hacerse cargo de métodos que deberían ser ejecutados por (*delegados a*) otras clases de la simulación.
- **Acoplamiento**: Cuando la modificación de una componente implica que es necesario modificar otra componente para que la implementación del cambio sea correcta y completa. Por ejemplo, si al modificar los atributos de una clase A, también se deben modificar los atributos de otra clase B, se dice que hay *alto acoplamiento* entre las clases A y B. Un buen diseño intenta reducir el acoplamiento entre clases.

para llamar a una funcion indirectamente (sin poner su nombre) usamos señales

para usar señales, primero hay que crearlas, emitirlas y conectarlas

- `senal_procesar = pyqtSignal(str)`
- `self.senal_procesar.emit(texto_input)`
- `ventana.senal_procesar.connect(procesador.procesar_input)`

Toda clase que crea como atributo una pyqtSignal deve heredar de QObject y llame a su constructor

# 5.- Ejemplo conexion entre ventanas