# Docker

## Why?
* to prevent matrix from hell (= compatibility/dependency issues, long setup time, different dev/test/prod environments)
* --> containerize apps and run each service with its own dependencies in separate containers

<br>

## Structure
* Hardware Infrastructure > OS > Docker > Container
* Host where docker is installed: 'docker host' or 'docker engine'

<br>

## Containers
* are running instances of images
* meant to host a specific task or process
* are completely isolated environments
* share the same OS kernel (not meant to host an OS)
* application + libraries + dependencies
* can easily provision applications and scale them
* run several instances of the same app for load balancing. If one fails, destroy and launch new one.
* only lives as long as the process inside it is alive

<br>

## VMs
* have additionally the OS in them and instead of Docker a Hyervisor
* need more size, higher utilization, booting is slower
* though complete isolation from kernel
* can run different applications built on different OS
* can easily provision or decommission docker hosts

<br>

## Images
* is a package / template / plan
* used to create a container
* handed over from dev to ops (is guaranteed to run)

<br>

## Docker Editions
* Community Edition (free products) & Enterprise Edition (Enterprise Add-ons)

<br>

## Docker commands

### Setup
* Docker version: `sudo docker version`
* Create whale image: `docker run docker/whalesay cowsay 'Muh, muh!'` for https://hub.docker.com/r/docker/whalesay
* Uninstall old versions: `sudo apt-get remove docker docker-engine docker.io containerd runc`
* Setup repo:
   ```
   sudo apt-get update
   sudo apt-get install ca-certificates curl gnupg
   ```
add Dockers GPG key:
   ```
   sudo install -m 0755 -d /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   sudo chmod a+r /etc/apt/keyrings/docker.gpg
   ```
Setup repo:
   ```
   echo \
     "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
     "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
* Install latest Docker Engine:
    ```
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
Verify:
`sudo docker run hello-world`

<br>

### Containers
* list of running containers: `docker ps`

* list of running *and* exited containers: `docker ps -a`
* stop container `docker stop <name_of_container>` or `docker stop <name_of_id`
* remove container: `docker rm <name_of_container>`
* more details about a container: `docker inspect <name_of_container>`
* container logs: `docker logs <name_of_container>`

<br>

### Images
* list images: `docker images`

* download image: `docker pull <name_of_image>`
* run image (attached mode): `docker run <name_of_image>` (pulls image from Docker Hub if not present locally, output can be seen)
* run container from image in detached/background mode: `docker run -d <name_of_image>`
* run container from image again attached: `docker attach <first_few_chars_of_container_id>`
* run specific version: `docker run <name_of_image>:<tag>` (no tag: default is `latest`)
* remove image: `docker rmi <name_of_image>` (stop & remove all dependent containers before)
* remove image with tag: `docker rmi <name_of_image:tag>`

<br>

### Flags
* force `-f` or `--force`
* give container name: `--name <name>`
* interactive mode: `i` (docker runs by default in a non-interactive mode)
* attached to terminal in interactive mode: `-it` (for prompts, login, ...)

<br>

### Specific commands (examples):
* fun bash command on CentOS Linux: `docker run -it centos bash`
* run container from centos image for 20 sec: `docker run -d centos sleep 20`
* print content of the hosts-file: `docker exec <name_of_container> cat /etc/hosts`
* execute a command inside a running containter: `docker exec <container_id> <command>`, e.g. `docker exec 377561ba1e39 cat /etc/*release*`
* port mapping: `-p`
* volume mapping: `-v`

<br>

## Port mapping
Map port inside docker container to a free port on docker host.  
Map port 80 on docker (local) host to port 5000 on the docker container: `docker run -p 80:5000 kodekloud/webapp`  
This way it is possible to run multiple instances of an application and map them to different ports on the docker host: `docker run -p 8000:5000 kodekloud/webapp`  
Or run instances of different applications on different ports: `docker run -p 3306:3306 mysql`  
You cannot map to the same port on the docker host.

## Volume mapping
If data should persist when container is removed, map directory outside container on docker host to a directory inside the container: `docker run -v <dir_host>:<dir_container> <image>`  
e.g.: `docker run -v /opt/datadir:/var/lib/mysql mysql`