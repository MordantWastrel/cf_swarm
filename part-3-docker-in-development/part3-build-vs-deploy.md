# Managing Docker: Build and Deploy Repositories

{% hint style="info" %}
Setting up a Build Environment and Building the Commandbox Image \(Time Required: 10 Minutes\)
{% endhint %}

The lion's share of the work in creating the Docker images used in the CF world has already been done, and we could spin up a CF container using "official" images -- whether Adobe's, Lucee's, or Commandbox -- without making any changes, especially since we can make a number of changes without altering the image at all just by using environment variables.

Since we're laying out infrastructure for an entire development pipeline, though, we'll want to make our own images, even if the changes we make might just be one or two lines to copy a configuration file. So before we get to our development environment, let's put in some plumbing that will let us easily build the Docker images we'll deploy in dev later.

## Docker Imaegs and the Container Registry

When we ran `docker-compose up` in the previous section, we created a **container** based on a publicly-available Ortus Commandbox Image. A Docker **image** is a blueprint for a container that is itself made from one or more, previous Docker images. The Ortus Commanndbox image, for example, is based on the [official AdoptOpenJDK Java 8 'slim' image](https://hub.docker.com/r/adoptopenjdk/openjdk8), which is itself based on the [official Ubuntu 18 image](https://hub.docker.com/_/ubuntu), as you can see in the [AdoptOpenJDK Dockerfile](https://github.com/AdoptOpenJDK/openjdk-docker/blob/master/8/jdk/ubuntu/Dockerfile.hotspot.releases.slim).

When 