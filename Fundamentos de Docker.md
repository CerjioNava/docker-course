# **Fundamentos de Docker**

- INTRODUCCIÓN:
  - Problemas del desarrollo de software.
  - Virtualización vs Containerización.
  - Arquitectura de Docker.
- CONTENEDORES:
  - Conceptos generales.
  - nginx.
- DATOS EN DOCKER
  - Bind Mounts.
  - Volúmenes.
  - Insertar y extraer datos.
- IMAGENES
  - Concepto de imágenes.
  - Creación de imagen propia.
  - Sistema de Capas.
  - Docker commit.
- DOCKER COMO HERRAMIENTA DE DESARROLLO
  - a
- DOCKER COMPOSE
  - a
- DOCKER AVANZADO
  - a

-------------------------------------------------------------------------------

## **INTRODUCCIÓN**

### **Problemas del desarrollo de software profesional**

1.- Problems when building:
  
- Development dependencies (packages).
- Runtime versions.
- Equivalence of development environments (code sharing).
- Equivalence of production environments(go to production)
- Versions / compatibility(integration of other services e..g.: databases).
  
2.- Problems when distributing:

- Different build generations.
- Access to production servers.
- Native vs. distributed execution.
- Serverless.

3.- Problems when executing:
- Application dependencies.
- Operating System Compatibility.
- Availability of external services.
- Hardware Resources.
  
Docker allows to: 

    Build, distribute and run your code anywhere without worrying.

Para los problemas de ejecución de código, se utilizan los conceptos de contenedores, volúmenes. Para la distribución y construcción de software, entra el concepto de imágenes.

### **Virtualización**

Permite atacar en simultáneo los tres problemas del desarrollo de software profesional:

1. Peso: En el orden de los GBs. Repiten archivos en común. Inicio lento.
   
2. Costo de Administración: Necesita mantenimiento igual que cualquier otra computadora.
   
3. Múltiples formatos: VDI, VMDK, VHD, raw, etc.
   
### **Containerización**

El empleo de contenedores para construir y desplegar software.

Los contenedores son:

- Flexibles.
- Livianos.
- Portables
- Bajo acoplamiento.
- Escalables.
- Seguros.

### **Virtualizacion vs Containerización**

A diferencia de una máquina virtual, que es una abstracción del hardware y emula toda una computadora (o servidor), un contenedor es una abstracción del software y éste puede empaquetar el código, el runtime necesario y las dependencias de una aplicación.

Los contenedores se basan en el kernel del sistema operativo host y solo puede contener aplicaciones, algunas API ligeras del sistema operativo y servicios que se ejecutan en modo de usuario.

### **Arquitectura de Docker**

1. Docker Daemon: Es aquel que va a interactuar con el sistema operativo para permitirnos utilizar los contenedores y todo lo que esto respecta. Es el centro de docker y gracias a el, podemos comunicarnos con los servicios de docker.
   
2. REST API: Como cualquier otra API, es la que nos permite visualizar docker de forma “gráfica”. Asi, podemos comunicarnos con nuestra propia máquina con Daemon por HTTP, o desde una máquina remota del mismo modo gracias a la API que expone.
   
3. Docker CLI: El cliente o Command Line Interface se comunica con el Docker Daemon con las instrucciones que le mandamos a través del CLI.
   
Dentro de la arquitectura de Docker encontramos:

1. Containers: Es la razón de ser de Docker, es donde podemos encapsular nuestras imagenes para llevarlas a otra computadora, o servidor, etc.
   
2. Images: Son los artefactos que utiliza Docker para empaquetar contenedores y para poder distribuirlo a través de distintas instalaciones de Docker
   
3. Data Volumes: Es la forma en que Docker nos permite acceder, con seguridad y de manera flexible, al sistema de archivos de la máquina anfitriona sin comprometer la seguridad del mismo.
 
4. Network: Permite a los distintos contenedores de Docker comunicarse entre sí o con el mundo exterior.

-------------------------------------------------------------------------------

## **CONTENEDORES**


- Es una agrupación de procesos.

- Es una entidad lógica, no tiene el limite estricto de las máquinas virtuales, emulación del sistema operativo simulado por otra más abajo.

- Ejecuta sus procesos de forma nativa.

- Docker corre de forma nativa solo en Linux.

- Los procesos que se ejecutan adentro de los contenedores ven su universo como el contenedor lo define, no pueden ver mas allá del contenedor, a pesar de estar corriendo en una maquina más grande.

- No tienen forma de consumir más recursos que los que se les permite. Si esta restringido en memoria ram por ejemplo, es la única que pueden usar.

- A fines prácticos los podemos imaginar cómo maquinas virtuales, pero NO lo son. Máquinas virtuales livianas.

- Sector del disco: Cuando un contenedor es ejecutado, el daemon de docker le dice, a partir de acá para arriba este disco es tuyo, pero no puedes subir mas arriba.

- Docker hace que los procesos adentro de un contenedor este aislados del resto del sistema, no le permite ver más allá.

- Cada contenedor tiene un ID único, también tiene un nombre.

### **Algunos comandos generales:**

- Ejecutar un nuevo contenedor de Docker:
  
      docker run <image_name>
      docker run hello-world (corro el contenedor hello-world)

- Ejecutar un nuevo contenedor a partir de una imagen:

      docker run -it -d <image_name>

- Mostrar contenedores activos:             

      docker ps
      docker ps -aq   (SOLO LOS ID)

- Mostrar todos los contenedores:

      docker ps -a 

- Mostrar detalles de un contenedor (ID o name):

      docker inspect <container_id> 
      docker inspect <name> 

- Ejecutar y asignar custom name:

      docker run --name <custom_name> <image_name>
      docker run --name hello-platzi hello-world

- Cambiar nombre:

      docker rename <image_name> <new_name>
      docker rename hello-platzi hola-platzy

- Eliminar un contenedor (ID o name):

      docker rm <container_id>
      docker rm <name>

- Eliminar un contenedor (ID o name) activo:

      docker rm -f <container_id>
      docker rm -f <name>

- Eliminar todos los contenedores detenidos:

      docker container prune

- Eliminar todos los contenedores:

      docker rm -f

- Acceder a un contenedor activo:

      docker exec -it <container_id> bash

- Detener un contenedor activo:

      docker stop <container_id>

- Detener inmediatamente la ejecución de un contenedor:

      docker kill <container_id>

...

https://www.edureka.co/blog/docker-commands/

**NOTA: Detach (-d o --detach)**

El detach desvincula el standar input/output del contenedor, haciendolo correr en background.

### **Modo interactivo (-it)**

Modo interactivo para entrar al contenedor y ejecutar comandos dentro del mismo.

* Main process: Determina la vida del contenedor, un contenedor corre siempre y cuando su proceso principal este corriendo.

* Sub process: Un contenedor puede tener o lanzar procesos alternos al main process, si estos fallan el contenedor va a seguir encedido a menos que falle el main.

Entramos en el CLI del contenedor:

    docker run ubuntu       (Ejecuta nuevo contenedor ubuntu)
    docker run -it ubuntu   (Ejecuta y accede al interactivo de ubuntu)

El contenedor se detiene si el Main Process se detiene.

    docker run --name alwaysup -d ubuntu tail -f /dev/null 
    (El "tail -f /dev/null" será el comando a ejecutar el contenedor)

Para ejecutar un comando o proceso (distinto al main process) en un contenedor activo:

    docker exec -it alwaysup bash

Se puede matar un Main process desde afuera del contenedor, esto se logra conociendo el id del proceso principal del contenedor que se tiene en la maquina. Para saberlo se ejecuta los siguientes comandos:

    docker inspect --format '{{.State.Pid}}' alwaysup
    kill 2474       (el id obtenido anteriormente)

Batch como Main process

Agujero negro (/dev/null) como Main process

### **nginx**

Es uno de los webserver y reverse proxy más importantes del mundo.

Ejecuto nginx:

      docker run -d --name proxy nginx

IMPORTANTE: Cada contenedor tiene su propio stack de networking e interfaz de redes virtual. En PORTS se observa el puerto al que está escuchando el contendor, sin embargo, ese puerto no es el de nuestra máquina, así que debemos llevarlo a nuestra máquina. 

Detengo el contenedor:

      docker stop proxy

Elimino el contenedor:

      docker rm proxy (borro el contenedor)

NOTA: No se puede eliminar un contenedor si este se encuentra activo.

Detengo y elimino el contenedor:
            
      docker rm -f <contenedor> (lo para y lo borra)

Si quiero exponer un contenedor a un puerto de mi máquina. Entonces ejecuto nginx y expongo el puerto 80 del contenedor en el puerto 8080:
 
      docker run -d --name proxy -p 8080:80 nginx

NOTA: "-p" es de publish/port y asigna ---> **puerto_maquina:puerto_contenedor**.

localhost:8080 (desde mi navegador compruebo que funcione)
            
Logs del contenedor:

      docker logs proxy
      docker logs -f proxy                (follow)
      docker logs --tail 10 -f proxy      (10 últimas entradas del log)

-------------------------------------------------------------------------------

## **DATOS EN DOCKER**

Hay que recordar que los contenedores son entidades autocontenidas que no pueden acceder al SO de la máquina. Aún así, es necesario trabajar con data, como accedemos a ella?

### **Bind Mounts**
¬
Creo un directorio en mi máquina para la data:

      mkdir dockerdata

Ejecutamos un contenedor para mongo y entramos al bash:

      docker run -d --name db mongo
      docker exec -it db bash

DENTRO DEL BASH: 
      
      Nos conectamos a la base de datos:  mongo
      Mostramos las BBDD:                 shows dbs 
      Creo la BBDD platzi:                use platzi
      Inserto un nuevo dato:              db.users.insert({“nombre”:“guido”}) 
      Veo el dato cargado:                db.users.find()

Si salgo del contenedor y lo elimino, al crear uno nuevo ya los datos no existen porque se perdieron con el contenedor.

IMPORTANTE: Necesito un directorio en mi máquina con una versión de la misma, que pueda ocurrir dentro del contenedor, para esto utilizamos un Bind Mounts.

Creamos el directorio:

      mkdir       mongodata
      cd          mongodata
      pwd         (obtenemos el path del directorio)

Ejecutamos el contenedor de mongo en modo detach, con un bind mount:

      docker run -d --name db -v <path_de_mi maquina>:<path_dentro_del_contenedor(/data/db_mongo)>

      docker run -d --name db -v /home/developer/Documentos/Github/docker-course/dockerdata/mongodata:/data/db mongo

Accedemos nuevamente al contenedor, añadiremos data, eliminaremos y al ejecutar un nuevo contenedor con las mismas rutas, veremos como los datos persistiran:

      docker exec -it db bash
            mongo
                  use platzi
                  db.users.insert({“nombre”:“guido”})
                  db.users.find()
                  exit
            exit
      docker rm -f db

      docker run -d --name db -v /home/developer/Documentos/Github/docker-course/dockerdata/mongodata:/data/db mongo

      docker exec -it db bash
            mongo
                  use platzi
                  db.users.find()

NOTA: Mongo guarda datos por defecto en la ruta "/data/db".

Los Bind Mounts permiten compartir data entre contenedores y la máquina anfitriona, atando una ruta dentro de la máquina que ejecuta el contenedor con una ruta dentro del contenedor. Lo que ocurra dentro del contenedor en dicha ruta, se verá reflejado en la máquina anfitriona.

Sin embargo, hay problemas de seguridad al utilizar Bind Mounts ya que el contenedor tendrá acceso a todo lo que se encuentre en dicha ruta. 

Por ello, nacieron los volúmenes.

### **Volúmenes**

Es la forma estándar de manejar datos en Docker.

Mostrar lista de volumenes:

      docker volume ls

Crear un volumen:

      docker volume create dbdata

Ejecutar la BBDD y montamos el volumen, indicando Source (src) y Destino (dst).
      
      docker run -d --name db --mount src=dbdata,dst=/data/db
      
      mongo (corro la BBDD y monto el volume)

Mostrar la información detallada del contenedor:

      docker inspect db

      mongo (me conecto a la BBDD)

            shows dbs 
            use platzi
            db.users.insert({“nombre”:“guido”})
            db.users.find() 

Mismo procedimiento borrando el contenedor y rehaciendolo para comprobar que la data siga allí.

      docker run -d --name db --mount src=dbdata,dst=/data/db
      ...

### **Insertar y extraer archivos de un contenedor**

Creo un archivo en mi máquina:

      touch prueba.txt

Ejecuto un contenedor de ubuntu con tail para permanecer activo, y entro al contenedor:

      docker run -d --name copytest ubuntu tail -f /dev/null
      docker exec -it copytest bash 

Creamos un directorio en el directorio del contenedor y salimos:

      mkdir testing 
      exit

Copiamos el archivo dentro del contenedor:

      docker cp prueba.txt copytest:/testing/test.txt

      docker cp <file> <container_name>:<inside_container_root>

Si entramos al contenedor y revisamos la carpeta "testing" que habíamos creado, ahora contiene el archivo que acabamos de copiar.

Copiamos el directorio de un contenedor a mi máquina (es lo opuesto):

      docker cp copytest:/testing localtesting
      
      docker cp <container_name>:<inside_container_root> <new_dir_name>

NOTA: Con “docker cp” no hace falta que el contenedor esté corriendo

NOTA: Puedo usar "docker cp" aún si el contenedor no está activo.

Resumen:

- Host: Donde Docker esta instalado (mi máquina).
  
- Bind Mount: Vincula directamente el sistema de archivos con el contenedor, guarda los archivos en la maquina local persistiendo y visualizando estos datos (No seguro). 
  
- Volume: Parecido al Bind Mount, a diferencia de que el área del sistema de archivos que el contenedor maneja, es docker quien lo administra (Seguro).
  
- TMPFS Mount (Temporary File System Mount): Guarda los archivos temporalmente y persiste los datos en la memoria del contenedor, cuando muera sus datos mueren con el contenedor. Esto solo en Linux.

-------------------------------------------------------------------------------

### **IMÁGENES**

Las imágenes son plantillas o moldes a partir de las cuales Docker crea contenedores. Es una pieza de software liviana que contiene lo necesario para que un contenedor pueda ejecutarse en cualquier dependencia.

Una imágen es un conjunto de distintas capas de datos (distribución, diferente software, librerías y personalización), es decir, una imágen se conforma de distintas capas de personalización en base a una capa inicial (base image, el más puro estado del SO).

El Tag de una imagen apunta a la capa superior de una imagen.

Muestro las imagenes actuales:

      docker image ls

Histórico de una imagen:

      docker history <image>

Bajar la imagen o una nueva versión desde un repositorio externp:

      docker pull ubuntu:20.04

Eliminar una imagen:

      docker image rm -f <image_id>

Por defecto, el repositorio público es Docker Hub, pero podríamos utilizar en algún momento alguno privado.

### **Construyendo una imagen propia**

Creamos directorio de imagenes, entramos al directorio y creamos un Dockerfile:

      mkdir imagenes 
      cd imagenes 
      touch Dockerfile 
      code .

Contenido del Dockerfile (comando a ejecutar en tiempo de build), hay que partir de alguna imagen anterior: 

      FROM ubuntu:latest
      RUN touch /usr/src/hola-platzi.txt 

Creamos una imagen con contexto Build <directorio>, tomando de este directorio el Dockerfile:

      docker build -t ubuntu:platzi .

Ejecutamos el contenedor con la nueva imagen:

      docker run -it ubuntu:platzi 

Logeamos en docker hub:

      docker login

Cambiamos el tag "ubuntu" que es de su repositorio público, para poder subirla al docker hub y publicamos:

      docker tag ubuntu:platzi miusuario/ubuntu:platzy
      docker push miusuario/ubuntu:platzi

### **Sistema de Capas**

En Docker Hub podemos ver los Dockerfile de las imagenes que descargamos, y cada comando en el Dockerfile es una capa.

Si accedemos al histórico de una imagen, se pueden observar las distintas capas creadas por el Dockerfile de la imagen y el historial de capaz:

      docker history <image>

**IMPORTANTE:** Hay una herramienta llamada "Dive", que nos permite navegar entre el histórico entre las capas de una imagen de Docker y su configuración.

      dive ubuntu:platzi

      dive <image>
      
Usamos Tab y Ctrl+U para navegar entre los cambio del histórico.

Ahora, si añadimos al Dockerfile una nueva linea de comando (una nueva capa) y rehacemos el build:

      FROM ubuntu:latest
      RUN touch /usr/src/hola-platzi.txt 
      RUN rm /usr/src/hola-platzi.txt 

      docker build -t ubuntu:platzi .

Vemos como se rehacen las capas y ahora se muestra la nueva capa adicional. Los cambios de la imagen se generan en dicha nueva capa, como si fuera una especie de nuevo commit.

### **Extra: Docker Commit**

Docker commit tiene un funcionamiento muy parecido al commit de git, guarda el estado de la capa mutable en un tag de la imagen a partir de la cual se generó el contenedor creando una capa más para crear uno o más contenedores a partir de esta nueva imagen.

-------------------------------------------------------------------------------

### **DOCKER COMO HERRAMIENTA DE DESARROLLO** 

Descargamos el siguiente repositorio:

      git clone https://github.com/platzi/docker

Si vemos el Dockerfile del proyecto:

      # Imagen base (node en la version 12)
      FROM node:12

      # Copiar todo lo que hay en la carpeta actual a la nueva
      # ruta dentro del contenedor.
      COPY [".", "/usr/src/"]

      # Establecer el directorio de trabajo (cd /usr/src).
      WORKDIR /usr/src

      # Descarga las dependencias del proyecto.
      RUN npm install

      # Le dice al contenedor que escuche el puerto 3000 de la máquina.
      EXPOSE 3000 

      # Ejecuta el comando node index.js
      CMD ["node", "index.js"]

Creamos la imagen local:

      docker build -t platziapp . 

Creamos un contenedor a partir de esta imagen y si este contenedor se detiene, se eliminará. Esto se publicará en el puerto 3000:

      docker run -d --rm -p 3000:3000 platziapp 

### **Caché de capas para estructurar correctamente tus imágenes**

Se habla del caché porque si realizamos algún cambio entre alguno de los archivos del proyecto, no queremos que cada vez que construyamos la imagen se instalen todas las dependencias de nuevo. Es decir, no instalar todo solo por modificar código.

Creo la imagen local:

      docker build -t platziapp . 

El Dockerfile se observa:

      FROM node:12

      COPY [".", "/usr/src/"]

      WORKDIR /usr/src

      RUN npm install

      EXPOSE 3000

      CMD ["node", "index.js"]

Cambiaremos a:

      FROM node:12

      COPY ["package.json", "package-lock.json", "/usr/src/"]

      WORKDIR /usr/src

      RUN npm install

      COPY [".", "/usr/src/"]

      EXPOSE 3000

      CMD ["node", "index.js"]

Ahora, si queremos que el contenedor se actualice cada vez que realicemos un cambio en el código, sin necesidad de hacer build siempre, compartiremos nuestro código con el contenedor. Además haremos uno de Nodemon.

Cambiamos:

      CMD ["node", "index.js"]
      # Ahora será:
      CMD ["npx", "nodemon", "index.js"]

Ejecuto un contenedor y monto el archivo index.js para que se actualice dinámicamente con nodemon que está declarado en mi Dockerfile:

      docker run --rm -p 3000:3000 -v /home/developer/Documentos/Github/docker/index.js:/usr/src/index.js platziapp

### **Docker networking: colaboración entre contenedores**

Hasta ahora, la comunicación con Mongo ha sido suponiendo que la base de datos se encuentre dentro del mismo contenedor. Ahora comunicaremos la base y proyecto dentro de una misma red.

**IMPORTANTE:** La base para crear todo tipo de aplicaciones es la comunicación entre contenedores en una red propia.

Mostrar lista de redes:

      docker network ls 

Aquí observamos tres redes por defecto:

  * Bridge: Es la red default, posee retrocompatibilidad con versiones anteriores. 
  
  * Host: Es una representación en Docker de la red real de mi máquina. A través de esta red podría permitir que el contenedor tenga acceso a la interfaz real de la máquina.
  
  * None: Es una red especial de Docker que no permite que un contenedor tenga algún tipo de acceso a la red. 

Creamos la red:

      docker network create --attachable plazinet 

NOTA: "--attachable" permite que otros contenedores puedan conectarse a dicha red.

Mostrar los detalles de la red:

      docker inspect plazinet 

Ejecuto un contenedor y conecto al contenedor hacia la red creada:

      docker run -d --name db mongo
      docker network connect plazinet db

Ahora si inspeccionamos la network, se observa en el estatus al contenedor de mongo conectado a la red.

Ejecuto un nuevo contenedor y le pasamos un valor para la variable que recibe en el env (podemos ver esto dentro del index.js).

      docker run -d --name app -p 3000:3000 --env MONGO_URL=mongodb://db:27017/test platziapp

      # MONGO_URL=<protocolo><hostname>:<puerto>/<host_name> <image>

NOTA: Contenedores pueden encontrarse en la misma red a través del "host_name" como se acaba de apuntar.

Ahora si conectamos dicho contenedor a la red.

      docker network connect plazinet app 

Ahora si inspeccionamos nuevamente la network, ambos contenedores (mongo y proyecto) estan conectados a la misma red, y en el puerto local 3000 se obtiene la respuesta esperada por el index.js.

-------------------------------------------------------------------------------

### **DOCKER COMPOSE**

Docker compose es una herramienta que simplifica el uso docker, permitiendonos describir con una estructura declarativa la arquitectura de nuestra aplicación. Dicha herramienta utiliza un compose file (docker-compose.yml).

A partir de archivos YAML es mas sencillo crear contendores, conectarlos, habilitar puertos, volumenes, etc.

Con Compose puedes crear diferentes contenedores y al mismo tiempo, en cada contenedor, diferentes servicios, unirlos a un volúmen común, iniciarlos y apagarlos, etc. Es un componente fundamental para poder construir aplicaciones y microservicios.

Docker Compose te permite mediante archivos YAML, poder instruir al Docker Engine a realizar tareas, programáticamente. Y esta es la clave, la facilidad para dar una serie de instrucciones, y luego repetirlas en diferentes ambientes.

En el archivo "docker-compose.yml" de nuestro proyecto:

      version: "3.8"

      services:
            app:
                  image: platziapp
                  environment:
                        MONGO_URL: "mongodb://db:27017/test"
                  depends_on:
                        - db
                  ports:
                        - "3000:3000"
            db:
                  image: mongo

  * services: Son los servicios que componen a nuestra aplicación, para que esta se ejecute correctamente. Un servicio de compose es parecido parecido al container, solo que a diferencia de un contenedor, un servicio puede tener uno o más contenedores de la misma imagen.
    
  * image: La imagen para el contenedor.
  
  * environment: Para definir las variables de entorno.
    
  * depends_on: indica de que otros servicios depende, si uno de estos no funciona o no se levantó primero, el servicio que depenga de ellos no se levantará.
  
  * ports: Los puertos donde se expone la aplicación. 

Para crear todo lo declarado en el archivo "docker-compose.yml":

      docker-compose up -d 

NOTA: El prefijo de los container se asocian automáticamente al nombre de la carpeta/proyecto que los contiene, de manera que estos no tengan conflictos entre si dentro del mismo servicio.

### **Algunos sub comandos**

Docker Compose conecta de manera automática a todos los contenedores de un mismo servicio a una red.

Lista de redes, definición de red:

      docker network ls
      docker network inspect docker_default

Se observa que docker-compose se encarga de asociar el nombre del hostname al mismo nombre del contenedor respectivo, cosa muy necesaria.

Muestro todos los logs del docker compose, o solo les de la app:

      docker-compose logs 
      docker-compose logs app 
      docker-compose logs -f app 

Entramos al shell del contenedor app:

      docker-compose exec app bash

Vemos los contenedores generados por el docker compose y eliminamos todo:

      docker-compose ps

Eliminamos y limpiamos el estado de nuestro compose:

      docker-compose down

### **Docker como herramienta de desarrollo**

-------------------------------------------------------------------------------

### **a**

-------------------------------------------------------------------------------

### **a**

-------------------------------------------------------------------------------

### **EXTRA: Definiciones**

- -d: Detach. Desvincula el standar input/output del contenedor.

- -p: Proxy/Port. Define el puerto donde correr contenedor.

- -f: Follow. Para seguir en tiempo real una ejecución de proceso.

- -it: Interactive. Modo interactivo al acceder a contenedor.

- -a: All. Muestra todos (all) en "docker ps".

- -aq: ____. Solo los ids en "docker ps".

- -f: Force. 
  
- -v: ____. Especificamos Bind Mount.

-------------------------------------------------------------------------------



-------------------------------------------------------------------------------


-------------------------------------------------------------------------------