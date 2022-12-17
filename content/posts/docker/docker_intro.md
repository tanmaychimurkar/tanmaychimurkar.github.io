---
title: "The basics of Docker to get started"
date: 2022-08-10T04:17:32
description: "This post goes over the basic concepts of Docker, and why we might want to use it for our software!"
tags: ["Docker Images","Basics"]
categories: ["Basics", "Docker"]
author: "Tanmay"
showToc: true
TocOpen: true
weight: 1
cover:
    image: "https://www.Docker.com/wp-content/uploads/2022/03/horizontal-logo-monochromatic-white.png"
    alt: Docker
    caption: "The [Docker](https://www.Docker.com/) logo showing a **`Whale`** as a Docker to dock **`containers`** for us"
ShowBreadCrumbs: true
---
 ## Why and What⁉️
️In Software Development cycle, developers of every level have either heard or said the following:
> It works on my machine, I am not sure why it isn't running on yours

Different software projects have different dependencies, that only keep going uphill as the product itself evolves.
When this happens, we need to make sure all interactions between different components have been done correctly in 
order for the whole application to come together and `run`.

`This is the very scenario where things can fall apart very easily!`

### Why Docker🤔

In scenarios like mentioned above, wouldn't it be useful to have a tool that makes sure that a software is bundled 
in a way that is irrespective of the base dependencies it requires to be built on?

As I primarily work in [Python](https://www.python.org/)🐍, I would like to throw in an analogy. This abstraction tool 
can be thought of like a [`virual environment`](https://github.com/pyenv/pyenv-virtualenv), with each project having 
its own separate dependencies which do not interfere with a different project's virtual environment, and every 
environment satisfying all the module level dependencies that the project needs.

> In short, the tool that we need should have isolation and dependency matched as per the part of the project that is 
> running on it!

Having such a tool to manage whole software, such that it handles all the base dependencies like that of the OS and any 
other package level dependencies are managed for us would be great, wouldn't it?

### What is Docker🐳

In-line with the objectives that we want the above-mentioned tool to perform, let's take a look at Docker. 

>Docker helps abstract away the dependencies for components between varying OS, software, and hardware requirements, 
> and lets every part of the project run in an isolated environment.

Feels like technical mumbo jumbo? Well let's take another go at understanding this. The abstraction that Docker provides
can be thought of in the following way:

1) Imagine breaking the whole software into multiple smaller pieces, with each piece handling one part of 
some logic. This is what we call an isolated part of the project.
2) Each part will naturally have a set of requirements in the forms of packages or configurations for it to be able to 
run correctly. This is the dependency that the project has.
3) Running our project using Docker will abstract away the above said requirements for other people, and anybody running
 our project will use the configurations we have set when moving the project to Docker.

I hope it is more or less clear what Docker does. If not, fret not! Things should become clearer as we go further with
terminology.

## Terminology📖

So far, we have been trying to understand what Docker does in our own analogy and terms. However, things can go out of 
hand if one does not follow a [standard terminology](http://www.computing.surrey.ac.uk/ai/pointer/report/section1.html)🐱.

Let's dive in the terminology that will make our life easier to follow.

Obviously, one should at the end refer to the official [glossary](https://docs.Docker.com/glossary/#image) mentioned by 
Docker. However, to quickly get us started, I have mentioned some base terminologies below. 

### Docker Image✒️

Since we need to create an isolated environment, we first need to decide what will the base of such an environment have. 
This base that each isolated environment needs is defined in a Docker image, which contains union of layered filesystems
stacked on top of each other. 

>We can think of image as a small pre-compiled software on which we can run our assigned part of the project, 
> and using an image is like installing Python or Ubuntu on your own machine.

Because of the awesome strength of the Docker [community](https://www.docker.com/community/), they decided to keep a 
collection of the images that people need. This collection of images is listed on the 
[Docker Hub](https://hub.docker.com/), and we can find all sorts of images, like Alpine Linux and Python.

`Note`: Although the Docker hub has many images readily available, we can also create our own Docker images. And as we 
read above that images are union of layered filesystems, our own image will also be a layer over some base image.

A more detailed explanation of Docker Images is [here]({{< ref "docker_images" >}} "Docker Images").

### Docker Container📦

So we saw that image in Docker is a base system that we want to run our piece of the project. When we run such an image,
we get a Docker container. 
>In short, a Docker container is a running instance of a Docker image. 

A container always needs an image, and along with it, we sometimes pass a set of execution parameters such that the 
container has the parameters that we want, for example it's name.

A more detailed explanation of Docker containers is [here]({{< ref "docker_containers" >}} "Docker Containers").

## Summing it up!💡

What we saw so far is a very basic understanding of what Docker is, why we might need it, and what are the basic things 
we need to understand for breaking our project into small running containers. These small running containers can also
be thought of as very small virtual machines, and this will be more clear when we look at the 
[Docker Containers]({{< ref "docker_containers" >}} "Docker Containers") post. In the 
[next]({{< ref "docker_images" >}} "Docker Images") part, let's look at Docker Images in a bit more detail and how we 
can create our image. 😺

[//]: # (like having multiple `small-separate` machines running different pieces of the software &#40;which we )

[//]: # (specify&#41;, running the whole software as Lego blocks, with each of the small machines helping each other. )

[//]: # ()
[//]: # ()
[//]: # ()
[//]: # (Containers in Docker have their own network, their own volume mounts, their own networks, and are just like VMs. The)

[//]: # (only difference is that all the containers share the same os as the machine on which they are running, unlike VMs who)

[//]: # (also have their own OS when created.)

[//]: # ()
[//]: # (Container use the kernel to run their process. In such, a linux based OS will only be able to run linux based containers)

[//]: # (on it, and a windows based container will not be able to run on a windows based machine.)

[//]: # ()
[//]: # (Images in Docker are packages or templates that are used to create one or more containers. every container needs an)

[//]: # (image to be able to run.)

[//]: # ()
[//]: # (DockerFile is the configuration of an image should be initialized. DockerFile creates a Docker image, and image runs)

[//]: # (a container.)
