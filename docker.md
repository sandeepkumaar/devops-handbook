## Virtualisation
Virtualisation is the act of creating *virtual* version of something at the **same abstraction level** -- wiki

## Operating sytems and their layers
Operating system is a software that runs on a hardware utilising all the hardware resources like memory, drives etc

OS consists of 2 layers
- Kernel 
- Application layer

Kernel is one that interacts with the hardware. contains the processor like intel, AMD, Mac M1
Application layer is the GUI, libs that the user interacts

## Difference between VMs and docker: 
VMs virtualises both **Kernel and Application** layer  
Docker virtualises on **Application** layer and uses the **Host's** kernel  

VMs can run on any host containing any OS (Windows, linux, Mac)  
Since it virtualizes the entire OS,  the **size of VM is high**, and scaling the VMs vertically may have some limitations  

Docker on the other hand only virtualizes the *Appllication layer* and uses the Host kernel.  
Since it uses the Host's kernel, its **size is very small** and can run on any Host sharing the *same kernel* 
It can also be easily distributed through dockerhub.   
But it cannot run on a different kernel. Ie. a docker image created with linux based image cannot run on windows machine/kernel.  

Ref: https://www.youtube.com/watch?v=5GanJdbHlAA&t=55s


## Docker image
APP + Services needed + OS 

Registry Versioning - tags


## DockerFile
```
FROM node:19-alpine // base image from which we want to build our app. it contains, Linux OS, node, npm

COPY pacakge.json /app/  // COPY is similar to cp. it copies files/folders from local to image. '/' at the end creates dirs if not available
COPY src /app/

WORKDIR   // equivalent to cd. goes to specific path in the image.


RUN npm install    // RUN is used to run any shell commands in the linux os.
```
Note: Each entry in the Dockerfile creates a image layer and laid on top of the previous entry. 
Having more image layers will slow down building process and may increase the size.

## Dockerfile best practices

1. Use Official Base image - for security
2. Use specific version of the Base image. Having 'Latest' tag may pull new version which may break the app
3. Use smaller base images - Eg: official node:alpine comes with a minimal linux OS with node and npm. 
4. Use .dockerignore file to avoid copying unnecessary files that are not required to build/run
5. Dont use Root user to run container - By default, dockerfile commands are executed in Root user. while running the cmd to run the application
its better to use a non-root user. so the app has limited priveleges on the Host system.
```
RUN chown -R node:node /app # some base images have a generic non-root users. here 'node'
USER node  # command to change user
CMD ['node', 'index.js']
```
6. Running docker scan as part of the pipeline

7. Optimize caching Layer
We know that dockerfile commands are created as a image layer, and each layer will be cached locally.
When we rebuild the image again. docker will check whether the contents with respect to each command has changed or not.
If there is no change, it will use the cached layer for that command. 
If there is a change, it will create that layer and also invalidates all other subsequent layers and creates them again. -- even if they haven't changed

Eg: we have src/, package.json. src changes often and package.json changes rarely. 
using COPY to copy both of them will again create the layer with new nodemodules even though it hasn't changed.
So its better to split the commands. so the most changing ones go to the bottom
```
COPY package.json /app
RUN npm install

COPY src /app
CMD ['node', 'index.js']
```

8. Build vs Run
In most application, Build dependencies are not required to *run* the application. 
Having build dependencies packaged in to the final image will increase its size. 
Our final image - should only have necessary files and libs to run the application.
To ignore the build dependencies from the final image. we use **multi-stage builds**

```
# Build stage
FROM node:alpine 
WORKDIR /app
COPY package.json /app/
npm install 
COPY src /app/
npm run build

# Copy only the build and run
FROM node:alpine
WORKDIR /app/
npm install -g http-server
COPY --from=build /app/public /usr/var/
CMD ['http-server', 'public']
EXPOSE 8080
```

Ref: https://www.youtube.com/watch?v=8vXoMqWgbQQ


## Docker cheatsheet

### Pulling a container
```
$ docker pull image:tag
```

### Running a container
```
$ docker run [options] image
-it     # to run in interactive mode in tty
--name  # container name
-h      # constainer hostname     
-p      # hostPort:containerPort
-v      # volume 
```

## List all container
```
$ docker ps # list running container
--all # list all 
```

## Inpecting a container
```
$ docker logs <container>
```

## Pruning Container/Images

## Building image
```
$ docker build --tag # create image with name
```
## TODO
- ENV variables
- Volumes
- Network
- DockerCompose 
- How to spin a mongodb container and connect to it.














