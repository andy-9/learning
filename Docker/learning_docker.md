# Docker

## Why?
* to prevent matrix from hell (= compatibility/dependency issues, long setup time, different dev/test/prod environments)  
  --> containerize apps and run each service with its own dependencies in separate containers

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
* --> are isolated groups of processes running on a single host, which fulfill a set of “common” features
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

## Repository
* a collection of images

<br>

## Docker Editions
* Community Edition (free products) & Enterprise Edition (Enterprise Add-ons)

<br>

## Docker commands

### Setup
* Docker version: `sudo docker version`
* Show disc consumption (without duplicates) of images, containers and local volumes: `docker system df`
* Show disc consumption (with duplicates) of images, containers and local volumes: `docker sysem df -v`
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
* list of running containers: `docker ps` (ps = processes)

* list of running *and* exited containers: `docker ps -a`
* keep container running: add `tail -f /dev/null` at the end
* stop container `docker stop <name_of_container>` or `docker stop <name_of_id`
* remove container: `docker rm <name_of_container>`
* more details about a container: `docker inspect <name_of_container>`
* container logs: `docker logs <name_of_container>`

<br>

### Images
* list information about the Docker installed on particular system: `docker info`
* list images: `docker images`

* download image: `docker pull <name_of_image>`
* run image (attached mode): `docker run <name_of_image>` (pulls image from Docker Hub if not present locally, output can be seen)
* run container from image in detached/background mode: `docker run -d <name_of_image>`
* run container from image again attached: `docker attach <first_few_chars_of_container_id>`
* run specific version: `docker run <name_of_image>:<tag>` (no tag: default is `latest`)
* remove image: `docker rmi <name_of_image>` (stop & remove all dependent containers before)
* remove image with tag: `docker rmi <name_of_image:tag>`
* remove unused images: `docker image prune`
  options:  
  `--all`, `-a`: all  
  `--filter`: filter values, e.g. `--filter until=<timestamp>`  
  `--force`, `-f`: do not prompt for confirmation
* history: `docker history <name_of_image>`

<br>

### Flags
* restrict CPU use: `--cpus=<number_between_0_and_1>`
* detached `-d `
* environment variable `-e`
* modify entrypoint: `--entrypoint <command>`
* specify name of Dockerfile: `-f <name_of_Dockerfile>`
* force `-f` or `--force`
* remote: `-H=<remote-docker-engine>:<port>`
* interactive mode: `i` (docker runs by default in a non-interactive mode)
* attached to terminal in interactive mode: `-it` (for prompts, login, ...)
* restrict memory usage in MB:`--memory=<number>m`
* mount volume: `--mount` (add several key-value-pairs!)
* give container name: `--name <name>`
* network configurations: `--network`
* port mapping: `-p`
* restart: `--restart <no>(default)|<always>|<unless-stopped>|<on-failure>`
* tag: `-t`
* other user: `-u <user_name>`
* volume mapping: `-v` (better use `--mount`!)

<br>

### Specific commands (examples):
* fun bash command on CentOS Linux: `docker run -it centos bash`
* run container from centos image for 20 sec: `docker run -d centos sleep 20`
* Create whale image: `docker run docker/whalesay cowsay 'Muh, muh!'` for https://hub.docker.com/r/docker/whalesay
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
* `-e <NAME_OF_VARIABLE>=<value>`  
  e.g. `docker run -e APP_COLOR=blue simple-webapp-color`

* Set password for db: `docker run -d --name <name_of_container> -e <NAME_OF_VARIABLE>=<password> mysql`
* Inspect environment variables: `docker inspect <name_of_container>`  
   ("Config": {  
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

<br>

## Docker Registry
* central repository (cloud) of all docker images
* DockerHub is a hosted registry solution by Docker Inc.  
  Besides public and private repositories, it also provides: automated builds and integration with source control solutions like Github and Bitbucket etc.

* image: `docker.io/nginx/nginx`  
  `docker.io`: registry (Docker Hub)  
  `nginx`: user/account  
  `nginx`: image/repo
* Other registries, e.g. `gcr.io` (google)
* private registry:
  ```
  docker login private-registry.io
  docker run private-registry.io/apps/internal-app
  ```
* Deploy private registry:  
  (`registry:2` is the name of the image)
  ```
  docker run -d -p 5000:5000 --name <registry> <registry:2>
  docker image tag <my-image> localhost:5000/<my-image>
  docker push localhost:5000/<my-image>
  ```
* Pull private image:
  ```
  docker pull localhost:5000/<my-image>
  docker pull 192.168.56.100:5000/<my-image>
  ```

<br>

## Docker engine
* docker engine: host with docker installed on it

* 3 different components: Docker Daemon, REST API, Docker CLI
  - Docker Daemon: background processes that manages Docker objects (e.g. images, containers, volumes, networks)
  - REST API Server: API interface that programs can use to talk to the daemon and provide instructions
  - Docker CLI: CLI to perform actions (containers, images, ...)  
    Can be on another system (e.g. laptop): `docker -H=<remote-docker-engine>:<port>`, e.g. run nginx container on remote docker host: `docker -H=10.123.2.1:2375 run nginx`

<br>

### Containerization
* Use namespaces to isolate workspace: Process ID, Mount, Network, InterProcess, ...

* Namespace PID: When Linux boots it starts with one process ID (PID): 1. This is the root process that kicks off every other process.
* PID is unique
* PIDs in container (PID: 1, PID: 2) are actually just another processes in the base Linux system (PID: 5, PID: 6). The container PIDs are just visible within the container.
* The underlying Docker host as well as the containers share the same system resources (e.g. CPU, memory)
* There is no restriction as to how much CPU or memory a container can use.  
  But it is possible to restrict with `cgroups` (control groups): `docker run --cpus=.5 ubuntu` (max. of 50 % CPU) or `docker run --memory=100m ubuntu` (max. of 100 MB memory)

<br>

## Docker storage
* Docker stores data on local filesystem at `/var/lib/docker`  
  Subdirectories:  
  - `diff/`: contents of each layer, each stored in a separate subdir  
  - `layers/`: metadata about how image layers are stacked  
  - `mnt/`:  mount points, one per image or container layer, which are used to assemble and mount the unified filesystem for a container

* Layered architecture:
  - image layers are read only
  - container layer is read and write (e.g. temp-files, logs or files modified by user) --> will be destroyed when container is destroyed
  - copy-on-write: files from image layers can be modified within the container - but they are only a copy of the original file in the image
* How to persist this data? --> create a volume under: `docker volume create data_volume`  
   path will be:  `/var/lib/docker/volumes/data_volume`
* Run container and mount volume: `docker run -v <volume_name>:<location_inside_container> <image>`,  
  e.g. `docker run -v data_volume:/var/lib/mysql mysql`  
  The data will persist even if container is destroyed.  
  This is called **volume mounting** (mount volume from the `volumes`-directory).  
  It is enough to run only the last command (and not the `create`-command) - volume gets created automatically if it is not there yet.  
* Store data on another path than the default one: specify path to directory  
  e.g. `docker run -v /data/mysql:/var/lib/mysql mysql`   
  This is called **bind mounting** (mount directory from any location on the Docker host).  
* New and better way to write is with `--mount`  
  e.g.: `docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql`  
  `type` of the mount (`bind`, `volume` or `tmpfs`)  
  `source` is the location on my host  
  `target` is the location on my container  
  optional `readonly`: mount into container as read-only  
  optional `bind-propagation`: changes the bind propagation (`rprivate`, `private`, `rshared`, `shared`, `rslave`, `slave`)
* Who is responsible for all these operations? The **storage drivers**  
  e.g. AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2

<br>

## Docker networking
* 3 networks:  
  - Bridge: default network a container gets attached to, private internal network on the host, port usually `172.17.x.x`
  - none: containers not attached to any network, no acces to external network or other containers (isolated network)
  - host: associate the container to the host network (this takes out the network isolation, no port mapping necessary, not possible to run multiple web containers on the same host on the same port)

* attach container to another network with `--network` parameter  
  e.g. `docker run Ubuntu --network=none`
* By default Docker creates only one internal Bridge network  
  Create own internal network with the name `custom-isolated-network`:
  ```
  docker network create \
      --driver bridge \
      --subnet 182.18.0.0/16
      custom-isolated-network
  ```
* gateway: e.g. `--gateway 182.18.0.1`
* list networks: `docker network ls`
* show networks settings: `docker inspect <container_name>`
* inspect network: `docker network inspect <name_of_network>`
* embedded DNS - containers can reach each other using their names  
  e.g. webserver wants to access database: `mysql.connect(mysql)`  
  built-in DNS server always runs at `127.0.0.11`
* Specific example: Deploy a web application named `webapp` using the `kodekloud/simple-webapp-mysql` image. Expose the port to 38080 on the host. The application makes use of two environment variable:
  - 1: `DB_Host` with the value `mysql-db`
  - 2: `DB_Password` with the value `db_pass123`
Make sure to attach it to the newly created network called `wp-mysql-network`.  
Also make sure to link the `MySQL` and the `webapp` container.  
`docker run --network=wp-mysql-network -e DB_Host=mysql-db -e DB_Password=db_pass123 -p 38080:8080 --name webapp --link mysql-db:mysql-db -d kodekloud/simple-webapp-mysql`

<br>

## Docker on Windows
Linux container on Windows host:
* use either: Docker toolbox (uses VirtualBox)
* or: Docker Desktop for Windows (uses MS Hyper-V)
* only possible to use either one!
* Docker Desktop for Windows also supports Windows containers  
  Container types:
  - Windows Server
  - Hyper-V isolation (each container is run within a highly optimized virtual machine guaranteeing complete kernel isolation between containers and host)
  
<br>

## Docker on Mac
Linux container on Mac host:
* use either: Docker toolbox (uses VirtualBox)
* or: Docker Desktop for Mac (uses HyperKit)

<br>

## Container Orchestration - Docker Swarm & Kubernetes
* Container orchestration: set of tools and scripts that can help host containers in a production environment
* Typically multiple docker hosts that can host containers
* Even if one fails the app is still accessible
* Some orchestration solutions help to automatically scale up or down (containers and hosts), also networking between containers across different hosts, load balancing, user request, sharing storage, support for config management and security
* Orchestration solutions:
  - Docker Swarm
  - kubernetes (google)
  - MESOS (Apache)

<br>

### Docker Swarm
* Combine multiple Docker machines together into a single cluster - Docker swarm takes care of distributing the services or app instances into separate hosts for high availability and load balancing and across different systems and hardware
* Necessary to have multiple hosts with Docker on them  
  Then designate one host to be the manager (Swarm manager):  
  `docker swarm init --advertise-addr 192.168.1.12`
  Then join worker nodes:  
  `docker swarm join --token <token>`
* Docker services are one or more instances of a single application or service that runs across the nodes in the swarm cluster
  ```
  docker run my-web-server
  docker service create --replicas=3 -p 8080:80 --network frontend my-web-server
  ```

<br>

### Docker Kubernetes
* Kubernetes uses Docker host to host apps in the form of Docker containers
* A cluster is a set of nodes grouped together
* Master/Manager is a node with a Kubernetes control plain component. Watches over the nodes in the cluster and is responsible for the actual orchestration of containers on the worker nodes.
* Installing Kubernetes installs the following components:
  - API Server (acts as frontend)
  - etcd Sever (key-value-store)
  - kubelet service (agent that uns on each node in the cluster, responsible for making sure the containers run as expected)
  - Container Runtime (underlying software to run containers, e.g. Docker engine)
  - Controller (brain behind the orchestration)
  - Scheduler (distributing work or containers across multiple nodes)
* Kubernetes CLI: `kubectl` (kubecontrol)  
  Run 1000 nodes:
  ```
  docker run my-web-server
  kubectl run --replicas=1000 my-web-server
  ```
  Scale up to 2000: `kubectl scale --replicas=2000 my-web-server`  
  Rolling upgrade: `kubectl rolling-update my-web-server --image=web-server:2`  
  Roll back images if something goes wrong: `kubectl rolling-update my-web-server --rollback`  
* Deploy app on cluster: `kubectl run hello-minikube`  
  View information about the cluster: `kubectl cluster-info`  
  List all the nodes part of the cluster: `kubectl get nodes`  
  Run 1000 instances of app across 1000 nodes: `kubectl run my-web-app --image=my-web-app --replicas=1000`
