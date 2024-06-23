#books 

* Application orchestrator (deploys and manages applications)

### Intro

Kubernetes can deploy the application, scale it up and down dynamically, self-heal it when things go wrong, perform zero-downtime rolling updates and rollbacks

Orchestrator wars -> Docker Swarm, Mesosphere DCOS, and Kubernetes -> Kubernetes won
Docker Swarm is still under active development and is popular with small companies that need a simple alternative to Kubernetes.

Kubernetes (k8s) -> Helmsman (Greek) - person steers the ship

Kubernetes is like a courier service. You package the app as a container, give it a Kubernetes manifest, and let Kubernetes take care of deploying it and keeping it running.


#### Kubernetes as a cluster

* Cluster -a bunch of machines(nodes) to host applications.
* A cluster consists of a control plane and worker node.
* Control plane -> Intelligence, exposes the API, has a scheduler for assigning work, and responsible for keeping apps healthy (self-healing, auto-scaling, rollouts).
* Workers -> hard work of executing user applications


#### Kubernetes as an orchestrator

- Orchestrator -> a system that takes care of deploying and managing applications
* Just like a football coach
##### how it works?
1. Design and write applications as small independent microservices
2. Package each microservice as its own container
3. Wrap container in Kubernetes Pod
4. Deploy pods to the cluster


### Control Plane

1. Control Plane runs a collection of system services (masters, heads, head nodes) that make up the control plane of the cluster.
2. Multiple control plane nodes can configured for high availability
#### API server

* Internal and external user components communicate through API server - all roads lead to the API server
* Exposes RESTful API that you POST YAML configuration files (manifests) over HTTPS

#### The Cluster store

* 
#### Controller manager and Controllers

#### Scheduler


#### Cloud control manager
