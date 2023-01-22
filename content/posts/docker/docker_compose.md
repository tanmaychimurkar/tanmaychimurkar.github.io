---
title: "Docker Compose"
date: 2022-11-16T10:26:41
description: "This post explores how we can run multiple Docker containers using a Docker Compose file"
tags: ["Docker", "microservices"]
categories: ["microservices"]
author: "Tanmay"
showToc: true
TocOpen: true
cover:
    image: "https://tcude.net/content/images/2022/01/MainImage-2.jpeg"
    alt: Docker
    caption: "The Docker Compose file orchestrating multiple Docker Containers"
ShowBreadCrumbs: true
---
 ## Why `Docker Compose`⁉️

In the [previous]({{< ref "docker_containers" >}} "Docker Containers") post, we saw how to run a Docker Container from a Docker image, either custom or pre-built image from the [DockerHub](https://hub.docker.com/). However, as we discussed, a big software is not built on just one Docker Container but many containers, working as microservices. When running multiple such containers, it is important that they are able to interact with each other, be able to exchange data, and thus act as independent blocks of a larger piece of software. In this post, we will go over how to thus run multiple containers together.

### What is `Docker Compose` file?🐙
Running multiple containers can be done by a `Docker Compose` file, that acts as a configuration file to choose multiple `Docker Images` and start them as containers, such that they are able to interact with each other. 

`Docker Compose` file, also known as `docker-compose.yml`, is a configuration file used to define the services, networks and volumes for a multi-container Docker application. With the help of `docker-compose` command, the services defined in the file can be easily started, stopped, scaled, and managed as a single unit. The file is written in `YAML` format, which is easy to read and understand. It allows developers to define the complete environment for an application in a single file, and manage it more efficiently.

Don't be fooled by me throwing the terms `services`, `networks` and `volumes` from the description as some sort of concepts that everyone must know. These are terminologies specific to the `docker-compose.yml` file, and are used as a standard base to create the compose configuration file. Let's look at them in a short detail below. 

### `docker-compose.yml` terminology📚

Below is a list of some of the main terminology that a `docker-compose.yml` file should follow. 

1) `services`: The `services` section is used to define the individual components or microservices that make up the application. Each service is defined as a separate block, with its own configuration options. The options available for each service include the `image` to use, `environment variables`, `ports` to expose, `volumes` to mount, and links to other services. For example, a simple web application might have two services: a web server and a database. The web server service would be defined with the image to use, environment variables, and ports to expose. The database service would be defined with the image to use, environment variables, and volumes to mount.

2) `networks`: The `networks` section is used to define virtual networks that the services in the application can connect to. Services can be connected to multiple networks, allowing them to communicate with each other. When a `service` is connected to a network, it is given an IP address on that network and can communicate with other services on the same network using their IP addresses. This allows services to be isolated from the host system and from the external network, while still being able to communicate with each other. It's also possible to create custom networks and connect services to them. For example, a `docker-compose.yml` file may define a default network called `default` and a custom network called `backend` where all services that need to communicate with each other privately are connected to.
3) `volumes`: The `volumes` section is used to define storage volumes that can be used by the services in the application. A volume is a way to store data outside a container's filesystem, so that it can be accessed by multiple containers and persist data even if the container is deleted. The storage volumes defined by us are stored in our local machine, under the `/var/lib/docker/volumes` folder. `Volumes` can be defined in a `docker-compose.yml` file and then mounted to a specific service's container. They can also be defined as `external`, which means that they are managed outside `docker-compose` and can be shared by multiple applications. For example, if you have a service that needs to store some data, you can create a volume and mount it to that service's container. This way, even if the container is recreated or deleted, the data stored in the volume will persist. It's also possible to use named volumes, which allows you to reference the volume by name instead of by path on the host. This can make it easier to manage volumes across different environments.

These are only some of the standard terms that are in the `docker-compose` file. However, let us now look at an example of the `docker-compose` file itself to see what the structure of it is. 

## Structure of the `docker-compose.yml` file📋

As we read before, the `docker-compose.yml` is only a `YAML` configuration file. Below is the structure of a very small compose file, that runs the [`PostgreSQL`](https://www.postgresql.org/)
and the [pgAdmin](https://www.pgadmin.org/) UI tool together. 

The below `docker-compose.yml` file is designed in such a way that we use most of the possible terms that appear in the `docker-compose.yml` file. Do not worry if you don't completely understand each and every term in the file, we will go through their descriptions later. For now, let us have a look at how the file itself looks first. 

`NOTE`: If you want to learn more about what `YAML` files are, have a look at [YAML]({{< ref "../configurations/yaml_files" >}} "YAML Configuration Files") or read the official [YAML](https://yaml.org/) documentation. 

```yaml
version: "3"

services:
    postgres:
        image: postgres
        restart: always
        environment:
            POSTGRES_PASSWORD: postgres
            POSTGRES_USER: postgres
        ports:
            - 5432:5432
        volumes:
            - postgres:/var/lib/postgresql/data

    pgadmin:
        image: dpage/pgadmin4
        environment:
            PGADMIN_DEFAULT_EMAIL: admin@pgadmin.com
            PGADMIN_DEFAULT_PASSWORD: password
            PGADMIN_LISTEN_PORT: 80
        ports:
            - 1542:80
        volumes:
            - pgadmin:/var/lib/pgadmin
        depends_on:
            - postgres

volumes:
    postgres:
    pgadmin:
```

We can now see that the terms `services` and `volumes` are defined in the above compose file, however there are also new terms that we did not yet see. Let us now go through the compose file above to see what each entry means.

1) `postgres/pgadmin`: This is the name we want to give to the `services` before we start defining them. Once we run the containers defined in the compose file, we will see each running container for a service by this name. And just like in the [previous]({{< ref "docker_containers" >}} "Docker Containers") post, we can `exec` into each of the `services` by the name that we choose to give it. Please note that the name is given by us, and is not something that is attached to a particular image.
2) `image`: This is the image that we want the `service` to use when we run it. This can be either an image from the [DockerHub](https://hub.docker.com/), or our own image. If we are using owr own custom image in the compose file, then it's `relative path` compared to the `docker-compose.yml` file should be passed in the `image` term.
3) `environment`: This term defines a list of `environment` variables that we want our `services` to use. The `environment` variables are not shared across different services, and should be defined individually for each of the service that we define in the `docker-compose` file.
4) `ports`: This term is central for the end user running the `docker-compose` file. This term defines the `port` that is to be exposed on the local machine from inside the `docker` network that is defined when the `docker-compose.yml` file is run. For us to be able to access a running application, we define the port in the first part of the ports, and the second part is the port that the `service` is running on inside the docker `network`.
5) `volumes`: There are 2 ways `volumes` can be defined in the `docker-compose` file:
   1) `named volumes`: These volumes are managed by docker-compose and can be referenced by name in the docker-compose.yml file.
   2) `bind mounts`: These volumes are defined by a specific path on the host machine and mounted inside the container.
   Usually, it is advised to use `named volumes` instead of `bind volumes` in the compose file, as `named volumes` are managed directly by `Docker`, and can be moved/deleted using the `Docker CLI`.
   The `volume` that is defined in the above compose file are also `named volumes`, where the first half of the `volumes` term is the name we want to give to the names volume, and the second half is the actual location of the directory that we want to be stored in our named `volume`.
6) `depends_on`: This is a term that controls if a service is dependent on any other service. We have to define the names of the `services` that we want to start before the current `service` where the `depends_on` term is defined. This makes sense by looking at the above scenario: We do not want the `pgadmin` service to start before the `postgres` service, as there will be no database to attach to, and thus the tool will not be able to locate our database. 

Phew!! That was a very long terminology that is followed inside the `docker-compose.yml` file. However, there should still be some alarm bells ringing in your head if you read the above descriptions.

> 1) How do we know what `volume` to mount on our local machine? 
> 2) How do we know what `environment` variables to define for a `service` to be able to run successfully? 
> 3) What is the default `port` that are particular service starts at when we run it? 

Fret not! These questions all have a answer that is easy to find. 

### Get variables for a `service` based on the `image`🔍
All the above questions are dependent on the `Docker Image` that a particular `service` is using. For example, if you look at the `Environment Variables` section of the [postgres](https://hub.docker.com/) image on DockerHub, then you will find that the `environment` variable `POSTGRES_PASSWORD` is the only required variable for the container to run successfully. We also pass the `POSTGRES_USER` variable just so we see how to pass other `environment` variables to a particular service as well.

And from the `PG_DATA` section on the webpage, we can find that default directory where `postgres` stores the data is `/var/lib/postgresql/data` directory, and that is the directory that we choose to mount in a `named` volume in our compose file. 

Now that we have MOST of the terminology that a `docker-compose.yml` file has, it is time to run the `compose` file to see things in action. 

`IMPORTANT`: Before running the next part, be sure to have `Docker Compose` installed. Please follow [this](https://docs.docker.com/compose/install/) link to install the `Compose` tool on your machine.

## Running your containers📦
Run the below command in your terminal to start all the `services` from the `docker-compose.yml` file. 

```bash
docker-compose up
```

`NOTE`: Depending on which tool you install, you should either have the `docker-compose up` command or the `docker compose up` command.

Once you run the command, you should see something like this in your terminal window:

```bash
> docker-compose up
> docker-compose up
Creating network "user_default" with the default driver
Pulling postgres (postgres:)...
latest: Pulling from library/postgres
8740c948ffd4: Pull complete
c8dbd2beab50: Pull complete
05d9dc9d0fbd: Pull complete
ddd89d5ec714: Pull complete
f98bb9f03867: Pull complete
0554611e703f: Pull complete
64e0a8694477: Pull complete
8b868a753f47: Pull complete
12ed9aefbab3: Pull complete
825b08d51ffd: Pull complete
8f272b487267: Pull complete
ba2eed7bd2cc: Pull complete
ff59f63f47d6: Pull complete
Digest: sha256:6b07fc4fbcf551ea4546093e90cecefc9dc60d7ea8c56a4ace704940b6d6b7a3
Status: Downloaded newer image for postgres:latest
Pulling pgadmin (dpage/pgadmin4:)...
latest: Pulling from dpage/pgadmin4
8921db27df28: Pull complete
d10ee54273de: Pull complete
3cf1e77a6858: Pull complete
07b97201e1e9: Pull complete
b77bae207213: Pull complete
0fcc0c06a94f: Pull complete
3c9a847b1b09: Pull complete
6ad9bb3cc48b: Pull complete
246134c219b2: Pull complete
ac0085153d3a: Pull complete
8860f79c6eae: Pull complete
8b0e5eb7caab: Pull complete
2387bc6168f4: Pull complete
0be474dc7144: Pull complete
Digest: sha256:79b2d8da14e537129c28469035524a9be7cfe9107764cc96781a166c8374da1f
Status: Downloaded newer image for postgres:latest
Status: Downloaded newer image for dpage/pgadmin4:latest
Creating user_postgres_1 ... done # Creating Users
Creating user_pgadmin_1  ... done
Attaching to user_postgres_1, user_pgadmin_1
# Starting services
postgres_1  | 
postgres_1  | PostgreSQL Database directory appears to contain a database; Skipping initialization
postgres_1  | 
postgres_1  | 2023-01-21 23:47:11.847 UTC [1] LOG:  starting PostgreSQL 15.1 (Debian 15.1-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
postgres_1  | 2023-01-21 23:47:11.847 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
postgres_1  | 2023-01-21 23:47:11.847 UTC [1] LOG:  listening on IPv6 address "::", port 5432
postgres_1  | 2023-01-21 23:47:12.035 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
postgres_1  | 2023-01-21 23:47:12.233 UTC [29] LOG:  database system was interrupted; last known up at 2023-01-17 13:43:30 UTC
postgres_1  | 2023-01-21 23:47:14.213 UTC [29] LOG:  database system was not properly shut down; automatic recovery in progress
postgres_1  | 2023-01-21 23:47:14.315 UTC [29] LOG:  redo starts at 0/249F8F8
postgres_1  | 2023-01-21 23:47:14.315 UTC [29] LOG:  invalid record length at 0/249F9E0: wanted 24, got 0
postgres_1  | 2023-01-21 23:47:14.315 UTC [29] LOG:  redo done at 0/249F9A8 system usage: CPU: user: 0.00 s, system: 0.00 s, elapsed: 0.00 s
postgres_1  | 2023-01-21 23:47:14.469 UTC [27] LOG:  checkpoint starting: end-of-recovery immediate wait
postgres_1  | 2023-01-21 23:47:14.948 UTC [27] LOG:  checkpoint complete: wrote 3 buffers (0.0%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.149 s, sync=0.049 s, total=0.564 s; sync files=2, longest=0.025 s, average=0.025 s; distance=0 kB, estimate=0 kB
postgres_1  | 2023-01-21 23:47:14.999 UTC [1] LOG:  database system is ready to accept connections
# Now pgAdmin starts
pgadmin_1   | [2023-01-21 23:47:19 +0000] [1] [INFO] Starting gunicorn 20.1.0
pgadmin_1   | [2023-01-21 23:47:19 +0000] [1] [INFO] Listening at: http://[::]:80 (1)
pgadmin_1   | [2023-01-21 23:47:19 +0000] [1] [INFO] Using worker: gthread
pgadmin_1   | [2023-01-21 23:47:19 +0000] [86] [INFO] Booting worker with pid: 86
```

From the above output of running the `docker-compose.yml` file, we see note the following:

1) The images are first pulled, which is what we saw in the [Docker Images]({{< ref "docker_images" >}} "Docker Images") post, 
2) Once they are pulled, we see the there are `users` created that we had defined in the compose file above. However, you should see at `# Creating Users` line in the description above to see that the `user` names have the prefix `_1` attached to them. This is the default behaviour by `Docker`, and we can have our own prefix by passing the argument `-p OUR_PREFIX_NAME`.
3) On the comment `# Starting Services` in the above output, we see that the services themselves have started. We see first logs from the `postgres` service, and then from the `pgadmin` service. Notice that the `pgadmin` service only starts after the `# Now pgAdmin starts` comment in the above output. This is inline with the `depends_on` clause that we had defined in our `compose` file.

Once you see the above output, you can go on `localhost:1542` (or whichever port you had chosen to be exposed on your local machine), and you should see the `pgAdmin` default webpage. Once there, login with the `email` and the `password` defined in the `PGADMIN_DEFAULT_EMAIL` and the `PGADMIN_DEFAULT_PASSWORD` environment variables. You should then be logged in, and should then see that the default `postgres` database is also visible. 

## Congratulations🙌🎉🥳🙌🎉🥳

Well Well Well!! Congratulations to you on being a pro Docker user and on coming this far.

You are now equipped with the most awesome and popular microservice creation tool in your skills bag. With this skill, you are now unstoppable in creating the largest piece of software by dividing it into many smaller parts, that are much easier to manage than a giant Monolith of code. 

Thank you for reading this far, and I hope I was able to help you learn something new!! Until next time 😎😎😎

