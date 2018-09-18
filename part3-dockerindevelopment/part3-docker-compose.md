# docker-compose.yml: A Closer Look

{% hint style='tip' %}
A line-by-line breakdown of our sample Docker-in-Development Stack (Time Required: 10 Minutes)
{% endhint %}

{% hint style='info' %}
Anyone working with Docker should bookmark the official reference document for `docker compose`: https://docs.docker.com/compose/compose-file/
{% endhint %}

Open up the `docker-compose.yml` file in the root of the repository, or view it online: 

https://github.com/inLeagueLLC/simple-cfml-complete/blob/master/docker-compose.yml
{% hint style='info' %}
### What's a YML?

A YML (or properly, 'YAML' for **Y**et **A**nother **M**arkup **L**anguage) file is just a text file with strict formatting rules that govern how the file is divided into sections and how one line relates to the next. YAML files use two spaces (not tabs) for each level of indentation, and Docker is very particular about YAML syntax. Throughout this guide, we'll refer to "levels" of indentation, e.g. "2nd level" would mean an indentation of four spaces.

Your IDE should have a plugin to facilitate YAML syntax, like Visual Studio's [YAML](https://marketplace.visualstudio.com/items?itemName=adamvoss.yaml) by Adam Voss or Sublime Text's [Pretty YAML](https://packagecontrol.io/packages/Pretty%20YAML). 
{% endhint %}

## docker-compose up
`docker-compose up` looks for a `docker-compose.yml` file in the current directory and translates the instructions in that file to a series of `docker run` CLI commands (behind-the-scenes; you won't see this part). The `stdout` output of each container will go to your console window unless you add the option`-d` for `--detach`(ed); that output will still be available to you in Docker's logs but you'll get your console back right away.

### # Comments ahoy!
Docker won't read anything after `#` in a compose file, so we can comment on the same line as our directives. 

Let's go through each line of `docker-compose.yml`:

```
version: '3.6'  # if no version is specificed then v1 is assumed. Recommend v2 minimum
```

### version (top-level)

The **version** directive specifies the minimum version of Docker required to execute the file. Which versions of docker-compose.yml map to which versions of the Docker engine are available in the official document reference, but unless you know you have to support an older version of Docker, use a version corresponding to a recent `stable` Docker CE release. 

```
services:       
```
### services (top-level) -- Docker Services vs. Docker Containers

The **services** directive indicates that the next (first) level indentation will be the names of the services we're asking Docker to create. We can refer to these services through the Docker CLI via [docker service](https://docs.docker.com/engine/reference/commandline/service/) commands.

For our purposes, the difference between a **container** and a **service** is simple: a container is a single "instance" of a service, but a service may be **replicated** more than once -- that is, it may have more than one container of the same type.

You can have a container without a service, but every service needs a container.

```
  cfswarm-mysql:        # a friendly name. this is also DNS name inside network
```

### service name (1st level)

The first level of indentation is reserved for service name definitions. Each service must have a unique name, and everything underneath it at the second or higher indentation level describes some aspect of the service. It's easy to break a docker compose file (or a docker stack file, as you'll see later) into digestible sections once you know how the file is divided.

In this case, we're specifying a service for our MySQL container. Note that the service name is also the the DNS name: when our CF containers need to connect to MySQL, the hostname they'll use is **cfswarm-mysql**.

{% hint style='tip' %}
### Why not 'mysql' instead of 'cfswarm-mysql?'

You could name your service **mysql**. You can name a service whatever you want, so long as it's unique on your Docker host. That's why we gave ours a more specific name -- so it wouldn't interfere with any services you might already have!

{% endhint %}

```
    image: mysql:5.7
```

### image (2nd level)

Every container is created from an image, and this line specifies the image for our MySQL service:

`[registry address]/[path-to-image]:[tag]`

If no registry address is specified, Docker will first check to see if the image exists locally, and if it doesn't, pull the image from  [Docker Hub](http://hub.docker.com). Docker Hub allows unlimited storage of public images and is typically where you'll pull from unless and until you make your own images -- even then, those images usually extend public images from Docker Hub. 

 If no tag is specified, Docker will default to pulling the `:latest` tag. This can have unintended consequences -- you might upgrade your database or your web server without realizing that you've done so. `:latest`  for MySQL is currently version 8, but we'll get MySQL 5.7 because we've specified that tag. The REEADME for a Docker image will usually document available tags (like the official [Docker Hub page for MySQL](https://hub.docker.com/_/mysql/) does, and a full list is often available under the **tags** link on that page.
 
``` 
    container_name: cfswarm-mysql
```

### container_name (2nd level)
Without this directive, Docker will assign a randomly generated name to the container for our MySQL service. While it's easy to see the names of running containers with `docker container ls`, we want to be able to refer to our containers quickly and easily, whether we're outputting logs (`docker logs cfswarm-mysql`) or logging in to the container (`docker exec -it cfswarm-mysql bash`). 

Note that you can only use the `container_name` directive when you have a single container in your service. If we were replicating our service -- running multiple containers in parallel -- `container_name` would be forbidden.

```
environment:
      MYSQL_ROOT_PASSWORD: 'myAwesomePassword'
      MYSQL_DATABASE: 'cfswarm-simple-dev'
      MYSQL_ROOT_HOST: '%'
      MYSQL_LOG_CONSOLE: 'true'
```

### environment (2nd level; variables on 3rd level)

You can specify environment variables one-at-a-time in `docker-compose.yml`. These particular variables set the root password, create a database called `cfswarm-simple-dev` (if it doesn't already exist), routes the log to the console, and allows connections from any host. 

```
    volumes:    
      - type: volume
        source: sql-data
        target: /var/lib/mysql
```

### volumes (service-level: 2nd, 3rd, & 4th level)

Since containers are stateless, anything in their filesystem will be destroyed when the container is destroyed. Stopping and restarting a container will preserve its filesystem, but we need to be able to destroy and re-create our MySQL container without losing our database. The solution is a `docker volume`, a virtual, persistent storage device managed by the Docker daemon. We can remove our container entirely and it will persist unless we explicitly delete the volume using `docker volume delete`.  The above example is a "long form" definition of a single volume:

* **type** specifies a volume rather than a **bind mount**, which will mount a directory on the host into the container. We'll look at those shortly, but volumes perform much better than bind mounts and are the best choice for a database container. 
* **source** refers to the name of a volume defined in the top-level **volumes** directive. Multiple containers can share the same volume, so we have 
* **target** specifies the mount point in the container for the volume.
        ports: 
      - 3306:3306
    networks:
        - cfswarm-simple
```
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
    - dev
    depends_on:
      - mssql
      - nginx
    networks:
        - cfswarm-simple
    depends_on:
      - cfswarm-mysql
      - cfswarm-nginx
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

volumes:
  sql-data:
networks:
  cfswarm-simple:
secrets:
  cfconfig:
    file: ./config/cfml/cfconfig/cfconfig.json
```

