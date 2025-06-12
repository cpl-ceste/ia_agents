# Desarrollo MCP Remoto (MCP Server SSE)

En el dir de trabajo `sse_mcp_project`, ejecutaremos el servidor en un contenedor.

1) Para crear la imagen, inicializamos el proyecto y preparamos las dependencias

`$ uv init --python 3.11`

Creamos el entorno virtual y lo activamos

`$ uv venv`

`$ .venv\Scripts\activate`

Instalamos las dependencias, las librerías de `arxiv`, `mcp` y el cliente para el `inspector`

`$ uv add mcp arxiv "mcp[cli]" mcpo`

Opcionalmente podemos comprobar que todo funciona con una conexion STDIO con el `inspector`

`$ mcp dev research_server.py`

Finalmente construimos la imagen desde el directorio donde esta nuestro `Dockerfile`

`$ docker build -t arxiv-mcp-server .`

2) Cuando tenemos la imagen podemos correr nuestro servidor, exponiendo el puerto `8002` en el host, con el siguiente comando desde el cmd de windows, por ejemplo. Notese que mapeamos el directorio donde se almacenan los resultados de las busquedas, para poder acceder a ellas desde fuera del contenedor

`$ docker run -p 8002:8000 -v //c//Users/Carmen/_demo/sse_mcp_project/papers:/app/papers arxiv-mcp-server`

Arrancamos el `inspector`, que tambien puede arrancarse con un comando de Node

`$ npx -y @modelcontextprotocol/inspector`

3) Arrancamos MCPO que expondra en modo API REST las herramientas de nuestro servidor MCP en el  puerto 8001

`mcpo --port 8001 --server-type "sse" -- http://127.0.0.1:8002/sse`

El API REST Que expone MCPO es Open API compliant y podemos visitar la web de documentacion que crea automaticamente con `Swagger UI`.

4) Finalmente ya podemos utilizar en Open WebUI nuestro servidor

5) Esto puede hacerse cualquier otro servidor MCP que queramos conectar y nos permite tener un asistente mucho mas inteligente.

Por ejemplo, para conectar el [MCP server de excell](https://github.com/haris-musa/excel-mcp-server), podemos clonar el repo de github y segir los siguientes pasos:

Es necesario subir la version de python a 3.11 en el `project.toml` y `.python-version` para que pueda instalarse mcpo.

Creamos el entorno virtual y descargar mcpo para conectarlo a Open WebUI

`$ uv venv`

`$ .venv\Scripts\activate`

`$ uv add mcpo`

Lanzar el servidor MCP de excell con protocolo de transporte SSE 

`$ cd src/excel_mcp`

`$ uvx excel-mcp-server sse`

Podemos arrancar el inspector en otra consola y conectarnos con SSE en el puerto 8000

`$ npx -y @modelcontextprotocol/inspector`

Podemos arrancar mcpo en el puerto 8008 y verlo en swagger 

`$ mcpo --port 8008 --server-type "sse" -- http://127.0.0.1:8000/sse`

Y conectarnos a el desde Open Web UI

`http://localhost:8008/docs`
