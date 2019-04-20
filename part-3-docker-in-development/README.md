# Part 3: Docker in Development

## The Prize: Turnkey Development Environment

In this section, we'll install Docker, configure resource limits, and work with a **Docker Compose** file to configure containers for a database, a front-end web server, our Coldfusion application server, and \(optionally\) a dedicated cache. By the end of this section, you should have a Docker environment ready to be deployed on any development machine, and that environment will look something like this:

![Figure 3.1: Local Development Topology ](../.gitbook/assets/cf-development-diagram%20%282%29.png)

## Managing Docker: CLI vs. GUI

When I started learning Docker in late 2017, ours had long been a Windows shop and hadn't really dealt with administration via CLI since I'd had to care for Linux machines in 2001. At first, it was intimidating to not only have to deal with Docker's CLI, but also think about CF and application deployments under Linux. I was eager to try [Portainer](https://portainer.io), the popular GUI managemnet tool for Docker and Docker Swarm, and we're going to spend a lot of time on Portainer for our Production stack -- but for our development environment, we'll want to familiarize ourselves with enough of the CLI that we can navigate it when the situation arises. As of the Fall of 2018, Portainer is sufficiently feature-complete that this won't be very often, but it also won't be never. It's just a good idea to understand what your GUI is doing "under the hood," and this guide only covers the essentials. Portainer also won't help you very much with building your own Docker images.

