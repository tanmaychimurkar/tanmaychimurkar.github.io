---
title: "Docker Container and docker-compose"
date: 2022-08-10T04:17:32
description: "This post goes over the usage of Docker container, how to run them, and how to create a compose file so that many Docker images can interact with each other"
tags: ["Docker", "microservices"]
categories: ["microservices"]
author: "Tanmay"
showToc: true
TocOpen: true
cover:
    image: "https://diginomica.com/sites/default/files/images/2017-09/docker-container.jpg"
    alt: Docker
    caption: "The Docker container being run by the Dockerfile inside"
ShowBreadCrumbs: true
---
 ## What are they?

In the [previous]({{< ref "docker_images" >}} "Docker Images") post, we saw how to find Docker images form the [DockerHub](https://hub.docker.com/), and also how to create our own Docker image that `executes` a function. In this post, we will go more over the execution part of Docker, mainly about how to run a Docker image as a container. 

[Docker Containers](https://www.docker.com/resources/what-container/) are Docker images that become containers at run time. Docker containers are short pieces of software that runs an application quickly, as a lightweight standalone package. The containers need an engine to run, and this is the [Docker Engine](https://docs.docker.com/engine/). 

Basically, Docker containers make it easier to decentralize a large piece of software such that it can run in isolated environments, and they are what we can refer to as `microservices`. So in this part of the post, we will be running our first use case of `microservices`. We will start with running a single Docker image first, then we will combine many Docker images to run together so that they can interact with each other and run the whole software.

## Running a Container

In the [previous]({{< ref "docker_images" >}} "Docker Images") post, we saw how we can either pull a pre-built Docker image from the DockerHub, or create our own custom image. We saw at the end that we had to `run` a specific command in the terminal to get the output from a Docker image. The commands that we ran in the previous post ran the Docker image, and during runtime, it created a Docker container that runs the piece of code that we put inside our own image or the outputs from a pre-built image from the DockerHub.

If you followed along with the [previous]({{< ref "docker_images" >}} "Docker Images") post, you should now see the `Ubuntu` image in the terminal when you execute the below command in your terminal.

```bash
> docker images
REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
custom_image        latest    432ba341135f   4 weeks ago     932MB
ubuntu              latest    6b7dfa7e8fdb   5 weeks ago     77.8MB
```

In case you did not follow the previous post, we can run the following command in the terminal first to pull a Docker image and then continue:

```bash
docker pull ubuntu:latest 
```

Now we are ready to proceed. To start the Docker container, we can start by typing the following command in the terminal

```bash
docker run ubuntu
```

If all is correct, you should see, well nothing. But why is that? We just ran our `Ubuntu` image as a Docker container, but we don't see anything on the terminal when we run it. 

### `docker ps` command
To check whether a particular image is successfully run or not, we need to execute the following command in the terminal:

```bash
docker ps
```

The [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps/) command lists all the containers. So if you ran the `Ubuntu` image as per the previous step, then the `docker ps` command should show that the `Ubuntu` container is running. Let's try to check if that is the case. In the terminal, execute the following command:

```bash
docker ps
```

However, the output we see in the terminal should be the following:

```bash
> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Hmm, weird right? The documentation says that the `docker ps` command gives us a list of the containers. But we do not see anything in the terminal. This is because the `docker ps` command only list the containers that are `running` by default. And for our `Ubuntu` image, we did not give any specific command to run when we started the container via the `docker run` command. Which is why it is not showing up under the `docker ps` command. To check if the container was ranning when we started it, we need to pass the `-a` flag to the `docker ps` command. Let's run the following command in the terminal to check this:

```bash
> docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                      PORTS     NAMES
4c42acd7b1cd   ubuntu                "bash"                   10 minutes ago   Exited (0) 10 minutes ago             condescending_villani
```

We should see something like above in the terminal, with a different string under the `NAMES`  and the `CONTAINER ID` column. Let's see what terms shown in the output of the `docker ps -a` command mean.

Below is a list explaining what each of the terms given in the output above mean:
1) `CONTAINER ID`: This column lists the id that a Docker container is given when it is run. This is a random id that is generated everytime a Docker container is run. If we run the same Docker image multiple times to start containers, it is very unlikely that the `CONTAINER ID` will be the same across different containers.
2) `IMAGE`: This column gives the name of the Docker image that was used to start the Docker container. In our case, since we started the container using the command `docker run ubuntu` in the terminal, the `IMAGE` section correctly lists the `ubuntu` image in its run.
3) `COMMAND`: This column lists the default command that is run when the Docker container is started. Since for the case of the `ubuntu` container, we did not specify any command, a default `bash` command is run on startup. 
4) `CREATED`: This field mentions the time when the container is started. 
5) `STATUS`: This field gives us a status of the container. In our case, we see the status code as being `Exited (0) 10`. This signifies that the `ubuntu` container was started, and is now shut down and is not running. The `STATUS` field helps us figure out the current status of a Docker container once it is started, and it can take different values, ranging from `Creating ...` to `Up ...`, which indicates that the container is either starting up or it is running for the amount of time mentioned after the `Up` string, respectively.
6) `PORTS`: This field gives a list of ports that are to be exposed from inside the Docker container to the local user's machine, so that the local user can access the port outside of Docker's network
7) `NAMES`: This field outputs the name that the docker container has when it is running. The name of a Docker container when it is run is also generated at random everytime a Docker container is started, but unlike the `CONTAINER ID` field, the name can be controlled and kept static for a particular Docker container. 

So, these are all the fields that are showed by the `docker ps -a` command. Now let's start tampering with the `docker run` command to get a bit more out of the containers when we run them.

### Passing arguments to `docker run` command

In the previous step, we just used the `docker run` command out of the box on the `Ubuntu` image. However, that did not give us much, not even a friendly terminal to let us inside the container that is running. Let's now try to get inside the `Ubuntu` container to check what it has.

To do this, we first need to make the `Ubuntu` container execute a different command at runtime when it is started. For this, we can pass additional arguments to the `docker run` command in the terminal as follows:

```bash
docker run ubuntu sleep 3600
```

The `sleep` command that we pass is not a Docker argument, but a `Linux` system argument.

`Tip`: You can check more about the sleep command by running `man sleep` in your terminal (Only works for Linux-based OS like Ubuntu or MacOS)

The `sleep` command will override the default `bash` command from the `Ubuntu` container, and make the container sleep for the 3600 seconds 60 minutes. That should give us enough time to check what is inside the container.

So, once we have run the above command, we should see that the terminal does not exit like in the initial `docker run` command, but is in a way just stuck. However, if you open a new terminal and list all the containers with `docker ps -a`, we should now see the following:


```bash
> docker ps -a
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS        PORTS     NAMES
8ec5ceccbd72   ubuntu    "sleep 3600"   3 seconds ago   Up 1 second             fervent_mclaren
```

Notice how in the `STATUS` field it now shows `Up 1 second`. And also take a look `COMMAND` section to see that it now shows `sleep 3600` as the command instead of `bash` like the previous case. That means the command that we passed while starting the container worked, and is executed as soon as the `Ubuntu` container is started. Let us now see what is inside the `Ubuntu` container.

### Going inside a running container

Now that the `ubuntu` container is running, we can look inside it. The default method to look what is inside a container is to get a terminal, and we can do the same for the `ubuntu` container. To get a terminal inside the container, we can run the following command:

```bash
> docker exec -it <identifier>
```
 
The [`exec`](https://docs.docker.com/engine/reference/commandline/exec/) command is used to run a command inside a running container. And to let Docker know which container we want to choose to execute the command, we need to pass an `<identifier>` above, which can either be the `NAME` of the container or the `CONTAINER ID`. Let's pass the `CONTAINER ID` for now, but we can pass `NAME` in exactly the same way.

`Pro Tip`: For passing the `CONTAINER ID` as an identifier for the `exec` command, we do not need to pass the full container id. Instead, we can just pass the first 3 characters of the `CONTAINER ID` to the exec command, and it will still be executed inside the correct running container. 

To execute and get a shell inside the running container, run the following command:

```bash
docker exec -it 8ec bash
```

Please make sure to pass the correct first 3 characters from your `CONTAINER ID`, since every `CONTAINER ID` is randomly generated, and your `CONTAINER ID` will not be the same as mine.

Once we run the above command in a new terminal, we should see the following output:

```bash
> docker exec -it 8ec bash
root@8ec5ceccbd72:/# 
```

Well, well. We are now inside our running Docker container!!!🐕

We can now `ls` to check what files/directories the container has, and we can look around inside them as part of exploration. Let's now see how we can give a name to a Docker container when we run it, and why is it useful.

### Some tips for running Docker containers

1) It is very beneficial to give your Docker container a name, since doing so will always maintain a streamlined process of seeing inside the container and accessing it for checking your processes. This can be done by passing in the `--name` flag to the `docker run` command, which is a `Docker` flag unlike the `sleep` command we saw earlier. Let's execute the following command in the terminal:

```bash
docker run  --name my_ubuntu_image ubuntu sleep 1000
```

You can pass either `my_ubutu_image` as the name of your container, or you can get creative and pass something that you like as a name for your Docker container. Once we run the above command, you can open a new terminal and check running containers with `docker ps -a`. Once you do, you should now see something like the following output:


```bash
> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
f04164a6395f   ubuntu    "sleep 1000"             3 seconds ago    Up 1 second               my_ubuntu_image
```

Notice how the docker container now has in the `NAMES` field the name `my_ubuntu_image`, or whatever name you passed along in the `--name` flag. Now if you want to `exec` inside the running container, instead of passing the `CONTAINER ID`, we can pass the name of the container and it will go inside. Let's try this out with `docker exec -it my_ubuntu_image bash` to get bash inside the docker container. You should definitely see something like the following:


```bash
> docker exec -it my_ubuntu_image bash
root@f04164a6395f:/# 
```

Now we can always refer to our container while running commands with its name.

2) Now that we have a `Ubuntu` container running, we need to know how to stop it. We need to stop old containers before running new ones, since Docker does not let us run two containers with the same name twice. To check this, run the following command again in a new terminal:

```bash
docker run  --name my_ubuntu_image ubuntu sleep 1000
```

You should get the following error message:

```bash
> docker run --name my_ubuntu_image ubuntu
docker: Error response from daemon: Conflict. The container name "/my_ubuntu_image" is already in use by container "f04164a6395f2c472bc5c6f6e59e304baa793b5d7edf106066e0764b51e1ad5d". You have to remove (or rename) that container to be able to reuse that name.
```

The error message tells us that there is a conflict with the container name, since we are already running a `Ubuntu` container with the same name. To rerun a new container with the same name, we first need to stop the one that is running currently. For this, we need to run the following command in a new terminal:

```bash
> docker stop my_ubuntu_image
```

Once you run this command, the terminal will be back after the command is executed successfully, and if you now get a list of all the containers, you should see the following:

```bash
> docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                       PORTS     NAMES
f04164a6395f   ubuntu    "sleep 1000"             12 minutes ago   Exited (137) 9 seconds ago             my_ubuntu_image
```
As you can see, the `STATUS` field now shows `Exited ...` under it instead of the previously shown `Up for x seconds`. This means that our `Ubuntu` container has stopped running, and we are ready to start another container with the same desired name. 

3) As per the above step, you now know how to stop a docker container. However, if you run the container again, you will still get the same error message with the `name conflict` as before. That is because docker not only needs you to stop the container, but to completely remove it before starting a new container. To do this, we can run the following command in a new terminal:

```bash
docker system prune
```

You should then show the following prompt in your terminal window:

```bash
> docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - all dangling build cache

Are you sure you want to continue? [y/N] 
```

[`docker system prune`](https://docs.docker.com/engine/reference/commandline/system_prune/) simply removes all the unnecessary data from your Docker environment, like stopped containers, containers that could not start correctly, or cache that a container created on your machine when it was started. In the above prompt, type `y` and press enter to let docker clear all stopped containers and the data related to them. Once this is successful, you should see a message as follows:

```bash
Deleted Containers:
f04164a6395f2c472bc5c6f6e59e304baa793b5d7edf106066e0764b51e1ad5d

Total reclaimed space: 0B
```

This message indicates that all the stopped containers are now removed, alongwith the caches that they had created when started. To check if there are any containers still, you can run
`docker ps -a`. 

If you want to manually remove only one specific container from the stopped containers afters stopping it, you can run the following command instead:

```bash
docker rm <container name or id>
```

This will remove only the container whose name or id you passed. 

The methodology of running containers from pre-built docker images can be extended to running custom images as containers that we create according to the [previous]({{< ref "docker_images" >}} "Docker Images") post, or any custom container of your choice. All the list of commands remain the same, only the pre-built `ubuntu` image from the above commands needs to be replaced with your own custom image.

## Running multiple containers and make them interact with each other

So far, we have seen how to run a single image as a container, give it a name, pass some commands to override the default commands, how to stop and remove it. However, large pieces of software are rarely built on only one running container, and there are always many containers handling one specific task of the software independent of other tasks that are run by the software.

For this, we need to run multiple containers at once, and these containers also need to be able to interact with each other in order to make the software run successfully. Once simple examples where different containers can be run to do different tasks is run a [`PostgreSQL`](https://www.postgresql.org/) database to store some results from an API, a container to run the API itself, and a [pgAdmin](https://www.pgadmin.org/) UI tool to interact with the database to check if the data that the API returns are being stored successfully inside the postgreSQL database. This sounds like a very general purpose use-case that many companies have in common, and this can be managed very efficiently via docker containers. We will see more about this in the [next]({{< ref "docker_compose" >}} "Docker Compose") post of `Docker Compose` files, which allows us to run multiple containers at once.


