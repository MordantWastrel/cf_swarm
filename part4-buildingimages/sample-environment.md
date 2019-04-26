# Our First Image: A Sample Build Environment

Clone our sample Docker build environment at https://github.com/inLeagueLLC/simple-docker-build and let's look a couple ways to make our own images.

```
git clone https://github.com/inLeagueLLC/simple-docker-build.git
```

Docker images are build from Dockerfiles. From the invaluable [Dockerfile Reference:](https://docs.docker.com/engine/reference/builder/)

```
Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.
```

## What is a "Docker Build" process?

Running the [docker build](https://docs.docker.com/engine/reference/commandline/build/) or `docker-compose build` commands will create a temporary Docker container based on the previous image (specified in the `FROM` directive below) and then translate all of the other directives in the Dockerfile into configuration for the container or else commands to run inside the Container. Every individual Dockerfile command creates a new image layer, and Docker will always rebuild the fewest layers possible when it can re-use "higher" layers -- that is, if you take a Dockerfile with 10 directives, build it, and then add an 11th directive to copy a file from the host onto the image, when you build it again it will only execute the 11th directive; the previous 10 layers haven't changed.  

{% hint style="tip" %}
It's always a good idea to put the directives that will change most often later in your Dockerfile, and the directives that will not change very often above them; this will keep your Docker builds from re-building layers that haven't changed.
{% endhint %}

Let's have a line-by-line look at `Dockerfile` and an alternate "tag" build, `Dockerfile.Alpine`. As with `docker-compose.yml`, lines begining with `#` are comments and are disregarded by Docker.

## FROM ortussolutions/commandbox

This is a common, shorthand reference for the following Docker image:
`docker.io/ortussolutions/commandbox`
`ortussolutions` is the account name and `commandbox` is the image name. `docker.io` is Docker Hub, and Docker will always use that address unless you specify a fully-qualified domain name for a different container registry (e.g. `registry.mycompany.com/path/to/image`). 
 
The term "Docker Image" usually refers to an amalgam of several images: for example, the "Ortus Commandbox Docker Image" (singular) is actually several images, each of which has its own Dockerfile. Every Docker image is built on another Docker image, which is why you'll often see Docker images visualized like this:

![](/assets/Layer3b.png)
(source: Neo Kobo's "[Docker Container](http://neokobo.blogspot.com/2017/03/docker-container.html)" blog, March 2017)

Every Dockerfile starts with a `FROM` directive that points to the image "beneath" it in the stack. This is one reason Docker images are so powerful: it's trivial to build on stable, well-established, "official" images; you may need to customize a few things here or there, but you don't need to re-invent the wheel. 

### Image tags and Alternate Build Files
Once you're comfortable building Docker images, you may find that you want several versions of the same image: perhaps you want one Commandbox image for local development and another for production, or perhaps you want to experiment with a different version of Adobe CF or Lucee without changing your existing deployments. You could simply make a whole new image, but this gets unwieldy very quickly, especially when the images may only have small differences between them. The solution is **image tags**. For example, the Commandbox image is based on Ubuntu Linux, but a common alternative for many official Docker images is Alpine Linux. That is what we're asking for in our alternate Dockerfile, `Dockerfile.Alpine`:

`FROM ortussolutions/commandbox:alpine`

You'll often see image tags used to specify individual versions of applications, e.g. `mysql:5.5` versus `mysql:8.0`. If no tag is specified, Docker will use the tag `latest` by default.

## LABEL maintainer "Samuel Knowlton <sam@inleague.org>"

LABELs are optional metadata descriptors for your Dockerfile that will appear whenever an image based on your image is inspected via `docker inspect`. You can use any label, or you can conform to the [Label Schema Convention](http://label-schema.org/). Or you can ignore Labels entirely.

## ARG DEBIAN_FRONTEND=noninteractive

`ARG` is similar to `ENV` that we'll see in a moment: it sets an environment variable inside the container, in this case telling Ubuntu that the console is being run by an automated tool and to ignore some warning output it might otherwise show us. Unlike `ENV`, `ARG` environment variables are only set during the build process. This particular `ARG` is not necessary, but it makes the output of the rest of the build process a little cleaner.

## RUN apt-get update && apt-get install -y nano && \ rm -rf /var/lib/apt/lists/*

It's three commands in one! Why?

**Because each directive in a Dockerfile creates a new layer.** A common convention in Dockerfiles is to "chain" commands that all relate to one logical stage of the build process: in this case, `apt-get update` (since Docker images usually clean out `apt` archives before they finish, as ours will, too); `apt-get install -y nano` will install the nano text editor without prompting us to confirm; and `rm -rf /var/lib/apt/lists/*` will delete the `apt` archives we downloaded with `apt-get update`. The resulting layer will have all of those changes processed as a single layer, such that the only difference between this layer and the previous one is that `nano` has been installed.

For readability, these chained commands are usually written as some variation of the following:
```
RUN doSomeCommand &&
    \ doSecondCommand &&
    \ doThirdCommand && 
    \ doFourthCommand
```
