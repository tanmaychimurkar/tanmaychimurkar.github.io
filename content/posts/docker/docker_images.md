---
title: "Docker Images and DockerFile"
date: 2022-08-11T10:18:38
description: "This post explores what are Docker Images and how to create one using a DockerFile"
tags: ["Docker", "microservices"]
categories: ["microservices"]
author: "Tanmay"
showToc: true
TocOpen: true
cover:
    image: "https://www.Docker.com/wp-content/uploads/2022/03/horizontal-logo-monochromatic-white.png"
    alt: Docker
    caption: "The [Docker](https://www.Docker.com/) logo showing a **`Whale`** as a Docker to dock containers for us"
ShowBreadCrumbs: true
---
 ## Why and What?
In Software Development cycle, developers of every level have either heard or said the following:
> It works on my machine, I am not sure why it isn't running on yours

Different software projects have different dependencies, that only keep going uphill, as the product itself evolves.
When this happens, we need to make sure all interactions between different components have been done correctly in 
order for the whole application to come together and `run`.

`This is the very scenario where things can fall apart very easily!`

### Why Docker?

In scenarios like mentioned above, wouldn't it be useful to have a tool that makes sure that a software is bundled 
in a way that is irrespective of the base dependencies it requires to be built on?

As I primarily work in [Python](https://www.python.org/), I would like to throw in an analogy. This abstraction tool 
can be thought of like a [`virual environment`](https://github.com/pyenv/pyenv-virtualenv), with each project having 
its own separate dependencies which do not interfere with a different project's virtual environment, and every project
satisfies all the module level dependencies it has once run from inside the virtual environment.

Having such a tool to manage whole software, such that it handles all the base dependencies like that of the OS and any 
other package level dependencies are managed for us would be great, wouldn't it?

### What is Docker?

In-line with the objectives that we want the above-mentioned tool to perform, let's take a look at Docker. 
>Docker helps abstract away the dependencies for components between varying OS and hardware requirements, and lets
every part of the project run in an isolated environment.

Feels like technical mumbo jumbo? Well let's take another go at understanding this. The abstraction that Docker provides
can be thought of in the following way:

1) Imagine breaking the whole software into multiple smaller pieces, with each piece handling one `complete` part of 
some logic. This is what we call an isolated part of the project.
2) For the whole application to be able to run, we need each of the isolated part of the project we created in the step 
above to be able to run. 
3) To be able to run each isolated part of the project, we need to satisfy the dependencies that every isolated part 
needs. Satisfying these dependencies can be thought of as creating environments for each part of the project that we
isolated.

I hope it is more or less clear what Docker does. If not, fret not! Things should become clearer as we go further with
terminology.

## Terminology

So far, we have been trying to understand what Docker does in our own analogy and terms. However, things can go out of 
hand if one does not follow a [standard terminology](http://www.computing.surrey.ac.uk/ai/pointer/report/section1.html).

Let's dive in the terminology that will make our life easier to follow.

Obviously, one should at the end refer to the official [glossary](https://docs.Docker.com/glossary/#image) mentioned by 
Docker. However, to quickly get us started, I have mentioned some base terminologies below. 

### Image

Since we need to create an isolated environment, we first need to decide what will the base of such an environment have. 
This base that each isolated environment needs is defined in a Docker image, which contains union of layered filesystems
stacked on top of each other. 

>We can think of image as a small pre-compiled software on which we can run our assigned part of the project, 
> and using an image is like installing Python or Ubuntu on your own machine.

Because of the awesome strength of the Docker [community](https://www.docker.com/community/), they decided to keep a 
collection of the images that people need. This collection of images is listed on the 
[Docker Hub](https://hub.docker.com/), and we can find all sorts of images, like Alpine Linux and Python.

`Note`: Although the Docker hub has many images readily available, we can also create our own Docker images.

### Container

With the image


like having multiple `small-separate` machines running different pieces of the software (which we 
specify), running the whole software as Lego blocks, with each of the small machines helping each other. 



Containers in Docker have their own network, their own volume mounts, their own networks, and are just like VMs. The
only difference is that all the containers share the same os as the machine on which they are running, unlike VMs who
also have their own OS when created.

Container use the kernel to run their process. In such, a linux based OS will only be able to run linux based containers
on it, and a windows based container will not be able to run on a windows based machine.

Images in Docker are packages or templates that are used to create one or more containers. every container needs an
image to be able to run.

DockerFile is the configuration of an image should be initialized. DockerFile creates a Docker image, and image runs
a container.
