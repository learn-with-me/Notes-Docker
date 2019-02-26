# Docker

Docker Inc. is the main company behind docker technology. To really understand Docker, you should start with Containers. Containers are like light-weight, fast, virtual machines. Dockers makes running our app inside containers really easy. Apps running in containers are often referred as containerized apps.

### Containers

##### History

No one really knows what kind of infrastructure an application needs. At the same time, need of hardware of an application changes with time based on the load \(cpu/memory\) that an application requires to function well in a real world. This ended up having organizations buy huge servers to support their needs, which eventually led to wastage of system resources or having to frequently upgrade hardware. In some cases, there is not enough hardware bought to support application's needs. This is a lot of cost, in both hardware and maintenance to the organizations.

##### VMware \(Virtualization Era\)

Instead of running one application on a server, VMware, with HyperVisor technology, made it possible to run tons of applications on a single server via Virtual Machines. Virtual Machine \(VM\) is a slice of a physical hardware. Each VM needs a dedicated operating system.

Along with the upsides, the downside of this model was, that these OS on each VMs consumed a ton of system resources just to get up and running without even running the application to start with. This leaves a small share of system resources for the application to run on. Along with this came the cost of OS licenses for each of those VMs. The model, without a doubt, is way better than historical implementations, however still had some pain-points in terms of efficient usage of system resources, cost of operation, administration, updates, anti-virus, etc.

##### Solution \(Virtualization 2.0\)

Containers came to rescue. They are a slice of the operating system, as compared to slice of the hardware. We run one app per container. Spinning up containers are way faster than spinning up a VM. A container is made up of an image that can be executed using docker. Containers have paved the way for micro-services. They do not entirely replace VMs, in a lot of cases they live side-by-side \(especially in cases where organizations are not completely on cloud yet\).

##### Suitable workloads

* Stateless applications
  * Doesn't remember stuff. Just like the app is running on day one.
* Stateful applications
  * Has to remember stuff

### Docker \(Enterprise Edition Advantages\)

* Official Support
* Stable
* Steady releases
* Enterprise Web UI
* Security features
* AD/LDAP integration to manager users for the organization
* Private registry - run on-prem or on cloud
* Policies
* FIPS
* Pipelines...

### Orchestration

Think about an orchestra. We do not want to call the shots manually. All we should focus on is having the game plan. The orchestrator should be able to execute the game plan for this. This is what Orchestration is all about.

Kubernetes is a powerful application orchestrator, however a smaller and simpler product is `Docker Swarm`. At its core, Docker Swarm does the same thing to play and manage micro-services apps, its just got a lot less features and a lot less momentum when compared to Kubernetes.

### Great place to start

* CI/CD projects
* Hand-picked projects
* Messaging and Education is important
* Infra services
  * Resilient infra
  * Logging
  * Monitoring
  * Orchestration



