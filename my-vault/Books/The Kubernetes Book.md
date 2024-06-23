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
[]()
- Orchestrator -> a system that takes care of deploying and managing applications
* Just like a football coach
##### how it works?
1. Design and write applications as small independent micro-services
2. Package each micro-service as its own container
3. Wrap container in Kubernetes Pod
4. Deploy pods to the cluster


### Control Plane

1. Control Plane runs a collection of system services (masters, heads, head nodes) that make up the control plane of the cluster.
2. Multiple control plane nodes can configured for high availability
#### API server

* Internal and external user components communicate through API server - all roads lead to the API server
* Exposes RESTful API that you **POST** YAML configuration files (*manifests*) over HTTPS

#### The Cluster store

* **No cluster store, no cluster**
* Stores the entire configuration and state of the cluster
* Based on *etcd*, a popular distributed database
* As it is the single source of truth for a cluster, you should run between 3-5 etcd replicas for high-availability
* A default installation of Kubernetes installs a replica of the cluster store on every control plane node and automatically configures HA

#### Controller manager and Controllers

* The controller manager (*controller of controllers*) implements all background controllers that monitor cluster components and respond to events
* The goal is to ensure the *observed* state of the cluster matches the *desired* state.
	* Obtain desired state
	* Observe current state
	* Determine differences
	* Reconcile differences


#### Scheduler

* It watches the API server for new work tasks and assigns them to appropriate healthy worker nodes
* It is responsible for picking the nodes to run tasks (by ranking), it isn't responsible for running them.
* A *task* is normally a Pod/container

#### Cloud control manager

* If you're running your cluster on a supported public cloud platform, such as AWS, Azure, GCP, or Linode, your control plane will be running a *cloud controller manager*.
* Facilitates integrations with cloud services, such as instances, load-balancers, and storage.
![[Pasted image 20240623104213.png]]

### Worker nodes

1. Watch the API server for new work assignments
2. Execute work assignments
3. Report back to the control plane (via API server)
![[Pasted image 20240623104501.png]]


#### Kubelet

* Main Kubernetes agent runs on every worker node
* When you join a node to a cluster, the process installs the kubelet, which is responsible for registering it with the cluster.
* Its main job is to watch the API server for new work tasks.

#### Container runtime

* A kubelet needs container runtime to perform container-related tasks (pulling images, and starting and stopping containers)

#### Network proxy (kube-proxy)

* This runs on every node and is responsible for local cluster networking.
* Each node gets its own unique IP address, and it implements local iptables or IPVS rules to handle routing and load-balancing of traffic.

### Pods and Containers

* Pod -> pod of whales -> group of whales -> Docker logo
* Simplest model is run a single container in every Pod. But we can run multiple containers in a single Pod.
* Pods themselves don't actually run applications - applications always run in containers.
* Pod is just an execution environment to run one or more containers.
* If there are multiple containers running in a Pod, they all share the same Pod environment including the network stack, volumes, IPC namespace, shared memory, and more. Ex: all containers in the same Pod will share the same IP address.

#### Pod as the unit of scaling

* Pod - minimum unit of scheduling in Kubernetes.
* If you need to scale an app, you add or remove Pods. You *do not* scale by adding more containers to existing Pods

#### Pod - atomic operations

* Deployment of a Pod is an atomic operation. This means a Pod is only ready for service when all its containers are up and running.

#### Pod lifecycle

* Pods are mortal - they're created, they live and they die. When they die, Kubernetes starts a new one in its place with new ID and new IP address.

#### Pod immutability

* Pods are immutable. They don't change once they're running.
* Once a Pod is running, you never log on to it and change or update its configuration.
* If you need to change or update it, you replace it with a new one running with new configuration. Updating Pods -> delete the old one and replace it with a new one.


### Deployments

* Mostly you'll deploy Pods indirectly via higher-level controllers like *Deployments*, *DaemonSets*, and *StatefulSets*.
* Deployment is higher-lever Kubernetes object that wraps around a Pod and adds features such as self-healing, scaling, zero-downtime rollouts, and versioned rollbacks.
* Deployments, DaemonSets, and StatefulSets are implemented as controllers that run as watch loops constantly observing the cluster making sure observed state matches desired state.

### Service objects and stable networking

* Pods are unreliable so Services come in to play.
* They provide a reliable networking for a set of Pods.
* Services provide stable IP addresses and DNS names to the unstable world of Pods.
![[Pasted image 20240623134923.png]]`