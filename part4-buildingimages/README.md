# Part 4: Building Images and Container Registries

{% hint style="info" %}
Setting up a Build Environment and Building Your Own Commandbox Image \(Time Required: 10 Minutes\)
{% endhint %}

The lion's share of the work in creating the Docker images used in the CF world has already been done, and we could spin up a CF container using "official" images -- whether Adobe's, Lucee's, or Commandbox -- without making any changes, especially since we can make a number of changes without altering the image at all just by using environment variables.

Since we're laying out infrastructure for an entire development pipeline, though, we'll want to make our own images, even if the changes we make might just be one or two lines to copy a configuration file. So before we get to our development environment, let's put in some plumbing that will let us easily build the Docker images we'll deploy in dev later.

## Docker Images and the Container Registry

When we ran `docker-compose up` in the previous section, we created a **container** based on a publicly-available Ortus Commandbox Image. A Docker **image** is a blueprint for a container that is itself made from one or more previous Docker images. The Ortus Commanndbox image, for example, is based on the [official AdoptOpenJDK Java 8 'slim' image](https://hub.docker.com/r/adoptopenjdk/openjdk8), which is itself based on the [official Ubuntu 18 image](https://hub.docker.com/_/ubuntu), as you can see in the [AdoptOpenJDK Dockerfile](https://github.com/AdoptOpenJDK/openjdk-docker/blob/master/8/jdk/ubuntu/Dockerfile.hotspot.releases.slim).

When you run the Commandbox image, whether from `docker run` or `docker-compose up`, it pulls each of the dependent image layers from the [Docker Hub](https://www.docker.com/products/docker-hub), a publicly accessible Container Registry. Each layer of the image is downloaded to your local Docker daemon and then cached.

You can create your own images and store them on Docker Hub for free, so long as they're publicly available. We'll cover some alternatives for private container registries in the Appendix.

{% hint style="warning" %}
### Before You Continue: Create a Docker Hub Account

While it's not required, there's no reason not to [create a Docker Hub account](https://hub.docker.com/signup) so that you can push \(store\) and use the images you create here. If you don't, you will still be able to use the images you create locally, but you won't be able to use them later on in the Swarm section.
{% endhint %}

## Making Your Own Docker Image

There are many reasons why making your own Docker images is a good idea:

* **Warming Up Your Server**: When you started the Commandbox image, you may have noticed that it took a few minutes the first time you ran it, but that subsequent restarts of the container were much faster. That's because Commandbox had to download the CF engine from Forgebox and decompress it. We refer to Commandbox after it has downloaded the necessary engine as "**warmed-up**". Even if you have no other reason to make your own image, warming up the server will save valuable time whether your image is used in development or production. While Ortus supplies a variety of pre-warmed images, it's still best to do this step yourself; you can upgrade to a new release on your own schedule, or use a beta or prerelease version. The `:[engine-version]` tags on the Ortus Commandbox image are a wonderful convenience in a pinch, but we're building our own pipeline; we should have our own.
* **Adding Lucee Extensions**: If you want any extensions that aren't included in the default Lucee distribution, you'll need to add them, and the Docker build process is a conveinent way to do that so you're not waiting on Lucee to find and decompress extensions when you start your container.
* **Adding Commandbox Modules**: Commandbox has several modules that can help support your application but which are not included in the default installation -- [commandbox-migrations](https://forgebox.io/view/commandbox-migrations), [commandbox-hostupdater](https://www.forgebox.io/view/commandbox-hostupdater), or [commandbox-fusionreactor](https://forgebox.io/view/commandbox-fusionreactor) are just a few of these. There's no reason to install them every time your container starts when you can bake them right into your own base image.
* **Adding packages to your container OS**: It's not unusual for an application to require custom fonts, custom .JARs, or some other package dependency.

{% hint style="info" %}
If you regularly build your own Docker images, make sure you regularly `docker pull` the nearest dependent image -- in this case, `ortussolutions/commandbox`.
{% endhint %}

For this exercise, we'll make two different versions of a custom image, `tag` them, and store them on Docker Hub.

