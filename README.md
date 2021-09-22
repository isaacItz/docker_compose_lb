# docker_compose_lb


## Contenedor Docker Compose de Python/Flask y un balanceador de carga (Load Balancer)
1) Crear un nuevo repositorio con visibilidad pública en GitHub
* Nombre: `docker_compose_lb`
* Descripción: `Ejemplo para contenedor Python con Redis usando Docker Compose y un Balanceador de Carga`
2) Copiar los archivos desde la URL https://github.com/Juli-BCN/docker_compose_lb
* Cada uno puede utilizar el método preferido para copiar como hacerlo a mano, usar un clon en Visual Studio (git clone + git push) o hacer un "Fork" desde GitHub
3) Modificar las línea 4 para agregar los valores personales de cada uno:
```
LABEL maintainer="JuliBCN <julibcn@gmail.com>"
```
4) Vamos a comprobar el contenido del archivo llamado `docker-compose.yml`:
```
version: "3"
services:
    web:
        build: .
    redis:
        image: redis
        volumes:
            - "./data:/data"
        command: redis-server --appendonly yes
    lb:
        image: dockercloud/haproxy
        ports:
            - 22222:80
        links:
            - web
        volumes:
            - /var/run/docker.sock:/var/run/docker.sock
```
5) Primero instalamos `docker-compose` si no estuviese instalado, lo que podemos asegurar con el comando `docker-compose --version`:
> docker-compose --version

6) Vamos a probar a crear la apliación en 2 contenedores con el comando `docker-compose up` y ver qué pasa:
> docker-compose up -d --scale web=2
* Tenemos dos contenedores detrás del balanceador de carga, así que cada llamada cambiará el nombre del contenedor


## Parada del contenedor Docker Compose de Python/Flask y un balanceador de carga (Load Balancer)
Para parar un contenedor en ejecución, primero hemos de saber el ID de dicho contenedor con el comando `docker ps`. Una vez averiguado, podemos pararlo con `docker stop`. Pero ahora hay 4 contenedores para apagar.... o utilizar el comando `docker-compose stop`:
> docker-compose stop

Si queremos además borrar los volúmenes:
> docker-compose down -v
