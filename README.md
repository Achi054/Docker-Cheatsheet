[[__TOC__]]

# Docker
Docker is a container technology: A tool for creating and managing containers.

## What is Docker
- LightWeight, open, secure platform
- Simplify building, shipping and running apps
- Shipping container for code
- Runs natively on Windows and Linux server
- Relies on Images and Containers

## Container
A standardized unit of software. A package of code and dependencies to run that code. 

ex: NodeJs code + NodeJs runtime

## Images
Templates or blueprints for containers. This contains code and required tools and runtime. We create multiple containers(running application) based on the image.

## Why do we need containers
- We want to have exact same versions in development and production. This ensures that it works as expected in all environment.
- It should be easy to share a common development environment. Setting a new system.
- We dont want to uninstall and re-install local dependencies all the time.

## Virtual Machine vs Container
![container_vs_virtual-machine](./Images/containers-vs-virtual-machines.png)

## Docker Engine
Docker engine is the underlying technology that runs the containers.

## Docker Desktop
This includes Docker Daemon and CLI tool. The Daemon is a process that keeps on running that runs the Docker and is the heart of Docker.

## Commands to run an app
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

## Build image
```js
-- Move to the folder containing the _Dockerfile_
docker build
```

## Run container
```js
docker run -p 3000:3000 <container-id>
```

## Stop container
```js
-- get running processes
docker ps

-- stop container
docker stop <container-name>
```

## Running container with interactive session
```js
docker run -it <container>
```





