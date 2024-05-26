#books

## Docker technology
 * runtime
 * daemon (aka engine)
 * orchestrator
 ![[Pasted image 20240526170401.png]]

## Docker Engine

![[Pasted image 20240512195625.png]]
![[Pasted image 20240512195648.png]]
## Images
* Stopped containers
```bash
docker images --filter dangling=true

docker images --filter=reference="*:latest"

docker images --format "{{.Repository}}: {{.Tag}}: {{.Size}}"

# deleting images
docker rmi <id>

# deleting all images on Docker host
docker rmi $(docker images -q) -f
```


## Containers
* runtime instance of an image
* `docker run` with start containers

```bash
docker run -it ubuntu /bin/bash

docker exec -it <id> bash

docker stop <id> # stops the container

docker rm <id> # removes the container
```

`docker stop` sends a **SIGTERM** signal to main application
if it's still running sends a **SIGKILL** after 10s

#### Self-healing containers with restart policies
* always
* unless-stopped
* on-failure
```bash
# always restarts a failed container unless explicitly stopped
docker run -d --name always -it --restart always alpine sleep 1d

docker run -d --name unless-stopped --restart unless-stopped alpine sleep 1d

### stop both containers
docker stop always unless-stopped

### restart docker
systemctl restart docker

docker ps -a
```
![[Pasted image 20240512211400.png]]

#### Web server
```bash
# runs server a local machine that maps the container port 8080 to the host port 80
docker run -d --name webserver -p 80:8080 nigelpoulton/ddd-book:web0.1
```


## Containerizing an app

1. Start with your application and dependencies
2. Create a *Dockerfile* that describes the app, dependencies and how to run it.
3. Build an image by the passing the *Dockerfile* to the `docker build`
4. Push the new image to a public registry(DockerHub)
5. Run a container from the image
```bash
docker build -t test-image:t1.0 .

### Pushing the image
docker login

docker tag test-image:t1.0 radar07/test-image:t1.0

docker push radar07/test-image:t1.0
```
#### Commands

* `docker build` reads Dockerfile and containerize the application.
	* `-t` - tag of image
	* `-f` - name and location of Dockerfile
* `FROM` base image of the new image we are building
* `RUN` run commands inside the image
* `COPY` add files into the image as new layer
* `EXPOSE` network port an application uses
* `ENTRYPOINT` sets the default application to run when the image is started as container
* `LABEL, ENV, ONBUILD, HEALTHCHECK`


## Multi-container apps with Compose

* Compose ships with Docker engine by default (`docker compose`)
* `compose.yaml` or `compose.yml`


## Swarm
#### Swarm Primer
* Nodes are configures as *managers* or *workers*
* *Managers* look after the control plane (state of the worker and dispatch tasks to *workers*)
* *Workers* accept tasks from *managers* and execute them
![[Pasted image 20240515215601.png]]

```bash
docker swarm init

docker swarm join-token manager

docker swarm join-token worker

docker node ls # lists all the managers and workers in the swarm
```

* Only one active manager (leader).
* One or more managers can fail and the survivors will keep the swarm running (High-Availability)
* Deploy an odd number of managers
* Don't deploy too many managers (3 or 5 recommended)
* Spread managers across availability zones
## Networking

1. The Container Network Model (CNM) - design specification
2. Libnetwork - real-world implementation of CNM.
3. Drivers - extends the model by implementing the specific network topologies
![[Pasted image 20240526185418.png]]

#### Single-host bridge networks

* Only spans a single Docker host and can only connect containers that are on the same host
* Every docker host gets a default single-host bridge network. Linux(bridge) and Window(nat)

#### Multi-layer overlay networks

* Single network can span every node in a swarm, allowing containers on different hosts to communicate (container-to-container communication)

```bash
docker network create -d macvlan \
--subnet=10.0.0.0/24 \
--ip-range=10.0.0.0/25 \
--gateway=10.0.0.1 \
-o parent=eth0.100 \
macvlan100
```

## Overlay networking

* Built on top of `libnetwork` and the native `overlay` driver.

## Volumes

1. Persistent data
		containers need to store data in volumes.z
2. Non-persistent data
		every docker container gets its own non-persistent storage and coupled with the lifecycle of a container. Deleting the container will delete the storage and data.

Containers are designed to be read-only but many applications require a read-write filesystem in order to run. So, containers have thin read-write layers on top of read-only images they're based on.

* Volumes are independent objects that are not tied to the lifecycle of the containers.
* Volumes can be mapped to specialized external storage systems.
* Volumes enable multiple containers on different Docker hosts to access and share the same data.

#### Creating and managing Docker volumes

```bash
# creates volume with buitin `local` driver
docker volume create myvol

docker volume prune # deletes all volumes not mounted containers
docker volume rm # specify which volume to delete
```
## Docker stacks

## Security in Docker
	
