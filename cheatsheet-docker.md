# Cheatsheet: Docker

Docker is broken into two parts: Client & Server \(together they are called as docker engine\). Based on the commands fired on the console using docker client, the client makes API calls to daemon. Daemon implements the Docker Remote API, which calls appropriate APIs based on the service requested.

* **Client -** Code that runs natively on the operating system to support docker command interactions. MacOS will have OS/Arch as _darwin/amd64_.
* **Server/Daemon -** runs inside Mobi Linux VM with OS/Arch as _linux/amd64_. Server makes docker tool as docker-on-linux under the hood.. Hence it is only going to work on docker-based linux containers

#### Installation/Help/Authentication

```
MacOS - install from docker.com
Windows - install from docker.com
Linux/Ubuntu - $ wget -q0- https://get.docker.com/ | sh

$ docker version            # Display docker client and server information
$ docker info               # Display all containers, images, storage, plugins, etc
$ docker --help             #

$ docker login              # Docker Hub credentials

Inspection
$ docker inspect <container_name>    # Even displays information like persistent volume location on host
```

### Images

##### Build Image

```
$ docker image build -t <image_name> .            # Build docker image from the current directory
$ docker image build -t goel4ever/webhooks:2 .    # Example
```

##### View Image

```
As a standard rule, try keeping your images below 300Mb in size

$ docker images                                   # View all the docker images
$ docker image ls                                 # View all the docker images (same as above)
$ docker image ls <image_name>                    # View details for an image
```

##### Remove Images

```
$ docker rmi <image_name>
$ docker rmi <image_id>
$ docker rmi alpine

$ docker rmi <image_name>:<version>
$ docker rmi ubuntu:14.04

$ docker rmi $(docker images -q)            # Remove all images
```

##### Tagging images

```
$ docker tag <image_name>
```

##### Push image to a registry/Pull images locally

```
Registry can be on-prem or on cloud, from Docker Hub, Google Container Registry, etc . . .
Docker Hub is the default registry for pulling/pushing images. Can be configured to whatever we want.

$ docker image push <image_name>                  # Push the image to a registry
$ docker push <image_name>                        # Push the image to a registry

$ docker pull <image_name>
$ docker pull <image_name>:<version>
$ docker pull alpine
$ docker pull ubuntu:14.04
```

### Containers

##### View Containers

```
$ docker ps                # List all active containers
$ docker container ls

$ docker ps -a             # List all running as well as stopped containers
$ docker container ls -a
```

##### Running a container

```
$ docker run <image_name>                    # Run a new container
$ docker run hello-world                     # If image not found, downloads from default container registry

$ docker run -it --name temp ubuntu:latest /bin/bash
    # -it refers to interactive. Opening in standard in-stream and assign it a terminal
    # To exit the container, without killing it, press Ctrl + P + Q

# Runs a container
$ docker container run -d --name web -p 8000:8080 -v $(pwd):/var/www/ -w "/var/www/" <image_name> <command>
$ docker container run -d --name web -p 8000:8080 -v $(pwd):/var/www/ -w "/var/www/" node npm start
    # -d refers to running the container in a detached mode in background
    # web is the name of the container
    # -p refers to the port exposed. Here 8080 is the container port, 8000 is the docker host port
    # -v refers to the volume (persistent storage)
        pwd refers to present working directory for host location
        /var/www refers to working directory for the container
    # -w represents the working directory
```

##### Starting/Stopping a container

```
$ docker start <container_name>                   # Start a container
$ docker start <container_id>
$ docker start web

# Stopping a container does not remove all the data it has stored. Just simply stops it
$ docker stop <container_name>                    # Stop a container
$ docker stop <container_id>
$ docker stop web
$ docker stop $(docker ps -aq)                    # Stop all the containers without dealing with IDs
```

##### Removing a container

```
$ docker rm <container_id>                  # Remove container by ID
$ docker rm <container_name>                # Remove container by ID
$ docker rm eloquent_murdock
$ docker rm -v <container_name>             # Remove container by ID, along with cleaning the volume

$ docker rm $(docker ps -aq)                # Remove all containers
```



