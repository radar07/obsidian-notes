
Define a way of deploying, scaling, monitoring and replacing of containers independent of the cloud service

![[Pasted image 20240620225436.png]]

![[Pasted image 20240621074748.png]]

## Pod
* Pods can have one or more containers. (typically 1)
* Pods are ephemeral. (they are no persistent)
* Contain shared resources (volumes) and communicate with other pods.
* Has an internal cluster IP by default.
* To manage pods, we need Controllers.

## Deployment
* Controls 1 or more pods.
* We set a desired state (defining pods) and the controller checks the current state and do operations to acquire the desired state.
* Can be paused, deleted and rolled back.
* Deployments can be scaled dynamically and automatically.
