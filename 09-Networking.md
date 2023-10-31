##### Semana 09 - Networking

# 1. Conceptos Básicos

Para comunicar dos computadores necesitamos
- Un medio como un cable Ethernet o WiFi
- Un destinatario y saber como encontrarlo (direcciones IPs)
- Un protocolo para transmitir mensajes (TCP y UDP)
- Un protocolo de aplicación (El contenido del mensaje. Ej: HTTP, DNS, SMTP)

## Como identificar una entidad en Internet

**Host:** Una maquina que es capaz de comunicarse con el exterior

### Dirección IP

Es una etiqueta que permite que un router encuentre al destinatario. Este número debe ser único en casi toda la red.

Hay dos versiones.
- **IPv4**: Un número de 4 bytes. Se escribe en una notación _human-readable_, se separan los bytes por un punto.

- **IPv6**: Es de 128 bits. Se escribe como 8 grupos de 16 bits separados por dos puntos, cada grupo se escribe en hexadecimal

El protocolo **DNS** _(Domain Name Service)_ es el que hace cuando vamos a una página web, tenemos que poner una URL y no una IP.

En este protocolo se usan **nombres jerárquicos** como `cursos.canvas.uc.cl` que se refiere a un *host* de nombre `cursos` dentro de un **dominio** de nombre `canvas.uc.cl`, el cual a su vez es parte de un **dominio** más grande de nombre `uc.cl`, y que a su vez es parte de un **dominio** más grande aún, denominado `cl` que concentra todas los nombres utilizados en Chile y que es mantenido por la empresa [NIC.cl](https://www.nic.cl).

### Puertos

Para que no se mezclen los mensajes de todos los programas que están corriendo en un PC, cada programa usa un **puerto** distinto para comunicarse (Esto es como un edificio donde conviven múltiples personas que comparten la misma dirección (dirección IP), pero que se diferencian por su número de departamento (el puerto).)

Un puerto es un número de 16 bits. Un programa puede ocupar más de un puerto de un host pero no se pueden compartir puertos con otros programas. El puerto de origen no tiene que ser igual al puerto de destino.

Existen una convención sobre que puertos van para que usos.

### Protocolos de comunicación

En la Internet existen dos protocolos que permiten que un mensaje emitido por un **programa emisor** sea recibido por un **programa receptor**. Estos protocolos se conocen como **protocolos de transporte**.

En internet, los mensajes son **secuencias de bytes serializados**. Los mensajes pueden ser muy grandes pero no se puede enviar como una sola unidad ya que pueden haber errores, (Ej: la interferencia electromagnética).

Los **protocolos de transporte** dividen el mensaje en paquetes, estos se etiquetan con la Ips y el puerto del destinatario.

### TCP (Transmission Control Protocol)

Protocolo para enviar y recibir un _stream (corriente o flujo) de bytes serializados_ de forma **confiable**, o sea que se asegura que todos los paquetes lleguen sin errores.
##### Como lo hace
1) Se verifica cada paquete (Se le aplican funciones)
2) Pide confirmación sobre si llego bien el paquete
3) Retransmite los paquetes perdidos

Para iniciar la transmisión se inicia un protocolo llamado _handshake_ en el que se intercambian información como la cantidad de paquetes, tiempo de espera, etc; por eso se dice que TCP es un protocolo _connection-oriented_

Hay otros protocolos de aplicaciónn que usan TCP por la confiabilidad como HTTP, SMTP o BitTorrent

### UDP (User Datagram Protocol)

Aquí se descartan los paquetes con errores o perdidos. La gracia que tiene es que es más rápido, tiene menos latencia y al final es más liviano (tiene menos overhead).

Es un protocolo **connectionless** o sea que no es necesario que se pongan de acuerdo ambas partes, los paquetes solo llegan y ya.

### Encapsulamiento

La comunicación entre dos procesos se divide conceptualmente en capas. Existen dos modelos (que son equivalentes entre sí), el modelo OSI y el TCP/IP

# 2. Networking con Python TCP

_La arquitectura cliente-servidor_

## Sockets

Es un objeto del sistema operativo para comunicarse con otro programa en otra máquina o en otro puerto de la misma máquina. Son nuestro "punto de entrada y salida a/desde la red"

Para obtener uno necesitamos especificar que dirección IP usaremos (IPv4 o IPv6) y que protocolo de transporte usaremos (TCP o UDP)

En `Python`, usaremos el módulo `socket`.

Para crear uno se debe instanciar la clase `socket(family, type)`, `family` es el tipo de dirección IP que usaremos y `type` el protocolo de transporte. Algunos de los valores permitidos son:

- `family`
    - `AF_INET` para direcciones IPv4
    - `AF_INET6` para direcciones IPv6
- `type`
    - `SOCK_STREAM` para TCP
    - `SOCK_DGRAM` para UDP

Por ejemplo, para crear unn socket TCP con IPv4:
```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

### Cliente TCP

Para una conexión TCP necesitamos:
1) la IP del host destinatario
2) El puerto del host destinatario

Para conectarse a un servidor TCP usamos el método `connect((host, port))`; El método recibe unan tupla, siendo (IP, puerto)

También en vez de poner una ip podemos poner un string con el _hostname_ como `github.com`

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('146.155.123.21', 80))
```

#### Enviar mensajes

Como solo envian _bytes_, debemos codificar. Podemos usar uno estos métodos:

- `send(bytes)`: **Trata** de enviar el mensaje pero puede no mandarlo completo o resultar en un error, entonces por eso retorna la cantidad de _bytes_ para volver a hacer otro `send` con lo restante.

- `sendall(bytes)`: **Se asegura** de mandar todos los bytes del mensaje. En realidad también usa `send` pero si pasó algo en el envío, solo lanza un error entonces no sabemos que se envió hasta antes del error.

##### Ejemplo:

Vamos a mandar una _HTTP Request_

```python
solicitud = 'GET / HTTP/1.1\n\n\n'
sock.sendall(solicitud.encode('ascii'))
```

Para ver la respuesta del servidor ocupamos el método `recv(buffer)`, el parámetro que recibe es la cantidad máxima de bytes que se leerá. La documentación sugiere colocar potencias de 2.

```python
data = sock.recv(4096)
print(data.decode('ascii'))
```

Por último, cerrar la conexión.

```python
sock.close()
```

##### Otro Ejemplo, ahora todo junto:

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

host = "iic2333.ing.puc.cl"
port = 80

try:
    sock.connect((host, port))
    sock.sendall('GET / HTTP/1.1\nHost: iic2333.ing.puc.cl\n\n'.encode('utf-8'))
    data = sock.recv(4096)
    print(data.decode('utf-8'))
except ConnectionError as e:
    # ConnectionError es la clase base de BrokenPipeError, ConnectionAbortedError,
    # ConnectionRefusedError y ConnectionResetError
    print("Ocurrió un error.")
finally:
    # ¡No olvidemos cerrar la conexión!
    sock.close()
```

### Servidor TCP

Para escuchar las conexiones de un puerto debemos enlazar un _socket_ a ese puerto con el método `bind((host, port))`. `host` es el _hostname_ del _host_ en que estamos corriendo el servidor; `port` es el puerto donde queremos escuchar las conexiones.

##### Ejemplo:

```python
import socket

sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Así podemos obtener el hostname de la máquina actual
host = socket.gethostname()
port = 9000

sock.bind((host, port))
```

*En el cliente no era neceario hacer `bind` porque el __OS__ lo hace implicitamente con el método `connect`, le asigna un puerto al azar*

Para escuchar conexiones: el método `listen()`

Ahora para aceptarlas: el método `accept()` que retorna un _socket_ para comunicarse solo con ese cliente y además una tupla con la IP y el puerto del cliente.

Para terminar el programa servidor, se debe cerrar el socket que esperaba las conexiones: `sock.close()`

# 3. Ejemplos

bla bla bla

## ¡Cliente!

```python
import socket
import threading


class Client:
    """
    Maneja toda la comunicación desde el lado del cliente.

    Implementa el esquema de comunicación donde los primeros 4 bytes de cada
    mensaje indicarán el largo del mensaje enviado.
    """

    def __init__(self, port, host):
        print("Inicializando cliente...")

        self.host = host
        self.port = port
        self.socket_client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

        try:
            self.connect_to_server()
            self.listen()
            self.repl()
        except ConnectionError:
            print("Conexión terminada.")
            self.socket_client.close()
            exit()

    def connect_to_server(self):
        """Crea la conexión al servidor."""

        self.socket_client.connect((self.host, self.port))
        print("Cliente conectado exitosamente al servidor.")

    def listen(self):
        """
        Inicializa el thread que escuchará los mensajes del servidor.

        Es útil hacer un thread diferente para escuchar al servidor,
        ya que de esa forma podremos tener comunicación asíncrona con este.
        Luego, el servidor nos podrá enviar mensajes sin necesidad de
        iniciar una solicitud desde el lado del cliente.
        """

        thread = threading.Thread(target=self.listen_thread, daemon=True)
        thread.start()

    def send(self, msg):
        """
        Envía mensajes al servidor.

        Implementa el mismo protocolo de comunicación que mencionamos;
        es decir, agregar 4 bytes al principio de cada mensaje
        indicando el largo del mensaje enviado. 
        Luego, puedes elegir entre usar send() en un ciclo, o un solo sendall().
        En este ejemplo, solo usamos un sendall(), pero puedes cambiarlo por 
        el ciclo del ejemplo anterior para asegurar que toda la información
        se enviará correctamente.
        """

        msg_bytes = msg.encode()
        msg_length = len(msg_bytes).to_bytes(4, byteorder='big')
        self.socket_client.sendall(msg_length + msg_bytes)

    def listen_thread(self):
        while True:
            response_bytes_length = self.socket_client.recv(4)
            response_length = int.from_bytes(
                response_bytes_length, byteorder='big')
            response = bytearray()

            # Recibimos datos hasta que alcancemos la totalidad de los datos
            # indicados en los primeros 4 bytes recibidos.
            while len(response) < response_length:
                read_length = min(4096, response_length - len(response))
                response.extend(self.socket_client.recv(read_length))

            print(f"{response.decode()}\n>>> ", end='')

    def repl(self):
        """
        Captura el input del usuario.

        Lee mensajes desde el terminal y después se pasan a `self.send()`.
        """

        print("------ Consola ------\n>>> ", end='')
        while True:
            msg = input()
            response = self.send(msg)


if __name__ == "__main__":
    port = 8080
    host = socket.gethostname()

    client = Client(port, host)
```

## ¡Servidor!

```python
import socket
import threading


class Server:
    def __init__(self, port, host):
        print("Inicializando servidor...")

        self.host = host
        self.port = port
        self.socket_server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.bind_and_listen()
        self.accept_connections()

    def bind_and_listen(self):
        """
        Enlaza el socket creado con el host y puerto indicado.

        Primero, se enlaza el socket y luego queda esperando
        por conexiones entrantes.
        """
        self.socket_server.bind((self.host, self.port))
        self.socket_server.listen()
        print(f"Servidor escuchando en {self.host}:{self.port}...")

    def accept_connections(self):
        """
        Inicia el thread que aceptará clientes.

        Aunque podríamos aceptar clientes en el thread principal de la
        instancia, es útil hacerlo en un thread aparte. Esto nos
        permitirá realizar la lógica en la parte del servidor sin dejar
        de aceptar clientes. Por ejemplo, seguir procesando archivos.
        """
        thread = threading.Thread(target=self.accept_connections_thread)
        thread.start()

    def accept_connections_thread(self):
        """
        Es arrancado como thread para aceptar clientes.

        Cada vez que aceptamos un nuevo cliente, iniciamos un
        thread nuevo encargado de manejar el socket para ese cliente.
        """
        print("Servidor aceptando conexiones...")

        while True:
            client_socket, _ = self.socket_server.accept()
            listening_client_thread = threading.Thread(
                target=self.listen_client_thread,
                args=(client_socket, ),
                daemon=True)
            listening_client_thread.start()

    @staticmethod
    def send(value, sock):
        """
        Envía mensajes hacia algún socket cliente.

        Debemos implementar en este método el protocolo de comunicación
        donde los primeros 4 bytes indicarán el largo del mensaje.
        Luego, puedes elegir entre usar send() en un ciclo, o un solo sendall().
        En este ejemplo, solo usamos un sendall(), pero puedes cambiarlo por 
        el ciclo del ejemplo anterior para asegurar que toda la información
        se enviará correctamente.
        """
        stringified_value = str(value)
        msg_bytes = stringified_value.encode()
        msg_length = len(msg_bytes).to_bytes(4, byteorder='big')
        sock.sendall(msg_length + msg_bytes)

    def listen_client_thread(self, client_socket):
        """
        Es ejecutado como thread que escuchará a un cliente en particular.

        Implementa las funcionalidades del protocolo de comunicación
        que permiten recuperar la informacion enviada.
        """
        print("Servidor conectado a un nuevo cliente...")

        while True:
            response_bytes_length = client_socket.recv(4)
            response_length = int.from_bytes(
                response_bytes_length, byteorder='big')
            response = bytearray()

            while len(response) < response_length:
                read_length = min(4096, response_length - len(response))
                response.extend(client_socket.recv(read_length))

            received = response.decode()

            if received != "":
                # El método `self.handle_command()` debe ser definido.
                # Este realizará toda la lógica asociado a los mensajes
                # que llegan al servidor desde un cliente en particular.
                # Se espera que retorne la respuesta que el servidor
                # debe enviar hacia el cliente.
                response = self.handle_command(received, client_socket)
                self.send(response, client_socket)

    def handle_command(self, received, client_socket):
        print("Comando recibido:", received)
        # Este método debería ejecutar la acción y enviar una respuesta.
        return "Acción asociada a " + received


if __name__ == "__main__":
    port = 8080
    host = socket.gethostname()

    server = Server(port, host)
```

# Bonus...
