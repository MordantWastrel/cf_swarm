# docker-compose.yml: A Closer Look

### A line-by-line breakdown of our sample Docker-in-Development Stack \(Time Required: 20 Minutes\)

{% hint style="info" %}
Anyone working with Docker should bookmark the official reference document for `docker compose`: [https://docs.docker.com/compose/compose-file/](https://docs.docker.com/compose/compose-file/)
{% endhint %}

Open up the `docker-compose.yml` file in the root of the repository, or view it online:

[https://github.com/inLeagueLLC/simple-cfml-complete/blob/master/docker-compose.yml](https://github.com/inLeagueLLC/simple-cfml-complete/blob/master/docker-compose.yml)

{% hint style="info" %}
### What's a YML?

A YML \(or properly, 'YAML' for **Y**et **A**nother **M**arkup **L**anguage\) file is just a text file with strict formatting rules that govern how the file is divided into sections and how one line relates to the next. YAML files use two spaces \(not tabs\) for each level of indentation, and Docker is very particular about YAML syntax. Throughout this guide, we'll refer to "levels" of indentation, e.g. "2nd level" would mean an indentation of four spaces.

Your IDE should have a plugin to facilitate YAML syntax, like Visual Studio's [YAML](https://marketplace.visualstudio.com/items?itemName=adamvoss.yaml) by Adam Voss or Sublime Text's [Pretty YAML](https://packagecontrol.io/packages/Pretty%20YAML).
{% endhint %}

## docker-compose up

`docker-compose up` looks for a `docker-compose.yml` file in the current directory and translates the instructions in that file to a series of `docker run` CLI commands \(behind-the-scenes; you won't see this part\). The `stdout` output of each container will go to your console window unless you add the option`-d` for `--detach`\(ed\); that output will still be available to you in Docker's logs but you'll get your console back right away.

### \# Comments ahoy!

Docker won't read anything after `#` in a compose file, so we can comment on the same line as our directives.

Let's go through each line of `docker-compose.yml`:

### version \(top level\)

```text
version: '3.6'  # if no version is specificed then v1 is assumed. Recommend v2 minimum
```

The **version** directive indirectly specifies the version of Docker required to execute the file. It's indirect because the version number doesn't refer to a Docker version, but the reference edition of `docker-compose.yml` that tracks which directives are included and deprecated over time.  Which versions of docker-compose.yml map to which versions of the Docker engine are available in the official document reference, but unless you know you have to support an older version of Docker, use a version corresponding to a recent `stable` Docker CE release.

### volumes \(top level\)

```text
volumes:
  sql-data:
```

Persistent storage is one of the more difficult concepts in Docker -- especially in a Swarm. We'll spend more time on this subject later, but for a simple development stack, it's a straightforward topic.

Since containers are stateless, anything in their filesystem will be destroyed when the container is destroyed. Stopping and restarting a container doesn't destroy it \(though in older versions of Docker, it used to\); a simple restart will preserve its filesystem, but we need to be able to destroy and re-create our MySQL container without losing our database. The solution is a `docker volume`, a virtual, persistent storage device managed by the Docker daemon. We can remove our container entirely and it will persist unless we explicitly delete the volume using `docker volume delete`.

{% hint style="info" %}
### Docker Volumes vs. Bind Mounts

Some of the terminology around Docker volumes can be confusing. "Volumes" can refer to one of two things:

* A [docker volume](https://docs.docker.com/storage/volumes/), which is a persistent data storage device managed entirely by the Docker daemon, or
* The `volumes` directive in `docker compose` and `docker stack`, which governs both docker volumes and bind mounts. Both involve persistent storage, but a bind mount takes a directory that already exists on the host and mounts it into the container. This means that the host \(rather than Docker\) is managing the filesystem. Bind mounts are slower than docker volumes -- especially on Windows and MacOS. The benefit of bind mounts is that they allow for easy read and write access between the host and the container, which is what we want for a development environment; on a regular docker volume, you can only copy files between the host and the container through the `docker copy` command, unless your container supports some other form of file sharing \(e.g. NFS\). 
{% endhint %}

Every service that makes use of a volume will have a service-level `volumes` directive that specifies the source and target mount point for each volume it requires.

### networks \(top level\)

```text
networks:
  cfswarm-simple:
```

Docker's default behavior without any `networks` directive is to create a "project" network that applies to any service created in the same directory. This would work just fine, but it's best to name your networks so that you can assign each container to one or more of them explicitly. Docker networking is fairly complicated under the hood, but for our purposes, we just need to know which containers need to be able to access which other containers \(and so should share a network\). Our simple solution here places all of our containers on one network so that each container can access any other container by its name; we'll take advantage of this when configuring our nginx proxy and our CF datasources.

The top-level `networks` directive just names the network we'll be referring to later on.

### secrets \(top level\)

```text
secrets:
  cfconfig:
    file: ./config/cfml/cfconfig/cfconfig.json
```

Like the `networks` directive, the top-level `secrets` directive defines a "Docker Secret" that we'll refer to in each of our services. We'll cover this in more detail below; for now, it's sufficient to know that we want to define individual pieces of data or files that we will mount into one or more containers, and that they're called secrets because this is a mechanism that is used to great effect for storing credentials and any other sensitive information we don't want accessible to anything outside our container.

### services \(top-level\) -- Docker Services vs. Docker Containers

```text
services:
```

The **services** directive indicates that the next \(first\) level indentation will be the names of the services we're asking Docker to create. We can refer to these services through the Docker CLI via [docker service](https://docs.docker.com/engine/reference/commandline/service/) commands.

For our purposes, the difference between a **container** and a **service** is simple: a container is a single "instance" of a service, but a service may be **replicated** more than once -- that is, it may have more than one container of the same type.

You can have a container without a service, but every service needs a container.

## MySQL Service

Since our sample app needs a database, let's define that service first.

### service name \(1st level\)

```text
  cfswarm-mysql:        # a friendly name. this is also DNS name inside network
```

The first level of indentation is reserved for service name definitions. Each service must have a unique name, and everything underneath it at the second or higher indentation level describes some aspect of the service. It's easy to break a docker compose file \(or a docker stack file, as you'll see later\) into digestible sections once you know how the file is divided.

In this case, we're specifying a service for our MySQL container. Note that the service name is also the the DNS name: when our CF containers need to connect to MySQL, the hostname they'll use is **cfswarm-mysql**.

{% hint style="info" %}
### Why not 'mysql' instead of 'cfswarm-mysql?'

You could name your service **mysql**. You can name a service whatever you want, so long as it's unique on your Docker host. That's why we gave ours a more specific name -- so it wouldn't interfere with any services you might already have!
{% endhint %}

### image \(2nd level\)

```text
image: mysql:5.7
```

Every container is created from an image, and this line specifies the image for our MySQL service:

`[registry address]/<image name>:[tag]`

If no registry address is specified, Docker will first check to see if the image exists locally, and if it doesn't, pull the image from [Docker Hub](http://hub.docker.com). Docker Hub allows unlimited storage of public images and is typically where you'll pull from unless and until you make your own images -- even then, those images usually extend public images from Docker Hub.

If no tag is specified, Docker will default to pulling the `:latest` tag. This can have unintended consequences -- you might upgrade your database or your web server without realizing that you've done so. `:latest` for MySQL is currently version 8, but we'll get MySQL 5.7 because we've specified that tag. The README for a Docker image will usually document available tags \(like the official [Docker Hub page for MySQL](https://hub.docker.com/_/mysql/) does, and a full list is often available under the **tags** link on that page.

Only the name of the image is required. For ubiquitous software like mysql or nginx, it may just be the name of the software, but it's often a path to an image \(e.g. **ortussolutions/commandbox**

### container\_name \(2nd level\)

```text
    container_name: cfswarm-mysql
```

Without this directive, Docker will assign a randomly generated name to the container for our MySQL service. While it's easy to see the names of running containers with `docker container ls`, we want to be able to refer to our containers quickly and easily, whether we're outputting logs \(`docker logs cfswarm-mysql`\) or logging in to the container \(`docker exec -it cfswarm-mysql bash`\).

Note that you can only use the `container_name` directive when you have a single container in your service. If we were replicating our service -- running multiple, identical containers in parallel -- `container_name` would be forbidden.

### environment \(2nd level; variables on 3rd level\)

```text
    environment:
      MYSQL_ROOT_PASSWORD: 'myAwesomePassword'
      MYSQL_DATABASE: 'cfswarm-simple-dev'
      MYSQL_ROOT_HOST: '%'
      MYSQL_LOG_CONSOLE: 'true'
```

You can specify environment variables one-at-a-time in `docker-compose.yml`. These particular variables set the root password, create a database called `cfswarm-simple-dev` \(if it doesn't already exist\), routes the log to the console, and allows connections from any host.

### volumes \(service-level: 2nd, 3rd, & 4th level\)

```text
    volumes:
    - type: volume
     source: sql-data
     target: /var/lib/mysql
```

The above example is a "long form" definition of a single volume; the [compose reference](https://docs.docker.com/compose/compose-file/#volumes) explains the difference between the long and shorthand expression for the volume directive.

* **type** specifies a volume rather than a **bind mount**, which will mount a directory on the host into the container. Since volumes perform much better than bind mounts, they are the best choice for a database container. 
* **source** refers to the name of a volume defined in the top-level **volumes** directive. Multiple containers can share the same volume, so we're referring back to our top-level definition that would \(but in this case, does not\) contain any volume options like driver specifications.
* **target** specifies the mount point in the container for the volume.

Whenever you see a `volume` directive under a service, you'll know to look for a top-level volume configuration reference. Because volumes can be shared between services, only the service-specific information goes in the service-level definition.

### ports \[HOST:CONTAINER\] \(2nd & 3rd level\)

```text
    ports: 
      - 3306:3306
```

The `ports` directive maps one or more ports on the service container to the specified port on the host. In this example, we're mapping MySQL's port 3306 to the same port on our host. Without this directive, our other containers could access MySQL since they all share a `network`, but we wouldn't be able to connect to MySQL using any tools on our host unless we ran those tools in Docker as well.

{% hint style="info" %}
### ports \(long form\)

Like `volumes` and some other `docker compose` directives, `ports` has both a short form \(shown above\) and a long form that can be easier to read. Here is the same directive in long form:

```text
    ports:
      - target: 3306
        published: 3306
        protocol: tcp
        mode: host
```

As always, consult the [docker compose](https://docs.docker.com/compose/compose-file/#ports) reference for `ports` for the best, current information.
{% endhint %}

### networks \(2nd & 3rd level\)

```text
    networks:
        - cfswarm-simple
```

The service-level `networks` directive refers back to the top-level `network` name we defined earlier. Every container that needs to access that network should have a service-level directive referring to it.

## CF Engine Service

Our CFML service definition uses some of the same directives as the `mysql` service, but but with a few alternatives and additions.

```text
  cfswarm-cfml:
    image: ortussolutions/commandbox:alpine
    container_name: cfswarm-cfml
    volumes:
      - type: bind
        source: ./app-one
        target: /app
    env_file:
      - ./config/cfml/simple-cfml.env
    secrets:
      - source: cfconfig # this isn't really a secret but non-stack deploys don't support configs so let's make it one
        target: cfconfig.json
    networks:
        - cfswarm-simple
    depends_on:
      - cfswarm-mysql
      - cfswarm-nginx
```

### volumes \(bind mount\)

```text
    volumes:
      - type: bind
        source: ./app-one
        target: /app
```

This directive is the same as our MySQL `volume`, but instead of letting Docker create and manage the volume, we're "binding" a directory on the host into the container. This is generally not suitable for production, but if we're actively developing an app then Docker isn't much use to us if we can't read and write to our app folder while it's running.

### env\_file \(2nd & 3rd level\)

```text
    env_file:
      - ./config/cfml/simple-cfml.env
```

In our MySQL service, we specified each environment variable on its own line. Since environment variables are likely to change more often than our service definitions, it's cleaner to store the environment variables in a separate file and just refer to that file in your `docker compose.yml`. The effect is the same -- the container created for your service doesn't care whether you put your environment variables in your compose file or somewhere else, but the best practice is to keep them separate.

### secrets \(2nd & 3rd level\)

```text
    secrets:
      - source: cfconfig # this isn't really a secret but non-stack deploys don't support configs so let's make it one
        target: cfconfig.json
```

The service-level `secrets` directive refers back to the secret we defined at the top level. But what is a secret, and why are we using it?

Docker has two mechanisms to mount individual files into a container at runtime: [**configs**](https://docs.docker.com/engine/swarm/configs/) and [**secrets**](https://docs.docker.com/engine/swarm/secrets/). They're very similar, but with two key distinctions:

* `docker configs` are not \(yet\) supported by `docker compose` -- they're meant for `docker stack` deployments, which we'll use in production.
* `docker secrets` are for files containing sensitive information: credentials, encryption keys, anything that you don't want to put somewhere easily accessible \(such as an environment variable or a layer of your Docker image\). These are meant to be supplied to Docker once and then referenced in deployments, rather than read from a file. 

Our CFConfig JSON file doesn't have anything sensitive in it -- it defines a datasource but relies on environment variables for the credentials. We could have built our own Docker image and copied it directly into the image, but it's conceivable that different services might have different CFConfig options; we also want to be able to update CFConfig without rebuilding an entire image. A `config` is really what we need, but since it's not supported in `docker compose`, we'll use a `secret` instead. The syntax is nearly identical; much like `volumes` and `networks`, secrets can be shared between services, so this service-level definition only supplies the `source` \(the name of the secret in the top-level `secrets` config and the name of the file to mount into the `/run/secrets` folder in our container.

### depends\_on \(2nd & 3rd level\)

```text
    depends_on:
      - cfswarm-mysql
      - cfswarm-nginx
```

**depends\_on** tells `docker compose` that this service requires other services to be running before it can start. Since our CF engine isn't much use to us without NGINX and MySQL, `docker compose` will start these services prior to starting `cfswarm-cfml`. Note that Docker doesn't check to see if the services are actually ready -- health checks are more involved and beyond the scope of our development stack. Even so, we want Docker to bring up MySQL and NGINX before starting the CF container.

## NGINX Container

The NGINX container will proxy HTTP and HTTPS requests made to the host on ports 80 and 443 to one or more CF containers. There's only one new directive in this service:

```text
    cfswarm-nginx:
    image: nginx
    command: [nginx-debug, '-g', 'daemon off;']
    container_name: cfswarm-nginx
    ports:
      - 80:80
      - 443:443
    volumes:
      - type: bind
        source: ./app-one
        target: /var/www/app-one
      - type: bind
        source: ./app-two
        target: /var/www/app-two
      - type: bind
        source: ./nginx/
        target: /etc/nginx
    networks:
      - cfswarm-simple
```

### command \(2nd level\)

Every Docker image executes a command when the container is started. This command is specified in the `Dockerfile` that governs how the image is built. Our CF containers are running a script that triggers Commandbox's `server start`. NGINX's [Dockerfile](https://github.com/nginxinc/docker-nginx/blob/866b071f099f96898563f9a003c2dbb03bb90339/mainline/stretch/Dockerfile) starts the container with the `nginx` command, but for development we'll run NGINX in DEBUG mode.

We can override the `CMD` directive from the Dockerfile with the `command` directive in `docker-compose.yml`:

```text
command: [nginx-debug, '-g', 'daemon off;']
```

This replaces the default startup command with `nginx-debug -g daemon off`.

