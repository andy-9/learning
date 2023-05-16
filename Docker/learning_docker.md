# Docker

## Setup
* Hardware Infrastructure > OS > Docker > Container

## Containers
* are running instances of images
* are completely isolated environments
* share the same OS kernel
* application + libraries + dependencies
* can easily provision applications and scale them
* run several instances of the same app for load balancing. If one fails, destroy and launch new one.

## VMs
* have additionally the OS in them and instead of Docker a Hyervisor
* need more size, higher utilization, booting is slower
* though complete isolation from kernel
* can run different applications built on different OS
* can easily provision or decommission docker hosts

## Image
* is a package or template
* used to create a container