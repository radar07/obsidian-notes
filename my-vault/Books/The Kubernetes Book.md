#books 

* Application orchestrator (deploys and manages applications)

### Intro

Kubernetes can deploy the application, scale it up and down dynamically, self-heal it when things go wrong, perform zero-downtime rolling updates and rollbacks

Orchestrator wars -> Docker Swarm, Mesosphere DCOS, and Kubernetes -> Kubernetes won
Docker Swarm is still under active development and is popular with small companies that need a simple alternative to Kubernetes.

Kubernetes (k8s) -> Helmsman (Greek) - person steers the ship

Kubernetes is like a courier service. You package the app as a container, give it a Kubernetes manifest, and let Kubernetes take care of deploying it and keeping it running.