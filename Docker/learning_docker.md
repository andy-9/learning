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
* meant to run a specific task or process (host instance of web server or db, carry out a computation, ...)
* are completely isolated environments
* share the same OS kernel (not meant to host an OS)
* application + libraries + dependencies
* can easily provision applications and scale them
* run several instances of the same app for load balancing. If one fails, destroy and launch new one.
* only live as long as the process inside them are alive - exit after task is done or container crashes

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

Login: `docker login`

<br>

### Containers
* list of running containers: `docker ps`

* list of running *and* exited containers: `docker ps -a`
* keep container running: add `tail -f /dev/null` at the end
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
* history: `docker history <name_of_image>`

<br>

### Flags
* detached `-d `
* environment variable `-e`
* modify entrypoint: `--entrypoint <command>`
* force `-f` or `--force`
* interactive mode: `i` (docker runs by default in a non-interactive mode)
* attached to terminal in interactive mode: `-it` (for prompts, login, ...)
* give container name: `--name <name>`
* port mapping: `-p`
* tag: `-t`
* other user: `-u <user_name>`
* volume mapping: `-v`

<br>

### Specific commands (examples):
* fun bash command on CentOS Linux: `docker run -it centos bash`
* run container from centos image for 20 sec: `docker run -d centos sleep 20`
* print content of the hosts-file: `docker exec <name_of_container> cat /etc/hosts`
* execute a command inside a running container: `docker exec <container_id> <command>`, e.g. `docker exec 377561ba1e39 cat /etc/*release*`
* Run container named `blue-app` using image `kodekloud/simple-webapp`, set environment variable `APP_COLOR` to `blue`, host port ist `38282`, the app listens on port `8080`: `docker run APP_COLOR=blue -p 38282:8080 --name blue-app kodecloud/simple-webapp`

<br>

## Port mapping
* Find IP-address and port: `docker inspect <id_of_container>`  
* Map port inside docker container to a free port on docker host: `docker run -p <host_port>:<container_port> <name_of_mage>`.  
e.g. map port 80 on docker (local) host to port 5000 on the docker container: `docker run -p 80:5000 kodekloud/webapp`  
* This way it is possible to run multiple instances of an application and map them to different ports on the docker host: `docker run -p 8000:5000 kodekloud/webapp`  
* Or run instances of different applications on different ports: `docker run -p 3306:3306 mysql`  
* You cannot map to the same port on the docker host.  

<br>

## Volume mapping
If data should persist when container is removed, map directory outside container on docker host to a directory inside the container: `docker run -v <dir_host>:<dir_container> <image>`  
e.g.: `docker run -v /opt/datadir:/var/lib/mysql mysql`

<br>

## Create image
1. Create dockerfile

2. Write down instructions:  
    a. OS Ubuntu  
    b. Update apt repo  
    c. Install dependencies  
    d. Install Python dependencies using pip  
    e. Copy source code to /opt folder  
    f. Run the web server using "flask" command

    Structure: INSTRUCTION argument
    ```
    FROM Ubuntu
    RUN apt-get update && apt-get -y install python
    RUN pip install flask-mysql
    COPY . /opt/source-code
    ENTRYPOINT FLASK_APP=/opt/sourc-code/app.py flask run
    ```
    Layer 1: Base Ubuntu Layer  
    Layer 2: Changes in apt packages  
    Layer 3: Changes in pip packages  
    Layer 4: Source code  
    Layer 5: Update Entrypoint with "flask" command

3. Build your image using the docker build command:  
   `docker build . -f Dockerfile -t mmumshad/my-custom-ap`

4. Make it available on public docker hub registry:  
   `docker push <docker_account_name>/<app_name>`  
   e.g. `docker push mmumshad/my-custom-app`

5. Styp by step in console:  
   ```
   mkdir <name_of_app>
   cd <name_of_app>
   ```

   ```
   cat > Dockerfile

   FROM ubuntu

   RUN apt-get update
   RUN apt-get install -y 2to3 python2-minimal python2 dh-python python-is-python3 python3-pip
   RUN pip3 install flask
   
   COPY app.py /opt/app.py

   ENTRYPOINT FLASK_APP=/opt/app.py python -m flask run --host=0.0.0.0
   ```
   hit `Ctrl+C`

   Build image with name 'test_image': `docker build . -t test_image`
   Run image: `docker run test_image`

   <br>

## Environment variables
`-e <NAME_OF_VARIABLE>=<value>`  
e.g. `docker run -e APP_COLOR=blue simple-webapp-color`

Inspect environment variables: `docker inspect <name_of_container>` ("Config": {
   "Env": ...
})

<br>

## CMD and ENTRYPOINT
Specify a different command to start the container:  
`docker run <image> <command>`  
e.g. `docker run ubuntu sleep 5`

```
FROM Ubuntu
CMD sleep 5
```

Or:
```
CMD ["sleep", "5"]
```

`docker run ubuntu-sleeper sleep 10`  
--> Command at startup: `sleep 10`

<br>

Invoke command automatically, just pass the number of seconds:  
```
FROM Ubuntu
ENTRYPOINT ["sleep"]
```

The ENTRYPOINT instruction is like the command instruction:  
`docker run ubuntu-sleeper 10`  
--> Command at startup: `sleep 10`

<br>

Configure default value:
```
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]
```
`docker run ubuntu-sleeper` runs in fact `docker run ubuntu-sleeper sleep 5`  
`docker run ubuntu-sleeper 10` runs `docker run ubuntu-sleeper sleep 10`

**Always specify the ENTRYPOINT and CMD instructions in a JSON-format!**

<br>

Modify/overwrite entrypoint: `docker run --entrypoint <command> <image> <command>`  
imaginary example: `docker run --entrypoint sleep2.0 unbuntu-sleeper 10`

<br>

## Docker compose

**`Docker compose` does not get installed automatically!**

Install:
```
DOCKER_CONFIG=${DOCKER_CONFIG:-$HOME/.docker}
mkdir -p $DOCKER_CONFIG/cli-plugins
curl -SL https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-linux-x86_64 -o $DOCKER_CONFIG/cli-plugins/docker-compose
chmod +x $DOCKER_CONFIG/cli-plugins/docker-compose
```

check version: `docker compose version`

Update:
```
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

Instead of
```
docker run mmumshad/simple-webapp
docker run mongodb
docker run redis:alpine
docker run ansible
```

`docker-compose.yml`:
```
services:
   web:
      image: "mmumshad/simple-webapp
   database:
      image: "mongodb"
   messaging:
      image: "redis:alpine"
   orchestration:
      image: "ansible"
```

Link containers together (**deprecated!**): `--link <name_of_container>:<name_of_host>`

<br>

### Specific example 1
```
docker run -d --name=redis redis
docker run -d --name=db postgres:9.4
docker run -d --name=vote -p 5000:80 --link redis:redis voting-app
docker run -d --name=result -p 5001:80 --link db:db result-app
docker run -d --name=worker --link db:db --link redis:redis worker
```

-->
`docker-compose.yml`
```
redis:
   image: redis
db:
   image: postgres:9.4
vote:
   build: ./vote
   ports:
      - 5000:80
   links:
      - redis
result:
   build: ./result
   ports:
      - 5001:80
   links:
      - db
worker:
   build: ./worker
   links:
      - db
      - redis
```

`build: <directory>`  
`directory` contains application code and a Dockerfile with instructions to build the docker image.

**Bring up entire stack: `docker-compose up`**

<br>

### Versions
It's important to specify the version on top of the .yml-file.

Differences v1 to v2:
* `version: '2'` on top of file
* `links` not necessary any more (bridged network created for this application and attaches all containers to this network - they can communicate to each other)
* new `depends_on` property for prioritization/dependency

e.g.
   ```
   version: '2'
   services:
      redis:
         image: redis
         networks:
            - back-end
      db:
         image: postgres:9.4
         networks:
            - back-end
      vote:
         build: ./vote
         ports:
            - 5000:80
         networks:
            - front-end
            - back-end
      result:
         build: ./result
         ports:
            - 5001:80
         networks:
            - front-end
            - back-end
      worker:
         build: ./worker
         networks:
            - back-end

   networks:
      front-end: 
      back-end: 
   ```

`networks` to contain traffic, e.g. separate user-generated traffic from internal traffic.

<br>

Differences v2 to v3:
* `version: '3'` on top of file
* support for `docker swarm`

<br>

### Specific example 2
`cat > docker-compose.yml`

```
version: '3'
services:

  redis:
    image: redis

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres

  vote:
    image: voting-app
    ports: 
      - 5000:80

  worker:
    image: worker-app

  result:
    image: result-app
    ports: 
      - 5001:80
```
