# Diseñando una RESTful API con Python y Flask

*Basado en el articulo de Miguel Grinberg [Designing a RESTful API with Python and Flask](http://blog.miguelgrinberg.com/post/designing-a-restful-api-with-python-and-flask)*

## Qué es REST

##### Un protocolo cliente/servidor sin estado



Cada mensaje HTTP contiene toda la información necesaria para comprender la petición. Como resultado, ni el cliente ni el servidor necesitan recordar ningún estado de las comunicaciones entre mensajes. Sin embargo, en la práctica, muchas aplicaciones basadas en HTTP utilizan cookies y otros mecanismos para mantener el estado de la sesión (algunas de estas prácticas, como la reescritura de URLs, no son permitidas por REST)

##### Un conjunto de operaciones bien definidas que se aplican a todos los recursos de información

HTTP en sí define un conjunto pequeño de operaciones, las más importantes son POST, GET, PUT y DELETE. Con frecuencia estas operaciones se equiparan a las operaciones CRUD en bases de datos (ABMC en castellano: crear,leer,actualizar,borrar) que se requieren para la persistencia de datos, aunque POST no encaja exactamente en este esquema.

| Verbo HTTP	 | Acción                             |
|----------------|------------------------------------|
|   GET          | Obtiener información de un recurso |
|   POST         | Crear un nuevo recurso             |
|   PUT          | Actualizar un recurso existente    |
|   DELETE       | Eliminar un recurso existente      |

##### Una sintaxis universal para identificar los recursos.

En un sistema REST, cada recurso es direccionable únicamente a través de su URI.

##### El uso de hipermedios, tanto para la información de la aplicación como para las transiciones de estado de la aplicación

La representación de este estado en un sistema REST son típicamente HTML o XML. Como resultado de esto, es posible navegar de un recurso REST a muchos otros, simplemente siguiendo enlaces sin requerir el uso de registros u otra infraestructura adicional.

*[Wikipedia, fuente original.](https://es.wikipedia.org/wiki/Representational_State_Transfer#Descripci.C3.B3n)*

## 1. Crear nuestra primera aplicación en FLask. Todo-list Api.

**El código esta en apy_0.py**

Instalamos flask en su versión más reciente.

```sh
$ mkdir todo-api
$ cd todo-api
$ virtualenv env
...
(env)$ env/bin/pip install flask
```

Creamos en fichero `todo-api/api.py` con el código mínimo necesario para arrancar nuestra aplicación web.

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return "Hello, World!"

if __name__ == '__main__':
    app.run(debug=True)
```

Arrancamos el servidor con nuestro virtualenv activado.

```sh
(env)$ python api.py
 * Running on http://127.0.0.1:5000/
 * Restarting with reloader
```

Basta con entrar en ``http://localhost:5000`` para ver que todo funciona.

## 2. Pasos a seguir

### Prerequisitos

#### Instalación de un cliente.

1- Extension de Chrome pensada para hacer de cliente REST: [Advanced REST client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo).

2- **curl**.
- GET simple: `curl -i [API_URL]`
- Petición compleja: `curl -u [USER]:[PASS] -i -H [HEADER] -X [HTTP_VERB] -d '[BODY]' [API_URL]`

### Paso a paso

#### api_1.py

Añadimos nuestra primera ruta `GET` para obtener todos nuestros tasks y lo devolvemos en formato `JSON`.

```sh
GET: http://localhost:5000/todo/api/v1.0/tasks
```

#### api_2.py

Añadimos una ruta para poder obtener un task segun su `ID` y lo devolvemos en formato `JSON`.

```sh
GET: http://localhost:5000/todo/api/v1.0/tasks/2
GET: http://localhost:5000/todo/api/v1.0/tasks/3
```

#### api_3.py

Reimplementamos la llamada al `error 404` para eliminar el mensaje 404 en HTML que tiene por defecto Flask.

```sh
GET: http://localhost:5000/todo/api/v1.0/tasks/3
```

#### api_4.py

Añadimos una ruta para poder crear nuevas tasks utilizando el metodo `POST`.

```
POST: http://localhost:5000/todo/api/v1.0/tasks
```

```js
header: "Content-Type: application/json"
body: {"title":"Read a book"}
```

```
GET: http://localhost:5000/todo/api/v1.0/tasks
```

#### api_5.py

Añadimos una ruta para poder actualizar las tasks existentes utilizando el metodo `PUT`.
Añadimos una ruta para poder eliminar las tasks existentes utilizando el metodo `DELETE`.

```
PUT http://localhost:5000/todo/api/v1.0/tasks/2
```

```js
header: "Content-Type: application/json"
body: {"done":true}
```

```
GET: http://localhost:5000/todo/api/v1.0/tasks/2
```

#### api_6.py

Añadimos una función para añadir información a las task para mostrar su URI al pedir informacion de todas las tasks. Esto cumple con el ultimo punto de la explicación teorica de REST, *El uso de hipermedios*.

```
GET: http://localhost:5000/todo/api/v1.0/tasks
```

### api.py

==Si utilizas **curl** como cliente, cambia el código de error del método `unauthorized()` de `403` a `401`==

Ejemplo completo al que se le ha añadido una capa de autenticación mediante `HTTPBasicAuth`.