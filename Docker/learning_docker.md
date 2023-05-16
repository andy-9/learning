# Docker

## Structure
* Hardware Infrastructure > OS > Docker > Container

## Containers
* are running instances of images
* meant to host a specific task or process
* are completely isolated environments
* share the same OS kernel (not meant to host an OS)
* application + libraries + dependencies
* can easily provision applications and scale them
* run several instances of the same app for load balancing. If one fails, destroy and launch new one.
* only lives as long as the process inside it is alive

## VMs
* have additionally the OS in them and instead of Docker a Hyervisor
* need more size, higher utilization, booting is slower
* though complete isolation from kernel
* can run different applications built on different OS
* can easily provision or decommission docker hosts

## Images
* is a package or template
* used to create a container
* handed over from dev to ops (is guaranteed to run)

## Docker Editions
* Community Edition (free products) & Enterprise Edition (Enterprise Add-ons)

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

### Containers
* list running containers: `docker ps`

* list running *and* exited containers: `docker ps -a`
* stop container `docker stop <name_of_container>` or `docker stop <name_of_id`
* remove container: `docker rm <name_of_container>`

### Images
* list images: `docker images`

* download image: `docker pull <name_of_image>`
* remove image: `docker rmi <name_of_image>`
* run nginx instance: `docker run nginx` (pulls image from Docker Hub if not present locally)
* run image attached: `docker run <name_of_image>` (see output)
* run image in detached/background mode: `docker run -d <name_of_image>`
* run image again attached: `docker attach <first_few_chars_of_container_id>`

### Other commands
* execute a command, e.g. print content of the hosts-file: `docker exec <name_of_container> cat /etc/hosts`