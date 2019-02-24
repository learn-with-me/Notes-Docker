# Cheatsheet: Docker

#### Installation & Help

```
$ docker version
$ docker info
$ docker --help
```

#### Build Image

```
$ docker image build -t <image_name> .            # Build docker image from the current directory
$ docker image build -t goel4ever/webhooks:2 .    # Example
```

#### View Image

```
As a standard rule, try keeping your images below 300Mb in size

$ docker image ls                                 # View all the docker images
$ docker image ls <image_name>                    # View details for an image
```

#### Push image to a registry

```
Registry can be on-prem or on cloud, from Docker Hub, Google Container Registry, etc . . .

$ docker image push <image_name>                  # Push the image to a registry
```

#### Running/Stopping a container

```
$ docker container run -d --name web -p 8000:8080 <image_name>    # Runs a container
$ docker stop web                                                 # Stop a container
$ docker start web                                                # Start a container
```



