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

A YML (or properly, 'YAML' for **Y**et **A**nother **M**arkup **L**anguage) file is just a text file with strict formatting rules that govern how the file is divided into sections and how one line relates to the next. YAML files use two spaces (not tabs) for each level of indentation, and Docker is very particular about YAML syntax. 

Your IDE should have a plugin to facilitate YAML syntax, like Visual Studio's [YAML](https://marketplace.visualstudio.com/items?itemName=adamvoss.yaml) by Adam Voss or Sublime Text's [Pretty YAML](https://packagecontrol.io/packages/Pretty%20YAML). 
{% endhint %}

## docker-compose up
`docker-compose up` looks for a `docker-compose.yml` file in the current directory and translates the instructions in that file to a series of `docker run` CLI commands (behind-the-scenes; you won't see this part). The `stdout` output of each container will go to your console window unless you add the option`-d` for `--detach`(ed); that output will still be available to you in Docker's logs but you'll get your console back right away.

Let's go through each line of `docker-compose.yml`:

```
version: '3.6'  # if no version is specificed then v1 is assumed. Recommend v2 minimum
```

### version (top-level)

The **version* directive specifies the minimum version of Docker required to execute the file. Which versions of docker-compose.yml map to which versions of the Docker engine are available in the official document reference, but unless you know you have to support an older version of Docker, use a version corresponding to a recent `stable` Docker CE release. 

```
services:       
```
### services (top-level) -- [ker Services vs. Docker Containers

The **services** directive indicates that the next (first) level indentation will be the names of the services we're asking Docker to create. We can refer to these services through the Docker CLI via [docker service](https://docs.docker.com/engine/reference/commandline/service/) commands.

For our purposes, the difference between a **container** and a **service** is simple: a container is a single "instance" of a service, but a service may be **replicated** more than once -- that is, it may have more than one container of the same type.

You can have a container without a service, but every service needs a container.


```
    cfswarm-mysql:        # a friendly name. this is also DNS name inside network
    image: mysql:5.7
    container_name: cfswarm-mysql
    environment:
      MYSQL_ROOT_PASSWORD: 'myAwesomePassword'
      MYSQL_DATABASE: 'cfswarm-simple-dev'
      MYSQL_ROOT_HOST: '%'
      MYSQL_LOG_CONSOLE: 'true'
    volumes:    
      - type: volume
        source: sql-data
        target: /var/lib/mysql
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

