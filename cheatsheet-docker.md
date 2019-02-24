# Cheatsheet: Docker

Docker is broken into two parts: Client & Server

* **Client -** Code that runs natively on the operating system to support docker command interactions. MacOS will have OS/Arch as _darwin/amd64_.
* **Server -** runs inside Mobi Linux VM with OS/Arch as _linux/amd64_. Server makes docker tool as docker-on-linux under the hood.. Hence it is only going to work on docker-based linux containers

#### Installation & Help

```
MacOS - install from docker.com
Windows - install from docker.com
Linux/Ubuntu - $ wget -q0- https://get.docker.com/ | sh

$ docker version            # Display docker client and server information
$ docker info               # 
$ docker --help             # 
```

#### Build Image

```
$ docker image build -t <image_name> .            # Build docker image from the current directory
$ docker image build -t goel4ever/webhooks:2 .    # Example
```

#### View Image

```
As a standard rule, try keeping your images below 300Mb in size

$ docker images                                   # View all the docker images
$ docker image ls                                 # View all the docker images (same as above)
$ docker image ls <image_name>                    # View details for an image
```

#### Push image to a registry

```
Registry can be on-prem or on cloud, from Docker Hub, Google Container Registry, etc . . .

$ docker image push <image_name>                  # Push the image to a registry
```

#### View Containers

```
$ docker ps
```

#### Running/Stopping a container

```
$ docker container run -d --name web -p 8000:8080 <image_name>    # Runs a container
$ docker stop web                                                 # Stop a container
$ docker start web                                                # Start a container
```



