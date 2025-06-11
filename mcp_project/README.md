# Desarrollo MCP Local

En el dir de trabajo `mcp_project`, creamos un proyecto con uv para un MCP que se conectará en local por STDIO. La version que usaremos de python será 3.11.

`$ uv init --python 3.11`

Creamos el entorno virtual y lo activamos

`$ uv venv`

`$ .venv\Scripts\activate`

Instalamos las dependencias, las librerías de `arxiv`, `mcp` y el cliente para el `inspector`

`$ uv add mcp arxiv "mcp[cli]"`

Arrancamos el servidor con el `inspector`

`$ mcp dev research_server.py`
