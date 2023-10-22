# Semana 06

# 1.- PyQt QThreads

## QThread

Es un thread como los de Threading

Es parte del módulo `PyQt6.QtCore`

en vez de ocupar `is_alive()` se ocupa `isRunning()`

Se define igual que un Thread, o sea:
- Se hace `super().__init__()`
- Definir el comportamiento en el método `run`
- Se ejecuta con el método `start`

## QMutex (locks)

Para poder usar los metodos `lock()` y `unlock()` se usa el objeto `QMutex`

# QTimer

A diferencia del resto, esto no es símil a `Timer` de `Threading`;
La diferencia es que en vez de ejecutar una sola vez una subrutina, lo hace varias veces, periodicamente

Y aunque por esto mismo se pueda parecer a `time.sleep()`, el problema que tiene este es que estamos obligados a forzar el thread a terminar (`terminate`) (que es mala práctica), en cambio QTimer tiene `start` y `stop`

Se le tiene que asignar un tiempo (`setInterval(milisegundos)`), después de empezar el `QTimer` y para conectarlo a una subrutina se usa `timer.timeout.connect(subrutina)`

## singleShot

Se usa por si queremos que se ejecute la función solo una vez

Aunque hay otras alternativas (como hacer `stop() en la misma función`), la ventaja es que no tenemos que incluir `stop()`

**Para usarlo**, después de instanciar la clase `Qtimer(self)`, llamamos al método `setSingleShot(True)` 

# PyQt Miscelaneo

## _isAutoRepeat_ en _KeyPressEvent_

_(supuestamente ya sabiamos que)_
El método `keyPressEvent` es para detectar cuando un tecla del teclado es presionada.
Pero para saber si se dejó presionada se usa el método `isAutoRepeat()`; retorna un boolean diciendo si es una repetición del evento o si es la primera vez que se presiona la tecla

## Sonidos en PyQt

### QMediaPlayer

Sirve para ejecutar archivos `.mp3`

#### Uso:
1. Instanciar el objeto: `self.media_player_mp3 = QMediaPlayer(self)`
2. Entregarle un objeto tipo QAudioOutput: `self.media_player_mp3.setAudioOutput(QAudioOutput(self))`. Esto es para que el reproductor entienda que va a desplegar un sonido.
3. Definir un objeto tipo `QURL` con el paths a nuestro sonido: `file_url = QUrl.fromLocalFile(join("sounds", "waku-waku.mp3"))`.
4. Entregarle la URL a nuestro reproductor: `self.media_player_mp3.setSource(file_url)`
5. Finalmente usar `play()` para reproducir el sonido.

### QSoundEffect

Este en cambio es para los archivos `.wav`

#### Uso:
1. Instanciar el objeto: `self.media_player_wav = QSoundEffect(self)`
2. Definir un objeto tipo `QURL` con el paths a nuestro sonido: `file_url = QUrl.fromLocalFile(join("sounds", "see-you-again.wav"))`.
3. Entregarle la URL a nuestro reproductor: `self.media_player_wav.setSource(file_url)`
4. Finalmente usar `play()` para reproducir el sonido. También podemos usar `stop()` para detenerlo.

# 4.- PyQt Main Window

Para ventanas más completas: `MainWindow`


![](img/pyqt-mainwindow-layout.png)

Esta va a tener:

- **Barra de estado**: Se crea con el método `statusBar()` de la clase `QApplication`

- **Barra de menú**

- **Barra de herramientas**

- **Contenido Central** (central widget): puede tener cualqueira de los widgets en `QtWidgets`, para agregarlos se usa el método `setCentralWidget(widget)`