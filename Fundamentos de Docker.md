# **Fundamentos de docker**

- INTRODUCCIÓN:
  - Problemas del desarrollo de software.
  - Virtualización vs Containerización.
  - Arquitectura de Docker.
- CONTENEDORES:
  - 
- DATOS EN DOCKER
- IMAGENES
- DOCKER COMO HERRAMIENTA DE DESARROLLO
- DOCKER COMPOSE
- DOCKER AVANZADO

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
4. 
5. Network: Permite a los distintos contenedores de Docker comunicarse entre sí o con el mundo exterior.

-------------------------------------------------------------------------------