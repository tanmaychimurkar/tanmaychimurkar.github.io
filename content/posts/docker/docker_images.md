---
title: "Docker Images and DockerFile"
date: 2022-08-11T10:18:38
description: "This post goes over what are DockerFiles and Docker Images"
tags: ["Docker","Basics","microservices"]
categories: ["Basics", "Docker"]
author: "Tanmay"
showToc: true
TocOpen: true
weight: 99
cover:
    image: "https://miro.medium.com/max/1400/1*vb_5008Zbt_pHj7qx44p0Q.png"
    alt: Docker
    caption: "Process flow for creating custom Docker Images using DockerFile"
ShowBreadCrumbs: true
---
 ## Docker Images🐳

As we saw in the [basics of Docker]({{< ref "docker_intro" >}} "Docker Basics") post, the base flow of every Docker
container is an image. A Docker image, whether custom or pre-built, is absolutely necessary for us to be able to run
anything using Docker. 

In essence, a Docker image is a union of layered filesystem stacked on top of each other. This stands true fro pre-built
images as well as custom images that we might create using some pre-built image as a base. 

`Trivia💡`: If every Docker image is a set of layered instructions, then how is the first Docker image created? What
did the very first Docker image have as a layer to be built on top of?

Hoping that the description of the Docker images is clear now, let's now take a look at our first Docker image. And
what better to look at than the image of **Ubuntu** itself!?!?

### Docker Image on Hub
Use [this](https://hub.docker.com/_/ubuntu) link to navigate to the Docker Hub page of the Ubuntu image. Once there,
go through the '**What is Ubuntu?**',  and '**What's in this image?**' section. As you might have read, this image ***is Ubuntu***.
We can use this image for general purpose stuff on Ubuntu. 

Now let's go to the '**Tags**' section on the webpage, and see what we find there. We can see that there many tags, 
and the first one that appears should have the TAG '**Latest**', and on it's right, there should be a command that says: 
`docker pull ubuntu:latest`. If we look at the next tag after **Latest**, notice how the `docker pull` command changes 
slightly with the name of the **TAG** that we are referring to! Also, take a look at the size of the image. It should be 
around 25-30 MB. WHATTTTTTT!!!! This means that the docker image that can run **Ubuntu** instructions is only 30 MB in size.🙀

Let's stop for a moment here and summarize what we have seen so far:
1) Docker Hub has many pre-built images ready for us to explore
2) Almost all the images have a description about what they are and a small summary of what they contain. Some have 
much greater description in terms of the **Environment Variables**, but let's look at it later
3) All the pre-built images on Docker Hub have **Tags**, and every tag is a version of the image
4) Every image tag has a set of instructions about how to **pull** it. 

Let's understand what the **pull** command does!🐕

### Docker Pull🧗

Seeing that we have a command linked to a image on Docker Hub, naturally the next step is to get our hands dirty. This
 helps me get out of my comfort zone, but also helps me bring a concept that I am trying to grasp much easier to 
understand. I hope this also works the same for you🐈

**Imp. Note🐳**: For getting our hands dirty here, it is advised to go through the installation of 
[Docker Desktop](https://docs.docker.com/get-docker/). Personally, I like the 
[Docker Engine](https://docs.docker.com/engine/install/ubuntu/) with the 
[compose](https://docs.docker.com/compose/install/other/) tool more, but to begin with, either of the two installations 
should work fine.

With Docker installed on your system, let's verify the docker version. This can be done by opening a terminal and 
typing:

```bash
docker --version 
```

If you see a message with version, then we are good to get into the fun part.

In the terminal, type the below command to check the current images that docker has on your machine: 
```bash 
docker images
```

If you see nothing, or only see the `hello-world` image, then we are good to go. Let's now run the command that we saw 
on Docker Hub page of **Ubuntu**:
```bash
docker pull ubuntu:latest
```

Once you run this command, you should see something similar to this:
```bash
latest: Pulling from library/ubuntu
6e3729cf69e0: Pull complete 
Digest: sha256:27cb6e6ccef575a4698b66f5de06c7ecd61589132d5a91d098f7f3f9285415a9
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest
```

What `docker pull` does is it pulls the image from Docker Hub into your local machine. Once the above output is visible 
in the terminal, we can check the images again with `docker images`, and now we should see the `ubuntu` image with the 
`latest` tag in the terminal.

This is the way for us to pull pre-built images from Docker Hub

## DockerFile🐳

Since Docker images are a union of layered filesystem stacked on top of each other, we can also build our own custom 
image that does something that we want by building it on top of another pre-built image. To build our own image, we 
first need to create a `DockerFile`, which is a configuration file that has commands that Docker uses to build our 
image. Let's have a look at the terminology that the `DockerFile` has.

### Terminology📖

`DockerFile` is just a text document that contains a set of instructions in order that Docker has to execute to build
our image. Let's now look at some of the most used commands that we need to build our own Docker image, while the main
 list of commands is still available under [DockerFile reference](https://docs.docker.com/engine/reference/builder/).

1) `FROM`: This commands sets the base image that we want our own image to be built on top of. Every `DockerFile` 
should start with a `FROM` command, since every image is built on top of another image. 

2) `RUN`: As the name suggests, this command is used to execute a set of instructions on top of the base image that we 
use for our own custom image. `RUN` commands can be *run* in two ways: either in *shell* form, or in *exec* form, which
means there are two ways to *run* commands for the `RUN` instruction. The *shell* form for `RUN` directly uses the 
instruction like `RUN sh -c echo hello`, while in the *exec* form, the same command would be run as `RUN ["sh", "-c", 
"echo hello"]`

3) `COPY`: This command is used to *copy* something from our project code inside the Docker image. The copy command 
needs a `source` location to copy from and a `destination` location to copy inside the Docker image. 
4) `WORKDIR`: Sets the current working directory for the Docker image, such that all the instructions that follow the
after setting `WORKDIR` will be executed from that directory.
5) `CMD`: This instruction is used to specify the default arguments that we want the Docker image to take when we run 
it. There can only be **one** `CMD` instruction in a DockerFile, and if we have multiple, then only the last one will 
be executed. `CMD` instruction works as setting default arguments when we want to run an image.
6) `ENTRYPOINT`: As `CMD` sets a default command, the `ENTRYPOINT` command can override it. However, how the command
is overridden depends on the whether the *shell* form is used or the *exec* form is used. The most intuitive explanation
of the interaction between `CMD` and `ENTRYPOINT` is in the 
[Docker documentation](https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact)

Whew!!😌 That was a lot of terminology, and we have not even covered eveything from the 
[DockerFile reference](https://docs.docker.com/engine/reference/builder/). However, we do not need to understand how 
each and every parameter works in order to get started. Instead, these are the most common commands that are usually 
inside a `DockerFile` while building custom images.

Let's now build our own Docker image by building a `DockerFile`🤩

### Creating a DockerFile✍️

If you have gone through the installation process of installing Docker, you might have come
across the `hello-world` example of docker. But in our case, let's redo the `hello-world` example in `Python`,
but by building our own Docker image.

First thing is to create a simple Python script by placing the following line inside:

```python
print(f'Hello world from inside the Docker container!')
```

Next step is to copy the below code into a file and naming it `Dockerfile`:

```dockerfile
FROM python

WORKDIR /usr/src/app

COPY hello_world.py ./

CMD ["python", "hello_world.py"]
```

In this file, we can see the following thing:

1) The first command we write is the `FROM` instruction, with the `python` image being used. Note that if we do not
mention a default tag, then the `latest` tag is taken as default.
2) The second instruction is setting the `WORKDIR`, which is like navigating to the `/usr/src/app` directory
inside the container.
3) The third instruction is `COPY`, which selects the `hello_world.py` to the current working directory set 
by the `WORKDIR` command
4) The last instruction is `CMD`, which sets the default command to run when the image is started as a container.

### Building your image⚒️
Once the above file is created, navigate to the folder containing the file and `build` the image using the 
`Dockerfile` that we created in the step above. The `build` command is used to build an image from a particular 
`Dockerfile` by using all the instructions from inside the `Dockerfile`. The `-t` flag is used to give a name and a tag
to our image, similar to the name and the tag we see on Docker Hub. Run the following command in the terminal where the 
`Dockerfile` is located:
```bash
docker build -t custom_image:latest .
```

Once the above command is run, we should see something like this in the terminal:
```bash
Step 1/4 : FROM python
latest: Pulling from library/python
f2f58072e9ed: Pull complete 
5c8cfbf51e6e: Pull complete 
aa3a609d1579: Pull complete 
094e7d9bb04e: Pull complete 
2cbfd734f382: Pull complete 
aa86ac293d0f: Pull complete 
4cffc9f44941: Pull complete 
ae2c75627c86: Pull complete 
2d2b74d2f0f7: Pull complete 
Digest: sha256:11560799e4311fd5abcca7ace13585756d7222ce5471162cd78c78a4ecaf62bd
Status: Downloaded newer image for python:latest
 ---> 539eccd5ee4e
Step 2/4 : WORKDIR /usr/src/app
 ---> Running in a63f44fb58c6
Removing intermediate container a63f44fb58c6
 ---> 266dd62fca08
Step 3/4 : COPY hello_world.py ./
 ---> 016f934d50e5
Step 4/4 : CMD ["python", "hello_world.py"]
 ---> Running in b38de1b256fa
Removing intermediate container b38de1b256fa
 ---> 432ba341135f
Successfully built 432ba341135f
Successfully tagged custom_image:latest
```

As we analyze the output of the `build` command, we can see the following things:
1) Every instruction starts with the `Step` comment, followed by the step number which is currently being executed. For example, in 
`Step 1/4`, we can see that the docker engine is pulling the image from the Docker Hub to our local machine.
2) Every instruction that we provided in the `Dockerfile` is echoed first, followed by its execution hash.
3) Once all the instructions are executed, we get the final message saying `Successfully built <hash>` and `Successfully
tagged custom_image:latest`. This message means that our custom image is now built.

### [Bonus]Running your custom image🏃

Once the above commands are run in the order we specify, we can check the list of images we have on our local machine by
running the following command in the terminal:
```bash
docker images
```

Here, we should see our `custom_image` being listed amongst other images, if any. Now that we have built our custom image,
it is time to run it. For now, let's copy the following command in the terminal to run the image as a container
(Don't worry about the below line of code, we will look at it in the
[here]({{< ref "docker_containers" >}} "Docker Containers")):
```bash
docker run custom_image:latest
```

Once we execute the above command, we should see our print message in the terminal: `Hello world from inside the Docker container!`

Congratulations!!🥳🎉 You have successfully built your first custom image. You are already a Docker Pro!!🤓

### Summing Up!💡

We saw in this post how once can either pull images from the Docker Hub or create their own custom images, and run them
from the terminal. The [next]({{< ref "docker_containers" >}} "Docker Containers") post will now focus more on the 
later part of running the image once we have it on our machine.

Until then, Cheers!!🕺💃
