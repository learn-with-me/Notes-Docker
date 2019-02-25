# Docker Swarm

Its' all about true native clustering. In docker world, swarm is a cluster.

Swarm consists of Manager nodes and Worker nodes.

* Manager nodes
  * Highly available \(3-5 recommended - odd number\)
  * Only one leader
  * If any manager node receives the command, it proxies it to the leader
  * Raft \(Raft Consensus Algorithm\) is the protocol used behind the scenes to bring order to the chaos, ensuring we achieve a distributed consensus.
  * Remember: managers are also workers
* Worker nodes
  * Accepts tasks from managers and executes them
* Services
  * Declarative way of running and scaling apps
  * New concept introduced in swarm mode
* Task
  * Atomic unit of work assigned to worker nodes
  * In some way, tasks mean containers

#### Terms

Docker Inc are calling native container-aware load balancer as the "routing mesh"

### Cheatsheet

##### Initialize

```
$ docker swarm init        # initiate a swarm

$ docker swarm init --advertise-addr <valid_ip>:2377 --listen-addr <valid_ip>:2377
$ docker swarm init --advertise-addr 172.31.12.161:2377 --listen-addr 172.31.12.161:2377
    # --advertise-addr - tells docker to use the current machine for swarm related stuff regardless of IP
        If the machine has a single IP, this option is not required
    # --listen-addr - this is what the node listens on, when swarm manages the traffic
```

##### Join Swarm

```
$ docker swarm join-token manager    # Join a manager
$ docker swarm join-token worker     # Join a worker

When adding a manager or worker, remember to add --advertise-addr and --listen-addr flags

$ docker node promote ><node_id>     # promotes the node to a manager. Only executed on a manager.
```

##### Manager

```
$ docker node ls        # Can only be run in a manager. Lists all the nodes
```

##### Services

```
Create service
$ docker service create --name svc-deploy -p 8000:8080 --replicas 5 <image_name>
$ docker service create --name svc-deploy -p 8000:8080 --replicas 5 hello-world
    # --replicas - creates the number of tasks/containers

List service
$ docker service ls
$ docker service ps <service_name>
$ docker service ps svcs-deploy

Drilling in config
$ docker service inspect --pretty <service_name>

Advanced configs (Rolling updates)
$ docker create service --update-parallelism 2 --update-delay 10s <service_name>
$ docker service update --image <image_name>:<version>
    --update-parallelism 2
    --update-delay 10s <service_name>
```

##### Scaling

```
$ docker service update --replicas
$ docker service scale <name>=<number>        # same as the command above
$ docker service scale svcs-deploy=7
```

##### Networking \(for swarm only\)

```
Network can be of type overlay, bridge, etc

Create an overlay network
$ docker network create -d overlay <network_name>
$ docker network create -d overlay svcs-net

List network
$ docker network ls


```



