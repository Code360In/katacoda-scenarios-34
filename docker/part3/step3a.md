# Docker Compose Introduction

Let's recap what you have just learned to create a full stack
application:
1. You have implemented a backend and frontend application using Pyhton scripts
2. You have built an image for each application in your Docker environment
3. You have defined and created a network so that the two applications can communicate each other
4. You have run frontend and backend containers inside the network created

Now can we define an overall Docker architecture using a single instruction or configuration?
For this purpose you can use **Docker Compose**!

Docker Compose allows you to define and running multiple containers using a yaml file configuration.
With a single command in terminal you can start and stop different containers that share the same network
or resources.

Using Docker Compose you can easily deploy in local environment all the resources you need to test
your application. For example, you can create volumes, network or running containers defining them in 
yaml configuration file.

Compose has commands for managing the whole life cycle of your application:
- Start and rebuild services
- View the status of your services
- Getting logs of running services

### Creating services
The first step is creating the configuration file for your environment. Let's try to create a compose
file for the project that you have just deployed in the previous steps

You need to create the following resources:
- running containers
- volumes
- networks

For each resource you must add a new section in docker compose file.

First delete the old containers of previous steps and then
create the configuration file in the root directory of project `project/step2c`.

`docker stop $(docker ps -a -q); docker rm $(docker ps -a -q)`{{execute}}

Check that you current directory is the root project folder `cd project/step2c` and
then create an empty docker-compose

`touch docker-compose.yml`{{execute}}

Now we add the following lines with visual text editor or `vim` (you can find this configuration
also in the file `docker-compose-temp1.yml` in the root project directory:

```
version: "2.2"
services:
  dockerchurn:
    build: .
    ports:
      - "8080:80"
    container_name: dockerchurn-container
  dockerchurn-fe:
    build: ./frontend
    ports:
      - "8081:8081"
    container_name: dockerchurn-container-fe
    environment:
      - HOST=dockerchurn-container
      - PORT=80
```
 In this step you are including:
- `services`: Services are docker containers that compose your application. You can include
containers from images in Dockerhub or you can build your own image using Dockerfile. In this 
scenario you need building images from source code seen in the previous steps:
  - `dockerchurn`: the backend service built from Dockerfile in the root directory (`build: .`)
  - `dockerchurn-fe`: the frontend service built from Dockerfile in the frontend folder
    directory (`build: ./frontend`)
    
- `ports`: In `ports` field there is the value of port mapping that you used in `docker run`
command with the parameter `-p`
  
- `container_name`: this is the container name that Docker Compose create when start the
application
  
- `environment`: in this section you can insert environment input variable for the container.
In this specific scenario the frontend service need the host name and port of the backend service

  
At the end of this step let's try this configuration starting the application with the following
command

`docker-compose up -d`{{execute}}

With `-d` parameter you can start your Docker full-stack architecture in detach mode: this option
run containers in background and print new container names (similar to `docker run`
command).

The first time this operation requires several minutes since you have to build
all the images for the containers that you defined in configuration file.

As you can see from the output, Docker has done the following steps automatically:
1. build image of the frontend and backend services
2. starts container using the specified port mapping, environment variables and container names

You can check the result using this command:

`docker ps`{{execute}}

or use this command to get logs

`docker-compose logs`{{execute}}

[comment]: <> (Using this command you can stop all the containers:)

[comment]: <> (`docker-compose stop`{{execute}})
