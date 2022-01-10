# **Fundamentos de Docker**

- INTRODUCCIÓN:
  - Problemas del desarrollo de software.
  - Virtualización vs Containerización.
  - Arquitectura de Docker.
- CONTENEDORES:
  - a
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

### **Algunos comandos:**

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

### Detach (-d o --detach)


-------------------------------------------------------------------------------


-------------------------------------------------------------------------------



-------------------------------------------------------------------------------


-------------------------------------------------------------------------------