# Docker


### Info

| **Command** | **Description** |
| --------------|-------------------|
|docker version|get version|
|docker info| get hostname info|
|docker system prune -a|Remove stopped container, unused images/networks|


### Images 

| **Command** | **Description** |
| --------------|-------------------|
|docker pull registry:5000/alpine | pull image|
|docker images| list images|
|docker images ls| list images|
|docker run -dt registry:5000/alpine  |run image in background|
|docker run -it registry:5000/alpine| run image in interactive mode(terminal)|
|docker image| manage image|
|docker build -t registry:5000/alpine-mod .|build docker image(Dockerfile)|
|docker push registry:5000/alpine-mod|push the image|
|docker commit container-id registry:5000/alpine-note |commit container as an image|
|docker export -o alpine.tar image-id |export an image as tar archive|
|docker rmi -f registry:5000/alpine-note|remove image by name|
|docker rmi -f image-id| remove image by id|

### Containers

| **Command** | **Description** |
| --------------|-------------------|
|docker ps| list running containers|
|docker container ls| list container|
|docker inspect container-id| inspect a container |
|docker container| manage container|
|docker attach container-id| attach to a running container|
|docker exec -it container-id /bin/sh|execute a command in a running container|
|docker stop container-id |stop container|
|docker start container-id| start container|
|docker kill container-id| kill container |
|docker rm container-id ... container-id2 ... container-id3 .. container-idn|remove stopped containers|


### Network

| **Command** | **Description** |
| --------------|-------------------|
|docker network ls| list networks|
|docker network | manage network|

