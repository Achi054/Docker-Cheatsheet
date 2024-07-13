[[__TOC__]]

# Docker
Docker is a container technology: A tool for creating and managing containers.

## What is Docker
- LightWeight, open, secure platform
- Simplify building, shipping and running apps
- Shipping container for code
- Runs natively on Windows and Linux server
- Relies on Images and Containers

## Virtual Machine vs Container
![container_vs_virtual-machine](./Images/containers-vs-virtual-machines.png)

## Docker Engine
Docker engine is the underlying technology that runs the containers.

## Docker Desktop
This includes Docker Daemon and CLI tool. The Daemon is a process that keeps on running that runs the Docker and is the heart of Docker.

## Images
Templates or blueprints for containers. This contains code and required tools and runtime. We create multiple containers(running application) based on the image.

### Image tag
A tag is a label applied to a Docker image in a repository. Tags are how various images in a repository are distinguished from each other.

### Build image
```js
-- Move to the folder containing the _Dockerfile_
docker build .

-- with name and tag
docker build -t <name>:<tag> .
```

### Get list of images
```js
docker images
```

### Inspect an images
```js
docker image inspect <image-id>
```

### Remove images
```js
-- make sure the images are not used in any containers
docker rmi <image-id> <image-id> ...
```

### Remove all images
```js
-- make sure the images are not used in any containers
docker images prune
```

### Create a clone of image
```js
docker tag <old-name>:<tag> <new-name>:<tag>
```

### Delete all images
```js
docker image prune -a
```

## Container
A standardized unit of software. A package of code and dependencies to run that code. 

ex: NodeJs code + NodeJs runtime

### Why do we need containers
- We want to have exact same versions in development and production. This ensures that it works as expected in all environment.
- It should be easy to share a common development environment. Setting a new system.
- We dont want to uninstall and re-install local dependencies all the time.

### Commands to run an app
```js
-- base image or application runtime image.
-- will check if 'node' image exists else pulls from the image repository.(hub.docker.com)
FROM node

-- set the directory inside the container, default is 'root(.)'. We push our application within a folder 'app'. Created if not exists.
WORKDIR /app

-- Copies file from our current directory(directory our script is running in) to containers working directory.
COPY . /app

-- command to run the installation so as to install the dependecies. Run once for the image composition.
RUN npm install

-- port inside container our app is exposed.
EXPOSE 80

-- command to be run when the container is spawned from the image.
CMD [ "node", "server.js" ]
```

### Run container
```js
docker run -p 3000:3000 <image-id>

-- create container with name
docker run -p 3000:3000 --name <name> <image-id>

-- create container from the image name:tag combination
docker run -p 3000:3000 --name <name> <image-name>:<image-tag>
```

### Run container in detached mode
```js
docker run -p 3000:3000 -d <image-id>
```

### Run container and remove on stop
```js
docker run -p 3000:3000 -d --rm <image-id>
```

### Re-start an already created container
```js
docker start <container-name>
```

### Re-start an already created container in attached mode
```js
docker start -a <container-name>
```

### Re-start an already created container in attached and interactive mode
```js
docker start -a -i <container-name>
```

### Attach to a running container
```js
docker attach <container-name>
```

### Fetch logs of a running container
```js
docker logs <container-name>

-- read future logs
docker logs -f <container-name>
```

### Running container with interactive session
```js
docker run -it <container>

-- i: interactive mode
-- t: terminal
```

### Stop container
```js
-- get running processes
docker ps

-- stop container
docker stop <container-name>
```

### Get list of containers
```js
docker ps

-- get all container
docker ps -a
```

### Remove containers
```js
-- make sure the containers are not running
docker rm <container-name> <container-name> ...
```

### Copy file or folder into a running container
```js
-- for file
docker cp <file> <container-name>:<path-in-container>

-- for entire folder
docker cp <folder>/. <container-name>:<path-in-container>
```

## DockerHub
Official Docker image registry. Contains public, private and official images.
[DockerHub.com](https://hub.docker.com/)

### Pushing local image to DockerHub
```js
-- requires you to login to DockerHub
docker login

-- push to your DockerHub repository
docker push <account-name>/<image-name>:<optional-tag>
```

### Pull images from repository
```js
docker pull <account-name>/<image-name>:<optional-tag>
```

## Volumes
Volumes are folders in the host machine hard drive which are mounted (made available, mapped) into the containers. 

Volumes persists even if a container is shuts down. A container can read and write to the volumn.

### Check existing volumes
```js
docker volume ls
```

### Run container with volume
#### Anonymous volumes
- Created specifically for a single container. 
- Suvives container shut-down/restart unless _--rm_ is used.
- Cannot be shared across containers
- Since it's anonymous it can't be reused. Unless you know the name(_docker volume ls_)

```js
docker run -p 3000:80 -d --name <container-name> -v /app/feedback <image-name>
```

#### Named Volumes
- Created in general, not tied to any container
- Survive container shut-down/restart
- Can be shared across containers
- Can be re-used for same container

```js
docker run -p 3000:80 -d --name <container-name> -v feedback:/app/feedback <image-name>

-- feedback:/app/feedback
-- <host-named-volume>:<container-volume>
```

#### Bind Mount
Bind mounts are volumns managed by you. You define a folder or path in your host machine that then maps within the container. Great for persisting, editable data.

Note: You need to enable file sharing in docker desktop. Settings -> Preferences -> File sharing.
Make sure to set your path from this list. [How to add a new folder?](https://headsigned.com/posts/mounting-docker-volumes-with-docker-toolbox-for-windows/)
Windows, if you dont find File sharing, its defaultly enabled.

- Location in host file system, not mapped to any container
- Survive container shut-down/restart
- Can be shared across containers
- Can be re-used for same container

```js
docker run -p 3000:80 -d --name <container-name> -v feedback:/app/feedback -v <complete-path-in-host-system>:<path-in-container> <image-name>
```

### List volumes
```js
docker volume ls
```

### Inspect volumes
```js
docker volume inspect <volume-name>
```

### Remove volumes
```js
docker volume rm <volume-name>
```

## ARGuments and ENVironment variables
### ARGuement
- Available inside the Dockerfile, not accessible by CMD or any application code
- Set on image build (_docker build_) via _--build-args_

```js
docker build -t <image-name>:<tag> --build-args DEFAULT_PORT=8000 .
```

### ENVironment
- Available inside of the Dockerfile, accessible in application code
- Set via _ENV_ in Dockerfile or via _--env_ on _docker run_

```js
docker run -p 3000:8000 --env PORT:8000 -d --rm <image-name>
```

You can also have a environment file(_.env_) containing the environment details
```js
docker run -p 3000:8081 --env-file ./.env -d --rm <image-name>
```

## Networks
Container networking refers to the ability for containers to connect to and communicate with each other, or to non-Docker workloads

### Create Network
```js
docker network create <network-name>
```

### List Networks
```js
docker network ls 
```

### Connecting to WWW 
Communicating to WWW from the container works outside the box. It works based on the port (-p) exposed from container to host machine.

### Connecting to host services
Communicating to host machine services is possible through an update in the containerized urls.
Use _host.docker.internal_ as address, this is understood by the docker engine and maps to the host machine IP address.

```js
From: mongodb:localhost:27017/swfavorites
To: mongodb:host.docker.internal:27017/swfavorites
```

### Connecting to different containers
Communicating to another container is possible through either by detecting the IP address of a running container by using _docker inspect <container-name>_ command, find _Networkings_ section with node _IPAddress_ and updating the app code with that IP address. This could be cumbersome, as we need to check the IP address of the containers each time its run and update all the apps with the new IP.

There is a better way to get this done by using _--network_
```js
-- create network
docker network create favorites-net

-- update apps connection with container name, the docker engine maps as they belong to the same network.

-- run containers in the same network
docker run --name mongodb -d --rm --network favorites-net mongo
docker run --name favorites-app -d --rm --network favorites-net favorites:0.3
```
NOTE: When connecting to container to container we do not need to provide the port(--p).

## Runnnig full stack application
Consider the reference project name _multi-01-starting-setup_.

```js
-- Running MongoDB database (Image downloaded for DockerHub)
docker run --name mongodb --rm -d --network goals-net -v data:/data/db -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo

-- Running backend server
docker run --name goals-backend --rm -d -p 80:80 --network goals-net -v C:\Users\sujit\source\git\Docker\multi-01-starting-setup\backend:/app -v logs:/app/logs -v /app/node_modules goals-node

-- Running frontend client
docker run --name goals-frontend --rm -d -p 3000:3000 -it -v C:\Users\sujit\source\git\Docker\multi-01-starting-setup\frontend:/app -v logs:/app/logs goals-react
```



