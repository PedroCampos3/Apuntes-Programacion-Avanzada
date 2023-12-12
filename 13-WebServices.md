##### Semana 13 - Webservices

# Webservices

## API (Application Programming Interface)

**API**: Funciones expuestas por un servicio para que la utilicen otro programas

Ej: El servicio podría ser una clase y la API sus métodos

Las APIs no necesariamente deben estar en el sistema local.

## HTTP (Hypertext Transfer Protocol)

Es un protocolo __request-response__ ya que el cliente hace una solicitud (__request__) y el servidor responde (__response__)

Para funcionar se basa en la definición de algo que indica una acción sobre un recurso, que puede ser algun dato.

La versión HTTP/1.1 incluye:
1. **GET**: Obtiene algo del servidor sin cambiar nada en este
2. **POST**: Crea un recurso
3. **PATCH**: Modifica un recurso
4. **PUT** Reemplaza totalmente algún recurso
5. **DELETE**: Elimina un recurso.

HTTP también tiene codigos que indican el estado de la respuesta (Ej: 404)

## Client-side script

Ahora como hacer un request a un server con un servicio web en python con la librería `requests`

**Generar petición**: `.get(url)` (retorna algo así que hay que asignarlo a una variable)

**Saber estado de la consulta**: Método de la response `.status_code`

_Algunas respuestas pueden ser transformadas a dict con json:_ `response.json()`

## Ejemplo:

Todas las APIs tienen una **URL BASE**.
Ej: `"https://fakerapi.it/api/v1/"`

y lo que viene después de ese "prefijo" son los endpoints.
Ej: `https://fakerapi.it/api/v1/addresses`

Cada endpoint tiene sus parámetros y podemos pasarselo como _kwarg_ en `get()`

**_NO todas las APIs responden en JSON_**

_Por seguridad muchas APIs piden clave_

## Uso de `post`

Para simular un API real usaremos la API de JSONPlaceholder

Con `POST` podemos enviar info a la API pasandole un diccionario al parámetro `data`

Ej:
```python
noticia = requests.post('https://jsonplaceholder.typicode.com/posts', data=data)
```

## Autenticación

Las requests  `.put()`, `.patch()` o `.delete()` generalmente hacen cambio en el servidor, para eso se necesitan autorización y usaremos el parámetro `headers` para eso.

La forma de usarlo también es creando un diccionario y crear algo como `{"Authorization": "MI-TOKEN"}` y después
```python
requests.post('https://jsonplaceholder.typicode.com/posts', headers=my_headers, data=data)
```

## EJEMPLO con Git

```python
import json

github_repo = 'juanito-iic2233-20XX-1'
token = "COMPLETAR"

body = {
    'title': "Creando una issue con la API",
    'body': "Ahora tengo el poder para hacer issues desde Python! 🎉"
}

my_headers = {
    'Authorization': 'token ' + token,
    'Accept': 'application/vnd.github.v3+json'
}

url = f"https://api.github.com/repos/IIC2233/{github_repo}/issues"

response = requests.post(url, data=json.dumps(body), headers=my_headers)
response.status_code
```

## Server-side App

La parte del servidor puede estar en otro lenguaje distinto al del cliente.

Como vamos a usar python, usaremos `WSGI`

Primero crearemos otro archivo servidor

después:
```python
solicitud = requests.get(URL)
solicitud.status_code
solicitud.json()
```

