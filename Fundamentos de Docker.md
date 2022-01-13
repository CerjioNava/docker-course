# **Fundamentos de Docker**

- INTRODUCCIÓN:
  - Problemas del desarrollo de software.
  - Virtualización vs Containerización.
  - Arquitectura de Docker.
- CONTENEDORES:
  - Conceptos generales
  - nginx
- DATOS EN DOCKER
  - a
- IMAGENES
  - a
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

### **Volúmenes**

Es la forma estándar de manejar datos en Docker.

Mostrar lista de volumenes:

      docker volume ls

Crear un volumen:

      docker volume create dbdata

Ejecutar la BBDD y montamos el volumen.
      
      docker run -d --name db --mount src=dbdata,dst=/data/db mongo (corro la BBDD y monto el volume)

Mostrar la información detallada del contenedor:

      docker inspect db

      mongo (me conecto a la BBDD)

shows dbs (listo las BBDD)
use platzi ( creo la BBDD platzi)
db.users.insert({“nombre”:“guido”}) (inserto un nuevo dato)
db.users.find() (veo el dato que cargué)

### **a**

-------------------------------------------------------------------------------

### **a**

-------------------------------------------------------------------------------

### **a** 

-------------------------------------------------------------------------------

### **a**

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