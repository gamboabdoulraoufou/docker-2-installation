## docker-2-installation

In this tutorial, you'll learn how to install Docker and use it on an existing installation of CentOS 7

> Prerequisites
- 64-bit CentOS 7 Droplet
- Non-root user with sudo privileges

> Installing Docker
```sh
# update the package database
sudo yum check-update

# add the official Docker repository, download the latest version of Docker, and install it
curl -fsSL https://get.docker.com/ | sh

# after installation has completed, start the Docker daemon
sudo systemctl start docker

# make sure it starts at every server reboot
sudo systemctl enable docker

# verify that it's running
sudo systemctl status docker
``` 

The output of the last commad should be similar to the following, showing that the service is active and running


Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client.

> Executing Docker Command Without Sudo

```sh  
# add your username to the docker group
sudo usermod -aG docker username
```

> Using the Docker Command

```sh
# view all available subcommands
docker

# view the switches available to a specific command
docker docker-subcommand --help

# view system-wide information
docker info
```

> Working with Docker Images

Docker containers are run from Docker images. By default, it pulls these images from Docker Hub, a Docker registry managed by Docker, the company behind the Docker project. Anybody can build and host their Docker images on Docker Hub, so most applications and Linux distributions you'll need to run Docker containers have images that are hosted on Docker Hub

```sh
# check whether you can access and download images from Docker Hub
docker run hello-world

# earch for images available on Docker Hub. For example, to search for the CentOS image
docker search centos

# once you've identifed the image that you would like to use, you can download it to your computer using 
docker pull centos

# after an image has been downloaded, you may then run a container using the downloaded image with the run subcommand. 
# If an image has not been downloaded when docker is executed with the run subcommand, the Docker client will first download the image, then run a container
docker run centos

#  see the images that have been downloaded to your computer
docker images

> Running a Docker Container

```sh
# run a container using the latest image of CentOS. The combination of the -i and -t switches gives you interactive shell access into the container
docker run -it centos

# install MariaDB server in the running container
yum install mariadb-server

```

> Committing Changes in a Container to a Docker Image

When you start up a Docker image, you can create, modify, and delete files just like you can with a virtual machine. The changes that you make will only apply to that container. You can start and stop it, but once you destroy it with the docker rm command, the changes will be lost for good

```sh
# after installing MariaDB server inside the CentOS container, you now have a container running off an image, but the container is different from the image you used to create it.
# to save the state of the container as a new image, first exit from it:
exit

# Then commit the changes to a new Docker image instance. 
# The -m switch is for the commit message, while -a is used to specify the author. 
# The container ID is the one you noted earlier in the tutorial when you started the interactive docker session. 
# Unless you created additional repositories on Docker Hub, the repository is usually your Docker Hub username:
docker commit -m "What did you do to the image" -a "Author Name" container-id repository/new_image_name

# list images
docker images

```

> Listing Docker Containers

```sh
# view active containers
docker ps

# view all containers
docker ps -a

# stopping a running or active container
docker stop container-id
``` 

> Pushing Docker Images to a Docker Repository

To push an image to Docker Hub or any other Docker registry, you must have an account there.
This section shows you how to push a Docker image to Docker Hub.
To create an account on Docker Hub: https://hub.docker.com/

```sh
# log into Docker Hub
docker login -u docker-registry-username

# push your own image 
docker push docker-registry-username/docker-image-name

``` 


