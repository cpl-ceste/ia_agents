# IA aplicacion de web IA con multiples contenedores

En el area de IA han surgido modelos de lenguaje muy potentes (Large Language Models - LLMs) que nos permiten crear interfaces de usuario cognitivas, es decir que soportan unas funcionalidades de lenguaje inteligentes y pueden chatear, entender el audio y el texto que los usuarios utilizan para comunicarse con la aplicacion.

Vamos a desplegar una aplicacion web que nos permite crear chatbots inteligentes e interactuar con ellos por texto o voz. Para ello utilizaremos dos aplicaciones que descargaremos como contenedores y ejecutaremos con docker-compose. Una de ellas es el motor de ejecucion de los LLMs y la otra es la interfaz web que nos permite trabajar con este motor.

Utilizaremos:

1- **Ollama**: [Ollama](https://ollama.com/) es una herramienta que permite ejecutar modelos grandes de lenguaje (LLMs) de inteligencia artificial de forma local en tu ordenador, sin necesidad de conexión a internet. Esto garantiza que todos los datos y procesos se mantengan en tu dispositivo, ofreciendo mayor privacidad y control. Ollama facilita la instalación y uso de diversos modelos open-source de IA  (LLAMA-3, DeepSeek-R1, Gemma 3, etc).

2- **WebUI**: [​Open WebUI](https://openwebui.com/) es una plataforma de inteligencia artificial autoalojada, extensible y fácil de usar, diseñada para operar completamente sin conexión a internet. Permite a los usuarios ejecutar y gestionar modelos de lenguaje de gran tamaño (LLMs) localmente a través de una interfaz web intuitiva. Permite integrarse con Ollama y APIs compatibles de modelos (i.e. OpenAI), puede ejecutarse sobre CPU y GPU (cuda) y tiene otras muchas funcionalidades (motor para RAG, etc.)

1)  Navegamos al directorio `/web-ai` y abrimos el fichero `docker-compose.yaml` o el fichero `docker-compose-cpu-based.yml`

Analizamos las partes del fichero, vemos los servicios que esta utilizando, los puertos y los volumenes que mapea en nuestro entorno local.

Analizamos las imagenes que usa cada uno de los servicios. Una de las imagenes vamos a crearla utilizando el `Dockerfile` que tenemos en el directorio. Analizamos el `Dockerfile` para entender la imagen que estamos creando. Vemos que hay un fichero `entrypoint.sh` que estamos utilizando para arrancar y lo  ¿que similitudes y diferencias observamos?

2) Levantamos la aplicacion en modo dettached, indicando que debe de construir la imagen en el despliegue y el fichero `yaml` que describe el despleigue.

Ejecutamos el comando para desplegarlo para que corra sobre GPU `$ docker compose -f docker-compose.yml up -d --build`

Analizamos como se ha construido la imagen e inspeccionamos los contenedores

4) Probamos la aplicación y vemos que es un entorno web de Chatbots de IA totalmente configurable para el despliegue de diferentes modelos de LLM

5) Podemos instalar más modelos para nuestro ChatBot. Navegamos a la web de Ollama para ver los modelos que soporta [Ollama](https://ollama.com/search) e instalamos alguno de ellos. 

Para instalarlo podemos abrir una terminal en el contenedor de `ollamadeepseek` y ejecutar un comando `ollama run gemma3:1b ` para bajarnos un modelo pequeñito, que será mas rapido.

Estos cambios desapareceran si borramos el contenedor y para hacerlos persistentes en realidad podemos incluirlos en el `Dockerfile`o en el fichero `entrypoint.sh`, pero para verlo de forma rápida modificando el contenedor nos bastará.

Probamos nuestra aplicación web (si no reconoce el nuevo modelo de forma dinamica podemos reiniciarla) y vemos que podemos abrir varios chats con cada modelo.

6) Probamos otros comandos de docker compose donde podemos trabajar con los contenedores individuales, identificados por el nombre del `service` que hemos definido en el fichero `compose.yaml`:

`$ docker compose ps`

`$ docker compose stop <service>`

`$ docker compose start <service>`

`$ docker compose logs <service>`

7) Ejecutamos el comando para borrar los contenedores y borramos redes y volumenes que queden sin borrar

`$ docker compose down`


